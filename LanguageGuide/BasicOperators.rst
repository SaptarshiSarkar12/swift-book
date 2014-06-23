Basic Operators
===============

An :newTerm:`operator` is a special symbol or phrase that you use to
check, change, or combine values.
For example, the addition operator (``+``) adds two numbers together
(as in ``let i = 1 + 2``).
More complex examples include the logical AND operator ``&&``
(as in ``if enteredDoorCode && passedRetinaScan``)
and the increment operator ``++i``,
which is a shortcut to increase the value of ``i`` by ``1``.

Swift supports most standard C operators
and improves several capabilities to eliminate common coding errors.
The assignment operator (``=``) does not return a value,
to prevent it from being mistakenly used when
the equal to operator (``==``) is intended.
Arithmetic operators (``+``, ``-``, ``*``, ``/``, ``%`` and so forth)
detect and disallow value overflow,
to avoid unexpected results when working with numbers that become larger or smaller
than the allowed value range of the type that stores them.
You can opt in to value overflow behavior
by using Swift's overflow operators,
as described in :ref:`AdvancedOperators_OverflowOperators`.

Unlike C, Swift lets you perform remainder (``%``) calculations on floating-point numbers.
Swift also provides two range operators (``a..<b`` and ``a...b``) not found in C,
as a shortcut for expressing a range of values.

This chapter describes the common operators in Swift.
:doc:`AdvancedOperators` covers Swift's advanced operators,
and describes how to define your own custom operators
and implement the standard operators for your own custom types.

.. _BasicOperators_Terminology:

Terminology
-----------

Operators are unary, binary, or ternary:

* :newTerm:`Unary` operators operate on a single target (such as ``-a``).
  Unary :newTerm:`prefix` operators appear immediately before their target (such as ``!b``),
  and unary :newTerm:`postfix` operators appear immediately after their target (such as ``i++``).
* :newTerm:`Binary` operators operate on two targets (such as ``2 + 3``)
  and are :newTerm:`infix` because they appear in between their two targets.
* :newTerm:`Ternary` operators operate on three targets.
  Like C, Swift has only one ternary operator,
  the ternary conditional operator (``a ? b : c``).

The values that operators affect are :newTerm:`operands`.
In the expression ``1 + 2``, the ``+`` symbol is a binary operator
and its two operands are the values ``1`` and ``2``.

.. _BasicOperators_AssignmentOperator:

Assignment Operator
-------------------

The :newTerm:`assignment operator` (``a = b``)
initializes or updates the value of ``a`` with the value of ``b``:

.. testcode:: assignmentOperator

   -> let b = 10
   << // b : Int = 10
   -> var a = 5
   << // a : Int = 5
   -> a = b
   /> a is now equal to \(a)
   </ a is now equal to 10

If the right side of the assignment is a tuple with multiple values,
its elements can be decomposed into multiple constants or variables at once:

.. testcode:: assignmentOperator

   -> let (x, y) = (1, 2)
   << // (x, y) : (Int, Int) = (1, 2)
   /> x is equal to \(x), and y is equal to \(y)
   </ x is equal to 1, and y is equal to 2

Unlike the assignment operator in C and Objective-C,
the assignment operator in Swift does not itself return a value.
The following statement is not valid:

.. testcode:: assignmentOperatorInvalid

   -> if x = y {
         // this is not valid, because x = y does not return a value
      }
   !! <REPL Input>:1:4: error: use of unresolved identifier 'x'
   !! if x = y {
   !!    ^
   !! <REPL Input>:1:8: error: use of unresolved identifier 'y'
   !! if x = y {
   !!        ^

This feature prevents the assignment operator (``=``) from being used by accident
when the equal to operator (``==``) is actually intended.
By making ``if x = y`` invalid,
Swift helps you to avoid these kinds of errors in your code.

.. TODO: Should we mention that x = y = z is also not valid?
   If so, is there a convincing argument as to why this is a good thing?

.. _BasicOperators_ArithmeticOperators:

Arithmetic Operators
--------------------

Swift supports the four standard :newTerm:`arithmetic operators` for all number types:

* Addition (``+``)
* Subtraction (``-``)
* Multiplication (``*``)
* Division (``/``)

.. testcode:: arithmeticOperators

   -> 1 + 2       // equals 3
   << // r0 : Int = 3
   -> 5 - 3       // equals 2
   << // r1 : Int = 2
   -> 2 * 3       // equals 6
   << // r2 : Int = 6
   -> 10.0 / 2.5  // equals 4.0
   << // r3 : Double = 4.0

Unlike the arithmetic operators in C and Objective-C,
the Swift arithmetic operators do not allow values to overflow by default.
You can opt in to value overflow behavior by using Swift's overflow operators
(such as ``a &+ b``). See :ref:`AdvancedOperators_OverflowOperators`.

The addition operator is also supported for ``String`` concatenation:

.. testcode:: arithmeticOperators

   -> "hello, " + "world"  // equals "hello, world"
   << // r4 : String = "hello, world"

Two ``Character`` values,
or one ``Character`` value and one ``String`` value,
can be added together to make a new ``String`` value:

.. testcode:: arithmeticOperators

   -> let dog: Character = "🐶"
   << // dog : Character = 🐶
   -> let cow: Character = "🐮"
   << // cow : Character = 🐮
   -> let dogCow = dog + cow
   << // dogCow : String = "🐶🐮"
   /> dogCow is equal to \"🐶🐮\"
   </ dogCow is equal to "🐶🐮"

See also :ref:`StringsAndCharacters_ConcatenatingStringsAndCharacters`.

.. TODO: should I also mention array concatenation here once we have it?

.. _BasicOperators_RemainderOperator:

Remainder Operator
~~~~~~~~~~~~~~~~~~

The :newTerm:`remainder operator` (``a % b``)
works out how many multiples of ``b`` will fit inside ``a``
and returns the value that is left over
(known as the :newTerm:`remainder`).

.. note::

   The remainder operator (``%``) is also known as
   a :newTerm:`modulo operator` in other languages.
   However, its behavior in Swift for negative numbers means that it is,
   strictly speaking, a remainder rather than a modulo operation.

Here's how the remainder operator works.
To calculate ``9 % 4``, you first work out how many ``4``\ s will fit inside ``9``:

.. image:: ../images/remainderInteger_2x.png
   :align: center

You can fit two ``4``\ s inside ``9``, and the remainder is ``1`` (shown in orange).

In Swift, this would be written as:

.. testcode:: arithmeticOperators

   -> 9 % 4    // equals 1
   << // r5 : Int = 1

To determine the answer for ``a % b``,
the ``%`` operator calculates the following equation
and returns ``remainder`` as its output:

``a`` = (``b`` × ``some multiplier``) + ``remainder``

where ``some multiplier`` is the largest number of multiples of ``b``
that will fit inside ``a``.

Inserting ``9`` and ``4`` into this equation yields:

``9`` = (``4`` × ``2``) + ``1``

The same method is applied when calculating the remainder for a negative value of ``a``:

.. testcode:: arithmeticOperators

   -> -9 % 4   // equals -1
   << // r6 : Int = -1

Inserting ``-9`` and ``4`` into the equation yields:

``-9`` = (``4`` × ``-2``) + ``-1``

giving a remainder value of ``-1``.

The sign of ``b`` is ignored for negative values of ``b``.
This means that ``a % b`` and ``a % -b`` always give the same answer.

.. _BasicOperators_FloatingPointRemainderCalculations:

Floating-Point Remainder Calculations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Unlike the remainder operator in C and Objective-C,
Swift's remainder operator can also operate on floating-point numbers:

.. testcode:: arithmeticOperators

   -> 8 % 2.5   // equals 0.5
   << // r7 : Double = 0.5

In this example, ``8`` divided by ``2.5`` equals ``3``, with a remainder of ``0.5``,
so the remainder operator returns a ``Double`` value of ``0.5``.

.. image:: ../images/remainderFloat_2x.png
   :align: center

.. _BasicOperators_IncrementAndDecrementOperators:

Increment and Decrement Operators
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Like C, Swift provides an :newTerm:`increment operator` (``++``)
and a :newTerm:`decrement operator` (``--``)
as a shortcut to increase or decrease the value of a numeric variable by ``1``.
You can use these operators with variables of any integer or floating-point type.

.. testcode:: arithmeticOperators

   -> var i = 0
   << // i : Int = 0
   -> ++i      // i now equals 1
   << // r8 : Int = 1

Each time you call ``++i``, the value of ``i`` is increased by ``1``.
Essentially, ``++i`` is shorthand for saying ``i = i + 1``.
Likewise, ``--i`` can be used as shorthand for ``i = i - 1``.

The ``++`` and ``--`` symbols can be used as prefix operators or as postfix operators.
``++i`` and ``i++`` are both valid ways to increase the value of ``i`` by ``1``.
Similarly, ``--i`` and ``i--`` are both valid ways to decrease the value of ``i`` by ``1``.

Note that these operators modify ``i`` and also return a value.
If you only want to increment or decrement the value stored in ``i``,
you can ignore the returned value.
However, if you *do* use the returned value,
it will be different based on whether you used the prefix or postfix version of the operator,
according to the following rules:

* If the operator is written *before* the variable,
  it increments the variable *before* returning its value.
* If the operator is written *after* the variable,
  it increments the variable *after* returning its value.

For example:

.. testcode:: arithmeticOperators

   -> var a = 0
   << // a : Int = 0
   -> let b = ++a
   << // b : Int = 1
   /> a and b are now both equal to \(a)
   </ a and b are now both equal to 1
   -> let c = a++
   << // c : Int = 1
   /> a is now equal to \(a), but c has been set to the pre-increment value of \(c)
   </ a is now equal to 2, but c has been set to the pre-increment value of 1

In the example above,
``let b = ++a`` increments ``a`` *before* returning its value.
This is why both ``a`` and ``b`` are equal to to the new value of ``1``.

However, ``let c = a++`` increments ``a`` *after* returning its value.
This means that ``c`` gets the old value of ``1``,
and ``a`` is then updated to equal ``2``.

Unless you need the specific behavior of ``i++``,
it is recommended that you use ``++i`` and ``--i`` in all cases,
because they have the typical expected behavior of modifying ``i``
and returning the result.

.. QUESTION: is this good advice
   (given the general prevalence of i++ in the world),
   and indeed is it even advice we need to bother giving
   (given that lots of people might disagree or not care)?

.. QUESTION: if so, have I followed this advice throughout the book?

.. _BasicOperators_UnaryMinusOperator:

Unary Minus Operator
~~~~~~~~~~~~~~~~~~~~

The sign of a numeric value can be toggled using a prefixed ``-``,
known as the :newTerm:`unary minus operator`:

.. testcode:: arithmeticOperators

   -> let three = 3
   << // three : Int = 3
   -> let minusThree = -three       // minusThree equals -3
   << // minusThree : Int = -3
   -> let plusThree = -minusThree   // plusThree equals 3, or "minus minus three"
   << // plusThree : Int = 3

The unary minus operator (``-``) is prepended directly before the value it operates on,
without any white space.

.. _BasicOperators_UnaryPlusOperator:

Unary Plus Operator
~~~~~~~~~~~~~~~~~~~

The :newTerm:`unary plus operator` (``+``) simply returns
the value it operates on, without any change:

.. testcode:: arithmeticOperators

   -> let minusSix = -6
   << // minusSix : Int = -6
   -> let alsoMinusSix = +minusSix  // alsoMinusSix equals -6
   << // alsoMinusSix : Int = -6

Although the unary plus operator doesn't actually do anything,
you can use it to provide symmetry in your code for positive numbers
when also using the unary minus operator for negative numbers.

.. _BasicOperators_CompoundAssignmentOperators:

Compound Assignment Operators
-----------------------------

Like C, Swift provides :newTerm:`compound assignment operators` that combine assignment (``=``) with another operation.
One example is the :newTerm:`addition assignment operator` (``+=``):

.. testcode:: compoundAssignment

   -> var a = 1
   << // a : Int = 1
   -> a += 2
   /> a is now equal to \(a)
   </ a is now equal to 3

The expression ``a += 2`` is shorthand for ``a = a + 2``.
Effectively, the addition and the assignment are combined into one operator
that performs both tasks at the same time.

.. note::

   The compound assignment operators do not return a value.
   You cannot write ``let b = a += 2``, for example.
   This behavior is different from the increment and decrement operators mentioned above.

A complete list of compound assignment operators can be found in :doc:`../ReferenceManual/Expressions`.

.. _BasicOperators_ComparisonOperators:

Comparison Operators
--------------------

Swift supports all standard C :newTerm:`comparison operators`:

* Equal to (``a == b``)
* Not equal to (``a != b``)
* Greater than (``a > b``)
* Less than (``a < b``)
* Greater than or equal to (``a >= b``)
* Less than or equal to (``a <= b``)

.. note::

   Swift also provides two :newTerm:`identity operators` (``===`` and ``!==``),
   which you use to test whether two object references both refer to the same object instance.
   For more information, see :doc:`ClassesAndStructures`.

Each of the comparison operators returns a ``Bool`` value to indicate whether or not the statement is true:

.. testcode:: comparisonOperators

   -> 1 == 1   // true, because 1 is equal to 1
   << // r0 : Bool = true
   -> 2 != 1   // true, because 2 is not equal to 1
   << // r1 : Bool = true
   -> 2 > 1    // true, because 2 is greater than 1
   << // r2 : Bool = true
   -> 1 < 2    // true, because 1 is less than 2
   << // r3 : Bool = true
   -> 1 >= 1   // true, because 1 is greater than or equal to 1
   << // r4 : Bool = true
   -> 2 <= 1   // false, because 2 is not less than or equal to 1
   << // r5 : Bool = false

Comparison operators are often used in conditional statements,
such as the ``if`` statement:

.. testcode:: comparisonOperators

   -> let name = "world"
   << // name : String = "world"
   -> if name == "world" {
         println("hello, world")
      } else {
         println("I'm sorry \(name), but I don't recognize you")
      }
   << hello, world
   // prints "hello, world", because name is indeed equal to "world"

For more on the ``if`` statement, see :doc:`ControlFlow`.

.. TODO: which types do these operate on by default?
   How do they work with strings?
   How about with tuples / with your own types?

.. _BasicOperators_TernaryConditionalOperator:

Ternary Conditional Operator
----------------------------

The :newTerm:`ternary conditional operator` is a special operator with three parts,
which takes the form ``question ? answer1 : answer2``.
It is a shortcut for evaluating one of two expressions
based on whether ``question`` is true or false.
If ``question`` is true, it evaluates ``answer1`` and returns its value;
otherwise, it evaluates ``answer2`` and returns its value.

The ternary conditional operator is shorthand for the code below:

.. testcode:: ternaryConditionalOperatorOutline

   >> let question = true
   << // question : Bool = true
   >> let answer1 = true
   << // answer1 : Bool = true
   >> let answer2 = true
   << // answer2 : Bool = true
   -> if question {
         answer1
      } else {
         answer2
      }

Here's an example, which calculates the height for a table row.
The row height should be 50 points taller than the content height
if the row has a header, and 20 points taller if the row doesn't have a header:

.. testcode:: ternaryConditionalOperatorPart1

   -> let contentHeight = 40
   << // contentHeight : Int = 40
   -> let hasHeader = true
   << // hasHeader : Bool = true
   -> let rowHeight = contentHeight + (hasHeader ? 50 : 20)
   << // rowHeight : Int = 90
   /> rowHeight is equal to \(rowHeight)
   </ rowHeight is equal to 90

The preceding example is shorthand for the code below:

.. testcode:: ternaryConditionalOperatorPart2

   -> let contentHeight = 40
   << // contentHeight : Int = 40
   -> let hasHeader = true
   << // hasHeader : Bool = true
   -> var rowHeight = contentHeight
   << // rowHeight : Int = 40
   -> if hasHeader {
         rowHeight = rowHeight + 50
      } else {
         rowHeight = rowHeight + 20
      }
   /> rowHeight is equal to \(rowHeight)
   </ rowHeight is equal to 90

The first example's use of the ternary conditional operator means that
``rowHeight`` can be set to the correct value on a single line of code.
This is more concise than the second example,
and removes the need for ``rowHeight`` to be a variable,
because its value does not need to be modified within an ``if`` statement.

The ternary conditional operator provides
an efficient shorthand for deciding which of two expressions to consider.
Use the ternary conditional operator with care, however.
Its conciseness can lead to hard-to-read code if overused.
Avoid combining multiple instances of the ternary conditional operator into one compound statement.

.. _BasicOperators_RangeOperators:

Range Operators
---------------

Swift includes two :newTerm:`range operators`,
which are shortcuts for expressing a range of values.

.. _BasicOperators_ClosedRangeOperator:

Closed Range Operator
~~~~~~~~~~~~~~~~~~~~~

The :newTerm:`closed range operator` (``a...b``)
defines a range that runs from ``a`` to ``b``,
and includes the values ``a`` and ``b``.

The closed range operator is useful when iterating over a range
in which you want all of the values to be used,
such as with a ``for``-``in`` loop:

.. testcode:: rangeOperators

   -> for index in 1...5 {
         println("\(index) times 5 is \(index * 5)")
      }
   </ 1 times 5 is 5
   </ 2 times 5 is 10
   </ 3 times 5 is 15
   </ 4 times 5 is 20
   </ 5 times 5 is 25

For more on ``for``-``in`` loops, see :doc:`ControlFlow`.

.. _BasicOperators_HalfClosedRangeOperator:

Half-Closed Range Operator
~~~~~~~~~~~~~~~~~~~~~~~~~~

The :newTerm:`half-closed range operator` (``a..<b``)
defines a range that runs from ``a`` to ``b``,
but does not include ``b``.
It is said to be :newTerm:`half-closed`
because it contains its first value, but not its final value.

Half-closed ranges are particularly useful when you work with
zero-based lists such as arrays,
where it is useful to count up to (but not including) the length of the list:

.. testcode:: rangeOperators

   -> let names = ["Anna", "Alex", "Brian", "Jack"]
   << // names : Array<String> = ["Anna", "Alex", "Brian", "Jack"]
   -> let count = names.count
   << // count : Int = 4
   -> for i in 0..<count {
         println("Person \(i + 1) is called \(names[i])")
      }
   </ Person 1 is called Anna
   </ Person 2 is called Alex
   </ Person 3 is called Brian
   </ Person 4 is called Jack

Note that the array contains four items,
but ``0..<count`` only counts as far as ``3``
(the index of the last item in the array),
because it is a half-closed range.
For more on arrays, see :ref:`CollectionTypes_Arrays`.

.. _BasicOperators_LogicalOperators:

Logical Operators
-----------------

:newTerm:`Logical operators` modify or combine
the Boolean logic values ``true`` and ``false``.
Swift supports the three standard logical operators found in C-based languages:

* Logical NOT (``!a``)
* Logical AND (``a && b``)
* Logical OR (``a || b``)

.. _BasicOperators_LogicalNOTOperator:

Logical NOT Operator
~~~~~~~~~~~~~~~~~~~~

The :newTerm:`logical NOT operator` (``!a``) inverts a Boolean value so that ``true`` becomes ``false``,
and ``false`` becomes ``true``.

The logical NOT operator is a prefix operator,
and appears immediately before the value it operates on,
without any white space.
It can be read as “not ``a``”, as seen in the following example:

.. testcode:: logicalOperators

   -> let allowedEntry = false
   << // allowedEntry : Bool = false
   -> if !allowedEntry {
         println("ACCESS DENIED")
      }
   <- ACCESS DENIED

The phrase ``if !allowedEntry`` can be read as “if not allowed entry.”
The subsequent line is only executed if “not allowed entry” is true;
that is, if ``allowedEntry`` is ``false``.

As in this example,
careful choice of Boolean constant and variable names
can help to keep code readable and concise,
while avoiding double negatives or confusing logic statements.

.. _BasicOperators_LogicalANDOperator:

Logical AND Operator
~~~~~~~~~~~~~~~~~~~~

The :newTerm:`logical AND operator` (``a && b``) creates logical expressions
where both values must be ``true`` for the overall expression to also be ``true``.

If either value is ``false``,
the overall expression will also be ``false``.
In fact, if the *first* value is ``false``,
the second value won't even be evaluated,
because it can't possibly make the overall expression equate to ``true``.
This is known as :newTerm:`short-circuit evaluation`.

This example considers two ``Bool`` values
and only allows access if both values are ``true``:

.. testcode:: logicalOperators

   -> let enteredDoorCode = true
   << // enteredDoorCode : Bool = true
   -> let passedRetinaScan = false
   << // passedRetinaScan : Bool = false
   -> if enteredDoorCode && passedRetinaScan {
         println("Welcome!")
      } else {
         println("ACCESS DENIED")
      }
   <- ACCESS DENIED

.. _BasicOperators_LogicalOROperator:

Logical OR Operator
~~~~~~~~~~~~~~~~~~~

The :newTerm:`logical OR operator`
(``a || b``) is an infix operator made from two adjacent pipe characters.
You use it to create logical expressions in which
only *one* of the two values has to be ``true``
for the overall expression to be ``true``.

Like the Logical AND operator above,
the Logical OR operator uses short-circuit evaluation to consider its expressions.
If the left side of a Logical OR expression is ``true``,
the right side is not evaluated,
because it cannot change the outcome of the overall expression.

In the example below,
the first ``Bool`` value (``hasDoorKey``) is ``false``,
but the second value (``knowsOverridePassword``) is ``true``.
Because one value is ``true``,
the overall expression also evaluates to ``true``,
and access is allowed:

.. testcode:: logicalOperators

   -> let hasDoorKey = false
   << // hasDoorKey : Bool = false
   -> let knowsOverridePassword = true
   << // knowsOverridePassword : Bool = true
   -> if hasDoorKey || knowsOverridePassword {
         println("Welcome!")
      } else {
         println("ACCESS DENIED")
      }
   <- Welcome!

.. _BasicOperators_CombiningLogicalOperators:

Combining Logical Operators
~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can combine multiple logical operators to create longer compound expressions:

.. testcode:: logicalOperators

   -> if enteredDoorCode && passedRetinaScan || hasDoorKey || knowsOverridePassword {
         println("Welcome!")
      } else {
         println("ACCESS DENIED")
      }
   <- Welcome!

This example uses multiple ``&&`` and ``||`` operators to create a longer compound expression.
However, the ``&&`` and ``||`` operators still operate on only two values,
so this is actually three smaller expressions chained together.
It can be read as:

If we've entered the correct door code and passed the retina scan;
or if we have a valid door key;
or if we know the emergency override password,
then allow access.

Based on the values of ``enteredDoorCode``, ``passedRetinaScan``, and ``hasDoorKey``,
the first two mini-expressions are ``false``.
However, the emergency override password is known,
so the overall compound expression still evaluates to ``true``.

.. _BasicOperators_Explicit Parentheses:

Explicit Parentheses
~~~~~~~~~~~~~~~~~~~~

It is sometimes useful to include parentheses when they are not strictly needed,
to make the intention of a complex expression easier to read.
In the door access example above,
it is useful to add parentheses around the first part of the compound expression
to make its intent explicit:

.. testcode:: logicalOperators

   -> if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {
         println("Welcome!")
      } else {
         println("ACCESS DENIED")
      }
   <- Welcome!

The parentheses make it clear that the first two values
are considered as part of a separate possible state in the overall logic.
The output of the compound expression doesn't change,
but the overall intention is clearer to the reader.
Readability is always preferred over brevity;
use parentheses where they help to make your intentions clear.