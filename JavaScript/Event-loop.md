# JavaScript Event Loop — Complete README (Hinglish, Interview Ready)

> **Purpose (Pro Jarvis format):** Ye file event loop ka complete, interview-friendly explanation deti hai — basic se advanced, browser vs Node differences, order-of-execution examples, common pitfalls, aur practice exercises. Sab kuch Hinglish mein, practical focus.

---

## Kaise padhna hai

1. Start with "What is event loop" section to get the mental model.
2. Read call stack / task queues / microtask macrotask ka flow with examples.
3. Practice examples (timers + promises) and check the explained order.
4. Read Node vs Browser section for platform differences.
5. Solve exercises and check solutions.

---

## Quick TL;DR

* JavaScript single-threaded: ek time pe ek call stack run hota hai.
* Event loop: stack empty hone par queued tasks (macrotasks) aur microtasks ko schedule karta hai.
* Microtasks (Promise callbacks, `process.nextTick` in Node) have higher priority — they run before the next macrotask and before rendering.
* Macrotasks (task queue) include: `setTimeout`, `setInterval`, I/O callbacks, `setImmediate` (Node), message events.

---

# 1) Basic concepts

### 1.1 Call stack

* Jab function call hota hai, wo stack pe push hota hai; return hone par pop hota hai.
* Synchronous code directly uses stack. Long-running sync code blocks everything.

### 1.2 Heap

* Memory allocation area for objects. Not central to event loop but related to GC.

### 1.3 Task queues (queues of jobs to run later)

* **Macrotask queue** (task queue): timers (`setTimeout`, `setInterval`), I/O callbacks, UI rendering tasks, `setImmediate` (Node), message events.
* **Microtask queue**: promise reactions (`.then/.catch/.finally`), `queueMicrotask`, `MutationObserver` callbacks (browser), `process.nextTick` (Node - special high-priority microtask-like queue).

### 1.4 Render/paint step (browser)

* Browser may repaint between macrotasks, but microtasks run before paint. So microtasks can block rendering if they keep scheduling more microtasks.

---

# 2) Core event loop behavior (simplified)

1. Execute global script (synchronous) — this fills the call stack.
2. When stack empty, event loop picks next **macrotask** from task queue and runs it.
3. After that macrotask completes, run **all** microtasks from microtask queue until empty.
4. Then (browser only) if needed, render/paint happens.
5. Repeat: pick next macrotask, run it, drain microtasks, render, ...

**Important rule:** microtasks always run to completion before the event loop considers the next macrotask or rendering.

---

# 3) Examples — read order carefully (classic interview content)

### Example A: `setTimeout` vs Promise

```js
console.log('start');
setTimeout(()=>console.log('timeout'),0);
Promise.resolve().then(()=>console.log('promise'));
console.log('end');
```

**Output order:** `start` → `end` → `promise` → `timeout`

**Why:** sync logs run first. `Promise.then` enqueues microtask, runs after stack empty but before macrotasks. `setTimeout` is macrotask.

---

### Example B: nested microtasks

```js
Promise.resolve().then(()=>{
  console.log('p1');
  Promise.resolve().then(()=>console.log('p2'));
});
console.log('sync');
```

**Order:** `sync` → `p1` → `p2`

**Why:** when the microtask (`p1`) runs, it schedules another microtask (`p2`) which will run in the same microtask checkpoint before moving to macrotasks.

---

### Example C: heavy microtasks block rendering

```js
Promise.resolve().then(function tick(){
  for (let i=0;i<1e9;i++){}
  console.log('done');
});
```

If microtask work is heavy, it blocks rendering and macrotasks — UI jank.

---

### Example D: `process.nextTick` (Node) vs Promises

Node has special `process.nextTick` queue that runs before microtasks (Promise jobs) — so `process.nextTick` is even higher priority in Node.

```js
process.nextTick(()=>console.log('nextTick'));
Promise.resolve().then(()=>console.log('promise'));
console.log('sync');
```

**Node order:** `sync` → `nextTick` → `promise`

---

# 4) Browser vs Node: differences you must know

### Browser

* Microtasks: Promises, `queueMicrotask`, `MutationObserver`.
* Macrotasks: `setTimeout`, `setInterval`, `requestAnimationFrame` (special scheduling for next frame), event callbacks (click, message), etc.
* `requestAnimationFrame` callbacks are queued to execute before the next repaint and after microtasks; typically they are scheduled before rendering.
* Rendering occurs between macrotasks after microtasks drained.

### Node

* Event loop phases (libuv): timers, pending callbacks, idle/prepare, poll, check, close callbacks. Each phase has its own queue.
* `setImmediate` callbacks run in the check phase; `setTimeout` runs in timers phase — ordering depends on where you are in the phases.
* `process.nextTick` is special: it's run immediately after current operation, before any other microtasks.
* Promises in Node are queued to a microtask queue (next tick equivalent but lower priority than `process.nextTick`).

**Tip:** For interviews, explain conceptually rather than memorizing every phase, but mention these differences and `process.nextTick` specialness.

---

# 5) Advanced ordering puzzles (common interview nails)

### Puzzle 1

```js
console.log('A');
setTimeout(()=>console.log('B'),0);
Promise.resolve().then(()=>console.log('C'));
console.log('D');
```

Answer: A D C B

### Puzzle 2 (mix with mutationObserver)

```js
console.log('start');
setTimeout(()=>console.log('t1'));
Promise.resolve().then(()=>console.log('p1'));
const div = document.createElement('div');
const mo = new MutationObserver(()=>console.log('mo'));
mo.observe(div, { attributes: true });
div.setAttribute('x', '1');
console.log('end');
```

**Order likely:** `start` `end` `p1` `mo` `t1` — microtasks (promises + mutation observer) run before macrotasks.

### Puzzle 3 (Node subtle)

```js
setTimeout(()=>console.log('timeout'),0);
setImmediate(()=>console.log('immediate'));
```

**Order in Node:** Could be either depending on when code runs relative to timers phase; usually `setImmediate` runs before `setTimeout` if both scheduled from within I/O callbacks. Mention non-determinism unless explicit environment described.

---

# 6) Practical patterns & anti-patterns

### Use cases where understanding event loop matters

* Debouncing / throttling UI events (don't block main thread).
* Scheduling heavy work to background (Web Workers / Child processes / setImmediate in Node) to avoid UI jank.
* Correct usage of `await` and promise chains — avoid long microtask chains that starve rendering.
* Avoid `while(true)` or heavy sync loops — they block event loop.

### Anti-pattern: using `setTimeout(fn,0)` as a scheduling cure-all

* People use it to defer work, but it's macrotask-level and can still block UI; prefer `queueMicrotask` or `Promise.resolve().then()` for microtask deferral when appropriate — but microtasks can also starve rendering.

### Long job strategy

* Break big work into chunks and schedule via `requestIdleCallback` (browser), `setImmediate`/`setTimeout` (Node/browser), or `postMessage` trick for yielding to event loop.

---

# 7) Visual mental model (one-liner)

* "Call stack executes synchronous code → when empty, event loop takes a macrotask from task queue and runs it → after that macrotask finishes, it drains the microtask queue fully → then browser may render → repeat."

---

# 8) Exercises (practice, interview style)

1. Predict output for mixed code with `setTimeout`, `Promise`, `MutationObserver` and `console.log`.
2. In Node, show difference between `process.nextTick` and `Promise.then` with console logs.
3. Implement `asyncQueue(tasks, concurrency)` that runs N tasks in parallel but schedules next ones via microtasks/macrotasks to keep UI responsive.
4. Given a long synchronous loop, refactor to chunked execution allowing UI updates between chunks.

---

# 9) Solutions & hints (brief)

* Exercises: provide predicted orders and code sketches (not fully pasted here — practice them in env).
* For chunking: use `setTimeout(fn, 0)` or `requestIdleCallback` or `await new Promise(r => setTimeout(r,0))` in async loops.

---

# 10) Cheatsheet (copyable)

* Microtasks: `Promise.then`, `queueMicrotask`, `MutationObserver`, `process.nextTick`(Node special).
* Macrotasks: `setTimeout`, `setInterval`, `setImmediate`(Node), I/O, UI events.
* Order rule: microtasks drain fully after each macrotask and before rendering.
* Node quirk: `process.nextTick` runs before other microtasks.

---

## Final notes (Pro Jarvis practical advice)

* Don't just memorize outputs — run code in browser/Node REPL and watch the order. Small variations across platforms exist.
* When explaining in interviews, draw the simple diagram: Call Stack → (when empty) pick Macrotask → run → drain Microtasks → render → repeat. Mention Node vs Browser difference and `process.nextTick` / `setImmediate` caveats.

---

Agar chaahe to main is file ko:

* export karke zip bana doon,
* ya runnable examples ke saath Node/browser sandboxes prepare kar doon,
* ya ek short one-page cheatsheet bana doon for whiteboard.

Bata seedha — next kya karna hai?
