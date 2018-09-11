# Scala Basics

1. Val and Var

```scala
val x = 1 + 1
println(x) // 2
x = 3 // This does not compile.

var x = 1 + 1
x = 3 // This compiles because "x" is declared with the "var" keyword.
println(x * x) // 9
```

2. block

The result of the last expression will be the result of the overall block.

```scala
println({
  val x = 1 + 1
  x + 1
}) // 3
```

3. function

```scala
val addOne = (x: Int) => x + 1
println(addOne(1)) // 2

// no parameters
val getTheAnswer = () => 42
println(getTheAnswer()) // 42
```

4. method

```scala
def add(x: Int, y: Int): Int = x + y
println(add(1, 2)) // 3

// mutiple parameter list
def addThenMultiply(x: Int, y: Int)(multiplier: Int): Int = (x + y) * multiplier
println(addThenMultiply(1, 2)(3)) // 9

// no parameters
def name: String = System.getProperty("user.name")
println("Hello, " + name + "!")
```

5. Classes

```scala
class Greeter(prefix: String, suffix: String) {
  def greet(name: String): Unit =
    println(prefix + name + suffix)
}

// we need "new" to instantiate a class
val greeter = new Greeter("Hello, ", "!")
greeter.greet("Scala developer") // Hello, Scala developer!
```

5. Case Classes

 By default, case classes are **immutable** and **compared by value**. You can define case classes with the `case class` keywords.

```scala
case class Point(x: Int, y: Int)
// no "new" when instantiating a case class
val point = Point(1, 2)
val anotherPoint = Point(1, 2)
val yetAnotherPoint = Point(2, 2)

// compared by value
if (point == anotherPoint) {
  println(point + " and " + anotherPoint + " are the same.")
} else {
  println(point + " and " + anotherPoint + " are different.")
} // Point(1,2) and Point(1,2) are the same.

if (point == yetAnotherPoint) {
  println(point + " and " + yetAnotherPoint + " are the same.")
} else {
  println(point + " and " + yetAnotherPoint + " are different.")
} // Point(1,2) and Point(2,2) are different.
```



5. Objects

Objects are **single** instances of their own definitions. 

```scala
// definition
object IdFactory {
  private var counter = 0
  def create(): Int = {
    counter += 1
    counter
  }
}

val newId = IdFactory.create() // 1
val newerId = IdFactory.create() // 2
```

5. Traits

Traits are **types** containing certain fields and methods.

```scala
trait Greeter {
  // a trait can have default implementations of its methods
  def greet(name: String): Unit =
    println("Hello, " + name + "!")
}

// extend a trait and override its methods
class DefaultGreeter extends Greeter

class CustomizableGreeter(prefix: String, postfix: String) extends Greeter {
  override def greet(name: String): Unit = {
    println(prefix + name + postfix)
  }
}

val greeter = new DefaultGreeter()
greeter.greet("Scala developer") // Hello, Scala developer!

val customGreeter = new CustomizableGreeter("How are you, ", "?")
customGreeter.greet("Scala developer") // How are you, Scala developer?
```

5. Main method

Following the convention of JVM (Java Virtual Machine), a scala program needs to main method as its entry point.

``` scala
// Using an object to define main methods
object Main {
  def main(args: Array[String]): Unit =
    println("Hello, Scala developer!")
}
```

