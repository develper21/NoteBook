# JavaScript Basics - A Complete Guide

## Table of Contents
1. [Introduction to JavaScript](#introduction)
2. [Variables and Data Types](#variables-and-data-types)
3. [Operators](#operators)
4. [Control Flow](#control-flow)
5. [Functions](#functions)
6. [Arrays](#arrays)
7. [Objects](#objects)
8. [String Methods](#string-methods)
9. [DOM Manipulation](#dom-manipulation)

## Introduction
JavaScript is a dynamic programming language that's used for web development, mobile development, desktop applications, and more. It was created to make web pages interactive and is now one of the most popular programming languages in the world.

### Real-World Usage
- Creating interactive websites
- Building web and mobile applications
- Game development
- Server-side development (Node.js)

## Variables and Data Types

### Variables

#### 1. Definition

Variables are **named containers** used to store data values in memory. They allow developers to reuse and manipulate data throughout a program.

In JavaScript, a variable can hold any type of data — numbers, strings, booleans, objects, functions, arrays, etc.

---

#### 2. Ways to Declare Variables

JavaScript provides **three** ways to declare variables:

1. **`var`** – Introduced in ES5 (legacy). Function-scoped or globally scoped.
2. **`let`** – Introduced in ES6. Block-scoped and can be reassigned.
3. **`const`** – Introduced in ES6. Block-scoped but cannot be reassigned.

Example:

```javascript
var city = "Delhi";   // Function-scoped
let name = "Rahul";   // Block-scoped, re-assignable
const age = 25;        // Block-scoped, NOT re-assignable
```

---

#### 3. Scope of Variables

**Scope** defines the accessibility or visibility of a variable.

| Type    | Scope Type                        | Description                                                                              |
| ------- | --------------------------------- | ---------------------------------------------------------------------------------------- |
| `var`   | **Function Scope / Global Scope** | Visible throughout the function or globally if declared outside any function.            |
| `let`   | **Block Scope**                   | Visible only within the nearest `{}` block (like inside loops, if blocks, or functions). |
| `const` | **Block Scope**                   | Same as `let`, but cannot be reassigned.                                                 |

Example:

```javascript
function example() {
    if (true) {
        var a = 10;   // function-scoped
        let b = 20;   // block-scoped
        const c = 30; // block-scoped
    }

    console.log(a); // Works (same function)
    console.log(b); // Error (block-scoped)
    console.log(c); // Error (block-scoped)
}
```

---

#### 4. Differences Between `var`, `let`, and `const`

##### (a) `var` vs `let`

| Feature            | `var`                                  | `let`                                            |
| ------------------ | -------------------------------------- | ------------------------------------------------ |
| **Scope**          | Function or global                     | Block                                            |
| **Hoisting**       | Hoisted (initialized as `undefined`)   | Hoisted but not initialized (Temporal Dead Zone) |
| **Re-declaration** | Allowed                                | Not allowed in the same scope                    |
| **Re-assignment**  | Allowed                                | Allowed                                          |
| **Use Case**       | Legacy code or function-wide variables | Modern, block-level variable declarations        |

Example:

```javascript
var x = 10;
var x = 20; // Allowed

let y = 10;
let y = 20; // Error: Identifier 'y' has already been declared
```

---

##### (b) `let` vs `const`

| Feature            | `let`                       | `const`                      |
| ------------------ | --------------------------- | ---------------------------- |
| **Scope**          | Block                       | Block                        |
| **Re-declaration** | Not allowed                 | Not allowed                  |
| **Re-assignment**  | Allowed                     | Not allowed                  |
| **Hoisting**       | Yes (TDZ applies)           | Yes (TDZ applies)            |
| **Use Case**       | When variable value changes | When variable value is fixed |

Example:

```javascript
let name = "Rahul";
name = "Raj"; // Allowed

const age = 25;
age = 26; // Error: Assignment to constant variable
```

---

##### (c) `var` vs `const`

| Feature            | `var`                              | `const`                                                           |
| ------------------ | ---------------------------------- | ----------------------------------------------------------------- |
| **Scope**          | Function/Global                    | Block                                                             |
| **Re-declaration** | Allowed                            | Not allowed                                                       |
| **Re-assignment**  | Allowed                            | Not allowed                                                       |
| **Hoisting**       | Yes (initialized as `undefined`)   | Yes (TDZ applies)                                                 |
| **Mutability**     | Fully mutable                      | Constant reference (can’t reassign, but object values can change) |
| **Use Case**       | Old JS style, avoid in modern code | For constants or fixed references                                 |

Example:

```javascript
var car = "BMW";
car = "Audi"; // Allowed

const bike = "Hero";
bike = "Bajaj"; // Error
```

---

#### 5. Summary Table

| Keyword | Scope           | Hoisting        | Re-declaration | Re-assignment | Best Use Case          |
| ------- | --------------- | --------------- | -------------- | ------------- | ---------------------- |
| `var`   | Function/Global | Yes (undefined) |   Yes          |  Yes         | Avoid in modern JS     |
| `let`   | Block           | Yes (TDZ)       |   No           |  Yes         | When value will change |
| `const` | Block           | Yes (TDZ)       |   No           |  No          | When value is constant |

---

#### 6. Practical Tip

* Prefer **`const`** by default.
* Use **`let`** when you know a variable's value will change.
* Avoid **`var`** unless maintaining legacy code.


### Data Types
1. **String**

```javascript
let customerName = "Priya Sharma";
let address = 'Mumbai, India';
```

2. **Number**
```javascript
let price = 999.99;
let quantity = 5;
```

3. **Boolean**
```javascript
let isLoggedIn = true;
let isAvailable = false;
```

4. **Array**
```javascript
// Menu items in a restaurant app
let menuItems = ["Dosa", "Biryani", "Naan", "Paneer"];
```

5. **Object**
```javascript
// User profile
let user = {
    name: "Amit Kumar",
    age: 28,
    city: "Delhi",
    isStudent: false
};
```

## Operators
JavaScript operators are tokens which perform computations on operands. Below are common operator types, their subtypes, and a single concise example for each subtype.

### Arithmetic Operators
Subtypes: Addition (+), Subtraction (-), Multiplication (*), Division (/), Modulus (%), Exponentiation (**), Increment (++), Decrement (--)
```javascript
let a = 10, b = 3;
console.log(a + b);   // Addition: 13
console.log(a - b);   // Subtraction: 7
console.log(a * b);   // Multiplication: 30
console.log(a / b);   // Division: 3.333...
console.log(a % b);   // Modulus: 1
console.log(a ** 2);  // Exponentiation: 100
a++;                  // Increment: a becomes 11
a--;                  // Decrement: a becomes 10 again
```

### Assignment Operators
Subtypes: =, +=, -=, *=, /=, %=, **=, <<=, >>=, >>>=, &=, ^=, |=, &&=, ||=, ??=
```javascript
let x = 5;
x += 2;   // x = x + 2  -> 7
x *= 3;   // x = x * 3  -> 21
let y = 0;
y ||= 10; // logical OR assignment (y = y || 10) -> 10
let z = null;
z ??= 4;  // nullish assignment -> 4
```

### Comparison Operators
Subtypes: ==, ===, !=, !==, >, <, >=, <=
```javascript
console.log(2 == '2');   // true  (loose equality)
console.log(2 === '2');  // false (strict equality)
console.log(5 != 3);     // true
console.log(5 !== '5');  // true
console.log(3 > 2);      // true
```

### Logical Operators
Subtypes: AND (&&), OR (||), NOT (!)
```javascript
let loggedIn = true;
let hasTicket = false;
console.log(loggedIn && hasTicket); // false
console.log(loggedIn || hasTicket); // true
console.log(!loggedIn);             // false
```

### Bitwise Operators
Subtypes: AND (&), OR (|), XOR (^), NOT (~), Left shift (<<), Right shift (>>), Zero-fill right shift (>>>)
```javascript
let p = 5;  // 0101 in binary
let q = 3;  // 0011 in binary
console.log(p & q);  // 1  (0001)
console.log(p | q);  // 7  (0111)
console.log(p ^ q);  // 6  (0110)
console.log(~p);     // -6 (bitwise NOT)
console.log(p << 1); // 10 (left shift)
```

### Unary Operators
Subtypes: typeof, void, delete, unary +, unary -, ++, --, !
```javascript
let obj = {a:1};
console.log(typeof obj); // 'object'
let arr = [1,2];
delete arr[0];           // removes element at index 0
console.log(+"42");    // unary + converts string to number -> 42
console.log(-"5");     // unary - -> -5
```

### Ternary Operator
Format: condition ? exprIfTrue : exprIfFalse
```javascript
let age = 18;
let canVote = (age >= 18) ? 'yes' : 'no'; // 'yes'
```

### Relational / Other Useful Operators
Subtypes: instanceof, in
```javascript
function Person(){}
let p = new Person();
console.log(p instanceof Person); // true
console.log('length' in [1,2,3]);  // true (arrays have length property)
```

### Optional Chaining and Nullish Coalescing
Subtypes: Optional chaining (?.), Nullish coalescing (??)
```javascript
let user = { profile: null };
console.log(user.profile?.name); // undefined (no error)
let val = null ?? 'default';     // 'default'
```

### Spread and Rest
Subtypes: Spread (...) for arrays/objects, Rest (...) in function params
```javascript
let arr1 = [1,2];
let arr2 = [...arr1, 3]; // [1,2,3]
function sum(...nums) { return nums.reduce((s,n) => s+n, 0); }
console.log(sum(1,2,3)); // 6
```

### Comma Operator
Performs multiple operations and returns last expression
```javascript
let a2 = (1, 2, 3); // a2 gets 3
```

### String Operator (Concatenation)
Subtype: + (string concatenation)
```javascript
console.log('Hello' + ' ' + 'World'); // 'Hello World'
```

### Delete Operator
Removes a property from an object
```javascript
let o = {k:1};
delete o.k; // o -> {}
```

### Composed / Compound Operators (examples)
```javascript
let n = 2;
n **= 3; // exponentiation assignment: n = n ** 3 -> 8
```

### Quick Real-World Note
Operators often combine: e.g., optional chaining + nullish coalescing for safe defaults:
```javascript
let config = { timeout: 0 };
let t = config.settings?.timeout ?? 3000; // 3000 if settings or timeout missing/null/undefined
```

## Control Flow

### If-Else Statements
```javascript
// Online shopping delivery check
let distance = 8;
let isServiceable = true;

if (distance <= 10 && isServiceable) {
    console.log("Delivery available in your area!");
} else {
    console.log("Sorry, we don't deliver to your location");
}
```

### Loops
1. **For Loop**
```javascript
// Sending notifications to multiple users
let users = ["Raj", "Neha", "Arun", "Meera"];

for (let i = 0; i < users.length; i++) {
    console.log(`Sending notification to ${users[i]}`);
}
```

2. **While Loop**
```javascript
// ATM cash withdrawal
let balance = 1000;
let withdrawalAmount = 200;

while (balance >= withdrawalAmount) {
    balance -= withdrawalAmount;
    console.log(`Withdrawn ₹${withdrawalAmount}. Remaining balance: ₹${balance}`);
}
```

## Functions

### Function — Definition
A function is a reusable block of code that performs a specific task. It can accept inputs (parameters), run code, and optionally return a value. Functions help organize code, avoid repetition, and make programs modular.

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

## Objects

### Object Methods and Properties
```javascript
// Restaurant menu item
let menuItem = {
    name: "Butter Chicken",
    price: 350,
    isVeg: false,
    ingredients: ["chicken", "butter", "spices"],
    
    displayInfo: function() {
        return `${this.name} - ₹${this.price}`;
    }
};
```

Real-World Example:
```javascript
// User Profile System
let userProfile = {
    name: "Ravi Kumar",
    email: "ravi@example.com",
    address: {
        street: "MG Road",
        city: "Bangalore",
        pincode: "560001"
    },
    orders: ["#123", "#456"],
    
    updateAddress: function(newAddress) {
        this.address = {...this.address, ...newAddress};
    }
};
```

## String Methods
```javascript
let text = "JavaScript is Amazing";

// Common string operations
let length = text.length;              // 21
let upper = text.toUpperCase();        // "JAVASCRIPT IS AMAZING"
let lower = text.toLowerCase();        // "javascript is amazing"
let words = text.split(" ");          // ["JavaScript", "is", "Amazing"]
```

Real-World Example:
```javascript
// Form input validation
function validateEmail(email) {
    email = email.trim();  // Remove extra spaces
    return email.includes("@") && email.includes(".");
}

// Format name
function formatName(name) {
    return name
        .toLowerCase()
        .split(' ')
        .map(word => word.charAt(0).toUpperCase() + word.slice(1))
        .join(' ');
}

console.log(formatName("john DOE")); // "John Doe"
```

## DOM Manipulation

### Selecting Elements
```javascript
// Get element by ID
let header = document.getElementById("header");

// Get elements by class name
let buttons = document.getElementsByClassName("btn");

// Query selector
let navbar = document.querySelector(".navbar");
```

### Modifying Elements
```javascript
// Change text content
header.textContent = "Welcome to our site!";

// Change HTML content
navbar.innerHTML = "<ul><li>Home</li><li>About</li></ul>";

// Change styles
header.style.color = "blue";
header.style.backgroundColor = "#f0f0f0";
```

Real-World Example:
```javascript
// Dynamic form validation
const validateForm = () => {
    let username = document.getElementById("username");
    let error = document.querySelector(".error-message");
    
    if (username.value.length < 3) {
        error.textContent = "Username must be at least 3 characters";
        username.style.borderColor = "red";
        return false;
    }
    
    error.textContent = "";
    username.style.borderColor = "green";
    return true;
}

// Add event listener
document.querySelector("form").addEventListener("submit", (e) => {
    if (!validateForm()) {
        e.preventDefault();
    }
});
```

## Event Handling
```javascript
// Click event
button.addEventListener("click", () => {
    console.log("Button clicked!");
});

// Form submission
form.addEventListener("submit", (event) => {
    event.preventDefault();
    // Handle form submission
});
```
 
