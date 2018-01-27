# Events - WHATWG Notes
* Mozilla & Opera Guys
[WHATWG Events Spec](https://dom.spec.whatwg.org/#events)



## Introduction to "DOM Events"
* Events dispatched to objects to signal an occurrence (ie network activity, user interaction).
  * These objects implement `EventTarget` interface
  * Observe events on these objects via `addEventListener()`
  * Removed event listeners via `removeEventListener()`
* Events are objects that implement the `Event` interface
* "synthetic events"
  * Custom events dispatched by applications themselves
* Apart from signaling, events can be used to give applications control over what happens next
  * ie: 'submit' event on forms -> `preventDefault()`
  * Synthetic events should examine result of `dispatchEvent()` to respond to calls to `preventDefault()`
* Events dispatched on objects in a tree reach listeners on ancestors
  1. All object's ancestor event listeners whose capture variable is `true` are invoked, in tree order
  2. Object's own event listeners are invoked
  3. If event's `bubbles` attribute is true, object's ancestor listeners are invoked agin, but in reverse tree order



## Event Interface


### Event Interface - Constructor
* `Constructor(type, [, eventInitDict])`
  * type (`DOMString`)
  * eventInitDict (`EventInit`)
    * For specifying `bubbles` and `cancelable` attributes


### Event Interface - Attributes
* Attributes (all are readonly)
  * type (`DOMString`)
    * type of event (ie `click`, `hashchange`, `submit`)
  * target (`EventTarget`)
    * object to which event is dispatched
  * currentTarget (`EventTarget`)
    * object whose event listener's **callback** is currently being invoked
  * eventPhase (`unsigned short`)
    * event's phase (`NONE`, `CAPTURING_PHASE`, `AT_TARGET`, `BUBBLING_PHASE`)
  * bubbles (`boolean`)
    * `true` if event was initialized to have a bubbling phase (reverse tree order of ancestors)
  * cancelable (`boolean`)
    * `true` if event was initialized to allow `preventDefault()` to cancel part or all of the operation
    * does not always carry meaning
  * defaultPrevented (`boolean`)
    * `true` if `cancelable` is `true` AND `preventDefault()` has been called
  * isTrusted (`boolean`) [Unforgeable]
    * `true` if event was dispatched by user agent
  * timeStamp (`DOMTimeStamp`)
    * creation time of event as number of milliseconds since 1 January 1970


### Event Interface - Methods
* Methods
  * stopPropagation()
    * prevents event from reaching any other objects other than current object
  * stopImmediatePropagation()
    * prevents event from reaching any other event listeners, including those on this object
  * preventDefault()
    * if `cancelable` is true, signals the operation that caused this to be dispatched needs to be canceled
  * initEvent(type, bubbles, cancelable)
    * supported for legacy content
    * replaced by event constructor



## CustomEvent Interface
* Extends `Event`


### CustomEvent Interface - Constructor
* `Constructor(type, [, eventInitDict])`
  * type (`DOMString`)
  * eventInitDict (`CustomEventInit`)
    * Adds additional `detail` attribute


### CustomEvent Interface - Attributes
* Attributes (all are readonly)
  * detail (`any`)
    * Any custom data event was created with, typically used for synthetic events


### CustomEvent Interface - Methods
* Methods
  * initCustomEvent(type, bubbles, cancelable, details)
    1. If `dispatch flag` is set, terminate
    2. Initialize context object with `type`, `bubbles`, and `cancelable`
    3. Set context object's `detail` attribute



## Constructing Events
* When an Event (or derived object) construtor is invoked:
  1. Create an event that uses the interface the constructor was invoked upon
  2. Set `initialized flag`
  3. Initialize `type` attribute
  4. Set each dictionary member from `eventInitDict` to its member attributes
  5. Return the event



## EventTarget Interface
* An object to which an event is dispatched when something has occurred.
* Each `EventTarget` has an associated list of event listeners.
* Methods
  * addEventListener(type, callback, [, capture = false])
    * Appends an event listener for events with `type` = type.
    * `callback` argument sets the callback that will be invoked when the event is dispatched
    * When `capture` is `true`, callback will not be invoked in `BUBBLING_PHASE`
      * When `false`, callback is not invoked during `CAPTURING_PHASE`
      * Regardless, callback always invoked during `AT_TARGET` phase
    * Never adds duplicates
  * removeEventListener(type, callback, [, capture = false])
    * Remove the listener in target's list of event listener
  * dispatchedEvent(event)
    * Dispatches a synthetic event to target
    * returns `true` if `cancelable` is `false` OR `preventDefault()` was never invoked
      * otherwise, returns `false`
    * `isTrusted` attribute of Event will always be `false`



## EventListener Interface
* An object which associates a callback with a specific event.
* `type`
  * Type of event
* `callback`
  * named `EventListener` for historical reasons. Actual EventListeners are a more broad concept
* `capture`
  * Whether it's a `capture` type listener



## Dispatching Events
* To dispatch an event to a given object, with an optional _target override_
1. Let _event_ be the event that is dispatched
2. Set _event's_ `dispatch flag`
3. Initialize _event's_ `target` attribute to _target override_ if provided, otherwise the object to which _event_ is dispatched
4. If _event's_ `target` is in a tree, let _event path_ be a static ordered list of all ancestors in tree order
5. Initialize _event's_ `eventPhase` attribute to `CAPTURING_PHASE`
6. For each object in _event path_, invoke its event listeners with event _event_, as long as _event's_ `stop propagation flag` is unset
7. Initialize _event's_ `eventPhase` attribute to `AT_TARGET`
8. Invoke the event listeners of _event's_ `target` attribute value with _event_, if _event's_ `stop propagation flag` is unset
9. If _event's_ `bubbles` attribute is `true`:
  1. Reverse the order of _event path_
  2. Initialize _event's_ `eventPhase` attribute to `BUBBLING_PHASE`
  3. For each object in _event path_, invoke its event listeners with event _event_ as long as _event's_ `stop propagation flag` is unset
10. Unset _event's_ `dispatch flag`
11. Initialize _event's_ `eventPhase` attribute to `NONE`
12. Initialize _event's_ `currentTarget` attribute to `null`
13. Return false if _event's_ `canceled flag` is set, `true` otherwise


## Invoking Events
* To invoke the event listeners for an object with an event
1. Let _event_ be the event for which the event listeners are invoked
2. Let _listeners_ be a copy of the event listeners associated with the object for which these steps are run
3. Initialize _event's_ `currentTarget` attribute to the object for which these steps are run
4. Then run these substeps for each event listener in _listeners_
  1. If _event's_ `stop immediate propagation flag` is set, terminate
  2. Let _listener_ be the event listener
  3. If _event's_ `type` attribute value is not _listener's_ **type**, terminate (and run steps for next event listener)
  4. If _event's_ `eventPhase` attribute is `CAPTURING_PHASE` and _listener's_ **capture** is `false`, terminate (and run steps for next event listener)
  5. If _event's_ `eventPhase` attribute is `BUBBLING_PHASE` and _listener's_ **capture** is `true`, terminate (and run steps for next event listener)
  6. Call _listener's_ **callback's** `handleEvent()`, with the event passed into these steps as the first argument, and _event's_ `currentTarget` attribute as **callback's** `this` value.
    * If this throws any exception, report the exception