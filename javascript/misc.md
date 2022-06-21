# Miscellaneous

#### Pass by value vs. Pass by reference in JavaScript:

Primitives i.e. `Boolean`, `null`, `undefined`, `String`, and `Number` are passed by value.

```js
let a = 2;
let b = a;
b = 1;
console.log(a); // 2
console.log(b); // 1
```

`Array`, `Function`, and `Object` are passed by reference.  
Destructure them if you wanna pass by value.

```js
let a = [1, 2];
let b = a;
b.push(3);
console.log(a); // [1, 2, 3]
console.log(b); // [1, 2, 3]

let a = { 'a': 1, 'b': 2};
let b = { ...a };
b['c'] = 3;
console.log(a); // { 'a': 1, 'b': 2}
console.log(b); // { 'a': 1, 'b': 2, 'c': 3}
```

#### Import and export ES5 vs. ES6:

```
// ES5:
module.exports = <function>;
const <variable> = require('<path>');

module.exports = { <function> };
const { <function> } = require('<path>');

// ES6
export default <function>;
import <function> from '<path>';

export function <function>() { 
};
import { <function> } from '<path>';
```
