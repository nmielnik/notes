## I Read The UI Events Spec
#### (and all I got were these lousy notes)
* <!-- .element class="fragment" -->Nate Mielnik
  * Reader of UI Events Spec(s)
  * Talkers of Talks
  * Noter of Notes
  * Redundanter of Redundancy



### Why Would You Do This?
* <!-- .element class="fragment" -->UI Automation Framework
* <!-- .element class="fragment" -->jQuery vs. Spec?
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