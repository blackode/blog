---
layout: post
title: When and Where to use cast,call & info messages in Elixir & Erlang — GenServer
---

![img](https://cdn-images-1.medium.com/max/720/1*6EEemxAhbyEn56M_MOv5nw.png)

Image designed at [https://canva.com](https://canva.com/)

> Getting into new habit of programming is always a challenging task.

Learning through live examples will add some extra taste to your interest which helps you to do things better, faster and make you to climb uphill without any hurdles.

**Note: ***Processes are the same in both Erlang and Elixir, so everything below is equally applicable to both languages.*

This article comprises of clean and clear explanation with simple words which enrich your knowledge in understanding `GenServer` **callbacks**. Let me talk **theory** part first next we will dive in to practical demonstration.

### Theory

#### — cast

*handle_cast( message, state ) :: { :noreply, state }*

Handle cast is used in the situation where you don’ t care or expect reply from the server. 
Suppose if you want to write a log server which will log details of the request whenever you make an unauthorized request. So, in that situation you cannot break your flow of execution waiting for reply whether the server has written the log or not. If you do so that will be the weirdest part of your code. This is where the **cast **come into play. 
All the log mechanism should run in backside. Hence you should use the cast to make **asynchronous** requests which do not block execution flow.

#### **— call**

*handle_call( message, _from, state ) :: ( :reply, return_value, state )*

This is just opposite to the **cast** which I mentioned above. This block the flow of execution till it return a value. The best example to consider the situation to ask yourself is **sign in and sign up **mechanisms. 
You cannot simply move to next without getting the proper response from the server based on your sign in request with out validation. Here you supposed to wait for the authentication from the server. This is where the **call **come into handy. Based on the server response you can redirect the flow.

**— info***handle_info(message,state) :: { :noreply, state }*

`handle_info/2` must be used for all other messages a server may receive that are not sent via `GenServer.call/2` or `GenServer.cast/2`, including regular messages sent with `Kernel.send/2`. This is highly useful in the situations like monitoring the process using `Process.monitor/1 `or if you want to schedule the work to repeat after certain interval of time using `Process.send_after/4`

So, whenever you make a request like `Kernel.send(pid,message)` , the `handle_info/2` callback is triggered in the registered process of `pid` . This `handle_info` does the asynchronous requests.

### Lab Practicals

Here I will keep explaining the simple file which contains all three callback functions which I defined with purpose. You can just copy-paste the code into your `iex` or save that into file and load in `iex`

Save the file with name `hello_server.ex` as convention. 
Load it in `iex` and start sending the messages to server.

```
$ iex hello_server.ex
```

#### Sending Messages to GenServer

**init**The `GenServer.startlink`A will call `init`definition here we are sending initial value `1` to initiate the server state.

```
iex> {:ok,pid} = GenServer.start_link,HelloServer,1,[]
{:ok, #PID<0.88.0>}
```

**call**A`call` is synchronous. It’ll return the value upon execution.The result can be assigned to a variable since the `call` returns the value. It blocks the calling process until the value is returned.

```
iex> GenServer.call pid,{:add,200}
"200 add"
```

```
iex> GenServer.call pid,:get
201
```

Here I purposely wrote two **call** functions to understand the code flow here. At the initial call with message `{:add,200}` it returns the string immediately, where we cannot observe whether the flow is blocked or not.
While coming to the second call with message `:get` I put `:timer.sleep 2000`purposely to understand that flow is blocked. You can observe the changes in your `iex` shell.

**cast**A cast is a Asynchronous. It won’t block the Execution flow. The `virtual-machine` says `:ok` I received a message and will process that later.

```
iex> GenServe.cast pid,:reset
:ok
iex>
"value has been reset" # it is printed after 2 seconds.
```

Once you make call with message `:reset` immediately the virtual machine will say `:ok` . Here I purposely added `:timer.sleep 2000` which sleeps for 2 seconds, but it won’t block the flow unlike the previous `handle_call` one did. It will wait in background. Mean while you will get the `iex` shell back so you can fire other commands too. So after **2 seconds** again it will print the string **“value has been reset”.**

**info**All messages that are sent to a process directly (instead of via **call** or **cast**) will end up here. It is *asynchronous* like **cast** and does not block the calling process.

```
iex> send pid,:work
This message prints every after 2 seconds
:work
This message prints every after 2 seconds
This message prints every after 2 seconds
         ... 
```

Unlike the cast and call you have to call this with `Kernel.send` definition, which will trigger the respective `handle_info` matching with the `message`passed. Here our message is `:work` .

Inside the `handle_info` we are calling a private function called `schedule_work` which executes `Process.send_after(self(),:work,2*1000)`which again fires the `handle_info` after every 2 seconds. `2*1000` . So the `handle_info` callback is triggered when the external requests are made unlike calling with `GenServer.call` or `GenServer.cast` . `Process.monitor` also triggers the `handle_info` definition.

Just execute the `Process.exit(pid,:kill)` to come out of printing continuously.

Happy coding !

If you like this ❤

![img](https://cdn-images-1.medium.com/max/720/1*oMFRh91W4IFF4SR56i3W6g.gif)

Share and Care
