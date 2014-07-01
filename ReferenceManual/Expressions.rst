Expressions
===========

In Swift, there are four kinds of expressions:
prefix expressions, binary expressions, primary expressions, and postfix expressions.
Evaluating an expression returns a value,
causes a side effect, or both.

Prefix and binary expressions let you
apply operators to smaller expressions.
Primary expressions are conceptually the simplest kind of expression,
and they provide a way to access values.
Postfix expressions,
like prefix and binary expressions,
let you build up more complex expressions
using postfixes such as function calls and member access.
Each kind of expression is described in detail
in the sections below.

.. langref-grammar

    expr          ::= expr-basic
    expr          ::= expr-trailing-closure expr-cast?

    expr-basic    ::= expr-sequence expr-cast?

    expr-sequence ::= expr-unary expr-binary*


.. syntax-grammar::

    Grammar of an expression

    expression --> prefix-expression binary-expressions-OPT
    expression-list --> expression | expression ``,`` expression-list


.. _Expressions_PrefixExpressions:

Prefix Expressions
------------------

:newTerm:`Prefix expressions` combine
an optional prefix operator with an expression.
Prefix operators take one argument,
the expression that follows them.

.. TR: Does it make sense to call out the left-to-right grouping?

The Swift standard library provides the following prefix operators:

* ``++`` Increment
* ``--`` Decrement
* ``!`` Logical NOT
* ``~`` Bitwise NOT
* ``+`` Unary plus
* ``-`` Unary minus

For information about the behavior of these operators,
see :doc:`../LanguageGuide/BasicOperators` and :doc:`../LanguageGuide/AdvancedOperators`.

In addition to the standard library operators listed above,
you use ``&`` immediately before the name of a variable that's being passed
as an in-out argument to a function call expression.
For more information and to see an example,
see :ref:`Functions_InOutParameters`.

.. TODO: Need to a brief write up on the in-out-expression.

.. langref-grammar

    expr-unary   ::= operator-prefix* expr-postfix

.. syntax-grammar::

    Grammar of a prefix expression

    prefix-expression --> prefix-operator-OPT postfix-expression
    prefix-expression --> in-out-expression
    in-out-expression --> ``&`` identifier

.. _Expressions_BinaryExpressions:

Binary Expressions
------------------

:newTerm:`Binary expressions` combine
an infix binary operator with the expression that it takes
as its left-hand and right-hand arguments.
It has the following form:

.. syntax-outline::

   <#left-hand argument#> <#operator#> <#right-hand argument#>

The Swift standard library provides the following binary operators:

.. The following comes from stdlib/core/Policy.swift

* Exponentiative (No associativity, precedence level 160)
    - ``<<`` Bitwise left shift
    - ``>>`` Bitwise right shift

* Multiplicative (Left associative, precedence level 150)
    - ``*`` Multiply
    - ``/`` Divide
    - ``%`` Remainder
    - ``&*`` Multiply, ignoring overflow
    - ``&/`` Divide, ignoring overflow
    - ``&%`` Remainder, ignoring overflow
    - ``&`` Bitwise AND

* Additive (Left associative, precedence level 140)
    - ``+`` Add
    - ``-`` Subtract
    - ``&+`` Add with overflow
    - ``&-`` Subtract with overflow
    - ``|`` Bitwise OR
    - ``^`` Bitwise XOR

* Range (No associativity, precedence level 135)
    - ``..<`` Half-closed range
    - ``...`` Closed range

* Cast (No associativity, precedence level 132)
    - ``is`` Type check
    - ``as`` Type cast

* Comparative (No associativity, precedence level 130)
    - ``<`` Less than
    - ``<=`` Less than or equal
    - ``>`` Greater than
    - ``>=`` Greater than or equal
    - ``==`` Equal
    - ``!=`` Not equal
    - ``===`` Identical
    - ``!==`` Not identical
    - ``~=`` Pattern match

* Conjunctive (Left associative, precedence level 120)
    - ``&&`` Logical AND

* Disjunctive (Left associative, precedence level 110)
    - ``||`` Logical OR

* Ternary Conditional (Right associative, precedence level 100)
    - ``?`` ``:`` Ternary conditional

* Assignment (Right associative, precedence level 90)
    - ``=`` Assign
    - ``*=`` Multiply and assign
    - ``/=`` Divide and assign
    - ``%=`` Remainder and assign
    - ``+=`` Add and assign
    - ``-=`` Subtract and assign
    - ``<<=`` Left bit shift and assign
    - ``>>=`` Right bit shift and assign
    - ``&=`` Bitwise AND and assign
    - ``^=`` Bitwise XOR and assign
    - ``|=`` Bitwise OR and assign
    - ``&&=`` Logical AND and assign
    - ``||=`` Logical OR and assign

For information about the behavior of these operators,
see :doc:`../LanguageGuide/BasicOperators` and :doc:`../LanguageGuide/AdvancedOperators`.

.. You have essentially expression sequences here, and within it are
   parts of the expressions.  We're calling them "expressions" even
   though they aren't what we ordinarily think of as expressions.  We
   have this two-phase thing where we do the expression sequence parsing
   which gives a rough parse tree.  Then after name binding we know
   operator precedence and we do a second phase of parsing that builds
   something that's a more traditional tree.

.. You're going to care about this if you're adding new operators --
   it's not a high priority.  We could probably loosely describe this
   process by saying that the parser handles it as a flat list and then
   applies the operator precedence to make a more typical parse tree.
   At some point, we will probably have to document the syntax around
   creating operators.  This may need to be discussed in the Language Guide
   in respect to the spacing rules -- ``x + y * z`` is different than
   ``x + y* z``.

.. note::

    At parse time,
    an expression made up of binary operators is represented
    as a flat list.
    This list is transformed into a tree
    by applying operator precedence
    For example, the expression ``2 + 3 * 5``
    is initially understood as a flat list of five items,
    ``2``, ``+``, ``3``, ``*``, and ``5``.
    This process transforms it into the tree (2 + (3 * 5)).

.. langref-grammar

    expr-binary ::= op-binary-or-ternary expr-unary expr-cast?
    op-binary-or-ternary ::= operator-binary
    op-binary-or-ternary ::= '='
    op-binary-or-ternary ::= '?'-infix expr-sequence ':'

.. syntax-grammar::

    Grammar of a binary expression

    binary-expression --> binary-operator prefix-expression
    binary-expression --> assignment-operator prefix-expression
    binary-expression --> conditional-operator prefix-expression
    binary-expression --> type-casting-operator
    binary-expressions --> binary-expression binary-expressions-OPT


.. _Expressions_AssignmentOperator:

Assignment Operator
~~~~~~~~~~~~~~~~~~~

The :newTerm:`assigment operator` sets a new value
for a given expression.
It has the following form:

.. syntax-outline::

   <#expression#> = <#value#>

The value of the *expression*
is set to the value obtained by evaluating the *value*.
If the *expression* is a tuple,
the *value* must be a tuple
with the same number of elements.
(Nested tuples are allowed.)
Assignment is performed from each part of the *value*
to the corresponding part of the *expression*.
For example:

.. testcode::

    >> var (a, _, (b, c)) = ("test", 9.45, (12, 3))
    << // (a, _, (b, c)) : (String, Double, (Int, Int)) = ("test", 9.45, (12, 3))
    -> (a, _, (b, c)) = ("test", 9.45, (12, 3))
    -> // a is "test", b is 12, c is 3, and 9.45 is ignored

The assignment operator does not return any value.

.. langref-grammar

    op-binary-or-ternary ::= '='

.. syntax-grammar::

    Grammar of an assignment operator

    assignment-operator --> ``=``


.. _Expressions_TernaryConditionalOperator:

Ternary Conditional Operator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :newTerm:`ternary conditional operator` evaluates to one of two given values
based on the value of a condition.
It has the following form:

.. syntax-outline::

   <#condition#> ? <#expression used if true#> : <#expression used if false#>

If the *condition* evaluates to ``true``,
the conditional operator evaluates the first expression
and returns its value.
Otherwise, it evaluates the second expression
and returns its value.
The unused expression is not evaluated.

For an example that uses the ternary conditional operator,
see :ref:`BasicOperators_TernaryConditionalOperator`.

.. langref-grammar

    op-binary-or-ternary ::= '?'-infix expr-sequence ':'

.. syntax-grammar::

    Grammar of a conditional operator

    conditional-operator --> ``?`` expression ``:``


.. _Expressions_Type-CastingOperators:

Type-Casting Operators
~~~~~~~~~~~~~~~~~~~~~~~

There are three type-casting operators,
the ``is`` operator, the ``as?`` operator,
and the ``as`` operator.
They have the following form:

.. syntax-outline::

    <#expression#> is <#type#>
    <#expression#> as? <#type#>
    <#expression#> as <#type#>

The ``is`` operator checks at runtime whether the *expression*
can be downcast to the specified *type*.
It returns ``true`` if the *expression* can be downcast to the specified *type*;
otherwise, it returns ``false``.
If casting to the specified *type*
is guaranteed to succeed or fail,
a compile-time error is raised.
For example, ``10 is Int`` and ``10 is String``
both raise compile-time errors.

.. assertion::

    -> "hello" is String
    !! <REPL Input>:1:9: error: 'is' test is always true
    !! "hello" is String
    !!         ^
    "hello" is Int
    !! <REPL Input>:1:9: error: cannot convert the expression's type 'Bool' to type 'StringLiteralConvertible'
    !! "hello" is Int
    !! ~~~~~~~~^~~~~~

.. If the bugs are fixed, this can be reworded:
    The ``is`` operator checks at runtime
    to see whether the *expression*
    can be cast to the specified *type*
    If so, it returns ``true``; otherwise, it returns ``false``.

    See also <rdar://problem/16639705> Provably true/false "is" expressions should be a warning, not an error
    See also <rdar://problem/16732083> Subtypes are not considered by the 'is' operator

The ``as?`` operator
performs a conditional cast of the *expression*
to the specified *type*.
The ``as?`` operator returns an optional of the specified *type*.
At runtime, if the cast succeeds,
the value of *expression* is wrapped in an optional and returned;
otherwise, the value returned is ``nil``.
If casting to the specified *type*
is guaranteed to fail,
a compile-time error is raised.
For example, casting to a type that's neither a subclass or superclass
of the type of the *expression* is an error.

The ``as`` operator performs a forced cast of the *expression* to the specified *type*.
The ``as`` operator returns a value of the specified *type*, not an optional type.
If the cast fails, a runtime error is raised.
The behavior of ``x as T`` is the same as the behavior of ``(x as? T)!``.

For more information about type casting
and to see examples that use the type-casting operators,
see :doc:`../LanguageGuide/TypeCasting`.

.. langref-grammar

    expr-cast ::= 'is' type
    expr-cast ::= 'as' type

.. syntax-grammar::

    Grammar of a type-casting operator

    type-casting-operator --> ``is`` type
    type-casting-operator --> ``as`` type
    type-casting-operator --> ``as`` ``?`` type


.. _Expressions_PrimaryExpressions:

Primary Expressions
-------------------

:newTerm:`Primary expressions`
are the most basic kind of expression.
They can be used as expressions on their own,
and they can be combined with other tokens
to make prefix expressions, binary expressions, and postfix expressions.

.. langref-grammar

    expr-primary  ::= expr-literal
    expr-primary  ::= expr-identifier
    expr-primary  ::= expr-super
    expr-primary  ::= expr-closure
    expr-primary  ::= expr-anon-closure-arg
    expr-primary  ::= expr-paren
    expr-primary  ::= expr-delayed-identifier

.. syntax-grammar::

    Grammar of a primary expression

    primary-expression --> identifier generic-argument-clause-OPT
    primary-expression --> literal-expression
    primary-expression --> self-expression
    primary-expression --> superclass-expression
    primary-expression --> closure-expression
    primary-expression --> parenthesized-expression
    primary-expression --> implicit-member-expression
    primary-expression --> wildcard-expression

.. NOTE: One reason for breaking primary expressions out of postfix
   expressions is for exposition -- it makes it easier to organize the
   prose surrounding the production rules.

.. TR: Is a generic argument clause allowed
   after an identifier in expression context?
   It seems like that should only occur when an identifier
   is a *type* identifier.


.. _Expressions_LiteralExpression:

Literal Expression
~~~~~~~~~~~~~~~~~~

A :newTerm:`literal expression` consists of
either an ordinary literal (such as a string or a number),
an array or dictionary literal,
or one of the following special literals:

================    ======  ===============================================
Literal             Type    Value
================    ======  ===============================================
``__FILE__``        String  The name of the file in which it appears.
``__LINE__``        Int     The line number on which it appears.
``__COLUMN__``      Int     The column number in which it begins.
``__FUNCTION__``    String  The name of the declaration in which it appears.
================    ======  ===============================================

Inside a function,
the value of ``__FUNCTION__`` is the name of that function,
inside a method it is the name of that method,
inside a property getter or setter it is the name of that property,
inside special members like ``init`` or ``subscript``
it is the name of that keyword,
and at the top level of a file it is the name of the current module.

An :newTerm:`array literal` is
an ordered collection of values.
It has the following form:

.. syntax-outline::

   [<#value 1#>, <#value 2#>, <#...#>]

The last expression in the array can be followed by an optional comma.
An empty array literal is written
as an empty pair of brackets (``[]``).
The value of an array literal has type ``[T]``,
where ``T`` is the type of the expressions inside it.
If there are expressions of multiple types,
``T`` is their closest common supertype.

A :newTerm:`dictionary literal` is
an unordered collection of key-value pairs.
It has the following form:

.. syntax-outline::

   [<#key 1#>: <#value 1#>, <#key 2#>: <#value 2#>, <#...#>]

The last expression in the dictionary can be followed by an optional comma.
An empty dictionary literal is written as
a colon inside a pair of brackets (``[:]``)
to distinguish it from an empty array literal.
The value of a dictionary literal has type ``[KeyType: ValueType]``,
where ``KeyType`` is the type of its key expressions
and ``ValueType`` is the type of its value expressions.
If there are expressions of multiple types,
``KeyType`` and ``ValueType`` are the closest common supertype
for their respective values.

.. langref-grammar

    expr-literal ::= integer_literal
    expr-literal ::= floating_literal
    expr-literal ::= character_literal
    expr-literal ::= string_literal
    expr-literal ::= '__FILE__'
    expr-literal ::= '__LINE__'
    expr-literal ::= '__COLUMN__'

.. syntax-grammar::

    Grammar of a literal expression

    literal-expression --> literal
    literal-expression --> array-literal | dictionary-literal
    literal-expression --> ``__FILE__`` | ``__LINE__`` | ``__COLUMN__`` | ``__FUNCTION__``

    array-literal --> ``[`` array-literal-items-OPT ``]``
    array-literal-items --> array-literal-item ``,``-OPT | array-literal-item ``,`` array-literal-items
    array-literal-item --> expression

    dictionary-literal --> ``[`` dictionary-literal-items ``]`` | ``[`` ``:`` ``]``
    dictionary-literal-items --> dictionary-literal-item ``,``-OPT | dictionary-literal-item ``,`` dictionary-literal-items
    dictionary-literal-item --> expression ``:`` expression


.. _Expressions_SelfExpression:

Self Expression
~~~~~~~~~~~~~~~

The ``self`` expression is an explicit reference to the current type
or instance of the type in which it occurs.
It has the following forms:

.. syntax-outline::

    self
    self.<#member name#>
    self[<#subscript index#>]
    self(<#initializer arguments#>)
    self.init(<#initializer arguments#>)

.. TODO: Come back and explain the second to last form (i.e., self(arg: value)).

In an initializer, subscript, or instance method, ``self`` refers to the current
instance of the type in which it occurs. In a static or class method,
``self`` refers to the current type in which it occurs.

The ``self`` expression is used to specify scope when accessing members,
providing disambiguation when there is
another variable of the same name in scope,
such as a function parameter.
For example:

.. testcode::

    -> class SomeClass {
           var greeting: String
           init(greeting: String) {
               self.greeting = greeting
           }
       }

In a mutating method of a value type,
you can assign a new instance of that value type to ``self``.
For example:

.. testcode::

    -> struct Point {
          var x = 0.0, y = 0.0
          mutating func moveByX(deltaX: Double, y deltaY: Double) {
             self = Point(x: x + deltaX, y: y + deltaY)
          }
       }
    >> var somePoint = Point(x: 1.0, y: 1.0)
    << // somePoint : Point = Point(1.0, 1.0)
    >> somePoint.moveByX(2.0, y: 3.0)
    >> println("The point is now at (\(somePoint.x), \(somePoint.y))")
    << The point is now at (3.0, 4.0)

.. syntax-grammar::

    Grammar of a self expression

    self-expression --> ``self``
    self-expression --> ``self`` ``.`` identifier
    self-expression --> ``self`` ``[`` expression ``]``
    self-expression --> ``self`` ``.`` ``init``


.. _Expressions_SuperclassExpression:

Superclass Expression
~~~~~~~~~~~~~~~~~~~~~

A :newTerm:`superclass expression` lets a class
interact with its superclass.
It has one of the following forms:

.. syntax-outline::

    super.<#member name#>
    super[<#subscript index#>]
    super.init(<#initializer arguments#>)

The first form is used to access a member of the superclass.
The second form is used to access the superclass's subscript implementation.
The third form is used to access an initializer of the superclass.

Subclasses can use a superclass expression
in their implementation of members, subscripting, and initializers
to make use of the implementation in their superclass.

.. langref-grammar

    expr-super ::= expr-super-method
    expr-super ::= expr-super-subscript
    expr-super ::= expr-super-constructor
    expr-super-method ::= 'super' '.' expr-identifier
    expr-super-subscript ::= 'super' '[' expr ']'
    expr-super-constructor ::= 'super' '.' 'init'

.. syntax-grammar::

    Grammar of a superclass expression

    superclass-expression --> superclass-method-expression | superclass-subscript-expression | superclass-initializer-expression

    superclass-method-expression --> ``super`` ``.`` identifier
    superclass-subscript-expression --> ``super`` ``[`` expression ``]``
    superclass-initializer-expression --> ``super`` ``.`` ``init``


.. _Expressions_ClosureExpression:

Closure Expression
~~~~~~~~~~~~~~~~~~

A :newTerm:`closure expression` creates a closure,
also known as a *lambda* or an *anonymous function*
in other programming languages.
Like function declarations,
closures contain statements which they execute,
and they capture values from their enclosing scope.
It has the following form:

.. syntax-outline::

   { (<#parameters#>) -> <#return type#> in
      <#statements#>
   }

The *parameters* have the same form
as the parameters in a function declaration,
as described in :ref:`Declarations_FunctionDeclaration`.

There are several special forms
that allow closures to be written more concisely:

* A closure can omit the types
  of its parameters, its return type, or both.
  If you omit the parameter names and both types,
  omit the ``in`` keyword before the statements.
  If the omitted types can't be inferred,
  a compile-time error is raised.

* A closure may omit names for its parameters.
  Its parameters are then implicitly named
  ``$`` followed by their position:
  ``$0``, ``$1``, ``$2``, and so on.

* A closure that consists of only a single expression
  is understood to return the value of that expression.
  The contents of this expression are also considered
  when performing type inference on the surrounding expression.

The following closure expressions are equivalent:

.. testcode:: closure-expression-forms

    -> myFunction {
           (x: Int, y: Int) -> Int in
           return x + y
      }
    ---
    -> myFunction {
           (x, y) in
           return x + y
       }
    ---
    -> myFunction { return $0 + $1 }
    ---
    -> myFunction { $0 + $1 }

For information about passing a closure as an argument to a function,
see :ref:`Expressions_FunctionCallExpression`.

A closure expression can explicitly specify
the values that it captures from the surrounding scope
using a :newTerm:`capture list`.
A capture list is written as a comma separated list surrounded by square brackets,
before the list of parameters.
If you use a capture list, you must also use the ``in`` keyword,
even if you omit the parameter names, parameter types, and return type.

.. Reasonably sure that accessing a variable that you didn't capture should be an error.
   <rdar://problem/17024367> REPL - Accessing a variable not included in the capture list causes a segfault

Each entry in the capture list can be marked as ``weak`` or ``unowned``
to capture a weak or unowned reference to the value.

.. testcode:: closure-expression-weak

    -> myFunction { print(self.title) }                    // strong capture
    -> myFunction { [weak self] in print(self!.title) }    // weak capture
    -> myFunction { [unowned self] in print(self.title) }  // unowned capture

You can also bind an arbitrary expression
to named values in the capture list.
The expression is evaluated when the closure is formed,
and captured with the specified strength.
For example:

.. testcode:: closure-expression-capture

    // Weak capture of "self.parent" as "parent"
    myFunction { [weak parent = self.parent] in print(parent!.title) }

For more information and examples of closure expressions,
see :ref:`Closures_ClosureExpressions`.

.. langref-grammar

    expr-closure ::= '{' closure-signature? brace-item* '}'
    closure-signature ::= pattern-tuple func-signature-result? 'in'
    closure-signature ::= identifier (',' identifier)* func-signature-result? 'in'
    expr-anon-closure-arg ::= dollarident

.. syntax-grammar::

    Grammar of a closure expression

    closure-expression --> ``{`` closure-signature-OPT statements ``}``

    closure-signature --> parameter-clause function-result-OPT ``in``
    closure-signature --> identifier-list function-result-OPT ``in``
    closure-signature --> capture-list parameter-clause function-result-OPT ``in``
    closure-signature --> capture-list identifier-list function-result-OPT ``in``
    closure-signature --> capture-list ``in``

    capture-list --> ``[`` capture-specifier expression ``]``
    capture-specifier --> ``weak`` | ``unowned`` | ``unowned(safe)`` | ``unowned(unsafe)``

.. _Expressions_ImplicitMemberExpression:

Implicit Member Expression
~~~~~~~~~~~~~~~~~~~~~~~~~~

An :newTerm:`implicit member expression`
is an abbreviated way to access a member of a type,
such as an enumeration case or a class method,
in a context where type inference
can determine the implied type.
It has the following form:

.. syntax-outline::

   .<#member name#>

For example:

.. testcode::

    >> enum MyEnumeration { case SomeValue, AnotherValue }
    -> var x = MyEnumeration.SomeValue
    << // x : MyEnumeration = <unprintable value>
    -> x = .AnotherValue

.. langref-grammar

    expr-delayed-identifier ::= '.' identifier

.. syntax-grammar::

    Grammar of a implicit member expression

    implicit-member-expression --> ``.`` identifier


.. _Expressions_ParenthesizedExpression:

Parenthesized Expression
~~~~~~~~~~~~~~~~~~~~~~~~

A :newTerm:`parenthesized expression` consists of
a comma-separated list of expressions surrounded by parentheses.
Each expression can have an optional identifier before it,
separated by a colon (``:``).
It has the following form:

.. syntax-outline::

   (<#identifier 1#>: <#expression 1#>, <#identifier 2#>: <#expression 2#>, <#...#>)

Use parenthesized expressions to create tuples
and to pass arguments to a function call.
If there is only one value inside the parenthesized expression,
the type of the parenthesized expression is the type of that value.
For example,
the type of the parenthesized expression ``(1)``
is ``Int``, not ``(Int)``.

.. langref-grammar

    expr-paren      ::= '(' ')'
    expr-paren      ::= '(' expr-paren-element (',' expr-paren-element)* ')'
    expr-paren-element ::= (identifier ':')? expr


.. syntax-grammar::

    Grammar of a parenthesized expression

    parenthesized-expression --> ``(`` expression-element-list-OPT ``)``
    expression-element-list --> expression-element | expression-element ``,`` expression-element-list
    expression-element --> expression | identifier ``:`` expression


.. _Expressions_WildcardExpression:

Wildcard Expression
~~~~~~~~~~~~~~~~~~~

A :newTerm:`wildcard expression`
is used to explicitly ignore a value during an assignment.
For example, in the following assignment
10 is assigned to ``x`` and 20 is ignored:

.. testcode::

    >> var (x, _) = (10, 20)
    << // (x, _) : (Int, Int) = (10, 20)
    -> (x, _) = (10, 20)
    -> // x is 10, 20 is ignored

.. <rdar://problem/16678866> Assignment to _ from a variable causes a REPL segfault

.. syntax-grammar::

    Grammar of a wildcard expression

    wildcard-expression --> ``_``


.. _Expressions_PostfixExpressions:

Postfix Expressions
-------------------

:newTerm:`Postfix expressions` are formed
by applying a postfix operator or other postfix syntax
to an expression.
Syntactically, every primary expression is also a postfix expression.

.. TR: Does it make sense to call out the left-to-right grouping?

The Swift standard library provides the following postfix operators:

* ``++`` Increment
* ``--`` Decrement

For information about the behavior of these operators,
see :doc:`../LanguageGuide/BasicOperators` and :doc:`../LanguageGuide/AdvancedOperators`.

.. langref-grammar

    expr-postfix  ::= expr-primary
    expr-postfix  ::= expr-postfix operator-postfix
    expr-postfix  ::= expr-new
    expr-postfix  ::= expr-init
    expr-postfix  ::= expr-dot
    expr-postfix  ::= expr-metatype
    expr-postfix  ::= expr-subscript
    expr-postfix  ::= expr-call
    expr-postfix  ::= expr-optional
    expr-force-value  ::= expr-force-value (typo in the langref; lhs should be expr-postfix)

.. syntax-grammar::

    Grammar of a postfix expression

    postfix-expression --> primary-expression
    postfix-expression --> postfix-expression postfix-operator
    postfix-expression --> function-call-expression
    postfix-expression --> initializer-expression
    postfix-expression --> explicit-member-expression
    postfix-expression --> postfix-self-expression
    postfix-expression --> dynamic-type-expression
    postfix-expression --> subscript-expression
    postfix-expression --> forced-value-expression
    postfix-expression --> optional-chaining-expression


.. _Expressions_FunctionCallExpression:

Function Call Expression
~~~~~~~~~~~~~~~~~~~~~~~~

.. TODO: After we rewrite function decls,
   revisit this section to make sure that the names for things match.

A :newTerm:`function call expression` consists of a function name
followed by a comma-separated list of the function's arguments in parentheses.
Function call expressions have the following form:

.. syntax-outline::

    <#function name#>(<#argument value 1#>, <#argument value 2#>)

The *function name* can be any expression whose value is of a function type.

If the function definition includes names for its parameters,
the function call must include names before its argument values
separated by a colon (``:``).
This kind of function call expression has the following form:

.. syntax-outline::

   <#function name#>(<#argument name 1#>: <#argument value 1#>, <#argument name 2#>: <#argument value 2#>)

A function call expression can include a trailing closure
in the form of a closure expression immediately after the closing parenthesis.
The trailing closure is understood as an argument to the function,
added after the last parenthesized argument.
The following function calls are equivalent:

.. testcode:: trailing-closure

    >> func someFunction (x: Int, f: Int -> Bool) -> Bool {
    >>    return f(x)
    >> }
    >> let x = 10
    // someFunction takes an integer and a closure as its arguments
    -> someFunction(x, {$0 == 13})
    <$ : Bool = false
    -> someFunction(x) {$0 == 13}
    <$ : Bool = false

If the trailing closure is the function's only argument,
the parentheses can be omitted.

.. testcode:: no-paren-trailing-closure

    >> class Data {
    >>    let data = 10
    >>    func someMethod(f: Int -> Bool) -> Bool {
    >>       return f(self.data)
    >>    }
    >> }
    >> let myData = Data()
    << // myData : Data = <Data instance>
    // someFunction takes a closure as its only argument
    -> myData.someMethod() {$0 == 13}
    << // r0 : Bool = false
    -> myData.someMethod {$0 == 13}
    << // r1 : Bool = false

.. langref-grammar

    expr-call ::= expr-postfix expr-paren
    expr-trailing-closure ::= expr-postfix expr-closure+

.. syntax-grammar::

    Grammar of a function call expression

    function-call-expression --> postfix-expression parenthesized-expression
    function-call-expression --> postfix-expression parenthesized-expression-OPT trailing-closure
    trailing-closure --> closure-expression

.. Multiple trailing closures in LangRef is an error,
   and so is the trailing typecast,
   per [Contributor 6004] 2014-03-04 email.
   Not documenting those in the prose or grammar
   even though they happen to still work.


.. _Expressions_InitializerExpression:

Initializer Expression
~~~~~~~~~~~~~~~~~~~~~~

An :newTerm:`initializer expression` provides access
to a type's initializer.
It has the following form:

.. syntax-outline::

    <#expression#>.init(<#initializer arguments#>)

You use the initializer expression in a function call expression
to initialize a new instance of a type.
Unlike functions, an initializer can't be used as a value.
For example:

.. testcode::

    >> class SomeClass { class func someClassFunction() {} }
    -> var x = SomeClass.someClassFunction // ok
    << // x : () -> () = <opaque>
    -> var y = SomeClass.init              // error
    !! <REPL Input>:1:19: error: initializer cannot be referenced without arguments
    !! var y = SomeClass.init              // error
    !!                   ^

You also use an initializer expression
to delegate to the initializer of a superclass.

.. testcode::

    -> class SomeSubClass: SomeSuperClass {
    ->     init() {
    ->         // subclass initialization goes here
    ->         super.init()
    ->     }
    -> }

.. langref-grammar

    expr-init ::= expr-postfix '.' 'init'

.. syntax-grammar::

    Grammar of an initializer expression

    initializer-expression --> postfix-expression ``.`` ``init``

.. _Expressions_ExplicitMemberExpression:

Explicit Member Expression
~~~~~~~~~~~~~~~~~~~~~~~~~~

A :newTerm:`explicit member expression` allows access
to the members of a named type, a tuple, or a module.
It consists of a period (``.``) between the item
and the identifier of its member.

.. syntax-outline::

   <#expression#>.<#member name#>

The members of a named type are named
as part of the type's declaration or extension.
For example:

.. testcode::

    -> class SomeClass {
           var someProperty = 42
       }
    -> let c = SomeClass()
    << // c : SomeClass = <C instance>
    -> let y = c.someProperty  // Member access
    << // y : Int = 42

The members of a tuple
are implicitly named using integers in the order they appear,
starting from zero.
For example:

.. testcode::

    -> var t = (10, 20, 30)
    << // t : (Int, Int, Int) = (10, 20, 30)
    -> t.0 = t.1
    -> // Now t is (20, 20, 30)

The members of a module access
the top-level declarations of that module.

.. TR: Confirm?

.. langref-grammar

    expr-dot ::= expr-postfix '.' dollarident
    expr-dot ::= expr-postfix '.' expr-identifier

.. syntax-grammar::

    Grammar of an explicit member expression

    explicit-member-expression --> postfix-expression ``.`` decimal-digit
    explicit-member-expression --> postfix-expression ``.`` identifier generic-argument-clause-OPT


.. _Expressions_PostfixSelfExpression:

Postfix Self Expression
~~~~~~~~~~~~~~~~~~~~~~~

A postfix ``self`` expression consists of an expression or the name of a type,
immediately followed by ``.self``. It has the following forms:

.. syntax-outline::

       <#expression#>.self
       <#type#>.self

The first form evaluates to the value of the *expression*.
For example, ``x.self`` evaluates to ``x``.

The second form evaluates to the value of the *type*. Use this form
to access a type as a value. For example,
because ``SomeClass.self`` evaluates to the ``SomeClass`` type itself,
you can pass it to a function or method that accepts a type-level argument.

.. syntax-grammar::

    Grammar of a self expression

    postfix-self-expression --> postfix-expression ``.`` ``self``


.. _Expressions_DynamicTypeExpression:

Dynamic Type Expression
~~~~~~~~~~~~~~~~~~~~~~~

A ``dynamicType`` expression consists of an expression,
immediately followed by ``.dynamicType``. It has the following form:

.. syntax-outline::

    <#expression#>.dynamicType

The *expression* can't be the name of a type.
The entire ``dynamicType`` expression evaluates to the value of the
runtime type of the *expression*, as the following example shows:

.. testcode::

    -> class SomeBaseClass {
           class func printClassName() {
               println("SomeBaseClass")
           }
       }
    -> class SomeSubClass: SomeBaseClass {
           override class func printClassName() {
               println("SomeSubClass")
           }
       }
    -> let someInstance: SomeBaseClass = SomeSubClass()
    << // someInstance : SomeBaseClass = C4REPL12SomeSubClass (has 1 child)
    -> // someInstance is of type SomeBaseClass at compile time, but
    -> // someInstance is of type SomeSubClass at runtime
    -> someInstance.dynamicType.printClassName()
    <- SomeSubClass

.. syntax-grammar::

    Grammar of a dynamic type expression

    dynamic-type-expression --> postfix-expression ``.`` ``dynamicType``


.. _Expressions_SubscriptExpression:

Subscript Expression
~~~~~~~~~~~~~~~~~~~~

A :newTerm:`subscript expression` provides subscript access
using the getter and setter
of the corresponding subscript declaration.
It has the following form:

.. syntax-outline::

   <#expression#>[<#index expressions#>]

To evaluate the value of a subscript expression,
the subscript getter for the *expression*'s type is called
with the *index expressions* passed as the subscript parameters.
To set its value,
the subscript setter is called in the same way.

.. TR: Confirm that indexing on
   a comma-separated list of expressions
   is intentional, not just a side effect.
   I see this working, for example:
   (swift) class Test {
             subscript(a: Int, b: Int) -> Int { return 12 }
           }
   (swift) var t = Test()
   // t : Test = <Test instance>
   (swift) t[1, 2]
   // r0 : Int = 12

For information about subscript declarations,
see :ref:`Declarations_ProtocolSubscriptDeclaration`.

.. langref-grammar

    expr-subscript ::= expr-postfix '[' expr ']'

.. syntax-grammar::

    Grammar of a subscript expression

    subscript-expression --> postfix-expression ``[`` expression-list ``]``


.. _Expressions_Forced-ValueExpression:

Forced-Value Expression
~~~~~~~~~~~~~~~~~~~~~~~

A :newTerm:`forced-value expression` unwraps an optional value
that you are certain is not ``nil``.
It has the following form:

.. syntax-outline::

   <#expression#>!

If the value of the *expression* is not ``nil``,
the optional value is unwrapped
and returned with the corresponding nonoptional type.
Otherwise, a runtime error is raised.

.. TR: In previous review, we noted that this also does downcast,
   but that doesn't match the REPL's behavior as of swift-600.0.23.1.11
    class A {}
    class B: A {}
    let l: Array<A> = [B(), A(), A()]
    var item: B = l[0] !        // Doesn't parse -- waiting for more expression
    var item: B = l[0]!         // Doesn't typecheck
    var item = l[0] as B!       // Ok

.. langref-grammar

    expr-force-value ::= expr-postfix '!'

.. syntax-grammar::

    Grammar of a forced-value expression

    forced-value-expression --> postfix-expression ``!``


.. _Expression_OptionalChainingOperator:

Optional-Chaining Expression
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

An :newTerm:`optional-chaining expression` provides a simplified syntax
for using optional values in postfix expressions.
It has the following form:

.. syntax-outline::

    <#expression#>?

On its own, the postfix ``?`` operator
simply returns the value of its argument as an optional.

Postfix expressions that contain an optional-chaining expression
are evaluated in a special way.
If the optional-chaining expression is ``nil``,
all of the other operations in the postfix expression are ignored
and the entire postfix expression evaluates to ``nil``.
If the optional-chaining expression is not ``nil``,
the value of the optional-chaining expression is unwrapped
and used to evaluate the rest of the postfix expression.
In either case,
the value of the postfix expression is still of an optional type.

If a postfix expression that contains an optional-chaining expression
is nested inside other postfix expressions,
only the outermost expression returns an optional type.
In the example below,
when ``c`` is not ``nil``,
its value is unwrapped and used to evaluate
both ``.property`` and ``.performAction()``,
and the entire expression ``c?.property.performAction()``
has a value of an optional type.

.. testcode:: optional-chaining

   >> class OtherClass { func performAction() -> Bool {return true} }
   >> class SomeClass { var property: OtherClass = OtherClass() }
   -> var c: SomeClass?
   << // c : SomeClass? = nil
   -> var result: Bool? = c?.property.performAction()
   << // result : Bool? = nil

The following example shows the behavior
of the example above
without using optional chaining.

.. testcode:: optional-chaining-if-let

    -> if let unwrappedC = c {
          result = unwrappedC.property.performAction()
       }

.. langref-grammar

    expr-optional ::= expr-postfix '?'-postfix

.. syntax-grammar::

   Grammar of an optional-chaining expression

   optional-chaining-expression --> postfix-expression ``?``
