# Udkyaeli Design

Udkyaeli is a project for a programming language. Feedback and/or contributions are much apreciated. All of the following is subject to change.

## Features

### Comments

#### Single line

Single line coments start with `//` and span until the end of a line.

#### Multiline

Multiline comments are enclosed between `/*` and `*/`.

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

#### Methods

Anonymous methods are defined by an arrow (`->`), followed by the argument list (including the type of each argument), another arrow, the return type, yet another arrow and a statement or block.

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

## To do

* Add a syntax for other bases besides 10.
* If possible, make the content type of lists infered instead of explicit for non empty lists.
