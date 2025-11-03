# JavaScript Built‑in Methods — Complete README (Single File)

> **Purpose:** Ye file JavaScript ke commonly used inbuilt methods ko cover karti hai — basic se lekar practical caveats aur examples tak. Ek hi file mein rakha gaya hai taaki interview prep ya quick reference ke liye handy rahe.

---

## Kaise use karein

1. Scroll kar ke relevant section (Array, String, Object, Map/Set, Function, Promise, etc.) dekho.
2. Har method ke saath short description, example, aur important caveats diye gaye hain.
3. Code snippets mostly JavaScript (Node/browser) mein run karne layak hain.

---

## Quick note style

* `O(...)` indicates time complexity (jab applicable ho).
* "Mutates" flag agar method original value change karta hai.
* "Returns" batata hai kya value return hoti hai.
* "Use-case / Tip" practical suggestion.

---

# 1) Array methods

## Mutating methods (original array change hota hai)

### `push(...items)`

* Adds items at end. Returns new length.
* Mutates: yes. Complexity: O(k) amortized.
* Example:

```js
const a = [1,2]; a.push(3); // a = [1,2,3]
```

### `pop()`

* Removes last element, returns removed item. Mutates.
* Complexity: O(1).

### `shift()` / `unshift(...items)`

* `shift()` removes first element; `unshift` adds to front.
* Both mutate. `shift` is O(n) because it shifts elements.
* Use-case: avoid on large arrays; prefer deque-like structure.

### `splice(start, deleteCount, ...items)`

* Insert/remove at position. Very powerful but mutates.
* Complexity: O(n).
* Example:

```js
const a = [1,2,3,4]; a.splice(1,2,'x'); // a => [1,'x',4]
```

### `sort([compareFn])`

* Sorts in-place, returns sorted array. Mutates.
* Default sort uses string comparison — be careful.
* Complexity: typically O(n log n).
* Tip: always pass compare function for numeric sorts: `(a,b)=>a-b`.

### `reverse()`

* Reverses in-place. Mutates. O(n).

### `fill(value, start=0, end=this.length)`

* Fills array elements. Mutates.

## Non-mutating / pure methods

### `slice(start=0, end=this.length)`

* Returns shallow copy slice. Non-mutating. O(n).
* Example: `arr.slice()` → shallow clone.

### `concat(...arraysOrValues)`

* Returns new concatenated array. Non-mutating.

### `map(fn)`

* Returns new array with `fn` applied to each item. Does not mutate original.
* Complexity: O(n).
* Example:

```js
[1,2,3].map(x=>x*2); // [2,4,6]
```

### `filter(fn)`

* Returns filtered new array. O(n).

### `reduce(fn, initial)`

* Accumulates values into single output.
* Useful for sums, grouping, flattening.
* Example:

```js
[1,2,3].reduce((s,x)=>s+x,0); // 6
```

### `forEach(fn)`

* Iterates but returns `undefined` (side-effects only). Use when you want to mutate external state.

### `find(fn)` / `findIndex(fn)`

* `find` returns first matching element (or `undefined`). `findIndex` returns index or -1.
* Stops early (best case O(1), worst O(n)).

### `some(fn)` / `every(fn)`

* `some` returns true if any matches. `every` if all match.
* Short-circuiting behaviour.

### `includes(value, fromIndex=0)`

* Checks presence using `===` semantics (except NaN special check behaves true for NaN).
* Complexity: O(n).

### `indexOf(value)` / `lastIndexOf(value)`

* Finds index using `===`.

### `flat(depth=1)` / `flatMap(fn)`

* `flat` flattens nested arrays up to `depth`.
* `flatMap` maps then flattens one level. Non-mutating.

### `entries()` / `keys()` / `values()`

* Return iterators for [index,value], indices, values respectively. Useful with `for..of`.

### `copyWithin(target, start=0, end=this.length)`

* Copies part of array within itself. Mutates. Tricky — read docs.

### `Array.from(iterableOrArrayLike, mapFn?)`

* Create array from iterable (string, NodeList) or array-like.
* Example: `Array.from('abc') // ['a','b','c']`.

### `Array.isArray(value)`

* Checks if value is array (preferred over `instanceof` across frames).

# 2) String methods

### `charAt(index)` / `charCodeAt(index)`

* Access char or code. O(1).

### `length` (property)

* String length in UTF-16 code units. Watch surrogate pairs for emojis.

### `slice(start, end)` / `substring(start, end)`

* Both extract parts; `substring` swaps arguments if start>end. `slice` supports negative indices.

### `substr(start, length)`

* Legacy, avoid.

### `indexOf(substr, fromIndex=0)` / `lastIndexOf` / `includes(substr)`

* Find substring. `includes` returns boolean.

### `startsWith(prefix)` / `endsWith(suffix)`

* Boolean checks. Useful for protocols, file extensions, etc.

### `split(separator, limit)`

* Split into array. Useful for parsing.

### `trim()` / `trimStart()` / `trimEnd()`

* Remove whitespace.

### `toLowerCase()` / `toUpperCase()`

* Locale-insensitive basic transforms.

### `replace(searchValue, replaceValue)` / `replaceAll()`

* `replace` replaces first match (if string) or all with global regex; `replaceAll` replaces all occurrences for string patterns.
* If replacement is function, it receives matched groups — powerful.

### `repeat(count)`

* Returns repeated string. Throws on negative/too large counts.

### Template literals (backticks) — not a method, but extremely useful

```js
const name = 'Raj'; const s = `Hi ${name}`;
```

# 3) Object methods (core)

### `Object.keys(obj)` / `Object.values(obj)` / `Object.entries(obj)`

* Enumerable own-string-key properties only.
* Useful for iteration and transformations.

### `Object.assign(target, ...sources)`

* Copies enumerable own properties from sources to target. Shallow copy for nested objects.
* Watch out: overwrites properties. Mutates target.

### `Object.create(proto, [descriptors])`

* Create object with given prototype. Useful for explicit prototype chains.

### `Object.defineProperty(obj, prop, descriptor)`

* Define property with `writable`, `enumerable`, `configurable`, `get`, `set` options.
* Use-case: create non-enumerable or read-only props.

### `Object.getOwnPropertyDescriptor(obj, prop)`

* Inspect descriptor.

### `Object.getPrototypeOf(obj)` / `Object.setPrototypeOf(obj, proto)`

* Manipulate prototype. `setPrototypeOf` is slow; prefer `Object.create`.

### `Object.freeze(obj)` / `Object.seal(obj)` / `Object.preventExtensions(obj)`

* `freeze`: no add/delete/update.
* `seal`: no add/delete, but can update existing writable props.
* `preventExtensions`: can't add new props.
* Note: shallow operations only — nested objects still mutable.

### `hasOwnProperty(prop)`

* `obj.hasOwnProperty` checks own property. Safer to use `Object.prototype.hasOwnProperty.call(obj, prop)` in case `hasOwnProperty` was shadowed.

# 4) Function methods

### `call(thisArg, ...args)` / `apply(thisArg, argsArray)`

* Invoke function with explicit `this`. `apply` takes arguments as array.
* Useful for borrowing methods or controlling `this`.
* Example:

```js
function greet(g){ return `${g}, ${this.name}` }
const o = { name: 'A' };
greet.call(o, 'Hi'); // 'Hi, A'
```

### `bind(thisArg, ...prependedArgs)`

* Returns new function with fixed `this` and optionally pre-filled args. Useful for event handlers.
* Note: bound function cannot be `new`-used to create proper instances (it can, but behavior is different).

### `toString()`

* Returns function source as string. Avoid relying on it.

### Arrow functions

* Not a method but note: arrow functions capture `this` lexically (no own `this`).

# 5) Promise & async methods

### `new Promise((resolve,reject)=>{})`

* Create promise. Use `resolve`/`reject`.

### `Promise.resolve(value)` / `Promise.reject(err)`

* Create settled promises quickly.

### `Promise.all(iterable)`

* Waits for all to fulfill; if any rejects, returns rejected promise. Time complexity: O(n) in scheduling.
* Use-case: parallelize independent async operations.

### `Promise.allSettled(iterable)`

* Returns results for all (fulfilled/rejected) — good for collecting everything.

### `Promise.race(iterable)`

* Resolves/rejects to first settled promise.

### `Promise.any(iterable)`

* Resolves when any fulfills; rejects only if all reject.

### `promise.then(onFulfilled, onRejected)` / `catch` / `finally`

* Chaining and error handling. `finally` doesn't get the result unless you pass through.

### `async/await` (syntax sugar)

* Use `await` inside `async` function to write sequential-looking async code. `await` pauses execution until promise resolves.
* Error handling: wrap in `try/catch`.

# 6) Map / Set / WeakMap / WeakSet

### `Map` (ordered key-value)

* Methods: `new Map(iterable)`, `set(k,v)`, `get(k)`, `has(k)`, `delete(k)`, `clear()`, `forEach`.
* Keys can be objects.
* Iteration: `map.entries()` yields [k,v].
* Complexity: average O(1) for get/set.

### `Set`

* Unique values. Methods: `add`, `has`, `delete`, `clear`, `size`.
* Useful for deduplication.

### `WeakMap` / `WeakSet`

* Keys (WeakMap) must be objects; entries are weakly held and GC-able if no other refs.
* No iteration or `size` because entries disappear with GC.
* Use-case: private data associated with objects (memory-safe caches).

# 7) Number, Math, parse

### `Number(x)` / `parseInt(str, radix=10)` / `parseFloat(str)`

* `parseInt` parses integer prefix; specify radix to avoid surprises. `Number` is stricter.

### `Number.isNaN` vs global `isNaN`

* `Number.isNaN` checks if value is actually NaN. Global `isNaN` coerces before check.

### `Number.isInteger(x)`, `Number.isFinite(x)`

* Type-safe checks.

### `Math` methods

* `Math.floor`, `Math.ceil`, `Math.round`, `Math.trunc`, `Math.max`, `Math.min`, `Math.abs`, `Math.pow`, `Math.sqrt`, `Math.random()`.
* Tip: to get integer in range: `Math.floor(Math.random()*n)`.

# 8) JSON

### `JSON.stringify(obj, replacer?, space?)`

* Serializes to JSON. Loses functions, `undefined` in objects, symbols, and non-enumerable props.
* `replacer` useful to filter or transform values.

### `JSON.parse(str, reviver?)`

* Parse string back to object. `reviver` can convert date strings into `Date` objects etc.

# 9) RegExp

### `new RegExp(pattern, flags)` or `/pattern/flags`

* Methods: `test(str)` (boolean), `exec(str)` (match object), `str.match(regex)` (string method), `str.replace(regex, ...)`, `str.split(regex, limit)`.
* Flags: `g` (global), `i` (ignore case), `m` (multiline), `s` (dotall), `u` (unicode), `y` (sticky).
* Note: using the same `RegExp` with `g` flag across multiple `exec` calls will remember `lastIndex`.

# 10) Date

### `new Date()` / `Date.now()` / `Date.parse()`

* `toISOString()` for standard format. `getFullYear()`, `getMonth()` (0-based), `getDate()`, `getTime()` (ms since epoch).
* Date parsing differences across engines — prefer ISO format `YYYY-MM-DDTHH:mm:ss.sssZ`.

# 11) Symbol

### `Symbol(description)`

* Create unique symbol. Useful for private-ish keys.
* `Symbol.for(key)` stores/retrieves global symbol registry.

# 12) Reflect & Proxy (metaprogramming)

### `Reflect.*`

* Mirrors operations like `Reflect.get`, `Reflect.set`, `Reflect.apply` which return boolean or results and are useful inside proxies.

### `Proxy(target, handler)`

* Intercept property access, apply, construct, etc. Use-case: validation, logging, virtualized objects.
* Caution: can make code harder to reason about; use judiciously.

# 13) Typed arrays & ArrayBuffer (brief)

* `ArrayBuffer`, `Uint8Array`, `Float32Array` etc for binary data.
* Useful for performance-critical code / WebGL / networking.

# 14) Performance & common pitfalls

* Mutating vs non-mutating: prefer non-mutating for predictable code.
* Shallow copy traps: `Object.assign` and spread are shallow only.
* Avoid heavy `splice`/`shift` on large arrays in hot paths.
* `for..in` iterates inherited enumerable properties — use `Object.keys` or `for..of` with `Object.entries` for safer iteration.
* `==` vs `===`: prefer `===` to avoid coercion surprises.
* Sorting: default `Array.prototype.sort()` sorts strings; always provide comparator for numbers.
* Avoid relying on property order for objects — ES6 defines stable order for own properties but don't make fragile logic.

# 15) Small recipes (practical snippets)

### Shallow clone an object

```js
const clone = { ...obj };
// or
const clone2 = Object.assign({}, obj);
```

### Deep clone (simple, lossy)

```js
const deep = JSON.parse(JSON.stringify(obj));
```

### Debounce using inbuilt `setTimeout` (not a built-in method of objects, but common pattern)

```js
function debounce(fn, wait){ let t; return (...a)=>{ clearTimeout(t); t = setTimeout(()=>fn(...a), wait); } }
```

### Convert NodeList to array

```js
const arr = Array.from(document.querySelectorAll('div'));
```

# 16) Appendix — interview tips

* Know which methods mutate vs return new values.
* Be able to explain complexity and memory implications (shallow vs deep clone).
* For `Array` and `Object` methods, give example and side-effect notes.
* Mention edge-cases: `NaN`, `-0`, surrogate pairs in strings, circular references.

---

If tu chaahe to main is file ko:

* compress karke zip kar doon for download,
* ya Python/Java translations add kar doon for a couple of key snippets,
* ya ek condensed PDF summary bana doon.

Bataa — kaunsa next step chahiye?
