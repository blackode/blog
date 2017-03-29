---
layout: post
title: How to build Elixir packages and Publish to Hex.pm ?
---

 Can I write? Yes. You can.

![img](https://cdn-images-1.medium.com/max/533/1*cQ3OD5bzoXDbtgg0hWLWNw.png)

> Creations are not ready made, they are out of thoughts….

The Elixir is the functional programming language used to build the distributed,scalable & fault tolerant system applications. Using the phoenix the elixir web framework you can develop websites. Elixir is also used in the embedded systems. This is just a small brief introduction about the elixir.

> **Creating the Elixir Package is not a rocket science.**

### Creating New Project

Open the command line and make sure that you have installed elixir with `elixir -v` and mix with `mix -v` after that you can create new project as follows.

```
mix new project_name
```

In the above command **project_name** is the name of your project. Here I create one project and will do explanation of the project in deep. My project name is `rocket` let’s do that .

```
mix new rocket
```

![img](https://cdn-images-1.medium.com/max/533/1*V_KwEKxHkm-6Olkyn2S4WA.png)

This creates some folders and files that are essential for the project. You can have look in the following sceenshot. Change your directory to the project root as `cd rocket` then execute the command `mix test`

![img](https://cdn-images-1.medium.com/max/533/1*q7xE2x7Ak1j58BZpyQAZLg.png)

### Project Structure

![img](https://cdn-images-1.medium.com/max/533/1*DD79B86LKDF7zWvCcM7vFA.png)

#### **/Config/config.exs**

This directory comes with one file `config.exs`. The name itself states that it stands for the configuration of module in purpose. *What else can you configure here ?* Everything, that you can.

This file is mainly responsible for your application and its dependencies with the module `Mix.Config` module. `use Mix.Config`

#### Scope of Configuration

This configuration is loaded before any dependency loading into the project. This configuration file is used only in this project. For an example if any other application depends on this project, this file won’t be loaded nor affect the parent project which is depending on this project. For any third party you should be done in your `mix.exs` file.

#### How to configure your application ?

```
config :rocket, author: "blackode"
```

Here `:rocket` is our project_name `author` is the key and `blackode` is values of the key. You can access this configuration in the application as

```
Application.get_env(:rocket,:author)
```

#### 3rd-Party app Configuration

You can also configure the third party app just like how you configure your application.

```
config :typex, version: "0.1.1"
```

Just like above `:typex` is the third party module and you are stating the ` `version` value as `o.1.1` You can also state multiple values as `list`

### Environment configuration Imports

You can also import the configuration files relative to this directory. For example you can import the configuration files based on the environment you are using.First you have to define the configuration files.

`Config/dev.exs` this file configuration is loaded when you are working in the development environment. It overrides the current configuration.

`Config/test.exs` this file configuration is loaded when you are in `testing`environment. It also overrides the current configuration too.

As the imported configuration files override the current configuration defined here, which is why you have import them in last. You can import them as follows……

```
import_config "#{Mix.env}.exs"
```

By default above like commented. You can uncomment if you want to import them. `Mix.env` returns the environment you are running the project. For an example if your testing, it returns the `test` and if you are using in development mode it returns `dev` so the files will look like the following ways …

```
import_config "test.exs"
import_config "dev.exs"
```

### mix.exs

This is the project metadata file . This is the place where you add dependencies to the project , important links like **documentation** , **github**, etc and you can also **describe** the project here. You can add some **aliases** to run automatically with `mix` tool. Let’s dive into it deeply

```
def project do
    [app: :rocket,
     version: "0.1.0",
     elixir: "~> 1.4-rc",
     build_embedded: Mix.env == :prod,
     start_permanent: Mix.env == :prod,
     deps: deps()]
  end
```

This is where you define about the project. You can also added sections like following…

```
[
   description: "This fly into the sky",
   aliases: aliases(),
   package: package()
]
```

After adding those sections you can add functions. Here for description we directly added the binary so no need to define the function but for `aliases`and `package` we have defined functions as `aliases/0` and `package/0` Let’s define them.

```
defp package do
    [
      maintainers: [" Blackode "],
      licenses: ["MIT"],
      links: %{"GitHub" => "https://github.com/blackode/printex"}
    ]
end
```

```
defp aliases do
      [c: "compile"]
end
```

The `aliases` can be used like this. In general we **compile** our project with **mix** tool as `mix compile` as we have created an `alias` for **compile **here you can use that as `mix c` to compile the project.

### Adding Dependencies

You can add your project dependencies in following way

#### Hex Packages

Adding hex packages is so simple . You can see a private definition as `defp deps/0` by default it returns the empty list `[]` You can added packages like

```
defp deps do
 [ 
  {:my_dep, "~> 0.3.0"}
 ]
end
```

If you do not know about the version number you can specify as `{:my_dep,">= 0.0.0 "}` This is another way of installing most recent version of specified package.

#### Github: git/path repos

```
defp deps do
    [
      {:my_dep, git: "https://github.com/elixir-lang/my_dep.git",  tag: "0.1.0"},
    ]
 end
```

**or**

```
defp deps do
  [
    {:ex_doc, github: "elixir-lang/ex_doc",override:true}
  ]
end
```

You have an option to specify when to use the dependency. Suppose you want add dependency in only development mode, you can add `only` option like following..

```
defp deps do
    [{:ex_doc, github: "elixir-lang/ex_doc", override: true,only: :dev},
     {:earmark, "~> 1.0", only: :dev}, 
     {:dialyxir, "~> 0.3", only: [:dev]}
   ]
end
```

The `override` option will override any other package with the specified one. `only` option takes list of options like `[:dev,:test]`

Type `mix help deps `for more examples and options.

### Lib

#### /lib/rocket.ex

Lib directory is the place where all you coding stuff sits. You can see a file with the same name as project name by default. As our project name is `rocket` , it generated a file called `/lib/rocket.ex` You can add folders and files . For an example, you want add `rocket`folder to the `lib` directory and all the module names are namespace like `Rocket.Module`

#### Examples

Consider there are two files in the `/lib/rocket` directory as `speed.ex` and `trans.ex` You have to define the module names as

`/lib/rocket/speed.ex`

```
defmodule Rocket.Speed do
  ...
end
```

similarly for `trans.ex` . So that there will not be any namespace problems.

### Testing

The testing is very easy in elixir. Every file you add in the **lib** folder will have its own test file in the **test** directory with suffix **_test** and with an extension **.exs**

#### Naming Convention

Suppose if the file name in the `lib` directory is `rocket.ex` then the test file will be `rocket_test.exs` in the `test` directory. This is the naming convention followed.

Elixir uses the `ExUnit.Case` testing frame work. You can add the test units as following…

`/test/rocket_test.exs`

```
defmodule RocketTest do
  use ExUnit.Case
  doctest Rocket
```

```
  test "my test" do
    assert 1 + 1 == 2
  end
  test "another test " do
    assert 0==0
  end
end
```

You can test the project by executing command `mix test` from the project root directory.

### Documentation

Elixir provides a module called `exdoc` which generates the documentation. You need not to write again explaining each and every module.

what you have to do?

While writing the module you have to provide `@moduledoc` and `@doc`sections which will generate documentation for you .

**Example** will look like this.

```
defmodule Rocket do
  @moduledoc ~S"""
  This module helps in launching the rockets 
  with some basic information.
  It also helps with more info like rocket structure and use.
  """
```

```
  @doc ~S"""
  Returns  a string representation of the type of a passed argument
```

```
  ## Examples
  
      iex> Rocket.launch(:mars)
      {:ok,:landed}
  """
  @spec launc(atom)::tuple
```

```
  def launch(place) do
    #code logic ...
  end
end
```

After writing all the modules go to command line stay in the project root directory and type command as

```
mix docs
```

This generate `doc` directory with some files look like `api_reference.html``index.html` `rocket.html` and also directories like `fonts` `dist` etc ..in the project root . The `index.html` is your entry point for documentation.

### Publishing to Hex.pm

The last and final step is publishing your project to `hex.pm` package manger.This is very simple just you need to execute few commands.

#### Registration

The first and foremost thing is you have to be the registered user in order to publish your package to **hex.pm**. If you already registered you can skip this section. For *registration* go to command line and execute the following command

```
mix hex.user register
```

Then it will ask you some details includes:**username,email,password,password_confirmation**

![img](https://cdn-images-1.medium.com/max/533/1*5YYL5Hi2ZC9eBNFDRPTILA.png)

**Building Project**

The project building is not as hard as you thinking. just execute the command

```
mix hex.build
```

#### Publishing

Execute the following command from the project root directory. Make sure you have generated the docs with `mix docs` command and then publish .

```
mix hex.publish
```

It will ask you for the pass phrase. Type the password you have one at the time of registration. If everything goes well, you will see your package sitting in `hex.pm`

Is this difficult to write packages in elixir? I hope not. How about you ? Stop reading and write one.

> Start with small and end with big.

Watch Video

Video on Package building and publishing from scratch

Your feedback is welcomed with **honor**
