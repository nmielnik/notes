# The Hybrid Backbone & React App
* Peter Piekarczyk
* UI Engineer : Trunk Club (@peterpme)
* Github: ppiekarczyk

## Past & Present
* Brunch with Panache
  * A modular approach to building web apps with Branch
* Backbone
* Chaplin
* JQuery
* Bcrunch
* ?? Coffee Script??

## It worked great until it stopped working great
### What could UI solve on its own?
* Numerous API calls
* Lots of useless re-rendering
* Increased load time
* Huge flame chart spikes
* User complaints
* Deidcation way too much time to render debugging

## React to the Rescue!
### Replacing Backbone view with React (instance gratification)
* Reusable, encapsulated components
* Efficient diff'ing algorithm re-renders only when it needs to
* Yadda yadda yadda

### Templates & Views
(A bunch of coffee script)

### CSS Styles as Components
(More coffee script I can't read)

### No magic, just good javascript
* React makes you use the good parts of javascript again, that were abstracted away by other frameworks (ie apply, call)

# Migration Process
* Start with small componenets
* Use a Backbone/React Mixin
* Get familiar with the API
  * lifecycle methods (mount, unmount, props)
* Convert your parent view to use React.createElement instead

## React Backbone Adapter
### Attaching a React component to a Parent level Backbone view
(Sweet, even more unreadable coffee script with impossible contrast)

1. Create
2. Identity
3. Attach
4. Render
5. Dispose

### Signup Controller

### React Backbone Wrapper
1. Check for mount
2. Check for updates
3. Pass in the props
4. Find the node in the DOM
5. Apply an indentifier class
6. Render!

### `signup-header.cjsx`
(god damn unreadble coffee script)

### Passing Down Data
* Multiple Models & Collections as Props

#### Props (yep)

#### State (yep)

#### Guess What?
(more coffee script!)


