# Debouncing — Complete Guide

> A practical, human-friendly README explaining debounce: what it is, why it matters, how to implement it, and how to use it across web apps and electronics.

---

## Table of contents

1. Summary
2. Why debouncing matters (plain language)
3. Key concepts and timeline
4. Debounce vs Throttle
5. Basic JavaScript implementation (step-by-step)
6. Immediate execution variant
7. Debounce that returns a Promise, cancel & flush API
8. Lodash and utilities
9. Using debounce in React (hooks + class examples)
10. TypeScript-friendly debounce
11. Practical examples and common use cases
12. Testing debounce (Jest + timers)
13. Electronics debouncing (hardware + software)
14. Performance, pitfalls, and best practices
15. FAQ
16. Quick reference / cheat sheet

---

## 1. Summary

Debouncing is a technique that delays the execution of a function until a certain amount of time has passed since the last time the function was requested. It’s used to prevent a function from firing too often — for example, to avoid making API calls for every keystroke in a search box.

This README covers the concept, plain-language explanations, practical code you can copy, and the typical places you should (and shouldn’t) use debouncing.

---

## 2. Why debouncing matters (plain language)

* **Reduce wasted work**: If the user triggers the same event many times in a short window (typing, resizing, clicking), you usually don't need to run the handler for each tiny event.
* **Preserve bandwidth and CPU**: Fewer calls reduces network requests, DOM operations, and computation.
* **Improve UX**: Avoid flicker, reduce load times, and make interfaces feel smoother.

---

## 3. Key concepts and timeline

**Core idea**: start or reset a timer each time the event happens; only run the action when the timer completes without being reset.

Simple timeline example (delay = 300ms):

```
0ms   keypress -> start timer (expires at 300ms)
100ms keypress -> clear timer, start new (expires at 400ms)
250ms keypress -> clear timer, start new (expires at 550ms)
600ms no keypress  -> timer expired at 550ms => handler runs once
```

So the handler will run **after the user has stopped firing events for the duration of the delay**.

---

## 4. Debounce vs Throttle

* **Debounce**: run the function after activity has stopped for `delay` ms.
* **Throttle**: run the function at most once every `interval` ms while activity continues.

Use debounce for "wait until the user stops" behaviour (search-as-you-type, form validation). Use throttle for "run periodically during continuous activity" (scroll handlers, tracking window resize during drag).

---

## 5. Basic JavaScript implementation (step-by-step)

This is the minimal, idiomatic implementation using `setTimeout`/`clearTimeout`.

```js
function debounce(func, delay) {
  let timer = null;
  return function(...args) {
    const context = this;
    if (timer) clearTimeout(timer);
    timer = setTimeout(() => {
      func.apply(context, args);
    }, delay);
  };
}

// Usage
const debounced = debounce(() => console.log('called'), 300);
document.querySelector('#input').addEventListener('input', debounced);
```

**How it works**:

* `debounce` returns a wrapper function.
* Each call clears the previous timer and sets a new one.
* The original `func` runs only when the timer completes.

**Edge notes**:

* Use `...args` so arguments pass through.
* Preserve `this` using `apply` if `func` depends on `this`.

---

## 6. Immediate execution variant

Sometimes you want the function to run **immediately** on the first call, then block subsequent calls until the delay finishes.

```js
function debounce(func, delay, immediate = false) {
  let timer;
  return function(...args) {
    const context = this;
    const callNow = immediate && !timer;
    clearTimeout(timer);
    timer = setTimeout(() => {
      timer = null;
      if (!immediate) func.apply(context, args);
    }, delay);
    if (callNow) func.apply(context, args);
  };
}

// Usage: immediate true => run right away, then wait
const fastClickHandler = debounce(() => console.log('fired immediately'), 500, true);
```

**Behavior**:

* `immediate=false` (default): function runs after delay.
* `immediate=true`: function runs at start, then only again after inactivity for the delay.

---

## 7. Debounce that returns a Promise, cancel & flush API

A more robust debounce helper returns a function with additional methods:

* `cancel()` — stop the timer and prevent future execution
* `flush()` — immediately run the pending invocation (if any)
* (optionally) return a Promise that resolves to the function's return value

```js
function debounceWithControl(func, wait) {
  let timer = null;
  let lastReject = null;

  function debounced(...args) {
    if (timer) clearTimeout(timer);
    // Cancel any previous pending promise
    if (lastReject) {
      lastReject({ cancelled: true });
      lastReject = null;
    }

    return new Promise((resolve, reject) => {
      lastReject = reject;
      timer = setTimeout(async () => {
        timer = null;
        lastReject = null;
        try {
          const result = await func.apply(this, args);
          resolve(result);
        } catch (err) {
          reject(err);
        }
      }, wait);
    });
  }

  debounced.cancel = () => {
    if (timer) clearTimeout(timer);
    if (lastReject) {
      lastReject({ cancelled: true });
      lastReject = null;
    }
    timer = null;
  };

  debounced.flush = () => {
    if (!timer) return;
    clearTimeout(timer);
    timer = null;
    // We intentionally do not resolve promise here for simplicity —
    // call original function synchronously if needed.
    return func();
  };

  return debounced;
}
```

**Why use this**: in some apps you need to cancel pending debounced calls (e.g., navigating away) or flush them on submit.

---

## 8. Lodash and utilities

Lodash provides a battle-tested implementation: `_.debounce(func, wait, options)`.

```js
import debounce from 'lodash/debounce';

const debounced = debounce((val) => apiSearch(val), 300, { leading: false, trailing: true });

// cancel
debounced.cancel();
// flush pending
debounced.flush();
```

`options` includes `leading` and `trailing` booleans. `leading=true` runs at the start; `trailing=true` runs at the end of the wait.

**Use lodash when**:

* You want battle-tested behavior.
* You need advanced options and small API (cancel/flush).

---

## 9. Using debounce in React

Two common patterns: use a custom hook or use lodash's debounce inside `useCallback` with proper cleanup.

### Hook: `useDebouncedValue` (simple)

Return a debounced value derived from a changing value.

```js
import { useState, useEffect } from 'react';

export function useDebouncedValue(value, delay) {
  const [debounced, setDebounced] = useState(value);

  useEffect(() => {
    const id = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(id);
  }, [value, delay]);

  return debounced;
}

// Usage
// const debouncedQuery = useDebouncedValue(query, 400);
// useEffect(() => { fetch(debouncedQuery) }, [debouncedQuery]);
```

### Hook: `useDebouncedCallback` (wraps a callback)

```js
import { useRef, useEffect, useCallback } from 'react';

export function useDebouncedCallback(fn, delay) {
  const timerRef = useRef(null);

  const debounced = useCallback((...args) => {
    if (timerRef.current) clearTimeout(timerRef.current);
    timerRef.current = setTimeout(() => fn(...args), delay);
  }, [fn, delay]);

  useEffect(() => () => clearTimeout(timerRef.current), []);

  return debounced;
}
```

**Important React tips**:

* Avoid creating a new debounced function every render — wrap with `useCallback` or `useRef`.
* If using lodash `debounce`, create it once (e.g., in `useMemo` or `useRef`) and call `cancel()` in cleanup.

### Example with lodash + useCallback

```js
import debounce from 'lodash/debounce';
import { useMemo, useEffect } from 'react';

function Search({ onSearch }) {
  const debouncedSearch = useMemo(() => debounce(onSearch, 400), [onSearch]);

  useEffect(() => () => debouncedSearch.cancel(), [debouncedSearch]);

  return <input onChange={(e) => debouncedSearch(e.target.value)} />;
}
```

---

## 10. TypeScript-friendly debounce

Simple typed wrapper example:

```ts
type Procedure = (...args: any[]) => any;

export function debounce<T extends Procedure>(func: T, wait = 0) {
  let timer: number | undefined;
  return function(this: ThisParameterType<T>, ...args: Parameters<T>) {
    if (timer) clearTimeout(timer);
    timer = window.setTimeout(() => func.apply(this, args), wait);
  } as unknown as T;
}
```

**Note**: Using `window.setTimeout` returns a number in browsers. If you target Node, types may differ.

---

## 11. Practical examples and common use cases

* **Search input**: debounce network requests until user paused typing.
* **Autosave**: wait until user stopped editing before persisting.
* **Resize/scroll**: prefer throttle for continuous updates; debounce if only final size matters.
* **Button double-click**: simple debounce to disable double submission.

**Example — search box**:

```js
const search = debounce((value) => fetch(`/search?q=${encodeURIComponent(value)}`), 300);
input.addEventListener('input', (e) => search(e.target.value));
```

**Example — prevent double submit**:

```js
const submit = debounce((form) => {
  // run submit once after user stops clicking
  form.submit();
}, 1000, true); // immediate run to allow quick first submit
```

---

## 12. Testing debounce (Jest)

Use fake timers to test timing-dependent logic.

```js
jest.useFakeTimers();

test('debounce calls function after delay', () => {
  const fn = jest.fn();
  const deb = debounce(fn, 200);
  deb();
  deb();
  // At this point fn has not been called
  expect(fn).not.toBeCalled();

  // Fast-forward time
  jest.advanceTimersByTime(250);
  expect(fn).toBeCalledTimes(1);
});
```

**Important**: reset timers and restore real timers after tests if other tests rely on them.

---

## 13. Electronics debouncing (brief)

When you press a mechanical switch, contacts will rapidly open/close for a few milliseconds (bouncing). Software or hardware debouncing removes that noise.

### Hardware approach

* **RC low-pass filter**: use a resistor and capacitor to smooth the transitions.
* **Schmitt trigger**: adds hysteresis to turn analog smoothing into a clean digital signal.

Example: resistor to pull-up, switch to ground, capacitor from input to ground — choose RC constant smaller than your required debounce interval but larger than bounce duration.

### Software approach

* Wait for the input to be stable for `N` ms before accepting the new state.
* Many microcontroller libraries offer `debounce()` helpers.

Pseudo-code:

```
if read_input() != last_state:
  start timer
else if timer expired:
  last_state = read_input()
  // event accepted
```

---

## 14. Performance, pitfalls, and best practices

* **Cleanup**: cancel pending debounce timers on unmount/navigation to avoid memory leaks.
* **Don't debounce everything**: debounce UI updates where final state matters; for continuous feedback, prefer throttle.
* **Server-side rendering**: avoid timers when running on server — guard with `typeof window !== 'undefined'`.
* **Accessibility**: don’t delay critical UI feedback that the user expects immediately (e.g., a visible click animation).
* **Default delay**: typical values — `100–300ms` for typing/search, `300–1000ms` for autosave, `50–100ms` for switch debounce in electronics.

---

## 15. FAQ

**Q: Can I debounce a Promise-returning function?**
A: Yes, but be careful: naive debounce wrappers may drop or replace pending promises. Use a wrapper that returns a promise and cancels/ rejects previous ones if needed.

**Q: Is debounce the same as rate-limiting an API?**
A: Related, but not the same. Debounce is client-side behavior to reduce calls. Rate-limiting is often server-side enforcement.

**Q: Should I use leading or trailing edge?**
A: Use `trailing` (default) when you want the final value after activity is done. Use `leading` when you want an immediate reaction and then block further activity.

---

## 16. Quick reference / cheat sheet

* **Basic**: `debounce(fn, delay)` — waits `delay` ms after last call.
* **Immediate**: `debounce(fn, delay, true)` — run at start, then wait.
* **Lodash**: `_.debounce(fn, wait, { leading, trailing, maxWait })`.
* **React**: use `useDebouncedValue` for values or `useDebouncedCallback` for functions. Ensure to cleanup.
* **Testing**: use fake timers; advance timers rather than using real delays.

---

### Closing notes

This README aims to be practical and copy-paste friendly. If you want:

* A **one-page cheat sheet** image or PDF, I can make it.
* A **React component** example ready to drop into a project (with TypeScript), I can add it.
* An **electronics schematic** image (RC + Schmitt trigger) — I can sketch it.

Pick one next step and I’ll add it directly.
