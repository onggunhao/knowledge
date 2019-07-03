---
layout: default
title: Javascript
parent: Languages
nav_order: 3
---

# Typescript

**Good typescript intro**
https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html

## Key things to remember
* Needs to be compiled from `.ts` files to `.js` using `tsc`
* magic of typescript is catching bugs through static analysis at compile time

### type annotations (person:string)

```
function greeter(person: string) {
    return "Hello, " + person;
}

let user = "Jane User";

document.body.innerHTML = greeter(user);

```
### interface
* Somewhat like structs, allow you to define objects with a particular structure. TS compiler will check if args being passed in are correct / objects are compatible 

### Classes
* Has constructor, etc. Makes javascript a class-based object-oriented language
* classes and interfaces are interoperable - see below example

```
class Student {
    fullName: string;
    constructor(public firstName: string, public middleInitial: string, public lastName: string) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);
```
