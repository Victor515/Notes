# Scala Classes

I will leave some examples here:

```scala
// class name should be capitalized
class User
val user1 = new User

// class definition with a constructor
// Note: parameters are declared with var here
class Point(var x: Int, var y: Int) {

  def move(dx: Int, dy: Int): Unit = {
    x = x + dx
    y = y + dy
  }

  override def toString: String =
    s"($x, $y)"
}

val point1 = new Point(2, 3)
point1.x  // 2
println(point1)  // prints (2, 3)

// parameter default value and how to pass in values
class Point(var x: Int = 0, var y: Int = 0)
val origin = new Point  // x and y are both set to 0
val point1 = new Point(1)
println(point1.x)  // prints 1
val point2 = new Point(y=2)
println(point2.y)  // prints 2

// private members and getter/setter methods
class Point {
  private var _x = 0
  private var _y = 0
  private val bound = 100

  def x = _x // getter
  def x_= (newValue: Int): Unit = { // setter
    if (newValue < bound) _x = newValue else printWarning
  }

  def y = _y
  def y_= (newValue: Int): Unit = {
    if (newValue < bound) _y = newValue else printWarning
  }

  private def printWarning = println("WARNING: Out of bounds")
}

val point1 = new Point
point1.x = 99
point1.y = 101 // prints the warning

// public/private parameters
class Point(val x: Int, val y: Int)
val point = new Point(1, 2)
point.x = 3  // <-- does not compile because we use val here

class Point(x: Int, y: Int)
val point = new Point(1, 2)
point.x  // <-- does not compile because x is not visible
```

