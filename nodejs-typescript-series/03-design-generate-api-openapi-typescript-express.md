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

- Start from a template
- Explain structure and maintainability choices

## 2: Install and configure `openapi-generator-cli`

- Installation via npm
- Minimal config for our needs

## 3: Generate the server stub and SDK

- Run generation command
- Review the generated structure and files

## 4: Implement an endpoint

- Add real logic to one generated route
- Show how the generated interfaces help with type safety

## 5: Test the endpoint

- Use Postman (or cURL) to send requests and verify responses

## 6: Recap what’s now possible

- Next steps (connecting data sources, adding more endpoints)
