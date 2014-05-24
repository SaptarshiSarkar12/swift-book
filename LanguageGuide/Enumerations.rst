Enumerations
============

:newTerm:`Enumerations` are a way to define multiple related values of a similar kind
and to work with those values in a type-safe way within your code.

If you are familiar with C,
you will know that C enumerations assign related names to a set of integer values.
Enumerations in Swift are much more flexible,
and do not have to provide a value for each member of the enumeration.
If a value (known as a “raw” value) *is* provided for each enumeration member,
the value can be a string, or a character, or any of the integer or floating-point number types.

Alternatively, enumeration members can specify
associated values of *any* type to be stored along with each different member value,
in a similar way to unions or variants in other languages.
You can define a common set of related members as part of one enumeration,
each of which has a different set of values of appropriate types associated with it.

Enumerations in Swift are first-class types in their own right.
They adopt many features traditionally supported only by classes,
such as computed properties to provide additional information about their values,
and instance methods to provide functionality related to the values they represent.
Enumerations can also define initializers to provide an initial member value;
can be extended to expand their functionality beyond their original implementation;
and can conform to protocols to provide standard functionality.

For more on these capabilities, see
:doc:`Properties`, :doc:`Methods`, :doc:`Initialization`,
:doc:`Extensions`, and :doc:`Protocols`.

.. TODO: this chapter should probably mention that enums without associated values
   are hashable and equatable by default (and what that means in practice)

.. _Enumerations_EnumerationSyntax:

Enumeration Syntax
------------------

You introduce enumerations with the ``enum`` keyword
and place their entire definition within a pair of braces:

.. testcode:: enums

   -> enum SomeEnumeration {
         // enumeration definition goes here
      }

Here's an example for the four main points of a compass:

.. testcode:: enums

   -> enum CompassPoint {
         case North
         case South
         case East
         case West
      }

The values defined in an enumeration
(such as ``North``, ``South``, ``East``, and ``West``)
are the :newTerm:`member values` (or :newTerm:`members`) of that enumeration.
The ``case`` keyword indicates that a new line of member values is about to be defined.

.. note::

   Unlike C and Objective-C,
   Swift enumeration members are not assigned a default integer value when they are created.
   In the ``CompassPoints`` example above,
   ``North``, ``South``, ``East`` and ``West``
   do not implicitly equal
   ``0``, ``1``, ``2`` and ``3``.
   Instead, the different enumeration members are fully-fledged values in their own right,
   with an explicitly-defined type of ``CompassPoint``.

Multiple member values can appear on a single line, separated by commas:

.. testcode:: enums

   -> enum Planet {
         case Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
      }

Each enumeration definition defines a brand new type.
Like other types in Swift, their names
(such as ``CompassPoint`` and ``Planet``)
should start with a capital letter.
Give enumeration types singular rather than plural names,
so that they read as self-evident:

.. testcode:: enums

   -> var directionToHead = CompassPoint.West
   << // directionToHead : CompassPoint = <opaque>

The type of ``directionToHead`` is inferred
when it is initialized with one of the possible values of ``CompassPoint``.
Once ``directionToHead`` is declared as a ``CompassPoint``,
you can set it to a different ``CompassPoint`` value using a shorter dot syntax:

.. testcode:: enums

   -> directionToHead = .East

The type of ``directionToHead`` is already known,
and so you can drop the type when setting its value.
This makes for highly readable code when working with explicitly-typed enumeration values.

.. _Enumerations_MatchingEnumerationValuesWithASwitchStatement:

Matching Enumeration Values with a Switch Statement
---------------------------------------------------

You can match individual enumeration values with a ``switch`` statement:

.. testcode:: enums

   -> directionToHead = .South
   -> switch directionToHead {
         case .North:
            println("Lots of planets have a north")
         case .South:
            println("Watch out for penguins")
         case .East:
            println("Where the sun rises")
         case .West:
            println("Where the skies are blue")
      }
   <- Watch out for penguins

You can read this code as:

“Consider the value of ``directionToHead``.
In the case where it equals ``.North``,
print ``"Lots of planets have a north"``.
In the case where it equals ``.South``,
print ``"Watch out for penguins"``.”

…and so on.

As described in :doc:`ControlFlow`,
a ``switch`` statement must be exhaustive when considering an enumeration's members.
If the ``case`` for ``.West`` is omitted,
this code does not compile,
because it does not consider the complete list of ``CompassPoint`` members.
Requiring exhaustiveness ensures that enumeration members are not accidentally omitted.

When it is not appropriate to provide a ``case`` for every enumeration member,
you can provide a ``default`` case to cover any members that are not addressed explicitly:

.. testcode:: enums

   -> let somePlanet = Planet.Earth
   << // somePlanet : Planet = <opaque>
   -> switch somePlanet {
         case .Earth:
            println("Mostly harmless")
         default:
            println("Not a safe place for humans")
      }
   <- Mostly harmless

.. _Enumerations_AssociatedValues:

Associated Values
-----------------

The examples in the previous section show how the members of an enumeration are
a defined (and typed) value in their own right.
You can set a constant or variable to ``Planet.Earth``,
and check for this value later.
However, it is sometimes useful to be able to store
:newTerm:`associated values` of other types alongside these member values.
This enables you to store additional custom information along with the member value,
and for this information to vary each time you use that member in your code.

You can define Swift enumerations to store associated values of any given type,
and the value types can be different for each member of the enumeration if needed.
Enumerations similar to these are known as
:newTerm:`discriminated unions`, :newTerm:`tagged unions`, or :newTerm:`variants`
in other programming languages.

For example, suppose an inventory tracking system needs to
track products by two different types of barcode.
Some products are labeled with 1D barcodes in UPC-A format,
which uses the numbers ``0`` to ``9``.
Each barcode has a “number system” digit,
followed by ten “identifier” digits.
These are followed by a “check” digit to verify that the code has been scanned correctly:

.. image:: ../images/barcode_UPC_2x.png
   :align: center

Other products are labeled with 2D barcodes in QR code format,
which can use any ISO 8859-1 character
and can encode a string up to 2,953 characters long:

.. image:: ../images/barcode_QR_2x.png
   :align: center

It would be convenient for an inventory tracking system to be able to store UPC-A barcodes
as a tuple of three integers,
and QR code barcodes as a string of any length.

In Swift, an enumeration to define product barcodes of either type might look like this:

.. testcode:: enums

   -> enum Barcode {
         case UPCA(Int, Int, Int)
         case QRCode(String)
      }

This can be read as:

“Define an enumeration type called ``Barcode``,
which can take either a value of ``UPCA``
with an associated value of type (``Int``, ``Int``, ``Int``),
or a value of ``QRCode`` with an associated value of type ``String``.”

This definition does not provide any actual ``Int`` or ``String`` values ---
it just defines the *type* of associated values
that ``Barcode`` constants and variables can store
when they are equal to ``Barcode.UPCA`` or ``Barcode.QRCode``.

New barcodes can then be created using either type:

.. testcode:: enums

   -> var productBarcode = Barcode.UPCA(8, 85909_51226, 3)
   << // productBarcode : Barcode = <opaque>

This example creates a new variable called ``productBarcode``
and assigns it a value of ``Barcode.UPCA``
with an associated tuple value of ``(8, 8590951226, 3)``.
The provided “identifier” value has an underscore within its integer literal ---
``85909_51226`` --- to make it easier to read as a barcode.

The same product can be assigned a different type of barcode:

.. testcode:: enums

   -> productBarcode = .QRCode("ABCDEFGHIJKLMNOP")

At this point,
the original ``Barcode.UPCA`` and its integer values are replaced by
the new ``Barcode.QRCode`` and its string value.
Constants and variables of type ``Barcode`` can store either a ``.UPCA`` or a ``.QRCode``
(together with their associated values),
but they can only store one of them at any given time.

The different barcode types can be checked using a switch statement, as before.
This time, however, the associated values can be extracted as part of the switch statement.
You extract each associated value as a constant (with the ``let`` prefix)
or a variable (with the ``var`` prefix)
for use within the ``switch`` case's body:

.. testcode:: enums

   -> switch productBarcode {
         case .UPCA(let numberSystem, let identifier, let check):
            println("UPC-A with value of \(numberSystem), \(identifier), \(check).")
         case .QRCode(let productCode):
            println("QR code with value of \(productCode).")
      }
   <- QR code with value of ABCDEFGHIJKLMNOP.

If all of the associated values for a enumeration member
are extracted as constants, or if all are extracted as variables,
you can place a single ``var`` or ``let`` annotation before the member name, for brevity:

.. testcode:: enums

   -> switch productBarcode {
         case let .UPCA(numberSystem, identifier, check):
            println("UPC-A with value of \(numberSystem), \(identifier), \(check).")
         case let .QRCode(productCode):
            println("QR code with value of \(productCode).")
      }
   <- QR code with value of ABCDEFGHIJKLMNOP.

.. _Enumerations_RawValues:

Raw Values
----------

The barcode example in :ref:`Enumerations_AssociatedValues`
shows how members of an enumeration can declare that they store
associated values of different types.
In addition to associated values,
enumeration members can come prepopulated with default values
(called :newTerm:`raw values`),
which are all of the same type.

.. QUESTION: it's not really "in addition to", it's "alternatively" - does this matter?

Here's an example that stores raw ASCII values alongside named enumeration members:

.. testcode:: rawValues

   -> enum ASCIIControlCharacter: Character {
         case Tab = "\t"
         case LineFeed = "\n"
         case CarriageReturn = "\r"
      }

Here, the raw values for an enumeration called ``ASCIIControlCharacter``
are defined to be of type ``Character``,
and are set to some of the more common ASCII control characters.
``Character`` values are described in :doc:`StringsAndCharacters`.

Note that raw values are *not* the same as associated values.
Raw values are set to prepopulated values
when you first define the enumeration in your code,
like the three ASCII codes above.
The raw value for a particular enumeration member is always the same.
Associated values are set when you create a new constant or variable
based on one of the enumeration's members,
and can be different each time you do so.

Raw values can be
strings, characters, or any of the integer or floating-point number types.
Each raw value must be unique within its enumeration declaration.
When integers are used for raw values,
they auto-increment if no value is specified for some of the enumeration members.

The enumeration below is a refinement of the earlier ``Planet`` enumeration,
with raw integer values to represent each planet's order from the sun:

.. testcode:: rawValues

   -> enum Planet: Int {
         case Mercury = 1, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
      }

Auto-incrementation means that ``Planet.Venus`` has a raw value of ``2``, and so on.

Access the raw value of an enumeration member with its ``toRaw`` method:

.. testcode:: rawValues

   -> let earthsOrder = Planet.Earth.toRaw()
   << // earthsOrder : Int = 3
   /> earthsOrder is \(earthsOrder)
   </ earthsOrder is 3

Use an enumeration's ``fromRaw`` method
to try to find an enumeration member with a particular raw value.
This example identifies Uranus from its raw value of ``7``:

.. testcode:: rawValues

   -> let possiblePlanet = Planet.fromRaw(7)
   << // possiblePlanet : Planet? = <opaque>
   // possiblePlanet is of type Planet? and equals Planet.Uranus

Not all possible ``Int`` values will find a matching planet, however.
Because of this, the ``fromRaw`` method returns an *optional* enumeration member.
In the example above, ``possiblePlanet`` is of type ``Planet?``,
or “optional ``Planet``.”

If you try to find a Planet with a position of ``9``,
the optional ``Planet`` value returned by ``fromRaw`` will be ``nil``:

.. testcode:: rawValues

   -> let positionToFind = 9
   << // positionToFind : Int = 9
   -> if let somePlanet = Planet.fromRaw(positionToFind) {
         switch somePlanet {
            case .Earth:
               println("Mostly harmless")
            default:
               println("Not a safe place for humans")
         }
      } else {
         println("There isn't a planet at position \(positionToFind)")
      }
   <- There isn't a planet at position 9

This example uses optional binding to try to access a planet with a raw value of ``9``.
The statement ``if let somePlanet = Planet.fromRaw(9)`` retrieves an optional ``Planet``,
and sets ``somePlanet`` to the contents of that optional ``Planet`` if it can be retrieved.
In this case, it is not possible to retrieve a planet with a position of ``9``,
and so the ``else`` branch is executed instead.

.. TODO: Switch around the order of this chapter so that all of the non-union stuff
   is together, and the union bits (aka Associated Values) come last.
