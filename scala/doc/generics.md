# Litterature
A very good and in-depth blog that explains Type Lambda etc:
https://rockthejvm.com/articles/scala-3-type-level-programming

# Type Lambdas
Scala 3 introduces a more concise syntax for Type Lambdas, making them easier to understand and use. A Type Lambda allows us to define type-level functions, which take types as input and return new types.

## Scenario: Creating a Type Alias with a Type Lambda
Imagine we have a generic container where we want to fix one type parameter while keeping another flexible.  
Type-lambdas are the equivalent of anonymous functions at the type-level.

A parameterized type definition
```
type T[X] = R
```

is regarded as a shorthand for an unparameterized definition with a type lambda as right-hand side:

```
type T = [X] =>> R
```

Another example:

```
type MyTypeLambda[A] = [T] =>> (A, T)
```

Here’s what’s happening:

MyTypeLambda[A] is a higher-kinded type that takes a type A as a parameter.
[T] =>> (A, T) defines a Type Lambda, which means:
- It takes a type T as an argument.
- It returns a tuple type (A, T).

##Usage Example
Let's instantiate this Type Lambda with concrete types.

```
type StringPair = MyTypeLambda[String] // Expands to: [T] =>> (String, T)
```

// Using StringPair with a concrete type
```
val example: StringPair[Int] = ("Hello", 42) // (String, Int)
```


# Polymorphic Function Type in Scala 3 
What Are Polymorphic Function Types?
In Scala 3, Polymorphic Function Types allow you to define functions that are generic over types directly in their type signature.

## Why Is This Useful?
Before Scala 3, we used type parameters in methods 

```
def myFunction[A](arg: A): A = arg
```

Now, in Scala 3, we can define polymorphic functions as values, making them first-class citizens.
This is useful when passing higher-order functions that operate on multiple types.

## Traditional Generic Method (Before Scala 3)
```
def identity[A](x: A): A = x
```

This is not a function value—it’s just a method.
We cannot pass it as a higher-order function without converting it to a function.

```
val identity: [A] =>> A => A = [A] => (x: A) => x
```
