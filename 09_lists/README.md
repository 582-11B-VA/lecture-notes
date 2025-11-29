# Lists

Most programming languages let us group related values into ordered
lists. Here is how to do so in Pyret:

```pyret
[list: "a", "b", "c"]
```

This expression is called a **list literal**. It produces a value of the
type `List` — or more precisely: `List<String>` (pronounced "list of
strings"). Lists can hold values of any type, but all elements of a
given list are expected to share the same type.

For instance, here is a list of numbers (`List<Number>`):

```pyret
[list: 1, 2, 3]
```

And a list of booleans (`List<Boolean>`):

```pyret
[list: true, false, false]
```

Since lists are values, we can give them names:

```pyret
words = [list: "This", "is", "a", "list", "of", "words"]
```

Note the use of the plural `words` to describe our list. Because lists
contain multiple values, their names are often plural.

## List operations

Pyret provides several operations for manipulating lists. Here are the
most common ones.

### Combining lists

The `+` operator can be used to combine two lists:

```pyret
>>> [list: 1, 2] + [list: 3, 4]
[list: 1, 2, 3, 4]
>>> [list: "a", "b"] + [list: "c"]
[list: "a", "b", "c"]
```

### Computing the length

The built-in `length` function returns the number of elements in a given
list:

```pyret
>>> length([list: "a", "b", "c"])
3
>>> length([list: "a", "b"])
2
>>> length([list: "a"])
1
>>> length([list: ])
0
```

### Indexing

The built-in `get` function returns the element at the given index or
position:

```pyret
>>> get([list: "a", "b", "c"], 0)
"a"
>>> get([list: "a", "b", "c"], 1)
"b"
>>> get([list: "a", "b", "c"], 2)
"c"
```

Remember that in most programming languages, the first position is 0.

As with strings, you can use `length` and `get` to access the last
element of a list:

```pyret
>>> numbers = [list: 5, 6, 7]
>>> get(numbers, length(numbers) - 1)
7
```

### Checking membership

The built-in `member` function tells you whether or not a list contains
a given value:

```pyret
>>> member([list: 1, 2, 3], 2)
true
>>> member([list: "a", "b", "c"], "a")
true
>>> member([list: 1, 2, 3], 4)
false
>>> member([list: "a", "b", "c"], "e")
false
```
