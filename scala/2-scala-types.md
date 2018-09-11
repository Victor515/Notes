# Scala Types

In Scala, all values have a type, including numerical values and **functions**.

 ![unified-types-diagram](./2-scala-types/unified-types-diagram.svg)



`Any` is the supertype of all types. It has methods like:

1. equals
2. hashCode
3. toString

It has two direct subclasses: `AnyVal` and `AnyRef`



`AnyVal` represent **value types**. There are nine predefined value types and they are **non-nullable**: `Double`, `Float`, `Long`, `Int`, `Short`, `Byte`, `Char`, `Unit`, and `Boolean`. 

`Unit` is a value type that carries no information. It has one literal value: `()`



`AnyRef` represents **reference types**. All non-value types are defined as reference types. Every user-defined type in Scala is a subtype of `AnyRef`. If Scala is used in the context of a Java runtime environment, `AnyRef` corresponds to `java.lang.Object`.



Let us see an example:

```scala
val list: List[Any] = List(
  "a string",
  732,  // an integer
  'c',  // a character
  true, // a boolean value
  () => "an anonymous function returning a string"
)
list.foreach(element => println(element))
```

result:

```
a string
732
c
true
<function>
```



**Type Casting**:



![type-casting-diagram](./2-scala-types/type-casting-diagram.svg)

```scala
val x: Long = 987654321
val y: Float = x  // 9.8765434E8 (note that some precision is lost in this case)

val face: Char = '☺'
val number: Int = face  // 9786

val x: Long = 987654321
val y: Float = x  // 9.8765434E8
val z: Long = y  // Does not conform
```



`Nothing` and `Null`

`Nothing` is subtype of all types, also known as bottom type. There is no value that has type as `Nothing`. It is usually used to represent a situation that an exception occurs.

`Null` is subtype of all reference types. It has one value:  `null`. Null type is basically used for interoperability with other JVM languages .