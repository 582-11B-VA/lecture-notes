# Variables

As we have seen before, the `=` operator is used in Pyret to bind a
value to a name:

```pyret
>>> c = 1
```

Once this statement has been read by Pyret, evaluating the name `c` is
the same as evaluating the number `1`. In both cases, the value is (and
will always be) `1`.

```pyret
>>> c
1
```

But what if we wanted to _change_ the value assigned to `c` _after_ it
has first been initialized? In that case, Pyret offers another kind of
binding, called a **variable**. To declare a variable, we use the `var`
keyword:

```pyret
>>> var v = 1
>>> v
1
```

In contrast with a binding like `c` whose value is _constant_, the value
of a variable can change over time. At one point in our program the
value of `v` can be `1`, and later become `2`.

To assign a new value to a variable, we use the `:=` operator:

```pyret
>>> v := 2
>>> v
2
```

You might be wondering when to use variables instead of constant
bindings. Since a program in which names always refer to the same values
is easier to read and reason about, you should generally prefer
constants over variables. But variables sometimes let us express
computations more "naturally" — that is, in a way that more closely
matches how humans tend to think about a problem.

Consider the operation of summing a list of numbers (e.g., 1, 2, 3),
which we expressed before using `fold`:

```pyret
>>> fold(lam(acc, n): acc + n end, 0, [list: 1, 2, 3])
6
```

Although the answer is correct, this code might not match how we would
solve this problem in our head. Let's look at an alternative:

```pyret
>>> var acc = 0
>>> for each(n from [list: 1, 2, 3]):
      acc := acc + n
    end
>>> acc
6
```

This construct is called a `for` loop. Loops work similarly to
higher-order functions such as `fold` and `map`, but they store their
result outside of their body. Here, the result is stored in the variable
`acc`. For each number `n` in our list, we reassign `acc` to be its
previous value plus `n`.
