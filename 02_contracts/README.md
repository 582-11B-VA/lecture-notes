# Contracts

Now that we are composing functions to build more complicated
expressions out of smaller ones, we will have to keep track of which
combinations make sense. Consider the following sample of Pyret code:

```pyret
8 * circle(25, "solid", "red")
```

What value would you expect this to produce? Multiplication is meant to
work on numbers, but this code asks Pyret to multiply a number and an
image. Does this even make sense?

This code does not make sense, and indeed Pyret will produce an error
message if we try to run it.

The bottom of the error message says:

> The * operator expects to be given two Numbers

Notice the word "Numbers". Pyret is telling you what kind of information
works with the `*` operator. In programming, values are organized into
**types** (e.g., number, string, image). These types are used in turn to
describe what kind of inputs and results (a.k.a., outputs) a function
works with. For example, `*` expects to be given two numbers, from which
it will return a number. The last expression we tried violated that
expectation, so Pyret produced an error message.

Talking about "violating expectations" sounds almost legal, doesn't it?
It does, and the term **contract** refers to the required types of
inputs and promised types of outputs when using a specific function.
Here are several examples of Pyret contracts (written in the notation
you will see in the documentation):

```
* :: (x1 :: Number, x2 :: Number) -> Number

circle :: (radius :: Number,
           mode :: String,
           color :: String) -> Image

rotate :: (degrees :: Number,
           img :: Image) -> Image

overlay :: (upper-img :: Image,
            lower-img :: Image) -> Image
```

Look at the notation pattern across these contracts. Can you label the
various parts and what information they appear to be giving you? Let's
look closely at the overlay contract to make sure you understand how to
read it. It gives us several pieces of information:

- There is a function called `overlay`.
- It takes two inputs (the parts within the parentheses), both of which
  have the type `Image`.
- The first input is the image that will appear on top.
- The second input is the image that will appear on the bottom.
- The output from calling the function (which follows `->`) will have
  the type `Image`.

In general, we read the double-colon (`::`) as "has the type". We read
the arrow (`->`) as "returns".

Whenever you compose smaller expressions into more complex expressions,
the types produced by the smaller expressions have to match the types
required by the function you are using to compose them. In the case of
our erroneous `*` expression, the contract for `*` expects two numbers
as inputs, but we gave an image for the second input. This resulted in
an error message when we tried to run the expression.

A contract also summarizes how many inputs a function expects. Look at
the contract for the `circle` function. It expects three inputs: a
number (for the radius), a string (for the style), and a string (for the
color). What if we forgot the style string, and only provided the radius
and color, as in:

```pyret
circle(100, "purple")
```

The error here is not about the _type_ of the inputs, but rather about
the _number_ of inputs provided.

## Format and notation errors

We've just seen two different kinds of mistakes that we might make while
programming: providing the wrong type of inputs and providing the wrong
number of inputs to a function. You've likely also run into one
additional kind of error, such as when you make a mistake with the
punctuation of programming. For example, you might have typed an example
such as these:

- `3+7`
- `circle(50 "solid" "red")`
- `circle(50, "solid, "red")`
- `circle(50, "solid," "red")`
- `circle 50, "solid," "red")`

Can you spot the error in each of these expressions? Evaluate them in
Pyret if necessary.

You already know various punctuation rules for writing prose. Code also
has punctuation rules, and programming tools are strict about following
them. While you can leave out a comma and still turn in an essay, a
programming environment won't be able to evaluate your expressions if
they have punctuation errors.

Here's the list of punctuation rules for Pyret so far:

- Spaces are required around arithmetic operators.
- Parentheses are required to indicate order of operations.
- When we use a function, we put a pair of parentheses around the
  inputs, and we separate the inputs with commas.
- If we use a double-quotation mark to start a string, we need another
  double-quotation mark to close that string.

In programming, we use the term **syntax** to refer to the rules of
writing proper expressions. Making mistakes in your syntax is common at
first. In time, you'll internalize the rules. For now, don't get
discouraged if you get errors about syntax from Pyret. It's all part of
the learning process.

## Finding other functions

At this point, you may be wondering what else you can do with images.
What other shapes might we make? Is there a list somewhere of everything
we can do with images?

Every programming language comes with **documentation**, which is where
you find out the various operations and functions that are available,
and your options for configuring their parameters. Documentation can be
overwhelming for novice programmers, because it contains a lot of detail
that you don't even know that you need just yet. Let's take a look at
how you can use the documentation as a beginner.

Open the [documentation for Pyret's image library][image library]. Focus
on the sidebar on the left. At the top, you'll see a list of all the
different topics covered in the documentation. Scroll down until you see
"rectangle" in the sidebar: surrounding that, you'll see the various
function names you can use to create different shapes. Scroll down a bit
further, and you'll see a list of functions for composing and
manipulating images.

If you click on a shape or function name, you'll bring up details on
using that function in the area on the right. You'll see the contract in
a shaded box, a description of what the function does (under the box),
and then a concrete example or two of what you type to use the function.
You could copy and paste any of the examples into Pyret to see how they
work (changing the inputs, for example).

For now, everything you need documentation wise is in the section on
images. We'll go further into Pyret and the documentation as we go.

[image library]: https://pyret.org/docs/latest/image.html
