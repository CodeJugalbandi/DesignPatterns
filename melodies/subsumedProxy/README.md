## Subsumed Proxy

As functions are first-class citizens in a functional programming language, passing them around to be wrapped into another function of the same type will be transparent to the client.  The wrapper function can then decide to delegate the call to the actual function as deemed appropriate.  This is really a proxy pattern.

Lets see how memoization is implemented to wrap an expensive computation and cache results of that computation for same inputs.

For memoization to work, the function that is being wrapped must be a pure function, whose output depends only on the inputs supplied.  Functions with side-effects should not be memoized.

Lets use Clojure this time around to make this happen.

```
(defn add [x y] 
  (do (println (str "add called with x = " x ", y = " y))
    (+ x y)))

; memoize is available out-of-box in Clojure
(def madd (memoize add))

(madd 2 3)
; add called with x = 2, y = 3
; 5

; next call does not execute add, instead returns value from cache.
(madd 2 3)
; 5
```

Though memoization is also available in Groovy, lets implement and see it in action ourselves.

```
def memoize(Closure fn) {
  def map = [:]
  return { ...args -> 
    def tuple = new Tuple(*args)
    if (map.containsKey(tuple)) {
      return map[tuple]
    } else { 
      def result = fn(*args)  
      map[tuple] = result
      result
    }
  }
}

def add = { x, y ->  
  println "add called with x = $x, y = $y"
  x + y
}

def madd = memoize(add)

println (madd(2, 3))
// add called with x = 2, y = 3
// 5

println (madd(2, 3))
// 5
```
