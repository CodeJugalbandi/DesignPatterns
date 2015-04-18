## Subsumed Proxy

As functions are first-class citizens in a functional programming language, passing them around to be wrapped into another function of the same type will be transparent to the client.  The wrapper function can then decide to delegate the call to the actual function as deemed appropriate.  This is really a proxy pattern.

Lets see how memoization is implemented to wrap an expensive computation and cache results of that computation for same inputs.


For memoization to work, the function that is being wrapped must be a pure function, whose output depends only on the inputs supplied.  Functions with side-effects should not be memoized.

Lets use Clojure this time around to make this happen.

```


```

