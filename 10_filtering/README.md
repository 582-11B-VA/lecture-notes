# Filtering

Imagine that we wanted to filter an e-commerce catalogue to keep only
products priced below ten dollars. With what we've studied so far, how
might we try to write this? We could imagine using a conditional, like
follows:

```pyret
prices = [list: 3, 7, 10]

if prices.get(0) < 10:
  prices.get(0)
else if prices.get(1) < 10:
  prices.get(1)
else if prices.get(2) < 10:
  prices.get(2)
end
```

There are a couple of reasons why we might not care for this solution.
First, if we have thousands of products, this will be terribly painful
to write. Second, there's a lot of repetition; only the index is
changing. Third, what happens if there are multiple products that match
our criterion?

This conditional is, however, the spirit of what we want to do: go over
the prices one at a time, identifying those that are less than ten. We
just don't want to be responsible for manually checking each price.
Fortunately for us, Pyret knows how to do that. Given any list, it can
go over its elements one position at a time, and keep only those
matching our criterion. We just need to tell Pyret what criterion that
is.

To do so, we use a predicate function that takes one element of the
original list, and returns `true` if we want to keep it in the resulting
list. In this case, we want:

```pyret
fun is-less-than-ten(price :: Number) -> Boolean:
  doc: "Return true if price is less than ten dollars."
  price < 10
end
```

Now, we just need a way to tell Pyret to use this criterion as it
searches through the prices. We do this with a function called `filter`
which takes two arguments: our predicate function and the list we want
to filter.

```pyret
filter(is-less-than-ten, prices)
```

Under the hood, `filter` works roughly like the conditional we outlined
above: it takes each element one at a time and calls the given predicate
function on it. But what does it do with the results? If you run the
above expression, you'll see that `filter` produces a new list
containing the matching prices.

## Anonymous functions

Let's look back at the program we just wrote:

```pyret
prices = [list: 3, 7, 10]

fun is-less-than-ten(price :: Number) -> Boolean:
  doc: "Return true if price is less than ten dollars."
  price < 10
end

filter(is-less-than-ten, prices)
```

This code might feel a bit verbose: do we really need to write a helper
function just to perform something as simple as `price < 10`? Wouldn't
it be easier to just write something like:

```pyret
filter(price < 10, prices)
```

If we run this expression, Pyret will produce an "unbound identifier"
error around the use of `price`. What is `price`? We mean for `price` to
be the elements from list in turn. Conceptually, that's what `filter`
does, but we don't have the mechanics right. When we call a function, we
evaluate the arguments before the body of the function. Hence, the error
regarding `price` being unbound. The whole point of the
`is-less-than-ten` helper function is to make `price` a parameter to a
function whose body is only evaluated once a value for `price` is
available.

To tighten the code then, we have to find a way to tell Pyret to make a
temporary function that will get its argument once `filter` is running.
The following notation achieves this:

```pyret
filter(lam(price): price < 10 end, prices)
```

We have added `lam(price)` and `end` around the expression that we want
to use in the `filter`. The `lam(price)` says to Pyret: "make a
temporary function that takes `price` as an argument". The `end` serves
to end the function definition, as when we use `fun`. The keyword `lam`
is short for "lambda", a form of anonymous function definition that
exists in many languages.

In practice, we use `lam` when we have to pass simple (single line)
functions to operations like `filter` (or `map`, which we will cover
next). Whether we pass `is-less-than-ten` or the lambda to `filter`, the
body is the same. But lambdas have one advantage over regular functions:
they can be defined _inside_ other functions.

Say for instance that we wanted to write a `search-products` function
that filters products based on a given search query. To do so, our
predicate function would need to receive both the name _and_ the query
as arguments. But the function we give to `filter` can only take one
argument: the elements from the list in turn. To get around this
limitation, we can define the predicate function inside
`search-products`:

```pyret
fun search-products(products :: List<String>, query :: String) -> List<String>:
  doc: "Return the products matching the given query."
  filter(lam(product): string-contains(product, query) end, products)
where:
  search-products([list: "display, keyboard"], "key") is [list: "keyboard"]
  search-products([list: "display, keyboard"], "apple") is [list: ]
end
```

Here, our predicate function takes only one argument: `product`. But
since the lambda is defined inside `search-products`, it also has access
to the names in its scope, including `query`.
