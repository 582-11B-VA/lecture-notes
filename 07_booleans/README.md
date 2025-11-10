# Booleans

Every expression evaluates in a value. So far, we have seen three types
of values: `Number`, `String`, and `Image`. These types have an
_infinite_ set of values; there are infinitely many numbers, and
infinitely many combinations of characters and images. But how can we
represent information with only a _finite_ set of possible values?
Consider a question that can be answered with either "yes" or "no", such
as: “Is the total of the bill greater than 10?” To express this kind of
information, we use **booleans**.

A boolean (`Boolean` in Pyret) is a type that has only two possible
values: `true` and `false`. There are many operations that produce
boolean values. Comparison is a common one:

```pyret
>>> 1 == 1
true
>>> 1 == 2
false
>>> "cat" == "dog"
false
>>> "cat" == "CAT"
false
```

In general, `==` checks whether two values are equal. Note that this is
different from the single `=` used to associate names with values in the
directory.

The last example is the most interesting: it illustrates that strings
are _case-sensitive_, meaning individual letters must match in their
case for strings to be considered equal.

Sometimes, we also want to compare strings to determine their
alphabetical order. Here are several examples:

```pyret
>>> "a" < "b"
true
>>> "a" >= "c"
false
>>> "that" < "this"
true
>>> "alpha" < "beta"
true
```

These comparisons follows the alphabetical order we're used to ("a"
_comes before_ "b", "a" _does not come after_ "c", etc.), but others
need some explaining:

```pyret
>>> "a" >= "C"
true
>>> "a" >= "A"
true
```

These use a convention laid down a long time ago in a system called
[ASCII].

There are even more boolean-producing operators, such as:

```pyret
>>> string-contains("will", "will.i.am")
true
```

In fact, just about every kind of data will have some boolean-valued
operators to enable comparisons.

[ASCII]: https://en.wikipedia.org/wiki/ASCII

## Combining booleans

Often, we want to base decisions on more than one boolean value. For
instance, you are allowed to vote if you're a citizen of a country _and_
you are above a certain age. You're allowed to board a bus if you have a
ticket _or_ the bus is having a free-ride day. We can even combine
conditions: you're allowed to drive if you are above a certain age _and_
have good eyesight _and_ either pass a test _or_ have a temporary
license. Also, you're allowed to drive if you are _not_ inebriated.

Corresponding to these forms of combinations, Pyret offers three main
operations: `and`, `or`, and `not`. Here are some examples of their use:

```pyret
>>> (1 < 2) and (2 < 3)
true
>>> (1 < 2) and (3 < 2)
false
>>> (1 < 2) or (2 < 3)
true
>>> (3 < 2) or (1 < 2)
true
>>> not(1 < 2)
false
```
