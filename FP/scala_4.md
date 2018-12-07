### Variance of Parametric Types

1. In general, we say that a parametric type C is **covariant** with respect to its type parameter S if: 

   ```S <: T implies C[S] <: C[T]```

2. In general, we say that a parametric type C is **contravariant** with respect to its type parameter S if: 

   ```S <: T implies C[T] <: C[S]```

   example: 

   ```scala
   abstract class Function1[-S,+T] {
   	def apply(x:S): T
   }
   ```

3. In general, we say that a parametric type C is invariant with respect to its type parameter S if: 

   ``` S <: T implies neither C[S] <: C[T] nor C[T] <: C[S]```

4. Syntax: 

   ```scala
   case class X[+A,-B,C] // covariance, contravariance, invariance
   ```

5. **Consumer-Producer** model:

   if S <: T

   (1) Consumer of S is super type of Consumer of T: Con(S) >: Con(T)

   (2) Producer of S is sub type of Producer of T: Prod(S) <: Prod(T)

   Notes: 

   * Container/Collection is considered producer

   * Consumed values are contra-variant
     (e.g., function arguments)
   * Produced values are co-variant
     (e.g., function return values)

6. We can check variance with *polarity annotation*

### Currying

7. Curring is just syntax sugar

```scala
def f(x0: T0, … ,xN: TN) = (y0: U0, …,yM: UM) => expr
```

can be rewritten as:

```scala
def f (x0: T0, … ,xN: TN)(y0: U0, …,yM: UM) = expr 
```

**Curring example**:  List's foldLeft and foldRight

```scala
abstract class List[+T] {
…
	def foldLeft[S](x: S)(f: (S, T) => S): S
	def foldRight[S](x: S)(f: (T, S) => S): S
}
```

### List Operations: fold, flatMap, filter, map and for-comprehensions

8. foldLeft and foldRight: foldLeft is **tail-recursive**, so it is preferred.

9. reduce operations should throw ```UnsupportedOperationException``` if applied to Empty list

10. zipWith and zip:

```scala
// in class List:
def zipWith[U,V](f: (T, U) => V)(that: List[U]): List[V] = {
	require (this.length == that.length)
	(this, that) match {
		case (Empty, Empty) => Empty
		case (Cons(x1,xs1), Cons(y1,ys1)) =>
			Cons(f(x1,y1), xs1.zipWith(f)(ys1))
	}
}

// in class List:
def zip[U](that: List[U]) = zipWith((_, _: U))(that)
```

11. flatten and flatMap: We can define flatMap as a method on lists directly and then define flatten in terms of it:

```scala
abstract class List[+T] { …
	def flatMap[S](f: Nothing => List[S]): List[S]
}
case object Empty extends List[Nothing] { …
	def flatMap[S](f: Nothing => List[S]) = Empty
}

case class Cons[+T](head: T, tail: List[T])
extends List[T] { …
	def flatMap[S](f: T => List[S]) =
	f(head) ++ tail.flatMap(f)
}
```

12. **For Expressions**:

    For expression reduces to a collection, depending on the types of collections iterated over.

    * Scala’s for-comprehensions are a concise **short-hand** for composing monadic operations: flatMap, map, filter.

```scala
for clauses yield body

// to
for (i <- 1 to 10) yield square(i) + 1 // we call this a "generator"

// until
for (i <- 0 until 10) yield square(i) + 1

// predicate(filter)
for {
	i <- 0 until 10
	if i % 2 == 0
} yield square(i) + 1

// steps
for (i <- 0 until 10 by 2)
	yield square(i) + 1
```

