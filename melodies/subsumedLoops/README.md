## Subsumed Loops

### Subsumed iteration

With the ability to pass functions around, we don't need looping constructs, expressed as keywords in language like - for, while-do, repeat-until, do-while.  Instead, we can just pass a function.  For example, in Java8, we could do:

```
void iterate(int times, Runnable body) {
  if (times <= 0) {
    return;
  }
  body.run();
  iterate(times - 1, body);
}

iterate(2, () -> System.out.println("Hello"));
// Hello
// Hello

```

The above is a simplified iteration without a predicate, but you get the idea.

### subsumed nested loops

With the ability to flatMap on Collections, we can eliminate nested loops.  For example, in Java8, one can use Stream to generate combinations.

```
List<List<?>> combinations(List<?> one, List<?> two) {
  return one.stream()
            .flatMap(f -> 
               two.stream().map(s -> Arrays.asList(f, s)))                  
            .collect(Collectors.toList());
}

List<Character> alphabets = Arrays.asList('a', 'b');
List<Integer> numbers = Arrays.asList(1, 2); 
List<List<?>> combo = combinations(alphabets, numbers);

System.out.println(combo); // [[a, 1], [a, 2], [b, 1], [b, 2]]
```

In Scala, the same could be written as

```
List('a', 'b') flatMap(c => List(1, 2) map(n => (c, n)))

// List((a,1), (a,2), (b,1), (b,2))
```

In Haskell, its called the bind operator.

```
['a', 'b'] >>= \c -> [1, 2] >>= \n -> [(c, n)]
-- [('a',1),('a',2),('b',1),('b',2)]

```

Both, Scala and Haskell provide what is called as comprehensions to make the above more readable.  In Scala, its the for-comprension

```
for {
  c <- List('a', 'b')
  n <- List(1, 2)
} yield (c, n)

// List((a,1), (a,2), (b,1), (b,2))
```

In Haskell, its list comprehension

```
[(c, n) | c <- ['a', 'b'], n <- [1, 2]]

-- [('a',1),('a',2),('b',1),('b',2)]
```
