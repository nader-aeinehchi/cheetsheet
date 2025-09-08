# Hello World with SBT

## Build a project from a template
```
sbt new scala/scala3.g8
```


See https://docs.scala-lang.org/getting-started/sbt-track/getting-started-with-scala-and-sbt-on-the-command-line.html

## Adding dependency
In `build.sbt`, add such a line:
```
libraryDependencies += "org.scala-lang.modules" %% "scala-parser-combinators" % "2.1.1"
```
> Note for Java Libraries: For a regular Java library, you should only use one percent (%) between the organization name and artifact name. Double percent (%%) is a specialisation for Scala libraries. You can learn more about the reason for this in the sbt documentation.

## Testing

You can run `sbt test`

```
import org.scalatest.funsuite.AnyFunSuite

  class CubeCalculatorTest extends AnyFunSuite {
      test("CubeCalculator.cube") {
          assert(CubeCalculator.cube(3) === 27)
      }
  }
```

> You’ve seen one way to test your Scala code. You can learn more about ScalaTest’s FunSuite on the official website. You can also check out other testing frameworks such as ScalaCheck and Specs2.

See https://docs.scala-lang.org/getting-started/sbt-track/testing-scala-with-sbt-on-the-command-line.html