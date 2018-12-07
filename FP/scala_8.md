### Semantics of Exceptions



### Implicit Conversions



### Value Classes



### State Monad

```scala
// Since scala.collection.immutable.Stack was deprecated
// in favor of List, let's declare a type alias.
// We'll alias List[Int] so that we don't have to deal
// with type parameters on our Stack type.

type Stack = List[Int]

val stack: Stack = List(3, 2, 1)

// To simulate stack mutation in functional code,
// our stack operations return a pair:
// new stack state -> operation result value

def push(x: Int, stack: Stack): (Stack, Unit) = (x :: stack) -> {}

def pop(stack: Stack): (Stack, Int) = {
  val x :: xs = stack
  xs -> x
}

// push followed by pop should yield the original stack
val (stack1, _) = push(4, stack)
val (stack2, top) = pop(stack1)
assert(stack == stack2)

// Now let's say we want a function to swap the top two elements on the stack.
// We could implement it like this...

def swapTopPairV1(stack: Stack): (Stack, Unit) = {
  val x :: y :: zs = stack
  (y :: x :: zs) -> {}
}

// ... but that's too easy!
// Since we want to model imperative state changes in functional code,
// we'll implement our swap function in terms of pop and push:

def swapTopPairV2(stack: Stack): (Stack, Unit) = {
  val (stack1, x) = pop(stack)
  val (stack2, y) = pop(stack1)
  val (stack3, _) = push(x, stack2)
  val (stack4, _) = push(y, stack3)
  stack4 -> {}
}

// The two versions of the swapTopPair function are equivalent.

val swappedStack1 = swapTopPairV1(stack)
val swappedStack2 = swapTopPairV2(stack)
assert(swappedStack1 == swappedStack2)

// It would be nice if we didn't have to explicitly thread that state around.
// E.g., if we accidentally used state2 a second time in place of state3,
// we would get the wrong result.
//
// If we create a new Monad type implementing map and flatMap,
// we can use those methods to implicitly thread the state through
// successive clauses of a for-expression.

trait State[S, A] {
  def run(initial: S): (S, A)

  def map[B](f: A => B): State[S, B] = {
    state =>
      val (statePrime, result) = run(state)
      statePrime -> f(result)
  }

  def flatMap[B](f: A => State[S, B]): State[S, B] = {
    state =>
      val (statePrime, result) = run(state)
      f(result).run(statePrime)
  }
}

type StackState[A] = State[Stack, A]

// Now we can re-write our push and pop operations
// as State Monad combinator functions:

def pop(): StackState[Int] = {
  stack =>
    val x :: xs = stack
    xs -> x
}

def push(x: Int): StackState[Unit] = {
  stack => (x :: stack) -> {}
}

// Note that we're taking advantage of Scala's ability
// to expand a lambda expression into a class with a single abstract method
// (similar to how lambdas work in Java 8).
// An equivalent "desugared" push method is given below.

def desugaredPush(x: Int): State[Stack, Unit] = new StackState[Unit] {
  def run(stack: Stack): (List[Int], Unit) = (x :: stack) -> {}
}

// Now we can implement our swap function with the StackState monad:

def swapTopPairV3(): StackState[Unit] = for {
  x <- pop()
  y <- pop()
  _ <- push(x)
  _ <- push(y)
} yield ()

swapTopPairV3().run(stack)

// Remember that the for-expression above
// expands to a chain of flatMap and map calls:

def desugaredSwapTopPairV3(): StackState[Unit] =
  pop().flatMap { x =>
    pop().flatMap { y =>
      push(x).flatMap { _ =>
        push(y).map { _ =>
          ()
        }
      }
    }
  }

// Is that really so much simpler?
// Yes, the for-expression version does look much less cluttered;
// however, the control flow is now significantly more complex.
// Here's another example for comparison:

locally {
  val (stack1, _) = swapTopPairV2(stack)
  val (stack2, _) = push(4, stack1)
  val (stack3, _) = swapTopPairV2(stack2)
  pop(stack3)
}

locally {
  for {
    _ <- swapTopPairV3()
    _ <- push(4)
    _ <- swapTopPairV3()
    result <- pop()
  } yield result
}.run(stack)

locally {
  swapTopPairV3().flatMap { _ =>
    push(4).flatMap { _ =>
      swapTopPairV3().flatMap { _ =>
        pop().map {
          result => result
        }
      }
    }
  }
}.run(stack)

// Using the for-expression syntax with the State Monad *does*
// give us a nicer way to thread state in a functional way.
// However, it has its limitations.
// For example, what if I want to push a List of elements onto a stack?

def pushAllSkeleton(xs: List[Int]): StackState[Unit] = xs match {
  case Nil => ???
  case y :: ys => ???
}

// We need a way to simply yield unit when we reach the base case.
// We'll add a new State Monad combinator for that:

def inject[S, R](result: R): State[S, R] = s => s -> result

// Now we can "inject unit" as a no-op in the base case,
// and combine push with a recursive pushAll in the recursive case.

def pushAll(xs: List[Int]): StackState[Unit] = xs match {
  case Nil => inject {}
  case y :: ys => for {
    _ <- push(y)
    _ <- pushAll(ys)
  } yield ()
}

// However, that's not tail recursive.
// The order in which the expressions are executed is also confusing.
// What's actually happening here is that we're recursively building
// a chain of StackState monad instances, which we would then thread
// a stack through later to simulate execution.
// This is similar to how we built parsers using the Parser Combinators,
// and then actually parsed the input afterward by calling parser.parseAll(...)
// on the resulting parser object.
//
// In contrast, the version using explicit state threading is a bit uglier,
// but it's tail recursive (via foldLeft).
// The control flow is also much easier to understand in this case,
// since there is no intermediate combinator object constructed,
// and no delayed execution of our state-transforming functions.

def pushAllExplicit(xs: List[Int], stack: Stack): (Stack, Unit) = {
  val basePair = stack -> {}
  xs.foldLeft(basePair) {
    (accPair, x) =>
      val (currentStack, _) = accPair
      (x :: currentStack) -> {}
  }
}

// Advanced Haskell-inspired libraries like Scalaz and Cats introduce
// other Monad tools that might let us build a better version of pushAll---
// but with an even steeper learning curve than what we have here.
//
// The State Monad is a neat idea,
// but unless you're programming in Haskell you probably won't use it.
```

