### Traits

1. Traits provide a way to factor out common behavior among multiple classes and *mix* it in where appropriate.

```scala
// 1. Traits can declare fields and full method definitions
// 2. Trait **cannot** decalre constructors
// 3. Use either "extends" or "with 
trait Echo {
	val language = "Portuguese"
	def echo(message: String) =
		message
}

class Parrot extends Echo {
	def fly() = {
	// forget to fly and talk instead
        echo("poly wants a cracker")
    }
}

class Parrot extends Bird with Echo {
    def fly() = {
    // forget to fly and talk instead
    echo("poly wants a cracker")
    }
}

// create a new class instance with a mixin trait
trait X
case class Foo()
new Foo() with X
```

2.  Traits with self-types: a trait is only valid when mixed-in with specific types

   ```scala
   trait SmartTalk { 
       // SmartTalk must be mixed-in with Echo and Smart
       this: Echo with Smart =>
       def talk() = echo(somethingClever)
   }
   ```

   Question: What is the difference between self-types and inheritance

* Self-typing allows introduction of a **cyclic dependency** between two types.



### Parser Combinator



### Streams

3. A form of "lazy" sequence. Because of lazy evaluation, we could do something impossible to **List**

