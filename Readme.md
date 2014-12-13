Execute a callback on every node of a source code's AST and stop walking whenever you see fit.

*A variation of [substack/node-detective](https://github.com/substack/node-detective)
and simplification of [substack/node-falafel](https://github.com/substack/node-falafel).*

`npm install node-source-walk`

### Usage

```javascript
  var Walker = require('node-source-walk');

  var walker = new Walker();

  // Assume src is the string contents of myfile.js

  walker.walk(src, function (node) {
    // Example: looking for the use of define()
    var callee = node.callee;

    if (callee &&
        node.type === 'CallExpression' &&
        callee.type === 'Identifier' &&
        callee.name === 'define') {
          console.log('AMD syntax');

          // No need to keep traversing since we found
          // what we wanted
          walker.stopWalking();
        }
  });

```

By default, Walker will use `acorn` supporting ES5, but you can switch to es6 as follows:

```js
var walker = new Walker({
  ecmaVersion: 6
});
```

* The supplied options are passed through to the parser, so you can configure it according
to acorn's documentation: https://github.com/marijnh/acorn

### Public Members

`walk(src, cb)`

* Generates and recursively walks through the AST for `src` and executes `cb`
on every node.

`stopWalking()`

* Halts further walking of the AST until another manual call of `walk`.
* This is super-beneficial when dealing with large source files

`traverse(node, cb)`

* Allows you to traverse an AST node and execute a callback on it
* Callback should expect the first argument to be an AST node, similar to `walk`'s callback.
