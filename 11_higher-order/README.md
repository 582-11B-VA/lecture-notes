# Higher-order functions

The `filter` function is what we call a **higher-order function**.
Higher-order functions are functions that take one or more functions as
arguments. While regular functions (also known as **first-order
functions**) let us abstract over _values_, higher-order functions let
us abstract over _operations_.

To better understand this, let's look at the contract for the `filter`
function:

```pyret
filter :: (f :: (a -> Boolean), lst :: List<a>) -> List<a>
```

As we've seen previously, `filter` takes two arguments:

- `f`: a function that an element `a` of the list `lst` and returns
  `true` if `a` should be included in the resulting list; and
- `lst`: the list to be filtered.

Note that _how_ we determine whether or not `a` should be included in
the resulting list is up to us. That operation was not known to the
author of `filter` when they wrote it. That's what we mean by
"abstracting operations".

## Transforming lists

Pyret includes other examples of higher-order functions. The built-in
function `map`, for instance, allows us to _transform_ every element of
a given list as to produce another list.

Say that we needed to increment every number of a list by 1. Here's how
to do it with `map`:

```pyret
fun incr(n :: Number) -> Number:
  doc: "Increment the number n by 1."
  n + 1
where:
  incr(2) is 3
  incr(4) is 5
end

map(incr, [list: 1, 2, 3])
```

When you run this program, Pyret produces `[list: 2, 3, 4]`. To obtain
this result, Pyret applied the function `incr` to `1`, then to `2`, then
to `3`, putting the return value of each call in a new list.

Here's the same program but using a lambda instead of a regular function
declaration:

```pyret
>>> map(lam(n): n + 1 end, [list: 1, 2, 3])
[list: 2, 3, 4]
```

## Other examples

Another higher-order function similar to `filter` and `map` is `all`.
While the function `filter` filters a list and the function `map`
transforms a list, the function `all` checks if _all_ the elements of a
list meet a given criterion.

```pyret
>>> words = [list: "hello", "world"]
>>> all(lam(word): string-contains(word, "e") end, words)
false
>>> all(lam(word): string-contains(word, "o") end, words)
true
```

As you can see, `all` also takes a predicate function as its first
argument. This function is applied in turn to every element of the given
list. The result is `true` only if _all_ the calls return `true`.
Otherwise, the result is `false`.

The function `any` works similarly, but it returns `true` if _any_ of
the calls return `true`.

```pyret
>>> any(lam(word): string-contains(word, "e") end, words)
true
>>> any(lam(word): string-contains(word, "z") end, words)
false
```
