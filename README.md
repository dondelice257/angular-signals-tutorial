# angular-signals-tutorial
This is a tutorial that covers the basis of the new angular feature "signals"

Signals are a concept used in many other frameworks like Vue js, The idea is to offer a few primitives to define the reactive state in applications and to allow the framework to know which components are impacted by a change, rather than having to detect changes on the whole tree of components.

This will be a significant change to how Angular works, as it currently relies on zone.js to detect changes in the whole tree of components by default. Instead, with signals, the framework will only dirty-check the components that are impacted by a change, which of course makes the re-rendering process more efficient.

Signals are released in Angular v16, as a developer preview API, meaning there might be changes in the naming or behavior in the future. But this preview is only a small part of the changes that will come with signals. In the future, signal-based components with inputs and queries based on signals, and different lifecycle hooks, will be added to Angular. Other APIs like the router parameters and form control values and status, etc. 

Enjoy Signals in angular v16 ( the future of reactivity in angular )!
Happy Coding 😍 !
