# angular-signals-tutorial
This is a tutorial that covers the basis of the new angular feature "signals"

Signals are a concept used in many other frameworks like Vue js, The idea is to offer a few primitives to define the reactive state in applications and to allow the framework to know which components are impacted by a change, rather than having to detect changes on the whole tree of components.

This will be a significant change to how Angular works, as it currently relies on zone.js to detect changes in the whole tree of components by default. Instead, with signals, the framework will only dirty-check the components that are impacted by a change, which of course makes the re-rendering process more efficient.

Signals are released in Angular v16, as a developer preview API, meaning there might be changes in the naming or behavior in the future. But this preview is only a small part of the changes that will come with signals. In the future, signal-based components with inputs and queries based on signals, and different lifecycle hooks, will be added to Angular. Other APIs like the router parameters and form control values and status, etc. should also be affected.

Now we've seen what are signals and why to use them, let's take a look at how to use them in our application

How to use signals?

A signal is a function that holds a value that can change over time. To create a signal, Angular offers a signal() function that you can import everywhere in your component :
`
//let first import signal from angular/core

import { signal } from '@angular/core';

// declare a signal

num = signal(0);
`
The type of num is WritableSignal<number>, which is a function that returns a number.

When you want to get the value of a signal, you have to call the created signal:
`
// consume the value of the signal, we do the same as on methods (while reading the method's returned value )

value = num();

Now, value will be equal to 0

You can also set the value of a signal:

// set the value of the signal

num.set(1);
`

num will be equal to 1 and value will be equal to 1 too, because of set function

Also you can update it:
`
//setting the value of the signal

count = signal(9)

// update the value of the signal

name.update((n)=>n+1) // after this, it will return 10

`

There is also a mutate method that can be used to mutate the value of a signal:
`

// mutate the value of the signal (handy for objects/arrays)

countries = signal({name:'Burundi', slug:'bi', currency : 'BIF'})

countries.mutate((country) => country.currency = 'FBU');

`

The difference between mutate and update is that with mutate the value changed only in the concerned feature but with update the value changed in the entire application ( everywhere we call the value )

You can also create a read-only signal, that can’t be updated, with asReadonly:
`
readonlyValue = count.asReadonly();

Once you have defined signals, you can define computed values that derive from them:

double = computed(() => count() * 2);
`
Computed values are automatically computed when one of the signals they depend on changes ( double changed everytime automatically when count changed ).
`
count.set(2);

console.log(double()); // logs 4
`
Note that they are lazily computed and only re-computed when one of the signals they depend on produces a new value. They are not writable, so you can’t use .set() or .update() or .mutate() on them (their type is Signal<T>).

Under the hood, a signal has a set of subscribers, and when the value of the signal changes, it notifies all its subscribers. Angular smartly does that, to avoid recomputing everything when a signal changes, by using internal counters to know which signals have changed since the last time a computed value was computed.

Finally, you can use the effect function to react to changes in your signals:
`
// log the value of the count signal when it changes

effect(() => console.log(count()));

An effect returns an object with a destroy method that can be used to stop the effect:

effectRef: EffectRef = effect(() => console.log(count()));

// stop executing the effect

effectRef.destroy();
`
An effect or a computed value will re-evaluate when one of the signals they depend on changes, without any action from the developer. This is usually what you want, and me too 😃.

Note that while signals and computed values will be common in Angular applications, effects will be used less often, as they are more advanced and low-level primitive. If you need to react to a signal change, for example, to fetch data from a server, you can use the RxJS interoperability layer that we detail below.

All these features (and terminology) are fairly common in other frameworks, so you won’t be surprised if you used Vue 3 before.

I think you might be wondering if it is possible to use signal value in your template or not 😊, The answer is YES ! let's see how we can do that
Consume signals values in your template

As mentioned above, you can use signals and computed values in your templates:
`
`<p>{{ count() }}</p>
`
If the counter value changes, Angular detects it and re-renders the component.

You can change this behavior though, by creating an effect with a specific option:
`
this.logEffect = effect(() => console.log(count()), { manualCleanup: true });
`
In this case, you will have to manually stop the effect when the component is destroyed:
`
ngOnDestroy() {

  this.logEffect.destroy();
  
}
`
It’s anyway quite interesting how frameworks inspire each other, with Angular taking inspiration from Vue for the reactivity part, whereas other frameworks are increasingly adopting the template compilation approach of Angular, with no Virtual DOM needed at runtime.

Enjoy Signals in angular v16 ( the future of reactivity in angular )!
Happy Coding 😍 !
