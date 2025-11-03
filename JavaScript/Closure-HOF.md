# Closures & Higher‑Order Functions — Interview Prep README

> **Purpose (Pro Jarvis format):** Ye single README file closures aur higher-order functions (HOF) ko basic → medium level tak cover karta hai, interview-friendly. Focus practical examples, common pitfalls, time/space notes, aur short exercises with hints. Code examples mostly JavaScript.

---

## Kaise use karein

1. **Start:** "What is a closure?" aur simple examples se.
2. **Next:** Practical HOFs — map/filter/reduce ka under-the-hood jaankari.
3. **Practice:** Exercises solve kar, phir solutions check kar.
4. **Quick reference:** Cheatsheet section for interview one-liners.

---

## Learning goals

* Closure kya hota hai aur kyun useful hai.
* `this` vs closure confusion clear karna.
* Build HOFs: functions that take/return functions.
* Currying, partial application, composition aur memoization samajh kar implement karna.
* Recognize memory/perf implications.

---

# 1) Closure — Definition (simple)

**Closure** = function + lexical environment where that function was declared. Matlab: inner function can access outer function ke variables even jab outer function execution complete ho chuka ho.

**Key idea:** JavaScript preserves variable bindings (not just values) for functions that reference them.

**Minimal example:**

```js
function makeCounter(){
  let count = 0; // lexical variable
  return function(){
    count += 1;
    return count;
  };
}
const c = makeCounter();
console.log(c()); // 1
console.log(c()); // 2
```

Yahan `c` ek closure hai jisme `count` ka binding retained hai.

---

## Why closures matter (interview points)

* Data encapsulation / privacy (private variables without classes).
* Function factories (create customized functions).
* Memoization & caching.
* Event handlers that remember state.

**One-liner for interviews:** "A closure gives a function access to variables from its outer scope, even after that outer function has returned."

---

# 2) How closures actually work (conceptually)

* JavaScript engine maintains lexical environment records for active scopes.
* If inner function references outer variables, the engine keeps that environment alive (garbage collector won’t collect it until no references remain).
* Closures capture *bindings*, not snapshots — if outer variable changes, inner sees updated value.

**Binding example:**

```js
function adder(x){
  return function(){ return x + 1 };
}
const a = adder(1);
// 'x' binding for `a` remains 1
```

**But:** when using loops, naive closures often capture same binding — common trap below.

---

# 3) Common pitfalls & gotchas

### 3.1 Loop + closure trap (classic)

```js
const fns = [];
for (var i = 0; i < 3; i++) {
  fns.push(function(){ return i; });
}
console.log(fns.map(fn=>fn())); // [3,3,3]
```

**Why:** `var` creates function-scoped `i`. All closures share same binding which ends at 3.

**Fixes:** use `let` (block-scoped) or create a new binding per iteration:

```js
for (let i = 0; i < 3; i++) fns.push(()=>i); // works: [0,1,2]
// or
for (var i=0;i<3;i++){ (function(j){ fns.push(()=>j); })(i); }
```

### 3.2 Memory leaks & long-lived closures

Closures can inadvertently keep large objects alive if inner function references them. Be mindful for long-running apps (e.g., Node servers, SPA state).

**Tip:** Avoid storing huge data in closure scopes unless necessary; release references by setting them `null` if done.

### 3.3 `this` vs closure variables

Arrow functions capture lexical `this` and should not be used when you need dynamic `this`. Closures capture lexical variables, not `this` unless explicitly stored.

```js
const obj = {
  value: 10,
  incLater(){
    setTimeout(function(){ console.log(this.value); }, 100); // undefined or global
  }
}
// Better
setTimeout(()=> console.log(obj.value), 100); // captures obj lexically
```

Explain in interviews: `this` is runtime binding; closure is lexical binding of variables.

---

# 4) Higher‑Order Functions (HOF) — Definition & basics

**Definition:** Function that either takes function(s) as arguments, or returns a function, or both.

**Examples in JS standard library:** `map`, `filter`, `reduce`, `sort` (compare function). These accept functions as inputs.

**Why HOFs are useful:** Abstraction, code reuse, composition, creating DSLs.

**Simple HOF example:**

```js
function times(n){
  return function(fn){
    for (let i=0;i<n;i++) fn(i);
  }
}
const thrice = times(3);
thrice(i=>console.log('run', i));
```

---

# 5) Patterns built from HOFs

### 5.1 Currying

* Transform function of multiple args into chain of unary functions.
* Useful for partial application and point-free style.

```js
// curry for two args
const add = a => b => a + b;
add(2)(3); // 5

// generic curry (simple)
function curry(fn){
  return function curried(...args){
    if (args.length >= fn.length) return fn.apply(this, args);
    return function(...rest){ return curried.apply(this, args.concat(rest)); };
  };
}
```

**Interview tip:** Curry preserves arity and allows partial application.

### 5.2 Partial application

* Fix some arguments, return a function expecting rest.
* `Function.prototype.bind` can do partial application for leading args.

```js
const multiply = (a,b,c)=>a*b*c;
const double = multiply.bind(null, 2); // partial: first arg fixed
```

### 5.3 Composition

* Combine small functions into bigger one: `compose(f,g)(x) = f(g(x))`.

```js
const compose = (f,g) => x => f(g(x));
const add1 = x => x+1; const double = x => x*2;
compose(add1, double)(3); // add1(double(3)) => 7
```

**Left vs right composition:** some libs call `pipe` (left-to-right).

### 5.4 Memoization (HOF returns caching function)

* Cache results for expensive pure functions.

```js
function memoize(fn){
  const cache = new Map();
  return function(...args){
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const res = fn.apply(this, args);
    cache.set(key,res);
    return res;
  }
}
```

**Note:** JSON.stringify as key has caveats (order, objects). Use tuple keys or WeakMap for object args.

---

# 6) Practical HOF examples (interview-style)

### 6.1 Retry wrapper

Retries a promise-returning function N times with delay.

```js
function retry(fn, times=3, delay=100){
  return async function(...args){
    let lastErr;
    for (let i=0;i<times;i++){
      try{ return await fn(...args); } catch(e){ lastErr = e; if (i < times-1) await new Promise(r=>setTimeout(r, delay)); }
    }
    throw lastErr;
  };
}
```

### 6.2 Once (ensure function runs only once)

```js
function once(fn){
  let called=false, result;
  return function(...a){ if (!called){ result = fn.apply(this,a); called=true; } return result; };
}
```

### 6.3 Throttle & Debounce (HOF style)

* Debounce: return function that delays call until inactivity.
* Throttle: return function that ensures calls happen at most once per window.

(Implementations are common interview questions — include in exercises.)

---

# 7) Memory & performance considerations

* Closures keep referenced variables alive. Avoid capturing large outer scopes when not needed.
* Memoization caches can cause memory growth — prefer bounded caches (LRU) or WeakMap when keys are objects.
* Frequent creation of small closures (like inside loops) is fine usually, but in hot loops create helper once.

**Practical rule:** If closure-created function is returned and will live long, keep its captured scope minimal.

---

# 8) Exercises (practice)

1. **Make counter with reset:** Implement `makeCounter()` that returns `{ inc, dec, reset, value }` where `value()` returns current count. Use closure for private `count`.
2. **Fix the loop-closure bug:** Given classic `for(var i...)` example, produce array of functions that return 0..n-1 correctly without using `let`.
3. **Implement `memoize`** that supports object args (use `WeakMap` for object keys and Map for primitives).
4. **Write `compose(...fns)`** supporting n functions (right-to-left) and ensure it short-circuits if value becomes `undefined`.
5. **Implement `onceAsync(fn)`** for async functions: ensure `fn` is invoked only once even if called concurrently — subsequent callers wait for the first promise.

Hints are in `solutions` section below; try before peeking.

---

# 9) Solutions & sketches (brief)

**Exercise 1 — makeCounter:**

```js
function makeCounter(){
  let count = 0;
  return {
    inc(){ return ++count; },
    dec(){ return --count; },
    reset(){ count = 0; },
    value(){ return count; }
  };
}
```

**Exercise 2 — fix loop-closure without `let`:**

```js
const fns = [];
for (var i=0;i<3;i++){
  (function(j){ fns.push(function(){ return j; }); })(i);
}
```

**Exercise 3 — memoize with object keys (sketch):**

* Use `WeakMap` chained approach: root WeakMap maps first arg -> WeakMap for second arg ... -> result. For primitives use Map at top or use Map for all with wrapper for primitives.

**Exercise 5 — onceAsync (concurrent safe):**

```js
function onceAsync(fn){
  let promise = null;
  return function(...args){
    if (!promise) promise = fn.apply(this,args).finally(()=>{ promise = null; });
    return promise;
  }
}
```

---

# 10) Cheatsheet (one-page)

* Closure = function + lexical env.
* HOF = function that takes/returns function.
* `let` vs `var` important for closure in loops.
* Memoize → cache results; watch memory.
* Currying → `a=>b=>c=>...`.
* Compose → `f(g(x))`.
* Prefer small closures; avoid capturing huge outer scope.

---

## Quick interview answers (copy-paste style)

* **Q:** "Explain closure in one line."
  **A:** "A closure is a function bundled with its lexical environment, allowing it to access outer-scope variables even after the outer function has returned."

* **Q:** "Why use closures?"
  **A:** "For data privacy (private state), function factories, memoization, and small DSLs without exposing internals."

---

If tu chaahe to main:

* Is file ko separate markdown files me split kar doon (exercises/solutions separate).
* Examples ko runnable Node/Browser sandbox format me bana doon.
* Kisi specific interview level ke liye tailored one-pager bana doon.

Bataa — kya next?
