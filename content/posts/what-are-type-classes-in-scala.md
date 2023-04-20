---
title: "What are type classes in Scala?"
date: 2022-11-20T09:03:20-08:00
draft: false
---

## Introduction

### One of the most comprehensive concepts in [**Scala**](http://www.scala.com) is **Type Classes** which brings pure functional programming concepts in Scala.

*note that this tutorial need some knowlage of **Scala implicits**. If you are not fimilier with this concept, please refer to my other blog posts to take some twitch before start reading this blog post*

---

Overral explanation for type classes in Scala is that adding new functionality to a type that will not expected other types have that 
functionality.

Lets get hands dirty and write some code to see what happen while defining type classes.

Here we have a class hierachy: 

```scala
trait Animal {
    def makeSound: String
}
class Dog(name: String) extends Animal {
    override def makeSound = s"$name dog is makeing Weeeeeew sound!"
}
class Cat(name: String) extends Animal {
    override def makeSound = s"$name cat is makeing Meeeeow sound!"
}
 ```
here `Cat` and `Dog` classes are both extends `Animal` trait, which means both inherit `makeSound` method from `Animal`.

As the code shown that `Dog` and `Cat` both have `makeSound` method which brings us to the question that what happen if we want to define a functionality for `Dog` that show that a *dog* has ability to take care of its owner. We know that this ability belongs to *dogs* only and *cats* don't have that.

Here, type classes bring us to this functionality.

Type classes in **Scala** define with three major characteristics:
> 1 - Ceating a trait with at list one genereic type.\
2 - Creating implicit instances to define type specific instances. \
3 - Creating syntaxes that will be service layer for final use case

What do I mean? \
Step 1:
```scala
trait GuardCapability[A] {
    def guarding(value: A): String // A trait that has typed with 'A' and 'value' with type A
}
```

Step 2:
As a best practice it's better to bring our code instances in a singleton object for better code formatting and also as what [**Scala Cats**](https://typelevel.org/cats/) library has been implemented.
```scala
object GuradInstances {
    implicit object DogGuard extends GuardCapability[Dog] {
        def guarding(dog: Dog): String = s"Dog with name ${dog.name} is guarding..."
    }
}
```

Step 3:

*to be countinue...*




