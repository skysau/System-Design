# Complete Interview Preparation Sheet
*For Backend Developer, DevOps Engineer, and Full Stack Developer Positions*

## Table of Contents
1. [Node.js Interview Questions](#nodejs-interview-questions)
2. [JavaScript Advanced Concepts](#javascript-advanced-concepts)
3. [TypeScript Fundamentals](#typescript-fundamentals)
4. [Data Structures & Algorithms](#data-structures--algorithms)
5. [SQL Database Knowledge](#sql-database-knowledge)
6. [Linux Commands & DevOps](#linux-commands--devops)
7. [System Design & Architecture](#system-design--architecture)

---

## Node.js Interview Questions

### 1. What is Node.js and why is it important?

**Answer:** Node.js is a runtime environment that allows you to run JavaScript outside the browser. It expanded JavaScript's use to backend development, enabling developers to build the entire stack using one language - JavaScript.

**Key Features:**
- Non-blocking, event-driven architecture
- Single-threaded with event loop
- Highly scalable for I/O-intensive tasks
- Large ecosystem via npm

**Example Use Cases:**
- RESTful APIs
- Real-time applications (chat, gaming)
- IoT projects
- Microservices architecture

### 2. Explain the Event Loop in Node.js

**Answer:** The event loop is the core mechanism that enables Node.js to handle multiple tasks efficiently on a single thread.

**Process:**
1. Executes synchronous code
2. Delegates async operations to the OS
3. Continues with other tasks
4. Processes callbacks when operations complete

**Example:**
```javascript
console.log("1");
setTimeout(() => {
    console.log("2"), 1000;
});
console.log("3");

// Output: 1, 3, 2
```

### 3. What is npm and package.json?

**Answer:** 
- **npm:** Node Package Manager for managing dependencies
- **package.json:** Blueprint of your project containing metadata, dependencies, and scripts

**Example package.json:**
```json
{
  "name": "my-app",
  "version": "1.0.0",
  "dependencies": {
    "express": "^4.18.0",
    "mongoose": "^6.0.0"
  },
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  }
}
```

### 4. How to create an HTTP server in Node.js?

**Answer:**
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello, World!');
});

server.listen(3000, () => {
    console.log('Server running on http://localhost:3000');
});
```

### 5. Difference between require() and import?

**Answer:**
- **require():** CommonJS syntax, synchronous, used in Node.js
- **import:** ES6 modules, can be asynchronous, modern standard

**Examples:**
```javascript
// CommonJS
const fs = require('fs');

// ES6 Modules (requires "type": "module" in package.json)
import fs from 'fs';
```

### 6. Explain the fs module and sync vs async operations

**Answer:** The fs module provides tools to interact with the file system.

**Asynchronous (Recommended):**
```javascript
const fs = require('fs');

fs.readFile('example.txt', 'utf8', (err, data) => {
    if (err) {
        console.error(err);
        return;
    }
    console.log(data);
});
```

**Synchronous (Blocking):**
```javascript
const data = fs.readFileSync('example.txt', 'utf8');
console.log(data);
```

### 7. What are Modules in Node.js?

**Answer:** Modules are reusable blocks of code organized into smaller, manageable pieces.

**Types:**
- **Core modules:** Built-in (fs, http, path)
- **Local modules:** Created by you
- **Third-party modules:** From npm

**Example:**
```javascript
// math.js
function add(a, b) {
    return a + b;
}
module.exports = add;

// app.js
const add = require('./math');
console.log(add(2, 3)); // 5
```

### 8. What are Streams in Node.js?

**Answer:** Streams process data piece-by-piece, making them memory-efficient for large datasets.

**Types:**
- Readable (read data)
- Writable (write data)
- Duplex (both)
- Transform (modify data while reading/writing)

**Example:**
```javascript
const fs = require('fs');

const readableStream = fs.createReadStream('largeFile.txt', 'utf8');

readableStream.on('data', (chunk) => {
    console.log('Chunk received:', chunk);
});

readableStream.on('end', () => {
    console.log('File reading completed');
});
```

### 9. Explain Middleware in Express.js

**Answer:** Middleware functions have access to request, response objects, and the next() function. Used for logging, authentication, error handling, and parsing requests.

**Example:**
```javascript
const express = require('express');
const app = express();

// Logging middleware
app.use((req, res, next) => {
    console.log(`${req.method} request to ${req.url}`);
    next(); // Pass control to next middleware
});

// Route handler
app.get('/', (req, res) => {
    res.send('Hello, World!');
});

app.listen(3000);
```

### 10. Error Handling in Node.js

**Answer:** Multiple patterns for handling errors:

**Callbacks:**
```javascript
fs.readFile('file.txt', 'utf8', (err, data) => {
    if (err) {
        console.error('Error reading file:', err.message);
        return;
    }
    console.log(data);
});
```

**Promises:**
```javascript
fs.promises.readFile('file.txt', 'utf8')
    .then((data) => console.log(data))
    .catch((err) => console.error('Error:', err.message));
```

**Async/Await:**
```javascript
async function readFile() {
    try {
        const data = await fs.promises.readFile('file.txt', 'utf8');
        console.log(data);
    } catch (err) {
        console.error('Error:', err.message);
    }
}
```

### 11. What is Clustering in Node.js?

**Answer:** Clustering allows creating multiple instances of your application to utilize multi-core processors.

**Example:**
```javascript
const cluster = require('cluster');
const http = require('http');
const os = require('os');

if (cluster.isMaster) {
    const numCPUs = os.cpus().length;
    
    for (let i = 0; i < numCPUs; i++) {
        cluster.fork(); // Create worker process
    }
} else {
    http.createServer((req, res) => {
        res.writeHead(200);
        res.end('Hello, World!');
    }).listen(3000);
}
```

### 12. Worker Threads vs Child Processes

**Answer:**
- **Worker Threads:** Share memory, efficient for CPU-intensive tasks
- **Child Processes:** Separate memory space, good for isolation

**Worker Threads Example:**
```javascript
const { Worker, isMainThread, parentPort } = require('worker_threads');

if (isMainThread) {
    const worker = new Worker(__filename);
    worker.on('message', (msg) => console.log(`Message: ${msg}`));
} else {
    parentPort.postMessage('Hello from worker thread!');
}
```

### 13. Environment Variables in Node.js

**Answer:** Store configuration details outside your codebase for security and flexibility.

**Using dotenv:**
```bash
npm install dotenv
```

**.env file:**
```
DB_HOST=localhost
DB_USER=admin
DB_PASS=secret123
PORT=3000
```

**Usage:**
```javascript
require('dotenv').config();

const dbHost = process.env.DB_HOST;
const port = process.env.PORT || 3000;

console.log(`Connecting to database at ${dbHost}`);
```

### 14. Memory Leaks and Debugging

**Answer:** Memory leaks occur when memory is not released properly.

**Common Causes:**
- Global variables
- Event listeners not removed
- Circular references
- Large objects kept in scope

**Prevention:**
```javascript
// Remove event listeners
process.removeListener('exit', handler);

// Use weak references
const weakMap = new WeakMap();

// Monitor memory usage
console.log(process.memoryUsage());
```

### 15. JWT Authentication Implementation

**Answer:**
```javascript
const jwt = require('jsonwebtoken');

// Generate token
function generateToken(user) {
    return jwt.sign(
        { userId: user.id, email: user.email },
        process.env.JWT_SECRET,
        { expiresIn: '24h' }
    );
}

// Verify token middleware
function authenticateToken(req, res, next) {
    const authHeader = req.headers['authorization'];
    const token = authHeader && authHeader.split(' ')[1];

    if (!token) {
        return res.sendStatus(401);
    }

    jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
        if (err) return res.sendStatus(403);
        req.user = user;
        next();
    });
}
```

---

## JavaScript Advanced Concepts

### 1. JavaScript Engine & Runtime

**Answer:** A JavaScript engine translates JavaScript code into machine-executable instructions.

**Key Components:**
- **Parser:** Creates Abstract Syntax Tree (AST)
- **Interpreter:** Executes code line by line
- **Compiler:** Optimizes frequently used code
- **JIT Compiler:** Combines interpretation and compilation

**Popular Engines:**
- V8 (Chrome, Node.js)
- SpiderMonkey (Firefox)
- JavaScriptCore (Safari)

### 2. Call Stack and Memory Heap

**Answer:**
- **Call Stack:** LIFO structure tracking function execution
- **Memory Heap:** Unordered storage for objects and variables

**Example:**
```javascript
function subtractTwo(num) {
    return num - 2;
}

function calculate() {
    const sumTotal = 4 + 5;
    return subtractTwo(sumTotal);
}

calculate(); // Call stack: calculate() -> subtractTwo() -> return
```

### 3. Event Loop and Concurrency

**Answer:** JavaScript is single-threaded but achieves concurrency through the event loop.

**Components:**
- **Call Stack:** Current execution
- **Web APIs:** Browser features (setTimeout, DOM, fetch)
- **Callback Queue:** Completed async operations
- **Microtask Queue:** Higher priority (Promises)

**Example:**
```javascript
console.log("1");
setTimeout(() => console.log("2"), 0);
Promise.resolve("3").then(console.log);
console.log("4");

// Output: 1, 4, 3, 2
```

### 4. Hoisting

**Answer:** Moving variable and function declarations to the top of their scope during compilation.

**Function Hoisting:**
```javascript
// This works due to hoisting
sayHello(); // "Hello!"

function sayHello() {
    console.log("Hello!");
}
```

**Variable Hoisting:**
```javascript
console.log(x); // undefined (not ReferenceError)
var x = 5;

// let and const are hoisted but not initialized
console.log(y); // ReferenceError
let y = 10;
```

### 5. Closures

**Answer:** A function's ability to access variables from its outer scope even after the outer function has finished execution.

**Example:**
```javascript
function outerFunction(x) {
    return function innerFunction(y) {
        return x + y;
    };
}

const addFive = outerFunction(5);
console.log(addFive(3)); // 8 (still has access to x = 5)
```

**Practical Use - Encapsulation:**
```javascript
function createCounter() {
    let count = 0;
    
    return {
        increment: () => ++count,
        decrement: () => --count,
        getCount: () => count
    };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.getCount()); // 1
```

### 6. This Keyword

**Answer:** Refers to the object that is calling the function (determined at call time).

**Four Ways This is Bound:**

**1. Implicit Binding:**
```javascript
const obj = {
    name: "Alice",
    greet() {
        console.log(`Hello, ${this.name}`);
    }
};
obj.greet(); // "Hello, Alice"
```

**2. Explicit Binding:**
```javascript
function greet() {
    console.log(`Hello, ${this.name}`);
}

const person = { name: "Bob" };
greet.call(person); // "Hello, Bob"
greet.apply(person); // "Hello, Bob"
const boundGreet = greet.bind(person);
boundGreet(); // "Hello, Bob"
```

**3. New Binding:**
```javascript
function Person(name) {
    this.name = name;
}

const alice = new Person("Alice");
console.log(alice.name); // "Alice"
```

**4. Arrow Functions:**
```javascript
const obj = {
    name: "Charlie",
    greet: () => {
        console.log(this.name); // undefined (inherits from global)
    },
    greetNormal() {
        const arrow = () => {
            console.log(this.name); // "Charlie" (inherits from greetNormal)
        };
        arrow();
    }
};
```

### 7. Promises and Async/Await

**Answer:** Handle asynchronous operations cleanly.

**Promise Creation:**
```javascript
const myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const success = true;
        if (success) {
            resolve("Operation successful!");
        } else {
            reject("Operation failed!");
        }
    }, 1000);
});
```

**Promise Patterns:**
```javascript
// Parallel execution
const promises = [fetchUser(), fetchPosts(), fetchComments()];
Promise.all(promises).then(results => {
    console.log("All completed:", results);
});

// Sequential execution
async function sequential() {
    const user = await fetchUser();
    const posts = await fetchPosts(user.id);
    const comments = await fetchComments(posts[0].id);
    return { user, posts, comments };
}

// Race (first to complete)
Promise.race([fastAPI(), slowAPI()]).then(result => {
    console.log("First completed:", result);
});
```

### 8. Prototypal Inheritance

**Answer:** Objects inherit properties and methods from other objects through prototypes.

**Example:**
```javascript
// Constructor function
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    console.log(`${this.name} makes a sound`);
};

function Dog(name, breed) {
    Animal.call(this, name);
    this.breed = breed;
}

// Inherit from Animal
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
    console.log(`${this.name} barks!`);
};

const dog = new Dog("Rex", "German Shepherd");
dog.speak(); // "Rex makes a sound"
dog.bark(); // "Rex barks!"
```

### 9. ES6+ Features

**Destructuring:**
```javascript
// Array destructuring
const [first, second, ...rest] = [1, 2, 3, 4, 5];

// Object destructuring
const { name, age, ...otherProps } = person;

// Function parameters
function greet({ name, age = 25 }) {
    console.log(`Hello ${name}, you are ${age}`);
}
```

**Template Literals:**
```javascript
const name = "Alice";
const age = 30;
const message = `Hello ${name}, you are ${age} years old.
This is a multi-line string.`;
```

**Arrow Functions:**
```javascript
// Traditional function
const add = function(a, b) {
    return a + b;
};

// Arrow function
const add = (a, b) => a + b;

// With single parameter
const double = x => x * 2;

// With no parameters
const greet = () => console.log("Hello!");
```

### 10. Modules (ES6)

**Named Exports:**
```javascript
// math.js
export const PI = 3.14159;
export function add(a, b) {
    return a + b;
}
export function multiply(a, b) {
    return a * b;
}

// main.js
import { PI, add, multiply } from './math.js';
```

**Default Exports:**
```javascript
// calculator.js
class Calculator {
    add(a, b) { return a + b; }
    subtract(a, b) { return a - b; }
}
export default Calculator;

// main.js
import Calculator from './calculator.js';
```

---

## TypeScript Fundamentals

### 1. Basic Types and Annotations

**Answer:** TypeScript adds static typing to JavaScript for better development experience.

**Basic Types:**
```typescript
// Primitive types
let name: string = "Alice";
let age: number = 30;
let isActive: boolean = true;
let items: number[] = [1, 2, 3];
let tuple: [string, number] = ["Alice", 30];

// Object type
interface User {
    name: string;
    age: number;
    email?: string; // Optional property
}

const user: User = {
    name: "Bob",
    age: 25
};
```

### 2. Functions in TypeScript

**Answer:**
```typescript
// Function declaration
function add(a: number, b: number): number {
    return a + b;
}

// Arrow function
const multiply = (a: number, b: number): number => a * b;

// Optional parameters
function greet(name: string, title?: string): string {
    return title ? `Hello ${title} ${name}` : `Hello ${name}`;
}

// Default parameters
function createUser(name: string, age: number = 18): User {
    return { name, age };
}

// Rest parameters
function sum(...numbers: number[]): number {
    return numbers.reduce((total, num) => total + num, 0);
}
```

### 3. Interfaces and Classes

**Interfaces:**
```typescript
interface Shape {
    area(): number;
    perimeter(): number;
}

interface Drawable {
    draw(): void;
}

// Multiple interface inheritance
class Rectangle implements Shape, Drawable {
    constructor(
        private width: number,
        private height: number
    ) {}

    area(): number {
        return this.width * this.height;
    }

    perimeter(): number {
        return 2 * (this.width + this.height);
    }

    draw(): void {
        console.log("Drawing rectangle");
    }
}
```

**Classes:**
```typescript
class Animal {
    protected name: string;

    constructor(name: string) {
        this.name = name;
    }

    protected makeSound(): void {
        console.log("Some generic sound");
    }
}

class Dog extends Animal {
    private breed: string;

    constructor(name: string, breed: string) {
        super(name);
        this.breed = breed;
    }

    public bark(): void {
        console.log(`${this.name} barks!`);
        this.makeSound();
    }
}
```

### 4. Generics

**Answer:** Allow creating reusable components that work with multiple types.

**Generic Functions:**
```typescript
function identity<T>(arg: T): T {
    return arg;
}

const stringResult = identity<string>("hello");
const numberResult = identity<number>(42);

// Generic with constraints
interface Lengthwise {
    length: number;
}

function logLength<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}

logLength("hello"); // Works
logLength([1, 2, 3]); // Works
// logLength(123); // Error: number doesn't have length
```

**Generic Classes:**
```typescript
class Stack<T> {
    private items: T[] = [];

    push(item: T): void {
        this.items.push(item);
    }

    pop(): T | undefined {
        return this.items.pop();
    }

    peek(): T | undefined {
        return this.items[this.items.length - 1];
    }

    isEmpty(): boolean {
        return this.items.length === 0;
    }
}

const numberStack = new Stack<number>();
const stringStack = new Stack<string>();
```

### 5. Union Types and Type Guards

**Union Types:**
```typescript
type Status = "loading" | "success" | "error";
type ID = string | number;

function processId(id: ID): string {
    if (typeof id === "string") {
        return id.toUpperCase();
    } else {
        return id.toString();
    }
}
```

**Type Guards:**
```typescript
interface Bird {
    fly(): void;
    layEggs(): void;
}

interface Fish {
    swim(): void;
    layEggs(): void;
}

type Pet = Bird | Fish;

// Type predicate function
function isBird(pet: Pet): pet is Bird {
    return (pet as Bird).fly !== undefined;
}

function moveAnimal(pet: Pet) {
    if (isBird(pet)) {
        pet.fly(); // TypeScript knows pet is Bird
    } else {
        pet.swim(); // TypeScript knows pet is Fish
    }
}
```

### 6. Utility Types

**Answer:**
```typescript
interface User {
    id: number;
    name: string;
    email: string;
    password: string;
    isActive: boolean;
}

// Pick - Select specific properties
type PublicUser = Pick<User, "id" | "name" | "email">;

// Omit - Exclude specific properties
type CreateUser = Omit<User, "id">;

// Partial - Make all properties optional
type UpdateUser = Partial<User>;

// Required - Make all properties required
type CompleteUser = Required<User>;

// Readonly - Make all properties read-only
type ImmutableUser = Readonly<User>;

// Record - Create object type with specific key-value types
type UserRoles = Record<string, string[]>;
const roles: UserRoles = {
    admin: ["read", "write", "delete"],
    user: ["read"]
};
```

### 7. Async/Await with TypeScript

**Answer:**
```typescript
interface ApiResponse<T> {
    data: T;
    status: number;
    message: string;
}

interface User {
    id: number;
    name: string;
    email: string;
}

async function fetchUser(id: number): Promise<ApiResponse<User>> {
    try {
        const response = await fetch(`/api/users/${id}`);
        const data = await response.json();
        
        return {
            data,
            status: response.status,
            message: "Success"
        };
    } catch (error) {
        throw new Error(`Failed to fetch user: ${error}`);
    }
}

// Usage
async function handleUser() {
    try {
        const result = await fetchUser(1);
        console.log(result.data.name);
    } catch (error) {
        console.error(error.message);
    }
}
```

---

## Data Structures & Algorithms

### 1. Big O Notation

**Answer:** Describes the performance or complexity of an algorithm.

**Common Time Complexities:**
- **O(1)** - Constant: Array access, hash table lookup
- **O(log n)** - Logarithmic: Binary search
- **O(n)** - Linear: Simple loops, linear search
- **O(n log n)** - Linearithmic: Merge sort, quick sort
- **O(n²)** - Quadratic: Nested loops, bubble sort
- **O(2^n)** - Exponential: Recursive fibonacci
- **O(n!)** - Factorial: Permutations

**Examples:**
```javascript
// O(1) - Constant
function getFirst(array) {
    return array[0];
}

// O(n) - Linear
function findMax(array) {
    let max = array[0];
    for (let i = 1; i < array.length; i++) {
        if (array[i] > max) {
            max = array[i];
        }
    }
    return max;
}

// O(n²) - Quadratic
function bubbleSort(array) {
    for (let i = 0; i < array.length; i++) {
        for (let j = 0; j < array.length - 1 - i; j++) {
            if (array[j] > array[j + 1]) {
                [array[j], array[j + 1]] = [array[j + 1], array[j]];
            }
        }
    }
    return array;
}
```

### 2. Arrays

**Answer:** Collection of elements stored in contiguous memory locations.

**Time Complexities:**
- Access: O(1)
- Search: O(n)
- Insertion: O(n)
- Deletion: O(n)

**Common Operations:**
```javascript
// Array methods and their complexities
const arr = [1, 2, 3, 4, 5];

// O(1) operations
arr.push(6);        // Add to end
arr.pop();          // Remove from end
arr[0];             // Access by index

// O(n) operations
arr.unshift(0);     // Add to beginning
arr.shift();        // Remove from beginning
arr.indexOf(3);     // Find element
arr.splice(2, 1);   // Remove from middle
```

### 3. Hash Tables / Objects

**Answer:** Data structure that maps keys to values using a hash function.

**Time Complexities:**
- Search: Average O(1), Worst O(n)
- Insertion: Average O(1), Worst O(n)
- Deletion: Average O(1), Worst O(n)

**Implementation:**
```javascript
class HashTable {
    constructor(size = 50) {
        this.size = size;
        this.buckets = new Array(size);
    }

    hash(key) {
        let hash = 0;
        for (let i = 0; i < key.length; i++) {
            hash += key.charCodeAt(i);
        }
        return hash % this.size;
    }

    set(key, value) {
        const index = this.hash(key);
        if (!this.buckets[index]) {
            this.buckets[index] = [];
        }
        
        const bucket = this.buckets[index];
        const existingPair = bucket.find(pair => pair[0] === key);
        
        if (existingPair) {
            existingPair[1] = value;
        } else {
            bucket.push([key, value]);
        }
    }

    get(key) {
        const index = this.hash(key);
        const bucket = this.buckets[index];
        
        if (bucket) {
            const pair = bucket.find(pair => pair[0] === key);
            return pair ? pair[1] : undefined;
        }
        return undefined;
    }
}
```

### 4. Linked Lists

**Answer:** Linear data structure where elements are stored in nodes, each containing data and a pointer to the next node.

**Time Complexities:**
- Access: O(n)
- Search: O(n)
- Insertion: O(1) at head/tail, O(n) at position
- Deletion: O(1) at head, O(n) at position

**Implementation:**
```javascript
class ListNode {
    constructor(data) {
        this.data = data;
        this.next = null;
    }
}

class LinkedList {
    constructor() {
        this.head = null;
        this.size = 0;
    }

    // Add to beginning - O(1)
    prepend(data) {
        const newNode = new ListNode(data);
        newNode.next = this.head;
        this.head = newNode;
        this.size++;
    }

    // Add to end - O(n)
    append(data) {
        const newNode = new ListNode(data);
        
        if (!this.head) {
            this.head = newNode;
        } else {
            let current = this.head;
            while (current.next) {
                current = current.next;
            }
            current.next = newNode;
        }
        this.size++;
    }

    // Remove by value - O(n)
    remove(data) {
        if (!this.head) return false;

        if (this.head.data === data) {
            this.head = this.head.next;
            this.size--;
            return true;
        }

        let current = this.head;
        while (current.next && current.next.data !== data) {
            current = current.next;
        }

        if (current.next) {
            current.next = current.next.next;
            this.size--;
            return true;
        }
        return false;
    }

    // Reverse linked list
    reverse() {
        let prev = null;
        let current = this.head;
        
        while (current) {
            const next = current.next;
            current.next = prev;
            prev = current;
            current = next;
        }
        
        this.head = prev;
        return this;
    }
}
```

### 5. Stacks and Queues

**Stacks (LIFO - Last In, First Out):**
```javascript
class Stack {
    constructor() {
        this.items = [];
    }

    // Add to top - O(1)
    push(element) {
        this.items.push(element);
    }

    // Remove from top - O(1)
    pop() {
        if (this.isEmpty()) return null;
        return this.items.pop();
    }

    // View top element - O(1)
    peek() {
        if (this.isEmpty()) return null;
        return this.items[this.items.length - 1];
    }

    isEmpty() {
        return this.items.length === 0;
    }

    size() {
        return this.items.length;
    }
}

// Use cases: Function call stack, undo operations, browser history
```

**Queues (FIFO - First In, First Out):**
```javascript
class Queue {
    constructor() {
        this.items = [];
    }

    // Add to rear - O(1)
    enqueue(element) {
        this.items.push(element);
    }

    // Remove from front - O(n) with array, O(1) with linked list
    dequeue() {
        if (this.isEmpty()) return null;
        return this.items.shift();
    }

    // View front element - O(1)
    front() {
        if (this.isEmpty()) return null;
        return this.items[0];
    }

    isEmpty() {
        return this.items.length === 0;
    }

    size() {
        return this.items.length;
    }
}

// Use cases: Task scheduling, BFS traversal, handling requests
```

### 6. Binary Trees

**Answer:** Hierarchical data structure where each node has at most two children.

**Binary Search Tree Implementation:**
```javascript
class TreeNode {
    constructor(value) {
        this.value = value;
        this.left = null;
        this.right = null;
    }
}

class BinarySearchTree {
    constructor() {
        this.root = null;
    }

    // Insert - Average O(log n), Worst O(n)
    insert(value) {
        this.root = this.insertNode(this.root, value);
    }

    insertNode(node, value) {
        if (!node) {
            return new TreeNode(value);
        }

        if (value < node.value) {
            node.left = this.insertNode(node.left, value);
        } else if (value > node.value) {
            node.right = this.insertNode(node.right, value);
        }

        return node;
    }

    // Search - Average O(log n), Worst O(n)
    search(value) {
        return this.searchNode(this.root, value);
    }

    searchNode(node, value) {
        if (!node || node.value === value) {
            return node;
        }

        if (value < node.value) {
            return this.searchNode(node.left, value);
        } else {
            return this.searchNode(node.right, value);
        }
    }

    // Traversals
    inOrderTraversal(node = this.root, result = []) {
        if (node) {
            this.inOrderTraversal(node.left, result);
            result.push(node.value);
            this.inOrderTraversal(node.right, result);
        }
        return result;
    }

    preOrderTraversal(node = this.root, result = []) {
        if (node) {
            result.push(node.value);
            this.preOrderTraversal(node.left, result);
            this.preOrderTraversal(node.right, result);
        }
        return result;
    }

    postOrderTraversal(node = this.root, result = []) {
        if (node) {
            this.postOrderTraversal(node.left, result);
            this.postOrderTraversal(node.right, result);
            result.push(node.value);
        }
        return result;
    }
}
```

### 7. Sorting Algorithms

**Bubble Sort - O(n²):**
```javascript
function bubbleSort(arr) {
    const n = arr.length;
    for (let i = 0; i < n - 1; i++) {
        for (let j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
            }
        }
    }
    return arr;
}
```

**Quick Sort - Average O(n log n), Worst O(n²):**
```javascript
function quickSort(arr, left = 0, right = arr.length - 1) {
    if (left < right) {
        const pivotIndex = partition(arr, left, right);
        quickSort(arr, left, pivotIndex - 1);
        quickSort(arr, pivotIndex + 1, right);
    }
    return arr;
}

function partition(arr, left, right) {
    const pivot = arr[right];
    let i = left;

    for (let j = left; j < right; j++) {
        if (arr[j] <= pivot) {
            [arr[i], arr[j]] = [arr[j], arr[i]];
            i++;
        }
    }

    [arr[i], arr[right]] = [arr[right], arr[i]];
    return i;
}
```

**Merge Sort - O(n log n):**
```javascript
function mergeSort(arr) {
    if (arr.length <= 1) return arr;

    const mid = Math.floor(arr.length / 2);
    const left = mergeSort(arr.slice(0, mid));
    const right = mergeSort(arr.slice(mid));

    return merge(left, right);
}

function merge(left, right) {
    const result = [];
    let leftIndex = 0;
    let rightIndex = 0;

    while (leftIndex < left.length && rightIndex < right.length) {
        if (left[leftIndex] <= right[rightIndex]) {
            result.push(left[leftIndex]);
            leftIndex++;
        } else {
            result.push(right[rightIndex]);
            rightIndex++;
        }
    }

    return result
        .concat(left.slice(leftIndex))
        .concat(right.slice(rightIndex));
}
```

### 8. Search Algorithms

**Binary Search - O(log n):**
```javascript
function binarySearch(arr, target) {
    let left = 0;
    let right = arr.length - 1;

    while (left <= right) {
        const mid = Math.floor((left + right) / 2);

        if (arr[mid] === target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    return -1; // Not found
}
```

**Depth-First Search (DFS):**
```javascript
// For trees
function dfsRecursive(node, target) {
    if (!node) return false;
    if (node.value === target) return true;

    return dfsRecursive(node.left, target) || 
           dfsRecursive(node.right, target);
}

// For graphs
function dfsGraph(graph, start, target, visited = new Set()) {
    if (start === target) return true;
    
    visited.add(start);
    
    for (const neighbor of graph[start]) {
        if (!visited.has(neighbor)) {
            if (dfsGraph(graph, neighbor, target, visited)) {
                return true;
            }
        }
    }
    
    return false;
}
```

**Breadth-First Search (BFS):**
```javascript
function bfsTree(root, target) {
    if (!root) return false;
    
    const queue = [root];
    
    while (queue.length > 0) {
        const node = queue.shift();
        
        if (node.value === target) return true;
        
        if (node.left) queue.push(node.left);
        if (node.right) queue.push(node.right);
    }
    
    return false;
}

function bfsGraph(graph, start, target) {
    const queue = [start];
    const visited = new Set([start]);
    
    while (queue.length > 0) {
        const node = queue.shift();
        
        if (node === target) return true;
        
        for (const neighbor of graph[node]) {
            if (!visited.has(neighbor)) {
                visited.add(neighbor);
                queue.push(neighbor);
            }
        }
    }
    
    return false;
}
```

### 9. Dynamic Programming

**Answer:** Optimization technique that solves problems by breaking them into smaller subproblems and storing results.

**Fibonacci with Memoization:**
```javascript
// Without memoization - O(2^n)
function fibonacciSlow(n) {
    if (n <= 1) return n;
    return fibonacciSlow(n - 1) + fibonacciSlow(n - 2);
}

// With memoization - O(n)
function fibonacciMemo(n, memo = {}) {
    if (n in memo) return memo[n];
    if (n <= 1) return n;
    
    memo[n] = fibonacciMemo(n - 1, memo) + fibonacciMemo(n - 2, memo);
    return memo[n];
}

// Bottom-up approach - O(n)
function fibonacciDP(n) {
    if (n <= 1) return n;
    
    const dp = [0, 1];
    
    for (let i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    
    return dp[n];
}
```

### 10. Common Algorithm Patterns

**Two Pointers:**
```javascript
// Check if string is palindrome
function isPalindrome(s) {
    let left = 0;
    let right = s.length - 1;
    
    while (left < right) {
        if (s[left] !== s[right]) {
            return false;
        }
        left++;
        right--;
    }
    
    return true;
}

// Two sum in sorted array
function twoSum(nums, target) {
    let left = 0;
    let right = nums.length - 1;
    
    while (left < right) {
        const sum = nums[left] + nums[right];
        
        if (sum === target) {
            return [left, right];
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    
    return [];
}
```

**Sliding Window:**
```javascript
// Maximum sum of subarray of size k
function maxSubArraySum(arr, k) {
    if (arr.length < k) return null;
    
    let maxSum = 0;
    let windowSum = 0;
    
    // Calculate sum of first window
    for (let i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    maxSum = windowSum;
    
    // Slide the window
    for (let i = k; i < arr.length; i++) {
        windowSum = windowSum - arr[i - k] + arr[i];
        maxSum = Math.max(maxSum, windowSum);
    }
    
    return maxSum;
}
```

---

## SQL Database Knowledge

### 1. Basic SQL Syntax and Queries

**SELECT Statements:**
```sql
-- Basic selection
SELECT column1, column2 FROM table_name;

-- Select all columns
SELECT * FROM users;

-- With conditions
SELECT name, email FROM users 
WHERE age > 18 AND status = 'active';

-- Ordering results
SELECT * FROM products 
ORDER BY price DESC, name ASC;

-- Limiting results
SELECT * FROM users 
ORDER BY created_at DESC 
LIMIT 10;

-- Distinct values
SELECT DISTINCT department FROM employees;
```

**Filtering with WHERE:**
```sql
-- Comparison operators
SELECT * FROM products WHERE price > 100;
SELECT * FROM users WHERE name = 'John';
SELECT * FROM orders WHERE date >= '2023-01-01';

-- Pattern matching
SELECT * FROM users WHERE name LIKE 'A%';  -- Starts with A
SELECT * FROM users WHERE email LIKE '%@gmail.com';  -- Ends with @gmail.com
SELECT * FROM products WHERE name LIKE '%phone%';  -- Contains 'phone'

-- IN and BETWEEN
SELECT * FROM users WHERE age IN (18, 21, 25);
SELECT * FROM products WHERE price BETWEEN 50 AND 200;

-- NULL checks
SELECT * FROM users WHERE phone IS NULL;
SELECT * FROM users WHERE phone IS NOT NULL;
```

### 2. Aggregate Functions

```sql
-- Count records
SELECT COUNT(*) FROM users;
SELECT COUNT(email) FROM users;  -- Excludes NULL values
SELECT COUNT(DISTINCT department) FROM employees;

-- Sum and Average
SELECT SUM(price) FROM products;
SELECT AVG(salary) FROM employees;
SELECT SUM(quantity * price) as total_value FROM order_items;

-- Min and Max
SELECT MIN(price), MAX(price) FROM products;
SELECT MIN(hire_date), MAX(hire_date) FROM employees;

-- Group by with aggregates
SELECT department, COUNT(*), AVG(salary)
FROM employees
GROUP BY department;

-- Having clause (filtering groups)
SELECT department, COUNT(*) as emp_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

### 3. Joins

**Inner Join:**
```sql
-- Basic inner join
SELECT u.name, p.title
FROM users u
INNER JOIN posts p ON u.id = p.user_id;

-- Multiple table join
SELECT u.name, p.title, c.content
FROM users u
INNER JOIN posts p ON u.id = p.user_id
INNER JOIN comments c ON p.id = c.post_id;
```

**Left Join:**
```sql
-- Get all users with their posts (including users with no posts)
SELECT u.name, p.title
FROM users u
LEFT JOIN posts p ON u.id = p.user_id;

-- Count posts per user
SELECT u.name, COUNT(p.id) as post_count
FROM users u
LEFT JOIN posts p ON u.id = p.user_id
GROUP BY u.id, u.name;
```

**Right Join:**
```sql
-- Get all posts with user info (including orphaned posts)
SELECT u.name, p.title
FROM users u
RIGHT JOIN posts p ON u.id = p.user_id;
```

**Full Outer Join:**
```sql
-- Get all users and all posts
SELECT u.name, p.title
FROM users u
FULL OUTER JOIN posts p ON u.id = p.user_id;
```

### 4. Subqueries

```sql
-- Subquery in WHERE clause
SELECT name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary) FROM employees
);

-- Subquery with IN
SELECT name
FROM users
WHERE id IN (
    SELECT user_id FROM orders WHERE total > 1000
);

-- Correlated subquery
SELECT e1.name, e1.salary
FROM employees e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department = e1.department
);

-- EXISTS
SELECT name
FROM users u
WHERE EXISTS (
    SELECT 1 FROM orders o WHERE o.user_id = u.id
);
```

### 5. Data Manipulation (DML)

**INSERT:**
```sql
-- Insert single record
INSERT INTO users (name, email, age)
VALUES ('John Doe', 'john@example.com', 25);

-- Insert multiple records
INSERT INTO users (name, email, age)
VALUES 
    ('Jane Smith', 'jane@example.com', 30),
    ('Bob Johnson', 'bob@example.com', 28);

-- Insert from another table
INSERT INTO active_users (name, email)
SELECT name, email FROM users WHERE status = 'active';
```

**UPDATE:**
```sql
-- Update single record
UPDATE users 
SET email = 'newemail@example.com' 
WHERE id = 1;

-- Update multiple columns
UPDATE users 
SET email = 'updated@example.com', age = 26 
WHERE name = 'John Doe';

-- Update with JOIN
UPDATE users u
SET u.last_login = o.order_date
FROM users u
INNER JOIN orders o ON u.id = o.user_id
WHERE o.order_date = (
    SELECT MAX(order_date) 
    FROM orders 
    WHERE user_id = u.id
);
```

**DELETE:**
```sql
-- Delete specific records
DELETE FROM users WHERE age < 18;

-- Delete with JOIN
DELETE u
FROM users u
INNER JOIN orders o ON u.id = o.user_id
WHERE o.status = 'cancelled';

-- Delete all records (but keep table structure)
DELETE FROM users;
```

### 6. Data Definition Language (DDL)

**CREATE TABLE:**
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    age INTEGER CHECK (age >= 0),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- With foreign key
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    content TEXT,
    user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**ALTER TABLE:**
```sql
-- Add column
ALTER TABLE users ADD COLUMN phone VARCHAR(20);

-- Modify column
ALTER TABLE users ALTER COLUMN name TYPE VARCHAR(150);

-- Drop column
ALTER TABLE users DROP COLUMN phone;

-- Add constraint
ALTER TABLE users ADD CONSTRAINT unique_email UNIQUE (email);

-- Add foreign key
ALTER TABLE posts 
ADD CONSTRAINT fk_user 
FOREIGN KEY (user_id) REFERENCES users(id);
```

**INDEXES:**
```sql
-- Create index
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_posts_user_id ON posts(user_id);

-- Composite index
CREATE INDEX idx_posts_user_date ON posts(user_id, created_at);

-- Unique index
CREATE UNIQUE INDEX idx_users_unique_email ON users(email);

-- Drop index
DROP INDEX idx_users_email;
```

### 7. Views

```sql
-- Create view
CREATE VIEW active_users AS
SELECT id, name, email
FROM users
WHERE status = 'active' AND deleted_at IS NULL;

-- Use view
SELECT * FROM active_users WHERE age > 25;

-- Update view (if updatable)
UPDATE active_users SET email = 'new@example.com' WHERE id = 1;

-- Drop view
DROP VIEW active_users;
```

### 8. Common Table Expressions (CTEs)

```sql
-- Basic CTE
WITH sales_summary AS (
    SELECT 
        product_id,
        SUM(quantity) as total_quantity,
        SUM(price * quantity) as total_revenue
    FROM order_items
    GROUP BY product_id
)
SELECT p.name, s.total_quantity, s.total_revenue
FROM products p
JOIN sales_summary s ON p.id = s.product_id;

-- Recursive CTE (for hierarchical data)
WITH RECURSIVE employee_hierarchy AS (
    -- Base case: top-level managers
    SELECT id, name, manager_id, 0 as level
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive case: employees with managers
    SELECT e.id, e.name, e.manager_id, eh.level + 1
    FROM employees e
    JOIN employee_hierarchy eh ON e.manager_id = eh.id
)
SELECT * FROM employee_hierarchy ORDER BY level, name;
```

### 9. Window Functions

```sql
-- Row number
SELECT 
    name, 
    salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) as rank
FROM employees;

-- Ranking functions
SELECT 
    name, 
    department,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) as dept_rank,
    DENSE_RANK() OVER (ORDER BY salary DESC) as overall_rank
FROM employees;

-- Running totals
SELECT 
    order_date,
    amount,
    SUM(amount) OVER (ORDER BY order_date) as running_total
FROM orders;

-- Moving averages
SELECT 
    order_date,
    amount,
    AVG(amount) OVER (
        ORDER BY order_date 
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) as moving_avg_3_days
FROM orders;

-- Lead and Lag
SELECT 
    order_date,
    amount,
    LAG(amount, 1) OVER (ORDER BY order_date) as prev_amount,
    LEAD(amount, 1) OVER (ORDER BY order_date) as next_amount
FROM orders;
```

### 10. Database Design and Normalization

**First Normal Form (1NF):**
- Each column contains atomic values
- Each column contains values of single type
- Each column has unique name
- Order doesn't matter

**Second Normal Form (2NF):**
- Must be in 1NF
- No partial dependencies (all non-key attributes fully dependent on primary key)

**Third Normal Form (3NF):**
- Must be in 2NF
- No transitive dependencies (non-key attributes don't depend on other non-key attributes)

**Example of Normalization:**
```sql
-- Unnormalized table
CREATE TABLE orders_bad (
    order_id INT,
    customer_name VARCHAR(100),
    customer_email VARCHAR(100),
    customer_phone VARCHAR(20),
    product_names TEXT, -- Multiple products in one field
    product_prices TEXT  -- Multiple prices in one field
);

-- Normalized tables
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    phone VARCHAR(20)
);

CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL
);

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(customer_id),
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE order_items (
    order_item_id SERIAL PRIMARY KEY,
    order_id INT REFERENCES orders(order_id),
    product_id INT REFERENCES products(product_id),
    quantity INT NOT NULL,
    unit_price DECIMAL(10,2) NOT NULL
);
```

### 11. Performance Optimization

**Query Optimization Tips:**
```sql
-- Use indexes for WHERE, ORDER BY, and JOIN columns
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date);

-- Use EXPLAIN to analyze query performance
EXPLAIN ANALYZE 
SELECT * FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
WHERE o.order_date >= '2023-01-01';

-- Avoid SELECT * in production
-- Instead of: SELECT * FROM large_table;
SELECT id, name, email FROM users WHERE status = 'active';

-- Use LIMIT for large result sets
SELECT * FROM logs 
ORDER BY created_at DESC 
LIMIT 100;

-- Use EXISTS instead of IN for subqueries with large datasets
-- Instead of:
SELECT * FROM customers WHERE id IN (SELECT customer_id FROM orders);
-- Use:
SELECT * FROM customers c WHERE EXISTS (
    SELECT 1 FROM orders o WHERE o.customer_id = c.id
);
```

---

## Linux Commands & DevOps

### 1. File System Navigation

**Basic Navigation:**
```bash
# Current directory
pwd

# List files and directories
ls                    # Basic listing
ls -la               # Detailed listing with hidden files
ls -lh               # Human-readable file sizes
ls -lt               # Sort by modification time
ls -lS               # Sort by file size

# Change directory
cd /path/to/directory
cd ~                 # Home directory
cd -                 # Previous directory
cd ..                # Parent directory

# Create directories
mkdir directory_name
mkdir -p path/to/nested/directory

# Tree view (install with: sudo apt install tree)
tree .               # Current directory tree
tree -d .            # Only directories
tree -L 2 .          # Limit depth to 2 levels
```

### 2. File Operations

**Creating and Editing Files:**
```bash
# Create empty file or update timestamp
touch filename.txt

# Create file with content
echo "Hello World" > file.txt      # Overwrite
echo "New line" >> file.txt        # Append

# Copy files and directories
cp file1.txt file2.txt             # Copy file
cp -r dir1/ dir2/                  # Copy directory recursively
cp -i file1.txt file2.txt          # Interactive (prompt before overwrite)
cp -v file1.txt file2.txt          # Verbose output

# Move/rename files
mv oldname.txt newname.txt         # Rename file
mv file.txt /path/to/destination/  # Move file
mv -i file.txt destination/        # Interactive mode

# Remove files and directories
rm file.txt                        # Remove file
rm -r directory/                   # Remove directory recursively
rm -rf directory/                  # Force remove without prompts
rm -i file.txt                     # Interactive removal
```

### 3. File Content Operations

**Viewing File Content:**
```bash
# Display entire file
cat filename.txt
cat -n filename.txt              # With line numbers

# Display file page by page
less filename.txt                # Navigate with arrows, q to quit
more filename.txt                # Similar to less

# Display first/last lines
head filename.txt                # First 10 lines
head -n 20 filename.txt          # First 20 lines
tail filename.txt                # Last 10 lines
tail -n 15 filename.txt          # Last 15 lines
tail -f logfile.txt              # Follow file changes (real-time)

# Search in files
grep "pattern" filename.txt      # Search for pattern
grep -n "pattern" file.txt       # Show line numbers
grep -i "pattern" file.txt       # Case insensitive
grep -r "pattern" directory/     # Recursive search
grep -v "pattern" file.txt       # Invert match (exclude)
grep -c "pattern" file.txt       # Count matches

# Word count
wc filename.txt                  # Lines, words, characters
wc -l filename.txt               # Only line count
wc -w filename.txt               # Only word count
```

### 4. Text Processing

**Advanced Text Operations:**
```bash
# Sort and unique
sort filename.txt                # Sort lines
sort -n numbers.txt              # Numeric sort
sort -r filename.txt             # Reverse sort
uniq filename.txt                # Remove duplicate lines
sort filename.txt | uniq         # Sort then remove duplicates

# Cut columns
cut -d',' -f1,3 file.csv         # Extract columns 1 and 3 (comma delimiter)
cut -c1-10 file.txt              # Extract characters 1-10

# Replace text
sed 's/old/new/g' file.txt       # Replace all occurrences
sed 's/old/new/' file.txt        # Replace first occurrence per line
sed -i 's/old/new/g' file.txt    # Replace in-place (modify file)

# AWK for advanced text processing
awk '{print $1}' file.txt        # Print first column
awk -F',' '{print $2}' file.csv  # Print second column (comma delimiter)
awk '{sum+=$1} END {print sum}' numbers.txt  # Sum first column
```

### 5. File Permissions and Ownership

**Understanding Permissions:**
```bash
# View permissions
ls -l filename.txt
# Output: -rw-r--r-- 1 user group 1024 Jan 1 12:00 filename.txt
#         ^^^^ ^^^^ ^^^
#         user group other

# Change permissions
chmod 755 script.sh              # rwxr-xr-x
chmod 644 file.txt               # rw-r--r--
chmod u+x script.sh              # Add execute for user
chmod g-w file.txt               # Remove write for group
chmod o+r file.txt               # Add read for others
chmod a+r file.txt               # Add read for all

# Change ownership
chown user:group file.txt        # Change user and group
chown user file.txt              # Change only user
chgrp group file.txt             # Change only group
chown -R user:group directory/   # Recursive ownership change

# Special permissions
chmod u+s executable             # Set SUID
chmod g+s directory/             # Set SGID
chmod +t directory/              # Set sticky bit
```

### 6. Process Management

**Viewing Processes:**
```bash
# List processes
ps                               # Current terminal processes
ps aux                           # All processes with details
ps aux | grep process_name       # Filter specific process
ps -ef                           # Full format listing
ps -ef --forest                 # Tree view of processes

# Process tree
pstree                           # Hierarchical process view
pstree -p                        # Include process IDs

# Real-time process monitoring
top                              # Interactive process viewer
htop                             # Enhanced version (install: sudo apt install htop)
top -d 1                         # Update every second

# Find process by name
pgrep process_name               # Get PID by name
pgrep -l ssh                     # Get PID and name
pidof process_name               # Alternative to find PID
```

**Managing Processes:**
```bash
# Kill processes
kill PID                         # Terminate process
kill -9 PID                      # Force kill
kill -TERM PID                   # Graceful termination
killall process_name             # Kill all instances
pkill process_name               # Kill by process name

# Background and foreground jobs
command &                        # Run in background
jobs                             # List active jobs
fg %1                            # Bring job 1 to foreground
bg %1                            # Send job 1 to background
Ctrl+Z                           # Suspend current process
Ctrl+C                           # Terminate current process

# Nohup (no hangup)
nohup long_running_command &     # Continue after logout
```

### 7. System Information

**Hardware Information:**
```bash
# CPU information
lscpu                            # CPU details
cat /proc/cpuinfo                # Detailed CPU info

# Memory information
free -h                          # Memory usage (human readable)
cat /proc/meminfo                # Detailed memory info

# Disk information
df -h                            # Disk space usage
du -h directory/                 # Directory size
du -sh *                         # Size of all items in current directory
lsblk                            # Block devices

# Hardware listing
lshw                             # Complete hardware info
lshw -short                      # Concise hardware info
lspci                            # PCI devices
lsusb                            # USB devices

# System information
uname -a                         # System information
uptime                           # System uptime and load
whoami                           # Current user
id                               # User and group IDs
w                                # Who is logged in
last                             # Login history
```

### 8. Network Commands

**Network Configuration:**
```bash
# Network interfaces
ip addr show                     # Show IP addresses
ip link show                     # Show network interfaces
ifconfig                         # Legacy command (may need installation)

# Routing
ip route show                    # Show routing table
route -n                         # Numeric routing table

# Network connectivity
ping google.com                  # Test connectivity
ping -c 4 google.com             # Send 4 packets
traceroute google.com            # Trace route to destination
wget https://example.com/file    # Download file
curl https://api.example.com     # Make HTTP request

# Network monitoring
netstat -tulpn                   # Show listening ports
ss -tulpn                        # Modern alternative to netstat
lsof -i :80                      # Show what's using port 80

# DNS
nslookup google.com              # DNS lookup
dig google.com                   # Detailed DNS lookup
host google.com                  # Simple DNS lookup
```

### 9. File Search and Location

**Finding Files:**
```bash
# Find files by name
find /path -name "filename"      # Exact name
find /path -name "*.txt"         # Pattern matching
find /path -iname "*.TXT"        # Case insensitive

# Find by type
find /path -type f               # Files only
find /path -type d               # Directories only

# Find by size
find /path -size +100M           # Files larger than 100MB
find /path -size -1k             # Files smaller than 1KB

# Find by time
find /path -mtime -7             # Modified in last 7 days
find /path -atime +30            # Accessed more than 30 days ago

# Execute commands on found files
find /path -name "*.log" -exec rm {} \;  # Delete all .log files
find /path -name "*.txt" -exec grep "pattern" {} \;

# Locate (fast file search)
updatedb                         # Update locate database
locate filename                  # Find file quickly
locate -i filename               # Case insensitive
```

### 10. Archive and Compression

**Tar Operations:**
```bash
# Create archives
tar -czf archive.tar.gz directory/    # Create compressed tar
tar -cf archive.tar directory/        # Create uncompressed tar
tar -cjf archive.tar.bz2 directory/   # Create bzip2 compressed

# Extract archives
tar -xzf archive.tar.gz               # Extract gzip compressed
tar -xf archive.tar                   # Extract uncompressed
tar -xjf archive.tar.bz2              # Extract bzip2 compressed

# List archive contents
tar -tzf archive.tar.gz               # List files in gzip archive
tar -tf archive.tar                   # List files in uncompressed

# Zip operations
zip -r archive.zip directory/         # Create zip archive
unzip archive.zip                     # Extract zip archive
unzip -l archive.zip                  # List zip contents
```

### 11. System Services (systemd)

**Service Management:**
```bash
# Service status and control
systemctl status service_name         # Check service status
systemctl start service_name          # Start service
systemctl stop service_name           # Stop service
systemctl restart service_name        # Restart service
systemctl reload service_name         # Reload configuration

# Enable/disable services
systemctl enable service_name         # Enable at boot
systemctl disable service_name        # Disable at boot
systemctl is-enabled service_name     # Check if enabled

# List services
systemctl list-units --type=service   # List all services
systemctl list-units --state=active   # List active services
systemctl list-units --state=failed   # List failed services

# View logs
journalctl                            # System logs
journalctl -u service_name            # Service-specific logs
journalctl -f                         # Follow logs
journalctl --since "1 hour ago"       # Recent logs
```

### 12. SSH and Remote Access

**SSH Configuration:**
```bash
# Basic SSH connection
ssh user@hostname
ssh -p 2222 user@hostname            # Custom port
ssh -v user@hostname                 # Verbose output

# SSH key generation
ssh-keygen -t rsa -b 4096            # Generate RSA key
ssh-keygen -t ed25519                # Generate Ed25519 key
ssh-copy-id user@hostname            # Copy public key to remote

# SSH config file (~/.ssh/config)
Host myserver
    HostName 192.168.1.100
    User myuser
    Port 2222
    IdentityFile ~/.ssh/private_key

# File transfer
scp file.txt user@host:/path/        # Copy file to remote
scp user@host:/path/file.txt .       # Copy file from remote
scp -r directory/ user@host:/path/   # Copy directory

# rsync (advanced file sync)
rsync -avz /local/path/ user@host:/remote/path/
rsync -avz --delete local/ remote/   # Mirror directories
```

### 13. Package Management (Ubuntu/Debian)

**APT Commands:**
```bash
# Update package lists
sudo apt update

# Upgrade packages
sudo apt upgrade                     # Upgrade all packages
sudo apt full-upgrade                # Complete upgrade

# Install packages
sudo apt install package_name
sudo apt install package1 package2  # Multiple packages

# Remove packages
sudo apt remove package_name         # Remove but keep config
sudo apt purge package_name          # Remove completely
sudo apt autoremove                  # Remove unused dependencies

# Search packages
apt search keyword
apt list --installed                 # List installed packages
apt show package_name                # Show package details

# Clean up
sudo apt clean                       # Clear package cache
sudo apt autoclean                   # Clear old package cache
```

### 14. Environment Variables and Shell

**Environment Variables:**
```bash
# View environment variables
env                                  # All environment variables
echo $PATH                           # Specific variable
echo $HOME                           # Home directory
echo $USER                           # Current user

# Set environment variables
export VAR_NAME="value"              # Set for current session
export PATH=$PATH:/new/path          # Add to PATH

# Make permanent (add to ~/.bashrc or ~/.profile)
echo 'export PATH=$PATH:/new/path' >> ~/.bashrc
source ~/.bashrc                     # Reload configuration

# Aliases
alias ll='ls -la'                    # Create alias
alias ..='cd ..'                     # Navigate shortcut
unalias ll                           # Remove alias
alias                                # List all aliases
```

### 15. Cron Jobs and Scheduling

**Crontab Management:**
```bash
# Edit crontab
crontab -e                           # Edit current user's crontab
sudo crontab -e                      # Edit root's crontab

# List crontab
crontab -l                           # List current user's jobs
sudo crontab -l                      # List root's jobs

# Remove crontab
crontab -r                           # Remove all jobs

# Cron syntax: minute hour day month day_of_week command
# Examples:
# 0 2 * * *           /path/to/script.sh      # Daily at 2 AM
# */15 * * * *        /path/to/script.sh      # Every 15 minutes
# 0 0 1 * *           /path/to/script.sh      # First day of month
# 0 9 * * 1-5         /path/to/script.sh      # Weekdays at 9 AM
# @reboot             /path/to/script.sh      # At system startup
```

### 16. Monitoring and Performance

**System Monitoring:**
```bash
# Real-time monitoring
top                                  # Process monitor
htop                                 # Enhanced process monitor
iotop                                # I/O monitor
nethogs                              # Network usage by process

# System statistics
vmstat 1                             # Virtual memory stats
iostat 1                             # I/O statistics
sar 1 10                             # System activity report

# Load average
uptime                               # Current load
w                                    # Users and load

# Disk usage
df -h                                # Disk space
du -sh /path                         # Directory size
ncdu /path                           # Interactive disk usage

# Memory usage
free -h                              # Memory overview
ps aux --sort=-%mem | head           # Top memory consumers
```

---

## System Design & Architecture

### 1. High-Level Design (HLD) Concepts

**Scalability Patterns:**

**Horizontal vs Vertical Scaling:**
- **Vertical Scaling (Scale Up):** Add more power to existing server
- **Horizontal Scaling (Scale Out):** Add more servers

**Load Balancing:**
```
Client → Load Balancer → [Server1, Server2, Server3]
```

**Types of Load Balancers:**
- **Layer 4 (Transport):** Routes based on IP and port
- **Layer 7 (Application):** Routes based on content (HTTP headers, URLs)

**Load Balancing Algorithms:**
- Round Robin
- Weighted Round Robin
- Least Connections
- IP Hash
- Consistent Hashing

### 2. Database Design Patterns

**Database Scaling Strategies:**

**Master-Slave Replication:**
```
Master DB (Writes) → Slave DB1 (Reads)
                  → Slave DB2 (Reads)
                  → Slave DB3 (Reads)
```

**Master-Master Replication:**
```
Master DB1 ↔ Master DB2
```

**Database Sharding:**
```
Shard 1 (Users A-F) → DB1
Shard 2 (Users G-M) → DB2
Shard 3 (Users N-T) → DB3
Shard 4 (Users U-Z) → DB4
```

**Sharding Strategies:**
- **Range-based:** Partition by value ranges
- **Hash-based:** Use hash function on key
- **Directory-based:** Lookup service tracks shards

### 3. Caching Strategies

**Cache Levels:**
```
Browser Cache → CDN → Load Balancer → Application Server → Database Cache → Database
```

**Cache Patterns:**

**Cache-Aside (Lazy Loading):**
```javascript
function getUser(userId) {
    // Try cache first
    let user = cache.get(`user:${userId}`);
    
    if (!user) {
        // Cache miss - fetch from database
        user = database.getUser(userId);
        
        // Store in cache for next time
        cache.set(`user:${userId}`, user, TTL);
    }
    
    return user;
}
```

**Write-Through:**
```javascript
function updateUser(userId, userData) {
    // Write to database first
    database.updateUser(userId, userData);
    
    // Update cache
    cache.set(`user:${userId}`, userData, TTL);
}
```

**Write-Behind (Write-Back):**
```javascript
function updateUser(userId, userData) {
    // Write to cache immediately
    cache.set(`user:${userId}`, userData, TTL);
    
    // Asynchronously write to database
    asyncQueue.enqueue({
        operation: 'updateUser',
        userId,
        userData
    });
}
```

### 4. Message Queues and Event-Driven Architecture

**Message Queue Patterns:**

**Point-to-Point:**
```
Producer → Queue → Consumer
```

**Publish-Subscribe:**
```
Publisher → Topic → [Subscriber1, Subscriber2, Subscriber3]
```

**Message Queue Benefits:**
- Decoupling of components
- Asynchronous processing
- Load leveling
- Reliability and fault tolerance

**Kafka Architecture Example:**
```javascript
// Producer
const producer = kafka.producer();

await producer.send({
    topic: 'user-events',
    messages: [
        {
            key: userId,
            value: JSON.stringify({
                event: 'user-created',
                userId: userId,
                timestamp: Date.now()
            })
        }
    ]
});

// Consumer
const consumer = kafka.consumer({ groupId: 'user-service' });

await consumer.subscribe({ topic: 'user-events' });

await consumer.run({
    eachMessage: async ({ topic, partition, message }) => {
        const event = JSON.parse(message.value.toString());
        await handleUserEvent(event);
    }
});
```

### 5. Microservices Architecture

**Service Communication Patterns:**

**Synchronous Communication:**
- HTTP/REST APIs
- gRPC
- GraphQL

**Asynchronous Communication:**
- Message queues
- Event streams
- Webhooks

**Service Discovery:**
```javascript
// Service Registry Pattern
const serviceRegistry = {
    'user-service': ['http://user1:3001', 'http://user2:3001'],
    'order-service': ['http://order1:3002'],
    'payment-service': ['http://payment1:3003', 'http://payment2:3003']
};

function getServiceUrl(serviceName) {
    const instances = serviceRegistry[serviceName];
    return instances[Math.floor(Math.random() * instances.length)];
}
```

**Circuit Breaker Pattern:**
```javascript
class CircuitBreaker {
    constructor(threshold = 5, timeout = 60000) {
        this.failureThreshold = threshold;
        this.timeout = timeout;
        this.failureCount = 0;
        this.lastFailureTime = null;
        this.state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
    }

    async call(fn) {
        if (this.state === 'OPEN') {
            if (Date.now() - this.lastFailureTime > this.timeout) {
                this.state = 'HALF_OPEN';
            } else {
                throw new Error('Circuit breaker is OPEN');
            }
        }

        try {
            const result = await fn();
            this.onSuccess();
            return result;
        } catch (error) {
            this.onFailure();
            throw error;
        }
    }

    onSuccess() {
        this.failureCount = 0;
        this.state = 'CLOSED';
    }

    onFailure() {
        this.failureCount++;
        this.lastFailureTime = Date.now();
        
        if (this.failureCount >= this.failureThreshold) {
            this.state = 'OPEN';
        }
    }
}
```

### 6. Low-Level Design (LLD) Examples

**URL Shortener Service (like bit.ly):**

```javascript
class UrlShortener {
    constructor() {
        this.urlDatabase = new Map(); // url_id -> original_url
        this.reverseDatabase = new Map(); // original_url -> url_id
        this.base62 = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
        this.counter = 1000000; // Start from a large number
    }

    shortenUrl(originalUrl) {
        // Check if URL already exists
        if (this.reverseDatabase.has(originalUrl)) {
            const urlId = this.reverseDatabase.get(originalUrl);
            return this.encodeBase62(urlId);
        }

        // Generate new short URL
        const urlId = this.counter++;
        const shortCode = this.encodeBase62(urlId);
        
        // Store mappings
        this.urlDatabase.set(urlId, originalUrl);
        this.reverseDatabase.set(originalUrl, urlId);
        
        return shortCode;
    }

    expandUrl(shortCode) {
        const urlId = this.decodeBase62(shortCode);
        return this.urlDatabase.get(urlId);
    }

    encodeBase62(num) {
        let result = '';
        while (num > 0) {
            result = this.base62[num % 62] + result;
            num = Math.floor(num / 62);
        }
        return result;
    }

    decodeBase62(str) {
        let result = 0;
        for (let i = 0; i < str.length; i++) {
            result = result * 62 + this.base62.indexOf(str[i]);
        }
        return result;
    }
}
```

**Rate Limiter (Token Bucket Algorithm):**

```javascript
class TokenBucket {
    constructor(capacity, refillRate) {
        this.capacity = capacity;
        this.tokens = capacity;
        this.refillRate = refillRate; // tokens per second
        this.lastRefillTime = Date.now();
    }

    tryConsume(tokens = 1) {
        this.refill();
        
        if (this.tokens >= tokens) {
            this.tokens -= tokens;
            return true;
        }
        
        return false;
    }

    refill() {
        const now = Date.now();
        const timePassed = (now - this.lastRefillTime) / 1000;
        const tokensToAdd = timePassed * this.refillRate;
        
        this.tokens = Math.min(this.capacity, this.tokens + tokensToAdd);
        this.lastRefillTime = now;
    }
}

class RateLimiter {
    constructor() {
        this.buckets = new Map();
    }

    isAllowed(userId, maxRequests = 100, windowSizeSeconds = 3600) {
        if (!this.buckets.has(userId)) {
            this.buckets.set(userId, new TokenBucket(
                maxRequests,
                maxRequests / windowSizeSeconds
            ));
        }

        const bucket = this.buckets.get(userId);
        return bucket.tryConsume(1);
    }
}
```

### 7. Design Patterns for Backend Systems

**Repository Pattern:**
```javascript
// User Repository Interface
class UserRepository {
    async findById(id) { throw new Error('Not implemented'); }
    async findByEmail(email) { throw new Error('Not implemented'); }
    async save(user) { throw new Error('Not implemented'); }
    async delete(id) { throw new Error('Not implemented'); }
}

// MongoDB Implementation
class MongoUserRepository extends UserRepository {
    constructor(database) {
        super();
        this.collection = database.collection('users');
    }

    async findById(id) {
        return await this.collection.findOne({ _id: id });
    }

    async findByEmail(email) {
        return await this.collection.findOne({ email });
    }

    async save(user) {
        if (user._id) {
            await this.collection.updateOne(
                { _id: user._id },
                { $set: user }
            );
        } else {
            await this.collection.insertOne(user);
        }
        return user;
    }

    async delete(id) {
        await this.collection.deleteOne({ _id: id });
    }
}

// Service Layer
class UserService {
    constructor(userRepository, emailService) {
        this.userRepository = userRepository;
        this.emailService = emailService;
    }

    async createUser(userData) {
        // Validate user data
        if (!userData.email || !userData.password) {
            throw new Error('Email and password are required');
        }

        // Check if user already exists
        const existingUser = await this.userRepository.findByEmail(userData.email);
        if (existingUser) {
            throw new Error('User already exists');
        }

        // Hash password
        const hashedPassword = await bcrypt.hash(userData.password, 10);
        
        // Create user
        const user = {
            email: userData.email,
            password: hashedPassword,
            createdAt: new Date()
        };

        const savedUser = await this.userRepository.save(user);

        // Send welcome email (async)
        this.emailService.sendWelcomeEmail(user.email).catch(console.error);

        return { id: savedUser._id, email: savedUser.email };
    }
}
```

**Observer Pattern (Event System):**
```javascript
class EventEmitter {
    constructor() {
        this.events = {};
    }

    on(event, listener) {
        if (!this.events[event]) {
            this.events[event] = [];
        }
        this.events[event].push(listener);
    }

    emit(event, data) {
        if (this.events[event]) {
            this.events[event].forEach(listener => {
                try {
                    listener(data);
                } catch (error) {
                    console.error(`Error in event listener: ${error.message}`);
                }
            });
        }
    }

    off(event, listenerToRemove) {
        if (this.events[event]) {
            this.events[event] = this.events[event].filter(
                listener => listener !== listenerToRemove
            );
        }
    }
}

// Usage in application
class OrderService extends EventEmitter {
    async createOrder(orderData) {
        const order = await this.saveOrder(orderData);
        
        // Emit event after order creation
        this.emit('order:created', {
            orderId: order.id,
            userId: order.userId,
            total: order.total
        });
        
        return order;
    }
}

// Event listeners
const orderService = new OrderService();

// Inventory service listens for orders
orderService.on('order:created', async (orderData) => {
    await inventoryService.updateInventory(orderData);
});

// Email service listens for orders
orderService.on('order:created', async (orderData) => {
    await emailService.sendOrderConfirmation(orderData);
});

// Analytics service listens for orders
orderService.on('order:created', async (orderData) => {
    await analyticsService.trackOrder(orderData);
});
```

### 8. API Design Best Practices

**RESTful API Design:**
```javascript
// Express.js REST API example
const express = require('express');
const app = express();

// Middleware
app.use(express.json());
app.use(authMiddleware);

// Users API
app.get('/api/v1/users', async (req, res) => {
    const { page = 1, limit = 10, search } = req.query;
    
    try {
        const users = await userService.getUsers({
            page: parseInt(page),
            limit: parseInt(limit),
            search
        });
        
        res.json({
            data: users.data,
            pagination: {
                page: users.page,
                limit: users.limit,
                total: users.total,
                totalPages: Math.ceil(users.total / users.limit)
            }
        });
    } catch (error) {
        res.status(500).json({ error: 'Internal server error' });
    }
});

app.get('/api/v1/users/:id', async (req, res) => {
    try {
        const user = await userService.getUserById(req.params.id);
        
        if (!user) {
            return res.status(404).json({ error: 'User not found' });
        }
        
        res.json({ data: user });
    } catch (error) {
        res.status(500).json({ error: 'Internal server error' });
    }
});

app.post('/api/v1/users', async (req, res) => {
    try {
        const user = await userService.createUser(req.body);
        res.status(201).json({ data: user });
    } catch (error) {
        if (error.message === 'User already exists') {
            return res.status(409).json({ error: error.message });
        }
        res.status(400).json({ error: error.message });
    }
});

app.put('/api/v1/users/:id', async (req, res) => {
    try {
        const user = await userService.updateUser(req.params.id, req.body);
        res.json({ data: user });
    } catch (error) {
        if (error.message === 'User not found') {
            return res.status(404).json({ error: error.message });
        }
        res.status(400).json({ error: error.message });
    }
});

app.delete('/api/v1/users/:id', async (req, res) => {
    try {
        await userService.deleteUser(req.params.id);
        res.status(204).send();
    } catch (error) {
        if (error.message === 'User not found') {
            return res.status(404).json({ error: error.message });
        }
        res.status(500).json({ error: 'Internal server error' });
    }
});
```

### 9. Security Best Practices

**Authentication and Authorization:**
```javascript
// JWT middleware
const jwt = require('jsonwebtoken');

function authenticateToken(req, res, next) {
    const authHeader = req.headers['authorization'];
    const token = authHeader && authHeader.split(' ')[1];

    if (!token) {
        return res.status(401).json({ error: 'Access token required' });
    }

    jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
        if (err) {
            return res.status(403).json({ error: 'Invalid token' });
        }
        req.user = user;
        next();
    });
}

// Role-based authorization
function authorize(roles) {
    return (req, res, next) => {
        if (!req.user) {
            return res.status(401).json({ error: 'Unauthorized' });
        }

        if (!roles.includes(req.user.role)) {
            return res.status(403).json({ error: 'Insufficient permissions' });
        }

        next();
    };
}

// Usage
app.get('/api/admin/users', 
    authenticateToken, 
    authorize(['admin']), 
    getUsersHandler
);

// Input validation
const { body, validationResult } = require('express-validator');

app.post('/api/users',
    [
        body('email').isEmail().normalizeEmail(),
        body('password').isLength({ min: 8 }).matches(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/),
        body('name').trim().isLength({ min: 2, max: 50 })
    ],
    (req, res, next) => {
        const errors = validationResult(req);
        if (!errors.isEmpty()) {
            return res.status(400).json({ errors: errors.array() });
        }
        next();
    },
    createUserHandler
);
```

**Rate Limiting and Security Headers:**
```javascript
const rateLimit = require('express-rate-limit');
const helmet = require('helmet');

// Rate limiting
const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // limit each IP to 100 requests per windowMs
    message: 'Too many requests, please try again later'
});

app.use(limiter);

// Security headers
app.use(helmet());

// CORS configuration
const cors = require('cors');
app.use(cors({
    origin: process.env.ALLOWED_ORIGINS?.split(',') || 'http://localhost:3000',
    credentials: true
}));
```

### 10. Monitoring and Observability

**Application Monitoring:**
```javascript
// Prometheus metrics
const promClient = require('prom-client');

// Create metrics
const httpRequestsTotal = new promClient.Counter({
    name: 'http_requests_total',
    help: 'Total number of HTTP requests',
    labelNames: ['method', 'route', 'status_code']
});

const httpRequestDuration = new promClient.Histogram({
    name: 'http_request_duration_seconds',
    help: 'Duration of HTTP requests in seconds',
    labelNames: ['method', 'route'],
    buckets: [0.1, 0.3, 0.5, 0.7, 1, 3, 5, 7, 10]
});

// Middleware to collect metrics
function metricsMiddleware(req, res, next) {
    const start = Date.now();
    
    res.on('finish', () => {
        const duration = (Date.now() - start) / 1000;
        
        httpRequestsTotal
            .labels(req.method, req.route?.path || req.path, res.statusCode)
            .inc();
            
        httpRequestDuration
            .labels(req.method, req.route?.path || req.path)
            .observe(duration);
    });
    
    next();
}

app.use(metricsMiddleware);

// Metrics endpoint
app.get('/metrics', async (req, res) => {
    res.set('Content-Type', promClient.register.contentType);
    res.end(await promClient.register.metrics());
});
```

**Health Checks:**
```javascript
// Health check endpoint
app.get('/health', async (req, res) => {
    const healthcheck = {
        uptime: process.uptime(),
        message: 'OK',
        timestamp: Date.now(),
        checks: {}
    };

    try {
        // Database health check
        await database.ping();
        healthcheck.checks.database = 'OK';
        
        // Redis health check
        await redis.ping();
        healthcheck.checks.redis = 'OK';
        
        // External service health check
        const response = await fetch('https://api.external-service.com/health');
        healthcheck.checks.externalService = response.ok ? 'OK' : 'FAIL';
        
        res.status(200).json(healthcheck);
    } catch (error) {
        healthcheck.message = error.message;
        res.status(503).json(healthcheck);
    }
});
```

---

*This interview preparation sheet covers the essential topics for backend development, DevOps, and full-stack engineering roles. Practice implementing these concepts and be prepared to discuss real-world applications and trade-offs during interviews.*