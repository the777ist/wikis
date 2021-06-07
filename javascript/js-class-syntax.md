# JAVASCRIPT CLASS SYNTAX

#### Constructor, Properties, Instance Methods and Class Methods:
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
