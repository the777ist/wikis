# UTILS

#### Async operations on array avoiding looping over elements:
```js
async function operation(element) {
    // some operations
} 

async function asyncOperations(allElements) {
    const asyncRowsCalc = [];
    allElements.forEach(item => asyncRowsCalc.push(operation(item)));
    await Promise.all(asyncRowsCalc);
}

await asyncOperations([<ARRAY>]);
```
    
#### Chunked async operations:
Chunk a large array into smaller sub-arrays.  
Asynchronously perform operations on sub-array elements.  
After completion, move over to next sub-array
```js
async function operation(element) {
    // some operations
}

(function test(allItems) {
    const batchChunk = 20;
    const oprQueueBatch = [];
    const oprQueue = [];

    // chunk allItems into smaller sub-arrays
    for (let index = 0; index < allItems.length; index += batchChunk) {
        oprQueueBatch.push(allItems.slice(index, index + batchChunk));
    }

    // await operations on each subarray and loop over
    for (let index = 0; index < oprQueueBatch.length - 1; index += 1) {
        await oprBatch(oprQueueBatch[index]);
    }

    // create an array of promises for each item in sub-array, resolve when all async operations are complete
    const oprBatch = async times => {
        times.forEach(item => oprQueue.push(opr(item)));
        await Promise.all(oprQueue);
    }

    // perform async operation for each item of sub-array
    const opr = async item => await operation(item);
})([<ARRAY>]);
```

#### Hash Table alternative to nested loop:  
I have 2 arrays (arr1 and arr2), each containing 5000+ objects.  
I want to return an array having only those objects that are common to both arrays
```js
let arr1 = [
    { a: 'foo', b: 'bar' },
    { a: 'foo', b: 'baz' }
    // 5000+ more elements
];

let arr2 = [
    { a: 'baz', b: 'foo' },
    { a: 'foo', b: 'baz' }
    // 5000+ more elements
];

let hashTable = {}
let result = [];

// BAD - nested loop - O(n^2)
(function test() {
    arr1.forEach(key1 => arr2.forEach(key2 => {
        if (key1.a === key2.a && key1.b === key2.b) {
            result.push(key2)
        }
    }));

    console.log(result);
})();

// GOOD - hash table - O(n)
(function test() {
    arr1.forEach(item => {
        hashTable[`${item.a}-${item.b}`] = (hashTable[`${item.a}-${item.b}`] || 0) + 1;
    });

    arr2.forEach(item => {
        if (hashTable.hasOwnProperty(`${item.a}-${item.b}`)) {
            result.push(item);
        }
    });

    console.log(result);
})();
```

#### Find top-level parent of each array element in a hierarchy:
```js
let arr1 = [
    {
        id: 1,
        parentId: null,
        name: 'foo'
    },
    {
        id: 2,
        parentId: 1,
        name: 'bar'
    },
    {
        id: 3,
        parentId: 2,
        name: 'baz'
    },
    {
        id: 4,
        parentId: 3,
        name: 'fizz'
    },
];

let result = [];

async function asyncFindOrgUnit(unit, origUnit) {
    const parentUnit = result.find(item => unit.parentId === item.id);

    if (parentUnit) asyncFindOrgUnit(parentUnit, origUnit);
    else {
        const unitIndex = result.indexOf(result.find(item => item.id === origUnit.id));
        result[unitIndex]['topParent'] = unit.id;
    }
}

async function findTopLevelOrgUnit(units) {
    const asyncCalls = [];

    result = [...units];
    result.forEach(item => asyncCalls.push(asyncFindOrgUnit(item, item)));
    await Promise.all(asyncCalls);
}

(async function test() {
    await findTopLevelOrgUnit(arr1);
    console.log(result)
})();
```

#### List to tree:
```js
function listToTree(list) {
    const map = {};
    const roots = [];

    for (let i = 0; i < list.length; i += 1) {
        map[list[i].id] = i; // initialize map
        list[i].children = []; // initialize children
    }

    for (let i = 0; i < list.length; i += 1) {
        const node = list[i];
        if (node.parentId in map) {
            list[map[node.parentId]].children.push(node);
            list[map[node.parentId]].children = _.sortBy(list[map[node.parentId]].children,
                [child => child.name.toLowerCase(),]);
        } else {
            roots.push(node);
        }
    }
    return roots;
}
```

#### Check if a variable is an array or object:
```js
function isArray(a) {
    return (!!a) && (a.constructor === Array);
};

function isObject(a) {
    return (!!a) && (a.constructor === Object);
};
```

#### Check for leap year:
```js
function leapYear(year) {
    return ((year % 4 == 0) && (year % 100 != 0)) || (year % 400 == 0);
}
```
