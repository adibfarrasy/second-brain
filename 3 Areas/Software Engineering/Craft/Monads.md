_by Bartosz Milewski_
#software #functionalprogramming

reference:
[https://www.youtube.com/watch?v=gHiyzctYqZ0](https://www.youtube.com/watch?v=gHiyzctYqZ0)

<aside> 💡 “…they ask you what is a function, and you try to explain what a function is. Then you give them examples; you can have a function that takes a list of fishermen and orders them by the number of fishes they caught, or another function that takes a person and gives you their age… and they say wow… function must be like some powerful thing, it’s a kind of mushroom that you eat.”

</aside>

# Introducing Monad

Monads provide a composition for Kleisli arrows. Kleisli arrow is a notation that denotes the output of a function `f` that returns a functor of type `a` can be used as an input of another function `g` that requires an input of type `a` and returns a functor of type `b`. A _functor_ is an embellished type, e.g. list of T, pointer of T, Maybe of T, etc. where T is a type like integer, boolean, …

A monad is a functor with two additional properties `join` and `return`. Join is an operation where a nested embellished type can be flattened into a non-nested embellished type, i.e. `join:: m(ma) -> ma` . Return is an operation where the identity of the type returns an embellished type, i.e `return:: a -> m(a)` .

# Why is a Kleisli Arrow useful?

Motivations:

1. To replace impure functions born out of necessity (accessing global states, exceptions, I/O etc.) into pure functions that return embellished results.
2. To be able to do compositions, i.e. write mutliple small functions that produce side effects and combine them instead of one gigantic function. This is what the monad does for the Kleisli arrow.

# Pop Culture Monad Definition

<aside> 💡 “A monad is just a **monoid** in a category of **endofunctor**, what’s the problem?”

</aside>

A _monoid_ is a [semigroup](https://en.wikipedia.org/wiki/Semigroup) that has an [identity element](https://en.wikipedia.org/wiki/Identity_element). It defines at least an associative binary operation (usually addition and/or multiplication), and a identity element (`”“` for string type, `0` for unsigned integer, etc.). An identity element `e` doesnt change a set `s` when the binary operation is applied to it, i.e `e*s=s`

An _endofunctor_ is a kind of functor that does not change Category after mapping. In programming only endofunctor is possible, as the functor is dealing with only one Category — Category of types.