# Wildcards


## Vararg Splices
https://docs.scala-lang.org/scala3/reference/changed-features/vararg-splices.html

```
val arr = Array(0, 1, 2, 3)
val lst = List(arr*)                   // vararg splice argument
lst match
  case List(0, 1, xs*) => println(xs)  // binds xs to Seq(2, 3)
  case List(1, _*) =>                  // wildcard pattern

```

The old syntax for splice arguments will be phased out.

```
val lst = List(arr: _*)      // syntax error
lst match
    case List(0, 1, xs @ _*)  // ok, equivalent to `xs*`

```

## Wildcard Arguments in Types
https://docs.scala-lang.org/scala3/reference/changed-features/wildcards.html

The syntax of wildcard arguments in types is changing from _ to ?. Example:
```
List[?]
Map[? <: AnyRef, ? >: Null]
```

We would like to use the underscore syntax _ to stand for an anonymous type parameter, aligning it with its meaning in value parameter lists. So, just as f(_) is a shorthand for the lambda x => f(x), in the future C[_] will be a shorthand for the type lambda [X] =>> C[X]. This will make higher-kinded types easier to use. It will also remove the wart that, used as a type parameter, F[_] means F is a type constructor, whereas used as a type, F[_] means it is a wildcard (i.e. existential) type. In the future, F[_] will mean the same thing, no matter where it is used.

We pick ? as a replacement syntax for wildcard types, since it aligns with Java's syntax.

