### Case Class Continued

1. Reduction rule of class methods: first reduce the class and method arguments, then reduce the body of methods

2. patterns in assignment:

   ```scala
   def dotProduct(c1: Coordinate, c2: Coordinate) = {
   	val Coordinate(x1, y1) = c1
   	val Coordinate(x2, y2) = c2
   	x1*x2 + y1*y2
   }
   ```

3. **symbols** in patterns:

   * A symbol with a lower-case first character is a binding symbol
   * A character with an upper-case first character is a value
   * You can make a variable a constant using `backticks`

   ```scala
   val pi = 3.14
   val One = 1.0
   expr match {
   	case pi => "Pi"
   	case One => "One"
   }
   ```

### Singleton Objects

4. When compiling a Scala file, it is required that all constant and function definitions are placed *inside* a class or object

5. Access fields and functions in an object: using `dot`
6. Use case objects as a value, for pattern matching, for example. Use objects as a collection of **static** methods, or a value that won't be matched(?)

### Collections of some language syntax

7. Syntactic sugar for binary methods:

```scala
case class Coordinate(x: Int, y: Int) {
	def magnitude() = x*x + y*y
	def add(that: Coordinate) =
	Coordinate(x + that.x, y + that.y)
}

Coordinate(1,2).add(Coordinate(3,4)) // Coordinate(4,6)
Coordinate(1,2) add Coordinate(3,4)
```

8. For case classes, "==" is for structural equality, which is unlike Java

9. parameterless methods do not need to braces

   ```scala
   def toString = { … }
   Rational(4,6).toString
   ```

### Abstract Datatypes

10. **Intention** of abstract classes: abstract over a collection of compound datatypes that share common properties
11. With subclassing, we need to refine our rule of substitution when reducing a class method:
    * Reduce the receiver and arguments, left to right
    * **Find the body of m in C and reduce to that**,
      replacing constructor parameters with constructor
      arguments and method parameters with method
      argument
    * To find the body of m, we first check if there is one in C, then its immediate superclass  

12. Option class is a **collection** of zero or one items(abstraction). `Some[T]` represents the non-empty case, `None` represents the empty case.

13. (Design Templates) There are two cases to think about when designing a case class:  

    (1) We Expect Few New Functions But Many New Variants

    (2) We Expect Many New Functions But Few New Variants

14. **Sealed** keyword: adding sealed to a abstract class means that **all subclasses** of that type are in the same *compilation unit*.

### Recursively Defined Datatypes

15. Recursively defined datatype is for type with indefinite length

