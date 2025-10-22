# Names

The expressions that create images involve a bit of typing. It would be
nice to have shorthands so we can "name" images and refer to them by
their names. This is what the **definitions pane** is for: you can put
expressions and programs in the definitions pane, then use the "Run"
button to make the definitions available in the interactions pane.

Let's try it! Write the following code in the definitions pane:

```pyret
red-circ = circle(30, "solid", "red")
```

Then, click the "Run" button, and enter `red-circ` in the interactions
pane. You should see the red circle appear.

More generally, if you write code in the form:

```pyret
NAME = EXPRESSION
```

Pyret will associate the value of `EXPRESSION` to `NAME`. Anytime you
write the `NAME`, Pyret will automatically replace it with the value of
`EXPRESSION`. For example, if you write `x = 5 + 4` in the interactions
pane, then write `x`, Pyret will give you the value `9` (not the
original `5 + 4` expression).

What if you use a name that you haven't associated with a value? Try
typing `puppy` at the interactions pane prompt. Pyret will tell you that
the name is "unbound", meaning that it has been been associated (or
_bound_ to) a value.

## Names versus strings

At this point, we have seen words being used in two ways in programming:
(1) as data within strings and (2) as names for values (also called
**identifiers**). These are two very different uses, so it is worth
reviewing them.

- Syntactically (another way of saying “in terms of how we write it”),
  we distinguish strings and names by the presence of double quotation
  marks. Note the difference between `puppy` (name) and `"puppy"`
  (string).

- Strings can contain spaces, but names cannot. For example,
  `"hot pink"` is a valid piece of data, but `hot pink` is not a single
  name. When you want to combine multiple words into a name (like we did
  above with `red-circ`), use a hyphen to separate the words. Different
  programming languages allow different separators; for Pyret, we'll use
  hyphens.

- Entering a word as a name versus as a string at the interactions
  prompt changes the computation that you are asking Pyret to perform.
  If you enter `puppy` (the name, without double quotes), you are asking
  Pyret to lookup the value that you previously stored under that name.
  If you enter `"puppy"` (the string, with double quotes) you are simply
  writing down a piece of data (akin to typing a number like `3`): Pyret
  returns the value you entered as the result of the computation.

- If you enter a name that you have not previously associated with a
  value, Pyret will give you an "unbound identifier" error message. In
  contrast, since strings are just data, you won't get an error for
  writing a previously-unused string (there are some special cases of
  strings, such as when you want to put a quotation mark inside them,
  but we'll set that aside for now).

Novice programmers frequently confuse names and strings at first. For
now, remember that the names you associate with values using `=` cannot
contain quotation marks, while word- or text-based data must be wrapped
in double quotes.

# Expressions versus statements

Definitions and expressions are two useful aspects of programs, each
with their own role. Definitions tell Pyret to associate names with
values. Expressions tell Pyret to perform a computation and return the
result.

Enter each of the following at the interactions prompt:

- `5 + 8`
- `x = 14 + 16`
- `triangle(20, "solid", "purple")`
- `blue-circ = circle(x, "solid", "blue")`

The first and third are expressions, while the second and fourth are
definitions. What do you observe about the results of entering
expressions versus the results of entering definitions?

Hopefully, you notice that Pyret doesn't seem to return anything from
the definitions, but it does display a value from the expressions. In
programming, we distinguish **expressions**, which yield values, from
**statements**, which don't yield values but instead give some other
kind of instruction to the language. So far, definitions are the only
kinds of statements we've seen.

Assuming you still have the `blue-circ` definition from above in your
interactions pane, enter `blue-circ` at the prompt (you can re-enter
that definition if it is no longer there). Based on what Pyret does in
response, is `blue-circ` an expression or a definition?

Since `blue-circ` yielded a result, we infer that a name by itself is
also an expression. This exercise highlights the difference between
_making_ a definition and _using_ a defined name. One produces a value
while the other does not. But surely something must happen, somewhere,
when you run a definition. Otherwise, how could you use that name later?

## The program directory

Programming tools do work behind the scenes as they run programs. Given
the program `2 + 3`, for example, a calculation takes place to produce
`5`, which in turn displays in the interactions pane.

When you write a definition, Pyret makes an entry in an internal
directory in which it associates names with values. You can't see the
directory, but Pyret uses it to manage the values that you've associated
with names. If you write:

```pyret
width = 30
```

Pyret makes a new directory entry for `width` and records that `width`
has value `30`. If you then write

```pyret
height = width * 3
```

Pyret evaluates the expression on the right side (`width * 3`), then
stores the resulting value (here, `90`) alongside `height` in the
directory.

How does Pyret evaluate `width * 3`? Since `width` is a name (not a
string), Pyret looks up its value in the directory. Pyret then
substitutes that value for the name in the expression, resulting in
`30 * 3`, which then evaluates to `90`. After running these two
expressions, the directory looks like:

```
width → 30
height → 90
```

Note that the entry for `height` in the directory has the _result_ of
`width * 3`, not the expression. This will become important as we use
named values to prevent us from doing the same computation more than
once.

The program directory is an essential part of how programs evaluate. If
you are trying to track how your program is working, it sometimes helps
to track the directory contents on a sheet of paper (since you can't
view Pyret's directory).

What happens if you enter a subsequent definition for the same name,
such as `width = 50`? How does Pyret respond? What if you then ask to
see the value associated with this same name at the prompt? What does
this tell you about the directory?

When you try to give a new value to a name that is already in the
directory, Pyret will respond that the new definition "conflicts with an
earlier declaration of the same name". This is Pyret's way of warning
you that the name is already in the directory. If you ask for the value
associated with the name again, you'll see that it still has the
original value. Pyret doesn't let you change the value associated with
an existing name with the `NAME = VALUE` notation.

## Understanding the "Run" button

Now that we've learned about the program directory, let's discuss what
happens when you press the "Run" button. Let's assume the following
contents are in the definitions pane:

```pyret
width = 30
height = width * 3
blue-rect = rectangle(width, height, "solid", "blue")
```

When you press "Run", Pyret first clears out the program directory. It
then processes your file line by line, starting at the top. If you have
an `include` statement, Pyret adds the definitions from the included
library to the directory. After processing all of the lines for this
program, the directory will look like:

```
circle → <the circle operation>
rectangle → <the rectangle operation>
...
width → 30
height → 90
blue-rect → <the actual rectangle image>
```

If you now type at the interactions prompt, any use of an identifier (a
sequence of characters not enclosed in quotation marks) results in Pyret
consulting the directory.

If you now type

```pyret
beside(blue-rect, rectangle(20, 20, "solid", "purple"))
```

Pyret will look up the image associated with `blue-rect`.

Is the purple rectangle in the directory? What about the image with the
two rectangles? Neither of these shapes is in the directory. Why? We
didn't ask Pyret to store them there with a name. What would be
different if we instead wrote the following (at the interactions
prompt)?

```pyret
two-rects = beside(blue-rect, rectangle(20, 20, "solid", "purple"))
```

Now, the two-shape image would be in the directory, associated with the
name `two-rects`. The purple rectangle by itself, however, still would
not be stored in the directory.

## When to name values

The ability to name values can make it easier to build up complex
expressions. Let's put a rotated purple triangle inside a green square:

```pyret
overlay(
  rotate(45, triangle(30, "solid", "purple")),
  rectangle(60, 60, "solid", "green"))
```

However, this can get quite difficult to read and understand. Instead,
we can name the individual shapes before building the overall image:

```pyret
purple-tri = triangle(30, "solid", "purple")
green-sqr = rectangle(60, 60, "solid", "green")

overlay(rotate(45, purple-tri), green-sqr)
```

In this version, the `overlay` expression is quicker to read because we
gave descriptive names to the initial shapes.

Go one step further: let's add another purple-triangle on top of the
existing image:

```pyret
purple-tri = triangle(30, "solid", "purple")
green-sqr = rectangle(60, 60, "solid", "green")

above(
  purple-tri,
  overlay(rotate(45, purple-tri), green-sqr))
```

Here, we see a new benefit to leveraging names: we can use `purple-tri`
twice in the same expression without having to write out the longer
triangle expression more than once.

In practice, programmers don't name every individual image or expression
result when creating more complex expressions. They name ones that will
get used more than once, or ones that have particular significance for
understanding their program. We'll have more to say about naming as our
programs get more complicated.
