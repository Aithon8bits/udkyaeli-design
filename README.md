# Udkyaeli Design

Udkyaeli is a project for a programming language. Feedback and/or contributions are much apreciated. All of the following is subject to change.

## Features

### Comments

#### Single line

Single line coments start with `//` and span until the end of a line.

#### Multiline

Multiline comments are enclosed between `/*` and `*/`.

### Lexical scoping

Children scopes are created based on their position in the source code, not in the calling context.

### Garbage collection

Objects are destroyed when they are no longer accessible from the program.

### Literals

#### Null

The `Null` class has a single state. It is accessible through the literal `null`.

#### Boolean

The `Boolean` class has two states. They are accessible through the literals `true` and `false`.

#### Unbounded integers

Sequences of digits with an optional minus sign (`-`), such as `0`, `51` or `-8`, are interpreted as arbitrary precision integers, written in base 10. Unbounded integers do not support bitwise operations.

#### Fixed size integers

Numbers of the form `X#Y` with `X` being either `8`, `16`, `32` or `64`, and `Y` an integer, are interpreted as `Y` stored as a 2's compliment in `X` bits. Fixed size integers do support bitwise operations.

#### Floats

Numbers of the form `X.Y`, with `X` an integer and `Y` a natural, are interpreted as double-precision floating-point numbers.

#### Strings

Text enclosed in `"`'s is interpreted as strings. The following escape sequences are recognized: `\\` (backslash), `\"` (double quote), `\n` (ASCII linefeed), etc.

#### Lists

Arrays of elements enclosed between `TYPE::[` and `]` (with `TYPE` an expression that evaluates to a type common to all the elements) and separated by `,` are interpreted as lists of `TYPE`.
For example:

```
Integer::[0, 1, 1, 2, 3, 5, 8]
```

### Statements and blocks

#### No need for semicolons

Semicolons at the end of each statment are not required. They can, however, be used to put two statements in the same physical line.

#### Line continuation

A logical line can span multiple physical lines. To do so add a `\` at the continuation edges. Take note that no space is automatically inserted.

For example:
```
long_long_long_long_long_\
\long_long_variable_name
```

Usually, you don't break lines somewhere where you couldn't spare a space, so it's a good practice to put space before an ending `\` and after a starting one.

```
not_so_long_variable_name = \
\ 4
```

#### Blocks

Blocks of statements are sequences of lines one identation level to the right of the previous line. They define a new child scope.

### Open multi methods

#### Anonymous methods

Anonymous methods are defined by an arrow (`->`), followed by the argument list (`TYPE: PARAMETER_NAME`, separated by `,`), another arrow, the return type, yet another arrow and a statement or block.

For example:

```
-> Integer: x, Integer: y -> Float -> return x / y
```

```
-> Integer: x, Integer: y -> Float ->
    // do something
    // do more stuff
    return x / y
```

Even if you use a single statement a new scope is created.

#### Type checking

The types of each argument are checked to be children of their corresponding parameter type on every call that is not dispatched by a generic function. The return type is checked in return statements as well.

#### Additional conditions

You can specify additional conditions besides the type for the arguments by adding a `: CONDITION`.

For example, the following two methods calculate the absolute value:

```
-> Float: x: x < 0 -> Float -> return -x

-> Float: x: x >= 0 -> Float -> return x
```

#### Specific argument

Instead of specifying a type and name for a parameter, you can put only an expression. If you do so, calling that function requires that argument to be equal to the result of that expression.

For example, to the following method calculates the factorial for 0. To define it for the rest of the numbers a generic function is needed.

```
-> 0 -> Integer -> return 1
```

#### Generic functions

Generic functions are a collection of methods. They dispatch in runtime with virtuality in all their arguments, prioritizing from left to right.

To add a method to a function you use the `define` function.

Here is the full factorial example.

```
Function: factorial := Function.raw_new

define factorial -> 0 -> Integer -> return 1

define factorial -> Integer: x: x > 0 -> Integer -> return x * factorial (x - 1)
```

#### Call syntax

Functions and methods can be called by separating the callable and the arguments by spaces.

For example, to call `f` with arguments `x`, `y` and `z`:

```
f x y z
```

Alternatively, they can be called by `FIRST_ARGUMENT.CALLABLE REST_OF_ARGUMENTS`.

For example:

```
x.f y z
```

Functions that take no arguments can be called by appending a `.` to the left.

For example:

```
.main
```

### Type system

#### Global base type

All the types, both built-ins as user defined, inherit from `Data`.

#### Constant and mutable types

Mutable types are written with a `?` at the end. Constants are not. Mutable types inherit from their constant counterpart.

For example, let `MyType` be a (constant) type, it's mutable version is `MyType?`.

#### Multiple inheritance

Types are allowed to have multiple parents. For dispatch, their C3 linearization is used. If they are not linearizable by such algorithm, it's mandatory to specify an explicit linearization.

All the inheritance is virtual.

### User defined types

#### Enums

Enums are defined by a `$$` followed by a enum block.

For example:

```
Emotion := $$
    happy
    sad
    angry
    scared
```

#### Records

Records are defined by a `$` followed by a record block.

For example:

```
Person := $
    String: name
    Integer?: age
```

The function `raw_new` creates instances of it's argument.

```
Person: john := Person.raw_new
```

Members are accessed through the operator `@`.

```
john@name = "John"
john@age = 34
```

You often want to define a constructor for the record. You can use any function but it's usual to override `new`.

```
define new -> Person, String: name: name.lenght > 0, Integer: age: age >= 0 -> Person ->
    Person: self := Person.raw_new
    self@name = name
    self@age = age
    return self

Person: ann := Person.new "Ann" 27
```

#### Getters

To write a getter simply define a unary method and call it with the `.` operator.

```
define name -> Person: person -> String ->
    return person@name

console.write_line ann.name
```

## To do

* Add a syntax for other bases besides 10.
* If possible, make the content type of lists infered instead of explicit for non empty lists.
