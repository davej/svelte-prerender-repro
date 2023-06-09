# Stores

The `svelte/store` module exports functions for creating [readable](#readable), [writable](#writable) and [derived](#derived) stores.

Keep in mind that you don't _have_ to use these functions to enjoy the reactive `$store` syntax in your components. Any object that correctly implements `.subscribe`, unsubscribe, and (optionally) `.set` is a valid store, and will work both with the special syntax, and with Svelte's built-in [`derived` stores](#derived).

This makes it possible to wrap almost any other reactive state handling library for use in Svelte. Read more about the store contract to see what a correct implementation looks like.

## `writable`

```js
store = writable(value?: any)
```

```js
store = writable(value?: any, start?: (set: (value: any) => void) => () => void)
```

Function that creates a store which has values that can be set from 'outside' components. It gets created as an object with additional `set` and `update` methods.

`set` is a method that takes one argument which is the value to be set. The store value gets set to the value of the argument if the store value is not already equal to it.

`update` is a method that takes one argument which is a callback. The callback takes the existing store value as its argument and returns the new value to be set to the store.

```js
import { writable } from 'svelte/store';

const count = writable(0);

count.subscribe((value) => {
  console.log(value);
}); // logs '0'

count.set(1); // logs '1'

count.update((n) => n + 1); // logs '2'
```

If a function is passed as the second argument, it will be called when the number of subscribers goes from zero to one (but not from one to two, etc). That function will be passed a `set` function which changes the value of the store. It must return a `stop` function that is called when the subscriber count goes from one to zero.

```js
import { writable } from 'svelte/store';

const count = writable(0, () => {
  console.log('got a subscriber');
  return () => console.log('no more subscribers');
});

count.set(1); // does nothing

const unsubscribe = count.subscribe((value) => {
  console.log(value);
}); // logs 'got a subscriber', then '1'

unsubscribe(); // logs 'no more subscribers'
```

Note that the value of a `writable` is lost when it is destroyed, for example when the page is refreshed. However, you can write your own logic to sync the value to for example the `localStorage`.

## `readable`

```js
store = readable(value?: any, start?: (set: (value: any) => void) => () => void)
```

Creates a store whose value cannot be set from 'outside', the first argument is the store's initial value, and the second argument to `readable` is the same as the second argument to `writable`.

```js
import { readable } from 'svelte/store';

const time = readable(null, (set) => {
  set(new Date());

  const interval = setInterval(() => {
    set(new Date());
  }, 1000);

  return () => clearInterval(interval);
});
```

## `derived`

```js
store = derived(a, callback: (a: any) => any)
```

```js
store = derived(a, callback: (a: any, set: (value: any) => void) => void | () => void, initial_value: any)
```

```js
store = derived([a, ...b], callback: ([a: any, ...b: any[]]) => any)
```

```js
store = derived([a, ...b], callback: ([a: any, ...b: any[]], set: (value: any) => void) => void | () => void, initial_value: any)
```

Derives a store from one or more other stores. The callback runs initially when the first subscriber subscribes and then whenever the store dependencies change.

In the simplest version, `derived` takes a single store, and the callback returns a derived value.

```js
import { derived } from 'svelte/store';

const doubled = derived(a, ($a) => $a * 2);
```

The callback can set a value asynchronously by accepting a second argument, `set`, and calling it when appropriate.

In this case, you can also pass a third argument to `derived` — the initial value of the derived store before `set` is first called.

```js
import { derived } from 'svelte/store';

const delayed = derived(
  a,
  ($a, set) => {
    setTimeout(() => set($a), 1000);
  },
  'one moment...',
);
```

If you return a function from the callback, it will be called when a) the callback runs again, or b) the last subscriber unsubscribes.

```js
import { derived } from 'svelte/store';

const tick = derived(
  frequency,
  ($frequency, set) => {
    const interval = setInterval(() => {
      set(Date.now());
    }, 1000 / $frequency);

    return () => {
      clearInterval(interval);
    };
  },
  'one moment...',
);
```

In both cases, an array of arguments can be passed as the first argument instead of a single store.

```js
import { derived } from 'svelte/store';

const summed = derived([a, b], ([$a, $b]) => $a + $b);

const delayed = derived([a, b], ([$a, $b], set) => {
  setTimeout(() => set($a + $b), 1000);
});
```

## `get`

```js
value: any = get(store);
```

Generally, you should read the value of a store by subscribing to it and using the value as it changes over time. Occasionally, you may need to retrieve the value of a store to which you're not subscribed. `get` allows you to do so.

:::admonition type=info

This works by creating a subscription, reading the value, then unsubscribing. It's therefore not recommended in hot code paths.

:::

```js
import { get } from 'svelte/store';

const value = get(store);
```
