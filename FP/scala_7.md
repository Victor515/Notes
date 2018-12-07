### Monad

1. When looking at library classes, watch for implementations of **map**, **flatMap**, **withFilter**. When these functions are defined, consider expressing your computation with **for expressions**.

2. In for expressions, clauses can be enclosed in braces instead of parentheses:

   ```scala
   for (
   x <- xs
   if x >= 0
   if x % 2 == 0
   ) yield square(x) + 1
   
   // is the same as
   for {
   x <- xs
   if x >= 0
   if x % 2 == 0
   } yield square(x) + 1
   ```

3. Desugaring for expressions (this is very **important!**)

   ```scala
   // Typical map, flatMap, and filter
   abstract class C[A] {
   	def map[B](f: A => B): C[B]
   	def flatMap[B](f: A => C[B]): C[B]
   	def withFilter(p: A => Boolean): C[A]
   }
   ```




### Additional Scala Features

#### Scripting in Scala

4. run Scala script from shell

   ```
   // hello.scala
   #!/usr/bin/env scala
   println("hello“)
   
   // change to exectuable
   chmod u+x hello
   ```


#### Fields in Non-case classes

#### Auxiliary Constructor

#### Companion Object

#### Extractors

 

