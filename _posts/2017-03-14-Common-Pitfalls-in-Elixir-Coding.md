---
layout: post
title: Common Pitfalls in Elixir Coding
subtitle: To fall is to raise
---
![img](https://cdn-images-1.medium.com/max/720/0*gvT1CcmMLv0DYI9R.jpg)

This article is all about common mistakes of newbies in Elixir. Even experts in Elixir some times fall under this category. From the experience, I am revealing some sort of things to be careful while coding. This doesn’t mean that you fall in this category. I am just saying there are pits and you do not fall into it. Don’t worry I am just kidding. Lets jump into virtual pitfalls…

### 1. Functions with |> operator and parameters

This is the most common pitfall and one has to be more careful while piping the functions with parameters which leads to unusual results. Lets check the following example.

```elixir
String.graphemes "hello" |> length 
```

When you execute above line, you will get an `ArgumentError` . This is because the above lines are grouped as follows

```elixir
quote(do: String.graphemes "hello" |> length) |> Macro.to_string
"String.graphemes(\"hello\" |> length)"
```

So, you can use `quote` and `Macro.to_string` to see how your coding lines are grouped together. We think in the direction of output of `String.graphemes` is passed as input to the `length` function. But it does in other direction of taking the binary `"hello"` as input to the `length` function which gives you error as `list` is expected.

To avoid this we have to use the parenthesis for wrapping the parameters inside them for safety evaluation.

```elixir
String.graphemes("hello") |> length
5
```

### 2. Lists Concatenation / improper lists

Elixir provides `++` operator to add elements to the list. Basically it is used for concatenation of two lists, but we always forget to wrap the elements inside `[]`

```elixir
value = 99
list = [1,2,3,4,5]
list ++ value
```

We think that the result of `list ++ value` would be `[1,2,3,4,5,99]` but in general it will be `[1,2,3,4,5|99]` . This is a improper list. You cannot use `length` function over. In proper list, when you iterate over the list, the tail would be `[]` empty list. This is different with the improper list.

So, to overcome this we have to wrap the value inside the list as
 `list ++ [value]`

Read more about improper list [HERE](https://elixirforum.com/t/what-can-you-do-with-improper-lists/610).

### 3. Strings with charlists

In Elixir `"hello"` is not equals to `'hello'` . When you use the `""` it is considered to be `binary` in our language a string and when you use `''` , it is a list of characters. This is most common pitfall for newbies. Once if you come to a clear idea on `binary` and `charlists` , nothing to fear anymore.

```elixir
iex> is_binary 'hello'
false
iex> is_list 'hello'
true
iex> is_binary "hello"
true
iex> length 'hello'
5
iex> length "hello"
** (ArgumentError) argument error
    :erlang.length("hello")

```

### 4. Anonymous function calls and Piping

These are also called as lambda functions which are very handy in **data comprehensions. **The calling of these functions is little different from regular calling.

```elixir
square = & &1 * &1
```

we can write same thing as

```elixir
square = fn(x)-> x * x end
```

When you try to access them as `square(5)` you will get a compile error saying `undefined function square/1` . You suppose to call the lambda functions as `square.(3)` . This is just to differ from the normal functions defined inside the module.

This is very common mistake of newbies in elixir. One has to be very clear with such things. How ever, one can overcome this mistake by using `_` in the function name to differ them from the normal functions like `_square` . So by looking at the name you come to know it as a lambda function.

You can use your own valid sign to differ your lambda functions from the normal functions.

Piping the anonymous functions is also different. You have to do as following

```elixir
5
|> square.()
```

```
or
```

```elixir
5
|> (&(&1*&1)).()
```

### 5. Boolean value as False which is not false.

In Elixir `False` is not `false` . In Elixir only `false` and `nil` are considered to be `false` . The rest all evaluates to `true` . In `C Programming` even `0` are considered as `false` . But in Elixir they are not. In Elixir anything starts with uppercase is also an atom. So, `False` evaluates to `true` and surprise us when we think it is false.

So, It is very common to make a mistake with `False` . Always avoid something with capital letters until or unless it is compulsory.

```elixir
iex(1)> logic=False
False
iex(2)> if logic do
...(2)> " I am true"
...(2)> else
...(2)> "I am false"
...(2)> end
" I am true"
iex(3)>
```

### 6. Guard clause Failures

In elixir we can use the guard clauses in the function definitions by using `when` . However, if the guard clause has errors, it won’t leak out. It simply fails the guard clause. It will not raise any exception or run time error and looks for another matching definition by considering the previous guard as fail. Lets see the following example

```elixir
iex> length(15)
** (ArgumentError) argument error
    :erlang.length(15)
iex> case 15 do
...>   x when length(x) -> "Never match"
...>   x -> "value:  #{x}"
...> end
"value: 15"
```

In the above lines of code, when you try to find the `length` of an integer, It raised an error. But when you used same statement inside the guards it just skipped that definition considering the guard failure. This is very common for newbies making mistake in guard clauses.

### 7. Keywords order in Function call matters.

In Elixir, the order of passing the keywords as a function parameters really matters. Consider the following lines of code.

```elixir
defmodule User do
 def user name: name,age: age do
 "name: #{name}-- age: #{age}"
 end
end
```

Above lines of code defines a module `User` with function `user/1` . While defining the function we asked for `name` to be first followed by `age` . Lets check the different ways of calling the above function.

```elixir
iex> User.user age: 12,name: "hello"
** (FunctionClauseError) no function clause matching in User.user/1
    iex:2: User.user([age: 12, name: "hello"])
```

When you change the order of parameters, you got the no function error raising `FunctionClauseError` . This is because of Elixir functions are defined along with arity. So, pattern is not matched here.

```elixir
iex> User.user name: "hello",age: 12
"name: hello-- age: 12"
```

So, 
`def user name: name,age: age` 
is not equals to 
`def user age: age, name: name`

#### Solution

If you want keywords to be able to passed in any order, you should accept any list in your function head, and then use `Keyword.get` inside the function instead:

```elixir
defmodule User do
   def user(options) do
     name = Keyword.get(options, :user)
     age = Keyword.get(options, :age)
     "name: #{name}-- age: #{age}"
   end
end
```

This also allows you to specify useful default values for keywords that are not included when the function is called.

This advice is from [Wiebe-Marten/Qqwy](https://medium.com/@W_Mcode)

### 8. Retrieving values from Map

In Elixir if you define the map as `map = %{name: "hello"}` , you can access the map for taking out the `name` in two ways.

One is using `.` period like `map.name`. Beginners in Elixir will always try to access map with `.` period. It is **simple** and **dangerous** too because, it raises an error when you try to extract a `key` which is not defined in map. Look at the following lines of code.

```elixir
iex(1)> map = %{name: "hello"}
%{name: "hello"}
iex(2)> map.name
"hello"
iex(3)> map.age
** (KeyError) key :age not found in: %{name: "hello"}
    
iex(3)> map["name"]
nil
iex(4)> map[:name] 
"hello"
iex(5)> map[:age] 
nil

```

In the above lines of code, we are defining a `map` with `name` atom as a key. At the line of execution `iex(3)` , we are trying to take out the value of `age`using the `.` period, but it raised an error because, the key is not present.

In the same style, when you try to access the map with access operator `[]` , it gives you the `nil` value when the key is not present instead of raising an error. So, it is very handy when you access the map with `[:key]` or `["key"]`To use like `map["name"]` , the keys in map should be `binary` as follows

```elixir
map = %{"name" => "hello"}
```

This is why when you try with `key` as binary like `map["name"]` , it just returned `nil` because our `map` is defined with key as `atom` . So if you try with `map[:name]` in gives you the value of `name` .

**Note:** When you define a map with keys as binary, you cannot access them with `.` period. It is only accessible when the keys are atoms.

### 9.Pattern matching limitation

Pattern matching is highly useful and powerful too. How ever you cannot make the functions calls on the left side of the pattern match. Look at the following lines of code.

```elixir
iex(2)> 3 = length [1,2,3]
3
iex(3)> length [1,2,3] = 3
** (MatchError) no match of right hand side value: 3
```

In the above lines of code when I try to call the function on the left side to match right side, it raised an error.

Similarly for map datatypes too.

When you try `%{}=%{name: "hello"}` , this works fine. But, when you reverse the sides like `%{name: "hello"}=%{}` , this gives you the error. Try and see

```elixir
iex(3)> %{}=%{name: "hello"}
%{name: "hello"}
```

```elixir
iex(4)> %{name: "hello"}=%{}
** (MatchError) no match of right hand side value: %{}
```

`%{name: "hello"}=%{name: "hello",age: 23}` it is valid.

`%{name: "hello",age: 23}=%{name: "hello"}` it is invalid.

So, this is very common to miss understand above terms in pattern matching.

### 10 Guard clauses using `&&` ,`||,` `!` are invalid

We can check the multiple conditions in the guard clauses. In Elixir you cannot use either `&&` or`||` or `!` . But coming to guard clauses this is bit different. You have to use `and` , `or`, `not` for `&&` , `or` and `!` respectively. Look at the following example.

```elixir
iex(13)> x=25
25
iex(14)> case x do 
...(14)> y when y>12 && y<30 -> y
...(14)> y -> "nomatch"
...(14)> end
** (CompileError) iex:15: invalid expression in guard
    (elixir) expanding macro: Kernel.&&/2
             iex:15: (file)
```

In the above lines of code, we are attempting to check multiple conditions in the guard clause with code `when y>12 && y<30` . Even though they look similar, they are not allowed in the guard clauses. If you run the same lines of code by changing `&&` to `and`, it works fine. Lets check the working example.

```elixir
iex(14)> case x do               
...(14)> y when y>12 and y<30 -> y
...(14)> y -> "nomatch"           
...(14)> end                      
25
```

So, there is a chance to make mistake by using `&&`, `or`, `!` in guard clause conditions as they work fine in regular condition evaluations . Check the following example.

```elixir
iex(15)> x=25
iex(16)> if x>12 && x<30 do x else :invalid end
25
```

```elixir
iex(17) x=1
iex(18)> if x>12 && x<30 do x else :invalid end
:invalid
```

Did you observe that`&&` worked in regular conditions but not allowed in guard clause conditions.

If you find any other `pitfall` in elixir coding which I did not mention, please share and save others from falling into it.

Love to hear your things. ❤ ❤ ^-^

If you find this helpful, please recommend it so, others can benefit!
Sharing is Caring…

![img](https://cdn-images-1.medium.com/max/720/0*f6u0dRVA2PCnQr7E.jpg)

Google image Sharing is Caring
Love to recommend

Happy Coding…
