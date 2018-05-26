# Differences when overloading functions and operators
 
Let's define two functions `typeName` that return "Float" for Float values
and "Integer" for (Binary)Integer values:

    func typeName(_ x: Float) -> String {return "Float"}
    func typeName<I: BinaryInteger>(_ x: I) -> String {return "Integer"}

Now a brief test:
    
    typeName(1.1)   // "Float"
    typeName(1)     // "Integer"

That worked nicely.
 
Let's change the _functions_ 'typeName" to prefix _operators_ '!':

    prefix operator !
    prefix func !(_ x: Float) -> String { return "Float" }
    prefix func !<I: BinaryInteger>(_ x: I) -> String { return "Integer" }

Doing the test now with the operators reveals something interesting:

    !1.1   // "Float"
    !1     // "Float"  !!!

Didn't you expected `!1` to return "Integer", just as in the function `typeName` case?


## Explaination
 
The resolution of overloads works a little different for operators than for functions.
 
In the operators case overloads using non-generics types (like `Float`) are preferred over generics (like `<I: BinaryInteger>`). So the literal `1` is converted to `Float` and the `(Float) -> String` version is used.
 
For function overloads there is no preference towards non-generic types so the 'expected' version `<I: BinaryInteger>(I) -> String` is selected for the literal `1`.
