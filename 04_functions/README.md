# Functions

Consider the following two expressions to draw the flags of Armenia and
Austria (respectively). These two countries have the same flag, just
with different colors. The `frame` operator draws a small black frame
around the image.

```pyret
# Lines starting with # are comments for human readers.
# Pyret ignores everything on a line after #.

# armenia
frame(
  above(rectangle(120, 30, "solid", "red"),
    above(rectangle(120, 30, "solid", "blue"),
      rectangle(120, 30, "solid", "orange"))))

# austria
frame(
  above(rectangle(120, 30, "solid", "red"),
    above(rectangle(120, 30, "solid", "white"),
      rectangle(120, 30, "solid", "red"))))
```

Rather than write this program twice, it would be nice to write the
common expression only once, then just change the colors to generate
each flag. Concretely, we'd like to have a custom operator such as
`three-stripe-flag` that we could use as follows:

```pyret
# armenia
three-stripe-flag("red", "blue", "orange")

# austria
three-stripe-flag("red", "white", "red")
```

In this program, we provide `three-stripe-flag` only with the
information that customizes the image creation to a specific flag. The
operation itself would take care of creating and aligning the
rectangles. We want to end up with the same images for the Armenian and
Austrian flags as we would have gotten with our original program. Such
an operator doesn't exist in Pyret: it is specific only to our
application of creating flag images. To make this program work, then, we
need the ability to add our own operators (henceforth called
**functions**) to Pyret.

## Defining functions

If we have multiple concrete expressions that are identical except for a
couple of specific data values, we create a function with the common
code as follows:

1. Write down at least two expressions showing the desired computation
   (in this case, the expressions that produce the Armenian and Austrian
   flags).

2. Identify which parts are _fixed_ (i.e., the creation of rectangles
   with dimensions `120` and `30`, the use of `above` to stack the
   rectangles) and which are _changing_ (i.e., the stripe colors).

3. For each changing part, give it a name (say `top`, `middle`, and
   `bottom`), which will be the (configuration) **parameter** that
   stands for that part.

4. Rewrite the examples to be in terms of these parameters. For example:

   ```pyret
   frame(
     above(rectangle(120, 30, "solid", top),
       above(rectangle(120, 30, "solid", middle),
         rectangle(120, 30, "solid", bottom))))
   ```

5. Name the function something suggestive: e.g., `three-stripe-flag`.
   The name of a function should tell us either what it does or what it
   produces.

6. Write the syntax for functions around the expression, also known as
   the function **body**:

   ```pyret
   fun <function name>(<parameters>):
     <the expression goes here>
   end
   ```

   In programming, we often use angle brackets to say "replace with
   something appropriate"; the brackets themselves aren't part of the
   notation.

Here's the end product:

```pyret
fun three-stripe-flag(top, middle, bottom):
  frame(
    above(rectangle(120, 30, "solid", top),
      above(rectangle(120, 30, "solid", middle),
        rectangle(120, 30, "solid", bottom))))
end
```

While this looks like a lot of work now, it won't once you get used to
it. We will go through the same steps over and over, and eventually
they'll become so intuitive that you won't need to start from multiple
similar expressions.

With this function in hand, we can write the following two expressions
to generate our original flag images:

```pyret
three-stripe-flag("red", "blue", "orange")
three-stripe-flag("red", "white", "red")
```

When we provide values for the parameters of a function to get a result,
we say that we are **calling** (or **invoking**, or **applying**) the
function. We use the term **call** for expressions of this form.

If we want to name the resulting images, we can do so as follows:

```pyret
armenia = three-stripe-flag("red", "blue", "orange")
austria = three-stripe-flag("red", "white", "red")
```

Side note: Pyret only allows one value per name in the directory. If
your file already had definitions for the names `armenia` or `austria`,
Pyret will give you an error at this point. You can use a different name
(like `austria2`) or comment out the original definition using `#`.

## How functions evaluate

So far, we have learned three rules for how Pyret processes your
program:

1. If you write an expression, Pyret evaluates it to produce its value.
2. If you write a statement that defines a name, Pyret evaluates the
   expression (right side of `=`), then makes an entry in the directory
   to associate the name with the value.
3. If you write an expression that uses a name from the directory, Pyret
   substitutes the name with the corresponding value.

Now that we can define our own functions, we have to consider two more
cases: what does Pyret do when you define a function (using `fun`), and
what does Pyret do when you call a function (with values for the
parameters)?

- When Pyret encounters a function definition in your file, it makes an
  entry in the directory to associate the name of the function with its
  code. The body of the function does not get evaluated at this time.

- When Pyret encounters a function call while evaluating an expression,
  it replaces the call with the body of the function, but with the
  parameter values substituted for the parameter names in the body.
  Pyret then continues to evaluate the body with the substituted values.

As an example of the function-call rule, if you evaluate

```pyret
three-stripe-flag("red", "blue", "orange")
```

Pyret replaces the call with the function body,

```pyret
frame(
  above(rectangle(120, 30, "solid", top),
    above(rectangle(120, 30, "solid", middle),
      rectangle(120, 30, "solid", bottom))))
```

then substitutes the parameter values,

```pyret
frame(
  above(rectangle(120, 30, "solid", "red"),
    above(rectangle(120, 30, "solid", "blue"),
      rectangle(120, 30, "solid", "orange"))))
```

and finally evaluates the expression, producing the flag image.

Note that the second expression (with the substituted values) is the
same expression we started from for the Armenian flag. Substitution
restores that expression, while still allowing the programmer to write
the shorthand in terms of `three-stripe-flag`.
