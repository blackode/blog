---
layout: post
title: Mix Task Creation in Elixir Project
---
Tasks add extra cheese to your code.
![img](https://cdn-images-1.medium.com/max/720/1*o8jQOVCEisFwVb_zV6B5QQ.png)Grow with confidence in elixir

Here we fly in Elixir space to create mix tasks in mix project including with docs and short docs.

### What is our Task?

*Our Task is to list out the functions in the module you passed as parameter to the task.*

So, when you execute `mix my_tasks.functions ModuleName` , it has to list out all functions defined in `ModuleName`.

Before diving into coding we have to understand some terms which plays a key role in **task** creation.

### Mix.Task

This is the simple module responsible for creating and controlling tasks.

To behave our module as a task we have to follow some naming conventions.

Our module name should start with `Mix.Tasks` for an example, if our task name is `functions` , then module name is `Mix.Tasks.Functions` and we have to use the task behavior module `use Mix.Task` a first line of code in our module .

### run/1

This acts as a callback function to our task. All your task logic resides inside this function. This function takes list of binary `[binary]` values and returns `any`

```
run([binary]) :: any
```

### @shortdoc

This module attribute makes the task public with a short description that appears on `mix help` . It means when you execute a command `mix help` you can see all tasks available in the scope along with single line description about the task.

In addition we have two more attributes. `@recursive` used in umbrella projects to keep the task running continuously and `@referred_cli_env` which recommends environment to run task. We don’t need them now.

### @moduledoc

This module attribute is used to describe all about task. So, when some one wants help about your task, The binary of this attribute is shown as a response to a request `mix help my_tasks.functions` .

### Lets Create Task

Now it is time to create our task which list out all the functions defined inside the module passed as parameter.

Lets do this.

#### Create a new project

```
mix new my_tasks
```

#### Create Task directory

After creation of the project go to your `lib` directory and create one nested directory as `mix/tasks` . This is where all our tasks reside.

```
$ cd path/to/your/project
$ cd lib
$ mkdir -p mix/tasks
```

#### Testing Module Files

Now we will create two files in `lib` directory one `hello.ex` and other one is `hi.ex` with some random functions. We are going to list out those module functions using our task as proof of working.

This is how our files looks.

#### hello.ex

hello.ex

#### hi.ex

hi.ex

These files do nothing with the task literally. They are just for showing task demo.

#### Creating Task file

Now create a file `functions.ex` in the directory of tasks `/lib/mix/tasks` and add the following content.

Task file with out shortdoc

Every module defined in the project has its own `__info__` function. You can get all the information about that module. So, I am using that function by passing an atom `:functions` to get the information about functions.

#### Clarification

So far we have created three files `lib/mix/tasks/functions.ex` which is our task file and two more files `lib/hello.ex` and `lib/hi.ex`

#### Compiling the Project

So far we have created a task but it is not available until you compile your project.

Run the following command in your project root directory.

```
mix compile
```

After successful compilation you can run your task as

```
$ mix my_tasks.functions Hello
[hello1: 0, hello2: 0, hello3: 0]
```

```
$mix my_tasks.functions Hi
[hi1: 0, hi2: 0, hi3: 0]
```

If you see above outputs in your terminal, it means you are good to go.

As a final touch, let’s add docs to our task.

Till now, we have just created a task. We have not provided any docs for the task. So, you cannot find your task shown in the list when you try to execute `mix help` or `mix help my_tasks.functions` to available your task in the list, you suppose to add some module attributes to your task file.

Lets do that now .

You have to add module attributes `@shortdoc` and `@moduledoc` in task module . Keep your description simple and smart.

After adding the module attributes our file will look like following

Task with @shortdoc and @moduledoc

![img](https://cdn-images-1.medium.com/max/720/0*SOFSiJHylCKMOb-9.)

```
mix help
```

![img](https://cdn-images-1.medium.com/max/720/1*yJg00yiAN7WbU_ZiQxe6Qw.png)

shortdoc information of task

You can see your task in the list here with `@shortdoc` information saying **#Returns functions in module**

![img](https://cdn-images-1.medium.com/max/720/0*SOFSiJHylCKMOb-9.)

```
mix help my_tasks.functions
```

![img](https://cdn-images-1.medium.com/max/720/1*Dp_-fcb3fBhjf8PsGj29-Q.png)

moduledoc information of task

Well, you successfully created your Elixir Project Tasks. Do some funny things playing with tasks. So. you will become familiar with creation of tasks.

Your working mix task !!

If you find this helpful, please recommend it so, others can benefit!

![img](https://cdn-images-1.medium.com/max/720/0*DLDJUNPOESiQDgtC.jpg)

Share if you care google image search

**Sharing is caring !! smile :-)**

Happy coding!

![img](https://cdn-images-1.medium.com/max/720/1*GRM86mUUV_TYgxmNG29SwQ.gif)

love to recommend
