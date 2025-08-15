# DRAFT - Design and generate an API with OpenAPI, TypeScript, and Express

Want to design an API and auto-generate code to start development?

This guide walks you through:

1. The basics of an OpenAPI file.
2. Generating code stubs from an OpenAPI file.
3. Implementing a basic endpoint to show how an API is structured.

---

## Series index

1. [Set up a TypeScript and Node.js environment on macOS (with NVM)](01-setup-typescript-node-macos.md)
2. [Build a web service with TypeScript, Node.js, and Express](02-build-web-service-typescript-node-express.md)
3. **Design and generate an API with OpenAPI, TypeScript, and Express** (you are here)

---

## Overview

[OpenAPI](https://www.openapis.org) is a standard for describing RESTful APIs in a machine-readable format. It's a blueprint that defines every aspect of an API, including:

- Routes
- Parameters
- Responses
- Authentication methods
- And more

An OpenAPI file is like a contract between a client (a user of your API) and the server hosting your API.

### Why use OpenAPI?

Why exactly should you use an OpenAPI file?

- **Clarity**: Both humans and machines can understand your API.
- **Consistency**: Reduces guesswork across teams.
- **Automation**: Enables code generation, testing, and documentation.

We'll go into more depth about the automation benefits OpenAPI provides, but don't underestimate (especially if you're making enterprise software) the benefit of **clarity** and **consistency**. It's common for multiple teams to interact with an API. Your API might be used by:

- Developers
- QA
- Customer success
- Sales engineers
- Support

A single source of truth makes it easier for everyone to stay aligned.

### Docs-first API design

Instead of coding an API first and documenting it later, a **docs-first approach** starts with the OpenAPI spec. This ensures the API design is thoughtful, consistent, and reviewable before implementation.

We'll be taking a docs-first approach in this guide.

### Code generation

OpenAPI can generate code automatically for you:

- **Server stubs**: Skeleton backend code lets you focus on business logic rather than boilerplate. Any changes to your OpenAPI spec can be reflected automatically in the stubs.
- **SDKs/clients**: Ready-to-use code for consuming your API in multiple languages, which you can distribute to your users.

## 1: Create your OpenAPI file

We’ll start by creating a minimal `openapi.yaml` file in the root of your project:

```yaml
openapi: 3.0.3
info:
  title: Example Todo API
  version: 1.0.0
  description: An API for managing todo items.
servers:
  - url: http://localhost:3000
paths:
  /todos:
    get:
      summary: Get all todos
      operationId: getTodos
      responses:
        '200':
          description: A list of todo items.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Todo'
  /todos/{id}:
    get:
      summary: Get a single todo by ID
      operationId: getTodoById
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: A single todo item.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Todo'
        '404':
          description: Todo not found.
components:
  schemas:
    Todo:
      type: object
      properties:
        id:
          type: string
        title:
          type: string
        completed:
          type: boolean
```

This spec defines two endpoints:

- `GET /todos` — returns a list of todos.
- `GET /todos/{id}` — returns a single todo by its ID.

## 2: Install and configure `openapi-generator-cli`

We’ll use OpenAPI Generator to turn our YAML file into TypeScript + Express code.

Install globally via npm:

```sh
npm install @openapitools/openapi-generator-cli -g
```

Check to make sure it's installed:

```sh
openapi-generator-cli version
```

## 3: Generate the server stub and SDK

From the project root:

```sh
openapi-generator-cli generate \
  -i openapi.yaml \
  -g typescript-express-server \
  -o generated/server
```

This tells the generator:

- `-i` — the input OpenAPI file.
- `-g` — the generator type (typescript-express-server).
- `-o` — the output directory.

Inside generated/server, you’ll now have:

- `controllers/` — empty methods for each operation.
- `routes.ts` — Express route wiring.
- `models/` — TypeScript interfaces from your schemas.

## 4: Implement an endpoint

Open `controllers/TodosController.ts` and find `getTodos`:

```ts
export function getTodos(req: Request, res: Response): void {
  res.status(200).send([
    { id: '1', title: 'Write OpenAPI guide', completed: false },
    { id: '2', title: 'Implement example endpoint', completed: true },
  ]);
}
```

The generated types ensure your response matches the Todo schema in the OpenAPI spec.

## 5: Test the endpoint

Run the server:

```sh
npm start
```

Test with `curl`:

```sh
curl http://localhost:3000/todos
```

Expected output:

```json
[
  { "id": "1", "title": "Write OpenAPI guide", "completed": false },
  { "id": "2", "title": "Implement example endpoint", "completed": true }
]
```

## 6: What’s now possible?

At this point you have:

- An OpenAPI spec describing your API.
- Auto-generated server stubs with TypeScript typing.
- A working Express endpoint.

From here you can:

- Connect to a database.
- Add authentication.
- Expand the spec with more endpoints.
- Generate SDKs for different languages (-g typescript-fetch, -g python, etc.).
