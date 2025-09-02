# Complete Interview Preparation Sheet with Theory
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

**Theory & Explanation:**
Node.js represents a paradigm shift in how we think about JavaScript execution. Traditionally, JavaScript was confined to the browser environment, but Node.js liberated it by providing a server-side runtime built on Chrome's V8 JavaScript engine.

**Core Architectural Principles:**
- **Single-threaded Event Loop:** Unlike traditional multi-threaded servers that create a new thread for each request (consuming ~2MB of memory per thread), Node.js uses a single thread with an event loop that can handle thousands of concurrent connections.
- **Non-blocking I/O:** When a request involves I/O operations (database queries, file reads, network calls), Node.js doesn't wait. Instead, it delegates these operations to the system kernel and continues processing other requests.
- **Event-driven Architecture:** Everything in Node.js is built around events and callbacks, making it naturally asynchronous.

**Why This Matters:**
- **Resource Efficiency:** One Node.js process can handle thousands of concurrent connections with minimal memory overhead.
- **Developer Productivity:** Full-stack JavaScript means developers can work across the entire application stack without context switching between languages.
- **Real-time Applications:** The event-driven nature makes Node.js ideal for real-time applications like chat systems, gaming servers, and collaborative tools.

**When to Use Node.js:**
- I/O intensive applications (APIs, microservices)
- Real-time applications (WebSocket servers)
- RESTful services and APIs
- Server-side rendering (SSR)
- Build tools and CLI applications

**When NOT to Use Node.js:**
- CPU-intensive applications (image/video processing, complex calculations)
- Applications requiring heavy computational tasks

**Example:**
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    // This handler can process thousands of concurrent requests
    // because it doesn't block on I/O operations
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello, World!');
});

server.listen(3000, () => {
    console.log('Server is running on http://localhost:3000');
});
```

### 2. Explain the Event Loop in Node.js

**Theory & Explanation:**
The Event Loop is Node.js's secret weapon for achieving high concurrency on a single thread. It's based on the **reactor pattern** and implements a **non-blocking I/O model**.

**Event Loop Phases (in order):**

1. **Timers Phase:** Executes callbacks scheduled by `setTimeout()` and `setInterval()`
2. **Pending Callbacks Phase:** Executes I/O callbacks deferred to the next loop iteration
3. **Idle, Prepare Phase:** Internal use only
4. **Poll Phase:** Fetches new I/O events and executes I/O-related callbacks
5. **Check Phase:** Executes `setImmediate()` callbacks
6. **Close Callbacks Phase:** Executes close event callbacks (e.g., `socket.on('close', ...)`)

**Key Concepts:**
- **Call Stack:** Where synchronous code executes (LIFO - Last In, First Out)
- **Task Queue (Macrotask Queue):** Contains callbacks from `setTimeout`, `setInterval`, I/O operations
- **Microtask Queue:** Contains callbacks from `Promise.then()`, `process.nextTick()` (higher priority)
- **Thread Pool:** Handles file system operations, DNS lookups, CPU-intensive crypto operations

**Priority Order (highest to lowest):**
1. `process.nextTick()`
2. Promise microtasks
3. `setImmediate()`
4. `setTimeout()` / `setInterval()`

**How to Explain This:**
"Imagine the event loop as a restaurant manager. The manager (event loop) doesn't cook food or serve tables directly. Instead, they coordinate between the kitchen (thread pool) and waiters (callback functions). When a customer (request) orders food (I/O operation), the manager doesn't wait for the food to be cooked. They take the order, send it to the kitchen, and immediately attend to the next customer. When the food is ready, the kitchen notifies the manager, who then dispatches a waiter to serve it."

**Example:**
```javascript
console.log("1 - Start"); // Synchronous - executes immediately

setTimeout(() => {
    console.log("2 - setTimeout"); // Macrotask - goes to timer phase
}, 0);

process.nextTick(() => {
    console.log("3 - nextTick"); // Microtask - highest priority
});

Promise.resolve().then(() => {
    console.log("4 - Promise"); // Microtask - after nextTick
});

setImmediate(() => {
    console.log("5 - setImmediate"); // Macrotask - check phase
});

console.log("6 - End"); // Synchronous - executes immediately

// Output: 1, 6, 3, 4, 5, 2
```

### 3. What is npm and package.json?

**Theory & Explanation:**
npm (Node Package Manager) is more than just a package manager—it's the backbone of the Node.js ecosystem and the world's largest software registry.

**Core Concepts:**

**Dependency Management Philosophy:**
- **Semantic Versioning (SemVer):** Uses MAJOR.MINOR.PATCH versioning
  - MAJOR: Breaking changes
  - MINOR: New features (backward compatible)
  - PATCH: Bug fixes
- **Dependency Resolution:** npm uses a complex algorithm to resolve dependency conflicts
- **Lock Files:** `package-lock.json` ensures reproducible builds across environments

**package.json Structure & Theory:**
```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "Application description",
  "main": "index.js", // Entry point for require()
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "test": "jest",
    "build": "webpack"
  },
  "dependencies": {
    "express": "^4.18.0", // ^ allows minor updates
    "mongoose": "~6.0.0"  // ~ allows patch updates
  },
  "devDependencies": {
    "nodemon": "^2.0.0",
    "jest": "^27.0.0"
  },
  "peerDependencies": {
    "react": ">=16.0.0"
  },
  "engines": {
    "node": ">=14.0.0"
  }
}
```

**Dependency Types:**
- **dependencies:** Required for production
- **devDependencies:** Only needed for development/testing
- **peerDependencies:** Should be installed by the consuming application
- **optionalDependencies:** Won't break installation if they fail

**How to Explain:**
"Think of package.json as a recipe card and npm as your grocery store. The recipe (package.json) lists all ingredients (dependencies) you need and their versions. npm is like having a personal shopper that reads your recipe, goes to the store, and brings back exactly what you need. The lock file (package-lock.json) is like a detailed receipt that ensures if someone else follows your recipe, they get the exact same ingredients from the exact same suppliers."

### 4. How to create an HTTP server in Node.js?

**Theory & Explanation:**
Creating an HTTP server in Node.js demonstrates the core principles of Node.js architecture: event-driven programming and non-blocking I/O.

**HTTP Server Theory:**
- **HTTP Protocol:** Stateless request-response protocol
- **Server Object:** An EventEmitter that listens for 'request' events
- **Request Object:** Readable stream containing client data
- **Response Object:** Writable stream for sending data back to client

**Key Concepts:**
- **Event-driven:** Server listens for events (connections, requests)
- **Streams:** Request and response are streams, allowing efficient memory usage
- **Non-blocking:** Server can handle multiple requests concurrently

**Basic HTTP Server:**
```javascript
const http = require('http');

// Create server - returns an EventEmitter
const server = http.createServer((req, res) => {
    // This callback executes for every HTTP request
    
    // Set response headers
    res.writeHead(200, { 
        'Content-Type': 'text/plain',
        'Access-Control-Allow-Origin': '*' 
    });
    
    // Send response body
    res.end('Hello, World!');
});

// Start listening on port 3000
server.listen(3000, () => {
    console.log('Server running on http://localhost:3000');
});

// Error handling
server.on('error', (err) => {
    console.error('Server error:', err);
});
```

**Advanced HTTP Server with Routing:**
```javascript
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
    const parsedUrl = url.parse(req.url, true);
    const path = parsedUrl.pathname;
    const method = req.method;

    // Basic routing
    if (method === 'GET' && path === '/') {
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end('<h1>Home Page</h1>');
    } else if (method === 'GET' && path === '/api/users') {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ users: ['Alice', 'Bob'] }));
    } else if (method === 'POST' && path === '/api/users') {
        // Handle POST request with body parsing
        let body = '';
        
        req.on('data', chunk => {
            body += chunk.toString();
        });
        
        req.on('end', () => {
            try {
                const userData = JSON.parse(body);
                res.writeHead(201, { 'Content-Type': 'application/json' });
                res.end(JSON.stringify({ message: 'User created', user: userData }));
            } catch (error) {
                res.writeHead(400, { 'Content-Type': 'application/json' });
                res.end(JSON.stringify({ error: 'Invalid JSON' }));
            }
        });
    } else {
        res.writeHead(404, { 'Content-Type': 'text/plain' });
        res.end('Not Found');
    }
});

server.listen(3000);
```

**How to Explain:**
"Creating an HTTP server in Node.js is like setting up a restaurant. The `createServer()` method is like hiring a host who greets every customer (HTTP request). The host doesn't cook or serve food directly—they just take orders and coordinate with the kitchen. Each request is handled asynchronously, so the host can greet multiple customers simultaneously without making anyone wait in line."

### 5. Difference between require() and import?

**Theory & Explanation:**
The difference between `require()` and `import` represents the evolution of JavaScript module systems and highlights Node.js's transition from CommonJS to ES modules.

**CommonJS (require()) Theory:**
- **Synchronous Loading:** Modules are loaded synchronously at runtime
- **Dynamic Resolution:** Module paths can be computed at runtime
- **Caching:** Modules are cached after first load
- **Copy Exports:** Returns a copy of the exported object
- **Runtime Evaluation:** Code is evaluated when require() is called

**ES Modules (import) Theory:**
- **Static Analysis:** Imports/exports are determined at compile time
- **Asynchronous Loading:** Supports both sync and async loading
- **Live Bindings:** Imports are live references to exported values
- **Tree Shaking:** Unused exports can be eliminated by bundlers
- **Strict Mode:** Always runs in strict mode

**Detailed Comparison:**

**CommonJS Example:**
```javascript
// math.js
function add(a, b) {
    return a + b;
}

function multiply(a, b) {
    return a * b;
}

// Export object (can be modified at runtime)
module.exports = {
    add,
    multiply,
    PI: 3.14159
};

// Alternative exports
exports.subtract = (a, b) => a - b;

// app.js
const math = require('./math'); // Loads entire module
const { add } = require('./math'); // Destructuring

// Dynamic require (possible with CommonJS)
const moduleName = './math';
const dynamicMath = require(moduleName);

// Conditional require
if (condition) {
    const conditionalModule = require('./optional-module');
}
```

**ES Modules Example:**
```javascript
// math.js
export function add(a, b) {
    return a + b;
}

export function multiply(a, b) {
    return a * b;
}

export const PI = 3.14159;

// Default export
export default class Calculator {
    add(a, b) { return a + b; }
}

// app.js
import Calculator, { add, multiply, PI } from './math.js';
import * as math from './math.js'; // Import everything

// Dynamic import (ES2020)
async function loadModule() {
    const { add } = await import('./math.js');
    return add(2, 3);
}

// Conditional import
if (condition) {
    const module = await import('./optional-module.js');
}
```

**Live Bindings Demonstration:**
```javascript
// ES Modules - Live Bindings
// counter.js
export let count = 0;
export function increment() {
    count++;
}

// app.js
import { count, increment } from './counter.js';
console.log(count); // 0
increment();
console.log(count); // 1 (live binding - sees the change)

// CommonJS - Copy
// counter.js
let count = 0;
function increment() {
    count++;
}
module.exports = { count, increment };

// app.js
const { count, increment } = require('./counter');
console.log(count); // 0
increment();
console.log(count); // 0 (still 0 - copy of original value)
```

**How to Explain:**
"Think of require() like going to a library and photocopying a book. You get your own copy that doesn't change even if the original book is updated. ES modules with import are like having a live video call with the author—you see updates in real-time. Additionally, require() is like shopping without a list (you can decide what to buy when you're there), while import is like online shopping with a pre-planned cart (you decide what to import before the code runs)."

### 6. Explain the fs module and sync vs async operations

**Theory & Explanation:**
The File System (fs) module embodies Node.js's core philosophy of non-blocking I/O and provides both synchronous and asynchronous APIs for file operations.

**I/O Operation Theory:**
- **Blocking I/O (Synchronous):** Thread waits for operation to complete
- **Non-blocking I/O (Asynchronous):** Thread continues while operation happens in background
- **Event-driven Callbacks:** Operations complete with callbacks or promises
- **Thread Pool:** libuv manages a thread pool for file system operations

**Performance Implications:**
- **Synchronous:** Blocks the event loop, reducing concurrency
- **Asynchronous:** Allows handling multiple operations concurrently
- **Memory Usage:** Async operations are more memory-efficient for large files
- **Error Handling:** Different patterns for sync vs async errors

**Asynchronous File Operations:**
```javascript
const fs = require('fs');
const fsPromises = require('fs').promises;

// Callback-based (traditional async)
fs.readFile('large-file.txt', 'utf8', (err, data) => {
    if (err) {
        console.error('Error reading file:', err.message);
        return;
    }
    console.log('File contents:', data.length, 'characters');
});

// Promise-based (modern async)
fsPromises.readFile('large-file.txt', 'utf8')
    .then(data => {
        console.log('File contents:', data.length, 'characters');
    })
    .catch(err => {
        console.error('Error reading file:', err.message);
    });

// Async/await (most modern)
async function readFileAsync() {
    try {
        const data = await fsPromises.readFile('large-file.txt', 'utf8');
        console.log('File contents:', data.length, 'characters');
    } catch (err) {
        console.error('Error reading file:', err.message);
    }
}

// Writing files asynchronously
async function writeFileAsync() {
    try {
        await fsPromises.writeFile('output.txt', 'Hello, World!', 'utf8');
        console.log('File written successfully');
    } catch (err) {
        console.error('Error writing file:', err.message);
    }
}
```

**Synchronous File Operations:**
```javascript
const fs = require('fs');

try {
    // Blocks the event loop until file is read
    const data = fs.readFileSync('config.json', 'utf8');
    const config = JSON.parse(data);
    console.log('Configuration loaded:', config);
} catch (err) {
    console.error('Error reading config:', err.message);
    process.exit(1); // Exit if critical configuration fails
}

// Writing files synchronously
try {
    fs.writeFileSync('output.txt', 'Hello, World!', 'utf8');
    console.log('File written successfully');
} catch (err) {
    console.error('Error writing file:', err.message);
}
```

**When to Use Each Approach:**

**Use Synchronous (blocking):**
- Application initialization/startup
- Reading configuration files at startup
- CLI tools and build scripts
- When you need the result immediately for next operation

**Use Asynchronous (non-blocking):**
- Web server request handlers
- Any operation during normal application runtime
- When handling multiple concurrent operations
- Large file operations

**Stream-based File Operations (for large files):**
```javascript
const fs = require('fs');
const { pipeline } = require('stream');

// Reading large files efficiently
async function processLargeFile() {
    const readStream = fs.createReadStream('very-large-file.txt', { encoding: 'utf8' });
    const writeStream = fs.createWriteStream('processed-file.txt');

    readStream.on('data', chunk => {
        // Process chunk by chunk (memory efficient)
        const processedChunk = chunk.toUpperCase();
        writeStream.write(processedChunk);
    });

    readStream.on('end', () => {
        writeStream.end();
        console.log('File processing completed');
    });

    readStream.on('error', err => {
        console.error('Read error:', err);
    });
}
```

**How to Explain:**
"Think of file operations like ordering food. Synchronous operations are like going to a fast-food counter—you wait in line, place your order, and stand there until you get your food. Nothing else can happen until you're done. Asynchronous operations are like ordering from a restaurant—you place your order, get a number, and can talk to friends or check your phone while the kitchen prepares your meal. When it's ready, they call your number. The async approach lets the 'restaurant' (your application) serve many customers simultaneously."

### 7. What are Modules in Node.js?

**Theory & Explanation:**
Modules are Node.js's solution to the problem of code organization and namespace management. They implement the **separation of concerns** principle and enable **code reusability**.

**Module System Theory:**
- **Encapsulation:** Each module has its own scope
- **Dependency Management:** Modules can require other modules
- **Single Responsibility:** Each module should have one clear purpose
- **Loose Coupling:** Modules should depend on abstractions, not concretions

**Module Types in Detail:**

**1. Core Modules (Built-in):**
These are compiled into the Node.js binary and always available.

```javascript
// File system operations
const fs = require('fs');
const path = require('path');

// HTTP functionality
const http = require('http');
const https = require('https');

// Utilities
const util = require('util');
const crypto = require('crypto');

// Process and OS information
const os = require('os');
const process = require('process');

// Streams and events
const stream = require('stream');
const events = require('events');
```

**2. Local Modules (Your Code):**
These follow the **Single Responsibility Principle**.

```javascript
// userService.js - Business logic layer
class UserService {
    constructor(userRepository, emailService) {
        this.userRepository = userRepository;
        this.emailService = emailService;
    }

    async createUser(userData) {
        // Validation
        if (!userData.email || !userData.password) {
            throw new Error('Email and password are required');
        }

        // Business logic
        const existingUser = await this.userRepository.findByEmail(userData.email);
        if (existingUser) {
            throw new Error('User already exists');
        }

        // Create user
        const hashedPassword = await this.hashPassword(userData.password);
        const user = await this.userRepository.create({
            ...userData,
            password: hashedPassword
        });

        // Side effects
        await this.emailService.sendWelcomeEmail(user.email);

        return user;
    }

    async hashPassword(password) {
        const bcrypt = require('bcrypt');
        return bcrypt.hash(password, 10);
    }
}

module.exports = UserService;

// userRepository.js - Data access layer
class UserRepository {
    constructor(database) {
        this.db = database;
    }

    async findByEmail(email) {
        return this.db.collection('users').findOne({ email });
    }

    async create(userData) {
        const result = await this.db.collection('users').insertOne(userData);
        return { ...userData, _id: result.insertedId };
    }

    async findById(id) {
        return this.db.collection('users').findOne({ _id: id });
    }
}

module.exports = UserRepository;

// app.js - Composition root
const UserService = require('./userService');
const UserRepository = require('./userRepository');
const EmailService = require('./emailService');
const Database = require('./database');

// Dependency injection
const database = new Database(process.env.MONGODB_URL);
const userRepository = new UserRepository(database);
const emailService = new EmailService();
const userService = new UserService(userRepository, emailService);

module.exports = { userService, userRepository };
```

**3. Third-party Modules (npm packages):**
```javascript
// Express.js for web framework
const express = require('express');
const app = express();

// Mongoose for MongoDB
const mongoose = require('mongoose');

// Lodash for utilities
const _ = require('lodash');

// Moment for date handling
const moment = require('moment');

// Joi for validation
const Joi = require('joi');
```

**Module Loading Mechanism:**
```javascript
// Module resolution order:
// 1. Core modules (fs, http, etc.)
// 2. Relative paths (./file.js, ../folder/file.js)
// 3. node_modules (searches up the directory tree)

// Examples:
require('fs');           // Core module
require('./utils');      // Local file (./utils.js or ./utils/index.js)
require('../config');    // Parent directory
require('express');      // node_modules/express
require('lodash/get');   // Specific function from lodash
```

**Module Caching:**
```javascript
// counter.js
let count = 0;

function increment() {
    count++;
    console.log(`Count is now: ${count}`);
}

module.exports = { increment, getCount: () => count };

// app.js
const counter1 = require('./counter');
const counter2 = require('./counter'); // Same instance (cached)

counter1.increment(); // Count is now: 1
counter2.increment(); // Count is now: 2 (same instance)

console.log(counter1 === counter2); // true

// To get fresh instance, delete from cache
delete require.cache[require.resolve('./counter')];
const counter3 = require('./counter'); // Fresh instance
```

**How to Explain:**
"Think of modules like LEGO blocks. Each block (module) has a specific shape and purpose. You can't see inside a block (encapsulation), but you know how to connect it to other blocks through specific connection points (exports/imports). Core modules are like the basic blocks that come with every LEGO set. Local modules are the custom blocks you create for your specific project. Third-party modules are special blocks other people created and shared. The module system is like the instruction manual that tells you how all these blocks fit together to build something amazing."

### 8. What are Streams in Node.js?

**Theory & Explanation:**
Streams represent one of Node.js's most powerful abstractions for handling data. They implement the **Iterator pattern** and enable **memory-efficient processing** of large datasets.

**Stream Theory:**
- **Chunked Processing:** Data is processed piece by piece, not all at once
- **Memory Efficiency:** Only small chunks are kept in memory
- **Composability:** Streams can be piped together like Unix pipes
- **Backpressure:** Automatic handling of fast producers and slow consumers
- **Event-driven:** Based on EventEmitter pattern

**Stream Types:**

**1. Readable Streams:**
```javascript
const fs = require('fs');
const { Readable } = require('stream');

// File-based readable stream
const fileStream = fs.createReadStream('large-file.txt', {
    encoding: 'utf8',
    highWaterMark: 16 * 1024 // 16KB chunks
});

fileStream.on('data', chunk => {
    console.log(`Received ${chunk.length} bytes`);
    // Process chunk without loading entire file into memory
});

fileStream.on('end', () => {
    console.log('File reading completed');
});

fileStream.on('error', err => {
    console.error('Error:', err);
});

// Custom readable stream
class NumberStream extends Readable {
    constructor(options = {}) {
        super({ ...options, objectMode: true });
        this.current = 0;
        this.max = options.max || 100;
    }

    _read() {
        if (this.current < this.max) {
            this.push({ number: this.current, square: this.current ** 2 });
            this.current++;
        } else {
            this.push(null); // End the stream
        }
    }
}

const numbers = new NumberStream({ max: 10 });
numbers.on('data', data => {
    console.log(`Number: ${data.number}, Square: ${data.square}`);
});
```

**2. Writable Streams:**
```javascript
const fs = require('fs');
const { Writable } = require('stream');

// File-based writable stream
const outputStream = fs.createWriteStream('output.txt');

outputStream.write('Hello, ');
outputStream.write('World!');
outputStream.end(); // Close the stream

// Custom writable stream
class LogStream extends Writable {
    constructor(options = {}) {
        super({ ...options, objectMode: true });
    }

    _write(chunk, encoding, callback) {
        // Process the chunk
        const timestamp = new Date().toISOString();
        console.log(`[${timestamp}] ${chunk.toString()}`);
        
        // Call callback when done processing
        callback();
    }
}

const logger = new LogStream();
logger.write('Application started');
logger.write('User logged in');
```

**3. Transform Streams:**
```javascript
const { Transform } = require('stream');

// Custom transform stream
class UpperCaseTransform extends Transform {
    constructor(options = {}) {
        super(options);
    }

    _transform(chunk, encoding, callback) {
        // Transform the data
        const transformed = chunk.toString().toUpperCase();
        this.push(transformed);
        callback();
    }
}

// Usage with piping
const fs = require('fs');

fs.createReadStream('input.txt')
  .pipe(new UpperCaseTransform())
  .pipe(fs.createWriteStream('output.txt'));

// CSV processing transform
class CSVParser extends Transform {
    constructor(options = {}) {
        super({ ...options, objectMode: true });
        this.headers = null;
        this.buffer = '';
    }

    _transform(chunk, encoding, callback) {
        this.buffer += chunk.toString();
        const lines = this.buffer.split('\n');
        this.buffer = lines.pop(); // Keep incomplete line in buffer

        for (const line of lines) {
            if (!this.headers) {
                this.headers = line.split(',');
            } else {
                const values = line.split(',');
                const obj = {};
                this.headers.forEach((header, index) => {
                    obj[header.trim()] = values[index]?.trim();
                });
                this.push(obj);
            }
        }
        callback();
    }

    _flush(callback) {
        // Process any remaining data in buffer
        if (this.buffer && this.headers) {
            const values = this.buffer.split(',');
            const obj = {};
            this.headers.forEach((header, index) => {
                obj[header.trim()] = values[index]?.trim();
            });
            this.push(obj);
        }
        callback();
    }
}
```

**4. Duplex Streams:**
```javascript
const { Duplex } = require('stream');

class EchoServer extends Duplex {
    constructor(options = {}) {
        super(options);
        this.buffer = [];
    }

    _read() {
        if (this.buffer.length > 0) {
            this.push(this.buffer.shift());
        }
    }

    _write(chunk, encoding, callback) {
        // Echo back what was written
        this.buffer.push(`Echo: ${chunk.toString()}`);
        callback();
    }
}
```

**Stream Piping and Composition:**
```javascript
const fs = require('fs');
const zlib = require('zlib');
const crypto = require('crypto');

// Complex pipeline: Read file -> Compress -> Encrypt -> Write
fs.createReadStream('large-file.txt')
  .pipe(zlib.createGzip()) // Compress
  .pipe(crypto.createCipher('aes192', 'secret')) // Encrypt
  .pipe(fs.createWriteStream('output.gz.enc')); // Write

// With error handling
const { pipeline } = require('stream');

pipeline(
    fs.createReadStream('input.txt'),
    new UpperCaseTransform(),
    zlib.createGzip(),
    fs.createWriteStream('output.txt.gz'),
    (err) => {
        if (err) {
            console.error('Pipeline failed:', err);
        } else {
            console.log('Pipeline succeeded');
        }
    }
);
```

**Backpressure Handling:**
```javascript
const fs = require('fs');

// Manual backpressure handling
const readableStream = fs.createReadStream('huge-file.txt');
const writableStream = fs.createWriteStream('processed-file.txt');

readableStream.on('data', chunk => {
    const canWriteMore = writableStream.write(chunk.toString().toUpperCase());
    
    if (!canWriteMore) {
        // Pause reading until drain event
        readableStream.pause();
        
        writableStream.once('drain', () => {
            readableStream.resume();
        });
    }
});

readableStream.on('end', () => {
    writableStream.end();
});
```

**How to Explain:**
"Think of streams like a water pipeline system in your house. You don't need to store all the water in a giant tank before using it—it flows through pipes chunk by chunk. Readable streams are like faucets (sources of data), writable streams are like drains (destinations), and transform streams are like water filters that modify the water as it flows through. When you connect pipes together (piping), water flows automatically from source to destination. If one part of the system gets overwhelmed (backpressure), the system automatically slows down the flow to prevent overflow."

### 9. Explain Middleware in Express.js

**Theory & Explanation:**
Middleware embodies the **Chain of Responsibility** design pattern and implements **Aspect-Oriented Programming** concepts in Express.js applications.

**Middleware Theory:**
- **Chain of Responsibility:** Each middleware can handle the request or pass it to the next
- **Cross-cutting Concerns:** Address functionality that spans multiple parts of an application
- **Request-Response Cycle:** Middleware sits between raw HTTP request and final response
- **Composable:** Middleware functions can be composed to build complex behavior

**Middleware Execution Flow:**
```
Request → Middleware 1 → Middleware 2 → Route Handler → Middleware 2 (response) → Middleware 1 (response) → Response
```

**Types of Middleware:**

**1. Application-level Middleware:**
```javascript
const express = require('express');
const app = express();

// Logger middleware - runs for every request
app.use((req, res, next) => {
    const timestamp = new Date().toISOString();
    const method = req.method;
    const url = req.url;
    const userAgent = req.get('User-Agent');
    
    console.log(`[${timestamp}] ${method} ${url} - ${userAgent}`);
    
    // Track response time
    req.startTime = Date.now();
    
    // Override res.end to log response time
    const originalEnd = res.end;
    res.end = function(...args) {
        const responseTime = Date.now() - req.startTime;
        console.log(`Response time: ${responseTime}ms`);
        originalEnd.apply(this, args);
    };
    
    next(); // Pass control to next middleware
});

// Authentication middleware
app.use((req, res, next) => {
    const token = req.headers['authorization'];
    
    if (!token) {
        return res.status(401).json({ error: 'No token provided' });
    }
    
    try {
        const jwt = require('jsonwebtoken');
        const decoded = jwt.verify(token.replace('Bearer ', ''), process.env.JWT_SECRET);
        req.user = decoded; // Attach user to request
        next();
    } catch (error) {
        res.status(403).json({ error: 'Invalid token' });
    }
});
```

**2. Router-level Middleware:**
```javascript
const express = require('express');
const router = express.Router();

// Middleware specific to this router
router.use((req, res, next) => {
    console.log('Router-specific middleware');
    req.routerTime = Date.now();
    next();
});

// Route-specific middleware
router.get('/users/:id', 
    // Validation middleware
    (req, res, next) => {
        const { id } = req.params;
        
        if (!id || isNaN(id)) {
            return res.status(400).json({ error: 'Invalid user ID' });
        }
        
        req.userId = parseInt(id);
        next();
    },
    // Permission middleware
    (req, res, next) => {
        if (req.user.role !== 'admin' && req.user.id !== req.userId) {
            return res.status(403).json({ error: 'Access denied' });
        }
        next();
    },
    // Route handler
    async (req, res) => {
        try {
            const user = await userService.getUserById(req.userId);
            res.json({ data: user });
        } catch (error) {
            res.status(500).json({ error: 'Internal server error' });
        }
    }
);

app.use('/api', router);
```

**3. Error-handling Middleware:**
```javascript
// Error-handling middleware (must have 4 parameters)
app.use((err, req, res, next) => {
    console.error('Error occurred:', err);
    
    // Log error with context
    console.error({
        error: err.message,
        stack: err.stack,
        url: req.url,
        method: req.method,
        headers: req.headers,
        timestamp: new Date().toISOString()
    });
    
    // Different responses based on error type
    if (err.name === 'ValidationError') {
        return res.status(400).json({
            error: 'Validation failed',
            details: err.details
        });
    }
    
    if (err.name === 'UnauthorizedError') {
        return res.status(401).json({
            error: 'Authentication required'
        });
    }
    
    // Default error response
    res.status(500).json({
        error: process.env.NODE_ENV === 'production' 
            ? 'Something went wrong' 
            : err.message
    });
});

// 404 handler (should be last)
app.use((req, res) => {
    res.status(404).json({
        error: 'Route not found',
        path: req.path,
        method: req.method
    });
});
```

**4. Built-in and Third-party Middleware:**
```javascript
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');
const compression = require('compression');

const app = express();

// Security middleware
app.use(helmet({
    contentSecurityPolicy: {
        directives: {
            defaultSrc: ["'self'"],
            styleSrc: ["'self'", "'unsafe-inline'"],
            scriptSrc: ["'self'"]
        }
    }
}));

// CORS middleware
app.use(cors({
    origin: process.env.ALLOWED_ORIGINS?.split(',') || 'http://localhost:3000',
    credentials: true,
    optionsSuccessStatus: 200
}));

// Rate limiting
const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // limit each IP to 100 requests per windowMs
    message: 'Too many requests from this IP',
    standardHeaders: true,
    legacyHeaders: false
});
app.use('/api/', limiter);

// Compression
app.use(compression({
    filter: (req, res) => {
        if (req.headers['x-no-compression']) {
            return false;
        }
        return compression.filter(req, res);
    }
}));

// Body parsing
app.use(express.json({ 
    limit: '10mb',
    verify: (req, res, buf) => {
        req.rawBody = buf;
    }
}));
app.use(express.urlencoded({ extended: true, limit: '10mb' }));

// Static files
app.use('/public', express.static('public', {
    maxAge: '1d',
    etag: true
}));
```

**Custom Middleware Examples:**

**Request ID Middleware:**
```javascript
const { v4: uuidv4 } = require('uuid');

function requestId(req, res, next) {
    req.id = uuidv4();
    res.set('X-Request-ID', req.id);
    next();
}

app.use(requestId);
```

**Cache Middleware:**
```javascript
const cache = new Map();

function cacheMiddleware(duration = 300) { // 5 minutes default
    return (req, res, next) => {
        if (req.method !== 'GET') {
            return next();
        }
        
        const key = req.originalUrl;
        const cached = cache.get(key);
        
        if (cached && Date.now() - cached.timestamp < duration * 1000) {
            console.log('Serving from cache:', key);
            return res.json(cached.data);
        }
        
        // Override res.json to cache the response
        const originalJson = res.json;
        res.json = function(data) {
            cache.set(key, {
                data,
                timestamp: Date.now()
            });
            return originalJson.call(this, data);
        };
        
        next();
    };
}

app.use('/api/slow-endpoint', cacheMiddleware(600)); // 10 minutes cache
```

**How to Explain:**
"Think of middleware like a series of security checkpoints at an airport. Each checkpoint (middleware) has a specific job: checking tickets, scanning bags, verifying ID, etc. A passenger (HTTP request) must pass through each checkpoint in order. Each checkpoint can either let the passenger through to the next checkpoint (call next()), send them back (send an error response), or add something to their journey (modify the request object). The final destination is the gate (route handler), but the passenger might go through the checkpoints in reverse on their way out (response phase)."

### 10. Error Handling in Node.js

**Theory & Explanation:**
Error handling in Node.js is complex because of its asynchronous nature. Understanding the different types of errors and appropriate handling strategies is crucial for building robust applications.

**Error Types in Node.js:**

1. **Syntax Errors:** Caught at parse time
2. **Runtime Errors:** Occur during execution
3. **Asynchronous Errors:** Happen in callbacks, promises, or async functions
4. **Operational Errors:** Expected errors (network failures, validation errors)
5. **Programmer Errors:** Bugs in code (accessing undefined properties)

**Error Handling Patterns:**

**1. Callback Error Handling (Error-First Callbacks):**
```javascript
const fs = require('fs');

// Error-first callback pattern
function readConfigFile(callback) {
    fs.readFile('config.json', 'utf8', (err, data) => {
        if (err) {
            // Always check for errors first
            if (err.code === 'ENOENT') {
                return callback(new Error('Configuration file not found'));
            }
            return callback(err);
        }
        
        try {
            const config = JSON.parse(data);
            callback(null, config); // Success: null error, data as second argument
        } catch (parseError) {
            callback(new Error(`Invalid JSON in config file: ${parseError.message}`));
        }
    });
}

// Usage
readConfigFile((err, config) => {
    if (err) {
        console.error('Failed to load config:', err.message);
        process.exit(1);
    }
    
    console.log('Config loaded:', config);
});
```

**2. Promise Error Handling:**
```javascript
const fs = require('fs').promises;

// Promise-based error handling
function readConfigFilePromise() {
    return fs.readFile('config.json', 'utf8')
        .then(data => {
            try {
                return JSON.parse(data);
            } catch (parseError) {
                throw new Error(`Invalid JSON in config file: ${parseError.message}`);
            }
        })
        .catch(err => {
            if (err.code === 'ENOENT') {
                throw new Error('Configuration file not found');
            }
            throw err; // Re-throw other errors
        });
}

// Usage with .then/.catch
readConfigFilePromise()
    .then(config => {
        console.log('Config loaded:', config);
    })
    .catch(err => {
        console.error('Failed to load config:', err.message);
        process.exit(1);
    });

// Chaining with error propagation
readConfigFilePromise()
    .then(config => validateConfig(config))
    .then(validConfig => initializeApp(validConfig))
    .then(() => {
        console.log('Application started successfully');
    })
    .catch(err => {
        console.error('Application startup failed:', err.message);
        process.exit(1);
    });
```

**3. Async/Await Error Handling:**
```javascript
// Modern async/await error handling
async function readConfigFileAsync() {
    try {
        const data = await fs.readFile('config.json', 'utf8');
        const config = JSON.parse(data);
        return config;
    } catch (err) {
        if (err.code === 'ENOENT') {
            throw new Error('Configuration file not found');
        }
        if (err instanceof SyntaxError) {
            throw new Error(`Invalid JSON in config file: ${err.message}`);
        }
        throw err; // Re-throw unexpected errors
    }
}

// Usage
async function startApplication() {
    try {
        const config = await readConfigFileAsync();
        const validatedConfig = await validateConfig(config);
        await initializeDatabase(validatedConfig.database);
        await startServer(validatedConfig.server);
        console.log('Application started successfully');
    } catch (err) {
        console.error('Application startup failed:', err.message);
        process.exit(1);
    }
}
```

**4. Custom Error Classes:**
```javascript
// Base error class
class AppError extends Error {
    constructor(message, statusCode = 500, isOperational = true) {
        super(message);
        
        this.statusCode = statusCode;
        this.isOperational = isOperational;
        this.timestamp = new Date().toISOString();
        
        // Capture stack trace
        Error.captureStackTrace(this, this.constructor);
    }
}

// Specific error types
class ValidationError extends AppError {
    constructor(message, field) {
        super(message, 400);
        this.name = 'ValidationError';
        this.field = field;
    }
}

class NotFoundError extends AppError {
    constructor(resource) {
        super(`${resource} not found`, 404);
        this.name = 'NotFoundError';
        this.resource = resource;
    }
}

class DatabaseError extends AppError {
    constructor(message, originalError) {
        super(message, 500);
        this.name = 'DatabaseError';
        this.originalError = originalError;
    }
}

// Usage in service layer
class UserService {
    async getUserById(id) {
        if (!id || isNaN(id)) {
            throw new ValidationError('User ID must be a valid number', 'id');
        }
        
        try {
            const user = await this.userRepository.findById(id);
            
            if (!user) {
                throw new NotFoundError('User');
            }
            
            return user;
        } catch (err) {
            if (err instanceof AppError) {
                throw err; // Re-throw our custom errors
            }
            
            // Wrap database errors
            throw new DatabaseError('Failed to retrieve user', err);
        }
    }
}
```

**5. Global Error Handling:**
```javascript
// Uncaught exception handler
process.on('uncaughtException', (err) => {
    console.error('Uncaught Exception:', err);
    console.error('Stack:', err.stack);
    
    // Log the error to external service
    logErrorToService(err);
    
    // Graceful shutdown
    process.exit(1);
});

// Unhandled promise rejection
process.on('unhandledRejection', (reason, promise) => {
    console.error('Unhandled Rejection at:', promise, 'reason:', reason);
    
    // Log the error
    logErrorToService(reason);
    
    // Graceful shutdown
    process.exit(1);
});

// Express.js global error handler
function globalErrorHandler(err, req, res, next) {
    // Log error with request context
    console.error({
        error: err.message,
        stack: err.stack,
        url: req.url,
        method: req.method,
        headers: req.headers,
        body: req.body,
        params: req.params,
        query: req.query,
        user: req.user?.id,
        timestamp: new Date().toISOString()
    });
    
    // Send appropriate response based on error type
    if (err instanceof ValidationError) {
        return res.status(err.statusCode).json({
            error: err.message,
            field: err.field
        });
    }
    
    if (err instanceof NotFoundError) {
        return res.status(err.statusCode).json({
            error: err.message,
            resource: err.resource
        });
    }
    
    // Default error response
    const isDevelopment = process.env.NODE_ENV === 'development';
    
    res.status(err.statusCode || 500).json({
        error: isDevelopment ? err.message : 'Something went wrong',
        ...(isDevelopment && { stack: err.stack })
    });
}

app.use(globalErrorHandler);
```

**6. Graceful Error Recovery:**
```javascript
// Circuit breaker pattern for external services
class CircuitBreaker {
    constructor(threshold = 5, timeout = 60000, resetTimeout = 30000) {
        this.failureThreshold = threshold;
        this.timeout = timeout;
        this.resetTimeout = resetTimeout;
        this.failureCount = 0;
        this.lastFailureTime = null;
        this.state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
    }

    async execute(operation) {
        if (this.state === 'OPEN') {
            if (Date.now() - this.lastFailureTime > this.resetTimeout) {
                this.state = 'HALF_OPEN';
                console.log('Circuit breaker half-open, trying again');
            } else {
                throw new Error('Circuit breaker is OPEN - service unavailable');
            }
        }

        try {
            const result = await Promise.race([
                operation(),
                new Promise((_, reject) => 
                    setTimeout(() => reject(new Error('Operation timeout')), this.timeout)
                )
            ]);
            
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
            console.log('Circuit breaker opened due to failures');
        }
    }
}

// Retry mechanism with exponential backoff
async function withRetry(operation, maxRetries = 3, baseDelay = 1000) {
    let lastError;
    
    for (let attempt = 1; attempt <= maxRetries; attempt++) {
        try {
            return await operation();
        } catch (error) {
            lastError = error;
            
            if (attempt === maxRetries) {
                throw error;
            }
            
            // Exponential backoff with jitter
            const delay = baseDelay * Math.pow(2, attempt - 1) + Math.random() * 1000;
            console.log(`Attempt ${attempt} failed, retrying in ${delay}ms`);
            
            await new Promise(resolve => setTimeout(resolve, delay));
        }
    }
}
```

**How to Explain:**
"Error handling in Node.js is like having a comprehensive emergency response system in a building. You need different strategies for different types of emergencies (error types). Fire alarms (try/catch) handle immediate dangers, evacuation procedures (error propagation) ensure problems don't spread, emergency exits (.catch() handlers) provide safe ways out, and emergency services (global error handlers) handle the most serious situations. The key is having multiple layers of protection and always having a plan for when things go wrong, because in asynchronous environments, errors can happen anywhere and at any time."

### 11. What is Clustering in Node.js?

**Theory & Explanation:**
Clustering addresses Node.js's fundamental limitation: it runs on a single thread. While the event loop efficiently handles I/O operations, CPU-intensive tasks can block the entire application. Clustering enables horizontal scaling by creating multiple Node.js processes.

**Clustering Theory:**
- **Process vs Thread:** Node.js clustering creates separate processes, not threads
- **Inter-Process Communication (IPC):** Processes communicate through message passing
- **Load Distribution:** OS kernel distributes incoming connections across processes
- **Shared Server Port:** All worker processes listen on the same port
- **Process Isolation:** If one worker crashes, others continue running

**Master-Worker Architecture:**
```javascript
const cluster = require('cluster');
const http = require('http');
const os = require('os');

if (cluster.isMaster) {
    console.log(`Master ${process.pid} is running`);
    
    // Fork workers equal to CPU cores
    const numCPUs = os.cpus().length;
    console.log(`Forking ${numCPUs} workers`);
    
    // Create worker processes
    for (let i = 0; i < numCPUs; i++) {
        const worker = cluster.fork();
        
        // Listen for worker messages
        worker.on('message', (message) => {
            console.log(`Master received message from worker ${worker.process.pid}:`, message);
        });
    }
    
    // Handle worker events
    cluster.on('online', (worker) => {
        console.log(`Worker ${worker.process.pid} came online`);
    });
    
    cluster.on('exit', (worker, code, signal) => {
        console.log(`Worker ${worker.process.pid} died with code ${code} and signal ${signal}`);
        
        // Restart dead worker
        if (!worker.exitedAfterDisconnect) {
            console.log('Starting a new worker');
            cluster.fork();
        }
    });
    
    // Graceful shutdown
    process.on('SIGTERM', () => {
        console.log('Master received SIGTERM, shutting down gracefully');
        
        // Disconnect all workers
        for (const id in cluster.workers) {
            cluster.workers[id].disconnect();
        }
        
        // Force exit after timeout
        setTimeout(() => {
            console.log('Force exit');
            process.exit(0);
        }, 10000);
    });
    
} else {
    // Worker process
    const server = http.createServer((req, res) => {
        // Simulate some work
        const start = Date.now();
        while (Date.now() - start < 100) {
            // CPU-intensive work simulation
        }
        
        res.writeHead(200);
        res.end(`Hello from worker ${process.pid}\n`);
    });
    
    server.listen(3000, () => {
        console.log(`Worker ${process.pid} started`);
    });
    
    // Send periodic messages to master
    setInterval(() => {
        process.send({
            type: 'status',
            pid: process.pid,
            memory: process.memoryUsage(),
            uptime: process.uptime()
        });
    }, 30000);
    
    // Graceful shutdown
    process.on('SIGTERM', () => {
        console.log(`Worker ${process.pid} received SIGTERM`);
        server.close(() => {
            console.log(`Worker ${process.pid} closed server`);
            process.exit(0);
        });
    });
}
```

**Advanced Clustering with Load Monitoring:**
```javascript
const cluster = require('cluster');
const os = require('os');

class ClusterManager {
    constructor() {
        this.workers = new Map();
        this.maxWorkers = os.cpus().length;
        this.minWorkers = 2;
    }

    start() {
        if (cluster.isMaster) {
            console.log(`Master ${process.pid} starting`);
            
            // Start initial workers
            for (let i = 0; i < this.minWorkers; i++) {
                this.forkWorker();
            }
            
            // Monitor system load
            this.monitorLoad();
            
            // Handle worker events
            cluster.on('exit', (worker, code, signal) => {
                this.handleWorkerExit(worker, code, signal);
            });
            
        } else {
            this.startWorker();
        }
    }

    forkWorker() {
        const worker = cluster.fork();
        const workerData = {
            pid: worker.process.pid,
            startTime: Date.now(),
            requests: 0,
            errors: 0
        };
        
        this.workers.set(worker.id, workerData);
        
        worker.on('message', (message) => {
            this.handleWorkerMessage(worker.id, message);
        });
        
        return worker;
    }

    handleWorkerMessage(workerId, message) {
        const workerData = this.workers.get(workerId);
        if (!workerData) return;
        
        switch (message.type) {
            case 'request':
                workerData.requests++;
                break;
            case 'error':
                workerData.errors++;
                break;
            case 'status':
                console.log(`Worker ${message.pid} status:`, message.data);
                break;
        }
    }

    monitorLoad() {
        setInterval(() => {
            const load = os.loadavg()[0]; // 1-minute load average
            const numWorkers = Object.keys(cluster.workers).length;
            
            console.log(`System load: ${load.toFixed(2)}, Workers: ${numWorkers}`);
            
            // Scale up if load is high and we have capacity
            if (load > 0.8 && numWorkers < this.maxWorkers) {
                console.log('High load detected, scaling up');
                this.forkWorker();
            }
            
            // Scale down if load is low and we have excess workers
            if (load < 0.3 && numWorkers > this.minWorkers) {
                console.log('Low load detected, scaling down');
                this.killWorker();
            }
            
        }, 10000); // Check every 10 seconds
    }

    killWorker() {
        const workerIds = Object.keys(cluster.workers);
        if (workerIds.length <= this.minWorkers) return;
        
        // Find worker with least requests
        let targetWorkerId = workerIds[0];
        let minRequests = this.workers.get(parseInt(targetWorkerId))?.requests || 0;
        
        for (const id of workerIds) {
            const requests = this.workers.get(parseInt(id))?.requests || 0;
            if (requests < minRequests) {
                minRequests = requests;
                targetWorkerId = id;
            }
        }
        
        const worker = cluster.workers[targetWorkerId];
        if (worker) {
            console.log(`Gracefully shutting down worker ${worker.process.pid}`);
            worker.disconnect();
        }
    }

    handleWorkerExit(worker, code, signal) {
        console.log(`Worker ${worker.process.pid} died (${signal || code})`);
        this.workers.delete(worker.id);
        
        // Always maintain minimum workers
        if (Object.keys(cluster.workers).length < this.minWorkers) {
            this.forkWorker();
        }
    }

    startWorker() {
        const express = require('express');
        const app = express();
        
        // Middleware to track requests
        app.use((req, res, next) => {
            process.send({ type: 'request' });
            next();
        });
        
        app.get('/health', (req, res) => {
            res.json({
                pid: process.pid,
                uptime: process.uptime(),
                memory: process.memoryUsage()
            });
        });
        
        app.get('/cpu-intensive', (req, res) => {
            // Simulate CPU-intensive task
            const start = Date.now();
            let result = 0;
            
            for (let i = 0; i < 1000000; i++) {
                result += Math.random();
            }
            
            const duration = Date.now() - start;
            
            res.json({
                pid: process.pid,
                result: result,
                duration: duration
            });
        });
        
        // Error handler
        app.use((err, req, res, next) => {
            process.send({ type: 'error', error: err.message });
            res.status(500).json({ error: 'Internal server error' });
        });
        
        const server = app.listen(3000, () => {
            console.log(`Worker ${process.pid} listening on port 3000`);
        });
        
        // Graceful shutdown
        process.on('SIGTERM', () => {
            console.log(`Worker ${process.pid} shutting down`);
            server.close(() => {
                process.exit(0);
            });
        });
    }
}

// Start cluster manager
const clusterManager = new ClusterManager();
clusterManager.start();
```

**Zero-Downtime Deployment:**
```javascript
const cluster = require('cluster');

class ZeroDowntimeCluster {
    constructor() {
        this.workers = new Map();
        this.restarting = false;
    }

    start() {
        if (cluster.isMaster) {
            this.startMaster();
        } else {
            this.startWorker();
        }
    }

    startMaster() {
        const numCPUs = require('os').cpus().length;
        
        // Fork initial workers
        for (let i = 0; i < numCPUs; i++) {
            this.forkWorker();
        }
        
        // Listen for restart signal
        process.on('SIGUSR2', () => {
            console.log('Received SIGUSR2, performing zero-downtime restart');
            this.performZeroDowntimeRestart();
        });
        
        cluster.on('exit', (worker, code, signal) => {
            if (!this.restarting && !worker.exitedAfterDisconnect) {
                console.log(`Worker ${worker.process.pid} crashed, restarting`);
                this.forkWorker();
            }
        });
    }

    async performZeroDowntimeRestart() {
        if (this.restarting) {
            console.log('Restart already in progress');
            return;
        }
        
        this.restarting = true;
        const workers = Object.values(cluster.workers);
        
        console.log(`Restarting ${workers.length} workers`);
        
        // Restart workers one by one
        for (let i = 0; i < workers.length; i++) {
            const worker = workers[i];
            
            // Fork new worker first
            const newWorker = this.forkWorker();
            
            // Wait for new worker to be ready
            await this.waitForWorkerReady(newWorker);
            
            // Gracefully shutdown old worker
            console.log(`Shutting down old worker ${worker.process.pid}`);
            worker.disconnect();
            
            // Wait for old worker to exit
            await this.waitForWorkerExit(worker);
            
            // Small delay between restarts
            await new Promise(resolve => setTimeout(resolve, 1000));
        }
        
        this.restarting = false;
        console.log('Zero-downtime restart completed');
    }

    waitForWorkerReady(worker) {
        return new Promise((resolve) => {
            const timeout = setTimeout(() => {
                resolve(); // Assume ready after timeout
            }, 5000);
            
            worker.once('message', (message) => {
                if (message.type === 'ready') {
                    clearTimeout(timeout);
                    resolve();
                }
            });
        });
    }

    waitForWorkerExit(worker) {
        return new Promise((resolve) => {
            const timeout = setTimeout(() => {
                worker.kill('SIGKILL'); // Force kill if graceful shutdown fails
                resolve();
            }, 10000);
            
            worker.once('exit', () => {
                clearTimeout(timeout);
                resolve();
            });
        });
    }

    forkWorker() {
        const worker = cluster.fork();
        this.workers.set(worker.id, worker);
        return worker;
    }

    startWorker() {
        const app = require('express')();
        
        app.get('/', (req, res) => {
            res.json({
                pid: process.pid,
                uptime: process.uptime(),
                timestamp: new Date().toISOString()
            });
        });
        
        const server = app.listen(3000, () => {
            console.log(`Worker ${process.pid} ready`);
            // Signal that worker is ready
            process.send({ type: 'ready' });
        });
        
        // Graceful shutdown
        process.on('SIGTERM', () => {
            console.log(`Worker ${process.pid} shutting down gracefully`);
            server.close(() => {
                process.exit(0);
            });
        });
    }
}

const cluster = new ZeroDowntimeCluster();
cluster.start();

// To trigger zero-downtime restart:
// kill -USR2 <master_pid>
```

**When to Use Clustering:**
- **CPU-intensive applications:** Image processing, data analytics
- **High-traffic web applications:** Need to handle many concurrent requests
- **Improved fault tolerance:** Process isolation prevents single points of failure
- **Better resource utilization:** Use all available CPU cores

**When NOT to Use Clustering:**
- **I/O-intensive applications:** Single instance usually sufficient
- **Memory-constrained environments:** Multiple processes consume more memory
- **Stateful applications:** Shared state becomes complex across processes
- **Simple applications:** Overhead may not be worth the complexity

**How to Explain:**
"Imagine Node.js clustering like a restaurant that starts with one cook (single process). As the restaurant gets busier, they hire more cooks (workers) who can all work simultaneously in the same kitchen (server). The head chef (master process) coordinates the cooks, assigns orders, and if one cook gets sick (worker crashes), they can hire a replacement without closing the restaurant. Each cook works independently but they all serve customers from the same menu (application code) at the same restaurant (port). This way, the restaurant can handle much more traffic and if one cook has a problem, the others keep working."

### 12. Worker Threads vs Child Processes

**Theory & Explanation:**
Both Worker Threads and Child Processes solve Node.js's single-threaded limitations, but they use fundamentally different approaches with distinct trade-offs.

**Conceptual Differences:**

**Worker Threads:**
- **Shared Memory Space:** Can share ArrayBuffers and other transferable objects
- **Lightweight:** Lower overhead than processes
- **V8 Isolates:** Separate JavaScript contexts within same process
- **Limited Isolation:** Share some resources with main thread

**Child Processes:**
- **Complete Isolation:** Separate memory space and resources
- **Higher Overhead:** More memory and CPU usage
- **Cross-Platform:** Can run any executable, not just Node.js
- **Robust Separation:** Complete failure isolation

**Worker Threads Implementation:**
```javascript
// main.js - Worker Threads Example
const { Worker, isMainThread, parentPort, workerData } = require('worker_threads');
const os = require('os');

if (isMainThread) {
    // Main thread
    console.log('Main thread started');
    
    class WorkerPool {
        constructor(workerFile, poolSize = os.cpus().length) {
            this.workerFile = workerFile;
            this.poolSize = poolSize;
            this.workers = [];
            this.queue = [];
            this.workerIndex = 0;
            
            this.initializeWorkers();
        }
        
        initializeWorkers() {
            for (let i = 0; i < this.poolSize; i++) {
                this.createWorker();
            }
        }
        
        createWorker() {
            const worker = new Worker(__filename, {
                workerData: { isWorker: true }
            });
            
            worker.available = true;
            worker.tasks = 0;
            
            worker.on('message', (result) => {
                worker.available = true;
                
                if (result.error) {
                    result.reject(new Error(result.error));
                } else {
                    result.resolve(result.data);
                }
                
                this.processQueue();
            });
            
            worker.on('error', (error) => {
                console.error('Worker error:', error);
                this.replaceWorker(worker);
            });
            
            worker.on('exit', (code) => {
                if (code !== 0) {
                    console.error(`Worker stopped with exit code ${code}`);
                    this.replaceWorker(worker);
                }
            });
            
            this.workers.push(worker);
        }
        
        replaceWorker(deadWorker) {
            const index = this.workers.indexOf(deadWorker);
            if (index !== -1) {
                this.workers[index] = this.createWorker();
            }
        }
        
        execute(data) {
            return new Promise((resolve, reject) => {
                const task = { data, resolve, reject };
                
                const availableWorker = this.workers.find(w => w.available);
                if (availableWorker) {
                    this.assignTask(availableWorker, task);
                } else {
                    this.queue.push(task);
                }
            });
        }
        
        assignTask(worker, task) {
            worker.available = false;
            worker.tasks++;
            
            worker.postMessage({
                taskId: Date.now(),
                data: task.data,
                resolve: task.resolve,
                reject: task.reject
            });
        }
        
        processQueue() {
            if (this.queue.length === 0) return;
            
            const availableWorker = this.workers.find(w => w.available);
            if (availableWorker) {
                const task = this.queue.shift();
                this.assignTask(availableWorker, task);
            }
        }
        
        terminate() {
            return Promise.all(this.workers.map(worker => worker.terminate()));
        }
        
        getStats() {
            return this.workers.map((worker, index) => ({
                id: index,
                available: worker.available,
                tasksCompleted: worker.tasks
            }));
        }
    }
    
    // CPU-intensive task examples
    async function demonstrateWorkerThreads() {
        const pool = new WorkerPool(__filename, 4);
        
        console.log('Starting CPU-intensive tasks...');
        const startTime = Date.now();
        
        // Create multiple CPU-intensive tasks
        const tasks = Array.from({ length: 8 }, (_, i) => ({
            operation: 'fibonacci',
            input: 35 + i,
            id: i
        }));
        
        try {
            // Execute tasks in parallel
            const results = await Promise.all(
                tasks.map(task => pool.execute(task))
            );
            
            console.log('All tasks completed in', Date.now() - startTime, 'ms');
            console.log('Results:', results);
            console.log('Worker stats:', pool.getStats());
            
        } catch (error) {
            console.error('Task execution failed:', error);
        } finally {
            await pool.terminate();
        }
    }
    
    demonstrateWorkerThreads();
    
} else {
    // Worker thread
    console.log(`Worker thread ${workerData.id} started`);
    
    // CPU-intensive functions
    function fibonacci(n) {
        if (n < 2) return n;
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
    
    function primeFactors(n) {
        const factors = [];
        let divisor = 2;
        
        while (n >= 2) {
            if (n % divisor === 0) {
                factors.push(divisor);
                n = n / divisor;
            } else {
                divisor++;
            }
        }
        return factors;
    }
    
    function matrixMultiply(size = 100) {
        const a = Array(size).fill().map(() => Array(size).fill(Math.random()));
        const b = Array(size).fill().map(() => Array(size).fill(Math.random()));
        const result = Array(size).fill().map(() => Array(size).fill(0));
        
        for (let i = 0; i < size; i++) {
            for (let j = 0; j < size; j++) {
                for (let k = 0; k < size; k++) {
                    result[i][j] += a[i][k] * b[k][j];
                }
            }
        }
        
        return result[0][0]; // Return just first element to avoid large data transfer
    }
    
    // Listen for tasks from main thread
    parentPort.on('message', async (message) => {
        const { taskId, data } = message;
        
        try {
            let result;
            const startTime = Date.now();
            
            switch (data.operation) {
                case 'fibonacci':
                    result = fibonacci(data.input);
                    break;
                case 'primeFactors':
                    result = primeFactors(data.input);
                    break;
                case 'matrixMultiply':
                    result = matrixMultiply(data.input);
                    break;
                default:
                    throw new Error(`Unknown operation: ${data.operation}`);
            }
            
            const duration = Date.now() - startTime;
            
            // Send result back to main thread
            parentPort.postMessage({
                taskId,
                data: {
                    result,
                    duration,
                    workerId: workerData.id,
                    operation: data.operation,
                    input: data.input
                },
                resolve: message.resolve
            });
            
        } catch (error) {
            parentPort.postMessage({
                taskId,
                error: error.message,
                reject: message.reject
            });
        }
    });
    
    // Handle termination
    process.on('SIGTERM', () => {
        console.log(`Worker ${workerData.id} terminating`);
        process.exit(0);
    });
}
```

**Child Processes Implementation:**
```javascript
// main.js - Child Processes Example
const { spawn, fork, exec, execFile } = require('child_process');
const path = require('path');

class ChildProcessPool {
    constructor(scriptPath, poolSize = 4) {
        this.scriptPath = scriptPath;
        this.poolSize = poolSize;
        this.processes = [];
        this.queue = [];
        this.taskCounter = 0;
        
        this.initializeProcesses();
    }
    
    initializeProcesses() {
        for (let i = 0; i < this.poolSize; i++) {
            this.createProcess(i);
        }
    }
    
    createProcess(id) {
        const child = fork(this.scriptPath, [`--worker-id=${id}`], {
            stdio: ['pipe', 'pipe', 'pipe', 'ipc']
        });
        
        child.available = true;
        child.tasks = 0;
        child.workerId = id;
        child.pendingTasks = new Map();
        
        child.on('message', (message) => {
            const { taskId, error, result } = message;
            const pendingTask = child.pendingTasks.get(taskId);
            
            if (pendingTask) {
                child.pendingTasks.delete(taskId);
                child.available = true;
                
                if (error) {
                    pendingTask.reject(new Error(error));
                } else {
                    pendingTask.resolve(result);
                }
                
                this.processQueue();
            }
        });
        
        child.on('error', (error) => {
            console.error(`Child process ${id} error:`, error);
            this.replaceProcess(child);
        });
        
        child.on('exit', (code, signal) => {
            console.log(`Child process ${id} exited with code ${code}, signal ${signal}`);
            if (code !== 0) {
                this.replaceProcess(child);
            }
        });
        
        // Handle stdout/stderr
        child.stdout.on('data', (data) => {
            console.log(`Child ${id} stdout: ${data}`);
        });
        
        child.stderr.on('data', (data) => {
            console.error(`Child ${id} stderr: ${data}`);
        });
        
        this.processes.push(child);
        return child;
    }
    
    replaceProcess(deadProcess) {
        const index = this.processes.indexOf(deadProcess);
        if (index !== -1) {
            // Reject all pending tasks
            deadProcess.pendingTasks.forEach(task => {
                task.reject(new Error('Process died'));
            });
            
            // Create replacement
            this.processes[index] = this.createProcess(deadProcess.workerId);
        }
    }
    
    execute(operation, data) {
        return new Promise((resolve, reject) => {
            const taskId = ++this.taskCounter;
            const task = { taskId, operation, data, resolve, reject };
            
            const availableProcess = this.processes.find(p => p.available);
            if (availableProcess) {
                this.assignTask(availableProcess, task);
            } else {
                this.queue.push(task);
            }
        });
    }
    
    assignTask(process, task) {
        process.available = false;
        process.tasks++;
        process.pendingTasks.set(task.taskId, {
            resolve: task.resolve,
            reject: task.reject
        });
        
        process.send({
            taskId: task.taskId,
            operation: task.operation,
            data: task.data
        });
    }
    
    processQueue() {
        if (this.queue.length === 0) return;
        
        const availableProcess = this.processes.find(p => p.available);
        if (availableProcess) {
            const task = this.queue.shift();
            this.assignTask(availableProcess, task);
        }
    }
    
    async terminate() {
        return Promise.all(this.processes.map(process => {
            return new Promise((resolve) => {
                process.once('exit', resolve);
                process.kill('SIGTERM');
                
                // Force kill after timeout
                setTimeout(() => {
                    process.kill('SIGKILL');
                    resolve();
                }, 5000);
            });
        }));
    }
    
    getStats() {
        return this.processes.map(process => ({
            pid: process.pid,
            workerId: process.workerId,
            available: process.available,
            tasksCompleted: process.tasks,
            pendingTasks: process.pendingTasks.size
        }));
    }
}

// Demonstration
async function demonstrateChildProcesses() {
    const pool = new ChildProcessPool('./worker-process.js', 3);
    
    console.log('Starting child process tasks...');
    const startTime = Date.now();
    
    const tasks = [
        { operation: 'fibonacci', data: { n: 35 } },
        { operation: 'fileProcess', data: { filename: 'large-file.txt' } },
        { operation: 'httpRequest', data: { url: 'https://api.github.com/users/octocat' } },
        { operation: 'imageProcess', data: { width: 800, height: 600 } },
        { operation: 'dataAnalysis', data: { dataset: Array(10000).fill().map(() => Math.random()) } }
    ];
    
    try {
        const results = await Promise.all(
            tasks.map(task => pool.execute(task.operation, task.data))
        );
        
        console.log('All tasks completed in', Date.now() - startTime, 'ms');
        console.log('Results:', results.map(r => ({ ...r, data: r.data ? 'truncated' : null })));
        console.log('Process stats:', pool.getStats());
        
    } catch (error) {
        console.error('Task execution failed:', error);
    } finally {
        await pool.terminate();
    }
}

// Execute different types of child processes
async function demonstrateChildProcessTypes() {
    // 1. spawn - for streaming data
    console.log('=== SPAWN Example ===');
    const ls = spawn('ls', ['-la']);
    
    ls.stdout.on('data', (data) => {
        console.log(`stdout: ${data}`);
    });
    
    ls.on('close', (code) => {
        console.log(`ls process exited with code ${code}`);
    });
    
    // 2. exec - for shell commands
    console.log('\n=== EXEC Example ===');
    exec('echo "Hello from exec"', (error, stdout, stderr) => {
        if (error) {
            console.error(`exec error: ${error}`);
            return;
        }
        console.log(`stdout: ${stdout}`);
        console.error(`stderr: ${stderr}`);
    });
    
    // 3. execFile - for executable files
    console.log('\n=== EXECFILE Example ===');
    execFile('node', ['--version'], (error, stdout, stderr) => {
        if (error) {
            console.error(`execFile error: ${error}`);
            return;
        }
        console.log(`Node version: ${stdout}`);
    });
    
    // 4. fork - for Node.js modules with IPC
    console.log('\n=== FORK Example ===');
    if (require.main === module) {
        demonstrateChildProcesses();
    }
}

if (require.main === module) {
    demonstrateChildProcessTypes();
}
```

**worker-process.js:**
```javascript
// worker-process.js - Child Process Worker
const fs = require('fs').promises;
const https = require('https');

// Get worker ID from command line arguments
const workerId = process.argv.find(arg => arg.startsWith('--worker-id='))?.split('=')[1] || 'unknown';

console.log(`Child process ${workerId} (PID: ${process.pid}) started`);

// Task implementations
const tasks = {
    fibonacci(n) {
        if (n < 2) return n;
        return tasks.fibonacci(n - 1) + tasks.fibonacci(n - 2);
    },
    
    async fileProcess({ filename }) {
        try {
            // Create a large file for processing
            const content = Array(1000).fill('Sample line of text for file processing\n').join('');
            await fs.writeFile(filename, content);
            
            // Read and process the file
            const data = await fs.readFile(filename, 'utf8');
            const lines = data.split('\n').filter(line => line.length > 0);
            const wordCount = lines.reduce((count, line) => count + line.split(' ').length, 0);
            
            // Clean up
            await fs.unlink(filename);
            
            return {
                lines: lines.length,
                words: wordCount,
                characters: data.length
            };
        } catch (error) {
            throw new Error(`File processing failed: ${error.message}`);
        }
    },
    
    async httpRequest({ url }) {
        return new Promise((resolve, reject) => {
            https.get(url, (response) => {
                let data = '';
                
                response.on('data', chunk => {
                    data += chunk;
                });
                
                response.on('end', () => {
                    try {
                        const parsed = JSON.parse(data);
                        resolve({
                            status: response.statusCode,
                            headers: response.headers,
                            dataLength: data.length,
                            hasData: !!parsed
                        });
                    } catch (error) {
                        reject(new Error(`JSON parsing failed: ${error.message}`));
                    }
                });
                
            }).on('error', (error) => {
                reject(error);
            });
        });
    },
    
    imageProcess({ width, height }) {
        // Simulate image processing
        const pixels = width * height;
        const channels = 4; // RGBA
        const imageData = new Array(pixels * channels);
        
        // Simulate complex image processing
        for (let i = 0; i < pixels; i++) {
            const x = i % width;
            const y = Math.floor(i / width);
            
            // Simple pattern generation
            imageData[i * 4] = (x * 255) / width;     // Red
            imageData[i * 4 + 1] = (y * 255) / height; // Green  
            imageData[i * 4 + 2] = 128;               // Blue
            imageData[i * 4 + 3] = 255;               // Alpha
        }
        
        return {
            width,
            height,
            pixels,
            channels,
            dataSize: imageData.length
        };
    },
    
    dataAnalysis({ dataset }) {
        // Statistical analysis
        const sum = dataset.reduce((a, b) => a + b, 0);
        const mean = sum / dataset.length;
        
        const variance = dataset.reduce((acc, val) => acc + Math.pow(val - mean, 2), 0) / dataset.length;
        const stdDev = Math.sqrt(variance);
        
        const sorted = [...dataset].sort((a, b) => a - b);
        const median = sorted.length % 2 === 0
            ? (sorted[sorted.length / 2 - 1] + sorted[sorted.length / 2]) / 2
            : sorted[Math.floor(sorted.length / 2)];
        
        return {
            count: dataset.length,
            sum,
            mean,
            median,
            variance,
            stdDev,
            min: Math.min(...dataset),
            max: Math.max(...dataset)
        };
    }
};

// Handle messages from parent process
process.on('message', async (message) => {
    const { taskId, operation, data } = message;
    
    try {
        console.log(`Worker ${workerId} executing ${operation} task ${taskId}`);
        const startTime = Date.now();
        
        let result;
        if (tasks[operation]) {
            result = await tasks[operation](data);
        } else {
            throw new Error(`Unknown operation: ${operation}`);
        }
        
        const duration = Date.now() - startTime;
        
        process.send({
            taskId,
            result: {
                ...result,
                workerId,
                duration,
                operation
            }
        });
        
        console.log(`Worker ${workerId} completed task ${taskId} in ${duration}ms`);
        
    } catch (error) {
        console.error(`Worker ${workerId} task ${taskId} failed:`, error.message);
        process.send({
            taskId,
            error: error.message
        });
    }
});

// Handle termination signals
process.on('SIGTERM', () => {
    console.log(`Worker ${workerId} received SIGTERM, shutting down`);
    process.exit(0);
});

process.on('SIGINT', () => {
    console.log(`Worker ${workerId} received SIGINT, shutting down`);
    process.exit(0);
});

// Notify parent that worker is ready
process.send({ type: 'ready', workerId, pid: process.pid });
```

**Comparison Summary:**

| Feature | Worker Threads | Child Processes |
|---------|---------------|-----------------|
| Memory Sharing | Shared ArrayBuffer | No shared memory |
| Startup Time | Faster | Slower |
| Memory Usage | Lower overhead | Higher overhead |
| Isolation | Limited | Complete |
| Communication | MessagePort, SharedArrayBuffer | IPC, pipes, sockets |
| Error Isolation | Partial | Complete |
| Platform Support | Node.js 10.5+ | All versions |
| Use Cases | CPU-intensive JS tasks | Any executable, better isolation |

**How to Explain:**
"Think of Worker Threads vs Child Processes like two different ways to organize a company's work:

**Worker Threads** are like having multiple departments in the same office building. They can share resources like the cafeteria and parking lot (shared memory), communicate quickly through interoffice messages (MessagePort), and it's cheaper to set up new departments (lower overhead). However, if there's a fire in one department, it might affect others (limited isolation).

**Child Processes** are like having completely separate office buildings for different departments. Each building has its own facilities (separate memory), they communicate by phone or email (IPC), and it's more expensive to set up (higher overhead). However, if one building has a problem, others are completely unaffected (complete isolation). You can even have buildings run by different companies (different executables)."

### 13. Environment Variables in Node.js

**Theory & Explanation:**
Environment variables are a fundamental concept in software deployment and configuration management. They provide a way to externalize configuration from code, supporting the **Twelve-Factor App** methodology principle of storing configuration in the environment.

**Why Environment Variables Matter:**
- **Security:** Keep sensitive data out of source code
- **Flexibility:** Different configurations for different environments
- **Scalability:** Easy deployment across multiple environments
- **Compliance:** Meet security requirements for credential management

**Environment Variable Patterns:**

**1. Basic Environment Variable Usage:**
```javascript
// Access environment variables
const config = {
    // Database configuration
    database: {
        host: process.env.DB_HOST || 'localhost',
        port: parseInt(process.env.DB_PORT) || 5432,
        name: process.env.DB_NAME || 'myapp',
        username: process.env.DB_USERNAME || 'user',
        password: process.env.DB_PASSWORD, // Required, no default
    },
    
    // Server configuration
    server: {
        port: parseInt(process.env.PORT) || 3000,
        host: process.env.HOST || '0.0.0.0',
        nodeEnv: process.env.NODE_ENV || 'development'
    },
    
    // API keys and secrets
    jwt: {
        secret: process.env.JWT_SECRET, // Required
        expiresIn: process.env.JWT_EXPIRES_IN || '24h'
    },
    
    // Third-party services
    services: {
        redisUrl: process.env.REDIS_URL || 'redis://localhost:6379',
        emailService: {
            apiKey: process.env.SENDGRID_API_KEY,
            fromEmail: process.env.FROM_EMAIL || 'noreply@myapp.com'
        },
        aws: {
            accessKeyId: process.env.AWS_ACCESS_KEY_ID,
            secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
            region: process.env.AWS_REGION || 'us-east-1'
        }
    },
    
    // Feature flags
    features: {
        enableNewFeature: process.env.ENABLE_NEW_FEATURE === 'true',
        maxUploadSize: parseInt(process.env.MAX_UPLOAD_SIZE) || 10485760, // 10MB
        debugMode: process.env.DEBUG === 'true'
    }
};

// Validate required environment variables
function validateEnvironment() {
    const required = [
        'DB_PASSWORD',
        'JWT_SECRET'
    ];
    
    const missing = required.filter(key => !process.env[key]);
    
    if (missing.length > 0) {
        console.error('Missing required environment variables:', missing.join(', '));
        process.exit(1);
    }
}

validateEnvironment();
```

**2. Advanced Configuration Management:**
```javascript
// config/index.js - Centralized configuration management
class ConfigManager {
    constructor() {
        this.config = {};
        this.loadEnvironment();
        this.validateConfig();
        this.transformConfig();
    }
    
    loadEnvironment() {
        // Load from .env file if in development
        if (process.env.NODE_ENV !== 'production') {
            require('dotenv').config();
        }
        
        this.config = {
            environment: process.env.NODE_ENV || 'development',
            
            server: {
                port: this.getNumber('PORT', 3000),
                host: process.env.HOST || '0.0.0.0',
                cors: {
                    origins: this.getArray('CORS_ORIGINS', ['http://localhost:3000']),
                    credentials: this.getBoolean('CORS_CREDENTIALS', true)
                }
            },
            
            database: {
                url: process.env.DATABASE_URL,
                host: process.env.DB_HOST || 'localhost',
                port: this.getNumber('DB_PORT', 5432),
                name: process.env.DB_NAME || 'myapp',
                username: process.env.DB_USERNAME || 'postgres',
                password: process.env.DB_PASSWORD,
                ssl: this.getBoolean('DB_SSL', false),
                poolSize: this.getNumber('DB_POOL_SIZE', 10),
                connectionTimeoutMs: this.getNumber('DB_CONNECTION_TIMEOUT', 30000)
            },
            
            cache: {
                redis: {
                    url: process.env.REDIS_URL || 'redis://localhost:6379',
                    ttl: this.getNumber('CACHE_TTL', 3600),
                    maxRetries: this.getNumber('REDIS_MAX_RETRIES', 3)
                }
            },
            
            auth: {
                jwt: {
                    secret: process.env.JWT_SECRET,
                    expiresIn: process.env.JWT_EXPIRES_IN || '7d',
                    issuer: process.env.JWT_ISSUER || 'myapp',
                    audience: process.env.JWT_AUDIENCE || 'myapp-users'
                },
                oauth: {
                    google: {
                        clientId: process.env.GOOGLE_CLIENT_ID,
                        clientSecret: process.env.GOOGLE_CLIENT_SECRET,
                        callbackUrl: process.env.GOOGLE_CALLBACK_URL
                    }
                },
                passwordPolicy: {
                    minLength: this.getNumber('PASSWORD_MIN_LENGTH', 8),
                    requireUppercase: this.getBoolean('PASSWORD_REQUIRE_UPPERCASE', true),
                    requireNumbers: this.getBoolean('PASSWORD_REQUIRE_NUMBERS', true),
                    requireSymbols: this.getBoolean('PASSWORD_REQUIRE_SYMBOLS', true)
                }
            },
            
            logging: {
                level: process.env.LOG_LEVEL || 'info',
                format: process.env.LOG_FORMAT || 'json',
                destination: process.env.LOG_DESTINATION || 'console'
            },
            
            monitoring: {
                enabled: this.getBoolean('MONITORING_ENABLED', true),
                metricsPort: this.getNumber('METRICS_PORT', 9090),
                healthCheckInterval: this.getNumber('HEALTH_CHECK_INTERVAL', 30000)
            },
            
            rateLimit: {
                windowMs: this.getNumber('RATE_LIMIT_WINDOW_MS', 900000), // 15 minutes
                maxRequests: this.getNumber('RATE_LIMIT_MAX_REQUESTS', 100),
                skipSuccessfulRequests: this.getBoolean('RATE_LIMIT_SKIP_SUCCESS', false)
            },
            
            email: {
                provider: process.env.EMAIL_PROVIDER || 'sendgrid',
                apiKey: process.env.EMAIL_API_KEY,
                fromAddress: process.env.EMAIL_FROM_ADDRESS || 'noreply@myapp.com',
                templates: {
                    welcome: process.env.WELCOME_EMAIL_TEMPLATE_ID,
                    passwordReset: process.env.PASSWORD_RESET_TEMPLATE_ID
                }
            },
            
            aws: {
                accessKeyId: process.env.AWS_ACCESS_KEY_ID,
                secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
                region: process.env.AWS_REGION || 'us-east-1',
                s3: {
                    bucket: process.env.AWS_S3_BUCKET,
                    publicUrl: process.env.AWS_S3_PUBLIC_URL
                }
            }
        };
    }
    
    validateConfig() {
        const requiredVars = [
            'JWT_SECRET',
            'DB_PASSWORD'
        ];
        
        const conditionallyRequired = [
            { condition: this.config.environment === 'production', vars: ['DATABASE_URL'] },
            { condition: this.config.email.provider === 'sendgrid', vars: ['EMAIL_API_KEY'] },
            { condition: this.config.aws.s3.bucket, vars: ['AWS_ACCESS_KEY_ID', 'AWS_SECRET_ACCESS_KEY'] }
        ];
        
        // Check required variables
        const missing = requiredVars.filter(key => !process.env[key]);
        
        // Check conditionally required variables
        conditionallyRequired.forEach(({ condition, vars }) => {
            if (condition) {
                vars.forEach(varName => {
                    if (!process.env[varName]) {
                        missing.push(varName);
                    }
                });
            }
        });
        
        if (missing.length > 0) {
            console.error('❌ Missing required environment variables:', missing.join(', '));
            console.error('Please check your environment configuration.');
            process.exit(1);
        }
        
        // Validate format of specific variables
        this.validateFormats();
    }
    
    validateFormats() {
        const validations = [
            {
                key: 'PORT',
                value: process.env.PORT,
                validator: (val) => !val || (!isNaN(val) && parseInt(val) > 0 && parseInt(val) < 65536),
                message: 'PORT must be a number between 1 and 65535'
            },
            {
                key: 'JWT_SECRET',
                value: process.env.JWT_SECRET,
                validator: (val) => !val || val.length >= 32,
                message: 'JWT_SECRET must be at least 32 characters long'
            },
            {
                key: 'DATABASE_URL',
                value: process.env.DATABASE_URL,
                validator: (val) => !val || val.startsWith('postgres://') || val.startsWith('postgresql://'),
                message: 'DATABASE_URL must be a valid PostgreSQL connection string'
            },
            {
                key: 'REDIS_URL',
                value: process.env.REDIS_URL,
                validator: (val) => !val || val.startsWith('redis://') || val.startsWith('rediss://'),
                message: 'REDIS_URL must be a valid Redis connection string'
            }
        ];
        
        validations.forEach(({ key, value, validator, message }) => {
            if (value && !validator(value)) {
                console.error(`❌ Invalid ${key}: ${message}`);
                process.exit(1);
            }
        });
    }
    
    transformConfig() {
        // Transform database URL into components if provided
        if (this.config.database.url) {
            const url = new URL(this.config.database.url);
            this.config.database.host = url.hostname;
            this.config.database.port = parseInt(url.port) || 5432;
            this.config.database.name = url.pathname.slice(1);
            this.config.database.username = url.username;
            this.config.database.password = url.password;
        }
    }
    
    // Helper methods for type conversion
    getNumber(key, defaultValue) {
        const value = process.env[key];
        if (value === undefined) return defaultValue;
        const parsed = parseInt(value);
        return isNaN(parsed) ? defaultValue : parsed;
    }
    
    getBoolean(key, defaultValue) {
        const value = process.env[key];
        if (value === undefined) return defaultValue;
        return value.toLowerCase() === 'true';
    }
    
    getArray(key, defaultValue = []) {
        const value = process.env[key];
        if (!value) return defaultValue;
        return value.split(',').map(item => item.trim()).filter(Boolean);
    }
    
    // Environment-specific getters
    isDevelopment() {
        return this.config.environment === 'development';
    }
    
    isProduction() {
        return this.config.environment === 'production';
    }
    
    isTest() {
        return this.config.environment === 'test';
    }
    
    // Get configuration
    get(path) {
        return path.split('.').reduce((obj, key) => obj && obj[key], this.config);
    }
    
    // Print sanitized configuration (for debugging)
    printConfig() {
        const sanitized = this.sanitizeForLogging(this.config);
        console.log('📋 Application Configuration:');
        console.log(JSON.stringify(sanitized, null, 2));
    }
    
    sanitizeForLogging(obj) {
        const sensitiveKeys = ['password', 'secret', 'key', 'token', 'apikey'];
        const sanitized = {};
        
        for (const [key, value] of Object.entries(obj)) {
            if (typeof value === 'object' && value !== null) {
                sanitized[key] = this.sanitizeForLogging(value);
            } else if (sensitiveKeys.some(sensitive => key.toLowerCase().includes(sensitive))) {
                sanitized[key] = value ? '***REDACTED***' : undefined;
            } else {
                sanitized[key] = value;
            }
        }
        
        return sanitized;
    }
}

// Export singleton instance
const config = new ConfigManager();

// Load configuration on startup
if (config.isDevelopment()) {
    config.printConfig();
}

module.exports = config.config;
```

**3. dotenv Implementation and Best Practices:**
```javascript
// Environment-specific .env files

// .env (development defaults)
NODE_ENV=development
PORT=3000
DB_HOST=localhost
DB_PORT=5432
DB_NAME=myapp_dev
DB_USERNAME=postgres
# DB_PASSWORD=your_dev_password

# JWT Configuration
JWT_SECRET=your-super-secret-jwt-key-at-least-32-characters-long
JWT_EXPIRES_IN=7d

# Redis
REDIS_URL=redis://localhost:6379

# Logging
LOG_LEVEL=debug
DEBUG=true

# Features
ENABLE_NEW_FEATURE=true
MAX_UPLOAD_SIZE=10485760

// .env.production
NODE_ENV=production
PORT=8080

# Database (use DATABASE_URL in production)
# DATABASE_URL=postgresql://user:pass@host:port/dbname
DB_SSL=true
DB_POOL_SIZE=20

# Security
JWT_SECRET=${JWT_SECRET}
CORS_ORIGINS=https://myapp.com,https://www.myapp.com

# Monitoring
MONITORING_ENABLED=true
LOG_LEVEL=warn
LOG_FORMAT=json
LOG_DESTINATION=file

# Rate Limiting
RATE_LIMIT_MAX_REQUESTS=1000
RATE_LIMIT_WINDOW_MS=900000

// .env.test
NODE_ENV=test
DB_NAME=myapp_test
LOG_LEVEL=error
MONITORING_ENABLED=false

// Advanced dotenv setup
// config/env.js
const path = require('path');
const fs = require('fs');

function loadEnvironment() {
    const nodeEnv = process.env.NODE_ENV || 'development';
    
    // Load base .env file
    const basePath = path.resolve(process.cwd(), '.env');
    if (fs.existsSync(basePath)) {
        require('dotenv').config({ path: basePath });
    }
    
    // Load environment-specific .env file
    const envPath = path.resolve(process.cwd(), `.env.${nodeEnv}`);
    if (fs.existsSync(envPath)) {
        require('dotenv').config({ path: envPath, override: true });
    }
    
    // Load local overrides (git-ignored)
    const localPath = path.resolve(process.cwd(), '.env.local');
    if (fs.existsSync(localPath)) {
        require('dotenv').config({ path: localPath, override: true });
    }
    
    console.log(`📦 Environment loaded: ${nodeEnv}`);
}

module.exports = { loadEnvironment };
```

**4. Environment Variable Security:**
```javascript
// Security best practices for environment variables

// 1. Encryption for sensitive environment variables
const crypto = require('crypto');

class SecureEnvManager {
    constructor(encryptionKey) {
        this.algorithm = 'aes-256-gcm';
        this.key = crypto.scryptSync(encryptionKey, 'salt', 32);
    }
    
    // Encrypt sensitive values before storing
    encrypt(text) {
        const iv = crypto.randomBytes(16);
        const cipher = crypto.createCipher(this.algorithm, this.key);
        cipher.setAAD(Buffer.from('myapp', 'utf8'));
        
        let encrypted = cipher.update(text, 'utf8', 'hex');
        encrypted += cipher.final('hex');
        
        const authTag = cipher.getAuthTag();
        
        return {
            iv: iv.toString('hex'),
            encrypted,
            authTag: authTag.toString('hex')
        };
    }
    
    // Decrypt sensitive values when loading
    decrypt({ iv, encrypted, authTag }) {
        const decipher = crypto.createDecipher(this.algorithm, this.key);
        decipher.setAAD(Buffer.from('myapp', 'utf8'));
        decipher.setAuthTag(Buffer.from(authTag, 'hex'));
        
        let decrypted = decipher.update(encrypted, 'hex', 'utf8');
        decrypted += decipher.final('utf8');
        
        return decrypted;
    }
    
    // Load and decrypt environment variables
    loadSecureEnv() {
        const secureVars = [
            'DB_PASSWORD',
            'JWT_SECRET',
            'API_KEYS'
        ];
        
        secureVars.forEach(varName => {
            const encryptedValue = process.env[`${varName}_ENCRYPTED`];
            if (encryptedValue) {
                try {
                    const parsed = JSON.parse(encryptedValue);
                    process.env[varName] = this.decrypt(parsed);
                } catch (error) {
                    console.error(`Failed to decrypt ${varName}:`, error.message);
                }
            }
        });
    }
}

// 2. Environment variable validation and sanitization
class EnvValidator {
    static validate() {
        const validations = {
            PORT: {
                type: 'number',
                min: 1,
                max: 65535,
                required: false,
                default: 3000
            },
            DB_PASSWORD: {
                type: 'string',
                minLength: 8,
                required: true,
                pattern: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/
            },
            JWT_SECRET: {
                type: 'string',
                minLength: 32,
                required: true
            },
            EMAIL: {
                type: 'email',
                required: false
            },
            URL: {
                type: 'url',
                required: false
            }
        };
        
        Object.entries(validations).forEach(([key, rules]) => {
            const value = process.env[key];
            
            if (rules.required && !value) {
                throw new Error(`Required environment variable ${key} is missing`);
            }
            
            if (value && !this.validateValue(value, rules)) {
                throw new Error(`Environment variable ${key} is invalid`);
            }
        });
    }
    
    static validateValue(value, rules) {
        switch (rules.type) {
            case 'number':
                const num = parseInt(value);
                if (isNaN(num)) return false;
                if (rules.min !== undefined && num < rules.min) return false;
                if (rules.max !== undefined && num > rules.max) return false;
                return true;
                
            case 'string':
                if (rules.minLength && value.length < rules.minLength) return false;
                if (rules.maxLength && value.length > rules.maxLength) return false;
                if (rules.pattern && !rules.pattern.test(value)) return false;
                return true;
                
            case 'email':
                return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value);
                
            case 'url':
                try {
                    new URL(value);
                    return true;
                } catch {
                    return false;
                }
                
            default:
                return true;
        }
    }
}
```

**5. Docker and Kubernetes Integration:**
```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "${PORT:-3000}:3000"
    environment:
      - NODE_ENV=production
      - PORT=3000
      - DATABASE_URL=${DATABASE_URL}
      - REDIS_URL=${REDIS_URL}
      - JWT_SECRET=${JWT_SECRET}
    env_file:
      - .env
      - .env.production
    volumes:
      - ./logs:/app/logs

# Kubernetes ConfigMap and Secret
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  NODE_ENV: "production"
  PORT: "3000"
  LOG_LEVEL: "info"
  REDIS_URL: "redis://redis-service:6379"

---
apiVersion: v1
kind: Secret
metadata:
  name: app-secrets
type: Opaque
data:
  JWT_SECRET: <base64-encoded-secret>
  DB_PASSWORD: <base64-encoded-password>

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        ports:
        - containerPort: 3000
        envFrom:
        - configMapRef:
            name: app-config
        - secretRef:
            name: app-secrets
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: database-secret
              key: url
```

**How to Explain:**
"Environment variables are like having different sets of instructions for the same recipe depending on where you're cooking. When you're cooking at home (development), you might use different ingredients and tools than when you're cooking in a professional restaurant (production). Environment variables let your application adapt to different environments without changing the core code (recipe). They're also like keeping your secret ingredients in a safe place - you don't write your grandmother's secret spice blend on the recipe card that everyone can see (source code), but you keep it in a secure place (environment variables) that only trusted people can access."

---

This concludes the comprehensive theoretical explanations for the major Node.js interview questions. Each section now includes:

1. **Core Theory**: Fundamental concepts and principles
2. **Detailed Examples**: Practical implementations
3. **Best Practices**: Production-ready patterns
4. **Real-world Context**: When and why to use each approach
5. **Simple Analogies**: Easy-to-understand explanations for interviews

The document covers all the essential Node.js concepts that senior developers need to understand, from basic runtime concepts to advanced architectural patterns.