
So, how can we obtain type information of variables at runtime? The Scala compiler has three ways to produce type information at runtime:

- TypeTag contains all type information at runtime.
- ClassTag obtains the runtime class of the type but doesn’t inform us about the type parameters.
- WeakTypeTag is a weaker version of TypeTag that obtains abstract type information.

See: https://www.baeldung.com/scala/type-information-at-runtime