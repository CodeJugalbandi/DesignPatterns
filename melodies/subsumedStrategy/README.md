## Subsumed Strategy


Its common in retail banking to find maker and a checker for creating and approving transactions.  Sometimes transactions above certain financial limit, requires 2 checkers instead of 1.  Below is how this can be implemented as a Strategy in C# using the Object Oriented approach.

```java
class Maker { }
class Checker { }
interface Transaction { }

interface ApprovalStrategy {
  bool Approve(IList<Transaction> ts);
}

class SingleMakerChecker : ApprovalStrategy {
   public SingleMakerChecker(Maker m, Checker c) {
   }

   public bool Approve(IList<Transaction> ts) {
     Console.WriteLine("SingleMakerChecker.Approve");
     return true;
   }
}

class DoubleMakerChecker : ApprovalStrategy {
  public DoubleMakerChecker(Maker m, Checker c1, Checker c2) {
  }

  public bool Approve(IList<Transaction> ts) {
    Console.WriteLine("DoubleMakerChecker.Approve");
    return true;
  }
}

static class Transactions {
   public static bool Approve(IList<Transaction> ts, ApprovalStrategy aps) {
     Console.WriteLine("Approving...");
     return aps.Approve(transactions);
   }   
}



Maker m = new Maker();
Checker c = new Checker();
  
ApprovalStrategy approvalStrategy = new SingleMakerChecker(m, c);
IList<Transaction> transactions = new List<Transaction>();
Transactions.Approve(transactions, approvalStrategy);
```

The same can be achieved in C# by getting rid of the Approval Strategy interface, instead using function to pass around instead of interface.

```csharp
class Maker { }
class Checker { }
interface Transaction { }

class SingleMakerChecker {
   public SingleMakerChecker(Maker m, Checker c) {
   }

   public bool Approve(IList<Transaction> ts) {
       // checker approves
     Console.WriteLine("SingleMakerChecker.Approve");
     return true;
   }
}

class DoubleMakerChecker {
  public DoubleMakerChecker(Maker m, Checker c1, Checker c2) {
  }

  public bool Approve(IList<Transaction> ts) {
    // checker1 and checker2 approve
    Console.WriteLine("DoubleMakerChecker.Approve");
    return true;
  }
}

static class Transactions {
  public static bool Approve(IList<Transaction> ts, Func<IList<Transaction>, bool> aps) {
     Console.WriteLine("Approving...");
     return aps(transactions);
   }
}


Maker m = new Maker();
Checker c = new Checker();
SingleMakerChecker approvalStrategy = new SingleMakerChecker(m, c);

IList<Transaction> transactions = new List<Transaction>();
Transactions.Approve(transactions, approvalStrategy.Approve);

```



