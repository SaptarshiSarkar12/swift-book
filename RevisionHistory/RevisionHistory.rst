Document Revision History
=========================

This table describes the changes to *The Swift Programming Language*.

==========  ==========================================================================
Date        Notes
==========  ==========================================================================
2014-08-04  * :ref:`TheBasics_Optionals` no longer implicitly equate to
              ``true`` when they have a value and ``false`` when they do not,
              to avoid confusion when working with optional ``Bool`` values.
              Instead, make an explicit check against ``nil``
              with the ``==`` or ``!=`` operators
              to find out if an optional contains a value.

            * Swift now has a :ref:`BasicOperators_NilCoalescingOperator`
              (``a ?? b``), which unwraps an optional's value if it exists,
              or returns a default value if the optional is ``nil``.

            * Updated and expanded
              the :ref:`StringsAndCharacters_ComparingStrings` section
              to reflect and demonstrate that string and character comparison
              and prefix / suffix comparison are now based on
              Unicode canonical equivalence of extended grapheme clusters.

            * You can now try to set a property's value, assign to a subscript,
              or call a mutating method or operator through
              :doc:`../LanguageGuide/OptionalChaining`.
              The information about
              :ref:`OptionalChaining_CallingPropertiesThroughOptionalChaining`
              has been updated accordingly,
              and the examples of checking for method call success in
              :ref:`OptionalChaining_CallingMethodsThroughOptionalChaining`
              have been expanded to show how to check for property setting success.

            * Added a new section about
              :ref:`OptionalChaining_AccessingSubscriptsOfOptionalType`
              through optional chaining.

            * Updated the :ref:`CollectionTypes_AccessingAndModifyingAnArray` section
              to note that you can no longer append a single item to an array
              with the ``+=`` operator.
              Instead, use the ``append`` method,
              or append a single-item array with the ``+=`` operator.

            * Added a note that the start value ``a``
              for the :ref:`BasicOperators_RangeOperators` ``a...b`` and ``a..<b``
              must not be greater than the end value ``b``.

            * Rewrote the :doc:`../LanguageGuide/Inheritance` chapter
              to remove its introductory coverage of initializer overrides.
              This chapter now focuses more on the addition of
              new functionality in a subclass,
              and the modification of existing functionality with overrides.
              The chapter's example of
              :ref:`Inheritance_OverridingPropertyGettersAndSetters`
              has been rewritten to show how to override a ``description`` property.
              (The examples of modifying an inherited property's default value
              in a subclass initializer have been moved to
              the :doc:`../LanguageGuide/Initialization` chapter.)

            * Updated the
              :ref:`Initialization_InitializerInheritanceAndOverriding` section
              to note that overrides of a designated initializer
              must now be marked with the ``override`` modifier.

            * Updated the :ref:`Initialization_RequiredInitializers` section
              to note that the ``required`` modifier is now written before
              every subclass implementation of a required initializer,
              and that the requirements for required initializers
              can now be satisfied by automatically inherited initializers.

            * Infix :ref:`AdvancedOperators_OperatorFunctions` no longer require
              the ``@infix`` attribute.

            * The ``@prefix`` and ``@postfix`` attributes
              for :ref:`AdvancedOperators_PrefixAndPostfixOperators`
              have been replaced by ``prefix`` and ``postfix`` declaration modifiers.

            * Added a note about the order in which
              :ref:`AdvancedOperators_PrefixAndPostfixOperators` are applied
              when both a prefix and a postfix operator are applied to
              the same operand.

            * Operator functions for
              :ref:`AdvancedOperators_CompoundAssignmentOperators` no longer use
              the ``@assignment`` attribute when defining the function.

            * The order in which modifiers are specified when defining 
              :ref:`AdvancedOperators_CustomOperators` has changed.
              You now write ``prefix operator`` rather than ``operator prefix``,
              for example.

            * Added information about the ``dynamic`` declaration modifier
              in :ref:`Declarations_DeclarationModifiers`.

            * Added information about how type inference works
              with :ref:`LexicalStructure_Literals`.
----------  --------------------------------------------------------------------------
2014-07-21  * Added a new chapter about :doc:`../LanguageGuide/AccessControl`.

            * Updated the :doc:`../LanguageGuide/StringsAndCharacters` chapter
              to reflect the fact that Swift's ``Character`` type now represents
              a single Unicode extended grapheme cluster.
              Includes a new section on
              :ref:`StringsAndCharacters_ExtendedGraphemeClusters`
              and more information about
              :ref:`StringsAndCharacters_StringsAreUnicodeScalars`
              and :ref:`StringsAndCharacters_ComparingStrings`.

            * Updated the :ref:`StringsAndCharacters_Literals` section
              to note that Unicode scalars inside string literals
              are now written as ``\u{n}``,
              where ``n`` is between one and eight hexadecimal digits.

            * The ``NSString`` ``length`` property is now mapped onto
              Swift's native ``String`` type as ``utf16Count``, not ``utf16count``.

            * Swift's native ``String`` type no longer has
              an ``uppercaseString`` or ``lowercaseString`` property.
              The corresponding section in
              :doc:`../LanguageGuide/StringsAndCharacters`
              has been removed, and various code examples have been updated.

            * Added a new section about
              :ref:`Initialization_InitializerParametersWithoutExternalNames`.

            * Added a new section about
              :ref:`Initialization_RequiredInitializers`.

            * Added a new section about :ref:`Functions_OptionalTupleReturnTypes`.

            * Updated the :ref:`TheBasics_TypeAnnotations` section to note that
              multiple related variables can be defined on a single line
              with one type annotation.

            * The ``@optional``, ``@lazy``, ``@final``, and ``@required`` attributes
              are now the ``optional``, ``lazy``, ``final``, and ``required``
              :ref:`Declarations_DeclarationModifiers`.

            * Updated the entire book to refer to ``..<`` as
              the :ref:`BasicOperators_HalfClosedRangeOperator`
              (rather than the “half-closed range operator”).

            * Updated the :ref:`CollectionTypes_AccessingAndModifyingADictionary`
              section to note that ``Dictionary`` now has
              a Boolean ``isEmpty`` property.

            * Clarified the full list of characters that can be used
              when defining :ref:`AdvancedOperators_CustomOperators`.

            * ``nil`` and the Booleans ``true`` and ``false`` are now :ref:`LexicalStructure_Literals`.
----------  --------------------------------------------------------------------------
2014-07-07  * Swift's ``Array`` type now has full value semantics.
              Updated the information about :ref:`CollectionTypes_MutabilityOfCollections`
              and :ref:`CollectionTypes_Arrays` to reflect the new approach.
              Also clarified the
              :ref:`ClassesAndStructures_AssignmentAndCopyBehaviorForStringsArraysAndDictionaries`.

            * :ref:`CollectionTypes_ArrayTypeShorthandSyntax` is now written as
              ``[SomeType]`` rather than ``SomeType[]``.

            * Added a new section about :ref:`CollectionTypes_DictionaryTypeShorthandSyntax`,
              which is written as ``[KeyType: ValueType]``.

            * Added a new section about :ref:`CollectionTypes_HashValuesForDictionaryKeyTypes`.

            * Examples of :ref:`Closures_ClosureExpressions` now use
              the global ``sorted`` function rather than the global ``sort`` function,
              to reflect the new array value semantics.

            * Updated the information about :ref:`Initialization_MemberwiseInitializersForStructureTypes`
              to clarify that the memberwise structure initializer is made available
              even if a structure's stored properties do not have default values.

            * Updated to ``..<`` rather than ``..``
              for the :ref:`BasicOperators_HalfClosedRangeOperator`.

            * Added an example of :ref:`Generics_ExtendingAGenericType`.
----------  --------------------------------------------------------------------------
2014-06-02  * New document that describes Swift,
              Apple’s new programming language for building iOS and OS X apps.
==========  ==========================================================================
