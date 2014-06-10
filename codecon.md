# Best Practices - JavaScript Programming

Adhering to best practices while developing applications leads to code that is more maintainable, extensible, and testable. Understanding the rationale behind each best practice enables the programmer to make good judgment calls in situations where best practices are not clearly defined, or when it may be appropriate to deviate from best practices.

## Guidelines and Rules

Some of the rules below include example code with some examples labelled 'Correct' and others labelled 'Incorrect'. In most cases the 'Incorrect' examples are valid JavaScript, but should be considered poor practice as they are error-prone or bad for readability. A linting tool should be used and configured to raise warnings if the incorrect forms are encountered.

### Avoiding Pitfalls and Improving Correctness

* Avoid using global variables; pass references to local scopes as needed
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
	bar: 2, // <-- trailing comma
};
var data = [
	1,
	2, // <-- trailing comma
];
```
* Always place an opening brace on the same line as the associated statement
```javascript
// Correct
if (condition) {
	/* ... */
}

return {
	get: function () {
		/* ... */
	}
};

// Incorrect
if (condition)
{
	/* ... */
}

// In this case the return statement will not work as expected
// The returned value will be 'undefined' due to Automatic Semicolon Insertion
return
{
	get: function ()
	{
		/* ... */
	}
};
```
* Keep cyclomatic complexity less than 10. Cyclomatic complexity is a measure of the number of possible independent paths through code. Conditional statements introduce additional paths that may be followed at execution time. High cyclomatic complexity leads to code that is difficult to understand and error-prone. It can generally be avoided by further decomposing the code into submodules. JSHint can analyze the cyclomatic complexity of your code.
* Do not overwrite declared functions (leads to confusing code)
```javascript
function foo () { /* ... */ };
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
* Do not use multi-line strings: confusing (newline is not part of string); inconsistent browser support; poor editor support for indentation
```javascript
// Correct
var myString = 'a multi line string' +
	' using the concatenation operator' +
	' to avoid excessive line length';

var myString = 'a multi line string\n' +
	'with escaped newlines embedded\n' +
	'in the string';

// Incorrect
var myString = 'a multi line\
	string can be created\
	with backslashes';
```
* Do not use the global value `NaN` with comparison operators: use the `isNaN` function (comparisons with `NaN` always evaluate to false)
* Always pass the radix parameter to `parseInt` (prior to ES5 `parseInt` would auto-detect the radix with potentially surprising results)
* Do not use nested ternary operators (creates potentially confusing code)
* Avoid fallthrough behavior of switch statements
* Do not use ES5 strict mode globally (it may break 3rd-party libraries, such as Dojo)
* Avoid deeply nested functions (nesting beyond about 4 levels deep becomes difficult to understand and reason about)
* Filter `for...in` statements with `hasOwnProperty` if you only want properties on the object itself, not properties on all objects in the prototype chain as well
* Avoid using `with` (`with` blocks are error-prone and can easily lead to accidentally clobbering variables)
* Use feature detection instead of browser detection (feature detection is more reliable and future-proof)


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


### Performance

#### JavaScript

* Do not use the `caller` and `callee` properties of `arguments` (they are deprecated and prevent some code optimizations from being performed by the JavaScript engine)
* Do not delete object properties (can hinder code optimization; set to `undefined` instead)
* AJAX: keep it asynchronous (do not specify `false` for the `async` parameter to `XMLHttpRequest#open()`)
* Avoid unnecessary recalculation: maintain persistent references to calculated values
* Loops that have a lot of execution time should be hand-optimized (use `for` instead of array iteration methods like `forEach`)
* Cache collection lengths (e.g. array.length, NodeList.length) when iterating
* Do not use the `eval` function
* Always pass a function reference to `setTimeout` and `setInterval` (string values use `eval` to create a function)

#### Web Browsers - DOM and CSS

* Minimize DOM access: reference data in JavaScript variables when possible (do not use the DOM as a data store)
* Avoid repeated DOM queries: maintain persistent references to DOM objects and query results when useful
* Minimize the number of DOM nodes: fewer nodes are faster to render
* Minimize reflows and repaints
	* Avoid inline styles when possible: use CSS classes to vary between different layouts (each individual inline style assignment can trigger DOM updates; a group of style changes can be applied in a single DOM update by changing the CSS class of an element)
	* Change CSS classes as low in the DOM tree as possible. To change the display of an element, prefer changing the CSS class of the element directly instead of changing the CSS class of a parent element.
	* Minimize the number of CSS rules and remove unused rules
	* Remove animated elements from the document flow. When an element is animated it may trigger reflow of surrounding elements. If the element's position is independent of surrounding elements, use absolute or fixed positioning to remove the element from the document flow so that it can more efficiently be animated.
	* Avoid HTML tables for layout (or set `table-layout` to `fixed`). The layout of cells in non-fixed tables may be affected by cells lower down in the table, so the rendering process is slower.
* Put script elements at the bottom of the body element to prevent blocking rendering of the HTML
* Use [`DocumentFragment`](https://developer.mozilla.org/en-US/docs/Web/API/document.createDocumentFragment) when it makes sense
* Use event delegation when it makes sense
* Be specific with CSS selectors, but only as specific as necessary. Besides being less performant, overly specific selectors hinder maintenance and customization.
```css
/* If both match the same elements, prefer the first one: */
.elementClass {}
.containerClass .listClass .elementClass {}

/* If both match the same elements, prefer the first one: */
.containerClass li {}
.containerClass ul.widgetClass li {}
```


### Clean, Legible Coding

* Keep source code free of unused variables
* Do not commit unreachable code
* Do not commit code with `debugger` statements
* Do not create constructors that are used for side-effects (use functions to make it clear an action is being performed)
* Do not create constructors that are simply wrappers/adapters (use regular functions rather than constructors to decorate, wrap, or adapt an object)
```javascript
// This is correct if 'Draggable' is a constructor that creates and returns a new object
var draggableWidget = new Draggable(myWidget);

// If 'Draggable' just augments an object
// it should be implemented as a function
makeDraggable(myWidget);
```


### Syntax and Formatting

* Use single quotes for strings (more convenient to type; don't require escaping double-quotes)
* Use camel-casing for naming (e.g. `myMethod` for a function or method name, `MyClass` for a constructor name)
* Start constructor names with a capital letter
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
* Do not use octal escapes (e.g. '\251') in JavaScript strings: they are deprecated in ES5; use Unicode ('\uXXX') or hex ('\xXX')
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
* Wrap IIFEs (Immediately Invoked Function Expressions) with parentheses (for clarity)
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
```javascript
function MyWidgetConstructor () { /* ... */ }

MyWidgetConstructor.prototype = {
	render: function () {
		var self = this;
		var people = [
			{ firstName: 'Bill', lastName: 'West' },
			{ firstName: 'John', lastName: 'Smith' }
		];

		people.forEach(function (person) {
			// The anonymous function passed to 'forEach' has global
			// context: `this` will be the global object ('window')
			// Using the 'self' alias we can access the '_formatName' method
			self._formatName(person);
		});
	},

	_formatName: function (itemData) {
		return person.firstName + ' ' + person.lastName;
	}
};
```
* Do not use trailing underscores in variable names
* Specify the value being tested first and the value it is being compared against second in comparisons
```javascript
// Correct
if (color === 'red') {
	/* ... */
}

// Incorrect
if ('red' === color) {
	/* ... */
}
```
* Prefer dot notation when possible. If you know the name of a property, use dot notation to access it. Only use bracket notation when the property name contains special characters or is calculated at run-time.
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

* Always use parentheses when invoking constructor functions
```javascript
// Correct
var foo = new Foo();

// Incorrect
var foo = new Foo;
```


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

### Loop optimization

In general you should write code that is most convenient and legible. Depending on your target environment, `Array#forEach` may be a suitable default, or a similar method from a library, such as `dojo/_base/array#forEach`. In "hot" loops which are executed frequently or on large amounts of data it may be prudent to optimize the loop. Optimization varies across environments, so you should pick your worst-performing target environment and optimize and test there first. There are a few primary and simple optimizations that improve loop performance in most cases:

* Minimize the work done within the loop body: anything that does not need to be within the loop should be moved outside, such as variable and function declarations
* Use a `for` or `while` loop instead of `forEach`
* Cache the `length` value of the array (or loop in reverse)
```javascript
var data = [ /* ... */ ];
// Cache the length value
var length = data.length;
var i;

for (i = 0; i < length; i++) {
	/* ... */
}

// For the fastest looping, simplify the loop condition:
var i;
for (i = data.length; i--;) {
	/* ... */
}

// similar approach with 'while':
var i = data.length;
while (i--) {
	/* ... */
}
```

### Script loading

In browsers that do not [support](http://caniuse.com/#feat=script-async) the `async` attribute on `script` elements, the loading of `script` elements will block parallel downloads and delay the page rendering until the script has loaded and executed. If the scripts do not need to be processed prior to rendering the DOM (and in most cases, it is quite the opposite - scripts depend on the DOM being rendered first) the `script` tags should be placed at the bottom of the `body` element. This both enables the page to begin displaying useful information to the user faster and ensure that the DOM is ready by the time the scripts are loaded. For older browsers that do not support `async`, [`defer`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script#attr-defer) functions similarly ([compatibility table](http://caniuse.com/#feat=script-defer)). If you must support browsers that do not support `async`, placing the `script` elements at the bottom of the body is more reliable than using `defer`.

### DOM modifications, repaint, and reflow

Accessing the browser DOM from JavaScript is a relatively slow process and should be avoided when possible. DOM updates are of course an integral part of web applications, but when doing so one should consider the impact and strive to minimize repaints (re-rendering portions of the UI) and reflow (recalculating the position of elements within the document flow).

The provided guidelines help to speed up the rendering process and minimize repaints and reflows.

### Event delegation

The simplest and most common approach to handling DOM events is to register a handler with the element that will emit events. However, as the number of elements grows, the number of handlers to register (and unregister!) grows as well, which can negatively impact performance. In the case of lists or grids, where a large number of homogenous (or similar) child elements can all be handled by the same function event delegation should be used. The handler function is registered with the parent element, which allows a single handler to be registered and handle events for any number of child elements. Another benefit is that the handler will be called for dynamically added child elements as well. Event delegation with `dojo/on` is discussed in the [reference guide](http://dojotoolkit.org/reference-guide/dojo/on.html#event-delegation).
