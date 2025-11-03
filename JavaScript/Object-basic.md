<!-- # Objects — Basic to Medium (Interview Notebook) -->

> A single-file guide covering foundational to medium-level interview topics about *Objects* across common programming languages (JavaScript, Java, Python, C++). Use this as your quick study cheat-sheet + example bank.

---

## Table of Contents

1. What is an Object? (Concept)
2. Terminology & Core Concepts
3. Objects in Different Languages — Quick Comparison
4. Creating Objects (patterns & idioms)
5. Properties, Methods, and Access Control
6. Inheritance, Polymorphism & Abstraction
7. Composition vs Inheritance
8. Object Identity, Equality & Hashing
9. Copying / Cloning (shallow vs deep)
10. Immutability & Value Objects
11. Serialization / Deserialization
12. Memory, Lifecycle & Garbage Collection
13. Common Design Patterns involving Objects
14. Language-specific Medium Topics
15. Common Interview Questions (with concise answers)
16. Practical Exercises / Mini-tasks
17. Best Practices & Pitfalls
18. Quick Cheatsheet
19. Further Reading / Resources

---

## 1. What is an Object? (Concept)

An **object** represents a bundle of state (data) and behavior (functions/methods) that models an entity or concept in your software. In OOP, objects are *instances of classes* (or prototypes, depending on language).

Key idea: Objects encapsulate state and behavior and hide implementation details behind interfaces.

---

## 2. Terminology & Core Concepts

* **Class**: A blueprint describing object's structure and behavior.
* **Instance**: A concrete object created from a class.
* **Prototype**: In prototype-based languages (JS), objects inherit directly from other objects.
* **Field / Property / Attribute**: Data stored in an object.
* **Method**: Function attached to an object.
* **Encapsulation**: Hiding internal state; exposing a public API.
* **Inheritance**: Reusing behavior by deriving from another class/object.
* **Polymorphism**: Treating different objects via a common interface.
* **Abstraction**: Exposing high-level operations and hiding implementation details.
* **Composition**: Building objects using other objects (has-a), preferred for flexibility.

---

## 3. Objects in Different Languages — Quick Comparison

* **JavaScript**: Prototype-based. Objects are flexible, can be created via object literals, constructors, `class` (ES6 sugar), `Object.create()`. Dynamic properties.
* **Java**: Class-based. Strong typing, access modifiers (`private`, `protected`, `public`), explicit constructors, interfaces and abstract classes.
* **Python**: Class-based but dynamic. Special methods (`__init__`, `__str__`, `__repr__`), duck-typing, descriptors, metaclasses.
* **C++**: Class-based with manual memory control, value semantics, RAII, copy/move semantics, destructors.

---

## 4. Creating Objects (patterns & idioms)

### Common ways

* **Constructor** (Java, C++, Python `__init__`, JS `new` with constructor function or `class`)
* **Factory function / Factory pattern** — returns configured instances; useful for encapsulating complex creation logic.
* **Builder pattern** — for immutable or complex objects with many optional fields.
* **Prototype-based creation** (`Object.create(proto)` in JS)

### Examples (short)

**JavaScript (ES6 class & factory)**

```js
class User {
  constructor(name, age){ this.name = name; this.age = age }
  greet(){ return `Hi ${this.name}` }
}

// factory
function createUser(data){
  return new User(data.name || 'Anon', data.age || 0)
}
```

**Java (builder pattern)**

```java
public class User {
  private final String name; private final int age;
  private User(Builder b){ this.name=b.name; this.age=b.age; }
  public static class Builder{ String name; int age; Builder setName(String n){name=n;return this;} ... }
}
```

---

## 5. Properties, Methods, and Access Control

* **Access modifiers** (`private`, `public`, `protected`) control visibility (important in statically-typed languages).
* **Getters / Setters** encapsulate access and allow validation.
* **Methods** should be cohesive and single-responsibility.
* **Static vs Instance members**: static belongs to class, instance to object.
* **Property descriptors (JS)**: `enumerable`, `configurable`, `writable`, getters/setters via `Object.defineProperty`.

---

## 6. Inheritance, Polymorphism & Abstraction

* **Single vs Multiple Inheritance**: Java — single class inheritance + interfaces; Python allows multiple inheritance; C++ supports multiple inheritance.
* **Polymorphism**: method overriding (runtime/dynamic) vs overloading (compile-time, language specific).
* **Liskov Substitution Principle (LSP)**: Subtypes should be substitutable for their base types.
* **Abstract classes & interfaces** define contracts without full implementations.

---

## 7. Composition vs Inheritance

* Prefer composition (has-a) over inheritance (is-a) when:

  * You want to change behavior at runtime
  * Avoiding brittle inheritance hierarchies
* Use inheritance when there is a clear "is-a" relationship and shared behavior.

Example: `Car` has an `Engine` (composition). `Car` is a `Vehicle` (inheritance) when modeling shared base behavior.

---

## 8. Object Identity, Equality & Hashing

* **Identity**: whether two references point to the same object (e.g., `===` in JS, `==` reference compare in Java for objects).
* **Equality**: logical equivalence (override `equals` in Java, `__eq__` in Python, define your comparator).
* **Hashing**: objects used as keys must implement consistent `hashCode()` (Java) following the contract: equal objects must have same hash.
* **Pitfalls**: mutable objects as keys — avoid or ensure immutability.

---

## 9. Copying / Cloning (shallow vs deep)

* **Shallow copy**: copies top-level fields; nested objects still referenced.
* **Deep copy**: recursively duplicate all nested objects.
* **Language helpers**: JS `Object.assign` (shallow), structuredClone (deep, modern), JSON `parse/stringify` (deep but lossy), lodash `cloneDeep`.
* **Java**: `Cloneable` (rarely recommended), copy constructors, serialization-based copying.
* **C++**: copy constructor, assignment operator; move semantics for efficiency.

Example (JS shallow vs deep):

```js
const a = {x:1, nested:{y:2}};
const shallow = {...a}; // nested is same ref
const deep = JSON.parse(JSON.stringify(a)); // loses functions, dates, special types
```

---

## 10. Immutability & Value Objects

* **Immutable object**: state cannot change after creation. Benefits: thread-safety, simpler reasoning, safe as map keys.
* **Value object**: equality by value, often immutable (e.g., `Money`, `Coordinate`).
* **How to create immutables**: make fields final / readonly, return new object on change (builder pattern), use defensive copies.

---

## 11. Serialization / Deserialization

* Convert objects to a transport/storage format (JSON, XML, binary).
* Issues: versioning, transient fields, cyclic references, privacy leaks.
* Best practices: explicit DTOs, schema validation, version your serialized format.

---

## 12. Memory, Lifecycle & Garbage Collection

* Objects occupy heap memory (implementation dependent).
* **Garbage collection (GC)** reclaims memory of unreachable objects.
* **Finalizers / Destructors**: C++ destructor (~Obj) and RAII pattern; Java `finalize()` deprecated — prefer `try-with-resources` and explicit close.
* **Leaks**: global caches, event listeners, closures retaining large objects.

---

## 13. Common Design Patterns involving Objects

* **Singleton** (use carefully)
* **Factory / Abstract Factory**
* **Builder** (for complex objects)
* **Prototype** (clone objects)
* **Adapter / Decorator / Strategy** (favor composition)
  Include quick code templates for the ones you use a lot.

---

## 14. Language-specific Medium Topics (practical deepening)

### JavaScript (medium)

* Prototype chain, `__proto__`, `Object.getPrototypeOf`
* `this` binding rules (global, method, call/apply/bind, arrow functions)
* Property descriptors, symbols, `Object.freeze`/`seal`
* `class` under the hood (syntactic sugar)
* Event loop & how long-running object methods affect UI
* Performance: hidden classes, object shape, property access patterns

### Java (medium)

* `equals()` / `hashCode()` contract
* `Serializable` and `transient` fields
* Immutable objects and `final` semantics
* Factory methods vs constructors, effective Java best practices
* Reflection, proxies, and dynamic proxies

### Python (medium)

* `__new__` vs `__init__`
* Data classes (`@dataclass`), `__slots__` for memory
* Descriptor protocol (`__get__`, `__set__`)
* Metaclasses (when to use)

### C++ (medium)

* Copy constructor, move constructor, assignment operator
* Rule of three/five/zero
* Virtual destructors, vtable
* Object slicing

---

## 15. Common Interview Questions (with concise answers)

**Q: What's the difference between class and object?**
A: Class is the blueprint; object is an instance.

**Q: Shallow vs deep copy?**
A: Shallow copies top-level; deep duplicates nested structures. Use deep when isolation needed; beware performance.

**Q: Explain prototypal inheritance.**
A: Objects inherit directly from other objects; functions behave as constructors that set `this` and `prototype` chain is used at property lookup.

**Q: How do you implement immutability in Java?**
A: Make class `final`, fields `private final`, initialize via constructor, avoid exposing mutable internals or return defensive copies.

**Q: Why prefer composition over inheritance?**
A: Composition provides better modularity and avoids tight coupling and fragile base class problems.

*(Include 10–15 more in your notebook; practice answering succinctly.)*

---

## 16. Practical Exercises / Mini-tasks

1. Implement deep clone for nested objects (JS) without using external libs.
2. Implement `equals()` and `hashCode()` for a Java `Person(name, dob)` class.
3. Build an immutable `Money` class (Java/Python) with currency and amount; add `add()` returning new instance.
4. Implement Singleton in Java and explain thread-safety.
5. Convert a prototype-based inheritance example to class syntax in JS and explain differences.

---

## 17. Best Practices & Pitfalls

* Keep objects **small and focused** (Single Responsibility).
* Favor **composition** for flexibility.
* Avoid mutable objects as keys in hash-based structures.
* Avoid deep inheritance hierarchies.
* Don’t overuse singletons (global state is testing pain).
* Validate inputs in setters or constructors.
* Prefer explicit interfaces (contracts) over implicit assumptions.

---

## 18. Quick Cheatsheet

* `==` vs `===` (JS): `==` coerces, `===` strict.
* `equals()` vs `==` (Java): `==` reference equality, `equals()` logical equality.
* Shallow copy: `Object.assign({}, a)` (JS) / copy ctor (C++).
* Deep copy: manual recursion / structuredClone / libraries.
* Immutable pattern: return new object vs mutate.

---

## 19. Further Reading / Resources

* *Effective Java* — Joshua Bloch (Java objects & best practices)
* *You Don’t Know JS* — Kyle Simpson (JS objects & prototypes)
* *Design Patterns* — Gamma et al. (classic patterns)
* *Fluent Python* — Luciano Ramalho (Python object model)
* Official language docs (MDN for JS, Oracle Java docs, Python docs, C++ cppreference)

---

## Closing Tips (for interview prep)

* Practice explaining concepts in 1–2 sentences, then expand with an example.
* Use whiteboard sketches: show objects, arrows (composition), inheritance trees, and lifecycle.
* When given a design question: clarify responsibilities, prefer composition, produce a minimal class diagram, and discuss trade-offs.

---

### Appendix: Short Example Snippets (JS / Java / Python / C++)

(Full runnable snippets are in your exercises; keep these as quick reminders.)

**JS: prototype vs class**

```js
// prototype
function A(name){ this.name=name }
A.prototype.greet = function(){ return this.name }
const a = new A('x')

// class sugar
class B { constructor(name){this.name=name} greet(){ return this.name } }
```

**Java: equals & hashCode (shortcut)**

```java
@Override public boolean equals(Object o){
 if(this==o) return true; if(!(o instanceof Person)) return false;
 Person p=(Person)o; return Objects.equals(name,p.name) && Objects.equals(dob,p.dob);
}
@Override public int hashCode(){ return Objects.hash(name,dob); }
```

**Python: dataclass immutable**

```python
from dataclasses import dataclass
@dataclass(frozen=True)
class Point:
    x: int
    y: int
```

**C++: Rule of Three (sketch)**

```cpp
class Foo {
 public: Foo(const Foo& other); Foo& operator=(const Foo& other); ~Foo();
};
```

---

### How to use this file

* Read section by section; keep the exercises handy.
* Convert the "Practical Exercises" into real code in your notebook.
* When prepping for interviews, try writing short answers (30–60s). Then expand to 3–5 minute whiteboard explanations.

---

*If you want, I can:*

* Expand any section into language-specific deep-dive notes (one language at a time).
* Add 10–15 interview-style questions with model answers.
* Generate small runnable project examples for practice (e.g., JS cloning utility, Java Money class).

---

*End of README — Objects (Basic → Medium)*

## Objects: Basics

### 1) What is an object?

* **Definition:** Key-value store. Keys usually strings/symbols. Values: primitives, functions (methods), other objects.
* **Use-case:** Model user, request, config etc.

**Example — literal (JS):**

```js
const user = { name: 'Aisha', age: 28, isActive: true };
```

---

### 2) Property types

* Data properties: value, writable, enumerable, configurable.
* Accessor properties: getter and setter.

**Example — accessor:**

```js
const point = {
  x: 10,
  y: 20,
  get sum() { return this.x + this.y; },
  set move({ x, y }) { this.x = x; this.y = y; }
};
point.move = { x: 2, y: 3 };
console.log(point.sum); // 5
```

---

### 3) Object creation patterns (basic)

* Object literal `{}` — simplest.
* `new Object()` — rarely used directly.
* Constructor functions:

```js
function Person(name) { this.name = name; }
const p = new Person('Raj');
```

* ES6 `class` sugar:

```js
class Person { constructor(name){ this.name = name } }
```

* `Object.create(proto)` — create with a specific prototype.

---

### 4) `this` — basic rules

* In a regular function called as method: `this` = calling object.
* Standalone function call: `this` = `undefined` in strict mode, global in non-strict.
* Constructor (`new`): `this` = newly created instance.
* `call`/`apply`/`bind` override `this`.

**Simple example:**

```js
const obj = { val: 10, getVal() { return this.val; } };
obj.getVal(); // 10
const fn = obj.getVal;
fn(); // undefined (strict mode)
```

---

### 5) Property enumeration & common methods

* `Object.keys`, `Object.values`, `Object.entries`.
* `for..in` loops enumerable own+inherited keys.
* `Object.hasOwnProperty` to check own properties.

---

### 6) Shallow copy vs Reference

* Objects are reference types. Assignment copies reference.
* `Object.assign({}, obj)` or spread `{ ...obj }` makes shallow copy.
* Shallow copy does not clone nested objects.

**Example:**

```js
const a = { x: { y: 1 } };
const b = { ...a };
b.x.y = 2; // a.x.y also 2
```

---

### 7) Serialization

* `JSON.stringify` / `JSON.parse` — note: loses functions, symbols, `undefined`, and may not handle circular refs.
* `structuredClone` (newer) handles many cases.

---

### 8) Exercises (basic)

1. Create a `Car` object literal with make, model, and a method `age(currentYear)`.
2. Convert a constructor function to ES6 `class`.
3. Show shallow copy behavior with a nested object.

(Answers in `solutions.md`)

## Objects: Medium topics

### 1) Prototype & prototype chain

* Every JS object has an internal `[[Prototype]]` (accessible via `Object.getPrototypeOf` or `__proto__`).
* Property lookup walks the prototype chain.

**Example:**

```js
const proto = { greet() { return 'hi' } };
const obj = Object.create(proto);
obj.greet(); // 'hi'
```

**Interview angle:** Explain `__proto__` vs `prototype` (function property).

---

### 2) Inheritance patterns

* Functional mixins (compose behaviors).
* Pseudo-classical (constructor + prototype methods).
* ES6 `class` extends.
* Composition over inheritance — prefer composing small functions.

**Example — composition:**

```js
const canEat = state => ({ eat: () => state.hunger-- });
const canWalk = state => ({ walk: () => state.position++ });
const createPerson = name => Object.assign({ name }, canEat({ hunger: 10 }), canWalk({ position: 0 }));
```

---

### 3) Property descriptors & control

* `writable`, `enumerable`, `configurable`.
* `Object.defineProperty` and `Object.defineProperties`.

**Example — non-enumerable:**

```js
const obj = {};
Object.defineProperty(obj, 'secret', { value: 42, enumerable: false });
console.log(Object.keys(obj)); // []
```

---

### 4) Deep cloning strategies

* `JSON.parse(JSON.stringify(obj))` — fast but lossy.
* `structuredClone(obj)` — modern, handles more types and circular refs.
* Manual recursion with Map to track references — robust.

**Snippet idea (recursive clone sketch):**

```js
function deepClone(obj, map = new Map()) {
  if (obj === null || typeof obj !== 'object') return obj;
  if (map.has(obj)) return map.get(obj);
  const copy = Array.isArray(obj) ? [] : {};
  map.set(obj, copy);
  for (const key in obj) copy[key] = deepClone(obj[key], map);
  return copy;
}
```

---

### 5) Equality: shallow vs deep

* `===` compares references for objects.
* Deep equality requires structural checks (DFS, handle cycles).
* Time complexity O(n) where n = number of nodes in object graph.

---

### 6) Memory & performance

* Large object graphs ⇒ GC pressure.
* Avoid accidental retention (closures referencing large objects).
* When to freeze/seal objects for minor optimizations.

---

### 7) Proxy & Reflect

* `Proxy` can intercept get/set/ownKeys/has and more; useful for validation, logging, or ORM-like behavior.
* `Reflect` for default behaviour.

**Example:** basic validator

```js
const person = new Proxy({}, {
  set(target, prop, value) {
    if (prop === 'age' && typeof value !== 'number') throw TypeError('age must be number');
    return Reflect.set(target, prop, value);
  }
});
person.age = 25; // ok
// person.age = 'old' => throws
```

---

### 8) Symbols & private-like keys

* Symbols give unique keys.
* New private fields `#` in classes provide true privacy at language level.

---

### 9) Iteration protocols

* `Symbol.iterator` to make objects iterable.
* Generators often used to implement iterators.

**Example:**

```js
const range = { from: 1, to: 3 };
range[Symbol.iterator] = function*() { for (let v = this.from; v <= this.to; v++) yield v; };
for (const n of range) console.log(n);
```

---

### 10) Design patterns using objects (short)

* Factory, Builder, Singleton (with caveats), Decorator, Observer, Adapter.
* Prefer composition over inheritance; discuss SOLID briefly.

---

### 11) Common tricky interview topics

* `this` inside arrow functions vs regular functions.
* Methods borrowed from object prototypes and `hasOwnProperty` shadowing.
* `for..in` vs `Object.keys` differences.
* Prototype pollution and security.

---

### 12) Exercises (medium)

1. Implement a `deepEqual(a, b)` handling cycles.
2. Build a small `Proxy` based validation wrapper for an object schema.
3. Implement `Object.cloneWithLimitedDepth(obj, depth)` — clones only up to `depth`.
4. Explain difference between `Object.freeze` and `const`.

(Answers in `solutions.md`)

### Example A — Debounced search cache (object as cache)

```js
function createSearchCache(apiFn, ttl = 5000) {
  const cache = {};
  return async function(query) {
    if (cache[query] && Date.now() - cache[query].ts < ttl) return cache[query].res;
    const res = await apiFn(query);
    cache[query] = { res, ts: Date.now() };
    return res;
  };
}
```

### Example B — Object pool (reuse heavy objects)

```js
class ConnectionPool {
  constructor(create, size=5){ this.pool = Array.from({length:size}, create); }
  acquire(){ return this.pool.pop(); }
  release(obj){ this.pool.push(obj); }
}
```

### Example C — Shallow vs Deep clone demo (browser console)

```js
const a = { x: { y: 1 }};
const shallow = { ...a };
const deep = structuredClone(a);
shallow.x.y = 2; // a.x.y === 2
deep.x.y = 3; // a.x.y stays 2
```
