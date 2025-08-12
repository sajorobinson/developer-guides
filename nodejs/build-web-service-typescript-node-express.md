
# Build a web service with TypeScript, Node.js, and Express

Want to build a web service with TypeScript, Node.js, and Express?

**Before you begin**, make sure you've completed the setup described in the previous guide: [Set up a TypeScript & Node.js environment on macOS (with NVM)](setup-typescript-node-macos.md)

This guide walks you through:

1. Installing Express via npm
2. Defining your first endpoint
3. Running your web service and testing your endpoint

## Overview

### What is Express?

[Express](https://expressjs.com) is a fast, unopinionated, minimalist web framework for Node.js. It provides a simple way to build web servers, REST APIs, and other networked applications using JavaScript or TypeScript.

Express is essentially a thin layer on top of Node’s built-in `http` module. It gives you helpful tools and patterns to:

- Define routes (like `/`, `/api/users`, etc.)
- Handle HTTP methods (GET, POST, PUT, DELETE)
- Send responses (JSON, HTML, files, etc.)
- Process incoming data (like JSON in a request body)
- Use middleware for logging, validation, authentication, and more

### Why Use Express?

Express simplifies development by wrapping Node's lower-level `http` module, reducing the need for repetitive and boilerplate code.

Let’s look at the difference between some code **without Express** and **with Express**.

#### Without Express

```ts
import http from "http";

const server = http.createServer((req, res) => {
  if (req.url === "/" && req.method === "GET") {
    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("Hello, world!");
  }
});

server.listen(3000);
```

#### With Express

```ts
import express from "express";

const app = express();

app.get("/", (_req, res) => {
  res.send("Hello, world!");
});

app.listen(3000);
```

#### Comparison

| Task                | Node.js (`http`)                          | Express                               |
|---------------------|-------------------------------------------|----------------------------------------|
| Create server       | Manually with `http.createServer()`       | Handled by `express()`                 |
| Define routes       | Check `req.url` and `req.method`          | Use `app.get()`, `app.post()`, etc.    |
| Set headers         | Manually with `res.writeHead()`           | Often handled automatically            |
| Send response       | Use `res.end()`                           | Use `res.send()`, `res.json()`, etc.   |
| Parse JSON requests | Write your own logic                      | Use `app.use(express.json())`          |
| Middleware support  | Write your own logic                      | Built-in and extensible                |

## Prerequisites

You’ll need Node.js, npm, and TypeScript installed on your system. See: [Set up a TypeScript & Node.js environment on macOS (with NVM)](setup-typescript-node-macos.md)

## 1: Install Express

Run these commands in the root of your project directory.

- The first command installs Express
- The second command installs TypeScript support for Express.

```sh
npm install express
npm install --save-dev @types/express
```

## 2: Define Your First Route

Create a new file named `src/server.ts`:

```ts
import express from "express";

const app = express();
const port = 3000;

app.get("/", (_req, res) => {
  res.send("Hello from your web service!");
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```

## 3: Compile & Run

```sh
npm run build
node dist/server.js
```

Visit http://localhost:3000.

You should see:

```
Hello from your web service!
```

## 4: Optional quality of life improvements

These tools improve the developer experience but aren't required for production.

### 4.1: Auto-reloading with `nodemon`

```sh
npm install --save-dev nodemon
```

Add a dev script to your `package.json`:

```json
"scripts": {
  "build": "tsc",
  "dev": "nodemon dist/server.js"
}
```

Then run:

```sh
npm run dev
```

### 4.2: Run TypeScript directly with `ts-node`

```sh
npm install --save-dev ts-node
```

Add a start script to your `package.json`:

```json
"scripts": {
  "start": "ts-node src/server.ts"
}
```

Then run:

```sh
npm start
```
