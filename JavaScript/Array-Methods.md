# JavaScript Array Methods — From Basics to Advanced

This guide covers common JavaScript Array methods (basic → advanced) with clear explanations and code examples. Each method includes: what it does, whether it mutates the original array, and a short runnable example.

---

## Quick notes
- Arrays are zero-indexed.
- Some methods mutate the original array (mutating). Others return a new array/value (non-mutating).
- Callback signatures for iterators: `(element, index, array)`.

---

## 1. Creating arrays
```javascript
// Literal
const arr = [1, 2, 3];

// Constructor (less common)
const arr2 = new Array(4); // creates [ <4 empty items> ]

// Array.of
const arr3 = Array.of(1,2,3); // [1,2,3]

// Array.from (convert iterable/array-like to array)
const s = 'hey';
const chars = Array.from(s); // ['h','e','y']
```

---

## 2. Basic mutating methods (change original array)

### push(...items)
- Adds items to the end. Returns new length.
- Mutates original.
```javascript
const a = [1,2];
a.push(3,4); // returns 4
console.log(a); // [1,2,3,4]
```

### pop()
- Removes last element and returns it.
- Mutates original.
```javascript
const a = [1,2,3];
const last = a.pop(); // 3
console.log(a); // [1,2]
```

### shift()
- Removes first element and returns it.
- Mutates original (costly on large arrays).
```javascript
const a = [1,2,3];
const first = a.shift(); // 1
console.log(a); // [2,3]
```

### unshift(...items)
- Adds items at start. Returns new length.
- Mutates original.
```javascript
const a = [2,3];
a.unshift(0,1);
console.log(a); // [0,1,2,3]
```

### splice(start, deleteCount, ...items)
- Powerful: remove/replace/insert. Returns removed elements array.
- Mutates original.
```javascript
const a = [1,2,3,4,5];
const removed = a.splice(2, 2, 'a','b');
console.log(removed); // [3,4]
console.log(a); // [1,2,'a','b',5]
```

### sort([compareFn])
- Sorts in place. Default sorts strings lexicographically.
- Mutates original.
```javascript
const nums = [10, 2, 30];
nums.sort();
console.log(nums); // [10,2,30] -> lexicographic (not numeric)

// numeric sort:
nums.sort((x,y)=>x-y);
console.log(nums); // [2,10,30]
```

### reverse()
- Reverses order in-place.
- Mutates original.
```javascript
const a = [1,2,3];
a.reverse();
console.log(a); // [3,2,1]
```

### fill(value, start=0, end=array.length)
- Fills elements with value. Mutates.
```javascript
const a = [1,2,3,4];
a.fill(0,1,3);
console.log(a); // [1,0,0,4]
```

### copyWithin(target, start=0, end=array.length)
- Copies part of array within itself. Mutates.
```javascript
const a = [1,2,3,4,5];
a.copyWithin(0, 3, 5); // copy [4,5] to start
console.log(a); // [4,5,3,4,5]
```

---

## 3. Basic non-mutating methods (return new array/value)

### slice(start=0, end=array.length)
- Returns shallow copy of a portion. Does NOT mutate.
```javascript
const a = [1,2,3,4];
const part = a.slice(1,3);
console.log(part); // [2,3]
console.log(a); // [1,2,3,4]
```

### concat(...values)
- Returns new array combining arrays/values.
```javascript
const a = [1,2];
const b = a.concat([3,4], 5);
console.log(b); // [1,2,3,4,5]
console.log(a); // [1,2]
```

### join(separator=",")
- Returns string of elements joined.
```javascript
console.log(["a","b","c"].join('-')); // 'a-b-c'
```

### toString()
- Returns comma-separated string (like join with comma).
```javascript
console.log([1,2,3].toString()); // '1,2,3'
```

### includes(value, fromIndex=0)
- Returns boolean if value present (uses SameValueZero).
```javascript
console.log([1,2,3].includes(2)); // true
```

### indexOf(value, fromIndex=0)
- Returns first index or -1.
```javascript
console.log([1,2,2,3].indexOf(2)); // 1
```

### lastIndexOf(value, fromIndex=array.length-1)
- Returns last index or -1.
```javascript
console.log([1,2,2,3].lastIndexOf(2)); // 2
```

### slice vs splice quick note
- `slice` — non-mutating, returns a new array.
- `splice` — mutating, changes the array.

---

## 4. Iteration & mapping

### forEach(callback)
- Executes callback for each element. Returns undefined.
- Does NOT return a new array (use map for that).
```javascript
[1,2,3].forEach((v,i,arr)=> console.log(i, v));
// prints: 0 1, 1 2, 2 3
```

**Note**: `forEach` does not `await` async callbacks in sequence. Use `for..of` with `await` for async operations.

### map(callback)
- Returns new array where each element transformed by callback.
```javascript
const doubled = [1,2,3].map(x => x*2);
console.log(doubled); // [2,4,6]
```

### filter(callback)
- Returns new array with elements passing test.
```javascript
const evens = [1,2,3,4].filter(x=>x%2===0);
console.log(evens); // [2,4]
```

### reduce(callback(accumulator, current, index, array), initialValue?)
- Reduces array to single value. Very powerful: sum, flatten, group, etc.
```javascript
const sum = [1,2,3].reduce((acc,x)=>acc+x, 0);
console.log(sum); // 6

// flatten 2D array
const arr2d = [[1,2],[3,4]];
const flat = arr2d.reduce((acc, cur)=> acc.concat(cur), []);
console.log(flat); // [1,2,3,4]
```

### reduceRight(callback, initial)
- Like `reduce` but from right to left.

### flat(depth=1)
- Flattens nested arrays up to depth. Returns new array.
```javascript
console.log([1,[2,[3]]].flat()); // [1,2,[3]]
console.log([1,[2,[3]]].flat(2)); // [1,2,3]
```

### flatMap(callback)
- Map then flatten by 1 level. Shortcut for map + flat(1).
```javascript
console.log([1,2,3].flatMap(x=>[x, x*2])); // [1,2,2,4,3,6]
```

### find(callback)
- Returns first element matching predicate or undefined.
```javascript
console.log([1,2,3,4].find(x=>x>2)); // 3
```

### findIndex(callback)
- Returns index of first match or -1.
```javascript
console.log([1,2,3,4].findIndex(x=>x>2)); // 2
```

### some(callback)
- Returns true if any element passes.
```javascript
console.log([1,2,3].some(x=>x===2)); // true
```

### every(callback)
- Returns true only if all elements pass.
```javascript
console.log([1,2,3].every(x=>x>0)); // true
```

---

## 5. Iterators & entries

### entries(), keys(), values()
- Return iterators.
```javascript
const a = ['x','y'];
for (const [i, v] of a.entries()) {
  console.log(i, v);
}
// 0 'x' \n 1 'y'

for (const k of a.keys()) console.log(k); // 0,1
for (const v of a.values()) console.log(v); // 'x','y'
```

### at(index)
- Returns item at index; supports negative indexing (ES2022+).
```javascript
const a = [10,20,30];
console.log(a.at(0)); // 10
console.log(a.at(-1)); // 30
```

---

## 6. Utility methods

### Array.isArray(value)
```javascript
console.log(Array.isArray([1])); // true
console.log(Array.isArray('a')); // false
```

### Array.from(iterable, mapFn?)
```javascript
console.log(Array.from('abc')); // ['a','b','c']
console.log(Array.from({length:3}, (_,i)=>i)); // [0,1,2]
```

### Array.of(...items)
```javascript
console.log(Array.of(3)); // [3]
```

### spread operator `...`
- Copy or combine arrays non-mutatively.
```javascript
const a = [1,2];
const b = [...a,3]; // [1,2,3]
```

---

## 7. Mutation vs Non-mutation quick reference
- Mutating: push, pop, shift, unshift, splice, sort, reverse, fill, copyWithin
- Non-mutating: slice, concat, map, filter, reduce, flat, flatMap, join, includes

---

## 8. Advanced patterns & examples

### 8.1 Chaining (map/filter/reduce)
```javascript
const data = [1,2,3,4,5];
const result = data
  .map(x=>x*2)
  .filter(x=>x>5)
  .reduce((acc,x)=> acc + x, 0);
console.log(result); // (map->[2,4,6,8,10]) (filter->[6,8,10]) => sum = 24
```

### 8.2 Grouping by property with reduce
```javascript
const people = [
  {name:'A', age:20},
  {name:'B', age:20},
  {name:'C', age:21}
];
const grouped = people.reduce((acc,p)=>{
  (acc[p.age] ||= []).push(p);
  return acc;
}, {});
console.log(grouped);
// { '20': [{...},{...}], '21': [{...}] }
```

### 8.3 Immutability patterns
- Use `map`, `filter`, `slice`, `concat`, spread to keep original arrays intact.
```javascript
const arr = [1,2,3];
const newArr = [...arr, 4]; // original arr unchanged
```

### 8.4 Performance tip
- For large arrays, minimize creating many intermediate arrays in tight loops. Consider using a single `reduce` or in-place mutation when safe.

---

## 9. Useful gotchas & tips
- `forEach` does not return a value; use `map` if you want a transformed array.
- `sort()` mutates: if you need an ordered copy use `arr.slice().sort()`.
- `delete arr[index]` leaves a hole (sparse array). Prefer `splice` or filter.
- `==` vs `===`: prefer `===` for equality checks.
- Async in arrays: `await` inside `forEach` won’t wait — use `for..of`.

---

## 10. Quick cheatsheet (summary)
- Create: `[...], Array.of, Array.from`
- Mutate: `push/pop/shift/unshift/splice/sort/reverse/fill/copyWithin`
- Non-mutate: `slice/concat/map/filter/reduce/flat/flatMap/join/includes/indexOf/find` 

[Back to top](#javascript-array-methods---from-basics-to-advanced)
