## Subsumed Factory Method

With the ability to return functions from within functions, we have functions producing functions in a factory.  For example, lets look at Java8, the power function returns a function that can be utilized to create a square function or a cube function.

```
Function<Double, Double> power(double raiseTo) {
   return x -> Math.pow(x, raiseTo);
}

Function<Double, Double> square = power(2.0);
square.apply(2.0); // 4.0

Function<Double, Double> cube = power(3.0);
cube.apply(2.0);  // 8.0
```

The original GOF design pattern books says - 
> Define an interface for creating an object, but let subclasses decide which class to instantiate.  Factory method lets a class defer instantiation to subclasses

Taking the pattern in the spirit and not structurally, we have in functional programming functions that return functions as factory method.   Also to be more precise the above example is a variant - parameterized factory method as it takes in a parameter to generate either a square or a cube function.