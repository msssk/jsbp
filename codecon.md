# Best Practices - JavaScript Programming

Adhering to best practices while developing applications leads to code that is more maintainable, extensible, and testable. Understanding the rationale behind each best practice enables the programmer to make good judgment calls in situations where best practices are not clearly defined, or when it may be appropriate to deviate from best practices.

## Guidelines and Rules

### Syntax and Formatting

* Use single quotes for strings (more convenient to type; don't require escaping double-quotes)
* Use camel-casing for naming (e.g. `myMethod` for a function or method name, `MyClass` for a constructor name)
* Start constructor names with a capital letter
* Use a consistent brace style
* Terminate lines with a single semicolon when appropriate; do not rely on Automatic Semicolon Insertion (ASI)
* Avoid stray semicolons (only one semicolon at the end of lines)
* Define and adhere to a maximum line length (120 is reasonable for many development environments)
* Eliminate trailing whitespace
* Use consistent indentation
* Use consistent whitespace between keywords, functions, and operators
* Pad infix operators (+, -, etc.) with spaces on either side
* Ensure that `return`, `throw`, and `case` are always followed by a space
* Ensure that unary operators (e.g. `typeof`, `new`) are followed by a space
* Always enclose bodies of conditional statements in braces
* Only quote property names when necessary (if the property name contains special characters)
```javascript
// Correct
var data = {
	prop1: 'value',
	prop2: 5,
	'special-character-key': 'value'
};

// Incorrect
var data = {
	'prop1': 'value',
	'prop2': 5
};
```
* Do not use multi-line strings
* Do not use octal escapes (deprecated in ES5; use Unicode or hex)
* Do not introduce whitespace between a function name and the parentheses that invoke it (reduces clarity)
```javascript
// Correct
foo();

// Incorrect
foo ();
foo
();
```
* Do not wrap function calls with unnecessary parentheses
```javascript
// Correct
foo();

// Incorrect
(foo());
```
* Wrap IIFEs (Immediately Invoked Function Expressions) with parentheses
```javascript
// Correct
(function () {
	/* .... */
}());

// Incorrect
function () {
	/* .... */
}();
```
* Use a consistent variable name as an alias for `this` (recommended: `self`). Aliasing `this` is useful when calling functions that lose context.
* Do not reuse variable names as label names
* Do not use trailing underscores in variable names
* Specify the value being tested first and the value it is being compared against second in comparisons
```javascript
// Correct
if (color === 'red') {
	/* ... */
}

// Incorrect
if (red === 'color') {
	/* ... */
}
```
* Prefer dot notation when possible. If you know the name of a property, use dot notation to access it. Only use bracket notation when the property name is calculated at run-time.
* Always use parentheses when invoking constructor functions
```javascript
// Correct
var foo = new Foo();

// Incorrect
var foo = new Foo;
```


### Avoiding Pitfalls and Improving Correctness

* Define variables and functions before using them (improves clarity; reduces pitfalls related to implicit globals and hoisting)
* Declare all variables at the top of the scope and avoid implicit globals. Declaring variables at the top of the scope ensures that any assignment happens to the local reference.
* Use strict comparison operators (`===` and `!==` instead of `==` and `!=`) since the behavior is easier to understand and remember, and in most well-designed code is the desirable behavior anyway. (The rules JavaScript follows for type coercion can be complicated and few programmers know them all.)
* Don't use repeated spaces in regular expressions (counting them is error-prone)
```javascript
// Correct
var regex = /foo {3}bar/;

// Incorrect
var regex = /foo   bar/;
```
* Do not use trailing commas (behavior is inconsistent across browsers)
```javascript
// Correct
var data = {
	foo: 1,
	bar: 2
};
var data = [
	1,
	2
];

// Incorrect
var data = {
	foo: 1,
	bar: 2,
};
var data = [
	1,
	2,
];
```
* Keep cyclomatic complexity less than 10. Cyclomatic complexity is a measure of the number of possible independent paths through code. Conditional statements introduce additional paths that may be followed at execution time. High cyclomatic complexity leads to code that is difficult to understand and error-prone. It can generally be avoided by further decomposing the code into submodules. JSHint can analyze the cyclomatic complexity of your code.
* Do not overwrite declared functions (leads to confusing code)
```javascript
var foo = function () { /* ... */ };
// Incorrect: 'foo' is already defined!
foo = function () { /* ... */ };
```
* Do not reuse variable names in the parameter list to a `catch` clause (in IE <9 the `catch` clause does not create its own scope)
```javascript
var err = 'x';
try {
	throw 'problem';
}
// Incorrect: 'err' has already been defined
catch (err) {
	// As expected, 'err' is 'problem', not 'x'
}
// Unexpected: 'err' is 'problem', not 'x'!
```
* Do not assign a value to the exception parameter in a `catch` block
```javascript
try {
	/* ... */
}
catch (error) {
	// Incorrect: 'error' should be treated as read-only
	error = 5;
}
```
* Do not overwrite native objects (any object provided by the runtime environment, e.g. 'Array', 'Object', 'Math', etc.)
* Do not modify native objects (custom functionality should be provided in self-contained libraries)
* Do not redeclare variables (can be confusing)
* Do not shadow variables (can be confusing)
```javascript
var foo = 5;
function () {
	// Incorrect: shadows 'foo' from outer scope
	var foo = 2;
}
```
* Do not shadow restricted names (do not use 'undefined', 'arguments', 'eval', 'NaN', 'Infinity', etc. as variable names)
* Do not perform assignment in conditional expressions (reduces clarity, can be error-prone)
```javascript
// Correct
var isAllowed = foo();
if (isAllowed) {
	/* .... */
}

// Incorrect
var isAllowed;
if (isAllowed = foo()) {
	/* ... */
}
```
* Do not use the global value `NaN` with comparison operators: use the `isNaN` function (comparisons with `NaN` do not work)
* Do not use nested ternary operators (creates potentially confusing code)
* Only use `label` to mark the start of loops or switches
* Avoid fallthrough behavior of switch statements
* Do not use ES5 strict mode globally (it may break 3rd-party libraries, such as Dojo)
* Do not invoke the global objects `Math` and `JSON` as functions (use their methods)
* Avoid deeply nested functions (nesting beyond about 4 levels deep becomes difficult to understand and reason about)
* Filter `for...in` statements with `hasOwnProperty` if you only want properties on the object itself, not properties on all objects in the prototype chain as well
* Avoid using `with` (`with` blocks are error-prone and can easily lead to accidentally clobbering variables)
* Avoid using global variables; pass references to local scopes as needed
* Use feature detection instead of browser detection (feature detection is less complicated and future-proof)


### Objects

* New objects: use `{}` instead of `new Object()` (provides consistent syntax and behavior)
```javascript
// Correct
var data = {};
var data = {
	prop1: 'value'
};

// Incorrect:
var data = new Object();
```
* New arrays: use `[]` instead of `new Array()` (provides consistent syntax and behavior)
```javascript
// Correct
var data = [];
var data = [1, 2, 3];

// Incorrect
var data = new Array();
var data = new Array(1, 2, 3);
```
* Avoid duplicate keys in object literals:
```javascript
var data = {
	srcUrl: 'http://company.com',
	// Valid JavaScript, but bad practice:
	srcUrl: 'http://abc.company.com'
};
```
* Do not use the non-standard `__proto__` property


### Numbers

* Always include leading and trailing digits in decimal values
```javascript
// Correct
var num = 0.5;
var num = 2.0;
var num = -0.7;

// Incorrect
var num = .5;
var num = 2.;
var num = -.7;
```
* Always pass the radix parameter to `parseInt` (prior to ES5 `parseInt` would auto-detect the radix with potentially surprising results)


### Performance

#### JavaScript

* Do not use the `caller` and `callee` properties of `arguments` (they are deprecated and prevent some code optimizations from being performed by the JavaScript engine)
* Do not declare functions within loops (bad for performance)
* Do not declare variables within loops (bad for performance)
* Do not delete object properties (can hinder code optimization; set to `null` instead)
* AJAX: keep it asynchronous (do not specify `true` for the `async` parameter to `XMLHttpRequest#open()`)
* Avoid unnecessary recalculation: maintain persistent references to calculated values
* Loops that have a lot of execution time should be hand-optimized (use `for` instead of array iteration methods like `forEach`)
* Cache collection lengths (e.g. array.length, NodeList.length) when iterating
* Do not use the `eval` function
* Always pass a function reference to `setTimeout` and `setInterval` (string values use `eval` to create a function)

#### Browser: DOM/CSS

* Minimize DOM access: reference data in JavaScript variables when possible (do not use the DOM as a data store)
* Avoid repeated DOM access: maintain persistent references to DOM objects when useful
* Minimize reflows and repaints
* Put script elements at the bottom of the body element
* Use `DocumentFragment` when it makes sense
* Use event delegation when it makes sense
* Be specific with CSS queries, but only as specific as necessary
```css
/* If both match the same elements, the first selector is more performant: */
.elementClass {}
.containerClass .listClass .elementClass {}

/* If both match the same elements, the first selector is less error-prone: */
.containerClass .li {}
.containerClass * {}
```
* Perform CSS operations in batch when possible


### Clean, Legible Coding

* Keep source code free of unused variables (they introduce clutter and reduce clarity)
* Do not commit unreachable code
* Do not commit code with `debugger` statements
* Do not create constructors that are used for side-effects
* Do not create constructors that are simply wrappers/adapters




## Coding Conventions

Coding conventions generally relate to the non-functional aspects of code that nonetheless affect maintainability, such as indenting and spacing. Code that is easier to understand is easier to troubleshoot and extend. Some coding conventions have been defined to improve code correctness by avoiding mistakes that the language's design does not sufficiently guard against.

Each organization should adopt and enforce a set of coding conventions for indentation, spacing, and syntax. You can define your own or adopt publicly available standards. SitePen's coding conventions are documented on [GitHub](https://github.com/SitePen/.jshintrc).

Syntactic conventions can be checked by linting tools like [JSHint](http://jshint.com/). Some source control systems, such as [Git](http://git-scm.com), provide commit hooks that can check code with a linting tool and reject non-conforming code.

Consistent spacing and syntax reduce the cognitive overhead of parsing programming language symbols and free the reader's mind to focus on understanding the concepts and flow communicated by the code.


## Documentation

Source code should be composed with readability in mind, using meaningful names and modular code organization. Function names should be verbs, constructor and variable names should generally be nouns, and boolean variables should be descriptively prefixed (e.g. `isEnabled`, not `enabled`; `hasTouch`, not `touch`).

In addition to writing readable code, inline code comments should be provided in a format that can be processed by a formatted document generation tool like [jsdoc](http://usejsdoc.org/). While code comments are important, they should only be used to communicate information that is not already communicated by the code itself.

## Readability, Maintainability, Extensibility, Testability

### Keep It Simple

Keep each function or module simple and focused. It's easier to comprehend 50 lines of code than 500 lines of code. Writing code that is easy to understand facilitates readability, which in turn facilitates maintainability:

* Easier to understand is easier write tests for
* Easier to understand is easier to extend
* Easier to understand is easier to troubleshoot and debug

### Separation of Concerns

Organize code that handles separate concerns into separate functions or modules. Design each module to handle a specific set of concerns and avoid introducing code that performs external functionality. For example, calculating a list of values and updating the UI to display the values are separate concerns, so the code that handles these should be provided by separate modules. A module at a higher level of abstraction can manage references to each module and pass the output from the calculation process to the input of the UI updating process.

Maintaining a clean separation of concerns contributes to readability, modularity, testability, extensibility, and reusability. This greatly facilitates a layered approach to application architecture, where at the lowest level modules handle specific functionality like validating data or updating a small portion of the UI, and at progressively higher levels of abstraction modules are combined to handle processing data input, providing screens or pages of the UI, and at the top, loading the application and managing the entire application flow.

An important aspect of separation of concerns is loose coupling. Loose coupling is depending on the shape of a thing, not the specific name and location of a thing. If module A depends directly on module B, module A is somewhat tightly coupled to module B, i.e. module A cannot work without module B. When possible, modules should be designed to work with any object that supports an interface, the public methods and properties, of an object.

The following example, using AMD, demonstrates tight coupling where `moduleA` is specifically dependent on `moduleB`:
<pre>
// moduleA.js
define([
	'moduleB'
], function (moduleB) {
	return {
		myMethod: function () {
			return moduleB.doSomething();
		}
	};
});
</pre>

In many cases this specific dependency is appropriate or even necessary. However, when possible and reasonable, modules should be designed with a looser dependency as demonstrated below:

<pre>
// moduleB.js
define([
	// ... dependencies
], function () {
	return {
		doSomething: function () {
			// ... method implementation
		}
	};
});

// moduleA.js
define([
	// ... dependencies
], function () {
	return {
		myMethod: function (moduleB) {
			return moduleB.doSomething();
		}
	};
});

// controller.js
require([
	'moduleA',
	'moduleB'
], function (moduleA, moduleB) {
	moduleA.myMethod(moduleB);
});
</pre>

`moduleA` no longer has a direct dependency on `moduleB` - it will work with anything passed to it that provides the expected API. Elevating dependency management to higher levels of the application architecture (in this case, to `controller.js`) facilitates modular composition and automated testing. Dependency mocking becomes easier when dependencies are passed to the module at run-time, rather than specifically defined in the module's dependency list.

### Modularity

Organize code in modules that handle specific functionality, and organize modules in a hierarchy that facilitates exploration and discovery by new developers. For example, if an application has a variety of data models it needs to represent and interact with, the modules could go in a `data` directory; UI widgets could go in a `widgets` directory.

As the JavaScript language does not yet include any system for defining or loading modules, the [Asynchronous Module Definition](https://github.com/amdjs/amdjs-api/blob/master/AMD.md) (AMD) API is a good solution for modular JavaScript.

By following these principles you can create robust and reusable code. Reuse your own code where applicable, and also avoid reinventing the wheel when possible - there are numerous JavaScript libraries to handle a wide array of functionality.

## Performance

Premature performance optimization can hinder readability, correctness, and maintainability. However, in JavaScript there are some approaches that are preferable to similar approaches primarily for performance reasons, and do not introduce unnecessary complexity.

