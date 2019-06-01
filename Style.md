# Editorial Style Guide

## memberwise initializer
Not hyphenated as “member-wise”.

## non-optional
Hyphenated to avoid the misreading as nono-ptional.
Normal rules for hyphenation from Apple Style Guide would omit the hyphen.

See also commit 6ed6a956139772851e466e8419f48c5293f9574a and <rdar://problem/44881846>.

## punctuation before a code listing

Write a colon after a sentence that ends with a phrase like
“as follows” or “as shown below”,
where the code listing is basically acting like a part of the sentence,
and the prose is explicitly referring to it.
Use a colon after a sentence fragment like “For example:”.
Write a period after sentences that make a statement about the code
but don’t include words that refer to the code.
Use a period for sentences that begin with a phrase like
“In the following code”.

> **Note:**
> This usage isn’t entirely consistent in the existing text.
> We should have a discussion about this with Editorial.

# Semantic Line Breaks

The RST files in this repository use semantic line breaks,
where lines end at sentence and clause boundaries.
This keeps lines short enough to ensure that
diffs remain readable when shown in places like
pull requests,
commit notification emails,
and the terminal.
Because the lines break in meaningful places,
changes don’t require re-wrapping entire paragraphs,
so only the lines that changed get marked.
This lets tools like `git-blame` give per-line history.

As you’re writing,
aim to keep lines 80 characters or less.
Start a new line after each sentence,
and as needed at clause boundaries.
For a long list,
you can put each list item on its own line —
this is mostly helpful for lists you want to alphabetize
because it lets you sort those lines.
There are several different styles throughout the book,
so don’t feel the need to exactly follow any one approach.

When you’re editing existing text,
preserve the line breaks when feasible
to help keep the diffs small and preserve per-line history.
Don’t rewrap an existing line just because it’s a bit long
unless you actually need to make other changes to it.
There are parts of the book
that run the line length out to 90 or 100 characters,
and rewrapping them just for the sake of line length
would make history harder to follow.

More information about semantic line breaks
is available in the following places:

* “UNIX for Beginners”, Brian W. Kernighan, 1974 — Alex Martini has a PDF copy
* [Semantic Linefeeds](https://rhodesmill.org/brandon/2012/one-sentence-per-line/)
    blog post about the advantages and history of this format
    for authoring documentation.
* [Ventilated Prose](https://vanemden.wordpress.com/2009/01/01/ventilated-prose/)
    discusses Buckminster Fuller’s proposed use of this format
    to improve reading comprehension.
* [Semantic Line Breaks](https://sembr.org) describes a formalized spec,
    maintained by Mattt Zmuda

# Formal Grammar

**Write an ASCII arrow.**
The arrow (`-->`) can be read as "can consist of."

To make the arrow in RST, use two hyphens (`-`) followed by a right-hand angle bracket (`>`).
The production path is responsible for making it render as a nice Unicode arrow.

**Write literals with double backticks.**
For example:

    forty-two --> ``42``

**Write syntactic category names without any extra markup**
Within a syntax-grammar block, they appear in italics automatically.
Don't refer to them from the English prose above them.

**Use full English words as the names for syntactic categories.**
There are cases where this isn't feasible because of space considerations. 
For example, in the grammar for a C-style for statement,
the category that defines the initialization part of the for statement
had to be shortened to *for-init*
(instead of *for-initialization*, as the rule specifies).
In this case, nothing seems lost from a readability or pedagogical perspective.

    c-style-for-statement --> ``for`` for-init-OPT ``;`` expression-OPT ``;`` basic-expression-OPT brace-item-list
    c-style-for-statement --> ``for`` ``(`` for-init-OPT ``;`` expression-OPT ``;`` basic-expression-OPT ``)`` brace-item-list

    for-init --> variable-declaration | expression

**Use a pipe '|' to specify an alternative.**
When there are too many alternatives
to fit on a single line, use a new line for each alternative.
Don't mix pipes and newlines.

For example, to specify that a *case-block-item* can consist of a *declaration*, 
*expression*, or a *statement*, you can use a pipe instead of a new line,
because all three alternatives fit nicely on one line:

    code-block-item --> declaration | expression | statement

On the other hand, consider the grammar of a control transfer statement:

    control-transfer-statement --> break-statement
    control-transfer-statement --> continue-statement
    control-transfer-statement --> fallthrough-statement
    control-transfer-statement --> return-statement

There likely wouldn't be room on a single line to use a pipe to separate each alternative.
The following tends not to loog good:

    control-transfer-statement --> break-statement | continue-statement | fallthrough-statement | return-statement

**Append -OPT to indicate optionality.**
Within a syntax-grammar block,
this is translated to a subscript “opt” automatically.

**Use plural names for repetition.**
In BNF, this is represented with a plus (`+`) or (`*`).
The syntax of our formal grammar doesn't include repetition operators,
so we use two syntactic categories to allow repetition.
For example:

    categories --> category categories-OPT
    category --> More formal grammar goes here.

    switch-statement --> ``switch`` basic-expression { switch-cases-OPT }
    switch-cases --> switch-case switch-cases-OPT
    switch-case --> case-label statements
    switch-case --> default-label statements
    switch-case --> conditional-switch-case

A plural name consists of only a repeated list of the singular version.
If you need separators like commas, call it a "list".

    case-label --> attributes-OPT ``case`` case-item-list ``:``
    case-item-list --> pattern where-clause-OPT | pattern where-clause-OPT ``,`` case-item-list

As shown above, use right-recursion when dealing with repetition.

**Omit grouping parentheses.**
Our formal grammar doesn't use grouping parentheses.
Optionality using `-OPT` always applies to exactly one token before it,
and only one level of alternation using `|` or line breaks is allowed.

If you see BNF grammar for new language features that uses parentheses,
you need to exercise some creativity and judgment when removing them.
Here are some examples:

This one required coming up with a new category, which then needed to be defined.

    ('where' expr)? ==> guard-expression-OPT
    guard-expression --> ``where`` expression

This one was a bit dense and required the application of several of the rules above.

    stmt-switch-case ::= (case-label+ | default-label) brace-item*

was replaced with

    switch-case --> case-labels brace-items-OPT | default-label brace-items-OPT

