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



### Interesting Facts #1



### Introduction to "DOM Events" (WHATWG)
* Events dispatched to objects to signal an occurrence (ie network activity, user interaction).
  * These objects implement `EventTarget` interface
  * Observe events on these objects via `addEventListener()`
  * Removed event listeners via `removeEventListener()`


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


### Event Phases - Dispatching Events
* `EventTarget.dispatchEvent()` dispatches events
* Propagation path determined before starting phases, it CANNOT change
* Event listeners are not copied over when Nodes are copied (`Node.cloneNode()` or `Range.cloneContents()`)
* Exceptions thrown inside listeners don't affect propagation or path.
* Exceptions thrown inside listeners should not propagate outside scope of handler


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



### Interesting Facts #2




