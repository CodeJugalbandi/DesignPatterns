## Subsumed Template Method

The GOF design pattern book describes Template Method pattern as - 

> Define the skeleton of an algorithm in an operation, deferring somesteps to
subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure. 

In the context of functional programming, applying the pattern in the spirit and not structurally; with the ability to pass functions to functions subsumes this pattern.  

Lets start with a Java program

```java
abstract class Logger {
  enum Level { INFO, WARN, ERROR, FATAL };
  public void log(Level level, String message) {
    String logMessage = enrich(level, message);
    write(logMessage);
  }
  // Hook
  protected String enrich(Level level, String message) {
    return String.format("%s [%s] %s %s", new Date(), Thread.currentThread(), level, message);
  }
  //Mandate
  abstract void write(String message);
}

class DatabaseLogger extends Logger {
  DatabaseLogger(String url) { }
  void write(String message) {
    System.out.println("Database Logger writing => " + message);
  }
}

class ConsoleLogger extends Logger {
  void write(String message) {
    System.out.println("Console Logger writing => " + message);
  }
}

public static void main(String[] args) throws Exception {
  Logger db = new DatabaseLogger("url");
  db.log(Logger.Level.INFO, "Hello");
  Logger console = new ConsoleLogger();
  console.log(Logger.Level.INFO, "Hello");
}
```

Refactoring this to subsume the template mandate ```write```.  This part of the template will now become argument to the log method.

```java
import java.util.function.*;
import java.util.*;

class Logger {
  enum Level { INFO, WARN, ERROR, FATAL };
  public void log(Level level, String message, Consumer<String> destination) {
    String logMessage = enrich(level, message);
    destination.accept(logMessage);
  }
  
  protected String enrich(Level level, String message) {
    return String.format("%s [%s] %s %s", new Date(), Thread.currentThread(), level, message);
  }
}

class DatabaseWriter {
  DatabaseWriter(String url) { }
  void write(String message) {
    System.out.println("Database Logger writing => " + message);
  }
}

public static void main(String[] args) throws Exception {
  Logger logger = new Logger();
  DatabaseWriter db = new DatabaseWriter("url");
  logger.log(Logger.Level.INFO, "Hello", db::write);
  logger.log(Logger.Level.INFO, "Hello", System.out::println);
}

```
