---
title     : "Naturals: Natural numbers"
layout    : page
prev      : /Basics/
permalink : /Naturals/
next      : /NatData/
---

```
module plc.fp.Naturals where
open import Data.Bool
```

The night sky holds more stars than I can count, though fewer than five
thousand are visible to the naked eye.  The observable universe
contains about seventy sextillion stars.

But the number of stars is finite, while natural numbers are infinite.
Count all the stars, and you will still have as many natural numbers
left over as you started with.


## What is a natural number?

Everyone is familiar with the natural numbers

    0
    1
    2
    3
    ...

and so on. We write `ℕ` for the _type_ of natural numbers, and say that
`0`, `1`, `2`, `3`, and so on are _values_ of type `ℕ`, indicated by
writing `0 : ℕ`, `1 : ℕ`, `2 : ℕ`, `3 : ℕ`, and so on.

The set of natural numbers is infinite, yet we can write down
its definition in just a few lines.  Here is the definition
as a pair of _inference rules_:

                    m : ℕ
    --------      ---------
    zero : ℕ      suc m : ℕ

Inference rules tell us how we can draw conclusions given certain
forms of evidence.  Every inference rule has a horizontal line with
one statement or _judgment_ below it.  The judgment below the line is
called the _conclusion_ of the rule.  Each inference rule may also
have one or more judgments above the horizontal line, called the
_premises_ of the rule.

The first rule above tells us that zero is a natural number.  We do
not need any additional evidence to reach this conclusion, because the
rule has no premises at all: an inference rule _may_ have premises,
but it is not required to have them.  Since the first rule does not
use any other facts about natural numbers as a premise, we can see it
as a _base case_ for identifying natural numbers.

The second rule above does have one premise.  It tells us that if we
have evidence that some value is a natural number, then with the
second rule we have evidence that the _successor_ of the value is also
a natural number (`suc` is short for successor).  Since the second
rule does use evidence about another value being a natural number, we
can see it as an _inductive case_ for identifying natural numbers.

We can combine uses of inference rules to reach more complicated
conclusions that what a single rule can give us.  For example, we
write `2` just as shorthand for `suc (suc zero)` — the successor of
the successor of zero.  How do we know that `2 : ℕ`?  To start, we
know from the first inference rule that zero is, in fact, a natural.

    --------
    zero : ℕ

We can now apply the second rule to this first result: since zero is a
natural, its successor must be as well.

      --------
      zero : ℕ
    ------------
    suc zero : ℕ

When we line up inference rules this way, with the premise of one
supplied by the conclusion of another, we call the resulting structure
a _proof tree_.  We can use the conclusion of this proof tree to
supply the premise of the second inference rule again:

         --------
         zero : ℕ
       ------------
       suc zero : ℕ
    ------------------
    suc (suc zero) : ℕ

We have used our inference rules to show that `2` is a natural number,
and could continue the process to address any of the values we
normally consider to be naturals.

## The naturals as an inductive datatype

Rules like the two inference rules for the naturals correspond to a
_datatype_ definition in Agda:

```
data ℕ : Set where
  zero : ℕ
  suc  : ℕ → ℕ
```

Here `ℕ` is the name of the datatype we are defining, and `zero` and
`suc` are the _constructors_ of the datatype.  To the right of the
colon `:` after each of these two names, we have a *type declaration*
for that name.  Both the two informal inference rules and the two type
declarations in the datatype definition tell us the same two things:

* _Base case_: `zero` is a natural number.
* _Inductive case_: if `m` is a natural number, then `suc m` is also a
  natural number.

Further, these two rules give the _only_ ways of creating natural numbers.
Hence, the possible natural numbers are:

    zero
    suc zero
    suc (suc zero)
    suc (suc (suc zero))
    ...

We will see in the Pragma section below how we can associate the usual
base-10 notation with these expressions.

#### Exercise `seven` (starting) {#seven}

Write out `7` in longhand.

```
-- Your code goes here
```

#### Exercise `try-nat-defs` (starting) {#try-nat-defs}

This exercise essentially repeats the [`try-days` exercise of the
previous chapter]({{ site.baseurl }}/Basics/#try-days) for practicing
the use of `agda-mode` to examine and evaluate expressions.

 - First, open this source file `Naturals.lagda.md` in Emacs, and load
   it into an Agda process with `C-c C-l`.

 - Next, type `C-c C-n`.  Emacs will ask for an expression; try
   entering `suc (suc zero)`, and press the Enter key.

 - Emacs will display the result of simplifying this expression in a
   new-subwindow.  Agda understands the relationship between the
   formal longhand of `suc` and `zero` constructors, and the nice
   shorthand `2` which it shows in the sub-window.

Expression evaluation with `C-c C-n` will be a regular tool for you as
we encounter new definitions.  Take the time to experiment with
evaluating different expressions to make sure that you understand all
of the definitions we will encounter.

## Unpacking the datatype definition

Let's unpack the Agda definition. The keyword `data` tells us this is an
inductive definition, that is, that we are defining a new datatype
with constructors.  The phrase

    ℕ : Set

tells us that `ℕ` is the name of the new datatype, and that it is a
`Set`, which is the way in Agda of saying that it is a type.  The
keyword `where` separates the declaration of the datatype from the
declaration of its constructors. Each constructor is declared on a
separate line, which is indented to indicate that it belongs to the
corresponding `data` declaration.  The lines

    zero : ℕ
    suc  : ℕ → ℕ

give _signatures_ specifying the types of the constructors `zero` and `suc`.
They tell us that `zero` is a natural number and that `suc` takes a natural
number as argument and returns a natural number.

You may have noticed that `ℕ` and `→` don't appear on your keyboard.
They are symbols in _unicode_.  At the end of each section is a list
of all unicode symbols introduced in that section including
instructions on how to type them in the Emacs text editor.  Here
_type_ refers to typing with fingers as opposed to data types!


## The story of creation

Let's look again at the rules that define the natural numbers:

* _Base case_: `zero` is a natural number.
* _Inductive case_: if `m` is a natural number, then `suc m` is also a
  natural number.

Hold on! The second line defines natural numbers in terms of natural
numbers. How can that possibly be allowed?  Isn't this as useless a
definition as "Brexit means Brexit"?

In fact, it is possible to assign our definition a meaning without
resorting to unpermitted circularities.  Furthermore, we can do so
while only working with _finite_ sets and never referring to the
_infinite_ set of natural numbers.

We will think of it as a creation story.  To start with, we know about
no natural numbers at all:

    -- In the beginning, there are no natural numbers.

Now, we apply the rules to all the natural numbers we know about.  The
base case tells us that `zero` is a natural number, so we add it to the set
of known natural numbers.  The inductive case tells us that if `m` is a
natural number (on the day before today) then `suc m` is also a
natural number (today).  We didn't know about any natural numbers
before today, so the inductive case doesn't apply:

    -- On the first day, there is one natural number.
    zero : ℕ

Then we repeat the process. On the next day we know about all the
numbers from the day before, plus any numbers added by the rules.  The
base case tells us that `zero` is a natural number, but we already knew
that. But now the inductive case tells us that since `zero` was a natural
number yesterday, then `suc zero` is a natural number today:

    -- On the second day, there are two natural numbers.
    zero : ℕ
    suc zero : ℕ

And we repeat the process again. Now the inductive case
tells us that since `zero` and `suc zero` are both natural numbers, then
`suc zero` and `suc (suc zero)` are natural numbers. We already knew about
the first of these, but the second is new:

    -- On the third day, there are three natural numbers.
    zero : ℕ
    suc zero : ℕ
    suc (suc zero) : ℕ

You've got the hang of it by now:

    -- On the fourth day, there are four natural numbers.
    zero : ℕ
    suc zero : ℕ
    suc (suc zero) : ℕ
    suc (suc (suc zero)) : ℕ

The process continues.  On the _n_'th day there will be _n_ distinct
natural numbers. Every natural number will appear on some given day.
In particular, the number _n_ first appears on day _n+1_. And we
never actually define the set of numbers in terms of itself. Instead,
we define the set of numbers on day _n+1_ in terms of the set of
numbers on day _n_.

A process like this one is called _inductive_. We start with nothing, and
build up a potentially infinite set by applying rules that convert one
finite set into another finite set.

The rule defining zero is called a _base case_, because it introduces
a natural number even when we know no other natural numbers.  The rule
defining successor is called an _inductive case_, because it
introduces more natural numbers once we already know some.  Note the
crucial role of the base case.  If we only had inductive rules, then
we would have no numbers in the beginning, and still no numbers on the
second day, and on the third, and so on.  An inductive definition lacking
a base case is useless, as in the phrase "Brexit means Brexit".


## Philosophy and history

A philosopher might observe that our reference to the first day,
second day, and so on, implicitly involves an understanding of natural
numbers.  In this sense, our definition might indeed be regarded as in
some sense circular, but we need not let this disturb us.
Everyone possesses a good informal understanding of the natural
numbers, which we may take as a foundation for their formal
description.

While the natural numbers have been understood for as long as people
can count, the inductive definition of the natural numbers is relatively
recent.  It can be traced back to Richard Dedekind's paper "_Was sind
und was sollen die Zahlen?_" (What are and what should be the
numbers?), published in 1888, and Giuseppe Peano's book "_Arithmetices
principia, nova methodo exposita_" (The principles of arithmetic
presented by a new method), published the following year.


## Pragmas

In Agda, any text following `--` or enclosed between `{-`
and `-}` is considered a _comment_.  Comments have no effect on the
code, with the exception of one special kind of comment, called a
_pragma_, which is enclosed between `{-#` and `#-}`.

The pragma

```
{-# BUILTIN NATURAL ℕ #-}
```

after our definition of `ℕ` tells Agda that `ℕ` corresponds to the
natural numbers, and hence one is permitted to type `0` as shorthand
for `zero`, `1` as shorthand for `suc zero`, `2` as shorthand for `suc
(suc zero)`, and so on. The pragma must be given a previously declared
type (in this case `ℕ`) with precisely two constructors, one with no
arguments (in this case `zero`), and one with a single argument of the
given type (in this case `suc`).

As well as enabling the above shorthand, the pragma also enables a
more efficient internal representation of naturals.  Agda is built on
the language Haskell, which has well-tuned libraries and a compiler
which generates very efficient machine code.  This pragma allows `ℕ`
to be translated directly to the Haskell type for arbitrary-precision
integers.  Representing the natural _n_ with `zero` and `suc` requires
space proportional to _n_, whereas Haskell's arbitrary-precision
integer representation only requires space proportional to the
logarithm of _n_.


## Imports

Shortly we will want to write some equations that hold between terms
involving natural numbers.  To support doing so, we again import the
definition of equality and notations for reasoning about it from the
Agda standard library:

```
import Relation.Binary.PropositionalEquality as Eq
open Eq using (_≡_; refl)
open Eq.≡-Reasoning using (begin_; _≡⟨⟩_; _∎)
```

We have already seen the first two lines when we were giving unit
tests for boolean-valued functions.  The third line takes a module
that specifies operators to support reasoning about equivalence, and
adds all the names specified in the `using` clause into the current
scope.  In this case, the names added are `begin_`, `_≡⟨⟩_`, and `_∎`.
We will see how these are used below.  We take these as givens for
now, but will see how they are defined in the [Equality]({{
site.baseurl }}/Equality/) section.

We have already seen that Agda uses underbars to indicate where terms
appear in infix or mixfix operators. Thus, `_≡⟨⟩_` (like `_≡_` in the
previous section) is an infix operator, while `begin_` is prefix (it
is written before a term), and `_∎` is postfix (it is written after a
term).

## Operations on naturals are recursive functions {#plus}

Now that we have the natural numbers, what can we do with them?
For instance, can we define arithmetic operations such as
addition and multiplication?

As a child I spent much time memorising tables of addition and
multiplication.  At first the rules seemed tricky and I would often
make mistakes.  It came as a shock to me to discover _recursion_,
a simple technique by which every one of the infinite possible
instances of addition and multiplication can be specified in
just a couple of lines.

Here is the definition of addition in Agda:
```
_+_ : ℕ → ℕ → ℕ
zero + n = n
(suc m) + n = suc (m + n)
```

Let's unpack this definition.  Addition is an infix operator.  It is
written with underbars where the arguments go, hence its name is
`_+_`.  The first line is a signature specifying the type of the operator.
The type `ℕ → ℕ → ℕ`, indicates that addition accepts two naturals
and returns a natural.  Infix notation is just a shorthand for application;
the terms `m + n` and `_+_ m n` are equivalent.

The definition has a base case and an inductive case, corresponding to
those for the natural numbers.  The base case says that adding zero to
a number, `zero + n`, returns that number, `n`.  The inductive case
says that adding the successor of a number to another number,
`(suc m) + n`, returns the successor of adding the two numbers, `suc (m + n)`.
We say we use _pattern matching_ when constructors appear on the
left-hand side of an equation.

If we write `zero` as `0` and `suc m` as `1 + m`, the definition turns
into two familiar equations:

     0       + n  ≡  n
     (1 + m) + n  ≡  1 + (m + n)

The first follows because zero is an identity for addition, and the
second because addition is associative.  In its most general form,
associativity is written

     (m + n) + p  ≡  m + (n + p)

meaning that the location of parentheses is irrelevant.  We get the
second equation from the third by taking `m` to be `1`, `n` to be `m`,
and `p` to be `n`.  We write `=` for definitions, while we
write `≡` for assertions that two already defined things are the same.

The definition is _recursive_, in that the last line defines addition
in terms of addition.  As with the inductive definition of the
naturals, the apparent circularity is not a problem.  It works because
addition of larger numbers is defined in terms of addition of smaller
numbers.  Such a definition is called _well founded_.

For example, let's add two and three:
```
_ : 2 + 3 ≡ 5
_ =
  begin
    2 + 3
  ≡⟨⟩    -- is shorthand for
    (suc (suc zero)) + (suc (suc (suc zero)))
  ≡⟨⟩    -- inductive case
    suc ((suc zero) + (suc (suc (suc zero))))
  ≡⟨⟩    -- inductive case
    suc (suc (zero + (suc (suc (suc zero)))))
  ≡⟨⟩    -- base case
    suc (suc (suc (suc (suc zero))))
  ≡⟨⟩    -- is longhand for
    5
  ∎
```
we can make our proof a bit clearer by waiting to expand some shorthand
```
_ : 2 + 3 ≡ 5
_ =
  begin
    2 + 3
  ≡⟨⟩    -- is shorthand for
    (suc 1) + 3
  ≡⟨⟩    -- inductive case
    suc (1 + 3)
  ≡⟨⟩    -- 1 is shorthand for
    suc (suc zero + 3)
  ≡⟨⟩    -- inductive case
    suc (suc (zero + 3))
  ≡⟨⟩    -- base case
    suc (suc 3)
  ≡⟨⟩    -- has shorthand
    5
  ∎
```

The first line matches the inductive case by taking `m = 1` and `n =
3`, the second line matches the inductive case by taking `m = 0` and
`n = 3`, and the third line matches the base case by taking `n = 3`.

Both derivations consist of a signature or declaration on the first
line, and a binding on the remaining lines.  The signatures have three
elements: a name, a colon to separate the name from what follows, and
the example we want to make sure is true.  Here we use the underscore
`_` as a dummy name.  The dummy name can be reused, and is convenient
for examples.  Names other than `_` must be used only once in a
module.

In the binding, to the right of the `=` we have _evidence_ for the
equality which we assert in the signature.  Above we have written the
evidence in tabular form as a chain of equations.  The chain starts
with `begin` and finishes with `∎` (pronounced "Q.E.D." or
"tombstone," the latter from its appearance), and consists of a series
of terms separated by `≡⟨⟩`.  Agda's evaluation of expressions allows
it to verify that each equality in the series really does hold.

In fact, both proofs are longer than need be, and Agda is satisfied
with the following:

```
_ : suc (suc zero) + suc (suc (suc zero)) ≡ suc (suc (suc (suc (suc zero))))
_ = refl
```

We have taught Agda how to compute the value of `2 + 3`, so it can
immediately check that the result is the same as `5`.  Recall that
`refl` tells Agda that it can use evaluation to show that the two
expressions on either side of the `≡` are equal.  These uses of `refl`
form unit tests for the given examples.

In the chains of equations, all Agda checks is that each term
simplifies to the same value. If we jumble the equations, omit lines,
or add extraneous lines it will still be accepted.  It's up to us to
write the equations in an order that makes sense to a human reader.

There is a striking resemblence between the way we wrote function
definitions like `_+_`, `_∨_`, and `not`, and the way write these
examples and the evidence to justify them.  Both have a signature
declaring a name and its use.  Both then have a binding with more
details about the name.  This resemblance is no coincidence!  There is
a deep connection between the programming view of types and
computation, and the logical view of claims and justfying evidence.
This duality of interpretation — of a type as a proposition, and of a
term as evidence — is central to how we formalise concepts in Agda,
and will be a running theme throughout this book.

Note that when we use the word _evidence_ it is nothing equivocal.  It
is not like testimony in a court which must be weighed to determine
whether the witness is trustworthy.  Rather, it is ironclad.  The
other word for evidence, which we will use interchangeably, is _proof_.

#### Exercise `+-example` (practice) {#plus-example}

Compute `3 + 4`, writing out your reasoning as a chain of equations,
using the equations for `+`.

```
-- Your code goes here
```


## Multiplication

Once we have defined addition, we can define multiplication
as repeated addition:
```
_*_ : ℕ → ℕ → ℕ
zero    * n  =  zero
(suc m) * n  =  n + (m * n)
```
Computing `m * n` returns the sum of `m` copies of `n`.

Again, rewriting turns the definition into two familiar equations:

    0       * n  ≡  0
    (1 + m) * n  ≡  n + (m * n)

The first follows because zero times anything is zero, and the second
follows because multiplication distributes over addition.
In its most general form, distribution of multiplication over addition
is written

    (m + n) * p  ≡  (m * p) + (n * p)

We get the second equation from the third by taking `m` to be `1`, `n`
to be `m`, and `p` to be `n`, and then using the fact that one is an
identity for multiplication, so `1 * n ≡ n`.

Again, the definition is well founded in that multiplication of
larger numbers is defined in terms of multiplication of smaller numbers.

For example, let's multiply two and three:
```
_ : 2 * 3 ≡ 6
_ =
  begin
    2 * 3
  ≡⟨⟩    -- inductive case
    3 + (1 * 3)
  ≡⟨⟩    -- inductive case
    3 + (3 + (zero * 3))
  ≡⟨⟩    -- base case
    3 + (3 + zero)
  ≡⟨⟩    -- simplify
    6
  ∎
```
The first line matches the inductive case by taking `m = 1` and `n = 3`,
The second line matches the inductive case by taking `m = 0` and `n = 3`,
and the third line matches the base case by taking `n = 3`.
Here we have omitted the signature declaring `_ : 2 * 3 ≡ 6`, since
it can easily be inferred from the corresponding term.


#### Exercise `*-example` (practice) {#times-example}

Compute `3 * 4`, writing out your reasoning as a chain of equations,
using the equations for `*`.  (You do not need to step through the
evaluation of `+`.)

```
-- Your code goes here
```


#### Exercise `_^_` (recommended) {#power}

Define exponentiation, which is given by the following equations:

    m ^ 0        =  1
    m ^ (1 + n)  =  m * (m ^ n)

Check that `3 ^ 4` is `81`.

```
-- Your code goes here
```

## Monus

We can also define subtraction.  Since there are no negative
natural numbers, if we subtract a larger number from a smaller
number we will take the result to be zero.  This adaption of
subtraction to naturals is called _monus_ (a twist on _minus_).

Monus is our first use of a definition that uses pattern
matching against both arguments:
```
_∸_ : ℕ → ℕ → ℕ
m     ∸ zero   =  m
zero  ∸ suc n  =  zero
suc m ∸ suc n  =  m ∸ n
```
We can do a simple analysis to show that all the cases are covered.

  * Consider the second argument.
    + If it is `zero`, then the first equation applies.
    + If it is `suc n`, then we mus also consider the first argument.
      - If it is `zero`, then the second equation applies.
      - If it is `suc m`, then the third equation applies.

Again, the recursive definition is well founded because
monus on bigger numbers is defined in terms of monus on
smaller numbers.

For example, let's subtract two from three:
```
_ : 3 ∸ 2 ≡ 1
_ =
  begin
    3 ∸ 2
  ≡⟨⟩    -- Expand definitions of 3 and 2
    suc (suc (suc zero)) ∸ suc (suc zero)
  ≡⟨⟩    -- Apply the third clause of monus
    suc (suc zero) ∸ suc zero
  ≡⟨⟩    -- Apply the third clause of monus again
    suc zero ∸ zero
  ≡⟨⟩    -- Apply the first clause of monus
    suc zero
  ≡⟨⟩    -- Has shorthand
    1
  ∎
```
We did not use the second equation at all, but it will be required
if we try to subtract a larger number from a smaller one:
```
_ : 2 ∸ 3 ≡ 0
_ =
  begin
    suc (suc zero) ∸ suc (suc (suc zero))
  ≡⟨⟩    -- Apply the third clause of monus
    suc zero ∸ suc (suc zero)
  ≡⟨⟩    -- Apply the third clause of monus again
    zero ∸ suc zero
  ≡⟨⟩    -- Apply the second clause of monus
    0
  ∎
```

#### Exercise `∸-example₁` and `∸-example₂` (recommended) {#monus-examples}

Compute `5 ∸ 3` and `3 ∸ 5`, writing out your reasoning as a chain of equations.

```
-- Your code goes here
```

## Parity testing

Is 6 an even number?  Of course we know it is, but we can write a
recursive function so that Agda can know it as well.

```
isEven : ℕ → Bool
```

Functions such as `even` which return a `Bool` are sometimes called
*predicates*.

As usual, we need a base case, and the base case of our function
corresponds to the base case of the natural numbers.

```
isEven zero = true
```

But here we have not just one, but two base cases.  The second base
case is the smallest number which is *not* even, the smallest number
for which `even` should return `false`.

```
isEven (suc zero) = false
```

Our recursive case relies on the facts that:

 - If `n` is even, then `2+n` will be even as well.
 - If `n` is odd, then `2+n` will be odd as well.

```
isEven (suc (suc n)) = isEven n
```

We can verify that our function works correctly by testing several
cases.

```
_ : isEven 0 ≡ true
_ = refl

_ : isEven 1 ≡ false
_ = refl

_ : isEven 8 ≡ true
_ = refl

_ : isEven 13 ≡ false
_ = refl
```

We could define `odd` similarly,

```
odd : ℕ → Bool
odd zero = false
odd (suc zero) = true
odd (suc (suc n)) = odd n
```

#### Exercise `oddNot` (recommended) {#oddNot}
 
Replace the definition of `odd` above with one that simply negates the
result of a call to `even`.

#### Exercise `oddEven` (recommended) {#oddEven}

Agda allows us to write *mutually recursive*, where each calls the
other in their inductive cases.  Write mutually recursive versions of
`odd` and `even` called `odd'` and `even'`.

    odd' : ℕ → Bool
    even' : ℕ → Bool

    odd' zero = false
    odd' (suc n) = ?

    even' zero = false
    even' (suc n) = ?

Note that both type declarations come before both function
definitions, so that Agda knows how to type check the function bodies.

## Precedence

We often use _precedence_ to avoid writing too many parentheses.
Consider multiplication and addition.  You are used to the idea that
when you see `1+2*3`, it means `1+(2*3)`, and not `(1+2)*3`.  We say
that multiplication _binds more tightly_ than addition.  We also
sometimes say that addition _associates to the left_, and so write
`m + n + p`
to mean `(m + n) + p`.  In Agda the precedence and
associativity of infix operators needs to be declared:

```
infixl 6  _+_  _∸_
infixl 7  _*_
```
    infixl 8  _^_

These declarations state that operators `_+_` and `_∸_` have
precedence level 6, and operator `_*_` has precedence level 7.
Addition and monus bind less tightly than multiplication because they
are declared to have lower precedence.

In the same way, application binds more tightly than any operator, and
so we may write `suc m + n` to mean `(suc m) + n`.

Writing `infixl` indicates that all three operators associate to the
left.  One can also write `infixr` to indicate that an operator
associates to the right, or just `infix` to indicate that parentheses
are always required to disambiguate.

#### Exercise `ParenNats` (starting) {#ParenNats}

Add parenthesis to the following expressions to clarify the precedence
of the operators.

    2 ^ 3 ∸ 4
    2 * 3 + 4 * 5

#### Exercise `makeOperators` (starting) {#makeOperators}

Consider these declarations:

    infixl 5 _op1_
    infixl 7 _op2_

Complete the definitions of `_op1_` and `_op2` with two function
declarations.  It doesn't matter what they do; just make them distinct
enough for you to tell the difference between them as easily as you
can tell the difference between `+` and `*`.

How do `op1` and `op2` behave differently with respect to each other?
In a series of several applications of each?

Vary the declarations to use `infixr` instead of `infixl`, and to use
various different precedence values.  How does this change how the
expressions you tried above behave?

## The story of creation, revisited

Just as our inductive definition defines the naturals in terms of the
naturals, so does our recursive definition define addition in terms
of addition.

Again, it is possible to assign our definition a meaning without
resorting to unpermitted circularities.  We do so by reducing our
definition to equivalent inference rules for judgments about equality:

    n : ℕ
    --------------
    zero + n  =  n

    m + n  =  p
    ---------------------
    (suc m) + n  =  suc p

Here we assume we have already defined the infinite set of natural
numbers, specifying the meaning of the judgment `n : ℕ`.  The first
inference rule is the base case.  It asserts that if `n` is a natural number
then adding zero to it gives `n`.  The second inference rule is the inductive
case. It asserts that if adding `m` and `n` gives `p`, then adding `suc m` and
`n` gives `suc p`.

Again we resort to a creation story, where this time we are
concerned with judgments about addition:

    -- In the beginning, we know nothing about addition.

Now, we apply the rules to all the judgment we know about.
The base case tells us that `zero + n = n` for every natural `n`,
so we add all those equations.  The inductive case tells us that if
`m + n = p` (on the day before today) then `suc m + n = suc p`
(today).  We didn't know any equations about addition before today,
so that rule doesn't give us any new equations:

    -- On the first day, we know about addition of 0.
    0 + 0 = 0     0 + 1 = 1    0 + 2 = 2     ...

Then we repeat the process, so on the next day we know about all the
equations from the day before, plus any equations added by the rules.
The base case tells us nothing new, but now the inductive case adds
more equations:

    -- On the second day, we know about addition of 0 and 1.
    0 + 0 = 0     0 + 1 = 1     0 + 2 = 2     0 + 3 = 3     ...
    1 + 0 = 1     1 + 1 = 2     1 + 2 = 3     1 + 3 = 4     ...

And we repeat the process again:

    -- On the third day, we know about addition of 0, 1, and 2.
    0 + 0 = 0     0 + 1 = 1     0 + 2 = 2     0 + 3 = 3     ...
    1 + 0 = 1     1 + 1 = 2     1 + 2 = 3     1 + 3 = 4     ...
    2 + 0 = 2     2 + 1 = 3     2 + 2 = 4     2 + 3 = 5     ...

You've got the hang of it by now:

    -- On the fourth day, we know about addition of 0, 1, 2, and 3.
    0 + 0 = 0     0 + 1 = 1     0 + 2 = 2     0 + 3 = 3     ...
    1 + 0 = 1     1 + 1 = 2     1 + 2 = 3     1 + 3 = 4     ...
    2 + 0 = 2     2 + 1 = 3     2 + 2 = 4     2 + 3 = 5     ...
    3 + 0 = 3     3 + 1 = 4     3 + 2 = 5     3 + 3 = 6     ...

The process continues.  On the _m_'th day we will know all the
equations where the first number is less than _m_.

As we can see, the reasoning that justifies inductive and recursive
definitions is quite similar.  They might be considered two sides of
the same coin.


## The story of creation, finitely {#finite-creation}

The above story was told in a stratified way.  First, we create
the infinite set of naturals.  We take that set as given when
creating instances of addition, so even on day one we have an
infinite set of instances.

Instead, we could choose to create both the naturals and the instances
of addition at the same time. Then on any day there would be only
a finite set of instances:

    -- In the beginning, we know nothing.

Now, we apply the rules to all the judgment we know about.  Only the
base case for naturals applies:

    -- On the first day, we know zero.
    0 : ℕ

Again, we apply all the rules we know.  This gives us a new natural,
and our first equation about addition.

    -- On the second day, we know one and all sums that yield zero.
    0 : ℕ
    1 : ℕ    0 + 0 = 0

Then we repeat the process.  We get one more equation about addition
from the base case, and also get an equation from the inductive case,
applied to equation of the previous day:

    -- On the third day, we know two and all sums that yield one.
    0 : ℕ
    1 : ℕ    0 + 0 = 0
    2 : ℕ    0 + 1 = 1   1 + 0 = 1

You've got the hang of it by now:

    -- On the fourth day, we know three and all sums that yield two.
    0 : ℕ
    1 : ℕ    0 + 0 = 0
    2 : ℕ    0 + 1 = 1   1 + 0 = 1
    3 : ℕ    0 + 2 = 2   1 + 1 = 2    2 + 0 = 2

On the _n_'th day there will be _n_ distinct natural numbers, and
_n × (n-1) / 2_ equations about addition.  The number _n_ and all equations
for addition of numbers less than _n_ first appear by day _n+1_.
This gives an entirely finitist view of infinite sets of data and
equations relating the data.


## Writing definitions interactively

Agda is designed to be used with the Emacs text editor, and the two
in combination provide features that help to create definitions
and proofs interactively.

Begin by typing:

    _+_ : ℕ → ℕ → ℕ
    m + n = ?

The question mark indicates that you would like Agda to help with
filling in that part of the code. If you type `C-c C-l` (pressing
the control key while hitting the `c` key followed by the `l` key)
the question mark will be replaced:

    _+_ : ℕ → ℕ → ℕ
    m + n = { }0

The empty braces are called a *hole*, and 0 is a number used for
referring to the hole.  The hole will display highlighted in green.
Emacs will also create a window displaying the text

    ?0 : ℕ

to indicate that hole 0 is to be filled in with a term of type `ℕ`.
Typing `C-c C-f` will move you into the next hole.

We wish to define addition by recursion on the first argument.
Move the cursor into the hole and type `C-c C-c`.   You will be given
the prompt:

    pattern variables to case (empty for split on result):

Typing `m` will cause a split on that variable, resulting
in an update to the code:

    _+_ : ℕ → ℕ → ℕ
    zero + n = { }0
    suc m + n = { }1

There are now two holes, and the window at the bottom tells you the
required type of each:

    ?0 : ℕ
    ?1 : ℕ

Going into hole 0 and type `C-c C-,` (note that the comma is part of
the key sequence: control-c, and then control-comma) will display
information on the required type of the hole, and what free variables
are available:

    Goal: ℕ
    ————————————————————————————————————————————————————————————
    n : ℕ

This strongly suggests filling the hole with `n`.  After the hole is
filled, you can type `C-c C-space`, which will remove the hole:

    _+_ : ℕ → ℕ → ℕ
    zero + n = n
    suc m + n = { }1

Again, going into hole 1 and type `C-c C-,` will display information
on the required type of the hole, and what free variables are
available:

    Goal: ℕ
    ————————————————————————————————————————————————————————————
    n : ℕ
    m : ℕ

Going into the hole and type `C-c C-r` will fill it in with a
constructor (if there is a unique choice) or tell you what
constructors you might use, if there is a choice.  In this case, it
displays the following:

    Don't know which constructor to introduce of zero or suc

Filling the hole with `suc ?` and typing `C-c C-space` results in the
following:

    _+_ : ℕ → ℕ → ℕ
    zero + n = n
    suc m + n = suc { }1

Going into the new hole and typing `C-c C-,` gives similar information
to before:

    Goal: ℕ
    ————————————————————————————————————————————————————————————
    n : ℕ
    m : ℕ

We can fill the hole with `m + n` and type `C-c C-space` to complete
the program:

    _+_ : ℕ → ℕ → ℕ
    zero + n = n
    suc m + n = suc (m + n)

Exploiting interaction to this degree is probably not helpful for a
program this simple, but the same techniques can help with more
complex programs.  But even for a program this simple, using `C-c C-c`
to split cases can be helpful.

## More pragmas

Including the lines
```
{-# BUILTIN NATPLUS _+_ #-}
{-# BUILTIN NATTIMES _*_ #-}
{-# BUILTIN NATMINUS _∸_ #-}
```
tells Agda that these three operators correspond to the usual ones,
and enables it to perform these computations using the corresponding
Haskell operators on the arbitrary-precision integer type.
Representing naturals with `zero` and `suc` requires time proportional
to _m_ to add _m_ and _n_, whereas representing naturals as integers
in Haskell requires time proportional to the larger of the logarithms
of _m_ and _n_.  Similarly, representing naturals with `zero`
and `suc` requires time proportional to the product of _m_ and _n_ to
multiply _m_ and _n_, whereas representing naturals as integers in
Haskell requires time proportional to the sum of the logarithms of
_m_ and _n_.


#### Exercise `Bin` (stretch) {#Bin}

A more efficient representation of natural numbers uses a binary
rather than a unary system.  We represent a number as a bitstring:
```
data Bin : Set where
  ⟨⟩ : Bin
  _O : Bin → Bin
  _I : Bin → Bin
```
For instance, the bitstring

    1011

standing for the number eleven is encoded as

    ⟨⟩ I O I I

Representations are not unique due to leading zeros.
Hence, eleven is also represented by `001011`, encoded as:

    ⟨⟩ O I O I I

Define a function

    inc : Bin → Bin

that converts a bitstring to the bitstring for the next higher
number.  For example, since `1100` encodes twelve, we should have:

    inc (⟨⟩ I O I I) ≡ ⟨⟩ I I O O

Confirm that this gives the correct answer for the bitstrings
encoding zero through four.

Using the above, define a pair of functions to convert
between the two representations.

    to   : ℕ → Bin
    from : Bin → ℕ

For the former, choose the bitstring to have no leading zeros if it
represents a positive natural, and represent zero by `⟨⟩ O`.
Confirm that these both give the correct answer for zero through four.

```
-- Your code goes here
```

## Tests on natural numbers

We can define an equality testing function on two natural numbers.  As
in many languages, we use a different symbol than `=` for this
operation.  Since Agda already uses `=` for defining a function, it
uses `≡ᵇ` for comparing two natural numbers.

```
_≡ᵇ_ : ℕ → ℕ → Bool
```

There are three cases which we need to consider when comparing two
natural numbers.  Our base case is when we compare zero to zero.  In
this base case, the two numbers are obviously equal.

```
zero ≡ᵇ zero = true
```

The inductive case describes what happens when we compare two numbers
which are successors of other numbers.  In this case, our result is
the same reslt as when we compare those other numbers: if we want to
know whether `1+n` and `1+m` are the same, then we must check whether
`n` and `m` are the same.

```
suc x ≡ᵇ suc y = x ≡ᵇ y
```

Finally we have additional base cases when we can immediately decide
that numbers are defiitely different.  These cases are when the
numbers are built from different constructors.  In other words, two
numbers are different if one is zero and the other is non-zero.  We
could write these cases as two different clauses,

    zero ≡ᵇ (suc _) = false
    (suc _) ≡ᵇ zero = false

But we can also use the fact that Agda checks a function's cases in
the order we write them to capture these cases more succinctly:

```
_ ≡ᵇ _ = false
```

## Using boolean tests

The Agda standard library `Data.Bool` defines a *conditional
expression* `if_then_else_` which lets us write expressions like

    if (x ≡ᵇ y) then (x + 2) else (3 * y)

As we saw with the boolean operations `∧` and `∨`, the underscores `_`
in the declared name `if_then_else_` show where the three arguments of
this expression should appear.  Agda can use this technique of letting
underscores show where arguments appear for operators with more than
just two arguments.  But be careful when you use this feature of Agda!
It can be confusing both for writers and readers of code.

You may be used to thinking of `if` as a statement — in languages like
C or Java, the `if` block itself does not return a value.  But in a
functional language there are no statements.  In fact, the `if`
expression of Agda is like the `... ? ... : ...` operator in C-like
languages: it checks a boolean expression, and returns the result of
one of its two other arguments.

Here are some tests which we expect an implementation of `≡ᵇ` to
satisfy.

```
_ : zero ≡ᵇ zero ≡ true
_ = refl

_ : zero ≡ᵇ 1 ≡ false
_ = refl

_ : 1 ≡ᵇ zero ≡ false
_ = refl

_ : 1 ≡ᵇ 1 ≡ true
_ = refl

_ : 7 ≡ᵇ 9 ≡ false
_ = refl

_ : 11 ≡ᵇ 11 ≡ true
_ = refl
```

Let's review how we use the different symbols in these tests:

 - `≡ᵇ` is an operator (function) which we have defined ourselves, and
   which takes arguments and returns a value.

 - `≡` is the main connective in a statement we are making.  It is
   *not* a boolean function, and we could *not* use its result in a
   place where a value of type `Bool` is required!  Instead, it forms a
   sentence, whose truth Agda can help us assess.

 - `=` is the syntax Agda uses to make an association with a name
   (here `_`, used as a dummy name).

#### Exercise `natneq` (recommended) {#natneq}

Write a binary operator function `_≢ᵇ_` which is the negation of `_≡ᵇ_`.

    _≡ᵇ_ : ℕ → ℕ → Bool
    -- Your implementation goes here
    
    _ : zero ≢ᵇ zero ≡ false
    _ = refl
    
    _ : zero ≢ᵇ 1 ≡ true
    _ = refl
    
    _ : 1 ≢ᵇ zero ≡ true
    _ = refl
    
    _ : 1 ≢ᵇ 1 ≡ false
    _ = refl
    
    _ : 7 ≢ᵇ 9 ≡ true
    _ = refl
    
    _ : 11 ≢ᵇ 11 ≡ false
    _ = refl

#### Exercise `lesser` (recommended) {#lesser}

Write a function `lesser` which takes two `ℕ` arguments, and returns
the lesser of the two.

    lesser : ℕ → ℕ → ℕ
    -- Your definition here

    _ : lesser 0 0 ≡ 0
    _ = refl

    _ : lesser 2 0 ≡ 0
    _ = refl

    _ : lesser 0 3 ≡ 0
    _ = refl

    _ : lesser 2 10 ≡ 2
    _ = refl

    _ : lesser 10 3 ≡ 3
    _ = refl

#### Exercise `natcompare` (recommended) {#natcompare}

Write a binary operator `_<ᵇ_` which returns `True` when its first
argument is strictly less than its second argument

    _<ᵇ_ : ℕ → ℕ → Bool
    -- Your definition here
    
    _ : 0 <ᵇ 1
    _ = refl
    
    _ : 1 <ᵇ 9
    _ = refl

#### Exercise `factorial` (recommended) {#factorial}

Recall the standard mathematical `factorial` function:

       factorial(0) = 1
       factorial(n) = n * factorial(n-1)     (if n>0)
       
Translate this function into Agda, and write tests for applying
`factorial` to 0, 1, 2, 5 and 10.

## Standard library

The naturals, their constructors and the basic operators on them are
all defined in the standard library module `Data.Nat`:

```
-- import Data.Nat using (ℕ; zero; suc; _+_; _*_; _^_; _∸_)
```

Both this section and the standard library invoke the `NATURAL`
pragma, the former on `ℕ`, and the latter on the equivalent type
`Data.Nat.ℕ`.  Such a pragma can only be invoked once, as invoking it
twice would raise confusion as to whether `2` is a value of type `ℕ`
or type `Data.Nat.ℕ`.  Similar confusions arise if other pragmas are
invoked twice. For this reason, we will usually avoid pragmas in
future sections.  More information on pragmas can be found in the Agda
documentation.

#### Exercise `shapes` (recommended) {#shapes}

Agda's standard library also includes a library of floating-point
values.  Consider this type of geometric shapes:

    open import Data.Float
    open import Agda.Builtin.Float
      renaming (primFloatPlus   to _+_;
                primFloatMinus  to _-_;
                primFloatTimes  to _*_;
                primFloatDiv    to _÷_)
    data Shape : Set where
      circle : Float → Shape
      rectangle : Float → Float → Shape

Create a separate file for this exercise, since `Data.Float` loads
`Data.Nat`.

 - Write the functions `area` and `perimeter` which take a `Shape` and
   return the value of the respective property of the shape.

 - Add a constructor `triangle` to `Shape`.  The new constructor
   should take three values, the length of the sides of the triangle.

 - Add cases to `area` and `perimeter` for `triangle`.


### Naturals, strings and characters

As in most languages, functions in Agda's standard libraries will
relate different primitive types.  For example, in `Data.String`,

 - The function `length` returns the number of characters in a string
   as a `ℕ`.

       length "hello"

 - The function `replicate` forms a string by repeating a character a
   given number of times.

       replicate 10 'd'

(Note that these expressions will have an odd behavior if you
`C-c C-n` them from this file.  This file uses our demo version of ℕ,
where `Data.String` and `Data.Char` use the standard library version
of ℕ.  To try out the expressions in this section, load the next file
`NatData.lagda.md`, which uses `Data.Nat` instead of this file's
module.)

## Unicode

This section uses the following Unicode symbols:

    ᵇ (\^b)
    ℕ  U+2115  DOUBLE-STRUCK CAPITAL N (\bN)
    →  U+2192  RIGHTWARDS ARROW (\to, \r, \->)
    ∸  U+2238  DOT MINUS (\.-)
    ≡  U+2261  IDENTICAL TO (\==)
    ≢ U+2262  NOT IDENTICAL TO (\==n)
    ⟨  U+27E8  MATHEMATICAL LEFT ANGLE BRACKET (\<)
    ⟩  U+27E9  MATHEMATICAL RIGHT ANGLE BRACKET (\>)
    ∎  U+220E  END OF PROOF (\qed)

The command `\r` gives access to a wide variety of rightward arrows.
After typing `\r`, one can access the many available arrows by using
the left, right, up, and down keys to navigate.  The command remembers
where you navigated to the last time, and starts with the same
character next time.  The command `\l` works similarly for left arrows.
In place of left, right, up, and down keys, one may also use control
characters:

    C-b  left (backward one character)
    C-f  right (forward one character)
    C-p  up (to the previous line)
    C-n  down (to the next line)

We write `C-b` to stand for control-b, and similarly.  One can also navigate
left and right by typing the digits that appear in the displayed list.

---

*This page is derived from Wadler et al., with the factorial exercise
 from Pierce et al.  For more information see the [sources and
 authorship]({{ site.baseurl }}/Sources/) page.*
