# Documentation

Consider the `three-stripe-flag` function we implemented previously:

```pyret
fun three-stripe-flag(top, middle, bottom):
  frame(
    above(rectangle(120, 30, "solid", top),
      above(rectangle(120, 30, "solid", middle),
        rectangle(120, 30, "solid", bottom))))
end
```

What if we made a mistake, and tried to call the function as follows:

```pyret
three-stripe-flag(50, "blue", "red")
```

The first parameter to `three-stripe-flag` is supposed to be the color
of the top stripe. The value `50` is not a string (much less a string
naming a color). Pyret will substitute `50` for `top` in the first call
to `rectangle`, yielding the following:

```pyret
frame(
  above(rectangle(120, 30, "solid", 50),
    above(rectangle(120, 30, "solid", "blue"),
      rectangle(120, 30, "solid", "red"))))
```

When Pyret tries to evaluate the `rectangle` expression to create the
top stripe, it generates an error that refers to that call to
`rectangle`.

If someone else were using your function, this error might not make
sense: they didn't write an expression about rectangles. Wouldn't it be
better to have Pyret report that there was a problem in the use of
`three-stripe-flag` itself?

As the author of `three-stripe-flag`, you can make that happen by
annotating the parameters with information about the expected type of
value for each parameter. Here's the function definition again, this
time requiring the three parameters to be strings:

```pyret
fun three-stripe-flag(top :: String, middle :: String, bottom :: String):
  frame(
    above(rectangle(120, 30, "solid", top),
      above(rectangle(120, 30, "solid", middle),
        rectangle(120, 30, "solid", bottom))))
end
```

Notice that the notation here is similar to what we saw in contracts
within the documentation: the parameter name is followed by a
double-colon (`::`) and a type name (so far, one of `Number`, `String`,
or `Image`).

Run your file with this new definition and try the erroneous call again.
You should get a different error message that is just in terms of
`three-stripe-flag`.

It is also common practice to add a type annotation that captures the
type of the function's output. That annotation goes after the list of
parameters:

```pyret
fun three-stripe-flag(
      top :: String,
      middle :: String,
      bottom :: String) -> Image:
  frame(
    above(rectangle(120, 30, "solid", top),
      above(rectangle(120, 30, "solid", middle),
        rectangle(120, 30, "solid", bottom))))
end
```

We will think of types as playing two roles: giving Pyret information
that it can use to focus error messages more accurately, and guiding
human readers of programs as to the proper use of user-defined
functions.

Programmers also annotate a function with a **docstring**, a short,
human-language description of _what_ the function does. Here's what the
Pyret docstring might look like for `three-stripe-flag`:

```pyret
fun three-stripe-flag(
      top :: String,
      middle :: String,
      bottom :: String) -> Image:
  doc: "produce image of flag with three equal-height horizontal stripes"
  frame(
    above(rectangle(120, 30, "solid", top),
      above(rectangle(120, 30, "solid", middle),
        rectangle(120, 30, "solid", bottom))))
end
```

Docstrings are extremely helpful to anyone who has to read your program,
whether that is a co-worker, grader… or yourself, a couple of weeks from
now.
