## Subsumed Factory Method

The GOF design pattern book describes Factory Method pattern as - 
> Define an interface for creating an object, but let subclasses decide which class to instantiate.  Factory method lets a class defer instantiation to subclasses

In the context of functional programming, taking the pattern in the spirit and not structurally, the ability to return functions from within functions, exhibits a factory method pattern in use.  

For example, lets look at Java8, the power function returns a function that can be utilized to create a square function or a cube function.

```
Function<Double, Double> power(double raiseTo) {
   return x -> Math.pow(x, raiseTo);
}

Function<Double, Double> square = power(2.0);
square.apply(2.0); // 4.0

Function<Double, Double> cube = power(3.0);
cube.apply(2.0);  // 8.0
```

Also to be more precise the above example is a variant of factory method pattern - parameterized factory method as it takes in a parameter to generate either a square or a cube function.

### Currying/PFA and Factory Method

Additionally if we use Currying or Partial Function Application, we can wire in parameters to a parameterized factory method from different scopes.  In Java8,

```
Function<Double, Function<Double, Double>> power() {
   return raiseTo -> x -> Math.pow(x, raiseTo);
}

//this could be in one scope
Function<Double, Double> square = power().apply(2.0);

//application done in a different scope
square.apply(2.0); // 4.0

Function<Double, Double> cube = power().apply(3.0);
cube.apply(2.0);  // 8.0

```

In Scala,

```
val power = x => raiseTo => Math.pow(x, raiseTo)

//this could be in one scope
val square = power(2.0)

//application done in a different scope
square(2.0) // 4.0

val cube = power(3.0)
cube(2.0)  // 8.0
```

Using explicit currying syntax of Scala

```
def power(raiseTo: Double)(x: Double) = Math.pow(x, raiseTo)

val square = power(2.0) _

//application done in a different scope
square(2.0) // 4.0

val cube = power(3.0) _
cube(2.0)  // 8.0
```

In Haskell, as functions are curried by default, we have no special syntax like scala

```
power :: (Num a, Integral b) => b -> a -> a
power raiseTo number = number^raiseTo
square = power 2
cube = power 3
square 2 -- 4
cube 2   -- 8

```

We have seen currying always spices up things you do with functions :)




