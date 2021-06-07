# FUNCTIONAL PROGRAMMING:

#### Side effects:

A function or expression is said to have a side effect if it modifies some state variable value(s) outside its local environment, that is to say has an observable effect besides returning a value (the main effect) to the invoker of the operation.  
Pure functions don't have side effects.  

*Not a pure function:*
```js
let name = 'John';

function greet() {
    return `Hi I'm ${name}`
}
```

*Pure function:*
```js
function greet(name) {
    return `Hi I'm ${name}`
}
```

#### Higher Order Functions(HOF):

A function that can take another function as an input or return another function as an output.
```js
function makeAdjectifier(adjective) {
    return function (string) {
        return adjective + ' ' + string;
    };
}

var coolifier = makeAdjectifier('cool');
coolifier('conference');

// OUTPUT: “cool conference”
```
