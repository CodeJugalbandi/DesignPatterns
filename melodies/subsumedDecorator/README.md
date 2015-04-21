## Subsumed Decorator

The GOF says that with Decorator you can
> Attach additional responsibilities to an object dynamically.  Decorators provide a flexible alternative to subclassing for additional functionality

In functional programming, we can easily achieve this with the ability to compose embellishing functions and pass the composed function.  This time lets see this in action in Haskell.

```

compress :: String -> String
compress str = "compressed " ++ str
                
encrypt :: String -> String
encrypt str = "encrypted " ++ str

to_ascii :: String -> String
to_ascii str = "to ascii " ++ str

write :: (String -> String) -> String -> IO()
write fn str = print (fn str)

main :: IO ()
main = do
  -- pass individual functions
  write id "Hello"
  write encrypt "Hello"
  write compress "Hello"
  write to_ascii "Hello"
  
  -- pass embellishing functions using compose
  write (to_ascii . compress . encrypt) "Hello"
  
  -- pass embellishing functions using foldl
  write (foldl (.) id decorators) "Hello"
    where
      decorators = [to_ascii, compress , encrypt]
```

The same can be written in Groovy as

```
def compress = { str -> "compressed $str" }

def encrypt  = { str -> "encrypted $str" }

def to_ascii = { str -> "to ascii $str" }

def identity = { it }

def write(String data, Closure fn) {
  fn(data)
}

// pass individual functions
println write('Hello', identity)
println write('Hello', compress)
println write('Hello', encrypt)
println write('Hello', to_ascii)

// pass embellishing functions using compose
println write('Hello', encrypt >> compress >> to_ascii)

// pass embellishing functions using inject
def decorators = [encrypt, compress, to_ascii]
println write('Hello', decorators.inject(identity) {acc, elem -> acc >> elem })

```



