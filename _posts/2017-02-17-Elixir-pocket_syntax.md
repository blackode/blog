---
layout: post
title:  Elixir Pocket Syntax
date:   2017-02-17 14:57:05 +0530
image: elixirpocketimage.jpg
tags: [html,info,code,love]
category: Elixir
---


Uncommon Logical stuff of Elixir modules,definitions and some coding snippets that makes our life easy and fast. We go with very basic to the different approach.      

### 1. Creating Private Functions

#### Code
```elixir
defp defname(arguments) do
  # your definition goes here 
end
```

#### Example

```elixir
defmodule MyModule do
  @doc "This is the public function can be called out side"
  def public_function do
    IO.puts "I am a public function"
  end

  defp private_function do
    IO.puts "I am private function"
  end
end
```
#### Description   
Here `defp` stands for the private function which means you cannot call that function out side module by importing like this `MyModule.private_function`.This can be used only inside the another functions in the module it has been defined in. In our example it can be called inside the `public_function`.

### 2. Pipe Operations `|>'

#### Code
```elixir
id |> getName() |> toUpperCase()
```

#### Example
```elixir
defmodule MyModule do
  def function do
    admin = getNameById(id) |> toUpperCase |> isAdmin
    IO.puts admin
  end
end
```

#### Description   

Here the out put of the `getNameById` is passed as the first parameter to the `toUpperCase` in which it returns name as all capitals letters and that name is passed to the `isAdmin` function to check that the person is admin or not finally a `boolean` `true` or `false` is returned.

The only thing `|>` does, is to take the return value of the left-hand-side and insert it as first argument in the function on the right-hand-side. This allows you to write above example, instead of writing:

```elixir
isAdmin(toUpperCase(getNameById(id)))
```
As above example reads left-to-right (or top-to-bottom), this is more natural to read.

In a typical "object-oriented" language, the code to achieve a similar aim would look like this:
```js
getNameById(id).toUpperCase().isAdmin()
```
So, you can think of `|>` as being the Elixir version of `.` (though without the class lookup).



### 3. Import specific functions

#### Code

```elixir
import :math, only: [sqrt: 1]
import :math, except: [sin: 1, cos: 1]
```

#### Example

```elixir
defmodule MyModule do
  import :math, only: [sqrt: 1]

  def function do
    sqrt 4
  end
end
```

#### Description   
In the above example only the `sqrt` function is loaded is imported to the the module. This means that this _specific_ function can be accessed like `sqrt(4)` (or, without the brackets, `sqrt 4`). All other functions, even ones in the `:math` module still need to be accessed by prefixing the full module name, such as `:math.sin(2)`.      

### 4. Functions with Default Values

#### Code

```elixir
def fall_velocity(distance, gravity \\ 9.8) do
  # code
end
```

#### Example

```elixir
import :math, only: [sqrt: 1]
defmodule MyModule do
  def fall_velocity(distance, gravity \\ 9.8) do
    velocity=sqrt 2*gravity*distance
    IO.puts "the falling velocity is #{velocity}"
  end
end
```

#### Description

By adding default arguments, Elixir underwater adds multiple versions of the function for you, with different arities (taking a different amount of parameters). In above example, a `fall_velocity/1` and a `fall_velocity/2` is defined. The underwater implementation of `fall_velocity/1` is as follows:
```elixir
def fall_velocity(distance), do: fall_velocity(distance, 9.8)
```
So: the shorter version(s) of a function will call the longest version with the later argument(s) filled in with the specified default value(s).

#### Defaults With Pattern-Matching

If your function has multiple clauses, it must have a function head (like a clause without a body), and the default(s) must be defined _only there_.

#### Example

```elixir
defmodule MyModule do
  def greet(name, greeting \\ "Hello")

  def greet("", _) do
    IO.puts "Please tell me your name."
  end

  def greet(name, greeting) do
    IO.puts "#{greeting}, #ecto::tag name}"
  end
end
```

### 5. Documentation and specifications

#### Code

```elixir
defmodule Mymdule do
  @moduledoc """
  Explanation about the module
  """

  @vsn 0.1 # module version

  @doc """
    your documentation here can have multiple 
    lines of text 
    line1
    line2 
    etc
  """
  @spec function_name(number()) :: number()
  def function_name do
    # code
  end
end
```

#### How to use?

```elixir
h(Modulename.function_name)
s(Modulename.function_name)
```

#### Description   

Here `h()` stands for help when you type like that you will be displayed with the `@doc` text and similarly when you type `s()` you will be getting the function specifications it explains what parameters has to pass and what it returns when you call or simply what has to give to the function and what it will give in return.          

### 6. Using Erlang From Elixir

#### Code
```elixir
:application.which_applications
:erlang.module_info
```

#### Description

here the `list_applications` returns the list of applications being executed in an Erlang VM .This is the way to use the function from Elixir.         
The erlang would be like this           
```elixir
application:which_applications() # moduleName:functionName() 
```

### 7. Observer GUI

#### Code

```elixir
:observer.start
```

#### Description

You will see the Graphical representation of all process and sub prcesses.

### 8. Single Line definitions

#### Code

```elixir
defmodule MyModule do
  def callme, do: IO.puts "This is the single line definition"
  # no need of end here
end

# one line with module (useful in iex):
defmodule MyModule, do: def callme, do: IO.puts "This is the single line definition"
```

#### Explanation

The `definition name` and  `do`  block are separated by the `,`and no `end` after the do statement.

### 9. Single Line if 

#### Code

```elixir
message = if condition, do: "condition returns false", else: "otherwise"
IO.puts "This prints when #{message}"
```

### 10. Aliases

You can define the aliases in two ways

#### Code

```elixir
defmodule MyModule do
  alias Geometry.Rectangle, as: Rectangle

  def my_function do
    Rectangle.area({1, 2}, {3, 4})
  end
end
#The above style is most formal one 

defmodule MyModule do
  alias Geometry.Rectangle

  def my_function do
    Rectangle.area({1, 2}, {3, 4})
  end
end
#This is the fast style of aliasing .
```

#### Explanation

This line `alias Geometry.Rectangle, as: Rectangle` of code states that  we are naming the **alias** with name `Rectangle`  You can any kinda name but adding the last  name of the module scope is too good choice. By default if you don't specify the any  value for `as:something` then it assigns the default one i.e the last scope in name here `Rectangle`

### 11. String inner inner binary representation

A common trick in **Elixir** is to concatenate the null byte `<<0>>` to a string to see its inner binary representation:

#### Code

```shell
iex> "hełło" <> <<0>>
<<104, 101, 197, 130, 197, 130, 111, 0>>
```

### 12. Specifying the binary 

#### Code

```elixir
<<variable::size>>
<<2::4, 4::5>> # this is the 4+5=9 bits binary
<<2::size(4), 4::size(4)>> 
# both are same 
```

If you do not the size of the binary you can make call some thing like this 

```elixir
<<x::4, y::binary>>
```

#### Explanation

The above code line states that first 4 bits are to stored in the `x` variable. The remaining bits we are storing as the binary blocks in the the variable `y` which means that rest of the length should multiply by the `number 8` as the `binary` means `8` bits.The size of the tail must be evenly divisible by 8.

`<<x::4, y::binary, t::5>>` This is the wrong representation, It raised the compilation error . The unsized binary should be at the last i mean tail of the string as followed `<<x::4, t::5, y::binary>>`

### 13. Importing constants to another module

```elixir
defmodule Constants do
  @moduledoc false
  # This module contains compile time constants so that they don't have to be
  # defined in multiple places, but don't also have to be built up repeatedly.

  @doc false
  defmacro __using__(_) do
    quote do
      @constant1 10000
      @constant2 20000
    end
end

```

#### Explanation

Here you need to define macro `__using__`to define the constants. In side the `module` just `use Contants`

#### Code

```elixir
defmodule MyModule do
  use Constants

  def calleme, do: IO.puts "constant 1 value = #{@constant1}"
end
```

#### Note

make sure both are at the same hierarchy . According to you above code.If you need you can change but be  sure you have to change the `use path/to/module/` also :thanks:

### 14. Updating maps

Consider that you have `Map` like the below one I have shown ...

```elixir
person = %{name: "John", age: 22, place: "York Street"}
```

Here if you  want to change the `name` field of the **person** map , you can simply do like this. 

```elixir
new_person = %{person | name: "Jopra"}
```

You see that actually It won't rewrite the person data it gives you the new copy of the person data  with the **name** field changing to our update . The **person** data is immutable here. 
You can also update multiple keys here unlike the firsr example I have shown on this snippet.
```elixir
new_person = %{person | name: "Jopra", age: "secret"}
```
### 15 hd and tl  list function

```elixir
list = [1,2,3,4,5]
hd list # returns the first element in the list    1
tl list # returns the tail list i.e only the tail part of the list  [2,3,4,5]
```
### 16 Code Grouping Syntax

Some times we write nasty coding lines, we except something but the the line is grouped in another format and that surprises with different outputs. So you can use `quote` and `Macro.to_string` to see how your line of code looks .

```
iex(1)> quote(do: foo 1 |> bar()) |> Macro.to_string |> IO.puts
foo(1 |> bar())
:ok
iex(2)> quote(do: foo(1) |> bar()) |> Macro.to_string |> IO.puts
foo(1) |> bar()
:ok
```

