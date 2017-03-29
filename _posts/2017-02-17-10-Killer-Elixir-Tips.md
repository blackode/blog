---
layout: post
title: 10 Killer Elixir Tips
---

Happy Coding !

![img](https://cdn-images-1.medium.com/max/720/1*EIEGuEHn_nAwJvAwhNv6cQ.jpeg)

10 Killer Elixir Tips #1

If you are bad at reading watch video

YouTube Video on this Article.

#### 1. Multiple [ OR ]

#### **2. i( term)**

Prints information about the data type of any given term. Try that in `iex`and see the magic.

```
iex> i(1..5)
```

#### 3. iex Custom Configuration

Save the following file as `.iex.exs` in your `~` home directory and see the magic.

iex configuration file

![img](https://cdn-images-1.medium.com/max/720/1*iy-IELdB8fjTo5H0sABlBQ.png)

iex custom configuration

#### 4. Custom Sigils

Each `x` sigil call respective `sigil_x` definition

Defining your own sigils

#### 5. Custom Error Definitions

Defining Custom Error

**Usage**

```
$ iex bug_error.ex
iex> raise BugError
** (BugError) BUG BUG ..
iex> raise BugError, message: "I am Bug.."
** (BugError) I am Bug..
```

#### 6. Get a Value from Nested Maps

Getting a Value from Nested Maps

#### 7. with statement

The special form `with` is used to chain a sequence of matches in order and finally return the result of `do:` if all the clauses match. However, if one of the clauses does not match, its result of the miss matched expression is immediately returned.

```
iex> with 1 <- 1+0,
          2 <- 1+1,
          do: IO.puts "all matched"
"all matched"
```

```
iex> with 1 <- 1+0,
          2 <- 3+1,
          do: IO.puts "all matched"
4
## since  2 <- 3+1 is not matched so the result of 3+1 is returned.
```

**8. Writing Protocols**

Defining protocols

#### 9. Ternary Operator

There is no ternary operator like `true ? "yes" : "no"` . So, the following is suggested.

```
"no" = if 1 == 0, do: "yes", else: "no"
```

#### 10.????

Purposely I did not write the 1o tip. If you have one Share in the comment section below..
If you like them hit ❤

![img](https://cdn-images-1.medium.com/max/720/1*yY-xYJ7Y2Fm6JMOLofgVoA.gif)

Love to Recommend

to be continued …
