# Get generic info
## Info inside the class
>
    from typing import Self, reveal_type

    class Foo:
        def return_self(self) -> Self:
            ...
            return self

    class SubclassOfFoo(Foo): pass

    reveal_type(Foo().return_self())  # Revealed type is "Foo"
    reveal_type(SubclassOfFoo().return_self())  # Revealed type is "SubclassOfFoo"

See https://docs.python.org/3/library/typing.html




## Info about origin of a typing class object
>
    typing.get_origin(tp)
    Get the unsubscripted version of a type: for a typing object of the form X[Y, Z, ...] return X.

    If X is a typing-module alias for a builtin or collections class, it will be normalized to the original class. If X is an instance of ParamSpecArgs or ParamSpecKwargs, return the underlying ParamSpec. Return None for unsupported objects.

    Examples:

    assert get_origin(str) is None
    assert get_origin(Dict[str, int]) is dict
    assert get_origin(Union[int, str]) is Union
    P = ParamSpec('P')
    assert get_origin(P.args) is P
    assert get_origin(P.kwargs) is P
    Added in version 3.8.

## Info about typing arguments of a typing class object
>
    typing.get_args(tp)
    Get type arguments with all substitutions performed: for a typing object of the form X[Y, Z, ...] return (Y, Z, ...).

    If X is a union or Literal contained in another generic type, the order of (Y, Z, ...) may be different from the order of the original arguments [Y, Z, ...] due to type caching. Return () for unsupported objects.

    Examples:

    assert get_args(int) == ()
    assert get_args(Dict[int, str]) == (int, str)
    assert get_args(Union[int, str]) == (int, str)
    Added in version 3.8.

See https://docs.python.org/3/library/typing.html#typing.get_type_hints



## Info about types from outside a function, method, module or class object

Return a dictionary containing type hints for a function, method, module or class object.
Note that you cannot run this to get information about a class instance from inside the class definition!!!

>
    from typing import Annotated, get_type_hints
    def func(x: Annotated[int, "metadata"]) -> None: pass

    get_type_hints(func)
    {'x': <class 'int'>, 'return': <class 'NoneType'>}
    get_type_hints(func, include_extras=True)
    {'x': typing.Annotated[int, 'metadata'], 'return': <class 'NoneType'>}

>
    class ChildHeader(MessageHeader):
        pass

    class ChildBody(MessageBody):
        pass

    message = Message[ChildHeader, ChildBody](childHeader, childBody)
    import typing
    type_hints = typing.get_type_hints(obj=message, include_extras=True)
    print(type_hints)

which prints

>
    {'header': <class 'MessageHeader'>, 'body': <class 'MessageBody'>}

Note that it does not print the name of inherited classes!!!

See https://docs.python.org/3/library/typing.html#typing.get_type_hints

Also see:

>
    class Foo[H]:
        @staticmethod
        def showType(name: str):
            type_hints = typing.get_type_hints(obj=Foo.showType, include_extras=True)
            print("type_hints:", type_hints)

    print("..............................................")
    method1 = Foo[int].showType
    print(method1.__code__)
    print(method1.__type_params__)
    print("..............................................")

    foo = Foo[str]()
    foo.showType("Hello showType")
    print("..............................................")