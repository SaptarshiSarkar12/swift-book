Document Revision History
=========================

**2020-03-24**

* Updated for Swift 5.2.

* Added information about passing a key path instead of a closure
  to the :ref:`Expression_TypedKeyPathExpression` section.

* Added the :ref:`Declarations_SpecialFuncNames` section
  with information about syntactic sugar the lets instances of
  classes, structures, and enumerations be used with function call syntax.

* Updated the :ref:`Subscripts_SubscriptOptions` section,
  now that subscripts support parameters with default values.

* Updated the :ref:`Types_SelfType` section,
  now that the ``Self`` can be used in more contexts.

* Updated the :ref:`TheBasics_ImplicitlyUnwrappedOptionals` section
  to make it clearer that an implicitly unwrapped optional value
  can be used as either an optional or non-optional value.

**2019-09-10**

* Updated for Swift 5.1.

* Added information about functions
  that specify a protocol that their return value conforms to,
  instead of providing a specific named return type,
  to the :doc:`../LanguageGuide/OpaqueTypes` chapter.

* Added information about property wrappers
  to the :ref:`Properties_PropertyWrapper` section.

* Added information enumerations and structures
  that are frozen for library evolution
  to the :ref:`Attributes_frozen` section.

* Added the :ref:`Functions_ImplicitReturns`
  and :ref:`Properties_ImplicitReturn` sections
  with information about functions that omit ``return``.

* Added information about using subscripts on types
  to the :ref:`Subscripts_TypeSubscripts` section.

* Updated the :ref:`Patterns_EnumerationCasePattern` section,
  now that an enumeration case pattern can match an optional value.

* Updated the :ref:`Initialization_MemberwiseInitializersForStructureTypes` section,
  now that memberwise initializers support
  omitting parameters for properties that have a default value.

* Added information about dynamic members
  that are looked up by key path at runtime
  to the :ref:`Attributes_dynamicMemberLookup` section.

* Added ``macCatalyst`` to the list of target environments
  in :ref:`Statements_BuildConfigurationStatement`.

* Updated the :ref:`Types_SelfType` section,
  now that ``Self`` can be used to refer to the type
  introduced by the current class, structure, or enumeration declaration.

**2019-03-25**

* Updated for Swift 5.0.

* Added the :ref:`StringsAndCharacters_ExtendedDelimiters` section
  and updated the :ref:`LexicalStructure_StringLiterals` section
  with information about extended string delimiters.

* Added the :ref:`Attributes_dynamicCallable` section
  with information about dynamically calling instances as functions
  using the ``dynamicCallable`` attribute.

* Added the :ref:`Attributes_unknown` and :ref:`Statements_SwitchingOverFutureEnumerationCases` sections
  with information about handling future enumeration cases
  in switch statements using
  the ``unknown`` switch case attribute.

* Added information about the identity key path (``\.self``)
  to the :ref:`Expression_TypedKeyPathExpression` section.

* Added information about using the less than (``<``) operator
  in platform conditions to the :ref:`Statements_BuildConfigurationStatement` section.

**2018-09-17**

* Updated for Swift 4.2.

* Added information about accessing all of an enumeration's cases
  to the :ref:`Enumerations_AllCases` section.

* Added information about ``#error`` and ``#warning``
  to the :ref:`Statements_ErrorWarning` section.

* Added information about inlining
  to the :ref:`Attributes_DeclarationAttributes` section
  under the ``inlinable`` and  ``usableFromInline`` attributes.

* Added information about members that are looked up by name at runtime
  to the :ref:`Attributes_DeclarationAttributes` section
  under the ``dynamicMemberLookup`` attribute.

* Added information about the ``requires_stored_property_inits`` and ``warn_unqualified_access`` attributes
  to the :ref:`Attributes_DeclarationAttributes` section.

* Added information about how to conditionally compile code
  depending on the Swift compiler version being used
  to the :ref:`Statements_BuildConfigurationStatement` section.

* Added information about ``#dsohandle``
  to the :ref:`Expressions_LiteralExpression` section.

**2018-03-29**

* Updated for Swift 4.1.

* Added information about synthesized implementations of equivalence operators
  to the :ref:`AdvancedOperators_EquivalenceOperators` section.

* Added information about conditional protocol conformance
  to the :ref:`Declarations_ExtensionDeclaration` section
  of the :doc:`../ReferenceManual/Declarations` chapter,
  and to the :ref:`Protocols_DeclaringConditionalConformanceToAProtocol` section
  of the :doc:`../LanguageGuide/Protocols` chapter.

* Added information about recursive protocol constraints
  to the :ref:`Generics_RecursiveProtocol` section.

* Added information about
  the ``canImport()`` and ``targetEnvironment()`` platform conditions
  to :ref:`Statements_BuildConfigurationStatement`.

**2017-12-04**

* Updated for Swift 4.0.3.

* Updated the :ref:`Expression_TypedKeyPathExpression` section,
  now that key paths support subscript components.

**2017-09-19**

* Updated for Swift 4.0.

* Added information about exclusive access to memory
  to the :doc:`../LanguageGuide/MemorySafety` chapter.

* Added the :ref:`Generics_AssociatedTypesWithWhereClause` section,
  now that you can use generic ``where`` clauses
  to constrain associated types.

* Added information about multiline string literals
  to the :ref:`StringsAndCharacters_Literals` section
  of the :doc:`../LanguageGuide/StringsAndCharacters` chapter,
  and to the :ref:`LexicalStructure_StringLiterals` section
  of the :doc:`../ReferenceManual/LexicalStructure` chapter.

* Updated the discussion of the ``objc`` attribute
  in :ref:`Attributes_DeclarationAttributes`,
  now that this attribute is inferred in fewer places.

* Added the :ref:`Generics_Subscripts` section,
  now that subscripts can be generic.

* Updated the discussion
  in the :ref:`Protocols_ProtocolComposition` section
  of the :doc:`../LanguageGuide/Protocols` chapter,
  and in the :ref:`Types_ProtocolCompositionType` section
  of the :doc:`../ReferenceManual/Types` chapter,
  now that protocol composition types can contain a superclass requirement.

* Updated the discussion of protocol extensions
  in :ref:`Declarations_ExtensionDeclaration`
  now that ``final`` isn't allowed in them.

* Added information about preconditions and fatal errors
  to the :ref:`TheBasics_Assertions` section.

**2017-03-27**

* Updated for Swift 3.1.

* Added the :ref:`Generics_ExtensionWithWhereClause` section
  with information about extensions that include requirements.

* Added examples of iterating over a range
  to the :ref:`ControlFlow_ForLoops` section.

* Added an example of failable numeric conversions
  to the :ref:`Initialization_FailableInitializers` section.

* Added information to the :ref:`Attributes_DeclarationAttributes` section
  about using the ``available`` attribute with a Swift language version.

* Updated the discussion in the :ref:`Types_FunctionType` section
  to note that argument labels aren't allowed when writing a function type.

* Updated the discussion of Swift language version numbers
  in the :ref:`Statements_BuildConfigurationStatement` section,
  now that an optional patch number is allowed.

* Updated the discussion
  in the :ref:`Types_FunctionType` section,
  now that Swift distinguishes between functions that take multiple parameters
  and functions that take a single parameter of a tuple type.

* Removed the Dynamic Type Expression section
  from the :doc:`../ReferenceManual/Expressions` chapter,
  now that ``type(of:)`` is a Swift standard library function.

**2016-10-27**

* Updated for Swift 3.0.1.

* Updated the discussion of weak and unowned references
  in the :doc:`../LanguageGuide/AutomaticReferenceCounting` chapter.

* Added information about the ``unowned``, ``unowned(safe)``, and ``unowned(unsafe)``
  declaration modifiers
  in the :ref:`Declarations_DeclarationModifiers` section.

* Added a note to the :ref:`TypeCasting_TypeCastingForAnyAndAnyObject` section
  about using an optional value when a value of type ``Any`` is expected.

* Updated the :doc:`../ReferenceManual/Expressions` chapter
  to separate the discussion of parenthesized expressions and tuple expressions.

**2016-09-13**

* Updated for Swift 3.0.

* Updated the discussion of functions in the :doc:`../LanguageGuide/Functions` chapter
  and the :ref:`Declarations_FunctionDeclaration` section to note that
  all parameters get an argument label by default.

* Updated the discussion of operators
  in the :doc:`../LanguageGuide/AdvancedOperators` chapter,
  now that you implement them as type methods instead of as global functions.

* Added information about the ``open`` and ``fileprivate`` access-level modifiers
  to the :doc:`../LanguageGuide/AccessControl` chapter.

* Updated the discussion of ``inout`` in the :ref:`Declarations_FunctionDeclaration` section
  to note that it appears in front of a parameter's type
  instead of in front of a parameter's name.

* Updated the discussion of the ``@noescape`` and ``@autoclosure`` attributes
  in the :ref:`Closures_Noescape` and :ref:`Closures_Autoclosures` sections
  and the :doc:`../ReferenceManual/Attributes` chapter
  now that they are type attributes, rather than declaration attributes.

* Added information about operator precedence groups
  to the :ref:`AdvancedOperators_PrecedenceAndAssociativityForCustomOperators` section
  of the :doc:`../LanguageGuide/AdvancedOperators` chapter,
  and to the :ref:`Declarations_PrecedenceGroupDeclaration` section
  of the :doc:`../ReferenceManual/Declarations` chapter.

* Updated discussion throughout
  to use macOS instead of OS X,
  ``Error`` instead of ``ErrorProtocol``,
  and protocol names such as ``ExpressibleByStringLiteral``
  instead of ``StringLiteralConvertible``.

* Updated the discussion
  in the :ref:`Generics_WhereClauses` section
  of the :doc:`../LanguageGuide/Generics` chapter
  and in the :doc:`../ReferenceManual/GenericParametersAndArguments` chapter,
  now that generic ``where`` clauses are written at the end of a declaration.

* Updated the discussion in the :ref:`Closures_Noescape` section,
  now that closures are nonescaping by default.

* Updated the discussion
  in the :ref:`TheBasics_OptionalBinding` section
  of the :doc:`../LanguageGuide/TheBasics` chapter
  and the :ref:`Statements_WhileStatement` section
  of the :doc:`../ReferenceManual/Statements` chapter,
  now that ``if``, ``while``, and ``guard`` statements
  use a comma-separated list of conditions without ``where`` clauses.

* Added information about switch cases that have multiple patterns
  to the :ref:`ControlFlow_Switch` section
  of the :doc:`../LanguageGuide/ControlFlow` chapter
  and the :ref:`Statements_SwitchStatement` section
  of the :doc:`../ReferenceManual/Statements` chapter.

* Updated the discussion of function types
  in the :ref:`Types_FunctionType` section
  now that function argument labels are no longer part of a function's type.

* Updated the discussion of protocol composition types
  in the :ref:`Protocols_ProtocolComposition` section
  of the :doc:`../LanguageGuide/Protocols` chapter
  and in the :ref:`Types_ProtocolCompositionType` section
  of the :doc:`../ReferenceManual/Types` chapter
  to use the new ``Protocol1 & Protocol2`` syntax.

* Updated the discussion in the Dynamic Type Expression section
  to use the new ``type(of:)`` syntax for dynamic type expressions.

* Updated the discussion of line control statements
  to use the ``#sourceLocation(file:line:)`` syntax
  in the :ref:`Statements_LineControlStatement` section.

* Updated the discussion in :ref:`Declarations_FunctionsThatNeverReturn`
  to use the new ``Never`` type.

* Added information about playground literals
  to the :ref:`Expressions_LiteralExpression` section.

* Updated the discussion in the :ref:`Declarations_InOutParameters` section
  to note that only nonescaping closures can capture in-out parameters.

* Updated the discussion about default parameters
  in the :ref:`Functions_DefaultParameterValues` section,
  now that they can't be reordered in function calls.

* Updated attribute arguments to use a colon
  in the :doc:`../ReferenceManual/Attributes` chapter.

* Added information about throwing an error
  inside the catch block of a rethrowing function
  to the :ref:`Declarations_RethrowingFunctionsAndMethods` section.

* Added information about accessing the selector
  of an Objective-C property's getter or setter
  to the :ref:`Expression_SelectorExpression` section.

* Added information to the :ref:`Declarations_TypeAliasDeclaration` section
  about generic type aliases and using type aliases inside of protocols.

* Updated the discussion of function types in the :ref:`Types_FunctionType` section
  to note that parentheses around the parameter types are required.

* Updated the :doc:`../ReferenceManual/Attributes` chapter
  to note that the ``@IBAction``, ``@IBOutlet``, and ``@NSManaged`` attributes
  imply the ``@objc`` attribute.

* Added information about the ``@GKInspectable`` attribute
  to the :ref:`Attributes_DeclarationAttributes` section.

* Updated the discussion of optional protocol requirements
  in the :ref:`Protocols_OptionalProtocolRequirements` section
  to clarify that they are used only in code that interoperates with Objective-C.

* Removed the discussion of explicitly using ``let`` in function parameters
  from the :ref:`Declarations_FunctionDeclaration` section.

* Removed the discussion of the ``Boolean`` protocol
  from the :doc:`../ReferenceManual/Statements` chapter,
  now that the protocol has been removed from the Swift standard library.

* Corrected the discussion of the ``@NSApplicationMain`` attribute
  in the :ref:`Attributes_DeclarationAttributes` section.

**2016-03-21**

* Updated for Swift 2.2.

* Added information about how to conditionally compile code
  depending on the version of Swift being used
  to the :ref:`Statements_BuildConfigurationStatement` section.

* Added information about how to distinguish
  between methods or initializers whose names differ
  only by the names of their arguments
  to the :ref:`Expressions_ExplicitMemberExpression` section.

* Added information about the ``#selector`` syntax
  for Objective-C selectors
  to the :ref:`Expression_SelectorExpression` section.

* Updated the discussion of associated types
  to use the ``associatedtype`` keyword
  in the :ref:`Generics_AssociatedTypes`
  and :ref:`Declarations_ProtocolAssociatedTypeDeclaration` sections.

* Updated information about initializers that return ``nil``
  before the instance is fully initialized
  in the :ref:`Initialization_FailableInitializers` section.

* Added information about comparing tuples
  to the :ref:`BasicOperators_ComparisonOperators` section.

* Added information about using keywords as external parameter names
  to the :ref:`LexicalStructure_Keywords` section.

* Updated the discussion of the ``@objc`` attribute
  in the :ref:`Attributes_DeclarationAttributes` section to note that
  enumerations and enumeration cases can use this attribute.

* Updated the :ref:`LexicalStructure_Operators` section
  with discussion of custom operators that contain a dot.

* Added a note
  to the :ref:`Declarations_RethrowingFunctionsAndMethods` section
  that rethrowing functions can't directly throw errors.

* Added a note to the :ref:`Properties_PropertyObservers` section
  about property observers being called
  when you pass a property as an in-out parameter.

* Added a section about error handling
  to the :doc:`../GuidedTour/GuidedTour` chapter.

* Updated figures in the
  :ref:`AutomaticReferenceCounting_WeakReferencesBetweenClassInstances`
  section to show the deallocation process more clearly.

* Removed discussion of C-style ``for`` loops,
  the ``++`` prefix and postfix operators,
  and the ``--`` prefix and postfix operators.

* Removed discussion of variable function arguments
  and the special syntax for curried functions.

**2015-10-20**

* Updated for Swift 2.1.

* Updated the :ref:`StringsAndCharacters_StringInterpolation`
  and :ref:`LexicalStructure_StringLiterals` sections
  now that string interpolations can contain string literals.

* Added the :ref:`Closures_Noescape` section
  with information about the ``@noescape`` attribute.

* Updated the :ref:`Attributes_DeclarationAttributes`
  and :ref:`Statements_BuildConfigurationStatement` sections
  with information about tvOS.

* Added information about the behavior of in-out parameters
  to the :ref:`Declarations_InOutParameters` section.

* Added information to the :ref:`Expressions_CaptureLists` section
  about how values specified in closure capture lists are captured.

* Updated the
  :ref:`OptionalChaining_CallingPropertiesThroughOptionalChaining`
  section to clarify how assignment through optional chaining
  behaves.

* Improved the discussion of autoclosures
  in the :ref:`Closures_Autoclosures` section.

* Added an example that uses the ``??`` operator
  to the :doc:`../GuidedTour/GuidedTour` chapter.

**2015-09-16**

* Updated for Swift 2.0.

* Added information about error handling
  to the :doc:`../LanguageGuide/ErrorHandling` chapter,
  the :ref:`Statements_DoStatement` section,
  the :ref:`Statements_ThrowStatement` section,
  the :ref:`Statements_DeferStatement` section,
  and the :ref:`Expressions_TryExpression` section.

* Updated the :ref:`ErrorHandling_Represent` section,
  now that all types can conform to the ``ErrorType`` protocol.

* Added information about the new ``try?`` keyword
  to the :ref:`ErrorHandling_Optional` section.

* Added information about recursive enumerations
  to the :ref:`Enumerations_RecursiveEnumerations` section
  of the :doc:`../LanguageGuide/Enumerations` chapter
  and the :ref:`Declarations_EnumerationsWithCasesOfAnyType` section
  of the :doc:`../ReferenceManual/Declarations` chapter.

* Added information about API availability checking
  to the :ref:`ControlFlow_Available` section
  of the :doc:`../LanguageGuide/ControlFlow` chapter
  and the :ref:`Statements_AvailabilityCondition` section
  of the :doc:`../ReferenceManual/Statements` chapter.

* Added information about the new ``guard`` statement
  to the :ref:`ControlFlow_Guard` section
  of the :doc:`../LanguageGuide/ControlFlow` chapter
  and the :ref:`Statements_GuardStatement` section
  of the :doc:`../ReferenceManual/Statements` chapter.

* Added information about protocol extensions
  to the :ref:`Protocols_Extensions` section
  of the :doc:`../LanguageGuide/Protocols` chapter.

* Added information about access control for unit testing
  to the :ref:`AccessControl_AccessLevelsForTestTargets` section
  of the :doc:`../LanguageGuide/AccessControl` chapter.

* Added information about the new optional pattern
  to the :ref:`Patterns_OptionalPattern` section
  of the :doc:`../ReferenceManual/Patterns` chapter.

* Updated the :ref:`ControlFlow_DoWhile` section
  with information about the ``repeat``-``while`` loop.

* Updated the :doc:`../LanguageGuide/StringsAndCharacters` chapter,
  now that ``String`` no longer conforms
  to the ``CollectionType`` protocol from the Swift standard library.

* Added information about the new Swift standard library
  ``print(_:separator:terminator)`` function
  to the :ref:`TheBasics_PrintingConstantsAndVariables` section.

* Added information about the behavior
  of enumeration cases with ``String`` raw values
  to the :ref:`Enumerations_ImplicitlyAssignedRawValues` section
  of the :doc:`../LanguageGuide/Enumerations` chapter
  and the :ref:`Declarations_EnumerationsWithRawCaseValues` section
  of the :doc:`../ReferenceManual/Declarations` chapter.

* Added information about the ``@autoclosure`` attribute ---
  including its ``@autoclosure(escaping)`` form ---
  to the :ref:`Closures_Autoclosures` section.

* Updated the :ref:`Attributes_DeclarationAttributes` section
  with information about the ``@available``
  and ``@warn_unused_result`` attributes.

* Updated the :ref:`Attributes_TypeAttributes` section
  with information about the ``@convention`` attribute.

* Added an example of using multiple optional bindings
  with a ``where`` clause
  to the :ref:`TheBasics_OptionalBinding` section.

* Added information to the :ref:`LexicalStructure_StringLiterals` section
  about how concatenating string literals using the ``+`` operator
  happens at compile time.

* Added information to the :ref:`Types_MetatypeType` section
  about comparing metatype values and using them
  to construct instances with initializer expressions.

* Added a note to the :ref:`TheBasics_DebuggingWithAssertions` section
  about when user-defined assertions are disabled.

* Updated the discussion of the ``@NSManaged`` attribute
  in the :ref:`Attributes_DeclarationAttributes` section,
  now that the attribute can be applied to certain instance methods.

* Updated the :ref:`Functions_VariadicParameters` section,
  now that variadic parameters can be declared in any position
  in a function's parameter list.

* Added information
  to the :ref:`Initialization_OverridingAFailableInitializer` section
  about how a nonfailable initializer can delegate
  up to a failable initializer
  by force-unwrapping the result of the superclass's initializer.

* Added information about using enumeration cases as functions
  to the :ref:`Declarations_EnumerationsWithCasesOfAnyType` section.

* Added information about explicitly referencing an initializer
  to the :ref:`Expressions_InitializerExpression` section.

* Added information about build configuration
  and line control statements
  to the :ref:`Statements_CompilerControlStatements` section.

* Added a note to the :ref:`Types_MetatypeType` section
  about constructing class instances from metatype values.

* Added a note to the
  :ref:`AutomaticReferenceCounting_WeakReferencesBetweenClassInstances`
  section about weak references being unsuitable for caching.

* Updated a note in the :ref:`Properties_TypeProperties` section
  to mention that stored type properties are lazily initialized.

* Updated the :ref:`Closures_CapturingValues` section
  to clarify how variables and constants are captured in closures.

* Updated the :ref:`Attributes_DeclarationAttributes` section
  to describe when you can apply the ``@objc`` attribute to classes.

* Added a note to the :ref:`ErrorHandling_Catch` section
  about the performance of executing a ``throw`` statement.
  Added similar information about the ``do`` statement
  in the :ref:`Statements_DoStatement` section.

* Updated the :ref:`Properties_TypeProperties` section
  with information about stored and computed type properties
  for classes, structures, and enumerations.

* Updated the :ref:`Statements_BreakStatement` section
  with information about labeled break statements.

* Updated a note in the :ref:`Properties_PropertyObservers` section
  to clarify the behavior of ``willSet`` and ``didSet`` observers.

* Added a note to the :ref:`AccessControl_AccessLevels` section
  with information about the scope of ``private`` access.

* Added a note to the
  :ref:`AutomaticReferenceCounting_WeakReferencesBetweenClassInstances`
  section about the differences in weak references
  between garbage collected systems and ARC.

* Updated the
  :ref:`StringsAndCharacters_SpecialCharactersInStringLiterals` section
  with a more precise definition of Unicode scalars.


**2015-04-08**

* Updated for Swift 1.2.

* Swift now has a native ``Set`` collection type.
  For more information, see :ref:`CollectionTypes_Sets`.

* ``@autoclosure`` is now an attribute of the parameter declaration,
  not its type.
  There's also a new ``@noescape`` parameter declaration attribute.
  For more information, see :ref:`Attributes_DeclarationAttributes`.

* Type methods and properties now use the ``static`` keyword
  as a declaration modifier.
  For more information see :ref:`Declarations_TypeVariableProperties`.

* Swift now includes the ``as?`` and ``as!`` failable downcast operators.
  For more information,
  see :ref:`Protocols_CheckingForProtocolConformance`.

* Added a new guide section about
  :ref:`StringsAndCharacters_StringIndices`.

* Removed the overflow division (``&/``) and
  overflow remainder (``&%``) operators
  from :ref:`AdvancedOperators_OverflowOperators`.

* Updated the rules for constant and
  constant property declaration and initialization.
  For more information, see :ref:`Declarations_ConstantDeclaration`.

* Updated the definition of Unicode scalars in string literals.
  See :ref:`StringsAndCharacters_SpecialCharactersInStringLiterals`.

* Updated :ref:`BasicOperators_RangeOperators` to note that
  a half-open range with the same start and end index will be empty.

* Updated :ref:`Closures_ClosuresAreReferenceTypes` to clarify
  the capturing rules for variables.

* Updated :ref:`AdvancedOperators_ValueOverflow` to clarify
  the overflow behavior of signed and unsigned integers

* Updated :ref:`Declarations_ProtocolDeclaration` to clarify
  protocol declaration scope and members.

* Updated :ref:`AutomaticReferenceCounting_DefiningACaptureList`
  to clarify the syntax for
  weak and unowned references in closure capture lists.

* Updated :ref:`LexicalStructure_Operators` to explicitly mention
  examples of supported characters for custom operators,
  such as those in the Mathematical Operators, Miscellaneous Symbols,
  and Dingbats Unicode blocks.

* Constants can now be declared without being initialized
  in local function scope.
  They must have a set value before first use.
  For more information, see :ref:`Declarations_ConstantDeclaration`.

* In an initializer, constant properties can now only assign a value once.
  For more information,
  see :ref:`Initialization_ModifyingConstantPropertiesDuringInitialization`.

* Multiple optional bindings can now appear in a single ``if`` statement
  as a comma-separated list of assignment expressions.
  For more information, see :ref:`TheBasics_OptionalBinding`.

* An :ref:`Expression_OptionalChainingOperator`
  must appear within a postfix expression.

* Protocol casts are no longer limited to ``@objc`` protocols.

* Type casts that can fail at runtime
  now use the ``as?`` or ``as!`` operator,
  and type casts that are guaranteed not to fail use the ``as`` operator.
  For more information, see :ref:`Expressions_Type-CastingOperators`.

**2014-10-16**

* Updated for Swift 1.1.

* Added a full guide to :ref:`Initialization_FailableInitializers`.

* Added a description of :ref:`Protocols_FailableInitializerRequirements`
  for protocols.

* Constants and variables of type ``Any`` can now contain
  function instances. Updated the example in :ref:`TypeCasting_TypeCastingForAnyAndAnyObject`
  to show how to check for and cast to a function type
  within a ``switch`` statement.

* Enumerations with raw values
  now have a ``rawValue`` property rather than a ``toRaw()`` method
  and a failable initializer with a ``rawValue`` parameter
  rather than a ``fromRaw()`` method.
  For more information, see :ref:`Enumerations_RawValues`
  and :ref:`Declarations_EnumerationsWithRawCaseValues`.

* Added a new reference section about
  :ref:`Declarations_FailableInitializers`,
  which can trigger initialization failure.

* Custom operators can now contain the ``?`` character.
  Updated the :ref:`LexicalStructure_Operators` reference to describe
  the revised rules.
  Removed a duplicate description of the valid set of operator characters
  from :ref:`AdvancedOperators_CustomOperators`.

**2014-08-18**

* New document that describes Swift 1.0,
  Apple’s new programming language for building iOS and OS X apps.

* Added a new section about
  :ref:`Protocols_InitializerRequirements` in protocols.

* Added a new section about :ref:`Protocols_ClassOnlyProtocols`.

* :ref:`TheBasics_Assertions` can now use string interpolation.
  Removed a note to the contrary.

* Updated the
  :ref:`StringsAndCharacters_ConcatenatingStringsAndCharacters` section
  to reflect the fact that ``String`` and ``Character`` values
  can no longer be combined with the addition operator (``+``)
  or addition assignment operator (``+=``).
  These operators are now used only with ``String`` values.
  Use the ``String`` type's ``append(_:)`` method
  to append a single ``Character`` value onto the end of a string.

* Added information about the ``availability`` attribute to
  the :ref:`Attributes_DeclarationAttributes` section.

* :ref:`TheBasics_Optionals` no longer implicitly evaluate to
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
  Instead, use the ``append(_:)`` method,
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

* Added more information about curried functions.

* Added a new chapter about :doc:`../LanguageGuide/AccessControl`.

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
  where ``n`` is a hexadecimal number between 0 and 10FFFF,
  the range of Unicode's codespace.

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

* Swift's ``Array`` type now has full value semantics.
  Updated the information about :ref:`CollectionTypes_MutabilityOfCollections`
  and :ref:`CollectionTypes_Arrays` to reflect the new approach.
  Also clarified the assignment and copy behavior for strings arrays and dictionaries.

* :ref:`CollectionTypes_ArrayTypeShorthandSyntax` is now written as
  ``[SomeType]`` rather than ``SomeType[]``.

* Added a new section about :ref:`CollectionTypes_DictionaryTypeShorthandSyntax`,
  which is written as ``[KeyType: ValueType]``.

* Added a new section about :ref:`CollectionTypes_HashValuesForSetTypes`.

* Examples of :ref:`Closures_ClosureExpressions` now use
  the global ``sorted(_:_:)`` function
  rather than the global ``sort(_:_:)`` function,
  to reflect the new array value semantics.

* Updated the information about :ref:`Initialization_MemberwiseInitializersForStructureTypes`
  to clarify that the memberwise structure initializer is made available
  even if a structure's stored properties don't have default values.

* Updated to ``..<`` rather than ``..``
  for the :ref:`BasicOperators_HalfClosedRangeOperator`.

* Added an example of :ref:`Generics_ExtendingAGenericType`.
