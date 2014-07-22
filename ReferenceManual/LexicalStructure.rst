Lexical Structure
=================

The :newTerm:`lexical structure` of Swift describes what sequence of characters
form valid tokens of the language.
These valid tokens form the lowest-level building blocks of the language
and are used to describe the rest of the language in subsequent chapters.
A token consists of an identifier, keyword, punctuation, literal, or operator.

In most cases, tokens are generated from the characters of a Swift source file
by considering the longest possible substring from the input text,
within the constraints of the grammar that are specified below.
This behavior is referred to as :newTerm:`longest match`
or :newTerm:`maximal munch`.


.. _LexicalStructure_WhitespaceAndComments:

Whitespace and Comments
-----------------------

Whitespace has two uses: to separate tokens in the source file
and to help determine whether an operator is a prefix or postfix
(see :ref:`LexicalStructure_Operators`),
but is otherwise ignored.
The following characters are considered whitespace:
space (U+0020),
line feed (U+000A),
carriage return (U+000D),
horizontal tab (U+0009),
vertical tab (U+000B),
form feed (U+000C)
and null (U+0000).

.. Whitespace characters are listed roughly from
   most salient/common to least,
   not in order of Unicode codepoints.

Comments are treated as whitespace by the compiler.
Single line comments begin with ``//``
and continue until a carriage return (U+000D) or line feed (U+000A).
Multiline comments begin with ``/*`` and end with ``*/``.
Nesting multiline comments is allowed,
but the comment markers must be balanced.

.. langref-grammar

    whitespace ::= ' '
    whitespace ::= '\n'
    whitespace ::= '\r'
    whitespace ::= '\t'
    whitespace ::= '\0'

    comment    ::= //.*[\n\r]
    comment    ::= /* .... */

.. ** (Matches the * above, to fix RST syntax highlighting in VIM.)

.. No formal grammar.
   No other syntactic category refers to this one,
   and the prose is sufficient to define it completely.

.. _LexicalStructure_Identifiers:

Identifiers
-----------

:newTerm:`Identifiers` begin with
an uppercase or lowercase letter A through Z,
an underscore (``_``),
a noncombining alphanumeric Unicode character
in the Basic Multilingual Plane,
or a character outside the Basic Multilingual Plane
that isn't in a Private Use Area.
After the first character,
digits and combining Unicode characters are also allowed.

To use a reserved word as an identifier,
put a backtick (:literal:`\``) before and after it.
For example, ``class`` is not a valid identifier,
but :literal:`\`class\`` is valid.
The backticks are not considered part of the identifier;
:literal:`\`x\`` and ``x`` have the same meaning.

Inside a closure with no explicit parameter names,
the parameters are implicitly named ``$0``, ``$1``, ``$2``, and so on.
These names are valid identifiers within the scope of the closure.

.. langref-grammar

    identifier ::= id-start id-continue*
    id-start ::= [A-Za-z_]

    // BMP alphanum non-combining
    id-start ::= [\u00A8\u00AA\u00AD\u00AF\u00B2-\u00B5\u00B7-00BA]
    id-start ::= [\u00BC-\u00BE\u00C0-\u00D6\u00D8-\u00F6\u00F8-\u00FF]
    id-start ::= [\u0100-\u02FF\u0370-\u167F\u1681-\u180D\u180F-\u1DBF]
    id-start ::= [\u1E00-\u1FFF]
    id-start ::= [\u200B-\u200D\u202A-\u202E\u203F-\u2040\u2054\u2060-\u206F]
    id-start ::= [\u2070-\u20CF\u2100-\u218F\u2460-\u24FF\u2776-\u2793]
    id-start ::= [\u2C00-\u2DFF\u2E80-\u2FFF]
    id-start ::= [\u3004-\u3007\u3021-\u302F\u3031-\u303F\u3040-\uD7FF]
    id-start ::= [\uF900-\uFD3D\uFD40-\uFDCF\uFDF0-\uFE1F\uFE30-FE44]
    id-start ::= [\uFE47-\uFFFD]

    // non-BMP non-PUA
    id-start ::= [\u10000-\u1FFFD\u20000-\u2FFFD\u30000-\u3FFFD\u40000-\u4FFFD]
    id-start ::= [\u50000-\u5FFFD\u60000-\u6FFFD\u70000-\u7FFFD\u80000-\u8FFFD]
    id-start ::= [\u90000-\u9FFFD\uA0000-\uAFFFD\uB0000-\uBFFFD\uC0000-\uCFFFD]
    id-start ::= [\uD0000-\uDFFFD\uE0000-\uEFFFD]

    id-continue ::= [0-9]
    // combining
    id-continue ::= [\u0300-\u036F\u1DC0-\u1DFF\u20D0-\u20FF\uFE20-\uFE2F]
    id-continue ::= id-start

    dollarident ::= '$' id-continue+

.. syntax-grammar::

    Grammar of an identifier

    identifier --> identifier-head identifier-characters-OPT
    identifier --> ````` identifier-head identifier-characters-OPT `````
    identifier --> implicit-parameter-name
    identifier-list --> identifier | identifier ``,`` identifier-list

    identifier-head --> Upper- or lowercase letter A through Z
    identifier-head --> ``_``
    identifier-head --> U+00A8, U+00AA, U+00AD, U+00AF, U+00B2--U+00B5, or U+00B7--U+00BA
    identifier-head --> U+00BC--U+00BE, U+00C0--U+00D6, U+00D8--U+00F6, or U+00F8--U+00FF
    identifier-head --> U+0100--U+02FF, U+0370--U+167F, U+1681--U+180D, or U+180F--U+1DBF
    identifier-head --> U+1E00--U+1FFF
    identifier-head --> U+200B--U+200D, U+202A--U+202E, U+203F--U+2040, U+2054, or U+2060--U+206F
    identifier-head --> U+2070--U+20CF, U+2100--U+218F, U+2460--U+24FF, or U+2776--U+2793
    identifier-head --> U+2C00--U+2DFF or U+2E80--U+2FFF
    identifier-head --> U+3004--U+3007, U+3021--U+302F, U+3031--U+303F, or U+3040--U+D7FF
    identifier-head --> U+F900--U+FD3D, U+FD40--U+FDCF, U+FDF0--U+FE1F, or U+FE30--U+FE44
    identifier-head --> U+FE47--U+FFFD
    identifier-head --> U+10000--U+1FFFD, U+20000--U+2FFFD, U+30000--U+3FFFD, or U+40000--U+4FFFD
    identifier-head --> U+50000--U+5FFFD, U+60000--U+6FFFD, U+70000--U+7FFFD, or U+80000--U+8FFFD
    identifier-head --> U+90000--U+9FFFD, U+A0000--U+AFFFD, U+B0000--U+BFFFD, or U+C0000--U+CFFFD
    identifier-head --> U+D0000--U+DFFFD or U+E0000--U+EFFFD

    identifier-character --> Digit 0 through 9
    identifier-character --> U+0300--U+036F, U+1DC0--U+1DFF, U+20D0--U+20FF, or U+FE20--U+FE2F
    identifier-character --> identifier-head
    identifier-characters --> identifier-character identifier-characters-OPT

    implicit-parameter-name --> ``$`` decimal-digits


.. _LexicalStructure_Keywords:

Keywords and Punctuation
------------------------

The following keywords are reserved and can't be used as identifiers,
unless they're escaped with backticks,
as described above in :ref:`LexicalStructure_Identifiers`.

.. langref-grammar

    keyword ::= 'class'
    keyword ::= 'destructor'
    keyword ::= 'extension'
    keyword ::= 'import'
    keyword ::= 'init'
    keyword ::= 'def'
    keyword ::= 'metatype'
    keyword ::= 'enum'
    keyword ::= 'protocol'
    keyword ::= 'type'
    keyword ::= 'struct'
    keyword ::= 'subscript'
    keyword ::= 'typealias'
    keyword ::= 'var'
    keyword ::= 'where'
    keyword ::= 'break'
    keyword ::= 'case'
    keyword ::= 'continue'
    keyword ::= 'default'
    keyword ::= 'do'
    keyword ::= 'else'
    keyword ::= 'if'
    keyword ::= 'in'
    keyword ::= 'for'
    keyword ::= 'return'
    keyword ::= 'switch'
    keyword ::= 'then'
    keyword ::= 'while'
    keyword ::= 'as'
    keyword ::= 'is'
    keyword ::= 'new'
    keyword ::= 'super'
    keyword ::= 'self'
    keyword ::= 'Self'
    keyword ::= '__COLUMN__'
    keyword ::= '__FILE__'
    keyword ::= '__LINE__'

.. NOTE: The LangRef is out of date for keywords. The list of current keywords
    is defined in the file: swift/inclue/swift/Parse/Tokens.def

* Keywords used in declarations:
  ``class``,
  ``deinit``,
  ``enum``,
  ``extension``,
  ``func``,
  ``import``,
  ``init``,
  ``internal``,
  ``let``,
  ``operator``,
  ``private``,
  ``protocol``,
  ``public``,
  ``static``,
  ``struct``,
  ``subscript``,
  ``typealias``,
  and ``var``.

* Keywords used in statements:
  ``break``,
  ``case``,
  ``continue``,
  ``default``,
  ``do``,
  ``else``,
  ``fallthrough``,
  ``for``,
  ``if``,
  ``in``,
  ``return``,
  ``switch``,
  ``where``,
  and ``while``.

* Keywords used in expressions and types:
  ``as``,
  ``dynamicType``,
  ``false``,
  ``is``,
  ``nil``,
  ``self``,
  ``Self``,
  ``super``,
  ``true``,
  ``__COLUMN__``,
  ``__FILE__``,
  ``__FUNCTION__``,
  and ``__LINE__``.

.. langref-grammar

    get
    infix
    operator
    postfix
    prefix
    set
    type

* Keywords reserved in particular contexts:
  ``associativity``,
  ``convenience``,
  ``dynamic``,
  ``didSet``,
  ``final``,
  ``get``,
  ``infix``,
  ``inout``,
  ``lazy``,
  ``left``,
  ``mutating``,
  ``none``,
  ``nonmutating``,
  ``optional``,
  ``override``,
  ``postfix``,
  ``precedence``,
  ``prefix``,
  ``Protocol``,
  ``required``,
  ``right``,
  ``set``,
  ``Type``,
  ``unowned``,
  ``weak``
  and ``willSet``.
  Outside the context in which they appear in the grammar,
  they can be used as identifiers.

The following tokens are reserved as punctuation
and can't be used as custom operators:
``(``, ``)``, ``{``, ``}``, ``[``, ``]``,
``.``, ``,``, ``:``, ``;``, ``=``, ``@``, ``#``,
``&`` (as a prefix operator), ``->``, :literal:`\``,
``?``, and ``!`` (as a postfix operator).

.. _LexicalStructure_Literals:

Literals
--------

A :newTerm:`literal` is the source code representation of a value of a type,
such as a number or string.

The following are examples of literals:

.. testcode::

    -> 42               // Integer literal
    -> 3.14159          // Floating-point literal
    -> "Hello, world!"  // String literal
    -> true             // Boolean literal
    << // r0 : Int = 42
    << // r1 : Double = 3.14159
    << // r2 : String = "Hello, world!"
    << // r3 : Bool = true

A literal doesn't have a type on its own.
Instead, a literal is parsed as having infinite precision and Swift's type inference
attempts to infer a type for the literal. For example,
in the declaration ``var x: Int8 = 42``,
Swift uses the explicit type annotation (``: Int8``) to infer
that the type of the integer literal ``42`` is ``Int8``.
If there isn't suitable type information available,
Swift infers that the literal's type is one of the default literal types
defined in the Swift Standard Library.
The default types are ``Int`` for integer literals, ``Double`` for floating-point literals,
``String`` for string literals, and ``Bool`` for Boolean literals.
For example, in the declaration ``var str = "Hello, world"``,
the default inferred type of the string
literal ``"Hello, world"`` is ``String``.

.. syntax-grammar::

    Grammar of a literal

    literal --> integer-literal | floating-point-literal | string-literal
    literal --> ``true`` | ``false`` | ``nil``


.. _LexicalStructure_IntegerLiterals:


Integer Literals
~~~~~~~~~~~~~~~~

:newTerm:`Integer literals` represent integer values of unspecified precision.
By default, integer literals are expressed in decimal;
you can specify an alternate base using a prefix.
Binary literals begin with ``0b``,
octal literals begin with ``0o``,
and hexadecimal literals begin with ``0x``.

Decimal literals contain the digits ``0`` through ``9``.
Binary literals contain ``0`` and ``1``,
octal literals contain ``0`` through ``7``,
and hexadecimal literals contain ``0`` through ``9``
as well as ``A`` through ``F`` in upper- or lowercase.

Negative integers literals are expressed by prepending a minus sign (``-``)
to an integer literal, as in ``-42``.

Underscores (``_``) are allowed between digits for readability,
but they are ignored and therefore don't affect the value of the literal.
Integer literals can begin with leading zeros (``0``),
but they are likewise ignored and don't affect the base or value of the literal.

Unless otherwise specified,
the default inferred type of an integer literal is the Swift standard library type ``Int``.
The Swift standard library also defines types for various sizes of
signed and unsigned integers,
as described in :ref:`TheBasics_Integers`.

.. TR: The prose assumes underscores only belong between digits.
   Is there a reason to allow them at the end of a literal?
   Java and Ruby both require underscores to be between digits.
   Also, are adjacent underscores meant to be allowed, like 5__000?
   (REPL supports them as of swift-1.21 but it seems odd.)

.. langref-grammar

    integer_literal ::= [0-9][0-9_]*
    integer_literal ::= 0x[0-9a-fA-F][0-9a-fA-F_]*
    integer_literal ::= 0o[0-7][0-7_]*
    integer_literal ::= 0b[01][01_]*

.. NOTE: Updated the langref-grammer to reflect [Contributor 7746]' comment in
    <rdar://problem/15181997> Teach the compiler about a concept of negative integer literals.
    This feels very strange from a grammatical point of view.
    Updated the syntax-grammar below as well.
    Update: This is a parser hack, not a lexer hack. Therefore,
    it's not part of the grammar for integer literal, contrary to [Contributor 2562]'s claim.
    (Doug confirmed this, 4/2/2014.)

.. syntax-grammar::

    Grammar of an integer literal

    integer-literal --> binary-literal
    integer-literal --> octal-literal
    integer-literal --> decimal-literal
    integer-literal --> hexadecimal-literal

    binary-literal --> ``0b`` binary-digit binary-literal-characters-OPT
    binary-digit --> Digit 0 or 1
    binary-literal-character --> binary-digit | ``_``
    binary-literal-characters --> binary-literal-character binary-literal-characters-OPT

    octal-literal --> ``0o`` octal-digit octal-literal-characters-OPT
    octal-digit --> Digit 0 through 7
    octal-literal-character --> octal-digit | ``_``
    octal-literal-characters --> octal-literal-character octal-literal-characters-OPT

    decimal-literal --> decimal-digit decimal-literal-characters-OPT
    decimal-digit --> Digit 0 through 9
    decimal-digits --> decimal-digit decimal-digits-OPT
    decimal-literal-character --> decimal-digit | ``_``
    decimal-literal-characters --> decimal-literal-character decimal-literal-characters-OPT

    hexadecimal-literal --> ``0x`` hexadecimal-digit hexadecimal-literal-characters-OPT
    hexadecimal-digit --> Digit 0 through 9, a through f, or A through F
    hexadecimal-literal-character --> hexadecimal-digit | ``_``
    hexadecimal-literal-characters --> hexadecimal-literal-character hexadecimal-literal-characters-OPT


.. _LexicalStructure_Floating-PointLiterals:

Floating-Point Literals
~~~~~~~~~~~~~~~~~~~~~~~

:newTerm:`Floating-point literals` represent floating-point values of unspecified precision.

By default, floating-point literals are expressed in decimal (with no prefix),
but they can also be expressed in hexadecimal (with a ``0x`` prefix).

.. TODO: Confirm that using a Unicode special x operator below
   rather thas just the letter x is correct.
   This is used in the Guide too.
   APSG entry on 'x' says to use it in screen resolutions
   such as 600 x 800, but doesn't comment on this specific usage.
   Developer Publications SG entry on 'x' says:
   Used in place of a multiplication sign or the word by to describe dimensions: a 50 x 50 pixel resolution.

Decimal floating-point literals consist of a sequence of decimal digits
followed by either a decimal fraction, a decimal exponent, or both.
The decimal fraction consists of a decimal point (``.``)
followed by a sequence of decimal digits.
The exponent consists of an upper- or lowercase ``e`` prefix
followed by a sequence of decimal digits that indicates
what power of 10 the value preceding the ``e`` is multiplied by.
For example, ``1.25e2`` represents 1.25 × 10\ :superscript:`2`,
which evaluates to ``125.0``.
Similarly, ``1.25e-2`` represents 1.25 × 10\ :superscript:`-2`,
which evaluates to ``0.0125``.

Hexadecimal floating-point literals consist of a ``0x`` prefix,
followed by an optional hexadecimal fraction,
followed by a hexadecimal exponent.
The hexadecimal fraction consists of a decimal point
followed by a sequence of hexadecimal digits.
The exponent consists of an upper- or lowercase ``p`` prefix
followed by a sequence of decimal digits that indicates
what power of 2 the value preceding the ``p`` is multiplied by.
For example, ``0xFp2`` represents 15 × 2\ :superscript:`2`,
which evaluates to ``60``.
Similarly, ``0xFp-2`` represents 15 × 2\ :superscript:`-2`,
which evaluates to ``3.75``.

Unlike with integer literals, negative floating-point numbers are expressed
by applying the unary minus operator (``-``)
to a floating-point literal, as in ``-42.0``. The result is an expression,
not a floating-point literal.

Underscores (``_``) are allowed between digits for readability,
but are ignored and therefore don't affect the value of the literal.
Floating-point literals can begin with leading zeros (``0``),
but are likewise ignored and don't affect the base or value of the literal.

Unless otherwise specified,
the default inferred type of a floating-point literal is the Swift standard library type ``Double``,
which represents a 64-bit floating-point number.
The Swift standard library also defines a ``Float`` type,
which represents a 32-bit floating-point number.

.. langref-grammar

    floating_literal ::= [0-9][0-9_]*\.[0-9][0-9_]*
    floating_literal ::= [0-9][0-9_]*\.[0-9][0-9_]*[eE][+-]?[0-9][0-9_]*
    floating_literal ::= [0-9][0-9_]*[eE][+-]?[0-9][0-9_]*
    floating_literal ::= 0x[0-9A-Fa-f][0-9A-Fa-f_]*
                           (\.[0-9A-Fa-f][0-9A-Fa-f_]*)?[pP][+-]?[0-9][0-9_]*

.. syntax-grammar::

    Grammar of a floating-point literal

    floating-point-literal --> decimal-literal decimal-fraction-OPT decimal-exponent-OPT
    floating-point-literal --> hexadecimal-literal hexadecimal-fraction-OPT hexadecimal-exponent

    decimal-fraction --> ``.`` decimal-literal
    decimal-exponent --> floating-point-e sign-OPT decimal-literal

    hexadecimal-fraction --> ``.`` hexadecimal-digit hexadecimal-literal-characters-OPT
    hexadecimal-exponent --> floating-point-p sign-OPT decimal-literal

    floating-point-e --> ``e`` | ``E``
    floating-point-p --> ``p`` | ``P``
    sign --> ``+`` | ``-``


.. _LexicalStructure_StringLiterals:

String Literals
~~~~~~~~~~~~~~~

A string literal is a sequence of characters surrounded by double quotes,
with the following form:

.. syntax-outline::

    "<#characters#>"

String literals cannot contain
an unescaped double quote (``"``),
an unescaped backslash (``\``),
a carriage return, or a line feed.

Special characters
can be included in string literals
using the following escape sequences:

* Null Character (``\0``)
* Backslash (``\\``)
* Horizontal Tab (``\t``)
* Line Feed (``\n``)
* Carriage Return (``\r``)
* Double Quote (``\"``)
* Single Quote (``\'``)
* Unicode scalar (:literal:`\\u{`:emphasis:`n`:literal:`}`), where *n* is between one and eight hexadecimal digits

.. TR: Are \v and \f allowed for vertical tab and formfeed?
   We allow them as whitespace as of now --
   should that mean we want escape sequences for them too?

.. The behavior of \n and \r is not the same as C.
   We specify exactly what those escapes mean.
   The behavior on C is platform dependent --
   in text mode, \n maps to the platform's line separator
   which could be CR or LF or CRLF.

The value of an expression can be inserted into a string literal
by placing the expression in parentheses after a backslash (``\``).
The interpolated expression must not contain
an unescaped double quote (``"``),
an unescaped backslash (``\``),
a carriage return, or a line feed.
The expression must evaluate to a value of a type
that the ``String`` class has an initializer for.

For example, all the following string literals have the same value:

.. testcode::

   -> "1 2 3"
   << // r0 : String = "1 2 3"
   -> "1 2 \(3)"
   << // r1 : String = "1 2 3"
   -> "1 2 \(1 + 2)"
   << // r2 : String = "1 2 3"
   -> let x = 3; "1 2 \(x)"
   << // x : Int = 3
   << // r3 : String = "1 2 3"

The default inferred type of a string literal is ``String``.
The default inferred type of the characters that make up a string
is ``Character``. For more information about the ``String`` and ``Character``
types, see :doc:`../LanguageGuide/StringsAndCharacters`.

.. NOTE: We will have this as a single Unicode char, as well as Char which will be a
   single Unicode grapheme cluster.  Watch for changes around this and the
   single/double quotes grammar coming after WWDC.  For now, it might be best
   to just not document the single quoted character literal, because we know
   that it's going to change.  If we can't make it work right, it's possible we
   would just delete single quoted strings.  Right now, iterating over a String
   returns a sequence of UnicodeScalar values.  In the fullness of time, it
   should return a sequence of Char values.

.. langref-grammar

    character_literal ::= '[^'\\\n\r]|character_escape'
    character_escape  ::= [\]0 [\][\] | [\]t | [\]n | [\]r | [\]" | [\]'
    character_escape  ::= [\]x hex hex
    character_escape  ::= [\]u hex hex hex hex
    character_escape  ::= [\]U hex hex hex hex hex hex hex hex

    string_literal   ::= ["]([^"\\\n\r]|character_escape|escape_expr)*["]
    escape_expr      ::= [\]escape_expr_body
    escape_expr_body ::= [(]escape_expr_body[)]
    escape_expr_body ::= [^\n\r"()]

.. syntax-grammar::

    Grammar of a string literal

    string-literal --> ``"`` quoted-text-OPT ``"``
    quoted-text --> quoted-text-item quoted-text-OPT
    quoted-text-item --> escaped-character
    quoted-text-item --> ``\(`` expression ``)``
    quoted-text-item --> Any Unicode extended grapheme cluster except ``"``, ``\``, U+000A, or U+000D

    escaped-character --> ``\0`` | ``\\`` | ``\t`` | ``\n`` | ``\r`` | ``\"`` | ``\'``
    escaped-character --> ``\u`` ``{`` unicode-scalar-digits ``}``
    unicode-scalar-digits --> Between one and eight hexadecimal digits

.. Quoted text resolves to a sequence of escaped characters by way of
   the quoted-texts rule which allows repetition; no need to allow
   repetition in the quoted-text/escaped-character rule too.

.. Now that single quotes are gone, we don't have a character literal.
   Because we may one bring them back, here's the old grammar for them:

   textual-literal --> character-literal | string-literal

   character-literal --> ``'`` quoted-character ``'``
   quoted-character --> escaped-character
   quoted-character --> Any Unicode extended grapheme cluster except ``'``, ``\``, U+000A, or U+000D


.. _LexicalStructure_Operators:

Operators
---------

The Swift standard library defines a number of operators for your use,
many of which are discussed in :doc:`../LanguageGuide/BasicOperators`
and :doc:`../LanguageGuide/AdvancedOperators`.
The present section describes which characters can be used to define custom operators.

Custom operators can begin with one of the ASCII characters
``/``, ``=``, ``-``, ``+``, ``!``, ``*``, ``%``, ``<``, ``>``,
``&``, ``|``, ``^``, or ``~``, or one of the Unicode characters
defined in the grammar below. After the first character,
combining Unicode characters are also allowed.
You can also define custom operators as a sequence of two or more dots (for example, ``....``).

.. note::

   The tokens ``=``, ``->``, ``//``, ``/*``, ``*/``, ``.``,
   and the prefix operator ``&`` are reserved.
   These tokens can't be overloaded, nor can they be used as custom operators.

The whitespace around an operator is used to determine
whether an operator is used as a prefix operator, a postfix operator,
or a binary operator. This behavior is summarized in the following rules:

* If an operator has whitespace around both sides or around neither side,
  it is treated as a binary operator.
  As an example, the ``+`` operator in ``a+b`` and ``a + b`` is treated as a binary operator.
* If an operator has whitespace on the left side only,
  it is treated as a prefix unary operator.
  As an example, the ``++`` operator in ``a ++b`` is treated as a prefix unary operator.
* If an operator has whitespace on the right side only,
  it is treated as a postfix unary operator.
  As an example, the ``++`` operator in ``a++ b`` is treated as a postfix unary operator.
* If an operator has no whitespace on the left but is followed immediately by a dot (``.``),
  it is treated as a postfix unary operator.
  As an example, the  ``++`` operator in ``a++.b`` is treated as a postfix unary operator
  (``a++ .b`` rather than ``a ++ .b``).

For the purposes of these rules,
the characters ``(``, ``[``, and ``{`` before an operator,
the characters ``)``, ``]``, and ``}`` after an operator,
and the characters ``,``, ``;``, and ``:``
are also considered whitespace.

There is one caveat to the rules above.
If the ``!`` or ``?`` predefined operator has no whitespace on the left,
it is treated as a postfix operator,
regardless of whether it has whitespace on the right.
To use the ``?`` as the optional-chaining operator,
it must not have whitespace on the left.
To use it in the ternary conditional (``?`` ``:``) operator,
it must have whitespace around both sides.

In certain constructs, operators with a leading ``<`` or ``>``
may be split into two or more tokens. The remainder is treated the same way
and may be split again. As a result, there is no need to use whitespace
to disambiguate between the closing ``>`` characters in constructs like
``Dictionary<String, Array<Int>>``.
In this example, the closing ``>`` characters are not treated as a single token
that may then be misinterpreted as a bit shift ``>>`` operator.

.. NOTE: Once the parser sees a < it goes into a pre-scanning lookahead mode.  It
   matches < and > and looks at what token comes after the > -- if it's a . or
   a ( it treats the <...> as a generic parameter list, otherwise it treats
   them as less than and greater than.

   This fails to parse things like x<<2>>(1+2) but it's the same as C#.  So
   don't write that.

To learn how to define new, custom operators,
see :ref:`AdvancedOperators_CustomOperators` and :ref:`Declarations_OperatorDeclaration`.
To learn how to overload existing operators,
see :ref:`AdvancedOperators_OperatorFunctions`.

.. langref-grammar

    operator ::= [/=-+*%<>!&|^~]+
    operator ::= \.+

      Note: excludes '=', see [1]
            excludes '->', see [2]
            excludes unary '&', see [3]
            excludes '//', '/*', and '*/', see [4]

    operator-binary ::= operator
    operator-prefix ::= operator
    operator-postfix ::= operator

    left-binder  ::= [ \r\n\t\(\[\{,;:]
    right-binder ::= [ \r\n\t\)\]\},;:]

    any-identifier ::= identifier | operator

.. langref-grammar

    punctuation ::= '('
    punctuation ::= ')'
    punctuation ::= '{'
    punctuation ::= '}'
    punctuation ::= '['
    punctuation ::= ']'
    punctuation ::= '.'
    punctuation ::= ','
    punctuation ::= ';'
    punctuation ::= ':'
    punctuation ::= '='
    punctuation ::= '->'
    punctuation ::= '&' // unary prefix operator

.. NOTE: The ? is a reserved punctuation.  Optional-chaining (foo?.bar) is actually a
   monad -- the ? is actually a monadic bind operator.  It is like a burrito.
   The current list of reserved punctuation is in Tokens.def.

.. syntax-grammar::

    Grammar of operators

    operator --> operator-head operator-characters-OPT
    operator --> dot-operator-head dot-operator-characters-OPT

    operator-head --> ``/`` | ``=`` | ``-`` | ``+`` | ``!`` | ``*`` | ``%`` | ``<`` | ``>`` | ``&`` | ``|`` | ``^`` | ``~``
    operator-head --> U+00A1--U+00A7
    operator-head --> U+00A9 or U+00AB
    operator-head --> U+00AC or U+00AE
    operator-head --> U+00B0--U+00B1, U+00B6, U+00BB, U+00BF, U+00D7, or U+00F7
    operator-head --> U+2016--U+2017 or U+2020--U+2027
    operator-head --> U+2030--U+203E
    operator-head --> U+2041--U+2053
    operator-head --> U+2055--U+205E
    operator-head --> U+2190--U+23FF
    operator-head --> U+2500--U+2775
    operator-head --> U+2794--U+2BFF
    operator-head --> U+2E00--U+2E7F
    operator-head --> U+3001--U+3003
    operator-head --> U+3008--U+3030

    operator-character --> operator-head
    operator-character --> U+0300--U+036F
    operator-character --> U+1DC0--U+1DFF
    operator-character --> U+20D0--U+20FF
    operator-character --> U+FE00--U+FE0F
    operator-character --> U+FE20--U+FE2F
    operator-character --> U+E0100--U+E01EF
    operator-characters --> operator-character operator-characters-OPT

    dot-operator-head --> ``..``
    dot-operator-character --> ``.`` | operator-character
    dot-operator-characters --> dot-operator-character dot-operator-characters-OPT

    binary-operator --> operator
    prefix-operator --> operator
    postfix-operator --> operator
