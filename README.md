# Spell Checker

Let's write a spell checker!

This is an experimental teaching tool. The idea is to solve a real problem, but
do it in small steps, introducing additional complexity only as it is called for
and discussing each new concept thoroughly. At the same time, there is no
practical limit to how "deep" the rabbit hole can go (spell checkers can get
pretty complicated), so there is ample room for the project to grow with the
student.

This tutorial assumes that you've already learned a bit about programming in
Python and how to use a command line interface. It doesn't explain concepts like
functions or variables. Instead, its focus is on building an actual piece of
software one small step at a time. The goal is to learn how to design software,
not how to use any particular programming language.

## Overview

First, let's think about how the spell checker should work. Ultimately, we want
a script (a program) that we can point at a text file and get back a list of
potentially misspelled words. Some bonus goals include using a custom
dictionary, "adding" words to the dictionary when we run the program, and
suggesting alternative spellings.

The simplest possible usage might look something like this:

```
$ ./spell-checker.py my-memoirs.txt

Line 104:
    fujative -> fugitive
    parrole  -> parole, parrot
Line 431:
    felany   -> felony
```

We will use the Python 3 programming language, at least to start with. Nothing
else is required to get started.

## Phase 1

First, we need a function that can decide whether a word is misspelled, given a
word and a dictionary. This isn't going to do anything fancy like suggest an
alternative spelling, it's just going to tell us, `True` or `False`, whether the
word appears in the dictionary.

However, we do need to account for a couple things. First, capitalization. We
probably don't want "Pursuit" to come back misspelled because the dictionary
only contains "pursuit". Second, it would be great if we could handle at least a
few syntax constructs. For example, "prisoner's" shouldn't need to be in the
dictionary if "prisoner" is already there.

Ideally, we would also like to be able to handle plurals. These get tricky,
though, since some words are made plural by adding an "s" (rabbits), others with
an "es" (foxes), and still others with different spelling entirely (wolves). So,
we can ignore plurals and assume that our dictionary will contain both singular
and plural versions of each word.

The function prototype (the bit that declares the name and parameters) should
look like the one below. Note that we're using Python 3 type annotations here to
make things a bit easier to understand. The snippet below declares a function
that accepts two parameters: `word`, a string, and `dictionary`, a list of
strings. It returns a boolean value (`True` or `False`).

```python3
from typing import List

def is_misspelled(word: str, dictionary: List[str]) -> bool:
    pass
```

The function should pass the following tests:

```python3
assert is_misspelled("foo", ["bar", "foo"])
assert is_misspelled("Foo", ["bar", "foo"])
assert is_misspelled("foo's", ["bar", "foo"])
assert not is_misspelled("baz", ["bar", "foo"])
```

## Phase 2

We're going to need a way to represent a misspelled word within the program so
that we can eventually print out the line number and such. We can use a class to
do this. The class needs two fields (for now): the line number, and the word
that was misspelled.

The class should pass these tests:

```python3
m = Misspelling(47, "punnishment")

assert m.line_number == 47
assert m.word == "punnishment"
```

## Phase 3

Now that we've got a function that can check a single word, let's make a
function that can check an entire paragraph (or more). Note that we return a
list of `Misspelling` objects, each containing a line number and a misspelled
word. This is because the content could have more than one misspelled word! Note
that this function will need to call the `is_misspelled` function we wrote
earlier.

This function will operate by breaking the content up into words and checking
each one. However, it will need to handle some special circumstances, like words
that have puncuation immediately following them (such as at the end of a
sentence or before a comma).

```
from typing import List

def check_spelling(content: str, dictionary: List[str]) -> List[Misspelling]:
    pass
```

The function should pass the following tests:

```python3
assert check_spelling("foo bar", ["foo", "bar"]) == []
assert check_spelling("Foo bar", ["foo", "bar"]) == []
assert check_spelling("foo. bar", ["foo", "bar"]) == []
assert check_spelling("foo bar", ["foo"]) == [Misspelling(1, 'bar')]
```

