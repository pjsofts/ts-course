---
title: "Hello TypeScript"
description: "Setting up TypeScript"
---

# Hello TypeScript

## Anatomy of the project

Let’s consider a very simple TypeScript project that consists of only three files:
```python
package.json   # Package manifest
tsconfig.json  # TypeScript compiler settings
src/index.ts   # "the program"
```

package.json 
```json
{
  "name": "hello-ts",
  "license": "NOLICENSE",
  "devDependencies": {
    "typescript": "^4.3.2"
  },
  "scripts": {
    "dev": "tsc --watch --preserveWatchOutput"
  }
}
```

Note that…

+ We just have one dependency in our package.json: typescript.
+ We have a dev script (this is what runs when you invoke yarn dev-hello-ts from the project root)

<br>

+ It runs the TypeScript compiler in “watch” mode (watches for source changes, and rebuilds automatically).

<br>


The following is just about the simplest possible config file for the TS compiler:
tsconfig.json
```json
{
  "compilerOptions": {
    "outDir": "dist", // where to put the TS files
    "target": "ES3" // which level of JS support to target
  },
  "include": ["src"] // which files to compile
}
```

All of these things could be specified on the command line (e.g., tsc --outDir dist), but particularly as things get increasingly complicated, we’ll benefit a lot from having this config file:

<br>

Finally, we have a relatively simple and pointless TypeScript program. It does have **a few interesting things in it that should make changes to the "target" property in our tsconfig.json more obvious**:

+ Use of a built in Promise constructor (introduced in ES2015)
+ Use of async and await (introduced in ES2017)


Here is the original (TypeScript) source code that we aim to compile:

src/index.ts

```ts
/**
 * Create a promise that resolves after some time
 * @param n number of milliseconds before promise resolves
 */
function timeout(n: number) {
  return new Promise((res) => setTimeout(res, n))
}
 
/**
 * Add three numbers
 * @param a first number
 * @param b second
 */
export async function addNumbers(a: number, b: number) {
  await timeout(500)
  return a + b
}
 
//== Run the program ==//
;(async () => {
  console.log(await addNumbers(3, 4))
})()
```

## Running the compiler

```bash
npm run dev
```

You should see something in your terminal like:
```
12:01:57 PM - Starting compilation in watch mode...
```

## Changing target language level
If we go to hello-ts/tsconfig.json and change the “compilerOptions.target” property:

```diff
{
    "compilerOptions": {
        "outDir": "dist",
-       "target": "ES3"
+       "target": "ES2015"
    },
    "include": ["src"]
}
```


## Types of modules
Did you notice that the export keyword was still present in the build output for our program? We are generating ES2015 modules from our TypeScript source.

If you tried to run this file with node like this:
```
node packages/hello-ts/dist/index.js
```
There’s an error!

Node expects CommonJS modules 1, so we’ll have to tell TypeScript to output this kind of code.

Let’s add a new property to our tsconfig file:
```diff
"compilerOptions": {
    "outDir": "dist",
+   "module": "CommonJS",
```

Look at your packages/hello-ts/dist/index.js one more time now. You should see that the way the addNumbers function is exported has changed:

```js
exports.addNumbers = addNumbers
```

This is an indication that we’re emitting CommonJS modules! We could try running this program with node one more time:

```bash
node packages/hello-ts/dist/index.js
```

If the program works correctly at this point, we should see it pause for a short time and then print 7 to the console, before ending successfully.