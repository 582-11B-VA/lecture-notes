# Tests

In each of the functions we implemented previously, we've started with
some examples of what we wanted to compute, generalized from there to a
generic formula, turned this into a function, and then used the function
in place of the original expressions.

Now that we're done, what use are the initial examples? It seems
tempting to toss them away. However, there's an important rule about
software that you should learn:

> Software evolves.

Over time, any program that has any use will change and grow, and as a
result may end up producing different values than it did initially.
Sometimes these are intended, but sometimes these are a result of
mistakes (including such silly but inevitable mistakes like accidentally
adding or deleting text while typing). Therefore, it's always useful to
keep those examples around for future reference, so you can immediately
be alerted if the function deviates from the examples it was supposed to
generalize.

Pyret makes this easy to do. Every function can be accompanied by a
`where` clause that records the examples. For instance, the
`moon-weight` function can be modified to read:

```pyret
fun moon-weight(earth-weight :: Number) -> Number:
  doc: "Compute weight on moon from weight on earth."
  earth-weight * 1/6
where:
  moon-weight(100) is 100 * 1/6
  moon-weight(150) is 150 * 1/6
  moon-weight(90) is 90 * 1/6
end
```

When written this way, Pyret will actually check the answers every time
you run the program, and notify you if you have changed the function to
be inconsistent with these examples. For instance, replace the body of
the function with `earth-weight * 1/3` and see what happens.

Of course, it's pretty unlikely you will make a mistake with a function
this simple (except through a typo). After all, the examples are so
similar to the function's own body. Later, however, we will see that the
examples can be much simpler than the body, and there is a real chance
for things to get inconsistent. At that point, the examples become
invaluable in making sure we haven't made a mistake in our program. In
fact, this is so valuable in professional software development that good
programmers always write down large collections of examples (called
**tests**) to make sure their programs are behaving as they expect.

For our purposes, we are writing examples as part of the process of
making sure we understand the problem. It's always a good idea to make
sure you understand the question before you start writing code to solve
a problem. Examples are a nice intermediate point: you can sketch out
the relevant computation on concrete values first, then worry about
turning it into a function. If you can't write the examples, chances are
you won't be able to write the function either. Examples break down the
programming process into smaller, manageable steps.
