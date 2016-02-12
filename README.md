# Making Sense of Elixir Directives

![](http://share.ryandaigle.com/nozrq-20160212114122.png)

This is a lightning talk I gave at the [Triangle |> Elixir group](http://triangle-elixir.github.io) discussing the purpose of the various Elixir directives (which [I find to be confusing](https://medium.com/@rwdaigle/making-sense-of-elixir-directives-89190b05560) for new Elixirists).

* [Video](https://www.youtube.com/watch?v=7MPp4pQRTeE)
* [Slides](https://github.com/rwdaigle/elixir-directives/blob/master/slides.pdf)

---

## What are Directives?

* Way to declare, import and reference other Modules
* Elixir has *four* of them
* But, but, why?

^ Think `require` in Ruby, `import` in Python

^ Elixir has four, all with slightly different purposes. Based on conversation I was having I find them confusing so I dug in a bit

^ Wrote up my gripes, presented them to Jose & Chris McCord. Summary is they agree it's confusing, but have a hard time figuring out how to change things already set in motion. So, this is my effort to add some clarity to the situation.

---

## Elixir's Directives

* alias
* import
* require
* use

^ What are Elixir's directives?

^ Last one might not technically be a directive, but it's so closely tied to the purpose of directives that I'm going to abuse the term and include it

—

## alias

Used to reference a module in short-hand

```elixir
alias Foo.Bar, as: Bar

# Can now do
Bar.function
```

^ Can also rename a module to avoid conflicts or make clearer to your context

—

## import

Used to easily access functions or macros from other modules

```elixir
import List, only: [duplicate: 2]

# Can now do
duplicate("hello", 3)
```

^ Can import all or some functions

^ Seems like this is just special form of `alias`, yeah?

—

## require

Used to make sure module is compiled and available

```elixir
require Foo

Foo.a_macro
```

^ Used a lot for macros which need to be present at compile time

^ Ok, but when don't I want the thing to be compiled and available?

^ Turns out, so you can have circular dependencies, so use `alias` to soft-reference modules by default

—

## use

Callback that injects code into current context

```elixir
use ExUnit.Case, async: true

test "now have testing macros" do
	assert true
end
```

^ Hook point that lets modules bring in external functionality

^ Invokes `Module.__using__/1`

^ Automatically `require`s module

—

## Summary

* `alias` to more conveniently reference modules
* `import` to more conveniently reference functions
* `require` to ensure macros are available and compiled
* `use` to give module opportunity to "hook" into your code

^ All are lexically scoped

^ Biggest beef is that directives are implementation specific

^ Have to know if what you're using is a macro or just function to know how to invoke it

^ Have to know if module needs to hook into your code to know to use `use`

^ Shouldn't have to think about these things as a user

—

## Background

* Current [Elixir directive docs](http://elixir-lang.org/getting-started/alias-require-and-import.html)
* [IRC design discussion](https://gist.github.com/rwdaigle/d11bf4ff5466324b0951) between me, Jose Valim, and Chris McCord about this very topic
* [Mailing list discussion](https://groups.google.com/forum/#!msg/elixir-lang-core/EsiO881g2MA/k966k4BY2cAJ) about directives from long ago
* My [proposed changes](https://medium.com/@rwdaigle/making-sense-of-elixir-directives-89190b05560)
