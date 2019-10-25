This repository exists to demonstrate the difference between event capturing and event bubbling.

## Running

To run, simply open `event_capture.html` in a browser. Open the console, click the link, and observe the order of the fired events.

## Explanation

A simple two-level hierarchy is created: a `<div>` containing a link `<a>`. A number of event listeners are registered on both DOM elements.

First, event listeners are registered against the `<a>` itself (the target to be clicked). In order, these event listeners are set to fire on: bubble, capture, bubble, and captured. Next, two event listeners are registered on the parent: in order, a bubble and a capture event handler.

The order of registering is therefore as follows:
* target (bubble)
* target (capture)
* target (bubble)
* target (capture)
* parent (bubble)
* parent (capture)

The actual order of output is as follows:
* parent (capture)
* target (bubble)
* target (capture)
* target (bubble)
* target (capture)
* parent (bubble)

This therefore demonstrates the order of event capturing/bubbling:
* The event _capture_ phase goes from `window` to (but not including) the target element, triggering any event listeners along the way that have their `capture` value set to `true`.
* The event _target_ phase triggers all event listeners on the target element, regardless of their `capture` value, in order of their registration.
* The event _bubble_ phase goes from the target's parent (ie. not including the target) to the `window`, triggering any event listeners along the way that have the `capture` value set to false.

This demonstrates that we can therefore take advantage of event capturing in order to trigger certain callbacks to fire before the event bubbling phase. However, as demonstrated, this only works if we listen on a parent of the actual event target. Any events listeners registered on the event target trigger in order of registration, so we cannot control their order of activation.
