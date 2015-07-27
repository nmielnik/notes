# Event Spec Notes
[W3 Spec](http://www.w3.org/TR/DOM-Level-3-Events/)



## Conformance
* A browser conforms if it supports:
  * The DOM3 Core Spec
  * The 3 Event Phases
  * All interfaces and events in this spec
  * Complete set of [key](http://www.w3.org/TR/DOM-Level-3-Events-key/) and [code](http://www.w3.org/TR/DOM-Level-3-Events-code/) values
* Conforming browser must dispatch events of the right type for the right target
* A browser conforms if it implements interfaces and event types specified in [Event Types](http://www.w3.org/TR/DOM-Level-3-Events/#event-types)
* Conforming browser must support scripting, declarative interactivity, or some other means of detecting and dispatching events



## Dispatch Events
* `EventTarget.dispatchEvent()` dispatches events
* Propagation path determined before starting phases, it CANNOT change
* Event listeners are not copied over when Nodes are copied (`Node.cloneNode()` or `Range.cloneContents()`)
* Exceptions thrown inside listeners don't affect propagation or path.
* Exceptions thrown inside listeners should not propagate outside scope of handler


## Event Dispatch + DOM Event Flow

* Propagation Path is determined before executing phases, this will NOT change even if the DOM hierachy does
* Event listeners are NOT copied over by calls to `Node.cloneNode` or `Range.cloneContents`
* Exceptions thrown in event handlers should NOT stop propagation and MUST NOT propagate outside the scope of the event handler



## 3 Event Phases:

1. Capture Phase (window -> target parent)
2. Target Phase
3. Bubble Phase (target parent -> window)

* `Event.stopPropagation()` stops all of these phases
* Stored as `Event.eventPhase`
* All events must support Capture + Target phase
  * Some event types may not have a bubble phase
* Events are synchronous, so if during the event phase, the event fires another event, it must way for that events entire event phase to complete before it resumes its own event phase


### Diagram
![DOM Event Flow](http://www.w3.org/TR/DOM-Level-3-Events/eventflow.svg)



## Current Target

* Current target must be calculated during event phase
* Current target is the element currently being evaluated for listeners during the event phase
* The even listeners on current target are evaluated in the order they were attached assuming:
  * The listener has been registered for this even type
  * The listener corresponds to this event phase (capture vs bubble)
  * The event object's immediate propagation has not been stopped



## Default Actions

* Events that are cancellable if their default action can be prevented
* Example:
  * Mousedown is dispatched when user presses down a button a mouse.
  	* One possible default action -> Dragging an element
  	* Another default action -> Start text selection
  	* Another default action -> Click
  * Preventing default action means none of these can happen
* Except for a few exceptional cases, default action should not happen until the event dispatch completes
  * Interesting counter-example: Clicks on checkboxes toggle 'checked' before the event is processed.  If default action is prevented, the value is restored to its original state
* **NOTE:** Many implementation look at event listener return value for cancelling default action (return `false` -> cancel default action)
  * `window.onerror` are cancelled by returning `true`!?
* **NOTE:** Some cancelable events not have any observable default action (`mousemove`)



## Event Order

* Interesting example:
  * Double click on a text passage and press delete (highlight and erase a word)
  * mousedown -> mouseup -> click -> mousedown -> mouseup -> click -> dblclick -> select -> keydown -> beforeinput -> input -> keyup
* Events can be triggered asynchornously (ie 'load'), which means they don't have to respect the event order
  * However, individual events are still processed synchronously



## Trusted Events

* Events that are actually triggered by user action and handled by the browser are called 'Trusted'
* This is opposed to events triggered manually
  * DocumentEvent.createEvent("Event") | Event.initEvent() | EventTarget.dispatchEvent().
* Event should have a `isTrusted` attribute that reflects this
  * Only IE9+ and Firefox support this



## Activation Triggers + Behavior

* Certain targets (ie link, button) may have behavior that results from some events (click a link -> navigate to url)
  * For `<a>` tags, when focused, a `keydown` event with a key `Enter`


### Activation Trigger

* A user action or event that indicates that an 'activation behavior' should happen
* User-initiated activation triggers:
  * Click mouse button on an activatable element
  * Pressing 'Enter' key when an activatable element has focus
* Event-based activation triggers:
  * May include timer-based events that activate an element at a certain clock time or after a certain time period has elapsed
* Attributes on element may change activation trigger OR activation behavior
  * Example: `role` aria attribute
  * Browsers may not implement them, in which case content author would need to handle it (both trigger + behavior)


### Activation Event Synthesis

* Interesting:
  * For any activation trigger is not caused by a `click` event, the implementation MUST dispatch a `click` event as part of its default actions.  This includes the event being of the `MouseEvent` interface, and the event attributes corresponding to a left button click on the mouse.
  * Example: Highlighting a link and pressing `Enter` or `Space` actually dispatches a `click` event as the default action (which is an activation trigger for navigating to the url).
    * `keydown` -> if `Enter` or `Space`, default action = `click` event (with isTrusted='true')
    * `click` -> All default actions including activation behavior
  * This includes all events (touch, voice, etc.)


### Constructing Mouse & Keyboard Events

* Normal Modifiers for mouse & keyboard events:
  * Alt
  * Control
  * Meta (Not Supported in Windows)
  * Shift
* Modifier names supported for mouse & keyboard events:
  * AltGraph
  * CapsLock
  * Fn
  * FnLock
  * Hyper
  * NumLock
  * OS
  * ScrollLock
  * Super
  * Symbol
  * SymbolLock
* Guide for what these things are: [MDN getModifierState](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/getModifierState)



## Event Interfaces
[!Interface Diagram](http://www.w3.org/TR/DOM-Level-3-Events/event-inheritance.svg)


### EventTarget + EventListener + Document
* EventTarget
  * `addEventListener()`
  * `removeEventListener()`
  * `dispatchEvent()`
* EventListener
  * `handleEvent()`
* Document
  * `createEvent()`
    * Has been deprecated in favor of Event Constructors
      * Chrome 15 | Firefox 11 | Opera 11.6 | Safari Nightly


### Event (base interface)

#### Event Attributes
* DOM 2 Attributes (all are READONLY)
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
* DOM 4 Attributes
  * `defaultPrevented` (boolean)
    * IE9 | Chrome 18 | Firefox 6 | Safari 5 | Opera 11
  * `isTrusted` (boolean)
    * Firefox: TRUE = invoked by user | FALSE = invoked by script
    * IE: All are trusted except those created with `createEvent()`
    * Chrome: NOT SUPPORTED

#### Event Methods
* DOM 2 Methods (no return values)
  * `initEvent(eventType, canBubble, cancelable)`
    * Can only be called before dispatchEvent is called
    * Can be called multiple times, last one takes precedence
  * `preventDefault()`
  * `stopPropagation()`
* DOM 4 Methods
  * `stopImmediatePropagation()`
    * Stop event from reaching any other event listeners
    * IE9 | Chrome | Firefox | Safari | Opera
  

#### Event Constants
* Constants (for eventPhase)
* DOM 2 Constants
  * `CAPTURING_PHASE`
  * `AT_TARGET`
  * `BUBBLING_PHASE`
* DOM 4 Constants
  * `NONE`


### CustomEvent (inherits from Event)
* `initCustomEvent()` (method)
  * IE9 | Chrome | Firefox 6 | Safari 5.1 | Opera 11
  * Deprecated -> `CustomEvent` constructor
* `detail` (attribute)



## Event Types - Event
* abort
* error
* load
* select
* unload


## Event Types - UIEvent
* resize
* scroll


## Event Types - InputEvent
* beforeinput
* input


## Event Types - FocusEvent
* blur
* focus
* focusin
* focusout


## Event Types - MouseEvent
* click
* dblclick
* mousedown
* mouseenter
* mouseleave
* mousemove
* mouseout
* mouseover
* mouseup


## Event Types - KeyboardEvent
* keydown
* keyup


## Event Types - WheelEvent
* wheel


## Event Types - CompositionEvent
* compositionstart
* compositionupdate
* compositionend
