
#### Performance Questions:

* What tools would you use to find a performance bug in your code?
  * Ans: (assume JS code): I'll use Chrome's profiler
  * Ref: [Profiling JavaScript Performance](https://developer.chrome.com/devtools/docs/cpu-profiling)
* What are some ways you may improve your website's scrolling performance?
  * Ans:
    * There are several possible cases will affect scrolling performance:
      * Too many elements on the page
      * Do too many task with scroll event
      * Use heavy CSS so Repaint becomes a heavy task
    * To improve scrolling performance accordingly, I may
      * Reduce elements on the page by using Render/Load on Demand approach, or reconsider page structure.
      * Reduce the tasks when scrolling, probably apply throttle/debounce machenism.
      * Reduce loading of Repaint by adjust CSS or move something like fixed background to an independent layer.
* Explain the difference between layout, painting and compositing.
  * Ans:
    * layout: Once the browser knows which rules apply to an element it can begin to calculate how much space it takes up and where it is on screen.
    * painting: Painting is the process of filling in pixels. It involves drawing out text, colors, images, borders, and shadows, essentially every visual part of the elements. The drawing is typically done onto multiple surfaces, often called layers.
    * compositing: Since the parts of the page were drawn into potentially multiple layers they need to be drawn to the screen in the correct order so that the page renders correctly.
  * Ref: [Rendering performance](https://developers.google.com/web/fundamentals/performance/rendering/?hl=en)