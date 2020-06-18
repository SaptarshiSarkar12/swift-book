Extensions
==========

:newTerm:`Extensions` add new functionality to an existing
class, structure, enumeration, or protocol type.
This includes the ability to extend types
for which you don't have access to the original source code
(known as :newTerm:`retroactive modeling`).
Extensions are similar to categories in Objective-C.
(Unlike Objective-C categories, Swift extensions don't have names.)

Extensions in Swift can:

* Add computed instance properties and computed type properties
* Define instance methods and type methods
* Provide new initializers
* Define subscripts
* Define and use new nested types
* Make an existing type conform to a protocol

In Swift,
you can even extend a protocol to provide implementations of its requirements
or add additional functionality that conforming types can take advantage of.
For more details, see :ref:`Protocols_Extensions`.

.. note::

   Extensions can add new functionality to a type,
   but they can't override existing functionality.

.. assertion:: extensionsCannotOverrideExistingBehavior

   -> class C {
         var x = 0
         func foo() {}
      }
   -> extension C {
         override var x: Int {
            didSet {
               print("new x is \(x)")
            }
         }
         override func foo() {
            print("called overridden foo")
         }
      }
   !$ error: property does not override any property from its superclass
   !! override var x: Int {
   !! ~~~~~~~~     ^
   !$ error: invalid redeclaration of 'x'
   !! override var x: Int {
   !!              ^
   !$ note: 'x' previously declared here
   !! var x = 0
   !!     ^
   !$ error: method does not override any method from its superclass
   !! override func foo() {
   !! ~~~~~~~~      ^
   !$ error: invalid redeclaration of 'foo()'
   !! override func foo() {
   !!               ^
   !$ note: 'foo()' previously declared here
   !! func foo() {}
   !!      ^

.. _Extensions_ExtensionSyntax:

Extension Syntax
----------------

Declare extensions with the ``extension`` keyword:

.. testcode:: extensionSyntax

   >> struct SomeType {}
   -> extension SomeType {
         // new functionality to add to SomeType goes here
      }

An extension can extend an existing type to make it adopt one or more protocols.
To add protocol conformance,
you write the protocol names
the same way as you write them for a class or structure:

.. testcode:: extensionSyntax

   >> protocol SomeProtocol {}
   >> protocol AnotherProtocol {}
   -> extension SomeType: SomeProtocol, AnotherProtocol {
         // implementation of protocol requirements goes here
      }

Adding protocol conformance in this way is described in
:ref:`Protocols_AddingProtocolConformanceWithAnExtension`.

An extension can be used to extend an existing generic type,
as described in :ref:`Generics_ExtendingAGenericType`.
You can also extend a generic type to conditionally add functionality,
as described in :ref:`Generics_ExtensionWithWhereClause`.

.. note::

   If you define an extension to add new functionality to an existing type,
   the new functionality will be available on all existing instances of that type,
   even if they were created before the extension was defined.

.. _Extensions_ComputedProperties:

Computed Properties
-------------------

Extensions can add computed instance properties and computed type properties to existing types.
This example adds five computed instance properties to Swift's built-in ``Double`` type,
to provide basic support for working with distance units:

.. testcode:: extensionsComputedProperties

   -> extension Double {
         var km: Double { return self * 1_000.0 }
         var m: Double { return self }
         var cm: Double { return self / 100.0 }
         var mm: Double { return self / 1_000.0 }
         var ft: Double { return self / 3.28084 }
      }
   -> let oneInch = 25.4.mm
   -> print("One inch is \(oneInch) meters")
   <- One inch is 0.0254 meters
   -> let threeFeet = 3.ft
   -> print("Three feet is \(threeFeet) meters")
   <- Three feet is 0.914399970739201 meters

These computed properties express that a ``Double`` value
should be considered as a certain unit of length.
Although they are implemented as computed properties,
the names of these properties can be appended to
a floating-point literal value with dot syntax,
as a way to use that literal value to perform distance conversions.

In this example, a ``Double`` value of ``1.0`` is considered to represent “one meter”.
This is why the ``m`` computed property returns ``self`` ---
the expression ``1.m`` is considered to calculate a ``Double`` value of ``1.0``.

Other units require some conversion to be expressed as a value measured in meters.
One kilometer is the same as 1,000 meters,
so the ``km`` computed property multiplies the value by ``1_000.00``
to convert into a number expressed in meters.
Similarly, there are 3.28084 feet in a meter,
and so the ``ft`` computed property divides the underlying ``Double`` value
by ``3.28084``, to convert it from feet to meters.

These properties are read-only computed properties,
and so they are expressed without the ``get`` keyword, for brevity.
Their return value is of type ``Double``,
and can be used within mathematical calculations wherever a ``Double`` is accepted:

.. testcode:: extensionsComputedProperties

   -> let aMarathon = 42.km + 195.m
   -> print("A marathon is \(aMarathon) meters long")
   <- A marathon is 42195.0 meters long

.. note::

   Extensions can add new computed properties, but they can't add stored properties,
   or add property observers to existing properties.

.. assertion:: extensionsCannotAddStoredProperties

   -> class C {}
   -> extension C { var x = 0 }
   !$ error: extensions must not contain stored properties
   !! extension C { var x = 0 }
   !!                   ^

.. TODO: change this example to something more advisable / less contentious.

.. _Extensions_Initializers:

Initializers
------------

Extensions can add new initializers to existing types.
This enables you to extend other types to accept
your own custom types as initializer parameters,
or to provide additional initialization options
that were not included as part of the type's original implementation.

Extensions can add new convenience initializers to a class,
but they can't add new designated initializers or deinitializers to a class.
Designated initializers and deinitializers
must always be provided by the original class implementation.

If you use an extension to add an initializer to a value type that provides
default values for all of its stored properties
and doesn't define any custom initializers,
you can call the default initializer and memberwise initializer for that value type
from within your extension's initializer.
This wouldn't be the case if you had written the initializer
as part of the value type's original implementation,
as described in :ref:`Initialization_InitializerDelegationForValueTypes`.

If you use an extension to add an initializer to a structure
that was declared in another module,
the new initializer can't access ``self`` until it calls
an initializer from the defining module.

The example below defines a custom ``Rect`` structure to represent a geometric rectangle.
The example also defines two supporting structures called ``Size`` and ``Point``,
both of which provide default values of ``0.0`` for all of their properties:

.. testcode:: extensionsInitializers

   -> struct Size {
         var width = 0.0, height = 0.0
      }
   -> struct Point {
         var x = 0.0, y = 0.0
      }
   -> struct Rect {
         var origin = Point()
         var size = Size()
      }

Because the ``Rect`` structure provides default values for all of its properties,
it receives a default initializer and a memberwise initializer automatically,
as described in :ref:`Initialization_DefaultInitializers`.
These initializers can be used to create new ``Rect`` instances:

.. testcode:: extensionsInitializers

   -> let defaultRect = Rect()
   -> let memberwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
         size: Size(width: 5.0, height: 5.0))

You can extend the ``Rect`` structure to provide an additional initializer
that takes a specific center point and size:

.. testcode:: extensionsInitializers

   -> extension Rect {
         init(center: Point, size: Size) {
            let originX = center.x - (size.width / 2)
            let originY = center.y - (size.height / 2)
            self.init(origin: Point(x: originX, y: originY), size: size)
         }
      }

This new initializer starts by calculating an appropriate origin point based on
the provided ``center`` point and ``size`` value.
The initializer then calls the structure's automatic memberwise initializer
``init(origin:size:)``, which stores the new origin and size values
in the appropriate properties:

.. testcode:: extensionsInitializers

   -> let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
         size: Size(width: 3.0, height: 3.0))
   /> centerRect's origin is (\(centerRect.origin.x), \(centerRect.origin.y)) and its size is (\(centerRect.size.width), \(centerRect.size.height))
   </ centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)

.. note::

   If you provide a new initializer with an extension,
   you are still responsible for making sure that each instance is fully initialized
   once the initializer completes.

.. _Extensions_Methods:

Methods
-------

Extensions can add new instance methods and type methods to existing types.
The following example adds a new instance method called ``repetitions`` to the ``Int`` type:

.. testcode:: extensionsInstanceMethods

   -> extension Int {
         func repetitions(task: () -> Void) {
            for _ in 0..<self {
               task()
            }
         }
      }

The ``repetitions(task:)`` method takes a single argument of type ``() -> Void``,
which indicates a function that has no parameters and doesn't return a value.

After defining this extension,
you can call the ``repetitions(task:)`` method on any integer
to perform a task that many number of times:

.. testcode:: extensionsInstanceMethods

   -> 3.repetitions {
         print("Hello!")
      }
   </ Hello!
   </ Hello!
   </ Hello!

.. _Extensions_MutatingInstanceMethods:

Mutating Instance Methods
~~~~~~~~~~~~~~~~~~~~~~~~~

Instance methods added with an extension can also modify (or *mutate*) the instance itself.
Structure and enumeration methods that modify ``self`` or its properties
must mark the instance method as ``mutating``,
just like mutating methods from an original implementation.

The example below adds a new mutating method called ``square`` to Swift's ``Int`` type,
which squares the original value:

.. testcode:: extensionsInstanceMethods

   -> extension Int {
         mutating func square() {
            self = self * self
         }
      }
   -> var someInt = 3
   -> someInt.square()
   /> someInt is now \(someInt)
   </ someInt is now 9

.. _Extensions_Subscripts:

Subscripts
----------

Extensions can add new subscripts to an existing type.
This example adds an integer subscript to Swift's built-in ``Int`` type.
This subscript ``[n]`` returns the decimal digit ``n`` places in
from the right of the number:

* ``123456789[0]`` returns ``9``
* ``123456789[1]`` returns ``8``

…and so on:

.. testcode:: extensionsSubscripts

   -> extension Int {
         subscript(digitIndex: Int) -> Int {
            var decimalBase = 1
            for _ in 0..<digitIndex {
               decimalBase *= 10
            }
            return (self / decimalBase) % 10
         }
      }
   >> let r0 =
   -> 746381295[0]
   /> returns \(r0)
   </ returns 5
   >> let r1 =
   -> 746381295[1]
   /> returns \(r1)
   </ returns 9
   >> let r2 =
   -> 746381295[2]
   /> returns \(r2)
   </ returns 2
   >> let r3 =
   -> 746381295[8]
   /> returns \(r3)
   </ returns 7

.. x*  Bogus * paired with the one in the listing, to fix VIM syntax highlighting.

.. Rewrite the above to avoid bare expressions.
   Tracking bug is <rdar://problem/35301593>

.. TODO: Replace the for loop above with an exponent,
   if/when integer exponents land in the stdlib.
   Darwin's pow() function is only for floating point.

If the ``Int`` value doesn't have enough digits for the requested index,
the subscript implementation returns ``0``,
as if the number had been padded with zeros to the left:

.. testcode:: extensionsSubscripts

   >> let r4 =
   -> 746381295[9]
   /> returns \(r4), as if you had requested:
   </ returns 0, as if you had requested:
   >> let r5 =
   -> 0746381295[9]

.. TODO: provide an explanation of this example

.. Rewrite the above to avoid bare expressions.
   Tracking bug is <rdar://problem/35301593>


.. _Extensions_NestedTypes:

Nested Types
------------

Extensions can add new nested types to existing classes, structures, and enumerations:

.. testcode:: extensionsNestedTypes

   -> extension Int {
         enum Kind {
            case negative, zero, positive
         }
         var kind: Kind {
            switch self {
               case 0:
                  return .zero
               case let x where x > 0:
                  return .positive
               default:
                  return .negative
            }
         }
      }

This example adds a new nested enumeration to ``Int``.
This enumeration, called ``Kind``,
expresses the kind of number that a particular integer represents.
Specifically, it expresses whether the number is
negative, zero, or positive.

This example also adds a new computed instance property to ``Int``,
called ``kind``,
which returns the appropriate ``Kind`` enumeration case for that integer.

The nested enumeration can now be used with any ``Int`` value:

.. testcode:: extensionsNestedTypes

   -> func printIntegerKinds(_ numbers: [Int]) {
         for number in numbers {
            switch number.kind {
               case .negative:
                  print("- ", terminator: "")
               case .zero:
                  print("0 ", terminator: "")
               case .positive:
                  print("+ ", terminator: "")
            }
         }
         print("")
      }
   -> printIntegerKinds([3, 19, -27, 0, -6, 0, 7])
   << + + - 0 - 0 +
   // Prints "+ + - 0 - 0 + "

.. Workaround for rdar://26016325

This function, ``printIntegerKinds(_:)``,
takes an input array of ``Int`` values and iterates over those values in turn.
For each integer in the array,
the function considers the ``kind`` computed property for that integer,
and prints an appropriate description.

.. note::

   ``number.kind`` is already known to be of type ``Int.Kind``.
   Because of this, all of the ``Int.Kind`` case values
   can be written in shorthand form inside the ``switch`` statement,
   such as ``.negative`` rather than ``Int.Kind.negative``.
