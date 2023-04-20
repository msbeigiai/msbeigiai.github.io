---
title: "What are type classes in Scala?"
date: 2022-11-20T09:03:20-08:00
draft: true
---

## Introduction

One of the most comprehensive concepts in [**Scala**](http://www.scala.com) is **Type Classes**.

Overral explanation for type classes in Scala is that adding new functionality to a Class that will not expected other class have that functionality.

Lets get hands dirty and write some code to see what happen while defining type classes.

Here we have a class hierachy: 

```Scala
trait Animal {
    def makeSound: String
}
class Dog(name: String) extends Animal
class Cat(name: String) extends Animal
```
here *Cat* and *Dog* classes are both extends *Animal* trait, which means both inherit `makeSound` method from `Animal`.





