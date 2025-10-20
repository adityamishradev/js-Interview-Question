
# Express.js Interview Questions (20 Questions)

A quick guide to common Express.js interview questions with easy explanations and examples.

---

## 1. Explain middleware and the types of middleware in Express.

**Middleware** is a function that runs between the request and response in an Express app.  
It can modify the request or response, check something, or run some code before sending the final response.

**In short:**  
`Middleware = Function that works between request and response.`

| Type               | Example                     | Purpose                       |
|-------------------|-----------------------------|-------------------------------|
| Application-level  | `app.use()`                 | Run for every request         |
| Router-level       | `router.use()`              | Used inside routes            |
| Built-in           | `express.json()`            | Provided by Express           |
| Third-party        | `cors`, `morgan`            | Extra functionality           |
| Error-handling     | `app.use(err, req, res, next)` | Catch errors                |

---

## 2. How does request and response flow in Express?

When a user sends a request:

```
Request → Middleware → Route → Response → User
```

---

## 3. What is the use of `next()` in Express?

`next()` is a function used inside middleware.  
It tells Express: `"Go to the next middleware or route handler."`

- **Without `next()`** → request stops in that middleware  
- **With `next()`** → request moves forward to next middleware or route

---

## 4. How do you implement global error handling?

Global error handling catches all errors in one place.

```js
app.use((err, req, res, next) => {
  console.error(err.stack); // print error in console
  res.status(500).json({
    success: false,
    message: err.message || "Something went wrong!"
  });
});
```

---

## 5. How do you structure a scalable Express application?

```
project/
│
├─ controllers/
│   └─ userController.js
├─ models/
│   └─ userModel.js
├─ routes/
│   └─ userRoutes.js
├─ middleware/
│   └─ auth.js
├─ config/
│   └─ db.js
├─ app.js
└─ server.js
```

---

## 6. How do you secure an Express API (rate limiting, headers)?

Use: **Rate limiting, secure headers, CORS, input validation, authentication**

**Rate Limiting** (prevent DDOS):

```js
const rateLimit = require("express-rate-limit");
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // max 100 requests per 15 min
});
app.use(limiter);
```

**Secure Headers** (protect from attacks):

```js
const helmet = require("helmet");
app.use(helmet());
```

Other tips:  
- **CORS** → control which domains can access your API  
- **Input Validation** → prevent bad data  
- **Authentication** → JWT, OAuth, sessions  
- **HTTPS** → always use secure connection

---

## 7. Explain route parameter vs. query parameter.

- **Route params** → in URL path (required)  
- **Query params** → after `?` (optional)

---

## 8. What’s the role of CORS in Express and how to configure it?

**CORS = Cross-Origin Resource Sharing**  
Decides which websites can use your API. Use `cors` package in Express.

---

## 9. How do you handle file uploads?

Use **multer**:

```js
const multer = require("multer");
const upload = multer({ dest: "uploads/" });
app.post("/upload", upload.single("file"), (req, res) => {
  res.send("File uploaded!");
});
```

---

## 10. How would you implement logging in Express?

- Use **morgan**: logs requests  
- Or write custom middleware with `console.log()`  

---

## 11. What is the use of express-validator?

`express-validator` validates user input (like form data).  
Ensures data is correct and safe before saving to database.

---

## 12. How do you prevent SQL/NoSQL injection in Express?

- Always validate user input  
- Use safe queries or libraries like **Mongoose** or **Sequelize**  

---

## 13. What is Helmet and how does it help with security?

`Helmet` is middleware that sets secure HTTP headers to make Express apps safer.

---

## 14. How do you handle role-based authorization in Express?

- Give access to routes based on user roles  
- Use middleware to check user role before allowing access

---

## 15. What are virtual routes and when are they useful?

- **Virtual route** = dynamic route, not a real file  
- Can handle many URLs with one code

```js
app.get("/user/:id", (req, res) => {
  const userId = req.params.id;
  res.send(`User ID is ${userId}`);
});
```

---

## 16. Difference between synchronous and asynchronous middleware?

- **Synchronous** → runs one by one, waits for completion  
- **Asynchronous** → can do async tasks without stopping server

---

## 17. How do you optimize performance in Express apps?

- Use compression  
- Cache data  
- Serve static files quickly  
- Use async code  
- Keep middleware simple

---

## 18. What is a proxy in Express and how to set it?

- **Proxy** = middleman between client & server  
- Set in Express:  
```js
app.set('trust proxy', true)
```
- **Uses**: get real client IP, add security, load balancing, forward requests

---

## 19. What is the use of `app.locals` and `res.locals`?

**1️⃣ app.locals** → global data for the whole app

```js
app.locals.siteName = "My Cool App";
app.get("/", (req, res) => {
  res.send("Welcome to " + app.locals.siteName);
});
```

**2️⃣ res.locals** → data for current request only

```js
app.use((req, res, next) => {
  res.locals.user = { name: "Aditya" };
  next();
});

app.get("/", (req, res) => {
  res.send("Hello " + res.locals.user.name);
});
```

---

## 20. How can you implement request tracing in Express?

- Give each request a **unique ID** in middleware  
- Log it to track requests for debugging and performance
