# Structures

There are times when a single datum has many parts. We need to keep
these parts together, and sometimes take them apart. For instance:

- A Spotify entry contains a bunch of information about a single song:
  not only its name but also its singer, its length, its genre, and so
  on.
- An email application contains a bunch of information about a single
  message: its sender, the subject line, the conversation it's part of,
  the body, and quite a bit more.

In examples like this, we see the need for **structured data**: a single
datum has _structure_, i.e., it actually consists of many pieces. The
number of pieces is _fixed_, but may be of different types (some might
be numbers, some strings, some images, and different types may be mixed
together in that one datum). Some might even be other structures: for
instance, a date usually has at least three parts, the day, month, and
year. The parts of a structure are called its **fields**.

> [!NOTE]
> Different programming languages have different names to refer to
> structures and their fields. In JavaScript, for instance, we use the
> terms "objects" and "properties". In Python, they are called "objects"
> and "attributes". In Go, it's "structs" and "fields". There are some
> small differences in their capabilities depending on the language, but
> the concept is mostly the same.

## Defining structures

Let's see how we can define a structure in Pyret:

```pyret
data Song:
  song(name :: String, singer :: String, year :: Number)
end
```

This tells Pyret to introduce a new type of structured data called
`Song` (note the capitalization). According to this definition, every
song in our program has a name (a string), a singer (a string), and a
year (a number).

To create songs, we call the `song` function with the value for each
field:

```pyret
lver = song("La Vie en Rose", "Édith Piaf", 1945)
so = song("Stressed Out", "twenty one pilots", 2015)
wnkkhs = song("Waqt Ne Kiya Kya Haseen Sitam", "Geeta Dutt", 1959)
```

We say that the `song` function is the **constructor** of `Song`, since
calling it _constructs_ new `Song` instances.

## Accessing fields

Let's write a function named `song-age` that tells us how old a given
song is. First, we record some examples:

```pyret
song-age(lver) is 80
song-age(so) is 9
song-age(wnkkhs) is 66
```

As you can see, the function takes a song and produces a number. This
gives us the function's contract:

```pyret
fun song-age(s :: Song, current-year :: number) -> Number:
  doc: "Return the given song's age."
  ...
where:
  song-age(lver) is 80
  song-age(so) is 9
  song-age(wnkkhs) is 66
end
```

The `data` construct we used above creates a new type of data, which can
then be used in function contracts, just like `String`, `Number`,
`Boolean`, etc.

To access a structure's fields, we use a `.` followed by the field's
name. Thus, we get the `year` field of `s` with `s.year`. Here's the
entire function body:

```pyret
fun song-age(s :: Song, current-year :: number) -> Number:
  doc: "Return the given song's age."
  current-year - s.year
where:
  song-age(lver) is 80
  song-age(so) is 9
  song-age(wnkkhs) is 66
end
```

## Updating fields

Once a structure has been created, the value of its fields cannot be
changed. If you wish to update the value of a field, you need to create
a new instance of the structure based on the old one. For example,
here's how to change the year of the song bound to `so`:

```pyret
so2 = song(so.name, so.singer, 2016)
```

As you can see, we create a new song `so2` with the same name and singer
as `so`, but with a different year.
