Animation
=========

Microtool for animating from one value to another using requestAnimationFrame. This does not do any parsing or modification of DOM nodes or styles, it simply animates values.

Installation:
-------------

Using npm:

    npm install ainojs-animation

In the browser:

- Download a release at https://github.com/aino/ainojs-animation/releases
- Include the compiled file: ``<script src="ainojs-animation/dist/ainojs-animation.min.js"></script>``

Usage example:
--------------

    var animation = new Animation({
      initialValue: 100,
      duration: 400
    })

    var node = document.getElementById('foo')

    animation.on('frame', function(e) {
      node.style.left = e.value
    })

    animation.animateTo(400)

React usage example:
--------------------

    var App = React.createClass({

      getInitialState: function() {
        return { left: 100 }
      },

      componentWillMount: function() {
        this.animation = new Animation({
          initialValue: this.state.left,
          duration: 1000
        })
        this.animation.on('frame', this.onFrame)
      },

      onFrame: function(e) {
        this.setState({ left: e.value })
      },

      componentDidMount: function() {
        this.animation.animateTo(400)
      },

      render: function() {
        var style = {
          width: 100,
          height: 100,
          position: 'absolute',
          left: this.state.left,
          top: 100,
          background: '#000'
        }
        return (
          <div style={style} />
        )
      }
    })

    React.renderComponent(App(), document.body)

Methods:
--------
  
    animation.animateTo(destination) // starts the animation based on current value or initialValue
    animation.pause()                // pauses the animation
    animation.resume()               // resumes the animation after pause
    animation.end()                  // stops and force completes the animation
    animation.updateTo(destination)  // updates the destination while animating (within the same timeline)
    animation.isAnimating()          // returns true/false if the animation is running

Animation implemenets the ainojs-events interface. Example:
  
    // callback for each frame:
    animation.on('frame', function(e) {
      console.log(e.value) // animation value
      console.log(e.factor) // decimal value from 0-1
    })

    // callback for animation complete:
    animation.on('complete', function() {
      // animation is complete
    })

Events:
-------

- frame - triggers every frame. Event object: *value* and *factor*
- complete - triggers when animation is complete.

Options:
--------

- initialValue (0) - initial starting value for the animation value
- easing (function) - easing function (use ainojs-easing)
- duration (400) - duration in ms