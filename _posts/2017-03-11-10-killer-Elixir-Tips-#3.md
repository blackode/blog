---
layout: post
title: 10 killer Elixir Tips #3
---
Happy coding !!
![img](https://cdn-images-1.medium.com/max/720/1*0doefGzpEtz3VVCKJWy8sQ.jpeg)

Read if you missed [Killer Elixir Tips #1](https://medium.com/blackode/10-killer-elixir-tips-2a9be1bec9be?source=user_profile---------1----------) and [Killer Elixir Tips #2](https://medium.com/blackode/10-killer-elixir-tips-2-c5f87f8a70c8?source=user_profile---------5----------)

### 1. Functions as Guard Clauses

We cannot make use of the functions as guard clauses in elixir. It means, `when` cannot accept functions that returns Boolean values as conditions. Consider the following lines of code…

Compile Error Code lines…

Here we defined a **module** `Hello` and a function `hello` that takes two parameters of `name` and `age`. So, based on age I am trying `IO.puts`accordingly. If you do so you will get an error saying….

```
** (CompileError) hello.ex:2: cannot invoke local is_kid/1 inside guard
    hello.ex:2: (module)
```

This is because **when** cannot accept functions as guards. We need to convert them to `macros` lets do that…

Macros as guards compile successful….

In the above lines of code, we wrapped all our guards inside a module `MyGuards` and make sure the module is top of the module `Hello` so, the macros first gets compiled. Now compile and execute you will see the following output..

```
iex> Hello.hello "blackode",21
Hello Mister blackode
:ok
iex> Hello.hello "blackode",11
Hello Kid blackode
:ok
```

### 2. Finding the presence of Sub-String

Using `=~` operator we can find whether the **right** sub-string present in **left** string or not..

```
iex> "blackode" =~ "kode" 
true  
iex> "blackode" =~ "medium" 
false  
iex> "blackode" =~ "" 
true
```

### 3. Finding whether Module is loaded or not

Sometimes, we have to make sure that certain module is loaded before making a call to the function. We are supposed to ensure the module is loaded.

```
Code.ensure_loaded? <Module>
```

```
iex> Code.ensure_loaded? :kernel
true
iex> Code.ensure_loaded :kernel
{:module, :kernel}
```

Similarly we are having `ensure_compile` to check whether the module is compiled or not…

### 4. Binary to Capital Atom

Elixir provides a special syntax which is usually used for module names. What is called a module name is an **uppercase*** ASCII letter* followed by any number of **lowercase*** or ***uppercase*** ASCII letters*, *numbers*, or *underscores*.

This identifier is equivalent to an atom prefixed by `Elixir.`. So in the `defmodule Blackode` example `Blackode` is equivalent to `:"Elixir.Blackode"`

When we use `String.to_atom "Blackode"` it converts it into `:Blackode` But actually we need something like “**Blackode” **to **Blackode. **To do that we need to use `Module.concat`

```
iex(2)> String.to_atom "Blackode"
:Blackode
iex(3)> Module.concat Elixir,"Blackode"
Blackode
```

In Command line applications whatever you pass they convert it into **binary**. So, again you suppose to do some casting operations …

### 5. Pattern match [ vs ]destructure.

We all know that `=` does the pattern match for left and right side. We cannot do `[a,b,c]=[1,2,3,4]` this raise a `MatchError`

```
iex(11)> [a,b,c]=[1,2,3,4]
** (MatchError) no match of right hand side value: [1, 2, 3, 4]
```

We can use `destructure/2` to do the job.

```
iex(1)> destructure [a,b,c],[1,2,3,4]
[1, 2, 3]
iex(2)> {a,b,c}
{1, 2, 3}
```

If the left side is having more entries than in right side, it assigns the `nil`value for remaining entries..

```
iex> destructure([a, b, c], [1])
iex> {a, b, c} 
{1, nil, nil}
```

### 6. Data decoration [ inspect with :label ] option

We can decorate our output with `inspect` and `label` option. The string of `label` is added at the beginning of the data we are inspecting.

```
iex(1)> IO.inspect [1,2,3],label: "the list "
the list : [1, 2, 3]
[1, 2, 3]
```

If you closely observe this it again returns the inspected data. So, we can use them as intermediate results in `|>` pipe operations like following……

```
[1, 2, 3] 
|> IO.inspect(label: "before change") 
|> Enum.map(&(&1 * 2)) 
|> IO.inspect(label: "after change") 
|> length
```

You will see the following `output`

```
before change: [1, 2, 3]
after change: [2, 4, 6]
3
```

### 7. Anonymous functions to pipe

We can pass the anonymous functions in two ways. One is directly using `&`like following..

```
[1,2,3,4,5]
|> length
|> (&(&1*&1)).()
```

This is the most weirdest approach. How ever, we can use the reference of the anonymous function by giving its name.

```
square = & &1 * &1
[1,2,3,4,5]
|> length
|> square.()
```

The above style is much better than previous . You can also use `fn` to define anonymous functions.

### 8. Retrieve Character Integer Codepoints — ?

We can use `?` operator to retrieve character integer codepoints.

```
iex> ?a
97
iex> ?#
35
```

The following two tips are mostly useful for beginners…

### 9. Subtraction over Lists

We can perform the subtraction over lists for removing the elements in list.

```
iex> [1,2,3,4.5]--[1,2]
[3, 4.5]
iex> [1,2,3,4.5,1]--[1]  
[2, 3, 4.5, 1]
iex> [1,2,3,4.5,1]--[1,1]
[2, 3, 4.5]
iex> [1,2,3,4.5]--[6]    
[1, 2, 3, 4.5]
```

We can also perform same operations on char lists too..

```
iex(12)> 'blackode'--'ode'
'black'
iex(13)> 'blackode'--'z'    
'blackode'
```

If the element to subtract is not present in the list then it simply returns the list.

### 10. Using Previous results in IEx

When you are working with `iex` environment , you can see a number increment every time you evaluate an expression in the shell like `iex(2)>``iex(3)>`

Those numbers helps us to reuse the result with `v/1` function which has been loaded by default..

```
iex(1)> list = [1,2,3,4,5]
[1, 2, 3, 4, 5]
iex(2)> double_lsit = Enum.map(list, &(&1*2))
[2, 4, 6, 8, 10]
iex(3)> v 1         
[1, 2, 3, 4, 5]
iex(4)> v(1) ++ v(2)
[1, 2, 3, 4, 5, 2, 4, 6, 8, 10]
```

If you find this helpful, please recommend it so, others can benefit!

![img](https://cdn-images-1.medium.com/max/720/0*CVcKk38plDvS8stw.png)

Google Image Share

Sharing is Caring !!

Happy coding !!
