# UTILS

Async operations on array avoiding looping over elements:

    async function operation(element) {
        // some operations
    }

    async function asyncOperations(allElements) {
        const asyncRowsCalc = [];
        allElements.forEach(item => asyncRowsCalc.push(operation(item)));
        await Promise.all(asyncRowsCalc);
    }

    await asyncOperations([<ARRAY>]);

Safe JSON Stringify:

    function safeJSONStringify(obj) {
        const replaceErrors = (key, value) => {
            if (value instanceof Error) {
                const error = {};
                Object.getOwnPropertyNames(value).forEach((key) => {
                    error[key] = value[key];
                });
                return error;
            }
            return value;
        };
        return JSON.stringify(obj, replaceErrors);
    }

Safe JSON Parse:

    function safeJSONParse(str) {
        let data;
        try {
            data = JSON.parse(str);
        } catch (e) {
            data = undefined;
        }
        return data;
    }

Return hash value for supplied string:

    stringHash(str) {
        let hash = 5381;
        let i = str.length;
        while (i) {
            hash = (hash * 33) ^ str.charCodeAt(--i);
        }
        // unsigned bitshift
        return hash >>> 0;
    }

Is string a valid UUID:

    static isUUID4(str) {
        const uuidPattern = '^[a-fA-F0-9]{8}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{12}$';
        return (new RegExp(uuidPattern)).test(str);
    }

Shuffle array in place:

    static shuffleArray(a) {
        for (let i = a.length; i; i--) {
            const j = Math.floor(Math.random() * i);
            [a[i - 1], a[j],] = [a[j], a[i - 1],];
        }
    }

List to tree:

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

Find top-level parent of each child node of a tree asynchronously:

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

Check if a variable is an array or object:

    function isArray(a) {
        return (!!a) && (a.constructor === Array);
    };

    function isObject(a) {
        return (!!a) && (a.constructor === Object);
    };

Calculating dates based on diffs using moment:

    function findDailyParameters(date, diff) {
        const current = moment(date)
            .startOf('day')
            .add(-diff, 'minute')
            .format(timeFormat);
        const last = moment(date)
            .add(-1, 'day');

        return {
            'currentStart': current,
            'currentEnd': moment(date).add(1, 'day')
                .startOf('day')
                .add(-diff, 'minute')
                .format(timeFormat),
            'lastStart': moment(last)
                .startOf('day')
                .add(-diff, 'minute')
                .format(timeFormat),
            'lastEnd': moment(last).add(1, 'day')
                .startOf('day')
                .add(-diff, 'minute')
                .format(timeFormat),
            'timeType': ['hour', 'day', 'date', 'hour',],
            'currentDisplay': moment(date)
                .format(dayDisplayFormat),
            'lastDisplay': last.format(dayDisplayFormat),
            'diffIndex': 0,
            'dateFormat': 'YYYY-MM-DD HH:00:00',
            'max': 24,
        };
    };

    function findMonthlyParameters(date, diff) {
        const startOfMonth = moment(date)
            .startOf('month');
            
        return {
            'currentStart': startOfMonth.add(-diff, 'minute')
                .format(timeFormat),
            'currentEnd': moment(date).add(1, 'month')
                .startOf('month')
                .add(-diff, 'minute')
                .format(timeFormat),
            'lastStart': moment(date)
                .add(-1, 'month')
                .startOf('month')
                .add(-diff, 'minute')
                .format(timeFormat),
            'lastEnd': startOfMonth.add(-diff, 'minute')
                .format(timeFormat),
            'timeType': ['day', 'month', 'month', 'date',],
            'currentDisplay': moment(date)
                .format('MMMM YYYY'),
            'lastDisplay': moment(date)
                .add(-1, 'month')
                .format('MMMM YYYY'),
            'diffIndex': 1,
            'dateFormat': 'YYYY-MM-DD 00:00:00',
            'max': 31,
        };
    };

    function findYearlyParameters(date, diff) {
        const startOfYear = moment(date)
            .startOf('year');

        return {
            'currentStart': startOfYear.add(-diff, 'minute')
                .format(timeFormat),
            'currentEnd': moment(date).add(1, 'year')
                .startOf('year')
                .add(-diff, 'minute')
                .format(timeFormat),
            'lastStart': moment(date)
                .add(-1, 'year')
                .startOf('year')
                .add(-diff, 'minute')
                .format(timeFormat),
            'lastEnd': startOfYear.add(-diff, 'minute')
                .format(timeFormat),
            'timeType': ['month', 'year', 'year', 'month',],
            'currentDisplay': moment(date)
                .format('YYYY'),
            'lastDisplay': moment(date)
                .add(-1, 'year')
                .format('YYYY'),
            'diffIndex': 1,
            'dateFormat': 'YYYY-MM-01 00:00:00',
            'max': 12,
        };
    };

    function getDaysBetween(startDate, endDate) {
        let now = startDate.clone(), dates = [];

        while (now.isSameOrBefore(endDate)) {
            dates.push(now.format('YYYY-MM-DD 00:00:00'));
            now.add(1, 'days');
        }

        return dates;
    }

Check for leap year:

    function leapYear(year) {
        return ((year % 4 == 0) && (year % 100 != 0)) || (year % 400 == 0);
    }