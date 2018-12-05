### Introduction

1. Functional Programming is a style of programming that avoids side effects. Question: Why is this good?
2. Functional Programming is a style of programming that emphasizes functions as the basis of computation (functions applied to arguments, functions as parameters, functions as return values).
3. OOP is not the opposite of FP, they can complement each other. Scala is a language designed for both.

### Reasoning about the past: Arithmetic and algebra

4. Expressions has **static types**:  they either reduces to a value with specific value types, or a well-defined set of exceptions.  This means our computation is *type safe*.
5. The substitution rule/computation by reduction: (1) Reduce the arguments, left to right (2) replace the body of function, each parameter replaced by corresponding argument 
6. Type rules for functions

### Core Scala: primitive types and operators

7. If there is no recursion, we can elide the return type of a function in definition
8. Int are fixed-sized. Why is this good? What do we need to be careful about (overflow)?
9. Double: double-precision binary representation, also fixed size(64 bits)
10.  Distances between Doubles are not fixed, dense toward 0
11. Overflow (Double.PositiveInfinity, Double.NegativeInfinity) and Underflow(0.0, -0.0) in Doubles

```
1.0 / 0.0 ↦ Double.PositiveInfinity
1.0 / -0.0 ↦ Double.NegativeInfinity
Double.PositiveInfinity / 0.0 ↦ Double.PositiveInfinity
## Divide by Zero in Doubles
0.0 / 0.0 ↦ Double.NaN
-0.0 / 0.0 ↦ Double.NaN
```

12. Doubles break common algebraic rules:

```
(0.1 + 0.2) + 0.3 ↦
0.6000000000000001
0.1 + (0.2 + 0.3) ↦
0.6

Double.NaN != Double.NaN

100.0 * (0.1 + 0.2) ↦
30.000000000000004
100.0 * 0.1 + 100.0 * 0.2 ↦
30.0
```

13. When using Doubles, instead of writing x == y, write abs(x - y) <= error

### Conditional Expressions

14. Reduction rule a little bit different: (1) reduce the test clause (2) if test is true, reduce the then clause (3) if false, reduce the false clause (**short circuiting**)

### Constants and Compound Expressions

15. In Scala, `val` is immutable
16. `require` and `ensuring` clauses (good coding style): use require at the front of functions, ensuring at the end
17. Reduction of compound expressions:
    * First, compute the value of each constant definition,
      reducing top to bottom, left to right
    * Then reduce the result expression, substituting each
      occurrence of a constant name with its computed value

### Conditional Functions

18. Conditional functions
    * On ranges: use "if, else if, else" clauses
    * On point values, use "case, match" clauses
19.  We can use (1) primitive value (2) free variable (3) _(do not care) in the pattern

### Compound Datatypes

20. Sequential data -- Tuples and Arrays

    * tuples contain values of different types, array elements must have the same type
    * tuples are not zero-indexed, but arrays are
    * tuples are immutable, arrays are not
    * Empty tuples have type `Unit`, tuples with length one does not exist

    ```scala
    // Tuple accesing
    (1,2,3)._2 // 3
    
    // Tuple pattern
    def incomeTaxForBracketCutoff(income: Int, bracketCutoff: (Int, Int)) = {
    	require(income >= 0)
    	bracketCutoff match {
    		case (bracket, cutoff) => {
    			(income - cutoff) * bracket /
    			divisor + incomeTax(cutoff)
    		}
        }
    } ensuring (result => result >= 0) 
    
    // Accessing array
    Array(1,2,3)(2) // 3
    
    // Array Pattern
    def sumOfSquares(coordinates: Array[Int]) = {
    	coordinates match {
    		case Array(x,y,z) => x*x + y*y + z*z
    	}
    }
    ```

21. Structural data -- case classes

    * We can think of a case class as a tuple with its own type and accessors for its elements

    ```scala
    // accessing case class fields
    def magnitude(coordinate: Coordinate): Int = {
    	coordinate.x * coordinate.x +
    	coordinate.y * coordinate.y
    }
    
    // case class patterns
    def magnitude(coordinate: Coordinate): Int =
    	coordinate match {
    	case Coordinate(x,y) => x*x + y*y
    	}
    }
    ```
