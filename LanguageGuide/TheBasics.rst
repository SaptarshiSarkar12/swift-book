The Basics
==========

Swift is a new programming language for iOS and OS X app development.
Nonetheless, many parts of Swift will be familiar
from your experience of developing in C and Objective-C.

Swift provides its own versions of all of the fundamental C and Objective-C types,
such as ``Int`` for integers; ``Double`` and ``Float`` for floating-point values;
``Bool`` for Boolean values; and ``String`` for textual data.
Swift also provides powerful versions of the two primary collection types,
``Array`` and ``Dictionary`` (as described in :doc:`CollectionTypes`).

Like C, Swift uses variables to store and refer to values by an identifying name.
Swift also makes extensive use of variables whose values cannot be changed.
These are known as constants, and are much more powerful than constants in C.
Constants are used throughout Swift to make code safer and clearer in intent
when working with values that do not need to change.

In addition to all of the types you will be familiar with,
Swift also introduces some advanced types not found in Objective-C.
These include tuples,
which enable you to create and pass around ad-hoc groupings of values.
Tuples can be used to return multiple values from a function as a single compound value.

Swift also introduces optional types,
which are a way to handle the absence of a value.
Optionals are a way to say either “there *is* a value, and it equals *x*”;
or “there *isn't* a value at all”.
Optionals are similar to working with ``nil`` in Objective-C,
but in a way that works for any type, not just classes.
Optionals are safer and more expressive than ``nil`` pointers in Objective-C,
and are at the heart of many of Swift's most powerful features.

Optionals are an example of the fact that Swift is a *type safe* language.
This means that it helps you to be clear about the types of values your code can work with.
If part of your code expects a ``String``,
type safety means that you can't accidentally pass it an ``Int`` by mistake.
This enables you to catch and fix errors as early as possible in the development process.

.. _TheBasics_ConstantsAndVariables:

Constants and Variables
-----------------------

Constants and variables are a way to give a name
(such as ``maximumNumberOfLoginAttempts`` or ``welcomeMessage``)
to a value of a particular type
(such as the number ``10``, or the string ``"Hello"``).
A :newTerm:`constant` cannot be changed once it is set, whereas
a :newTerm:`variable` can be set to a different value in the future.

.. _TheBasics_DeclaringConstantsAndVariables:

Declaring Constants and Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Constants and variables must be declared before they are used.
You declare constants with the ``let`` keyword,
and variables with the ``var`` keyword.
Here's an example of how constants and variables can be used
to track the number of login attempts a user has made:

.. testcode:: constantsAndVariables

   -> let maximumNumberOfLoginAttempts = 10
   << // maximumNumberOfLoginAttempts : Int = 10
   -> var currentLoginAttempt = 0
   << // currentLoginAttempt : Int = 0

This can be read as:

“Declare a new constant called ``maximumNumberOfLoginAttempts``,
and give it a value of ``10``.
Then, declare a new variable called ``currentLoginAttempt``,
and give it an initial value of ``0``.”

In this example,
the maximum number of allowed login attempts is declared as a constant,
because the maximum value never changes.
The current login attempt counter is declared as a variable,
because this value must be incremented after each failed login attempt.

You can declare multiple constants or multiple variables on a single line,
separated by commas:

.. testcode:: multipleDeclarations

   -> var x = 0.0, y = 0.0, z = 0.0
   << // x : Double = 0.0
   << // y : Double = 0.0
   << // z : Double = 0.0

.. note::

   If a stored value in your code is not going to change,
   it should always be declared as a constant with the ``let`` keyword.
   Variables should only be used for
   storing values that need to be able to change.

.. TODO: I need to mention that globals are lazily initialized somewhere.
   Probably not here, but somewhere.

.. _TheBasics_TypeAnnotations:

Type Annotations
~~~~~~~~~~~~~~~~

You can provide a :newTerm:`type annotation` when you declare a constant or variable,
to be clear about the kind of values the constant or variable can store.
Type annotations are written by placing a colon after the constant or variable name,
followed by a space, followed by the name of the type to use.

This example provides a type annotation for a variable called ``welcomeMessage``,
to indicate that the variable can store ``String`` values:

.. testcode:: typeAnnotations
   :compile: true

   -> var welcomeMessage: String
   >> welcomeMessage = "hello"
   >> println(welcomeMessage)
   << hello

The colon in the declaration means *“…of type…,”*
so this can be read as:

“Declare a variable called ``welcomeMessage`` that is of type ``String``.”

The phrase “of type ``String``” means “can store any ``String`` value.”
Think of it as meaning “the type of thing” (or “the kind of thing”) that can be stored.

The ``welcomeMessage`` variable can now be set to any string value without error:

.. testcode:: constantsAndVariables

   >> var welcomeMessage = "Hello"
   << // welcomeMessage : String = "Hello"
   -> welcomeMessage = "Hello"

.. note::

   It is rare that you need to write type annotations in practice.
   If you provide an initial value for a constant or variable at the point that it is defined,
   Swift can almost always infer the type to be used for that constant or variable,
   as described in :ref:`TheBasics_TypeInference`.
   In the ``welcomeMessage`` example above, no initial value is provided,
   and so the type of the ``welcomeMessage`` variable is specified with a type annotation
   rather than being inferred from an initial value.

.. _TheBasics_NamingConstantsAndVariables:

Naming Constants and Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can use almost any character you like for constant and variable names,
including Unicode characters:

.. testcode:: constantsAndVariables

   -> let π = 3.14159
   << // π : Double = 3.14159
   -> let 你好 = "你好世界"
   << // 你好 : String = "你好世界"
   -> let 🐶🐮 = "dogcow"
   << // 🐶🐮 : String = "dogcow"

Constant and variable names cannot contain
mathematical symbols, arrows, private-use (or invalid) Unicode code points,
or line- and box-drawing characters.
They also cannot begin with a number,
although numbers may be included elsewhere within the name.

Once you've declared a constant or variable of a certain type,
you can't redeclare it again with the same name,
or change it to store values of a different type.
Nor can you change a constant into a variable,
or a variable into a constant.

.. note::

   If you need to give a constant or variable the same name as a reserved Swift keyword,
   you can do so by surrounding the keyword with back ticks (`````) when using it as a name.
   However, you should avoid using keywords as names unless you have absolutely no choice.

.. QUESTION: I've deliberately not given an example here,
   because I don't want to suggest that such an example is
   a good example of when you *should* use a keyword as a name.
   Is this the right approach to take?

The value of an existing variable can be changed to another value of a compatible type.
In this example, the value of ``friendlyWelcome`` is changed from
``"hello, world"`` to ``"👋, 🌎"``:

.. testcode:: constantsAndVariables

   -> var friendlyWelcome = "hello, world"
   << // friendlyWelcome : String = "hello, world"
   /> friendlyWelcome is \"\(friendlyWelcome)\"
   </ friendlyWelcome is "hello, world"
   -> friendlyWelcome = "👋, 🌎"
   /> friendlyWelcome is now \"\(friendlyWelcome)\"
   </ friendlyWelcome is now "👋, 🌎"

Unlike a variable, the value of a constant cannot be changed once it is set.
Attempting to do so is reported as an error when your code is compiled:

.. testcode:: constantsAndVariables

   -> let languageName = "Swift"
   << // languageName : String = "Swift"
   -> languageName = "Swift++"
   // this is a compile-time error – languageName cannot be changed
   !! <REPL Input>:1:14: error: cannot assign to 'let' value 'languageName'
   !! languageName = "Swift++"
   !! ~~~~~~~~~~~~ ^

.. QUESTION: should this section mention that Swift-clashing names
   can be qualified with a backtick (e.g. let `protocol` = 1)?
   It's of a kind with the contents of this section, but it's pretty damn niche…

.. _TheBasics_PrintingConstantsAndVariables:

Printing Constants and Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can print the current value of a constant or variable with the ``println`` function:

.. testcode:: constantsAndVariables

   -> println(friendlyWelcome)
   <- 👋, 🌎

``println`` is a global function that prints a value,
followed by a line break, to an appropriate output.
If you are working in Xcode, for example,
``println`` prints its output in Xcode's “console” pane.
(A second function, ``print``, performs the same task
without appending a line break to the end of the value to be printed.)

.. QUESTION: have I referred to Xcode's console correctly here?
   Should I mention other output streams, such as the REPL / playgrounds?

.. NOTE: this is a deliberately simplistic description of what you can do with println().
   It will be expanded later on.

.. QUESTION: is this *too* simplistic?
   Strictly speaking, you can't print the value of *any* constant or variable –
   you can only print values of types for which String has a constructor.

The ``println`` function prints any ``String`` value you pass to it:

.. testcode:: constantsAndVariables

   -> println("This is a string")
   <- This is a string

.. _TheBasics_StringInterpolation:

String Interpolation
____________________

The ``println`` function can print more complex logging messages,
in a similar manner to Cocoa's ``NSLog`` function.
These messages can include the current values of constants and variables.

Swift uses :newTerm:`string interpolation` to include a constant or variable's name
as a placeholder in a longer string,
and to prompt Swift to replace it with the current value of that constant or variable.
The constant or variable's name should be wrapped by parentheses,
and escaped with a backslash before the opening parenthesis:

.. testcode:: constantsAndVariables

   -> println("The current value of friendlyWelcome is \(friendlyWelcome)")
   <- The current value of friendlyWelcome is 👋, 🌎

.. note::

   The full set of options you can use with string interpolation
   are described in :ref:`StringsAndCharacters_StringInterpolation`.

.. _TheBasics_Comments:

Comments
--------

Comments are a way to include non-executable text in your code,
as a note or reminder to yourself.
Comments are ignored by the Swift compiler when your code is compiled.

Comments in Swift are very similar to comments in C.
Single-line comments begin with two forward-slashes (``//``):

.. testcode:: comments
   :compile: true

   -> // This is a comment

You can also write multi-line comments,
which start with a forward-slash followed by an asterisk (``/*``),
and end with an asterisk followed by a forward-slash (``*/``):

.. testcode:: comments
   :compile: true

   -> /* This is also a comment,
      but written over multiple lines */

Unlike C, multi-line comments can be nested inside other multi-line comments.
You write nested comments by starting a multi-line comment block,
and then starting a second multi-line comment within the first block.
The second block is then closed, followed by the first block:

.. testcode:: comments
   :compile: true

   -> /* This is the start of the first multi-line comment
         /* This is the second, nested multi-line comment */
      This is the end of the first multi-line comment */

Nested multi-line comments enable you to comment out large blocks of code quickly and easily,
even if the code already contains multi-line comments.

.. _TheBasics_Semicolons:

Semicolons
----------

Unlike many other languages,
Swift does not require you to write a semicolon (``;``) after each statement in your code
(although you can do so if you wish).
Semicolons *are* required, however,
if you want to write multiple separate statements on a single line:

.. testcode:: semiColons

   -> let cat = "🐱"; let dog = "🐶"
   << // cat : String = "🐱"
   << // dog : String = "🐶"

.. _TheBasics_Integers:

Integers
--------

:newTerm:`Integers` are whole numbers with no fractional component,
such as ``42`` and ``-23``.
Integers are either :newTerm:`signed` (which means they can be positive, zero or negative),
or :newTerm:`unsigned` (which means they can only be positive or zero).

Swift provides integers in signed and unsigned forms in sizes of
8, 16, 32, and 64 bits.
These integers follow a naming convention similar to C,
in that an 8-bit unsigned integer is of type ``UInt8``,
and a 32-bit signed integer is of type ``Int32``.
Like all types in Swift, these integer types have capitalized names.

.. _TheBasics_Int:

Int
~~~

In most cases, there's no need to pick a specific size of integer to use in your code.
Swift provides an additional integer type, ``Int``,
which has the same size as the current platform's native word size:

* On a 32-bit platform, ``Int`` is the same size as ``Int32``.
* On a 64-bit platform, ``Int`` is the same size as ``Int64``.

Unless you need to work with a specific size of integer,
always use ``Int`` for integer values in your code.
This aids code consistency and interoperability.
Even on 32-bit platforms, ``Int`` can store any value between ``-2,147,483,648`` and ``2,147,483,647``,
and is large enough for many integer ranges.

.. _TheBasics_UInt:

UInt
~~~~

Swift also provides an unsigned integer type, ``UInt``,
which has the same size as the current platform's native word size:

* On a 32-bit platform, ``UInt`` is the same size as ``UInt32``.
* On a 64-bit platform, ``UInt`` is the same size as ``UInt64``.

.. note::

   Use ``UInt`` only when you specifically need
   an unsigned integer type with the same size as the platform's native word size.
   If this is not the case, ``Int`` is preferred,
   even when the values to be stored are known to be non-negative.
   A consistent use of ``Int`` for integer values aids code interoperability,
   avoids the need to convert between different number types,
   and provides consistency when you use type inference, as described below.

.. _TheBasics_FloatingPointNumbers:

Floating-Point Numbers
----------------------

:newTerm:`Floating-point numbers` are numbers with a fractional component,
such as ``3.14159``, ``0.1``, and ``-273.15``.

Floating-point types can represent a much wider range of values than integer types,
and can store numbers that are much larger or smaller than can be stored in an ``Int``.
Swift provides two signed floating-point number types:

* ``Double``, which represents a 64-bit floating-point number,
  and should be used when floating-point values need to be very large or particularly precise
* ``Float``, which represents a 32-bit floating-point number,
  and should be used when floating-point values do not require 64-bit precision

.. note::

   ``Double`` has a precision of at least 15 digits,
   whereas the precision of ``Float`` can be as little as 6 digits.
   The appropriate floating-point type to use depends on the nature and range of
   values you need to work with in your code.

.. TODO: mention infinity, -infinity etc.

.. _TheBasics_TypeInference:

Type Inference
--------------

Swift is a :newTerm:`type safe` language.
This means that it encourages you to be clear about
the types of values your code can work with.
If part of your code expects a ``String``,
type safety means that you can't accidentally pass it an ``Int`` by mistake.

Because Swift is type safe,
it performs :newTerm:`type checks` when compiling your code,
and flags any mismatched types as errors.
This enables you to catch and fix errors as early as possible in the development process.

Type-checking helps avoid accidental errors when you're working with different types of values.
However, this doesn't mean that you have to specify the type of
every constant and variable that you declare.
If you don't specify the type of value you need,
Swift uses :newTerm:`type inference` to work out the appropriate type.
Type inference enables a compiler to automatically deduce
the type of a particular expression when it compiles your code,
simply by examining the values you provide.

Because of type inference, Swift requires far fewer type declarations
than languages such as C or Objective-C.
Constants and variables are still explicitly typed,
but much of the work of specifying their type is done for you.

Type inference is particularly useful
when you declare a constant or variable with an initial value.
This is often done by assigning a :newTerm:`literal value` (or :newTerm:`literal`)
to the constant or variable at the point that you declare it.
(A literal value is a one-off value that appears directly in your source code,
such as ``42`` and ``3.14159`` in the examples below.)

For example, if you assign a literal value of ``42`` to a new constant
without saying what type it is,
Swift infers that you want the constant to be an ``Int``,
because you have initialized it with a number that looks like an integer:

.. testcode:: typeInference

   -> let meaningOfLife = 42
   << // meaningOfLife : Int = 42
   // meaningOfLife is inferred to be of type Int

Likewise, if you don't specify a type for a floating-point literal,
Swift assumes that you want to create a ``Double``:

.. testcode:: typeInference

   -> let pi = 3.14159
   << // pi : Double = 3.14159
   // pi is inferred to be of type Double

Swift always chooses ``Double`` (rather than ``Float``)
when inferring the type of floating-point numbers.

If you combine integer and floating-point literals in an expression,
a type of ``Double`` will be inferred from the context:

.. testcode:: typeInference

   -> let anotherPi = 3 + 0.14159
   << // anotherPi : Double = 3.14159
   // anotherPi is also inferred to be of type Double

The literal value of ``3`` has no explicit type in and of itself,
and so an appropriate output type of ``Double`` is inferred
from the presence of a floating-point literal as part of the addition.

.. _TheBasics_NumericLiterals:

Numeric Literals
----------------

Integer literals can be written as:

* A :newTerm:`decimal` number, with no prefix
* A :newTerm:`binary` number, with a ``0b`` prefix
* An :newTerm:`octal` number, with a ``0o`` prefix
* A :newTerm:`hexadecimal` number, with a ``0x`` prefix

All of these integer literals have a decimal value of ``17``:

.. testcode:: numberLiterals

   -> let decimalInteger = 17
   << // decimalInteger : Int = 17
   -> let binaryInteger = 0b10001      // 17 in binary notation
   << // binaryInteger : Int = 17
   -> let octalInteger = 0o21         // 17 in octal notation
   << // octalInteger : Int = 17
   -> let hexadecimalInteger = 0x11     // 17 in hexadecimal notation
   << // hexadecimalInteger : Int = 17

Floating-point literals can be decimal (with no prefix),
or hexadecimal (with a ``0x`` prefix).
They must always have a number (or hexadecimal number) on both sides of the decimal point.
They can also have an optional :newTerm:`exponent`,
indicated by an uppercase or lowercase ``e`` for decimal floats,
or an uppercase or lowercase ``p`` for hexadecimal floats.

For decimal numbers with an exponent of ``exp``,
the base number is multiplied by 10\ :superscript:`exp`:

* ``1.25e2`` means 1.25 ⨉ 10\ :superscript:`2`, or ``125.0``.
* ``1.25e-2`` means 1.25 ⨉ 10\ :superscript:`-2`, or ``0.0125``.

For hexadecimal numbers with an exponent of ``exp``,
the base number is multiplied by 2\ :superscript:`exp`:

* ``0xFp2`` means 15 ⨉ 2\ :superscript:`2`, or ``60.0``.
* ``0xFp-2`` means 15 ⨉ 2\ :superscript:`-2`, or ``3.75``.

All of these floating-point literals have a decimal value of ``12.1875``:

.. testcode:: numberLiterals

   -> let decimalDouble = 12.1875
   << // decimalDouble : Double = 12.1875
   -> let exponentDouble = 1.21875e1
   << // exponentDouble : Double = 12.1875
   -> let hexadecimalDouble = 0xC.3p0
   << // hexadecimalDouble : Double = 12.1875

Numeric literals can contain extra formatting to make them easier to read.
Both integers and floats can be padded with extra zeroes
and can contain underscores to help with readability.
Neither type of formatting affects the underlying value of the literal:

.. testcode:: numberLiterals

   -> let paddedDouble = 000123.456
   << // paddedDouble : Double = 123.456
   -> let oneMillion = 1_000_000
   << // oneMillion : Int = 1000000
   -> let justOverOneMillion = 1_000_000.000_000_1
   << // justOverOneMillion : Double = 1000000.0000001

.. _TheBasics_NumericTypeConversion:

Numeric Type Conversion
-----------------------

Use the ``Int`` type for all general-purpose integer constants and variables in your code,
even if they are known to be non-negative.
Using the default integer type in everyday situations means that
integer constants and variables are immediately interoperable in your code
and will match the inferred type for integer literal values.

Use other integer types only when they are are specifically needed for the task at hand,
because of explicitly-sized data from an external source,
or for performance, memory usage, or other necessary optimization.
Using explicitly-sized types in these situations
helps to catch any accidental value overflows
and implicitly documents the nature of the data being used.

.. _TheBasics_IntegerBounds:

Integer Bounds
~~~~~~~~~~~~~~

You can access the minimum and maximum values of each integer type
with its ``min`` and ``max`` properties:

.. testcode:: constantsAndVariables

   -> let minValue = UInt8.min  // minValue is equal to 0, and is of type UInt8
   << // minValue : UInt8 = 0
   -> let maxValue = UInt8.max  // maxValue is equal to 255, and is of type UInt8
   << // maxValue : UInt8 = 255

The values of these properties are of the appropriate-sized number type
(such as ``UInt8`` in the example above)
and can therefore be used in expressions alongside other values of the same type.

.. _TheBasics_IntegerConversion:

Integer Conversion
~~~~~~~~~~~~~~~~~~

The range of numbers that can be stored in an integer constant or variable
is different for each numeric type.
An ``Int8`` constant or variable can store numbers between ``-128`` and ``127``,
whereas a ``UInt8`` constant or variable can store numbers between ``0`` and ``255``.
A number that will not fit into a constant or variable of a sized integer type
is reported as an error when your code is compiled:

.. testcode:: constantsAndVariablesOverflowError

   -> let cannotBeNegative: UInt8 = -1
   !! <REPL Input>:1:31: error: integer literal overflows when stored into 'UInt8'
   !! let cannotBeNegative: UInt8 = -1
   !!                        ^
   // UInt8 cannot store negative numbers, and so this will report an error
   -> let tooBig: Int8 = Int8.max + 1
   !! <REPL Input>:1:29: error: arithmetic operation '127 + 1' (on type 'Int8') results in an overflow
   !! let tooBig: Int8 = Int8.max + 1
   !!                      ^
   // Int8 cannot store a number larger than its maximum value,
   // and so this will also report an error

Because each numeric type can store a different range of values,
numeric type conversion is something you must opt in to on a case-by-case basis.
This opt-in approach avoids accidental errors
and helps make type conversion intentions explicit in your code.

To convert one specific number type to another,
you initialize a new number of the desired type with the existing value:

.. testcode:: typeConversion

   -> let twoThousand: UInt16 = 2_000
   << // twoThousand : UInt16 = 2000
   -> let one: UInt8 = 1
   << // one : UInt8 = 1
   -> let twoThousandAndOne = twoThousand + UInt16(one)
   << // twoThousandAndOne : UInt16 = 2001

The constant ``twoThousand`` is of type ``UInt16``,
whereas the constant ``one`` is of type ``UInt8``.
They cannot be added together directly,
because they are not of the same type.
Instead, this code calls ``UInt16(one)`` to create a new ``UInt16`` initialized with the value of ``one``,
and uses this value in place of the original.
Because both sides of the addition are now of type ``UInt16``,
the addition is allowed.
The output constant (``twoThousandAndOne``) is inferred to be of type ``UInt16``,
because it is the sum of two ``UInt16`` values.

``SomeType(ofInitialValue)`` is the default way to call the initializer of a Swift type
and pass in an initial value.
Behind the scenes, ``UInt16`` has an initializer that accepts a ``UInt8`` value,
and so this initializer is used to make a new ``UInt16`` from an existing ``UInt8``.
You can't pass in *any* type here, however –
it has to be a type for which ``UInt16`` provides an initializer.
Extending existing types to provide initializers that accept new types
(including your own type definitions)
is covered in :doc:`Extensions`.

.. _TheBasics_IntegerAndFloatingPointConversion:

Integer and Floating-Point Conversion
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Conversions between integer and floating-point numeric types must be made explicit:

.. testcode:: typeConversion

   -> let three = 3
   << // three : Int = 3
   -> let pointOneFourOneFiveNine = 0.14159
   << // pointOneFourOneFiveNine : Double = 0.14159
   -> let pi = Double(three) + pointOneFourOneFiveNine
   << // pi : Double = 3.14159
   /> pi equals \(pi), and is inferred to be of type Double
   </ pi equals 3.14159, and is inferred to be of type Double

Here, the value of the constant ``three`` is used to create a new value of type ``Double``,
so that both sides of the addition are of the same type.
Without this conversion in place, the addition would not be allowed.

The reverse is also true for floating-point to integer conversion,
in that an integer type can be initialized with a ``Double`` or ``Float`` value:

.. testcode:: typeConversion

   -> let integerPi = Int(pi)
   << // integerPi : Int = 3
   /> integerPi equals \(integerPi), and is inferred to be of type Int
   </ integerPi equals 3, and is inferred to be of type Int

Floating-point values are always truncated when used to initialize a new integer value in this way.
This means that ``4.75`` becomes ``4``, and ``-3.9`` becomes ``-3``.

.. FIXME: negative floating-point numbers cause an overflow when used
   to initialize an unsigned integer type.
   This has been filed as rdar://problem/16206455,
   and this section may need updating based on the outcome of that Radar.

.. note::

   The rules for combining numeric constants and variables are different from
   the rules for numeric literals.
   The literal value ``3`` can be added directly to the literal value ``0.14159``,
   because number literals do not have an explicit type in and of themselves.
   Their type is inferred only at the point that they are evaluated by the compiler.

.. NOTE: this section on explicit conversions could be included in the Operators section.
   I think it's more appropriate here, however,
   and helps to reinforce the “just use Int” message.

.. _TheBasics_TypeAliases:

Type Aliases
------------

:newTerm:`Type aliases` define an alternative name for an existing type.
You define type aliases with the ``typealias`` keyword.

Type aliases are useful when you want to refer to an existing type
by a name that is contextually more appropriate,
such as when working with data of a specific size from an external source:

.. testcode:: typeAliases

   -> typealias AudioSample = UInt16

Once you define a type alias,
you can use the alias anywhere you might use the original name:

.. testcode:: typeAliases

   -> var maxAmplitudeFound = AudioSample.min
   << // maxAmplitudeFound : UInt16 = 0
   /> maxAmplitudeFound is now \(maxAmplitudeFound)
   </ maxAmplitudeFound is now 0

Here, ``AudioSample`` is defined as an alias for ``UInt16``.
Because it is an alias,
the call to ``AudioSample.min`` actually calls ``UInt16.min``,
which provides an initial value of ``0`` for the ``maxAmplitudeFound`` variable.

.. note::

   Type aliases do not actually define a new type in Swift.
   They are simply an alternative name for an existing type.
   In the example above,
   ``maxAmplitudeFound`` is of type ``UInt16``, not ``AudioSample``.

.. _TheBasics_Booleans:

Booleans
--------

Swift has a basic :newTerm:`Boolean` type, called ``Bool``.
Boolean values are referred to as :newTerm:`logical`,
because they can only ever be true or false.
Swift provides two Boolean literal values,
``true`` and ``false``:

.. testcode:: booleans

   -> let orangesAreOrange = true
   << // orangesAreOrange : Bool = true
   -> let turnipsAreDelicious = false
   << // turnipsAreDelicious : Bool = false

The types of ``orangesAreOrange`` and ``turnipsAreDelicious``
have been inferred as ``Bool`` from the fact that
they were initialized with Boolean literal values.
As with ``Int`` and ``Double`` above,
you don't need to declare constants or variables as ``Bool``
if you set them to ``true`` or ``false`` as soon as you create them.
Type inference helps to make Swift code much more concise and readable
when initializing constants or variables with other values whose type is already known.

Boolean values are particularly useful when you work with conditional statements
such as the ``if``-``else`` statement:

.. testcode:: booleans

   -> if turnipsAreDelicious {
         println("Mmm, tasty turnips!")
      } else {
         println("Eww, turnips are horrible.")
      }
   <- Eww, turnips are horrible.

Conditional statements such as ``if``-``else`` are covered in more detail in :doc:`ControlFlow`.

Swift's type safety means that non-Boolean values cannot be substituted for ``Bool``.
The following example reports a compile-time error:

.. testcode:: booleansNotLogicValue

   -> let i = 1
   << // i : Int = 1
   -> if i {
         // this example will not compile, and will report an error
      }
   !! <REPL Input>:1:4: error: type 'Int' does not conform to protocol 'LogicValue'
   !! if i {
   !!   ^

However, it is valid to say:

.. testcode:: booleansIsLogicValue

   -> let i = 1
   << // i : Int = 1
   -> if i == 1 {
         // this example will compile successfully
      }

The result of the ``i == 1`` comparison is of type ``Bool``,
and so this second example passes the type-check.
(Comparisons like ``i == 1`` are discussed in :doc:`BasicOperators`.)

As with other examples of type safety in Swift,
this approach avoids accidental errors,
and ensures that the intention of a particular section of code is always clear.

.. note::

   Strictly speaking, an ``if``-``else`` statement's condition expression
   can be of any type that conforms to the ``LogicValue`` protocol.
   ``Bool`` is one example of a type that conforms to this protocol,
   but there are others, such as optionals, described below.
   The ``LogicValue`` protocol is described in more detail in :doc:`Protocols`.

.. TODO: I'm not quite happy with this yet.
   Introducing the LogicValue protocol at this early stage is a bit overkill.
   I'd like to revisit this if time permits, and maybe move this to Control Flow.

.. TODO: the LogicValue protocol is not yet described in the Protocols chapter.

.. _TheBasics_Tuples:

Tuples
------

:newTerm:`Tuples` group multiple values into a single compound value.
The values within a tuple can be of any type,
and do not have to be of the same type as each other.

Here's an example of a tuple:

.. testcode:: tuples

   -> let http404Error = (404, "Not Found")
   << // http404Error : (Int, String) = (404, "Not Found")
   /> http404Error is of type (Int, String), and equals (\(http404Error.0), \"\(http404Error.1)\")
   </ http404Error is of type (Int, String), and equals (404, "Not Found")

``(404, "Not Found")`` is a tuple that describes an *HTTP status code*.
An HTTP status code is a special value returned by a web server whenever you request a web page.
A status code of ``404 Not Found`` is returned if you request a webpage that doesn't exist.

The ``(404, "Not Found")`` tuple groups together an ``Int`` and a ``String``
to give the HTTP status code two separate values:
a number, and a human-readable description.
It can be described as “a tuple of type ``(Int, String)``”.

You can create tuples from whatever permutation of types you like,
and they can contain as many different types as you like.
There's nothing stopping you from having
a tuple of type ``(Int, Int, Int)``, or ``(String, Bool)``,
or indeed any other permutation you require.

You can :newTerm:`decompose` a tuple's contents into separate constants or variables,
which can then be accessed as usual:

.. testcode:: tuples

   -> let (statusCode, statusMessage) = http404Error
   << // (statusCode, statusMessage) : (Int, String) = (404, "Not Found")
   -> println("The status code is \(statusCode)")
   <- The status code is 404
   -> println("The status message is \(statusMessage)")
   <- The status message is Not Found

If you only need some of the tuple's values,
you can ignore parts of the tuple with an underscore (``_``)
when you decompose the tuple:

.. testcode:: tuples

   -> let (justTheStatusCode, _) = http404Error
   << // (justTheStatusCode, _) : (Int, String) = (404, "Not Found")
   -> println("The status code is \(justTheStatusCode)")
   <- The status code is 404

Alternatively,
you can access the individual element values in a tuple using index numbers starting at zero:

.. testcode:: tuples

   -> println("The status code is \(http404Error.0)")
   <- The status code is 404
   -> println("The status message is \(http404Error.1)")
   <- The status message is Not Found

.. _TheBasics_NamedTupleElements:

Named Tuple Elements
~~~~~~~~~~~~~~~~~~~~

You can name the individual elements in a tuple when the tuple is defined:

.. testcode:: tuples

   -> let http200Status = (statusCode: 200, description: "OK")
   << // http200Status : (statusCode: Int, description: String) = (200, "OK")

If you name the elements in a tuple,
you can use the element names to access the values of those elements:

.. testcode:: tuples

   -> println("The status code is \(http200Status.statusCode)")
   <- The status code is 200
   -> println("The status message is \(http200Status.description)")
   <- The status message is OK

Tuples are particularly useful as the return values of functions.
A function that tries to retrieve a web page might return the ``(Int, String)`` tuple type
to describe the success or failure of the page retrieval.
By returning a tuple with two distinct values,
each of a different type,
the function provides more useful information about its outcome
than if it could only return a single value of a single type.
Functions are described in detail in :doc:`Functions`.

.. note::

   Tuples are useful for temporary groups of related values.
   They are not suited to the creation of complex data structures.
   If your data structure is likely to persist beyond a temporary scope,
   model it as a class or structure, rather than as a tuple,
   See :doc:`ClassesAndStructures`.

.. _TheBasics_Optionals:

Optionals
---------

You use :newTerm:`optionals` in situations where a value may be absent.
An optional says:

* There *is* a value, and it equals *x*

…or…

* There *isn't* a value at all

.. note::

   This concept doesn't exist in C or Objective-C.
   The nearest thing in Objective-C is
   the ability to return ``nil`` from a method that would otherwise return an object,
   with ``nil`` meaning “the absence of a valid object.”
   However, this only works for objects – it doesn't work for
   structs, basic C types, or enumeration values.
   For these types,
   Objective-C methods typically return a special value (such as ``NSNotFound``) to indicate the absence of a value.
   This assumes that the method's caller knows there is a special value to test against,
   and remembers to check for it.
   Swift's optionals let you indicate the absence of a value for *any type at all*,
   without the need for special constants or ``nil`` tests.

Here's an example.
Swift's ``String`` type has a method called ``toInt``,
which tries to convert a ``String`` value into an ``Int`` value.
However, not every string can be converted into an integer.
The string ``"123"`` can be converted into the numeric value ``123``,
but the string ``"hello, world"`` does not have an obvious numeric value to convert to.

The example below shows how to use ``toInt`` to try and convert a ``String`` into an ``Int``:

.. testcode:: optionals

   -> let possibleNumber = "123"
   << // possibleNumber : String = "123"
   -> let convertedNumber = possibleNumber.toInt()
   << // convertedNumber : Int? = <unprintable value>
   // convertedNumber is inferred to be of type "Int?", or "optional Int"

Because the ``toInt`` method might fail,
it returns an *optional* ``Int``, rather than an ``Int``.
An optional ``Int`` is written as ``Int?``, not ``Int``.
The question mark indicates that the value it contains is optional,
meaning that it might contain *some* ``Int`` value,
or it might contain *no value at all*.
(It can't contain anything else, such as a ``Bool`` value or a ``String`` value –
it's either an ``Int``, or it's nothing at all.)

.. _TheBasics_OptionalsIfElse:

If-Else
~~~~~~~

You can use an ``if``-``else`` statement to find out whether an optional contains a value.
If an optional does have a value, it evaluates to ``true``;
if it has no value at all, it evaluates to ``false``.

Once you're sure that the optional *does* contain a value,
you can access its underlying value
by adding an exclamation mark (``!``) to the end of the optional's name.
The exclamation mark effectively says,
“I know that this optional definitely has a value – please use it.”

.. testcode:: optionals

   -> if convertedNumber {
         println("\(possibleNumber) has an integer value of \(convertedNumber!)")
      } else {
         println("\(possibleNumber) could not be converted to an integer")
      }
   <- 123 has an integer value of 123

``if``-``else`` statements are described in more detail in :doc:`ControlFlow`.

.. note::

   Trying to use ``!`` to access a non-existent optional value triggers
   an unrecoverable runtime error.

.. _TheBasics_OptionalBinding:

Optional Binding
~~~~~~~~~~~~~~~~

:newTerm:`Optional binding` is a convenient way to find out if an optional contains a value,
and to make that value available as a constant or variable if it exists.
Optional binding can be used with ``if``-``else`` and ``while`` statements
to check for a value inside the optional,
and to extract that value into a constant or variable,
as part of a single action.
(``if``-``else`` and ``while`` statements are described in more detail in :doc:`ControlFlow`.)

Optional bindings for the ``if``-``else`` statement are written in the following form:

.. syntax-outline::

   if let <#constantName#> = <#someOptional#> {
      <#statements#>
   }

The ``possibleNumber`` example from the ``if``-``else`` section above
can be rewritten to use optional binding:

.. testcode:: optionals

   -> if let actualNumber = possibleNumber.toInt() {
         println("\(possibleNumber) has an integer value of \(actualNumber)")
      } else {
         println("\(possibleNumber) could not be converted to an integer")
      }
   <- 123 has an integer value of 123

As in the ``if``-``else`` section above,
this example uses the ``toInt`` method from ``String``
to try and convert ``"123"`` into an ``Int``.
It then prints a message to indicate if the conversion was successful.

``if let actualNumber = possibleNumber.toInt()`` can be read as:

“If the optional ``Int`` returned by ``possibleNumber.toInt`` contains a value,
set a new constant called ``actualNumber`` to the value contained in the optional.”

If the conversion is successful,
the ``actualNumber`` constant becomes available for use within
the first branch of the ``if``-``else`` statement.
It has already been initialized with the value contained *within* the optional,
and so there is no need to use the ``!`` suffix to access its value.
In this example, ``actualNumber`` is simply used to print the result of the conversion.

You can use both constants and variables with optional binding.
If you wanted to manipulate the value of ``actualNumber``
within the first block of the ``if``-``else`` statement,
you could write ``if var actualNumber`` instead,
and the value contained within the optional
would be made available as a variable rather than a constant.

.. note::

   Constants or variables created with optional binding
   are only available within the code block following their creation,
   as in the first branch of the ``if``-``else`` statement above.
   If you want to work with the optional's value outside of this code block,
   you should declare a constant or variable yourself
   before the ``if``-``else`` statement begins.

.. _TheBasics_Nil:

nil
~~~

You can set an optional variable back to a valueless state
by assigning it the special value ``nil``:

.. testcode:: optionals

   -> var serverResponseCode: Int? = 404
   << // serverResponseCode : Int? = <unprintable value>
   /> serverResponseCode contains an actual Int value of \(serverResponseCode!)
   </ serverResponseCode contains an actual Int value of 404
   -> serverResponseCode = nil
   // serverResponseCode now contains no value

.. note::

   ``nil`` cannot be used with non-optional constants and variables.
   If a constant or variable in your code needs to be able to cope with
   the absence of a value under certain conditions,
   it should always be declared as an optional value of the appropriate type.

If you define an optional constant or variable without providing a default value,
the constant or variable is automatically set to ``nil`` for you:

.. testcode:: optionals

   -> var surveyAnswer: String?
   << // surveyAnswer : String? = <unprintable value>
   // surveyAnswer is automatically set to nil

.. note::

   Swift's ``nil`` is not the same as ``nil`` in Objective-C.
   In Objective-C, ``nil`` is a pointer to a non-existent object.
   In Swift, ``nil`` is not a pointer – it is the absence of a value of a certain type.
   Optionals of *any* type can be set to ``nil``, not just object types.

.. _TheBasics_ImplicitlyUnwrappedOptionals:

Implicitly Unwrapped Optionals
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

As described above,
optionals indicate that a constant or variable is allowed to have “no value”.
Optionals can be checked with an ``if``-``else`` statement to see if a value exists,
and can be conditionally unwrapped with optional binding
to access the optional's value if it does exist.

In a few cases, however,
it is clear from a program's structure that an optional will *always* has a value,
once its value is first set.
In these cases, it is useful to remove the need
to check and unwrap the optional's value every time it is accessed,
because it can confidently be assumed to have a value all of the time.

These kinds of optionals are defined as :newTerm:`implicitly unwrapped optionals`,
and are written by placing an exclamation mark (``String!``)
rather than a question mark (``String?``) after the type that you want to make optional.

Implicitly unwrapped optionals are useful when
an optional's value is confirmed to exist immediately after the optional is first defined,
and can definitely be assumed to exist at every point thereafter.
The primary use of implicitly unwrapped optionals in Swift is during class initialization,
as described in :ref:`Initialization_ImplicitlyUnwrappedOptionalProperties`.

An implicitly unwrapped optional is a normal optional behind the scenes,
but can also be used like a non-optional value,
without the need to unwrap the optional value each time it is accessed.
The following example shows the difference in behavior between
an optional ``String`` and an implicitly unwrapped optional ``String``:

.. testcode:: implicitlyUnwrappedOptionals

   -> let possibleString: String? = "An optional string."
   << // possibleString : String? = <unprintable value>
   -> println(possibleString!) // requires an exclamation mark to access its value
   <- An optional string.
   ---
   -> let assumedString: String! = "An implicitly unwrapped optional string."
   << // assumedString : String! = String!(<unprintable value>)
   -> println(assumedString)  // no exclamation mark is needed to access its value
   <- An implicitly unwrapped optional string.

You can think of an implicitly unwrapped optional as
giving permission for the optional to be unwrapped automatically whenever it is used.
Rather than placing an exclamation mark after the optional's name each time you use it,
you place an exclamation mark after the optional's type when you declare it.

.. note::

   If you try and access an implicitly unwrapped optional
   when it does not contain a value,
   you will trigger an unrecoverable runtime error.
   This is exactly the same as if you place an exclamation mark
   after a normal optional that does not contain a value.

You can still treat an implicitly unwrapped optional like a normal optional,
to check if it contains a value:

.. testcode:: implicitlyUnwrappedOptionals

   -> if assumedString {
         println(assumedString)
      }
   <- An implicitly unwrapped optional string.

You can also use an implicitly unwrapped optional with optional binding,
to check and unwrap its value in a single statement:

.. testcode:: implicitlyUnwrappedOptionals

   -> if let definiteString = assumedString {
         println(definiteString)
      }
   <- An implicitly unwrapped optional string.

.. note::

   Implicitly unwrapped optionals should not be used when there is a possibility of
   a variable becoming ``nil`` at a later point.
   Always use a normal optional type if you need to check for a ``nil`` value
   during the lifetime of a variable.

.. _TheBasics_Assertions:

Assertions
----------

Optionals enable you to check for values that may or may not exist,
and to write code that copes gracefully with the absence of a value.
In some cases, however, it is simply not possible for your code to continue execution
if a value does not exist, or if a provided value does not satisfy certain conditions.
In these situations,
you can trigger an :newTerm:`assertion` in your code to end code execution,
and to provide an opportunity to debug the cause of the absent or invalid value.

An assertion is a runtime check that some logical condition definitely evaluates to ``true``.
Literally put, an assertion “asserts” that a condition is true.
You use an assertion to make sure that an essential condition is satisfied
before executing any further code.
If the condition equates to ``true``, code execution continues as normal;
if the condition equates to ``false``, code execution ends, and your app is terminated.

If your code triggers an assertion while running in a debug environment,
such as when you build and run an app in Xcode,
an assertion enables you to see exactly where the invalid state occurred,
and to query the state of your app at the time that the assertion was triggered.
An assertion also gives you the opportunity to provide
a suitable debug message as to the nature of the assert.

You write an assertion by calling the global ``assert`` function.
You pass the ``assert`` function an expression that equates to ``true`` or ``false``,
and a string message to display if the result of the condition is ``false``:

.. testcode:: assertions

   -> let age = -3
   << // age : Int = -3
   -> assert(age >= 0, "A person's age cannot be less than zero")
   xx assert
   // this causes the assertion to trigger, because age is not >= 0

In this example, code execution will only continue if ``age >= 0`` equates to ``true`` –
that is, if the value of ``age`` is non-negative.
If the value of ``age`` *is* negative, as in the code above,
then ``age >= 0`` equates to ``false``,
and the assertion is triggered, terminating the application.

Assertion messages cannot use string interpolation.
The assertion message can be omitted if desired, as in the following example:

.. testcode:: assertions

   -> assert(age >= 0)
   xx assert

.. _TheBasics_WhenToUseAssertions:

When To Use Assertions
~~~~~~~~~~~~~~~~~~~~~~

Use an assert whenever a condition has the potential to be false,
but must *definitely* be true in order for your code to continue execution.
Suitable candidates for an assertion check include:

* A subscript index is passed to a custom subscript implementation,
  but the subscript index could be invalid or out of bounds.

* A value is passed to a function,
  but an invalid value means that the function cannot fulfil its task.

* An optional value is currently ``nil``,
  but a non-``nil`` value is essential for subsequent code to execute successfully.

Subscripts are described in :doc:`Subscripts`,
and functions are described in :doc:`Functions`.

.. note::

   Assertions cause your app to terminate,
   and are not a substitute for designing your code in such a way
   that invalid conditions are unlikely to arise.
   Nonetheless, in situations where invalid conditions are possible,
   an assertion is an effective way to ensure that
   such conditions are highlighted and noticed during development,
   before your app is published.

.. QUESTION: is this the right place for the assertions section to go?