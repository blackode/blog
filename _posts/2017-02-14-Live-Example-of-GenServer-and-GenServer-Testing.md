---
layout: post
title: Live Example of GenServer and GenServer Testing
---

![img](https://cdn-images-1.medium.com/max/533/1*7pQEtSNfuTz8ARYowi3s4w.png)

GenServer

> The **GenServer** is really a **night mare** for the beginner but if you learn how to ride with it , you will become **KNIGHT** in Elixir Programming.

In this article I will try my best to make you understand each line of code which will make you to understand `GenServer` in return.

### What we do Here?

We are going to implement the following actions in the `GenServer`

#### NOTE:

Here we assume There Exits only single user for our convenience.

1. User start the server and receives the `pid` of the server by initiating server state with some random password.
2. An `unlock` function to unlock the given password. If the password given is same as the one given at the time of the execution means the server has to return `:ok` or else it will written the `{:error "wrongpassword"}`
3. A `reset` function to reset the given password. If the `old_password` is correct , return `:ok` else `{:error,"wrongpassword"}`

These are the major tasks that we are going to implement from the scratch by building the `mix project` with title `PasswordLock` 
In addition you will learn How to `test` the `GenServer` by writing your own **logger **function that too using `GenServer` .

Hope that you all know how to create a project in Elixir using `mix` tool. If not just [click](https://medium.com/blackode/how-to-write-elixir-packages-and-publish-to-hex-pm-8723038ebe76) here.

Here we go with step by step process of development people who are more familiar with elixir might feel bore here — sorry for that. But my only intention is to bring the beginners to use `GenServer` with no excuses.

#### **Creating the mix project.**

`mix new password_lock`

This will give you nice folder structure. I am not going to explain what these files do here as I already explained in one of articles as I mentioned you earlier.

#### Writing the GenServer for password_lock

If you open your `lib` folder you will see a file with name `password_lock.ex`with some initial lines of coding stuff. Remove every line from the file and we will write down the module from the first line, so you will hold the full control of the code what you are writing.

> If you write your own lines of code with your own mind and fingers, I assure you will be the royal king of your code dynasty.

#### Coding Begins…

Hope that you all know how to write a module … The module skeleton will look like this ..

```
defmodule PasswordLock do
 #your creativity
   ...
end
```

That is how the module looks. How ever the simple thing we need to do here is .. in order to act your `module` as `GenServer` you should write a simple line of code which will give you high functionalities in return.

```
use GenServer
```

Add that line to your module as first line of code in the module `password_lock.ex` and do not forget that `S` ins `GenServer` is **CAPITAL.** 
I am just telling you this because I usually forget it while typing it fast. I don’t want you to fall in my category.

```
defmodule PasswordLock do
  use GenServer                # --> add this line to your module
```

```
 #your creativity
   ...
end
```

> Learning piece by piece will bring you piece of mind in coding.

What I am trying to say is .. if you eat a cake piece by piece you will feel the taste and enjoy while doing that.. however just remember how it looks when you hold a whole big cake with your two hands putting your mouth inside it to eat . That is something really unimaginable.

#### What << **use GenServer** >> do ?

That is really good question to ask. `GenServer` is a behavior module that will force you to implement certain definitions inside your module who ever `use` the `GenServer` module. That means you have to add few functions or definitions whatever you call which will act as your server callback functions to the client. It will just abstract the client server relation. It means your module should hold the standard set of definitions/functions.

#### Client API

Now we are going to write the client API. So the client is able to start our server and call certain functions which will call the server callback functions in return.

#### — Starting the Server

Here we provide `start_link` function to the client to start the `GenServer` 
So add the following lines of code to your module `password_lock.ex` after the line `use GenServer`

Add above lines to your module `password_lock.ex`

`GenServer.start_link(__MODULE__,password,[])` This returns a tuple as `{:ok,pid}` This `pid` helps the client to make requests according to his requirements. The another thing is it will call the `init` function from the server side which we will define once after the client is finished. The client should pass one parameter here **password** to initialize the server state with that password.

#### — Unlock / Reset definitions

In this section we will provide client to call certain functions with parameters which will make server requests about unlocking and resetting the password. The lines of code will like following

Add above lines of code to the file `lib/password_lock.ex` 
 — **unlock** 
This unlock definitions takes two parameters `server_pid` which is used to make the server callbacks and `password` to unlock the password which client initialized at the time of starting the server in the `start_link`function which we have defined at the top

— **reset**
This reset definition also takes two parameters `server_pid` and the second parameter is a `tuple` of old and new passwords .

**Code Observation**

If you closely observe the code you will find two lines with `GenServer.call`this actually calling the server callback function which we will define them in server API section. To make server requests we require `pid` .

`GenServer.call(server_pid,{:unlock,password})` 
This is will make a server request to `pid` passed as parameter by client. Here we are saving `pid` in a variable `server_pid` and `{:unlock,password}`here we are pattern matching the message send by the client to server to make request. The password is saved in the `password` variable. At server side it will execute the callback which matches the pattern `{:unlock, password}` . Similarly the call in reset definition also does same thing..
`GenServer.call(server_pid, {:reset,{old_password,new_password}})`

#### Server API

— **init**This `init` definition will let you initiate the `state` of your `GenServer`module which will be passed as a last parameter in the every callback definition in the server. This function is triggered when you make a call to `GenServer.start_link` function . The lines of code will look as following..,

Your `init` function should return a tuple `{:ok,state}` . In the state you can store whatever you want. Here I am saving **state** as **list** with the given **password** by the client `[password]` Add those lines to the module `/lib/password_lock.ex`

#### Server Callbacks

**Code Observation**

The server callback functions will look like the above. The callbacks come in three flavors. They are, as follows; `handle_call` `handle_cast``handle_info` .
Here we talk about onlu `handle_call` and `handle_cast`

I want keep this explanation about callbacks so simple. So I will just give you the complete information about them with single sentence. Hope you understand..
`handle_call` 
This is synchronous call which means you have wait after calling this till it returns the state.

`handle_cast` 
This is asynchronous call which means you no need to wait for the result of the call, you can proceed with the next line of code execution unlike above one.

`handle_info` 
All messages that are sent to a process directly (instead of via call or cast) will end up here. It is asynchronous like cast and does not block the calling process.

If you want to learn in more detail with live examples [click-here](http://hashnuke.com/2016/02/07/cast-call-and-info-elixir.html) This one so simple.

The messages which the client passed with `pid` — if any one of the callbacks match to the message passed by the client gets executed. Here in above lines of code we are defining the two callbacks as both are the synchronous requests.

If you clearly observe the above code you will find two extra parameters here.. one is `_from` and another is `passwords` . I want to talk about these two kids. `_from` that itself tells that where this message is coming from and the last one is our current state of the `GenServer` if that is updated in any of the calls gets affected in others call too. That means .. at the time of initiation we have set the given password as the initial state of our `GenServer` That is passed to every server callback.

Inside of the call back function we are writing the code logic. Our logic simple we are checking whether the given password in inside the state or not. if true then return `:ok` else return `{:error,"wrongpassword"}` but you have to return in the form of tuple only. The tuple will look like this .

{:reply,return_value,state}

`:reply` is atom here and `return_value` means what you have to return to the client as a **response** to his **request** and the last one `state` means the state after processing the client request. In the `:unlock` we are not updating the `state` we are simply sending the `state` back. Where as coming to the `:reset` request here if `old_password` matches the password in the `state` variable **passwords** we are updating the **state** to the `new_password`here. This is how we write the **server callbacks** in the server API.

**note:** every time you make a request with wrong password we will log that to the file `/tmp/password_logs` that is why you find a line `write_to_logfile` private function. Don’t get panic we will another module which handles the log requests.

If you look at the file it will look as following one.

In the above file except the private function `write_to_logfile` and its call in the error sections of callback functions `write_to_logfile password` you have written everything . This log file will call another module which we haven’t written it. Lets do that.

### Log Server

Create another file with file name `/lib/password_logger.ex` in your project directory and add the following code..

**Code Observation**

The file nothing new to you I think . This just like another `GenServer` that do the all log actions.

But only thing you have to observe here is, in the server side we are dealing with the **asynchronous** request unlike the `password_lock.ex` which handle **synchronous** requests. That is why, it is calling `GenServer.cast` instead of `GenServer.call` unlike in the previous file.

Another thing you have to observe here is `:noreply` return statement inside the server callback. That means it no more send the return statements except updating the state. So those requests will not block the client side code execution.

**__init()**

If you look at the `init` function here it is initiated the server state with **logfile **Nothing more than that. Don’t confuse with the `state` it can be anything at any moments after processing the requests.

**__handle_cast()**

If you look at the code lines first we are changing the permissions of the user file to get write permissions. 
Then we are opening the file in the **append** mode so that it log the same file every time you make the **wrong password** requests.

**__log_incorrect()**

The client side function which calls the server callback function.

Now you are done with writing two files. One is `password_lock.ex` and `password_logger.ex`

### GenServer Testing

Testing the **GenServer** is just a little different from normal testing. Elixir uses the **ExUnit** framework .

```
use ExUnit.Case
```

Add above line to your module `/test/passsword_lock_test.ex` to behave your module as unit test case.

In your project you will find the `test` directory with one file `password_lock_test.ex` This will act as a unit test file for the file `password_lock.ex` in the `lib` directory. Similarly you can add any test file by adding the suffix `_test` to the file name in your lib directories.

To test GenServer you need to `setup` the `GenServer` with one function called `setup` itself. Our setup will look like this

You suppose to add above code in `/test/password_lock_test.ex` file. In order to test server callbacks we need the `pid` of `password_lock.ex` so we are setting here . `{:ok,server_pid} = PasswordLock.start_link("foo")`Here we are calling the `start_link` function it will return the `{:ok,pid}`here we are saving our `pid` in the variable `server_pid` .

What ever the `setup` function returns you can access those **return values** in your unit test cases. According to above setup we are returning the `{:ok,server: server_pid}` The second element here `server: server_pid`can be accessed in your unit test cases. The unit test case will look like the following code.

Add above code snippets to the file `/test/password_lock_test.ex` . If you observe the line `test "unlock success test", %{server: pid}` this describes your unit test case and second parameter here is map. We are doing the pattern matching here. So, the `pid` will be the **process id** for`password_lock.ex` `GenServer` and the code line `assert :ok == PasswordLock.unlock(pid,"foo")` we checking here the assertiveness of the function call return `PasswordLock.unlock(pid,"foo")` Like this we will write three more cases. like `unlock failure test` `reset success and failure tests` those test cases will look like the code below. The over all file will look like following..

Now you are complete with `GenServer` and `GenServer Testing`

> The more experiment you do, the more you will understand about the `GenServer`

**Usage**

Now compile the project.. navigate to your project root directory and type following command

```
iex -S mix
```

**Test**

```
mix test
```

**warning:**

If you get any error like** file not found** just change the lines in the logger as follows because if the file won’t exist and you are trying to change the file will raise an error. So, first open the file and change the permission in the logger section here.

```
{:ok, file} = File.open file_name, [:append]    
File.chmod!(file_name,0o755)
```

The Complete Project is in [Github](https://github.com/blackode/password_lock)
