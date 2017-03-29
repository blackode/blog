---
layout: post
title: Write Your Own Sigil’s — Elixir
---
![img](https://cdn-images-1.medium.com/max/720/1*IvNRYzxeBer-sxByjkGtwQ.png)**Write and Explore in Elixir**

### Sigil Overview

Sigil is a symbol or letter which can do anything with binary or string that could possible into some logical outs. You can create magic with Sigil. We all know that Elixir is such a language which can be extended further meeting the needs of project by developing our own stuff. We can build things that are not existed which increase the speed of development.
Every body likes less typing while coding. This is where the Sigil comes in to action.

> Keep your code simple and powerful.

#### Notation

> ~x/binary/option

**note:** x must be single letter.

The symbol used to identify the sigil is `~` **tilde **. Here `x` can be any letter from `a..z A..Z` but one has to be careful with predefined sigils like `c C s S w W r R` . Here I am not going to explain what all these do in real programming. 
The `binary` here is the actual string which is delimited with `/` however you can use the following as delimiters of `binary` .

> `[..] (..) <..> /../ ".." '..' |..| {..}`

All the above symbols can be used as delimiters for sigil representation. `option` here is act as modifier for the sigil.
The `option` here act as a modifier.

#### sigil_x

Here `x` stands for the sigil letter.

Every sigil has its own function trigger. Lets take a sigil `~r` which is the most used one for representing the regular expression. It actually triggers a function call to `sigil_r` function which is defined to generate regular expression for the string you passed. So sigil is actually a macro which compiles to function of its style.

#### Custom Sigils

You can build your own magical sigils of your choice. The only thing needed is you have to define functions in your module for each sigil. Lets do that.

Here we will create sigil `p` which is used to create a path. We all know that when we pass two arguments to `Path.join` it will join both strings with `/` , but it is not for multiple strings. Lets build that feature here.

`~p/user 1234 delete/` If we pass like this it has to give the string like `"user/1234/delete"` This is what we are going to build one. Another one is, if we pass the option `u` at the end like `~p/medium.com blackode/u` it has to convert that into `"https://www.medium.com/blackode"` Here this one assumes that first string is the domain name and add the rest of the strings as the path. Here `option ` stands for URL for just convention but not in real.

![img](https://cdn-images-1.medium.com/max/720/1*Gef0dBd9p7ibf-AhVzKX5A.jpeg)

**Click to Zoom — Sigil Representation**

#### Sigil Coding

**Custom Sigil Implementation**

```
def sigil_p binary, [] && def sigil_p binary, [?u]
```

Those two lines differ with an option `u` modifier to perform two different tasks.

#### Sigil Usage

```
$ iex my_sigils.ex
iex> import MySigils
MySigils
iex> ~p/user 123 delete/
"user/123/delete"
iex> ~p/medium.com blackode/
"https://www.medium.com/blackode"
```

![img](https://cdn-images-1.medium.com/max/720/1*a-xIdKW2ZvJ9FvJaHFG5OA.png)

**Execution Screen Shot**

#### Sigil documentation

We can also get the `docs` of the particular sigil by using the help function. If you want to look out `p` sigil documentation then, copy above lines of code into a file called `my_sigils.ex` and save. Now change your directory to where the file exists. Now compile the file with docs as 
`elixirc my_sigils.ex` then `iex` then `h MySigils.sigil_p`That is so simple. Similarly you can check the predefined sigils documentation with their `sigil_x` function. Here `x` is the sigil letter.

```
$ cd /path/to/file
$ elxirc my_sigils.ex
$ iex
iex> h MySigils.sigil_p
....................
documentation
.....................
```

Now it is your turn to do the magic with **sigils**.

Thanks for your time … If you like, then hit .. ❤

Happy coding !!
