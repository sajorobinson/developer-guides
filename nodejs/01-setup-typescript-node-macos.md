# Set up a TypeScript & Node.js environment on macOS (with NVM)

Want to write backend code with TypeScript and Node.js on macOS?

This guide walks you through:

1. Installing Node.js using NVM
2. Setting up TypeScript for local development
3. Compiling your first `.ts` file for backend or CLI use

---

## Series index

1. **Set up a TypeScript & Node.js environment on macOS (with NVM)** (you are here)
2. [Build a web service with TypeScript, Node.js, and Express](02-build-web-service-typescript-node-express.md)
3. [Design and generate an API with OpenAPI, TypeScript, and Express](03-design-generate-api-openapi-typescript-express.md)

---

## Overview

### What is Node.js?

[Node.js](https://nodejs.org) is an open-source, cross-platform runtime environment that lets you run JavaScript on the server, outside the browser. It's widely used to build web apps, APIs, command line tools, and automation scripts.

### What is TypeScript?

[TypeScript](https://www.typescriptlang.org) is a statically typed superset of JavaScript that adds optional type annotations and catches type-related bugs at compile time.

### Why use TypeScript with Node.js?

TypeScript helps catch bugs before runtime, improves editor support through autocompletion and type hints, and makes large codebases easier to read, refactor, and maintain.

## Prerequisites

- A Mac running **macOS** (any recent version; this guide was tested on **macOS Sequoia 15.5**)
- Access to the **Terminal**
- **curl** installed (pre-installed on most macOS systems)

> Note: No prior installation of Node.js is required. This guide will walk you through installation.

## 1: Install NVM (Node Version Manager)

NVM (Node Version Manager) is a tool that lets you manage multiple versions of Node.js.

### 1.1: Download nvm

In your Terminal, copy and paste the following to download and install nvm:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

### 1.2: Source nvm

Next, copy and paste the following to source the file.

```sh
\. "$HOME/.nvm/nvm.sh"
```

**Sourcing** a file means loading its contents into your current shell so you can immediately use whatever it defines (commands, functions, environment variables, etc.) without restarting the Terminal.

In the command above, `$HOME` is an environment variable that points to your home directory. So, the command above is the same as:

```sh
\. "/Users/your_user/.nvm/nvm.sh"
```

In other words, **run the file at this location in the current shell session**.

## 2: Install Node.js via NVM

Copy and paste the following to download the latest version of Node.js 22.x:

```sh
nvm install 22
```

Then, verify the installation:

| Command       | Expected result                        |
|---------------|----------------------------------------|
| `node -v`     | Should print something like `v22.18.0` |
| `nvm current` | Should print something like `v22.18.0` |
| `npm -v`      | Should print something like `10.9.3`   |

> Note: The version numbers may differ slightly depending on when you install.

## 3: Create a project directory

The next step is to create a project directory where you'll store your Node.js project and TypeScript code:

Assuming you want to put the project on your desktop:

```sh
mkdir ~/Desktop/my-ts-project
cd ~/Desktop/my-ts-project
```

## 4: Initialize a project

Once the project directory is created, you're ready to initialize the project with `npm` (Node Package Manager).

```sh
npm init -y
```

> Note: The `-y` flag means initialization automatically responds "yes" to any prompts posed by npm.

You should see a message like the following print to the terminal:

```sh
Wrote to /Users/sam/Desktop/my-ts-project/package.json:
{
  "name": "my-ts-project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

This means that npm successfully created your project with a single file: `package.json`.

## 5: Install TypeScript

Next, you need to install TypeScript as a development dependency:

```sh
npm install --save-dev typescript
```

### Why `--save-dev` instead of `--save`?

The `--save-dev` flag tells `npm` that TypeScript is only needed during development for compiling `.ts` files to `.js` files.

By contrast, running this (`--save` is the default behavior and is therefore not included):

```sh
npm install typescript
```

...would add TypeScript to your **runtime dependencies**, meaning it would be expected to be present when your project is executed. This isn't necessary, because your app will run the compiled JavaScript, not the TypeScript source.

## 6: Install `@types/node`

Next, you need to install the `@types/node` package, which allows the TypeScript compiler to understand the types for built-in Node features (`process`, `fs`, `http`, etc.)

```sh
npm install --save-dev @types/node
```

## 7: Create a TypeScript config file

The next step is to generate a basic `tsconfig.json` file.

### 7.1: Create the TypeScript config file

```sh
npx tsc --init
```

This command instructs the TypeScript compiler (`tsc`) to create a new `tsconfig.json` file in the root of the project. This file is responsible for:

- Defining how TypeScript should interpret and compile the code.
- Enabling features like type-checking, strict mode, module resolution, output paths, etc.

A `tsconfig.json` file is **required** for doing anything with TypeScript beyond a single-file setup.

> **See also**: [Intro to the TSConfig Reference (types)](https://www.typescriptlang.org/tsconfig/)

The `npx` command lets you run a command from a local or remote npm package. In this case, it's running the `tsc` command from the `typescript` package we installed previously.

Here are a few examples of what `tsconfig.json` configures:

| Setting              | Purpose                                                                        |
|----------------------|--------------------------------------------------------------------------------|
| `target`             | Which version of JavaScript to compile to.                                     |
| `module`             | How modules are handled (e.g. CommonJS for Node.js).                           |
| `rootDir` / `outDir` | How TypeScript looks for input files and where it outputs compiled JavaScript. |
| `strict`             | Enables strict type-checking rules.                                            |
| `esModuleInterop`    | Allows importing CommonJS modules using ES-style imports.                      |

### 7.2: Copy and paste a simplified configuration into the file

If you open the newly created `tsconfig.json` file, you'll likely see something like this:

```ts
{
  // Visit https://aka.ms/tsconfig to read more about this file
  "compilerOptions": {
    // File Layout
    // "rootDir": "./src",
    // "outDir": "./dist",

    // Environment Settings
    // See also https://aka.ms/tsconfig/module
    "module": "nodenext",
    "target": "esnext",
    "types": [],
    // For nodejs:
    // "lib": ["esnext"],
    // "types": ["node"],
    // and npm install -D @types/node

    // Other Outputs
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true,

    // Stricter Typechecking Options
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,

    // Style Options
    // "noImplicitReturns": true,
    // "noImplicitOverride": true,
    // "noUnusedLocals": true,
    // "noUnusedParameters": true,
    // "noFallthroughCasesInSwitch": true,
    // "noPropertyAccessFromIndexSignature": true,

    // Recommended Options
    "strict": true,
    "jsx": "react-jsx",
    "verbatimModuleSyntax": true,
    "isolatedModules": true,
    "noUncheckedSideEffectImports": true,
    "moduleDetection": "force",
    "skipLibCheck": true,
  }
}
```

This file includes many options for modern Node.js and React environments. We don't need most of these settings for backend development.

To make things simpler, copy/paste this configuration into `tsconfig.json`:

```ts
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "rootDir": "src",
    "outDir": "dist",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "sourceMap": true,
    "types": ["node"]
  }
}
```

| Option            | Description                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------------------|
| `target`          | Compiles your TypeScript to JavaScript syntax compatible with ECMAScript 2020 (ES11) and later environments.         |
| `module`          | Uses CommonJS modules, which is the standard module system Node.js understands by default.                           |
| `rootDir`         | The directory where your TypeScript source files live.                                                               |
| `outDir`          | The directory where the compiled JavaScript files are placed after building.                                         |
| `strict`          | Enabled strict type-checking options to help catch bugs early.                                                       |
| `esModuleInterop` | Enables compatibility for importing CommonJS modules with ES6-style `import` syntax.                                 |
| `skipLibCheck`    | Skips type checking of all `.d.ts` declaration files to speed up the build, usually safe for stable libs.            |
| `sourceMap`       | Generates .map files to help debuggers map compiled JavaScript back to the original TypeScript source lines.         |
| `types`           | Specifies global type definitions to include. Here, we're including Node.js built-in from the `@types/node` package. |

## 8: Write some TypeScript code

You're finally ready to write some TypeScript code.

First, make a `src` directory to store your TypeScript files:

```sh
mkdir src
```

In your newly-created `src` directory, create a file called `index.ts` and add the following:

```ts
const greet = (name: string): string => {
  return `Hello, ${name}!`;
};

console.log(greet("world"));
```

## 9: Compile the code

Run the following to compile the code:

```sh
npx tsc
```

This compiles your TypeScript into JavaScript in the `dist/` folder.

To run your code:

```sh
node dist/index.js`
```

You should see `Hello, world!` print to the terminal.

## Optional: Add a build script

To make the build process easier, you can add a custom script to `package.json` under `scripts`:

```json
{
  "name": "my-ts-project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "tsc"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@types/node": "^24.2.0"
  }
}
```

This maps the command `build` to the TypeScript compiler `tsc`. With this defined, you can compile with:

```sh
npm run build
```
