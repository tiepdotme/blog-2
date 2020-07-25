---
layout: post
title: "Class: Static Members"
date: 2020-07-24 12:00:00 -0500
---

Classes in JavaScript can have static methods and properties. These members are members of the class, not members of the objects created from the class. Most likely, you would create them as utility methods (to compare class instance, clone or create objects).

### Static methods

```javascript
class Person {
    name;
    age;

    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    static orderByAge(a, b) {
        return a.age - b.age;
    }
}

const employees = [
    new Person("Parwinder", 22),
    new Person("Robert", 33),
    new Person("George", 18),
    new Person("Eliu", 101),
    new Person("Gaurav", 39)
]

employees.sort(Person.orderByAge);

console.log(employees);
```

In the example above, the `Person` class creates a person with a name and their age. We have a static method in the class called `orderByAge`. This method compares the age of all `Person`. The ordering of age does not belong to one specific person but to a group of them (or the parent class they were all created from).

The output of the above code will be:

```javascript
[ Person { name: 'George', age: 18 },
  Person { name: 'Parwinder', age: 22 },
  Person { name: 'Robert', age: 33 },
  Person { name: 'Gaurav', age: 39 },
  Person { name: 'Eliu', age: 101 } ]
```

Keep in mind that static methods are methods on the class alone! You cannot do the last two console logs below:

```javascript
class Person {
    name;
    age;

    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    static orderByAge(a, b) {
        return a.age - b.age;
    }

    static sayMyName(person) {
        return person.name;
    }
}

const me = new Person("Parwinder", 101);

console.log(me.name); // Parwinder => this is valid
console.log(me.age); // 101 => this is valid
console.log(me.orderByAge); // undefined or Property 'orderByAge' is a static member of type 'Person' 🚨
console.log(me.sayMyName); // undefined or Property 'sayMyName' is a static member of type 'Person' 🚨
```

### Static Properties (or public static fields)

🚨 This feature is in stage 3 of the ES proposal. Only Chrome 72 or above supports it at the moment. Check the [proposal here](https://github.com/tc39/proposal-static-class-features) and the [compatibility here](https://kangax.github.io/compat-table/esnext/#test-static_class_fields)

We use static fields when a field needs to exist only once per class and not on every instance we create. It could be used for storing config, endpoints, cache and so on.

```javascript
class Base {
    static field = "Base Class";
}

class Child extends Base {

}

class GrandChild extends Child {
    static field = "Grand Child Class";
}

console.log(Base.field); // Base Class
console.log(Child.field); // Base Class
console.log(GrandChild.field); // Grand Child Class
```
