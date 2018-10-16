# Scala Traits

Traits are similar to interfaces in Java 8. Classes can extend multiple traits, but traits cannot be instantiated. 



## Defining a trait

```scala
trait HairColor

// Note A here is a generic type
trait Iterator[A] {
  def hasNext: Boolean
  def next(): A
}

// Write an IntIterator class to extend Iterator Trait
class IntIterator(to: Int) extends Iterator[Int] {
  private var current = 0
  override def hasNext: Boolean = current < to
  override def next(): Int =  {
    if (hasNext) {
      val t = current
      current += 1
      t
    } else 0
  }
}

val iterator = new IntIterator(10)
iterator.next()  // returns 0
iterator.next()  // returns 1
```



## Subtyping

This is kind of like polymorphism.

```scala
import scala.collection.mutable.ArrayBuffer

trait Pet {
  val name: String
}

class Cat(val name: String) extends Pet
class Dog(val name: String) extends Pet

val dog = new Dog("Harry")
val cat = new Cat("Sally")

val animals = ArrayBuffer.empty[Pet]
animals.append(dog)
animals.append(cat)
animals.foreach(pet => println(pet.name))  // Prints Harry Sally
```

