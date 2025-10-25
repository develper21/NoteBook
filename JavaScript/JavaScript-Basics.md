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
Variables are containers for storing data. In JavaScript, we have three ways to declare variables:
```javascript
let name = "Rahul";         // Can be reassigned
const age = 25;             // Cannot be reassigned
var school = "DPS";         // Old way (not recommended)
```

Real-World Example:
```javascript
// E-commerce shopping cart
let cartTotal = 0;
const taxRate = 0.18;
let itemCount = 0;
```

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

### Arithmetic Operators
```javascript
let a = 10;
let b = 5;

let sum = a + b;        // Addition: 15
let diff = a - b;       // Subtraction: 5
let product = a * b;    // Multiplication: 50
let quotient = a / b;   // Division: 2
let remainder = a % b;  // Modulus: 0
```

Real-World Example:
```javascript
// Calculate total bill in a restaurant
let foodPrice = 450;
let drinks = 100;
let taxRate = 0.05;

let subtotal = foodPrice + drinks;
let tax = subtotal * taxRate;
let totalBill = subtotal + tax;
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

### Basic Function
```javascript
// Calculate Age
function calculateAge(birthYear) {
    let currentYear = 2025;
    return currentYear - birthYear;
}

let myAge = calculateAge(1995);  // Returns 30
```

### Arrow Function
```javascript
// Calculate discount
const calculateDiscount = (price, discountPercent) => {
    return price * (discountPercent / 100);
}

let savings = calculateDiscount(1000, 20);  // Returns 200
```

Real-World Example:
```javascript
// Online booking system
const checkRoomAvailability = (checkIn, checkOut, roomType) => {
    // Check if room is available for given dates
    let isAvailable = true;
    let price = 0;
    
    if (roomType === "deluxe") {
        price = 5000;
    } else {
        price = 3000;
    }
    
    return {
        available: isAvailable,
        price: price
    };
}
```

## Arrays

### Array Methods
```javascript
// Shopping cart items
let cart = ["Shirt", "Pants", "Shoes"];

// Add item
cart.push("Watch");           // Adds at end
cart.unshift("Belt");        // Adds at beginning

// Remove item
cart.pop();                  // Removes from end
cart.shift();               // Removes from beginning

// Find item
let hasShoes = cart.includes("Shoes");  // true
```

Real-World Example:
```javascript
// Todo List Application
let todos = [
    {id: 1, task: "Buy groceries", completed: false},
    {id: 2, task: "Pay bills", completed: true},
    {id: 3, task: "Call doctor", completed: false}
];

// Filter incomplete tasks
let pendingTasks = todos.filter(todo => !todo.completed);
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
