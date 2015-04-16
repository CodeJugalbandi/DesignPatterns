## Subsumed Factory Method

The GOF design pattern book describes Factory Method pattern as - 
> Define an interface for creating an object, but let subclasses decide which class to instantiate.  Factory method lets a class defer instantiation to subclasses

In the context of functional programming, taking the pattern in the spirit and not structurally,    the ability to return functions from within functions, exhibits a factory method pattern in use.  

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

