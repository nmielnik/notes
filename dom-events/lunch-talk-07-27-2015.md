## I Read The UI Events Spec
#### (and all I got were these lousy notes)



### Why Would You Do This?
* <!-- .element class="fragment" -->UI Automation Framework
* <!-- .element class="fragment" -->jQuery vs. custom logic vs. spec
* <!-- .element class="fragment" -->People were interested in things I was reading
* <!-- .element class="fragment" -->People encouraged me by pretending to be interested in things I was reading



### What Did You Do?
* I read through 3 specs and took notes:
  * <!-- .element class="fragment" -->[DOM Level 2 Event Model](http://www.w3.org/TR/2000/REC-DOM-Level-2-Events-20001113/events.html)
    * November 13, 2000
    * Editor: Tom Pixley (Netscape)
  * <!-- .element class="fragment" -->[UI Events (formerly DOM Level 3 Events)](http://www.w3.org/TR/DOM-Level-3-Events/)
    * April 28, 2015
    * Editor: Gary Kacmarcik (Google)
    * Editor: Travis Leithead (Microsoft)
  * <!-- .element class="fragment" -->[Events - DOM Living Standard](https://dom.spec.whatwg.org/#events)
    * July 23, 2015
    * Mozilla & Opera dudes



### Browser Support?
* IE9+
* Firefox 2+
* Opera 9+
* Chrome
* Safari


### What about IE < 9?
![U Mad Browser?](/dom-events/images/ie.jpg)


### What about IE < 9?
* They made up their own event system
  * <!-- .element class="fragment" -->`attachEvent()` (instead of `addEventListener()`)
  * <!-- .element class="fragment" -->`onclick` (instead of `click`)
  * <!-- .element class="fragment" -->reference to `Event` object was `window.event` (instead of passed as an argument)
  * <!-- .element class="fragment" -->`event.returnValue = false;` (instead of `preventDefault()`)
* <!-- .element class="fragment" -->The list goes on...



### Interesting Thoughts / Notes #1
* <!-- .element class="fragment" -->`click` is a mousedown and mouseup over same element
* <!-- .element class="fragment" -->Double click on a text passage and press delete (highlight and erase a word)
  * mousedown -> mouseup -> click -> mousedown -> mouseup -> click -> dblclick -> select -> keydown -> beforeinput -> input -> keyup



### Introduction to "DOM Events" (WHATWG)
* 3 Main objects defined
  * `Event`
  * `EventTarget`
  * `EventListener`


### Introduction to "DOM Events" (WHATWG)
* Events dispatched to objects to signal an occurrence (ie network activity, user interaction).
  * These objects implement `EventTarget` interface
  * Observe events on these objects via `addEventListener()`
  * Removed event listeners via `removeEventListener()`
  * Events can be triggered via `dispatchEvent()`


### Introduction to "DOM Events" (WHATWG)
* Apart from signaling, events can be used to give applications control over what happens next
  * ie: 'submit' event on forms -> `preventDefault()`
  * Synthetic events should examine result of `dispatchEvent()` to respond to calls to `preventDefault()`


### Introduction to "DOM Events" (WHATWG)
* Events dispatched on objects in a tree reach listeners on ancestors
  1. All object's ancestor event listeners whose capture variable is `true` are invoked, in tree order
  2. Object's own event listeners are invoked
  3. If event's `bubbles` attribute is true, object's ancestor listeners are invoked agin, but in reverse tree order


### Event Phases
* All events have `EventTarget` (`target` attribute)
* All events on a specific `EventTarget` are guaranteed to be triggered, but order is not guaranteed
  * Changed in DOM3
* Any exceptions thrown inside an `EventListener` will not stop propagation
* Events are synchronous and respect order of phases
  * Dispatching event while one is being processed halts the current event processing


### Event Phases
<!-- .element style="height: 600px" -->![DOM Event Flow](http://www.w3.org/TR/DOM-Level-3-Events/eventflow.svg)


### Event Phases
1. <!-- .element class="fragment" -->Capture Phase (window -> target parent)
2. <!-- .element class="fragment" -->Target Phase
3. <!-- .element class="fragment" -->Bubble Phase (target parent -> window)
  * Optional (`bubbles` attribute)
* <!-- .element class="fragment" -->`eventPhase` attribute (unsigned short)
  * `CAPTURING_PHASE`
  * `AT_TARGET`
  * `BUBBLING_PHASE`
  * `NONE`


### Event Phases
* <!-- .element class="fragment" -->`Event.stopPropagation()` stops all of these phases
* <!-- .element class="fragment" -->`Event.stopImmediatePropagation()` stops current target event listeners too


## Event Phases - Current Target
* Current target must be calculated during event phase
* Current target is the element currently being evaluated for listeners during the event phase
* The even listeners on current target are evaluated in the order they were attached assuming:
  * The listener has been registered for this event type
  * The listener corresponds to this event phase (capture vs bubble)
  * The event object's immediate propagation has not been stopped



### Dispatching Events
* `EventTarget.dispatchEvent()` dispatches events
* Propagation path determined before starting phases, it CANNOT change
* Event listeners are not copied over when Nodes are copied (`Node.cloneNode()` or `Range.cloneContents()`)
* Exceptions thrown inside listeners don't affect propagation or path.
* Exceptions thrown inside listeners should not propagate outside scope of handler


### Dispatching Events
* OLD
  * `document.createEvent(type)`
    * 'UIEvents' | 'MouseEvents' | 'MutationEvents'
  * `Event.initEvent(eventType, canBubble, cancelable)`
    * `initUIEvent()` | `initMouseEvent()` | `initMutationEvent()`
* NEW
  * `Constructor(type, [, eventInitDict])`



### Interesting Thoughts / Notes #2
* DOM2 -> Order of listeners not guaranteed
  * DOM3 -> Order they were attached
* DOM 0 / HTML 4 style of attaching events:
```html
  <input type="button" onclick="alert('oh my god!')" />
```

  * Treated as calling attachEventListener with capture = false
  * Changing the attribute is treated as removeEventListener + attachEventListener



## Default Actions
* Activation Triggers + Behavior
  * Certain targets (ie link, button) may have behavior that results from some events (click a link -> navigate to url)
  * For `<a>` tags, when focused, a `keydown` event with a key `Enter`


### Default Actions - Activation Trigger
* A user action or event that indicates that an 'activation behavior' should happen
* User-initiated activation triggers:
  * Click mouse button on an activatable element
  * Pressing 'Enter' key when an activatable element has focus


### Default Actions
* Events are 'cancelable' if their default action can be prevented
* <!-- .element class="fragment" -->Example:
  * Mousedown is dispatched when user presses down a button a mouse.
    * One possible default action -> Dragging an element
    * Another default action -> Start text selection
    * Another default action -> Click
* <!-- .element class="fragment" -->Preventing default action means none of these can happen
  * `Event.preventDefault()`


### Default Actions
* Except for a few exceptional cases, default action should not happen until the event dispatch completes
* <!-- .element class="fragment" -->**NOTE:** Many implementation look at event listener return value for cancelling default action (return `false` -> cancel default action)
* <!-- .element class="fragment" -->**NOTE:** Some cancelable events not have any observable default action (`mousemove`)


## Trusted Events
* Events that are actually triggered by user action and handled by the browser are called 'Trusted'
* This is opposed to events triggered manually
  * DocumentEvent.createEvent("Event") | Event.initEvent() | EventTarget.dispatchEvent().
* Event should have a `isTrusted` attribute that reflects this
  * Only IE9+ and Firefox support this



### Interesting Thoughts / Notes #3
* <!-- .element class="fragment" -->Default action should not happen until the event dispatch completes
  * `click` on checkboxes toggle 'checked' before the event is processed.
  * If default action is prevented, the value is restored to its original state
* <!-- .element class="fragment" -->Many implementation look at event listener return value for cancelling default action (return `false` -> cancel default action)
  * `window.onerror` are cancelled by returning `true`!?



### Interesting Thoughts / Notes #4
* <!-- .element class="fragment" -->For any activation trigger is not caused by a `click` event, the implementation MUST dispatch a `click` event as part of its default actions.
* <!-- .element class="fragment" -->Example: Highlighting a link and pressing `Enter` or `Space` actually dispatches a `click` event
  * `keydown` -> if `Enter` or `Space`, default action = `click` event (with isTrusted='true')
  * `click` -> All default actions including activation behavior
* <!-- .element class="fragment" -->This includes all events (touch, voice, etc.)



### Event Listener Interface
* `EventListener`
  * `type`
  * `callback`
  * `capture`
  * `handleEvent(event)`



### Event Interface - Attributes (DOM2)
* All Attributes are READONLY
  * `bubbles` (boolean)
  * `cancelable` (boolean)
  * `currentTarget` (EventTarget)
  * `eventPhase` (short)
  * `target` (EventTarget)
  * `timeStamp` (DOMTimeStamp)
  * `type` (DOMString)


### Event Interface - Attributes (DOM3)
* `defaultPrevented` (boolean)
  * IE9 | Chrome 18 | Firefox 6 | Safari 5 | Opera 11
* `isTrusted` (boolean)
  * Firefox: TRUE = invoked by user | FALSE = invoked by script
  * IE: All are trusted except those created with `createEvent()`
  * Chrome: NOT SUPPORTED


### Event Interface - Methods (DOM2)
* No Return Values
* `initEvent(eventType, canBubble, cancelable)`
  * Can only be called before dispatchEvent is called
  * Can be called multiple times, last one takes precedence
* `preventDefault()`
* `stopPropagation()`


### Event Interface - Methods (DOM3)
* `stopImmediatePropagation()`
  * Stop event from reaching any other event listeners
  * IE9 | Chrome | Firefox | Safari | Opera


### Event Interface - Constants
* Constants (for eventPhase)
* DOM 2 Constants
  * `CAPTURING_PHASE`
  * `AT_TARGET`
  * `BUBBLING_PHASE`
* DOM 4 Constants
  * `NONE`



### Event Types
![Interface Diagram](http://www.w3.org/TR/DOM-Level-3-Events/event-inheritance.svg)


### Event Types - Event
* abort
* error
* load
* select
* unload


### CustomEvent : Event
* `initCustomEvent()` (method)
  * IE9 | Chrome | Firefox 6 | Safari 5.1 | Opera 11
  * Deprecated -> `CustomEvent` constructor
* `detail` (attribute)


### UIEvent : Event
* resize
* scroll
* change (removed from spec)
* submit (removed from spec)
* reset (removed from spec)
* DOMActivate (deprecated)


### InputEvent : UIEvent
* beforeinput
* input


### FocusEvent : UIEvent
* blur
* focus
* focusin
* focusout
* DOMFocusIn (deprecated)
* DOMFocusOut (deprecated)


### MouseEvent : UIEvent
* click
* dblclick
* mousedown
* mouseenter
* mouseleave
* mousemove
* mouseout
* mouseover
* mouseup


### KeyboardEvent : UIEvent
* keydown
* keyup
* keypress (deprecated)


### WheelEvent : MouseEvent
* wheel


### CompositionEvent : UIEvent
* compositionstart
* compositionupdate
* compositionend


### MutationEvent : Event
* DOMSubtreeModified (deprecated)
* DOMNodeInserted (deprecated)
* DOMNodeRemoved (deprecated)
* DOMNodeRemovedFromDocument (deprecated)
* DOMNodeInsertedIntoDocument (deprecated)
* DOMAttrModified (deprecated)
* DOMCharacterDataModified (deprecated)



### Mouse & Keyboard Event Info
* `altKey` (boolean)
* `button` (unsigned short)
  * Start from 0 for left most button
  * On a left handed mouse, they are instead right to left!
* `clientX` (long)
  * relative to DOM implementation's client area
* `clientY` (long)
  * relative to DOM implementation's client area


### Mouse & Keyboard Event Info
* `ctrlKey` (boolean)
* `metaKey` (boolean)
* `relatedTarget` (EventTarget)
  * Secondary `EventTarget` related to a UI Event. Used with `mouseover` and `mouseout`
* `screenX` (long)
  * relative to the origin of the screen coordinate system
* `screenY` (long)
  * relative to the origin of the screen coordinate system
* `shiftKey` (boolean)


### Mouse & Keyboard Event Info
* Normal Modifiers for mouse & keyboard events:
  * Alt
  * Control
  * Meta (Not Supported in Windows)
  * Shift


### Mouse & Keyboard Event Info
* [Modifier names supported for mouse & keyboard events](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/getModifierState):
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



# DONE