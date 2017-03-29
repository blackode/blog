---
layout: post
title: Write Your First Macro — Elixir Meta Programming
---

code to code

![img](https://cdn-images-1.medium.com/max/720/0*mmDGbU9x6K9_eE4T.gif)

infinity coding skills

> **“code that writes the code “**

I love the phrase* “***code that writes the code*** “* . It is just like having third hand to the programmer, which allows him to write his own features to programming language as an extension and allowing him to write powerful libraries to his project.

**Meta programming** is a boost feature for the programmers to build his own content, literally adding an extra cheese.

#### Thanks Giving

This article is an inspiration from the creator of the Phoenix C[hrismccord](https://elixirforum.com/users/chrismccord)who wrote a book on **MetaProgramming. **After reading his book I just got thought to share what I understood after reading first two chapters of the book. The book is really great weapon to do war in MetaProgramming.

**Macros in Elixir** are the one which helps in our uphill battle of writing the code which writes the code in return.

### The Abstract Syntax Tree — AST

#### Briefly

Abstract Syntax Tree is the state of code before it converts into byte code. Elixir allows us to generate such code snippets using through `macro` AST has become more friendly to write directly in Elixir natural Syntax using `quote & unquote` .

![img](https://cdn-images-1.medium.com/max/720/0*35h3UgIsdUI8z7u5.png)

AST Elixir

### quote & unquote

The `quote` and `unquote` are our weapons given in a war of meta programming. To keep this simple, I will go with simple definitions. `quote`converts the code to AST and `unquote` resolve the expressions inside the `quote` .

```
iex> quote do: 1 + 2
{:+, [context: Elixir, import: Kernel], [1, 2]} # AST Structures
iex> quote do: unquote 1+2
3
```

#### Skeleton of Macro

```
defmacro macro_name(expression, do: block)
```

### What we will build here?

#### Adding a while Loop to Elixir

Today we are going add while loop, as Elixir is not provided, which is one of the richest features of C programmers, who had fallen in love already with its shape and functionality.

**An Example of what we build here**

```
while true do
  receive do
   {message, pid} -> IO.puts "Got #{message}"
  after 2000 -> break
  end
end
```

According to above logic, it keep on receiving the messages. If the process wait for 2000 milliseconds then it should break. No more receiving. Here we need to provide with features like infinity loop and break options whenever we want from the loop. Lets do that ..

#### Infinity Loop

Elixir do not have an infinity loop structures. We have to build that from the scratch. This is not as complicated as you are thinking. We use the `Stream.cycle` function to generate infinity stream and we use `for`comprehension to make our job done.

```
for _ <- Strem.cycle([:fine]) do
 # generate infinity stream of :fine
 ....
end
```

while loop with out break

Here we are doing the pattern matching directly . Lets match that to our code.

`while(x>y), do: IO.puts "hello"` if this is what you write in your original code then , macro will match like this `condition_expression = x>y` `do_block = IO.puts "hello"` everything inside the do block. This is how the pattern match goes.

Since we have to provide the AST we are using the `quote` to do so. Whenever you do `unquote` means that resolves to `value` of the variable.

#### Break

So far we did not write the `break` part lets do that.

while loop along with break statement

What we have done so far here is we wrapped the code `for loop` inside the `try catch block` in line number 4 . In the `else` part we are explicitly throwing `exception` `:break` at line number `9` and catching the same exception at line number `13` . So if the condition evaluates into `true` it will do the `do_block` stuff else throw `break` .

Is this hard to write the break ? I hope not. It is simple logic.

Lets do with more friendly approach here. We keep throwing the `:break`inside a function with name `break` . So our code will look like the following

while loop with break definiton

Here we just added a new **break** definition at line number `17` and we changed the statement inside the `else` block as `WhileLoop.break` . It does the same thing but little flexible. Lets check this in `iex`

Load the module `WhileLoop.exs` and `import WhileLoop` so , our macro load inside the `iex`

### Examples

```
iex> c "WhileLoop.exs"
[WhileLoop]
iex> import WhileLoop
iex> run_loop = fn ->
     pid = spawn(fn -> :timer.sleep(4000) end)
     while Process.alive?(pid) do
     IO.puts "Tick"
     :timer.sleep 1000
     end
     end
iex> run_loop.()
Tick
Tick
Tick
Tick
```

Here we are creating an anonymous function `run_loop` . Coming to inside of the definition `pid = spawn(fn -> :timer.sleep(4000) end)` this statement creates a process that sleeps for **four** seconds. As the `spawn` is `asynchronous`so it allows you to execute next line in sequence. As the process is sleeping for **four** seconds, `pid` will be `alive` for **four** seconds. So we are using the `while Process.alive?(pid)` as condition which will be **true** for **four** seconds then the condition become **false**. Inside the `while` loop we are printing the `Tick` to console and waiting for **one** second. Since while loop is **true** for **four** seconds it should print the `Tick` four times and `:ok` If everything goes well.

![img](https://cdn-images-1.medium.com/max/720/1*N_WLbBgLoe91jE5qKFArEw.gif)

iex Gif Examples

#### Infinity Loop Example

```
pid = spawn fn ->
  while true do
   receive do
    :stop ->
     IO.puts "Loop break"
     break
    message ->
     IO.puts "#{inspect message}"
   end
  end
end
```

When you run this in `iex` it keep on waiting for messages infinitely unless you send message as `:stop`

This how we send the messages to the `pid`

```
iex> send pid,:hello
:hello
iex> send pid, :blackode
:blackode
iex> send pid, :stop
"Loop break"
:stop
iex> Process.is_alive? pid
false
```

![img](https://cdn-images-1.medium.com/max/720/1*ZXTqJ29UskkUSrRLiS7bTQ.gif)

Infinity loop exmaples

Did you check that once you send the `:stop` message , The process dies.

**Tribute:** 
Examples are used from the book. I really recommend this Meta Programming. You can buy [here](http://www.goodreads.com/book/show/24791466-metaprogramming-elixir).

Now, it is time to create your own macros.

**Note:** Meta Programming is not limited to creating loops only. It is just up to creative level.

Thanks for your time. I Hope this helped you. If you like hit ❤

Happy coding !!

![img](https://cdn-images-1.medium.com/max/720/1*oMFRh91W4IFF4SR56i3W6g.gif)

love to recommend
