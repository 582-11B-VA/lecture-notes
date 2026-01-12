# Aliasing

Suppose the bank accounts we defined previously can be owned by mutliple
customers. We should thus separate information about customers from that
of the account:

```pyret
data Account:
  | account(id :: Number, ref balance :: Number)
end

data Customer:
  | customer(name :: String, account :: Account)
end
```

Specifically, suppose we have two accounts (`acct1` and `acct2`), where
`acct1` is owned jointly by Elena and Jorge:

```pyret
acct1 = account(8404, 500)
acct2 = account(8405, 350)
elena = customer("Elena", acct1)
jorge = customer("Jorge", acct1)
```

Now, let's say Elena earns an additional `150`. We want to update the
account to reflect this. How might we do it? First we have to access the
account itself: `elena.acct`. Then, we update it using the field update
syntax:

```pyret
a = elena.acct
a!{balance: a!balance + 150}
```

Sure enough, Elena's account will now have the value of `650` (the
original `500`, plus `150`).

```pyret
>>> elena.account!balance
650
```

The key question now is: what is Jorge's balance? Put differently, what
is the value of this expression?

```pyret
>>> jorge.account!balance
```

There are two very reasonable answers here:

1. Going by our prose, Jorge's account should also have `650`, because
   that's what it means to "share" an account.
2. Going by the visible code, Jorge's account should still have `500`,
   because the update was made through `elena.acct`, not `jorge.acct`.

What you find if you run the code is that Jorge's account also has
`650`. Thus we say that `elena.acct` and `jorge.acct` are **aliases**:
they are two different "names" for the exact same structure.

This is not the first time we have had shared data. However, until now,
it hasn't mattered that the data were aliased. But now that we have
mutation, aliases matter: the balance in `jorge.acct` has changed even
though we never made an explicit change using that name. It is as if
`elena.acct` exhibited spooky action at a distance.
