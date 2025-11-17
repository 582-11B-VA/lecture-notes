# Conditionals

We previously implemented a function to compute the cost of ordering
pens:

```pyret
fun order-total(qty :: Number, msg :: String) -> Number:
  doc: "Compute the order total for the given quantity and message."
  unit-cost = 25
  qty * (unit-cost + msg-cost(msg))
where:
  order-total(1, "a") is 1 * (25 + msg-cost("a"))
  order-total(2, "ab") is 58
end

fun msg-cost(msg :: String) -> Number:
  doc: "Compute the cost of the given message."
  char-cost = 2
  char-cost * string-length(msg)
where:
  msg-cost("a") is 2
  msg-cost("ab") is 4
end
```

Continuing the example, we now want to account for shipping costs. We’ll
determine shipping charges based on the cost of the order.

Specifically, we will write a function `add-shipping` to compute the
total cost of an order including shipping. Assume an order valued at $10
or less ships for $4, while an order valued above $10 ships for $8. As
usual, we will start by writing examples:

```pyret
add-shipping(10) is 10 + 4
add-shipping(3.95) is 3.95 + 4
add-shipping(20) is 20 + 8
add-shipping(10.01) is 10.01 + 8
```

Our proposed examples feature several strategic decisions:

- Including `10`, which is _at the boundary_ of charges based on the
  text.
- Including `10.01`, which is _just over the boundary_.
- Including _both natural and real (decimal) numbers_.
- Including examples that should _result in each shipping charge_
  mentioned in the problem (`4` and `8`).

So far, we have used a simple rule for creating a function body from
examples: locate the parts that are changing, replace them with names,
then make the names the parameters to the function. But two things are
new in this set of examples:

- The values of `4` and `8` differ across the examples, but they each
  occur in multiple examples.
- The values of `4` and `8` appear only in the computed answers — not as
  an input. Which one we use seems to _depend_ on the input value.

These two observations suggest that something new is going on with
`add-shipping`. In particular, we have clusters of examples that share a
fixed value (the shipping charge), but different clusters (a) use
different values and (b) have a pattern to their inputs (whether the
input value is less than or equal to `10`). This calls for being able to
_ask questions about inputs_ within our programs.

To ask a question about our inputs, we use a new kind of expression
called a **conditional**. Here’s the full definition of `add-shipping`:

```pyret
fun add-shipping(order-amt :: Number) -> Number:
  doc: "Add shipping costs to order total."
  if order-amt <= 10:
    order-amt + 4
  else:
    order-amt + 8
  end
where:
  add-shipping(10) is 10 + 4
  add-shipping(3.95) is 3.95 + 4
  add-shipping(20) is 20 + 8
  add-shipping(10.01) is 10.01 + 8
end
```

In a conditional expression, we use the `if` keyword to ask a question
that can produce an answer that is `true` or `false` (here
`order-amt <= 10`). We provide one expression for when the answer to the
question is `true` (`order-amt + 4`), and another for when the result is
`false` (`order-amt + 8`). The `else` keyword marks the answer in the
`false` case; we call this the **else clause**. We also need `end` to
tell Pyret we’re done with the question and answers.

## Asking multiple questions

Shipping costs are rising, so we want to modify the `add-shipping`
function to include a third shipping level: orders between $10 and $30
ship for $8, but orders over $30 ship for $12. This calls for two
modifications to our program:

- We have to be able to ask another question to distinguish situations
  in which the shipping charge is `8` from those in which the shipping
  charge is `12`.
- The question for when the shipping charge is `8` will need to check
  whether the input is _between_ two values.

We’ll handle these in order.

The current body of `add-shipping` asks one question: `order-amt <= 10`.
We need to add another one for `order-amt <= 30`, using a charge of `12`
if that question fails. Where do we put that additional question?

An expanded version of the if-expression, using `else if`, allows you to
ask multiple questions:

```pyret
if order-amt <= 10:
  order-amt + 4
else if order-amt <= 30:
  order-amt + 8
else:
  order-amt + 12
end
```

How does Pyret determine which answer to return? It evaluates each
question expression in order, starting from the one that follows `if`.
It continues through the questions, returning the value of the answer of
the first question that returns `true`. Here’s a summary of the
if-expression syntax and how it evaluates:

```pyret
if QUESTION1:
  <result in case first question true>
else if QUESTION2:
  <result in case QUESTION1 false and QUESTION2 true>
else:
  <result in case both QUESTIONs false>
end
```

A program can have multiple `else if` cases, thus accommodating an
arbitrary number of questions within a program.

The problem description for `add-shipping` said that orders _between_
`10` and `30` should incur an `8` charge. How does the above code
capture the interval implied by the word "between"? This is currently
entirely implicit. It depends on us understanding the way an `if`
expression evaluates. The first question is `order-amt <= 10`, so if we
continue to the second question, it means `order-amt > 10`. In this
context, the second question asks whether `order-amt <= 30`. That’s how
we’re capturing "between-ness".

How might we modify the above code to include the "between 10 and 30"
requirement explicitly into the question for the `8` case? Remember the
`and` operator on booleans? We can use that to express intervals, as
follows:

```pyret
(order-amt > 10) and (order-amt <= 30)
```

Here is what `add-shipping` look like with the `and` included:

```pyret
fun add-shipping(order-amt :: Number) -> Number:
  doc: "Add shipping costs to order total."
  if order-amt <= 10:
    order-amt + 4
  else if (order-amt > 10) and (order-amt <= 30):
    order-amt + 8
  else:
    order-amt + 12
  end
where:
  add-shipping(10) is 10 + 4
  add-shipping(3.95) is 3.95 + 4
  add-shipping(20) is 20 + 8
  add-shipping(10.01) is 10.01 + 8
  add-shipping(30) is 30 + 8
  add-shipping(32) is 32 + 12
end
```

Both versions of `add-shipping` support the same examples. Are both
correct? Yes. And while the first part of the second question
(`order-amt > 10`) is redundant, it can be helpful to include such
conditions for three reasons:

- They signal to future readers (including ourselves) the condition
  covering a case.
- They ensure that if we make a mistake in writing an earlier question,
  we won’t silently get surprising output.
- They guard against future modifications, where someone might modify an
  earlier question without realizing the impact it’s having on a later
  one.
