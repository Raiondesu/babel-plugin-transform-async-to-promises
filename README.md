babel-plugin-transform-async-to-promises
========================================

Babel plugin to transform `async` functions containing `await` expressions to the equivalent chain of `Promise` calls with use of minimal helper functions.

[![Build Status](https://travis-ci.org/rpetrich/babel-plugin-transform-async-to-promises.svg?branch=master)](https://travis-ci.org/rpetrich/babel-plugin-transform-async-to-promises)

### Input:

```javascript
async function fetchAsObjectURL(url) {
    const response = await fetch(url);
    const blob = await response.blob();
    return URL.createObjectURL(blob);
}
```

### Output:

```javascript
var fetchAsObjectURL = _async(function(url) {
	return _await(fetch(url), function(response) {
		return _await(response.blob(), function(blob) {
			return URL.createObjectURL(blob);
		});
	});
});
```

## JavaScript Language Features

### Full Support
- `async`/`await`
- `for`/`while`/`do` loops (including loops that would exhaust stack if dispatched recursively)
- `switch` statements (including fallthrough and `default` cases)
- conditional expressions
- logical expressions
- `try`/`catch`
- `break`/`continue` statements (on both loops and labeled statements)
- `throw` expressions
- Function hoisting
- Variable hoisting
- Arrow functions
- Methods
- `arguments`
- `this`
- Proper member dereference order of operations

### Partial Support
- Standards-compliant event loop ordering: error path inside `catch`/`finally` clauses are always dispatched asynchronously
- `Function.length`: `async` functions will often return a length of 0 (when the `_async` wrapper is used)

### No support
- `eval`: impossible to support without deep hooks into the runtime
- Async generator functions: not implemented or planned
- `Function.name`: rewrite pass removes function name instrumentation
- `new AsyncFunction(...)`: impossible to support without shipping babel and the plugin in the output
