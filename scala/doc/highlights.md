# Official site
https://docs.scala-lang.org/scala3/new-in-scala3.html

# Class definitions

## Traits with parameters
Traits move closer to classes and now can also take parameters, making them even more powerful as a tool for modular software decomposition.

https://docs.scala-lang.org/scala3/reference/other-new-features/trait-parameters.html

```
trait Greeting(val name: String):
  def msg = s"How are you, $name"

class C extends Greeting("Bob"):
  println(msg)


class D extends C, Greeting("Bill") // error: parameter passed twice
```

## Open classes
Unless a class is defined `Open`, it is closed and cannot be inherited. There are some exceptions, see:

https://docs.scala-lang.org/scala3/reference/other-new-features/open-classes.html

Extending classes that are not intended for extension is a long-standing problem in object-oriented design. To address this issue, open classes require library designers to explicitly mark classes as open.

```
// File Writer.scala
package p

open class Writer[T]:

  /** Sends to stdout, can be overridden */
  def send(x: T) = println(x)

  /** Sends all arguments using `send` */
  def sendAll(xs: T*) = xs.foreach(send)
end Writer

// File EncryptedWriter.scala
package p

class EncryptedWriter[T: Encryptable] extends Writer[T]:
  override def send(x: T) = super.send(encrypt(x))
```


## Opaque Typees
Hide implementation details behind opaque type aliases without paying for it in performance! 

Replacement for value classes: Opaque types supersede value classes and allow you to set up an abstraction barrier without causing additional boxing overhead.

https://docs.scala-lang.org/scala3/reference/other-new-features/opaques.html

In general, one can think of an opaque type as being ** only transparent in the scope of private[this] ** (unless the type is a top level definition - in this case, it's transparent only within the file it's defined in).

```
object MyMath:

  opaque type Logarithm = Double

  object Logarithm:

    // These are the two ways to lift to the Logarithm type

    def apply(d: Double): Logarithm = math.log(d)

    def safe(d: Double): Option[Logarithm] =
      if d > 0.0 then Some(math.log(d)) else None

  end Logarithm

  // Extension methods define opaque types' public APIs
  extension (x: Logarithm)
    def toDouble: Double = math.exp(x)
    def + (y: Logarithm): Logarithm = Logarithm(math.exp(x) + math.exp(y))
    def * (y: Logarithm): Logarithm = x + y

end MyMath
```
## Final classes

A final class cannot be exteded. Period.
```
final class Foo
```


## Sealed classes

The class can be extended only if the class and its subclasses are defined in the same file.


## Transient classes
Hide implementation details. Utility traits that implement behavior sometimes should not be part of inferred types. In Scala 3, those traits can be marked as transparent hiding the inheritance from the user (in inferred types).

https://docs.scala-lang.org/scala3/reference/other-new-features/transparent-traits.html

```
transparent trait S
trait Kind
object Var extends Kind, S
object Val extends Kind, S
val x = Set(if condition then Val else Var)
```


## Export
An export clause defines aliases for selected members of an object.
Composition over inheritance. This phrase is often cited, but tedious to implement. Not so with Scala 3’s export clauses: symmetric to imports, export clauses allow the user to define aliases for selected members of an object.

https://docs.scala-lang.org/scala3/reference/other-new-features/export.html


```
class BitMap
class InkJet

class Printer:
  type PrinterType
  def print(bits: BitMap): Unit = ???
  def status: List[String] = ???

class Scanner:
  def scan(): BitMap = ???
  def status: List[String] = ???

class Copier:
  private val printUnit = new Printer { type PrinterType = InkJet }
  private val scanUnit = new Scanner

  export scanUnit.scan
  export printUnit.{status as _, *}

  def status: List[String] = printUnit.status ++ scanUnit.status
```

## Enums

https://docs.scala-lang.org/scala3/reference/enums/enums.html

```
import Planet.*
enum Planet(mass: Double, radius: Double):
  private final val (mercuryMass, mercuryRadius) = (3.303e+23, 2.4397e6)

  case Mercury extends Planet(mercuryMass, mercuryRadius)             // Not found
  case Venus   extends Planet(venusMass, venusRadius)                 // illegal reference
  case Earth   extends Planet(Planet.earthMass, Planet.earthRadius)   // ok
object Planet:
  private final val (venusMass, venusRadius) = (4.869e+24, 6.0518e6)
  private final val (earthMass, earthRadius) = (5.976e+24, 6.37814e6)
end Planet

```

## NPE

- No more NPEs (experimental). Scala 3 is safer than ever: explicit null moves null out of the type hierarchy, helping you to catch errors statically; additional checks for safe initialization detect access to uninitialized objects.

# Generics

## Polymorphic function types

https://docs.scala-lang.org/scala3/reference/new-types/polymorphic-function-types.html

```

// A polymorphic method:
def foo[A](xs: List[A]): List[A] = xs.reverse

// A polymorphic function value:
val bar: [A] => List[A] => List[A]
//       ^^^^^^^^^^^^^^^^^^^^^^^^^
//       a polymorphic function type
       = [A] => (xs: List[A]) => foo[A](xs)


enum Expr[A]:
  case Var(name: String)
  case Apply[A, B](fun: Expr[B => A], arg: Expr[B]) extends Expr[A]


```




# Metaprogramming

## Inine
Makes the code faster!

As the basic starting point, the inline feature allows values and methods to be reduced at compile time. This simple feature already covers many use-cases and at the same time provides the entry point for more advanced features.

See inline, using, summon, sommonall etc

```
inline def doSomething(inline mode: Boolean): Unit =
  if mode then ...
  else if !mode then ...
  else error("Mode must be a known value")

doSomething(true)
doSomething(false)
val bool: Boolean = ...
doSomething(bool) // error: Mode must be a known value
```
# Contextual programming

## Using clauses

https://docs.scala-lang.org/scala3/reference/contextual/using-clauses.html

```
def max[T](x: T, y: T)(using ord: Ord[T]): T =
  if ord.compare(x, y) < 0 then y else x
```


## Given instances

Best description, see: 
- https://www.baeldung.com/scala/optional-braces

Other descriptions:
- https://docs.scala-lang.org/scala3/reference/contextual/givens.html
- https://alexn.org/blog/2022/10/24/scala-3-optional-braces/

```
trait Ord[T]:
  def compare(x: T, y: T): Int
  extension (x: T)
    def < (y: T) = compare(x, y) < 0
    def > (y: T) = compare(x, y) > 0

given intOrd: Ord[Int] with
  def compare(x: Int, y: Int) =
    if x < y then -1 else if x > y then +1 else 0

given listOrd[T](using ord: Ord[T]): Ord[List[T]] with

  def compare(xs: List[T], ys: List[T]): Int = (xs, ys) match
    case (Nil, Nil) => 0
    case (Nil, _) => -1
    case (_, Nil) => +1
    case (x :: xs1, y :: ys1) =>
      val fst = ord.compare(x, y)
      if fst != 0 then fst else compare(xs1, ys1)

```


# Control syntac

## Quiet:
You can omit parentheses or braces.

https://www.baeldung.com/scala/optional-braces

```
if x < 0 then
  "negative"
else if x == 0 then
  "zero"
else
  "positive"

if x < 0 then -x else x

while x >= 0 do x = f(x)

for x <- xs if x > 0
yield x * x

for
  x <- xs
  y <- ys
do
  println(x + y)

try body
catch case ex: IOException => handle
```

## Wildcard Arguments in Types

https://docs.scala-lang.org/scala3/reference/changed-features/wildcards.html

```
List[?]
Map[? <: AnyRef, ? >: Null]
```