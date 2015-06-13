# Event Spec Notes
[W3 Spec](http://www.w3.org/TR/2000/REC-DOM-Level-2-Events-20001113/events.html)



## Overview

### Basic Event Flow
* All events have `EventTarget` (`target` attribute)
* All events on a specific `EventTarget` are guaranteed to be triggered, but order is not guaranteed
* Any exceptions thrown inside an `EventListener` will not stop propagation
* Actions taken with `EventListener` can cause other events to fire.  This is done synchronously, so current processing halts until new event finishes processing


### Event Capture
* Processing event starting with top of tree (normally Document), down through ancestors to event's target
* Path of capture is pre-determined, so DOM manipulations won't affect the capture chain
* `stopPropagation()` halts moving down the DOM tree, including the target
  * Does not stop other listeners of the current `EventTarget`


### Event Bubbling
* Bubbling is optional (capture & target phases are not)
* After target phase, moves up DOM tree to the top of the tree (normally Document), processing listeners of each element
* Path of bubble is pre-determined before initial dispatch of the event, so DOM manipulations won't affect the bubble chain
* `stopPropagation()` halts moving up the DOM tree
  * Does not stop other listeners of the current `EventTarget`


### Event Cancelation
* Only events that are specified as cancelable can be canceled.
* Events may have a default action (ie clicking on link navigates browser)
* If any listener during any phase calls `preventDefault()` on the event, this default action will not happen
* DOM does NOT specify default actions of events



## Event listener registration



### EventTarget Interface
* Implemented by all `Node` objects
* `addEventListener(type, listener, useCapture)`
  * type (`DOMString`)
  * listener (`EventListener`)
  * useCapture (`boolean`)
  * `EventListener` added while processing an `EventTarget` won't be processed now, but could be at a later phase (ie bubble)
  * Identical `EventListener`s cannot be added, others are discarded and do not need to be removed
* `removeEventListener(type, listener, useCapture)`
  * type (`DOMString`)
  * listener (`EventListener`)
  * useCapture (`boolean`)
  * `EventListener` removed while processing an `EventTarget` wont' be processed
* `dispatchEvent(evt)`
  * evt (`Event`)
  * *RETURNS:* `boolean` (`false` if `preventDefault()` was called)


### EventListener Interface
* When a `Node` is copied via `cloneNode()`, `EventListener`s are not copied and must be added manually
* `handleEvent(evt)`
  * evt (`Event`)


### Interaction with HTML 4.0 event listeners
* HTML 4.0 supported attaching event listeners via HTML attributes
  * `<button onclick="alert('click');" />`
* To achieve compatability, these are still respected and treated as a call to `addEventListener` with `userCapture = false`
  * Changing the attribute to something else should be treated as `removeEventListener` + `addEventListener`
  * No specification for order of `EventListeners` on a specific target during a specific phase



## Event Interface



### Attributes
* `bubbles` (boolean)
* `cancelable` (boolean)
  * default action can be prevented
* `currentTarget` (EventTarget)
* `EventTarget` whose `EventListeners` are currently being processed
* `eventPhase` (short)
* `target` (EventTarget)
* `timeStamp` (DOMTimeStamp)
  * Time (milliseconds since epoch) when event was created
* `type` (DOMString)

#### DOM 4 Attributes
* `defaultPrevented` (boolean)
  * IE9 | Chrome 18 | Firefox 6 | Safari 5 | Opera 11
* `isTrusted` (boolean)
  * Firefox: TRUE = invoked by user | FALSE = invoked by script
  * IE: All are trusted except those created with `createEvent()`
  * Chrome: NOT SUPPORTED

### Methods
  * `initEvent(eventType, canBubble, cancelable)`
    * Can only be called before dispatchEvent is called
    * Can be called multiple times, last one takes precedence
  * `preventDefault()`
  * `stopPropagation()`

#### DOM 4 Methods
* `stopImmediatePropagation()`
  * Stop event from reaching any other event listeners
  * IE9 | Chrome | Firefox | Safari | Opera

#### Constants
* DOM 2
  * `CAPTURING_PHASE`
  * `AT_TARGET`
  * `BUBBLING_PHASE`
* DOM 4
  * `NONE`



## DocumentEvent Interface
### Methods
* `createEvent(eventType)`
  * eventType (`DOMString`)
    * Example: `UIEvents`
  * initialization method must be called after to set properties (ie `initUIEvent()`)



## Event Module Definition