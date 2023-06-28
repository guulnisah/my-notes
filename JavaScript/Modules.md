# Modules

The goal of modular programming is to allow large programs to be assembled using modules of code from disparate authors and sources and for all of that code to run correctly even in the presence of code that the various module authors did not anticipate.

# Modules in ES6

ES6 adds `import` and `export` keywords to JavaScript.

Each file is its own module, and constants, variables, functions, and classes defined within a file are private to that module unless they are explicitly exported. Values that are exported from one module are available for use in modules that explicitly import them. 

Code inside an ES6 module is automatically in strict mode. It means that code in modules cannot use the `with` statement or the `arguments` object or undeclared variables. In modules, `this` is undefined even in top-level code. (By contrast, scripts in web browsers and Node set `this` to the global object.)

## ES6 Exports

The `export` keyword can only appear at the top level of your JavaScript code. You may not export a value from within a class, function, loop, or conditional. (This is an important feature of the ES6 module system and enables static analysis: a modules export will be the same on every run, and the symbols exported can be determined before the module is actually run.)

### Regular export

If you have several value to export.

```jsx
export const PI = Math.PI;
export function degreesToRadians(d) { return d * PI / 180; }
```

Or just use this single export expression. 

```jsx
export { degreesToRadians, PI };
```

Can only be done declarations that have a name.

### Default export

If you have a module that only exports one value.

```jsx
export default class BitSet {
// implementation omitted
}
```

Default exports are easier to import. 

Default exports can be done on anonymous function expressions and anonymous class expressions. 

A module can have only ONE default export. 

## ES6 Imports

Importing values that were exported.

The simplest form of import is used for modules that define a `default export`:

```jsx
import BitSet from './bitset.js';
```

1. `import` keyword
2. an identifier
3. `from` keyword
4. a string literal that names the module whose default export we are importing
5. the default export value of the specified module becomes the value of the specified identifier in the current module

imports can only appear at the top level of a module and are *not allowed within classes, functions, loops, or conditionals*

imports are **“hoisted”** to the top, and all imported values are available for any of the module’s code runs

The module from which a value is imported is specified as a constant string literal in single quotes or double quotes. (You may not use a variable or other expression whose value is a string, and you may not use a string within backticks).

In web browsers, this string is interpreted as a URL relative to the location of the module that is doing the importing.

A module specifier string must be an absolute path starting with “/”, or a relative path starting with “./” or “../”, or a complete URL a with protocol and hostname. 

For now, **“bare module specifiers”** are not allowed. If you want to import a module from the same directory as the current one, simply place `“./”` before the module name and import from `“./util.js”` instead of `“util.js”`.

To import values from a module that exports multiple values:

```jsx
import { mean, stddev } from "./stats.js";
```

Non-default exports of a module have names in the exporting module, and when we import those values, we refer to them by those names.

When importing from a module that defines many exports you can easily import everything with an import statement like this:

```jsx
import * as stats from "./stats.js";
```

An import statement like this creates an object and assigns it to a constant named `stats`. All imports become a property of `stats`. Names of these exports are used as property names within the `stats` object.

Modules typically define either one default export or multiple named exports. It is legal, but **uncommon**, for a module to use both export and export default. Don’t use both, basically. But you can do it like this. 

```jsx
import Histogram, { mean, stddev } from "./histogramstats.js";
```

To include a no-exports module into your program, simply use the import keyword with the module specifier:

```jsx
import "./analytics.js";
```

A module like this runs the first time it is imported.

## Imports and Exports with Renaming

If you have the same name used in several modules or that name is already used in the file that you’re importing to, you have to rename them with the `as` keyword.

```jsx
import { render as renderImage } from "./imageutils.js";
import { render as renderUI } from "./ui.js";

// had two function called render in two separate modules
// renamed them 
```

Can do this with exports as well

```jsx
export {
layout as calculateLayout,
render as renderLayout
};
```

`as` expects as single identifier. This is wrong:

```jsx
export { Math.sin as sin, Math.cos as cos }; // SyntaxError
```

## Re-exports \\\\

## JavaScript Modules on the Web \\

Even though bundling tools may still be desirable in production, they are no longer required in development since all current browsers 1 provide native support for JavaScript modules.

If you want to natively use import in a web browser, you must tell the web browser that your code is a module by using a `<script type="module">` tag.

Scripts with the type="module" attribute are loaded and executed like scripts with the defer attribute. Loading of the code begins as soon as the HTML parser encounters the `<script>` tag (in the case of modules, this code-loading step may be a recursive process that loads multiple JavaScript files). But code execution does not begin **until HTML parsing is complete**. And once HTML parsing is complete, *scripts (both modular and non) are executed in the order in which they appear in the HTML document*.

## Dynamic Imports with import()

You pass a module specifier to `import()` and it returns a Promise object that represents the asynchronous process of loading and running the specified module. 

When the dynamic import is complete, the Promise is “fulfilled” and produces an object like the one you would get with the import * as form of the static import statement.

```jsx
import("./stats.js").then(stats => {
let average = stats.mean(data);
})

// or simplify it with async await
async analyzeData(data) {
let stats = await import("./stats.js");
return {
average: stats.mean(data),
stddev: stats.stddev(data)
};
}
```

The argument to import() should be a module specifier, exactly like one you’d use with a static import directive.

Dynamic import() looks like a function invocation, but it actually is not. Instead, import() is an operator and the parentheses are a required part of the operator syntax.

## import.meta.url \\

The primary use case of import.meta.url is to be able to refer to images, data files, or other resources that are stored in the same directory as (or relative to) the module. 

The URL() constructor makes it easy to resolve a relative URL against an absolute URL like import.meta.url. 

Suppose, for example, that you have written a module that includes strings that need to be localized and that the localization files are stored in an l10n/ directory, which is in the same directory as the module itself. Your module could load its strings using a URL created with a function, like this:

```jsx
function localStringsURL(locale) {
return new URL(l10n/${locale}.json, import.meta.url);
}
```

# Summary

The goal of modularity is to allow programmers to hide the implementation details of their code so that chunks of code from various sources can be assembled into large programs without worrying that one chunk will overwrite functions or variables of another. This chapter has explained three different JavaScript module systems: 

In the early days of JavaScript, modularity could only be achieved through the clever use of immediately invoked function expressions. Node added its own module system on top of the JavaScript language. 

Node modules are imported with require() and define their exports by setting properties of the Exports object, or by setting the module.exports property. 

In ES6, JavaScript finally got its own module system with import and export keywords, and ES2020 is adding support for dynamic imports with import().