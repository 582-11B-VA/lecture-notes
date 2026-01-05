# State

Imagine that we want to represent bank accounts, where each account has
a unique identifier and a balance:

```pyret
data Account:
  | account(id :: Number, balance :: Number)
end

acct1 = acct1 = account(8404, 500)
```

Now, let's say we learn that the account has just earned another 200. We
could always reflect the resulting account as follows:

```pyret
account(acct1.id, acct1.balance + 200)
```

However, this creates a _new_ account; if we look at the current balance
of `acct1`, by writing `acct1.balance`, it is still 500.

Rather, we want to _change_ the balance in the _existing_ account. This
requires a programming feature that we have not encountered until now:
data that can be changed. Such data are called **mutable**. In contrast,
until now, we have worked with **immutable** data: data that cannot be
altered.

To create a mutable data structure in Pyret, we have to declare that one
of its field can be changed. We do so using the `ref` (short for
"reference") keyword:

```pyret
data Account:
  | account(id :: Number, ref balance :: Number)
end
```

This Pyret definition says that `id` cannot be changed, while `balance`
can. With this definition, making accounts looks the same:

```pyret
acct1 = account(8404, 500)
```

But when we view the account in the right pane, we see something
special: `500` is surrounded by a yellow-and-black "caution tape". This
indicator is a reminder that the value can change, so what is shown on
screen may not be the _current_ value.

To access a mutable field, we don't use `.` but `!`:

```pyret
acc1.id # id is immutable
acc1!balance # balance is mutable
```

The `!` is there to remind us that what we are getting is the _current_
value of `balance`, and that it may be different later.

Let's see how to change that account balance by setting it to zero:

```pyret
acct1!{balance: 0}
```

Again, we use `!` for accessing mutable fields, then we put their names
and new values inside curly brackets.

The ability to change values introduces a new phenomenon called
**state**. In the program above, there is a state (i.e., a collection of
values for the defined names) in which the balance is 500, and another
state where it is 0. From now on, every programming instruction will run
in some state, and its actions will depend on the other values in that
state. If those values change, the same instruction may produce
different answers. This makes programming much harder, and we will have
to get used to the subtleties that come along with it.
