# Context

This was originally a solution to a hiring challenge. I didn't particularly
care about the role, but I was interested in toying with an idea I'd been
thinking about recently known as
[part blindness](https://www.hxa.name/notes/note-hxa7241-20131124T0927Z.html),
so I took this as a platform to experiment with that.

Naturally, the final solution isn't a particularly good solution for the
problem. It's way too complicated, and nobody should be using anything like
this in production. As a friend I showed this to earlier remarked, you can
probably solve this in two lines of python.

The gist of what I tried out here was to _minimize the use of naked ifs_.
For example, one constraint of the problem is that unique characters had to
appear without a number next to them. This _could_ be done by keeping just
a number and using an if condition, but instead, I defined this type:

```ml
type count =
  | Single
  | Multiple of int
```

Along with ensuring that there was only one place where the `Multiple`
constructor could appear, i.e. in a specially defined `inc` function:

```ml
let inc = function
  | Single -> Multiple 2
  | Multiple n -> Multiple (n + 1)
```

(As an aside, ideally I could've used a range type in Multiple to ensure
it was always >= 2, but ocaml doesn't have those :'()

Now, when I was generating the string for each character, I would _have_ to
deal with the situation where there was only one character, and because the
provenance of the condition was carried with the number stored _only_ along with
`Multiple`, the number of ways I could mess up were drastically cut down.

On the other hand, it turned out to be more complex when you had multiple
chained `if`s that you wanted to eliminate, and the code became more and more
unwieldy. Eventually I cut it short and left two `if`s in place.

So this turned out to be a pretty interesting experiment in the end. I spent
~30 minutes solving the problem itself, but the flipside is that I spent
exactly 0 minutes debugging it. Thinking of a way to avoid part blindness in
the core of the problem, along with the strict typechecking and refinement of
information meant that there _were_ very few ways I could mess it up. I'm
pretty happy with my solution, and while it is too complex, I'll be thinking
of ways to simplify it while retaining the power of this kind of solution.

I've retained the hiring challenge text as-is under the Usage section below.

# Usage

This solution is written in OCaml 4.11, though any OCaml version after 4.05
should be OK.

To run this solution,

1. Install ocaml. The OCaml website maintains
   [an install page](https://ocaml.org/docs/install.html).

2. No packages are required for the main program itself, however, `OUnit2`
   and `ocamlfind` are required to run tests. Install them with:

```sh
opam install ounit2 ocamlfind
```

3. Run:

```sh
make build
```

to build the `compress` executable, used as

```sh
compress <input_string>
```

4. Run:

```sh
make test
```

to run tests.

# Exercise

Write a function that compresses an alphanumeric string by collapsing consecutive values. The rules of the compression algorithm are defined by the test cases below.

## Test cases

Each item below has an input value and the expected output from the function.

1. `aaabccccdd` → `a3bc4d2`
2. `aaaaaffffffffffc` → `a5f10c`
3. `abcd` → `abcd` (note: not `a1b1c1d1`)
4. `ccceee12eccceee` → `c3e4c3e3` (numbers removed)
5. `effeac01cb65c` → `ef2eac2bc`

## Timeboxing

Try to complete this task within 30 minutes.

## What we look for

- Code structure — specifically how readable and understandable it is
  - (Optional) Consider using of whitespace and inline doc strings for clarity
- Git usage — granularity of commits and any branches or tags
- Edge cases — how they're handled so they won't be forgotten
  - (Optional) Describe or implement a solution for each case (if any)
- Bonus: Add a very basic test for each case

## What we _don't_ look for

- Language — use whatever programming language you're most comfortable with
- Number of lines — it can be any length as long as it's readable and understandable
- Time spent — just be honest and tell us how you used the time

## How to submit

1. Fork this repo to a public one on your account.
2. Add an empty file for your code and make a commit.
3. Make granular commits as you go so that each one is readible with an accurate commit message.
4. When you're done, submit your fork as a pull request back to this repo.
5. To help us stay organized, please email us at careers@quantifiedcitizen.com with the link and a few details about your experience and/or interest in working with us.
