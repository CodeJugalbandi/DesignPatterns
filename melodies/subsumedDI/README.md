## Subsumed Dependency Injection
Let’s say we have a Customer repository

```java
class CustomerRepository {
  public Customer findById(Integer id) {
    if (id > 0) 
      return new Customer(id);
    else
      throw new RuntimeException("Customer Not Found"); 
  }   
}

```

Now, we want to allow authorised calls to repo.  So, Let’s write an authorise function.

```java
class Authoriser {
  public Customer authorise(CustomerRepository rep, Request req) {
    //Some auth code here which guards the request.
    return rep.findById(req.get());
  }   
}
```

Let’s see them in action…

```java
CustomerRepository repo = new CustomerRepository();
Authoriser authoriser = new Authoriser();

Request req1 = new Request();
Customer customer1 = authoriser.authorise(repo, req1);

Request req2 = new Request();
Customer customer2 = authoriser.authorise(repo, req2);
```

Requests vary, however the CustomerRepository is same.  Can we avoid repeated injection of the repo? Yes! we can. We re-shape authorise to accept only one fixed parameter - CustomerRepository

```java
class Authoriser {
  public 
  Function<Request, Customer> authorise(CustomerRepository repo) {
    //Some auth code here which guards the request.
    return req -> repo.findById(req.get());
  }   
}
```

Now

```java
CustomerRepository repo = new CustomerRepository();
Function<Request, Customer> curriedAuthorise = authorise(repo);

Request req1 = new Request();
Customer customer1 = curriedAuthorise.apply(req1);

Request req2 = new Request();
Customer customer2 = curriedAuthorise.apply(req2);
```

We don’t have to provide all the arguments to the function at one go! This is partially applying the function.  In other words, currying enables Partial Function Application.

We saw how currying  decouples function arguments to facilitate just-in-time dependency injection.  But, how about constructor or setter dependency injection? Lets see how currying acts as a powerful decoupler, not just limited to the site function arguments (at least in OO languages).

Regular DI looks like...

```java
interface Transaction { }
interface ApprovalStrategy {
  boolean approve(List<Transaction> ts);
  //…
}
class Clearing {
  private final ApprovalStrategy aps; 

  Clearing(ApprovalStrategy aps) { 
     this.aps = aps;
  }

  public boolean approve(List<Transaction> ts) {
    return aps.approve(ts);
  }   
}

//main
ApprovalStrategy singleMakerChecker = new SingleMakerChecker();
Clearing clearing = new Clearing(singleMakerChecker);
clearing.approve(ts);
```

Curried DI

```java
interface Transaction { }
interface ApprovalStrategy {
  boolean approve(List<Transaction> ts);
  //…
}
class Clearing {

  public 
  Function<ApprovalStrategy, Boolean> approve(List<Transaction> ts) {
    return aps -> aps.approve(ts);
  }   
}
//main
Clearing clearing = new Clearing();
// ApprovalStrategy can now be injected from different contexts,
// one for production and a different one - say mock for testing,
// Just like in case of Regular DI.
clearing.approve(ts).apply(new SingleMakerChecker());
```

DI is achieved, not just by injecting functions, but also by currying functions.  When we curry arguments, we are injecting dependency.