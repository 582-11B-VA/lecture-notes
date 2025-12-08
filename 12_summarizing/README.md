# Summarizing

The `filter` and `map` functions have clear purposes: respectively, to
filter and transform the elements of a list. But what if we want to
_summarize_ a list? What if we need to examine each of its elements, and
reduce them to a single value? That's what the `fold` function does.

Consider the operation of summing a list of numbers (e.g., 1, 2, 3).
First, we need an **accumulator**, which starts at 0. Then, we add the
numbers in turn to the accumulator until we arrive at the result: 0 + 1
= 1, 1 + 2 = 3, and 3 + 3 = 6.

The `fold` function works similarly. It takes a function, an accumulator
and a list, and applies the function to each list element, updating the
accumulator with the result. For instance, here's how to use it to sum a
list of numbers:

```pyret
>>> fold(lam(acc, n): acc + n end, 0, [list: 1, 2, 3])
6
```

The function passed to `fold` receives two arguments: the accumulator
`acc` and each list element `n` in turn. In this case, it's called
thrice by Pyret: once for `1`, once for `2` and once for `3`. The value
of `acc` is `0` for the first call. Then, it becomes the return value of
the previous call. Here, the value of `acc` is `1` (0 + 1) for the
second call, and `3` (1 + 2) for the third call. The final result `6`
(3 + 3) is the return value of the last call.
