### Operator

1. operator precedence

2. ":" related operators are right-associative

3. Destructing Binary constructor patterns:

   ```scala
   // The “cons” operator for matching head and tail of list:
   val x :: xs = List(1, 2, 3, 4)
   
   // Any arity-2 case class constructor works:
   val a Tuple2 b = 5 → "five"
   
   // Used a lot in Scala’s parser combinators:
   A ~ B // match A followed by B
   ```

### Call-By-Name and Call-By-Value

4. Call-By-Name is a compiler-provided feature that allows us to delay evaluation of some expressions until they are really needed.

5. Comparison of call-by-value thunks and call-by-name syntax:

```scala
// purpose: delay the evaluation of right

// call-by-value
def myOr(left: Boolean, right: () => Boolean) =
	if (left) true
	else right()

// call by name
def myOr(left: Boolean, right: => Boolean) =
	if (left) true
	else right
```

6. Syntactic sugar for passing arguments: Any function that takes a single argument can be applied by passing the argument **enclosed in braces** instead of parentheses

   ```scala
   def myAssert(predicate: => Boolean) =
   	if (assertionsEnabled && !predicate)
   	throw new AssertionError
   
   myAssert {
   	def double(n: Int) = 2 * n
   	double(2) == 4
   }
   ```



### Immutable Collections: List, Set, Maps

7. Why are Sets invariant in its parametric element type (and why are keys in Maps also invariant)?