Tags: #js 
Created: 2023-09-20 10:09
References: https://bun.sh https://bun.sh/blog/bun-v1.0

# Bun
Bun's goal is to *eliminate slowness and complexity without throwing away everything that's great about [[JavaScript]]*. Bun makes a lot of tools from the JS ecosystem unnecessary:

**NodeJS**
Bun is a drop-in replacement for NodeJS; main cool things that are no longer required are:
- You no longer need the `donenv` dependency, because Bun can read `.env` files by default
- You no longer need `nodemon`, because Bun has built-in watch mode
- Many more things that Bun has built-in support for (such as `npx` - `bunx` is 5x faster)

**Transpilers**
Bun can directly run JSX and TS files, therefore it can replace `tsc` and `babel`.

**Bundlers**
Bun can generate a JS bundle by itself, which means that you will no longer need things like `webpack`.

**Package managers**
Bun is a package manager that is npm-compatible: it reads the `package.json` file and writes to the `node_modules` folder.

**Testing libraries**
Bun also comes with a test runner that is compatible with Jest.

While all these tools are good in their own right, using them together inevitably creates fragility and a slow developer experience, because together all the tools perform a lot of redundant work: for example, when you run `jest`, your code is parsed 3+ times (once by each of the tools). If you combine that with all the extra plugins and adapters that are required to make everything together, you get a lot of fragility.

## Bun features
Here's a list of things that Bun can do:
### JS Runtime
Bun is a JavaScript runtime, which aims to be a drop-in replacement for [[NodeJS]], which means that existing Node applications and npm packages *just work* in Bun. While **perfect** compatibility with Node is not possible, Bun can run virtually any NodeJS application.

### Speed
Bun is fast, starting up to 4x faster than Node. This difference is only magnified when running a [[TypeScript]] file, which requires transpilation (which Bun can also do) before it can be run by Node.

### TypeScript and [[JSX]] support
Bun has a JavaScript transpiler baked into the runtime, which means that you can run JS, TS and even JSX/TSX files without using any additional dependencies.

### [[ESM]] and [[CommonJS]] compatibility
The transition from CommonJS to ES modules has been really slow: it took NodeJS 5 years to support it without using the `--experimental-modules` flag. Regardless, the ecosystem is still full of CommonJS.

Bun supports both module systems and there is no need to worry about file extensions (`.js` vs `.cjs` vs `.mjs`), or including `"type": "module"` in your `package.json`. It's even possible to combine `import` and `require` in the same file.

### Web APIs
Bun has built-in support for the Web standard APIs that are available in browsers, such as `fetch`, `Request`, `Response`, `WebSocket` and `ReadableStream`.

### Hot reloading
You can run Bun with the `--hot` flag to enable hot reloading. Unlike tools that hard-restart the whole process, like `nodemon`, Bun reloads code without terminating the old process, which means that [[HTTP]] and [[WebSocket]] connections don't disconnect and state isn't lost.

### Plugins
Bun is highly customizable - you can define plugins that intercept imports and perform custom loading logic, for example. 

## Bun APIs
Bun ships with highly-optimized, standard-library APIs doe this things you need most as the developer (even though Bun is compatible with all the NodeJS APIs, these are faster).

