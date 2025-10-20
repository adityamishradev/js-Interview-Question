
# Node.js Interview Questions (20 Questions)

A quick guide to common Node.js interview questions with easy explanations and examples.

---

## 1. What is the event loop in Node.js and how does it work?

- Node.js is **single-threaded**. The **event loop** allows it to handle **asynchronous operations** without blocking the main thread.  
- It continuously checks the **event queue** and executes callbacks when operations complete.  

**Flow:**
1. Node executes synchronous code.
2. Async operations (like I/O, timers) go to **event queue**.
3. Event loop picks events and executes callbacks.
4. Process continues until queue is empty.

---

## 2. Difference between `process.nextTick()` and `setImmediate()`

| Feature               | `process.nextTick()`        | `setImmediate()`            |
|-----------------------|---------------------------|-----------------------------|
| Execution timing       | Runs **before** I/O events | Runs **after** I/O events   |
| Priority               | Higher priority            | Lower priority              |
| Use case               | Run ASAP after current operation | Run after current poll phase |

---

## 3. How does Node.js handle asynchronous I/O?

- Node uses **non-blocking I/O**.
- It offloads I/O tasks (file read, DB calls) to **libuv thread pool**.
- Callback/event is executed when operation completes.

---

## 4. What are streams and how do you use them?

- Streams = **data flows in chunks**, not all at once.
- Types:
  - **Readable** → read data (`fs.createReadStream`)
  - **Writable** → write data (`fs.createWriteStream`)
  - **Duplex** → read + write
  - **Transform** → modifies data while reading/writing
- Example:

```js
const fs = require('fs');
const readStream = fs.createReadStream('file.txt');
const writeStream = fs.createWriteStream('file-copy.txt');

readStream.pipe(writeStream);
```

---

## 5. How does clustering work in Node.js?

- Node is single-threaded → clustering allows **multiple instances** to run on **multiple CPU cores**.
- Example:

```js
const cluster = require('cluster');
const http = require('http');
const numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  for (let i = 0; i < numCPUs; i++) cluster.fork();
} else {
  http.createServer((req, res) => res.end('Hello')).listen(3000);
}
```

---

## 6. How’d you implement a caching layer in Node?

- Use **in-memory cache** (like `node-cache` or `Redis`) to store frequent data.
- Example with `node-cache`:

```js
const NodeCache = require('node-cache');
const cache = new NodeCache();

function getData(key, fetchFunc) {
  if (cache.has(key)) return cache.get(key);
  const data = fetchFunc();
  cache.set(key, data, 60); // cache 60 seconds
  return data;
}
```

---

## 7. How do you handle large file uploads efficiently?

- Use **streams** instead of reading entire file in memory.
- Use `busboy` or `multer` for multipart uploads.
- Example: stream file to disk or cloud directly.

---

## 8. What are child processes and how are they used?

- **Child processes** allow Node to run **external commands** or scripts.
- Types: `fork`, `exec`, `spawn`
- Example:

```js
const { spawn } = require('child_process');
const ls = spawn('ls', ['-lh']);

ls.stdout.on('data', (data) => console.log(`Output: ${data}`));
```

---

## 9. What is the role of buffers in Node.js?

- **Buffer** = stores **binary data** in memory.
- Used for streams, file I/O, network data.
- Example:

```js
const buf = Buffer.from('Hello');
console.log(buf.toString()); // "Hello"
```

---

## 10. How do you monitor and debug Node.js performance?

- Use **Node Inspector**, `console.time()`, `process.memoryUsage()`, or **profilers**.
- Tools: **PM2**, **Clinic.js**, **Node DevTools**

---

## 11. What are worker threads and when to use them?

- Worker threads allow **parallel execution** for CPU-heavy tasks.
- Used for **computationally expensive tasks**.
- Example:

```js
const { Worker } = require('worker_threads');
new Worker('./worker.js');
```

---

## 12. How’d you secure a Node.js application?

- Validate input (`express-validator`)  
- Use **Helmet** for headers  
- HTTPS  
- Authentication (JWT/OAuth)  
- Rate limiting  
- Avoid exposing sensitive info  

---

## 13. How do you manage environment variables securely?

- Use `.env` with `dotenv` package.  
- Do **not commit `.env`** to Git.  
- Example:

```js
require('dotenv').config();
const PORT = process.env.PORT || 3000;
```

---

## 14. Difference between CommonJS and ES modules

| Feature           | CommonJS             | ES Module          |
|------------------|-------------------|------------------|
| Syntax            | `require()` / `module.exports` | `import` / `export` |
| Loading           | Synchronous         | Asynchronous      |
| Node default      | ✅                 | Experimental in older versions |

---

## 15. How do you handle rate limiting in Node.js?

- Use `express-rate-limit` (or similar for plain Node HTTP)
- Example:

```js
const rateLimit = require('express-rate-limit');
const limiter = rateLimit({ windowMs: 15*60*1000, max: 100 });
app.use(limiter);
```

---

## 16. How do you implement JWT authentication in Node.js?

- Generate token on login:

```js
const jwt = require('jsonwebtoken');
const token = jwt.sign({ userId: user._id }, process.env.JWT_SECRET, { expiresIn: '1h' });
```

- Verify token on protected routes:

```js
jwt.verify(token, process.env.JWT_SECRET, (err, decoded) => { ... });
```

---

## 17. How does dependency injection work in Node?

- You **inject dependencies** into modules instead of importing inside.
- Helps in **testing and flexibility**.
- Example:

```js
function UserService(db) { this.db = db; }
const service = new UserService(databaseInstance);
```

---

## 18. What is the role of `fs/promises` in Node?

- Modern promise-based file system API.
- Avoids callback hell.
- Example:

```js
const fs = require('fs/promises');
async function readFile() {
  const data = await fs.readFile('file.txt', 'utf8');
  console.log(data);
}
```

---

## 19. How can you prevent memory leaks?

- Avoid global variables  
- Close DB connections  
- Use streams for large data  
- Use tools like **heapdump**, **Node Clinic**  

---

## 20. How would you implement logging and metrics?

- Logging: `winston`, `pino`  
- Metrics: `prom-client`, custom middleware  
- Example:

```js
const winston = require('winston');
const logger = winston.createLogger({ transports: [new winston.transports.Console()] });
logger.info("Server started");
```
