//20201126

## Roadmap

#### Introduction

* A collection is an object

* A missing object is also an object. 

#### Branching on Booleans

Replace branching with polymorphic calls

#### Immutable objects

How to avoid bugs due to mutability

#### Avoiding nulls

Null is not an object

#### Optional\<T> type

No more nulls in bussiness applications

----

```java
public class Main {
  public int sum(int[] values) {
    int sum = 0;
    for (int value: values) {
      sum += value;
    }
    return sum;
  }
  
  public static void main(String[] args) {
    
  }
}
```

What if we want sum even numbers or odd numbers?

```java
for (int value: values) {
  if (oddNumbersOnly || value % 2 != 0) {
    sum += value
  }
}
```

Hard-coded decisions impair maintenability

:warning: **Advice**:

Don't change code to modify behavior

Try to substitute an object with a different behavior



```java
public class Main {
  public int sum(int[] values, selector) {
    return values.sum(selector)
  }
}
// 
```

This class is the consumer. It needs a summation **service**

Consumers should not implement services themselves

