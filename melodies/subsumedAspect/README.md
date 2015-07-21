## Subsumed Aspect

### Time another function
In Java8, lets say we want to time a 1-arg function:
```
public <T, R> Function<T, R> time(Function<T, R> fn) {
  return t -> {
    long startTime = System.currentTimeMillis();
    R r = fn.apply(t);
    long timeTaken = System.currentTimeMillis() - startTime;
    System.out.println("Time taken = " + timeTaken + " ms");
    return r;
  };
}

Function<Integer, Integer> expensiveSquare = x -> {
  try {
    Thread.sleep(2000);
  } catch(InterruptedException ie) {
  }
  return x * x;
};
```

In Groovy, we could time an var-args function like this:

```
def time(Closure fn) {
  return { ...args ->
    long startTime = System.currentTimeMillis()
    def result = fn(*args);
    long timeTaken = System.currentTimeMillis() - startTime
    System.out.println("Time: " + timeTaken + " ms")
    result
  }
}

def expensiveSquare = { x -> 
  Thread.sleep(2000)
  x * x
}

def expensiveAdd = { x, y ->
  Thread.sleep(2000)
  x + y
}

def square = time(expensiveSquare)
println square(2)

def add = time(expensiveAdd)
println add(2, 3)
```

