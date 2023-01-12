---
title: Writing clean code
date: 2019-05-06 15:27:31
---
Legacy messy code slow you down.

## Software Rot
Here are the symptoms of bad design:-

- Rigidity: System is hard to change. Every change forces changes in other parts of the system.
- Fragility: When there are hidden dependencies consequences of changes and are not localized. Make changes in one place and hell breaks loose in unrelated parts of the System.
- Immobility: Inability to take desirable module from one system and reuse it. Code cannot be uncoupled. You may as well write from scratch.
- Viscosity: Structure of the system itself impedes everything you do. Can't write tests, needless complexity, repetition.
- Opacity: Everything is hard to understand.

> SOFTware should be a flexible product

## Design Principle

In a good design, the design itself reveals intent. The system should be adaptable enough to withstand the evolution of business needs.
Shift the design with change in requirement or software rot.

- Identify aspects that vary and separate them from what stays the same.
- Favor composition over inheritence. HAS A is better than IS A relationship. Delegate behavior to another object.
- Program to interface, not implementation. **`new`** Operator is concrete implementation, avoid it.
- Follow the Open/Close principle. We will talk of SOLID principles shortly.
- Strive for loosely coupled designs between objects that interact.
- Hollywood Principle- "Dont call us, we'll call you". This is Inversion of control.

## SOLID

1. **S**ingle Responsibility Principle: Class should have only one reason to change.
2. **O**pen/Close Principle: Open for extension, closed for modification. This is only possible if there is good partitioning.
3. **L**iskov Substitution Principle: Dont make a special case out of the subclass. Derived class should be substitutable for base class.
4. **I**nterface Segregation Principle: Keep Interfaces small. Dont make fat classes.
5. **D**ependency Inversion Principle: Low level details should depend on High Level Principle. HLP should be independent. 

## Refactor Code
1. One way is to get code to read like well written prose- construct well written code from names of variables and functions. There’s a very tight relationship between readability and maintainability.
You can use explanatory variable: the only purpose of this variable is to explain what its contents is example- isTestPage
> Code that is easy to read and reason about, is easy to maintain

Variables in small scope should have short names.
Name of functions should be inversely proportional to their scope.

4. Dont go up and down the abstraction hierarchy. Follow the SLAP principle or Single level of Abstraction Principle.

Look at the code below 
```java
   public boolean validate(String reference) {
   boolean hasExpectedPostFix = reference.trim().substring(reference.length() -8, reference.length() -1) == ":XZ-2020";
   boolean has10Digits = validatePrefix(reference);
   boolean totalSizeLengthIs18 = reference.length() == 18;

        return hasExpectedPostFix && has10Digits && totalSizeLengthIs18;
   }
   ```
Variables are trying to express some more abstract concepts but they are mixed with the low-level implementation detail.

Now look at code when the code inside is at the same level of abstraction.

```java
public boolean validate(String reference) {

     return ReferenceHelper.hasExpectedPostFix(reference) &&
            ReferenceHelper.has10Digits(reference) && 
            ReferenceHelper.totalSizeLengthIs18(reference);
}
```
3. Smaller functions are better. Nesting in a function should not be greater than one or two. Functions should not be large enough to hold nested structures.
4. Functions should do one thing. The computer scientist Dijkstra in the 1970’s first time mentioned the concept of separation of concerns in a publication about scientific thought. The idea of separating the different aspects of a problem allowed scientists to solve one problem at a time and eased the process of reasoning.
5. Extract till you drop. Using modern editors like IntelliJ you can extract easily.
6. Just then you start to move the functions into their own classes, packages and suddenly entire code starts to clean up.
When you have a set of functions manupilate a set of variables it is a class -> a class is hiding right there.

