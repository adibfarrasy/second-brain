#software #oop #kotlin 

For programming languages that support subtyping,

# Covariant

If we are able to assign the less specific type with the more specific sub-type, but not the other way around.

<aside> 💡 if A ≤ B, then I<A> ≤ I<B>

</aside>

```kotlin
interface TestCovariant<out T>

fun demoCovariant(moreDefinedT: TestCovariant<String>) {
	val lessDefinedT: TestCovariant<Any> = moreDefinedT // OK!
}
```

# Contravariant

The reverse of Covariant type, if we are able to assign the more specific sub-type with the less specific type.

<aside> 💡 if A ≤ B, then I<B> ≤ I<A>

</aside>

```kotlin
interface TestContravariant<in T>

fun demoContravariant(lessDefinedT: TestContravariant<Number>) {
	val moreDefinedT: TestContravariant<Int> = lessDefinedT // OK!
}
```

## Kotlin Notes:

in Kotlin, the `out` keyword denotes covariance and `in` keyword denotes contravariance.

Kotlin’s _declaration-site variance_ actually implies that the type can only be produced by the method for `out` , and can only be consumed for `in` .