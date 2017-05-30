## Subsumed Template Method

The GOF design pattern book describes Template Method pattern as - 
> Define an interface for creating an object, but let subclasses decide which class to instantiate.  Factory method lets a class defer instantiation to subclasses

In the context of functional programming, taking the pattern in the spirit and not structurally, the ability to pass functions to functions, exhibits a factory method pattern in use.  

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
  Logger console = new ConsoleLogger("filename");
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
