STCK
====
_a stack-based programming language_

STCK (pronounced stick) is is a programming languague inspired by [Forth](https://en.wikipedia.org/wiki/Forth_(programming_language)). Variables are never declared, values is just placed on a global stack. Syntax is minimal. The only supported data-type is 32-bit integers.


Installation
------------

A very limited choice of prebuilt binaries can be found in the /binaries folder. If you can't find a binary that works for you, you'll need a F# compiler (fsharpc/fshapri) to compile the STCK interpreter. [This guide](http://fsharp.org/use/linux/) is usefull if you're using Linux. When the compiler is installed, navigate to the folder containing `stck.fs` and run:

    fsharpc stck.fs && ./stck.exe ./samples/euler-one.stck

This should compile the interpreter into stck.exe, and run a stck-program, solving the first [Project Euler](https://projecteuler.net/) problem. Just running stck.exe will launch the interactive interpreter.


Using the languague
-------------------

**Delimiting lines**

When using the interpreter, return acts as the line delimiter.

    2 3 +
    [5]

The entire statement must fit on one line, as multi-line statements is not supported.

When executing files, `!` acts as the line-delimiter, making it possible to declare statements over several lines.

    2 3
    +
    !

Lines cannot be nested.

**working with the stack**

`.` will drop one element from the stack.

    2 3 4
    [4; 3; 2]
    .
    [3; 2]

`swap` will swap the two upmost elements on the stack.

    2 3 4
    [3; 2; 4]
    swap
    [2; 3; 4]

`dup` will duplicate the topmost element on the stack (TOS)

    2
    [2]
    dup
    [2; 2]

`2dup` will duplicate the two topmost elements on the stack.

    2 3
    [3; 2]
    2dup
    [3; 2; 3; 2]

`over` will copy the second element on the stack.

    2 3
    [3; 2]
    over
    [2; 3; 2]

`rot` will move the third element to the top of the stack.

    1 2 3
    [3; 2; 1]
    rot
    [1; 3; 2]

`len` will return the size of the stack and push the size onto the stack.

    1 1
    [1; 1]
    len
    [2; 1; 1]

`empty` will check if the stack is empty, and push the result onto the stack.

    []
    empty
    [1]

`max` and `min` will remove all elements on the stack, leaving only the largest or smallest integer.

    1 3 1 max
    [3]
    1 -2 1 min
    [-2]

In addition, the [Forth dokumentation](http://wiki.laptop.org/go/Forth_stack_operators) has a good description of different stack operators, along with reference implementations for less basic operators.

**Math**

The following operators are supported: `+` (addition), `-` (substraction), `*` (multiplication), `/` (division) `i/` (integer division) and `%` (modulo). All operators, except `/` perform on the two upmost elements on the stack, and push the result back on the stack as one number.

    5 2 -
    [3]

`/` divides two integers, and push the result of the division onto the stack as two numbers (integer quotient and remainder). The remainder is always given as a number with six digits. In the case where the remainder exceeds six digits, no rounding is performed.

    2 3 /
    [666666; 0]

In addition, the remainder can be computed directly with the `rem` operator.

    2 3 rem
    [666666]

**Boolean operators**

False is represented by `0`(zero) and anything else is considered true. The following boolean operators are supported: `=` (equal), `>` (greater than), `<` (less than), and `not`. All operators perform on the two upmost elements on the stack, and push the result back on the stack.

**Conditionals**

Conditionals follow the if-then-else construct: `<a boolean> ? <this will happen if true> : <this will happen if false> ;`

    3 3 = ? 1337 : 192 ;
    [1337]

    3 17 = ? 1337 : 192 ;
    [192]

**Subroutines**

Subroutines are declared by using `#`:

    # add-five 5 +
    []
    2 add-five
    [7]

The contents of a subroutine is contained within a line. So remeber to terminate the subroutine declaration with `!` when not using the interactive interpreter.

    // some-file.stck !
    
    # add-five 5 + !
    
    2 add-five !

**Comments**

`//` indicates the start of a comment. Comments are considered as statements, and therefore has to be delimited as regulare lines with `!`.

    // This is a comment !
    
    // This 
    is also 
    a commet !

**Utility functions**

`hprint` prints the content of the heap. This will list all declared subroutines.
`sprint` prints the content of the stack. This is equal to the reply given by the interpreter.
`quit` exits the interprenter.


Releaselog
----------

**V 1.1**

Changed behaviour of /. Integer division is now performed with i/ and / performs regular division.

Added the command `quit`, `2dup`, `len`, `max`, `min` and `remainder`.

**V 1.0**

Initial release.