## Static Factory Methods

sfm encapsulate the construction logic



## Constructor Chaining

```java
class BankAccount {
	BankAccount() {
		this(0);
	}
  BankAccount(double balance) {
    this(balance, 0.01);
  }
  BankAccount(double balance, double interest) {
    if (interest < 0) {
      throw new IllegalArgumentException("Interest rate can't be less than 0");
    }
    if (balance < 0) {
      throw new IllegalArgumentException("Starting balance can't be less than 0");
    }
    this.balance = balance;
    this.interest = interest;
  }
}
```

Constructor chaining helps adhere to the **DRY** principle





## Constructor Telescoping

Using the Builder Pattern

```java
class Pizza {
  static class Builder {
    // mandatory
    private final int size;
    
    // default is false
    private boolean cheese;
    private boolean ham;
    
    Builder(int size) {
      this.size = size;
    }
    Builder cheese(boolean value) {
      cheese = value;
      return this;
    }
    Builder ham(boolean value) {
      ham = value;
      return this;
    }
    Pizze build() {
      return new Pizza(this);
    }
  }
  
  private Pizza(Builder builder) {
    int size = builder.size;
    boolean cheese = builder.cheese;
    boolean ham = builder.ham;
  }
}

public class App {
  public static void main(String[] args) {
    Pizza pizza = new Pizza.Builder(12)
      											.cheese(true)
      											.ham(true)
      											.build();
  }
}
```



