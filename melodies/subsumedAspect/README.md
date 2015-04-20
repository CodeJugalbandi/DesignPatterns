## Subsumed Aspect

### Timer

```
def timer(Closure fn, ...args) {
  long startTime = System.currentTimeMillis()
  def result = fn(*args);
  long timeTaken = System.currentTimeMillis() - startTime
  System.out.println("Time: " + timeTaken + " ms")
  result
}
```
### Method Trace Logger



