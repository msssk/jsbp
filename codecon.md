# Best Practices - JavaScript Programming

Adhering to best practices while developing applications leads to code that is more maintainable, extensible, and testable. Understanding the rationale behind each best practice enables the programmer to make good judgment calls in situations where best practices are not clearly defined, or when it may be appropriate to deviate from best practices.

* New objects: use `{}` instead of `new Object()`
	* Correct:
```javascript
```
	* Incorrect:
<pre>
</pre>
* New arrays: use `[]` instead of `new Array()`
* AJAX: keep it asynchronous
* Minimize DOM access
* Maintain persistent references to DOM objects or calculated values
* Avoid deeply nested functions
* Optimize some loops
* Cache collection lengths (e.g. array.length, NodeList.length) when iterating
* Minimize in-loop operations (e.g. variable declarations)
* Minimize reflows and repaints
* Perform CSS operations in batch when possible
* Put script elements at bottom of body
* Use `DocumentFragment` when it makes sense
* Be specific with CSS queries
* Use event delegation when it makes sense

modularity

* Use local variables; minimize usage of globals
* Loosely couple where it makes sense

misc

* Filter `for...in` statements with `hasOwnProperty`
* Use parentheses for IIFEs
* Avoid using `with`
* Don't modify native object prototypes (e.g. `Array`, `Object`)
* Use feature detection instead of browser detection


## Coding Conventions

Coding conventions generally relate to the non-functional aspects of code that nonetheless affect maintainability, such as indenting and spacing. Code that is easier to understand is easier to troubleshoot and extend. Some coding conventions have been defined to improve code correctness by avoiding mistakes that the language's design does not sufficiently guard against.

Each organization should adopt and enforce a set of coding conventions for indentation, spacing, and syntax. You can define your own or adopt publicly available standards. SitePen's coding conventions are documented on [GitHub](https://github.com/SitePen/.jshintrc).

Syntactic conventions can be checked by linting tools like [JSHint](http://jshint.com/). Some source control systems, such as [Git](http://git-scm.com), provide commit hooks that can check code with a linting tool and reject non-conforming code.

Consistent spacing and syntax reduce the cognitive overhead of parsing programming language symbols and free the reader's mind to focus on understanding the concepts and flow communicated by the code.

JavaScript has a few deficiencies that can be guarded against by using and enforcing coding conventions:

* **Implicit globals**: assigning a value to an undeclared variable will automatically create a global variable and assign the value to it.
	* **Suggestion**: declare all variables at the top of the scope and disallow implicit globals. Declaring variables at the top of the scope ensure that any assignment happens to the local reference.
* **Type coercion**: JavaScript's C-like comparison operators (`==`, `!=`) will coerce the types of its operands to the same type. The rules JavaScript follows for the coercion can be complicated and few programmers know them all.
	* **Suggestion**: use strict comparison operators (`===` and `!==`) since the behavior is easier to understand and remember, and in most well-designed code is the desirable behavior anyway.
* **Automatic Semicolon Insertion** (ASI): the language syntax does not require lines to end with a semicolon, however, the rules for automatic semicolon insertion are not always intuitive.
	* **Suggestion**: it is safest to always use a semicolon and not rely on ASI.
 
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

Maintaining a clean separation of concerns contributes to readability, modularity, testability, extensibility, and reusability. In combination with modularity this greatly facilitates a layered approach to application architecture, where at the lowest level modules handle specific functionality like validating data or updating a small portion of the UI, and at progressively higher levels of abstraction modules are combined to handle processing data input, providing screens or pages of the UI, and at the top, loading the application and managing the entire application flow.

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

