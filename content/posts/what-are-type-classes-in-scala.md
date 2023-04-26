---
title: "What are type classes in Scala?"
date: 2022-11-20T09:03:20-08:00
draft: false
---

## Introduction

### One of the most comprehensive concepts in [**Scala**](http://www.scala.com) is **Type Classes** which brings pure functional programming concepts in Scala.

*note that this tutorial need some knowlage of **Scala implicits**. If you are not fimilier with this concept, please refer to my other blog posts to take some twitch before start reading this blog post*

---
> qoute: This post codes will capable with `Scala 2.3.*` and `Scala 3.*`

Overral explanation for type classes in Scala is that adding new functionality to a type that will not expected other types have that 
functionality.

Lets get hands dirty and write some code to see what happen while defining type classes.

Here we have a class hierachy: 

```scala
trait Animal {
    def makeSound: String
}

class Dog(val name: String) extends Animal {
    override def makeSound: String = s"$name dog is making Weeeeeew sound!"
}

class Cat(name: String) extends Animal {
    override def makeSound: String = s"$name cat is making Meeeeow sound!"
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
    def guarding(animal: A): String
}
```

Step 2:
As a best practice it's better to bring our code instances in a singleton object for better code formatting and also as what [**Scala Cats**](https://typelevel.org/cats/) library has been implemented.
```scala
object GuardInstances {
    implicit object DogGuard extends GuardCapability[Dog] {
        override def guarding(animal: Dog): String = s"Dog with name ${animal.name} guards his owner."
    }
}
```

Step 3:
What **Cats Library** has done is that all the objects that access to the type class is centralized in syntax object. So, I here, do the same thing. Although there are several way to impliment type class *service layer* (what I call that), So we focus on the way on mentioned:
```scala
object GuradSyntax {
  object GuardSyntax {
    implicit class GuardOps[A](value: A) {
      def guard(implicit g: GuardCapability[A]): String = g.guarding(value)
    }
}
```

To test the code that we wrote, we define two instances of two `Cat` and `Dog` class. Then it shows that the capabilty of guarding for `Dog` will shows result but it will thorows an exception for `Cat` class instance.

```scala
def main(args: Array[String]): Unit = {
    import GuardInstances._
    import GuardSyntax._

    val teddy = new Dog("Teddy")
    val kitty = new Cat("Kitty")

    println(teddy.guard)
    // println(kitty.guard) --> // this will throw an exception
}
```

The output:

```
# Dog with name Teddy guards his owner.
```

Notice that those two lines of code:
```scala
import GuardInstances._
import GuardSyntax._
```

by importing these two lines, we tell the compiler to bring all the implicit objects and classes into the scope.
I will explain more about `implicits` and implicit scopes in the future but now the thing that is mentionable, is that we should import objects that have been contains **implicits**.

Type classes are very tremendous subject in Scala programming language and are very useful tools to develop applications. 
With that said, It needs more time and practice to get comftable with.\
In future posts, I will take more time to explain type classes and we will write more codes to get fimilier with that topic.


