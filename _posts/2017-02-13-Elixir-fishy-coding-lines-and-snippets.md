---
layout: post
title: Elixir fishy coding lines and snippets
---

Catch them with code

![img](https://cdn-images-1.medium.com/max/533/1*n0FFA586j4A9zH6kwUnPNQ.png)

**Use code nets to catch fishy lines of code in elixir.**

Don’t miss understand the word **fishy . **You can use that word as `**suspect**` or`**shady**` . I just want to expose those **shady **things to light for beginners.

#### 1. Finding the Terminal Width and height

it works on resizing your terminal also

#### 2. Underscores in Number representation

You can specify **underscore** decimal numbers may contain underscores 
**(_ ) **these are often used to separate groups of three digits when writing large numbers, so one million could be written 1_000_000 (or perhaps 100_0000 in China and Japan or 10_00_000 in India).
You can also perform any arithmetic operations as they are just numbers.

```
number = 1_000_777
number + 7_000
```

#### 3. Function Execution Time

#### 4. Empty Map pattern matching

```
%{} = %{name: "blackode"} # pattern matches
%{name: "blackode"} = %{} # pattern do not match here.
%{name: "blackode"} = %{name: "blackode",age: "hello"}#match pattern
```

#### 5.Infinite Streams

```
(1..10_000_000)|> Enum.map(&(&1))|> Enum.take(5) 
```

The above line takes almost **8 seconds**

```
(1..10_000_000)|> Stream.map(&(&1))|> Enum.take(5)
```

See the magic after running those two lines.

#### 6. Character Lists [ vs ] Numbers

```
[67,65,84]
'CAT'            # Out put of above list.
```

In the list if all the numbers are of **printable** characters then they are not more considered as ****number**** unlike we expected. So, be careful with the lists.

#### 7. Single line Module and Definitions — No more End

There is no need to specify the **end** for every time you write the module.

```
defmodule M, do: (
                   def hi, do: (IO.puts "Hi!")
                   def hello, do: (IO.puts "Hello!!") 
                   def bye, do: (IO.puts "Bye!!")
                  )
```

You can use just the **braces** for separating the multiple lines of code in do block.

#### 8. #iex:break

There will be situation that when you type mistake that you no where go to back or end the that line of code.

```
iex(1)> ["ab 
...(1)> c" 
...(1)> " 
...(1)> ] 
...(1)> #iex:break ** (TokenMissingError) iex:1: incomplete expressionThis helps you to come out of the loop.
```

#### 9. Shell Variables in IEx

If you want to check the all the variables which are defined in the current `iex` shell just do `binding()`

```
binding()
```

This returns the list of key value pairs with variable_name as key and its bound value.

#### 10. Chained anonymous functions

```
wish = fn -> fn -> fn -> IO.write "this is chain" end end end
wish.().().()   # usage
```

#### 11. Multiple or conditions — in

```
number = fn(num) when num in [1,2,3,4,5] -> IO.write num end
```

This code works only if you pass `num` value `1<=num<=5` other wise it returns `non match function class error` .

As the medium is meant for medium articles which are to be medium in size. So I am keeping this short and sweet and will meet on next part which will cover more topics…

Just got a thought of talking on this article and I want to show the code snippets by executing my self. So, I made a YouTube video . The audio is not good, please bear with me.. soon I will improve the audio quality.

If you like then hit ❤

![img](https://cdn-images-1.medium.com/max/533/1*oMFRh91W4IFF4SR56i3W6g.gif)

to be continued….
