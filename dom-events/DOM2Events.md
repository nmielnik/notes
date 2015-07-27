# Event Spec Notes
[W3 Spec](http://www.w3.org/TR/2000/REC-DOM-Level-2-Events-20001113/events.html)



## Overview
### Browsers
* IE9+
* Firefox 2+
* Chrome
* Safari
* Opera 9+


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
  * `EventListener` removed while processing an `EventTarget` won't be processed
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


### Methods
* `initEvent(eventType, canBubble, cancelable)`
  * Can only be called before dispatchEvent is called
  * Can be called multiple times, last one takes precedence
* `preventDefault()`
* `stopPropagation()`


#### Constants
* `CAPTURING_PHASE`
* `AT_TARGET`
* `BUBBLING_PHASE`



## DocumentEvent Interface



### Methods
* `createEvent(eventType)`
  * eventType (`DOMString`)
    * Example: `UIEvents`
  * initialization method must be called after to set properties (ie `initUIEvent()`)



## UIEvent Interface
* Created via `DocumentEvent.createEvent('UIEvents')`



### Attributes (readonly)
* `detail` (`long`)
  * Specifies some detail information about the Event
* `view` (`AbstractView`) readonly
  * Describes the `AbstractView` from which the event was generated (window)
  * Chrome | Firefox | Opera


### Methods
* `initUIEvent(type, canBubble, cancelable, view, detail)`
  * type (`DOMString`)
  * canBubble (`boolean`)
  * cancelable (`boolean`)
  * view (`AbstractView`)
  * detail (`long`)
  * Can only be called before dispatchEvent is called
  * Can be called multiple times, last one takes precedence

### Types of UIEvents
* DOMFocusIn
  * Bubbles: **YES**
  * Cnacelable: **NO**
* DOMFocusOut
  * Bubbles: **YES**
  * Cancelable: **NO**
* DOMActiate
  * Bubbles: **YES**
  * Cancelable: **YES**
  * Context Info: detail (the numerical value)
    * 1 = Simple Activation (simple click or Enter)
    * 2 = Hyperactviation (double click or Shift Enter)
  * Occurs when an element is activated, thru mouse click or a keypress


## Mouse Event Interface
* Created via `DocumentEvent.createEvent('MouseEvents')`


### Attributes (readonly)
* `altKey` (boolean)
* `button` (unsigned short)
  * Start from 0 for left most button
  * On a left handed mouse, they are instead right to left!
* `clientX` (long)
  * relative to DOM implementation's client area
* `clientY` (long)
  * relative to DOM implementation's client area
* `ctrlKey` (boolean)
* `metaKey` (boolean)
* `relatedTarget` (EventTarget)
  * Secondary `EventTarget` related to a UI Event. Used with `mouseover` and `mouseout`
* `screenX` (long)
  * relative to the origin of the screen coordinate system
* `screenY` (long)
  * relative to the origin of the screen coordinate system
* `shiftKey` (boolean)


### Methods
* `initMouseEvent(type, canBubble, cancelable, view, detail, screenX, screenY, clientX, clientY, ctrlKey, altKey, shiftKey, metaKey, button, relatedTarget)`
  * type (`DOMString`)
  * canBubble (`boolean`)
  * cancelable (`boolean`)
  * view (`AbstractView`)
  * detail (`long`)
  * screenX (`long`)
  * screenY (`long`)
  * clientX (`long`)
  * clientY (`long`)
  * ctrlKey (`boolean`)
  * altKey (`boolean`)
  * shiftKey (`boolean`)
  * metaKey (`boolean`)
  * button (`unsigned short`)
  * relatedTarget (`EventTarget`)
  * Can only be called before dispatchEvent is called
  * Can be called multiple times, last one takes precedence


### Types of MouseEvents
* Unless stated otherwise, the attributes of these events are:
  * Bubbles: **YES**
  * Cancelable: **YES**
  * Context Info: screenX, screenY, clientX, clientY, altKey, ctrlKey, metaKey, shiftKey, button, detail
* `click`
  * `click` is defined as `mousedown` and `mouseup` over the same screen location.
  * sequence: `mousedown` -> `mouseup` -> `click`
* `mousedown`
  * occurs when pointing device button is pressed over an element
* `mouseup`
  * occurs when pointing device button is released over an element
* `mouseover`
  * occurs when pointing device is moved onto an element.
  * Context Info: screenX, screenY, clientX, clientY, altKey, ctrlKey, shiftKey, metaKey, relatedTarget (what EventTarget the pointing device is exiting)
* `mousemove`
  * occurs when pointing device is moved while over an element
  * Cancelable: **NO**
  * Context Info: screenX, screenY, clientX, clientY, altKey, ctrlKey, shiftKey, metaKey
* `mouseout`
  * occurs when pointing device is moved away from an element
  * Context Info: screenX, screenY, clientX, clientY, altKey, ctrlKey, shiftKey, metaKey, relatedTarget (what EventTarget the pointing device is entering)



## Key Event Interface
* Haha, just kidding, it doesn't exist in DOM2...joke's on you!



## Mutation Event Interface
* Designed to allow for notification of changes to the structure of the document
* Was decided that these would not be cancelable, since it would be hard to make use of existing DOM interfaces which cause document modifications if DOM changes may or may not occur.  They will wait until the introduction of `transactions` into the DOM to allow for this
* Created via `DocumentEvent.createEvent('MutationEvents')`



### Constants
* attrChangeType (integer) - indicates which was the `Attr` was changed
  * ADDITION 
  * MODIFICATION
  * REMOVAL



### Attributes (readonly)
* `attrChange` (unsigned short)
  * For DOMAttrModified` event
* `attrName` (DOMString)
  * For `DOMAttrModified` event
* `newValue` (DOMString)
  * New value of `Attr` node for `DOMAttrModified`
  * New value of `CharacterData` for `DOMCharDataModified`
* `prevValue` (DOMString)
  * Previous value of `Attr` node for `DOMAttrModified`
  * Previous value of `CharacterData` for `DOMCharDataModified`
* `relatedNode` (Node)
  * Secondary node related to a mutation event, usually indicating the node that caused the change



### Methods
* `initMutationEvent(type, canBubble, cancelable, relatedNode, prevValue, newValue, attrName, attrChange)`
  * type (`DOMString`)
  * canBubble (`boolean`)
  * cancelable (`boolean`)
  * relatedNode (`Node`)
  * prevValue (`DOMString`)
  * newValue (`DOMString`)
  * attrName (`DOMString`)
  * attrChange (`unsigned short`)
  * Can only be called before dispatchEvent is called
  * Can be called multiple times, last one takes precedence



### Types of Mutation Events
* DOMSubtreeModified
  * General event for notification of all changes to the document.
  * Can fire after single modification or group of modifications (up to implementor)
  * Fired after any other events caused by mutation have fired
  * **TARGET**: lowest common parent of the changes which have taken place
  * Bubbles: **YES**
  * Context Info: **NONE**
* DOMNodeInserted
  * Fired when node has been added as a child of another node
  * Occurs after insertion has taken place.
  * **TARGET**: Node being inserted
  * Bubbles: **YES**
  * Context Info: relatedNode (parent node)
* DOMNodeRemoved
  * Fired when a node is removed from its parent node
  * Occurs before node is removed from the tree
  * **TARGET**: Node being removed
  * Bubbles: **YES**
  * Context Info: relatedNode (parent node)
* DOMNodeRemovedFromDocument
  * Fired when a node is removed from a document, via direct removal or part of a subtree
  * Occurs before node is removed from tree
  * If being removed directly, `DOMNodeRemoved` is fired before this
  * **TARGET**: Node being removed
  * Bubbles: **NO**
  * Context Info: **NONE**
* DOMNodeInsertedIntoDocument
  * Fired when a node is being inserted, via direct insertion or part of a subtree
  * Occurs after insertion has taken place
  * If being inserted directly, `DOMNodeInserted` is fired before this
  * **TARGET:** Node being inserted
  * Bubbles: **NO**
  * Context Info: **NONE**
* DOMAttrModified
  * Fired after an `Attr` has been modified on a node.
  * **TARGET**: Node whose `Attr` changed
  * Bubbles: **YES**
  * Context Info: attName, attrChange, prevValue, newValue, relatedNode (`Attr` node whose value has been affected)
* DOMCharacterDataModified
  * Fired when CharacterData within a node has been modified, but node hasn't been inserted or removed.
  * **TARGET**: Node whose CharacterData changed
  * Bubbles: **YES**
  * Context Info: prevValue, newValue



## HTML Event Types
* Created via `DocumentEvent.createEvent('HTMLEvents')`
* HTML Events use the base DOM Event interface
* Only `submit` is cancelable, and none have any defined Context Info
* `load`
  * Finished loading all content with a document
  * Finished loading all frames within a `FRAMESET` element
  * All (content/frames) within an `OBJECT` element
  * Bubbles: **NO**
* `unload`
  * Fired when a document is removed from a window or frame
  * Valid for `BODY` and `FRAMESET` elements
  * Bubbles: **NO**
* `abort`
  * Fired when page loading is stopped before an image.
  * Applies to `OBJECT` elements?
  * Bubbles: **YES**
* `error`
  * Fired when an image does not properly or error occurs during script execution
  * Valid for `OBJECT`, `BODY`, `FRAMESET` elements
  * Bubbles: **YES**
* `select`
  * Fired when user selects text in a text field
  * Valid for `INPUT` and `TEXTAREA` elements
  * Bubbles: **YES**
* `change`
  * Fired when a control loses the input focus and its value has been modified since gaining focus
  * Valid for `INPUT`, `SELECT`, and `TEXTAREA` elements
  * Bubbles: **YES**
* `submit`
  * Fired when a form is submitted. Only applies to `FORM` elements
  * Bubbles: **YES**
  * Cancelable: **YES**
* `reset`
  * Fired when a form is reset. Only applies to `FORM` elements
  * Bubbles: **YES**
* `focus`
  * Fired when an element receives focus via pointing device or tabbing
  * Valid for `LABEL`, `INPUT`, `SELECT`, `TEXTAREA`, and `BUTTON` elements
  * Bubbles: **NO**
* `blur`
  * Fired when an element loses focus via pointing device or tabbing
  * Valid for `LABEL`, `INPUT`, `SELECT`, `TEXTAREA`, and `BUTTON` elements
  * Bubbles: **NO**
* `resize`
  * Fires when a document view is resized
  * Bubbles: **YES**
* `scroll`
  * Fires when a document view is scrolled
  * Bubbles: **YES**

