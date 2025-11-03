### Function Types and Definitions
Below are the common function types in JavaScript with short definitions and examples.

1. Function Declaration (aka Function Statement)
    - Definition: A named function declared with the `function` keyword. These are hoisted (the entire function is available before its declaration).
    - Example:
    ```javascript
    function greet(name) {
         return `Hello, ${name}`;
    }
    console.log(greet('Asha'));
    ```

2. Function Expression
    - Definition: A function assigned to a variable. Can be named or anonymous. Only the variable binding is hoisted (not the initialized function when using `let`/`const`).
    - Example:
    ```javascript
    const greet = function(name) {
         return `Hi, ${name}`;
    };
    console.log(greet('Vikram'));
    ```

3. Arrow Function
    - Definition: A concise syntax using `=>`. Lexically binds `this` (does not have its own `this`). Good for short functions and callbacks.
    - Example:
    ```javascript
    const add = (a, b) => a + b;
    console.log(add(2, 3)); // 5
    ```

4. Generator Function
    - Definition: Declared with `function*`. Can yield multiple values lazily using `yield` and can be iterated.
    - Example:
    ```javascript
    function* idGenerator() {
      let id = 1;
      while(true) yield id++;
    }
    const gen = idGenerator();
    console.log(gen.next().value); // 1
    ```

5. Async Function
    - Definition: Declared with `async` and returns a Promise. Use `await` inside to wait for Promises.
    - Example:
    ```javascript
    async function fetchData() {
      const res = await fetch('/data');
      return res.json();
    }
    ```

6. Constructor Function
    - Definition: Regular function used with `new` to create objects (before `class` syntax). `this` refers to the new instance.
    - Example:
    ```javascript
    function Person(name) {
      this.name = name;
    }
    const p = new Person('Rohit');
    ```

7. Immediately Invoked Function Expression (IIFE)
    - Definition: A function that runs as soon as it is defined. Useful for creating a private scope.
    - Example:
    ```javascript
    (function() {
      console.log('IIFE runs immediately');
    })();
    ```

8. Higher-Order Function
    - Definition: A function that takes another function as an argument or returns a function.
    - Example:
    ```javascript
    function map(arr, fn) {
      const res = [];
      for (let i = 0; i < arr.length; i++) res.push(fn(arr[i]));
      return res;
    }
    console.log(map([1,2,3], x => x*2)); // [2,4,6]
    ```
### Function Hoisting (detailed)
Function hoisting describes how function declarations and variable bindings are moved to the top of their containing scope during JavaScript's compilation phase. Understanding hoisting prevents bugs caused by calling functions or accessing variables before they're defined.

- Function Declarations are hoisted with their body:
  ```javascript
  // Works: function declaration is hoisted
  console.log(square(4)); // 16
  function square(n) { return n * n; }
  ```

- Function Expressions and Arrow Functions assigned to `var` / `let` / `const` behave differently:
  - If you use `var fn = function() {}` the variable `fn` is hoisted but initialized to `undefined`, so calling `fn()` before assignment throws `TypeError: fn is not a function`.
  - With `let`/`const` the binding is in the temporal dead zone (TDZ) and accessing it before initialization throws `ReferenceError`.
  ```javascript
  // Using var
  console.log(f1); // undefined
  try { f1(); } catch (e) { console.log(e.name); } // TypeError
  var f1 = function() { console.log('f1'); };

  // Using const/let
  try { f2(); } catch (e) { console.log(e.name); } // ReferenceError (TDZ)
  const f2 = () => console.log('f2');
  ```

- Arrow functions are function expressions; they are not hoisted as declarations.

- Class methods and functions declared inside blocks are block-scoped and not hoisted to the outer scope.

Best practices regarding hoisting:
- Prefer function declarations when you intentionally want hoisting behavior and a named function.
- Prefer `const`/`let` with function expressions or arrow functions in modules for clearer initialization order.
- Avoid relying on hoisting for complex initialization; keep definitions near their usage for readability.

Examples showing pitfalls and safe patterns:
```javascript
// Pitfall (var + function expression)
try {
  fn(); // TypeError: fn is not a function
} catch (e) {
  console.log('Pitfall:', e.message);
}
var fn = function() { console.log('later'); };

// Safe: function declaration
callNow();
function callNow(){ console.log('available due to hoisting'); }
```
