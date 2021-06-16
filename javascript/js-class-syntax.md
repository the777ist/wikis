# JAVASCRIPT CLASS SYNTAX

#### Creating a class with instance method and class method:
```js
class Student {
    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName; 
    }

    getName() {
        return `${this.firstName} ${this.lastName}`;
    }

    static GetAllNames(students) {
        return students.map(student => `${student.firstName} ${student.lastName}`)
    }
}

let student1 = new Student('John', 'Doe');
let student2 = new Student('Jane', 'Doe');

student1.getName();
student2.getName();
Student.GetAllNames([student1, student2]);
```

#### Adding an instance method later on:
```js
let student3 = new Student('Michael', 'Corleone');

student3.getQuote = function () {
    return `${this.firstName} ${this.lastName} says 'Just when I thought I was out, they pull me back in!'`;
}

student3.getName();
student3.getQuote();
student1.getQuote(); // ERROR
```

#### Extending class:
```js
class BionicStudent extends Student {
    getMod() {
        return `${this.firstName} ${this.lastName} has a bionic arm.`;
    }
}

let student4 = new BionicStudent('Ava', '3301');
student4.getName();
student4.getMod();
student2.getMod(); // ERROR
```
