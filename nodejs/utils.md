# UTILS

#### Async operations on array avoiding looping over elements:

    async function operation(element) {
        // some operations
    }

    async function asyncOperations(allElements) {
        const asyncRowsCalc = [];
        allElements.forEach(item => asyncRowsCalc.push(operation(item)));
        await Promise.all(asyncRowsCalc);
    }

    await asyncOperations([<ARRAY>]);
    
#### Hash Table alternative to nested loop:  
I have 2 arrays (arr1 and arr2), each containing 5000+ objects.  
I want to return an array having only those objects that are common to both arrays

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
        arr1.forEach(key1 => {
            if (hashTable.hasOwnProperty(`${key1.a}-${key1.b}`)) {
                hashTable[`${key1.a}-${key1.b}`] += 1; 
            } else {
                hashTable[`${key1.a}-${key1.b}`] = 1; 
            }
        });

        arr2.forEach(key2 => {
            if (hashTable.hasOwnProperty(`${key2.a}-${key2.b}`)) {
                result.push(key2);
            }
        });

        console.log(result);
    })();

#### Return hash value for supplied string:

    stringHash(str) {
        let hash = 5381;
        let i = str.length;
        while (i) {
            hash = (hash * 33) ^ str.charCodeAt(--i);
        }
        // unsigned bitshift
        return hash >>> 0;
    }

#### Is string a valid UUID:

    static isUUID4(str) {
        const uuidPattern = '^[a-fA-F0-9]{8}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{12}$';
        return (new RegExp(uuidPattern)).test(str);
    }

#### List to tree:

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

#### Find top-level parent of each child node of a tree asynchronously:

    let allUnits = [];

    async function asyncFindOrgUnit(unit, origUnit) {
        const parentUnit = allUnits.find(item => unit.parentId === item.id);

        if (parentUnit) asyncFindOrgUnit(parentUnit, origUnit);
        else {
            const unitIndex = allUnits.indexOf(allUnits.find(item => item.id === origUnit.id));
            allUnits[unitIndex]['parentOrg'] = unit.id;
            return;
        }
    }

    async function findTopLevelOrgUnit(units) {
        allUnits = [...units];
        const asyncCalls = [];
        allUnits.forEach(item => asyncCalls.push(asyncFindOrgUnit(item, item)));
        await Promise.all(asyncCalls);
        console.log(allUnits);
    }

    await findTopLevelOrgUnit(<array of 8000 objects>)

#### Check if a variable is an array or object:

    function isArray(a) {
        return (!!a) && (a.constructor === Array);
    };

    function isObject(a) {
        return (!!a) && (a.constructor === Object);
    };

#### Check for leap year:

    function leapYear(year) {
        return ((year % 4 == 0) && (year % 100 != 0)) || (year % 400 == 0);
    }
