```java
@FunctionalInterface
public interface Comparator<T> {

    public int compare(T t1, T t2);
    
    public default Comparator<T> thenComparing(Comparator<T> cmp) {
        
        return (p1, p2) -> compare(p1, p2) == 0 ? cmp.compare(p1, p2) : compare(p1, p2) ;
    }
    
    public default Comparator<T> thenComparing(Function<T, Comparable> f) {
        
        return thenComparing(comparing(f)) ;
    }
    
    public static <U> Comparator<U> comparing(Function<U, Comparable> f) {
        
        return (p1, p2) ->  f.apply(p1).compareTo(f.apply(p2));
    }
}
```

```java
public class MainComparator {

    public static void main(String... args) {
        
        Comparator<Person> cmpAge = (p1, p2) -> p2.getAge() - p1.getAge() ;
        Comparator<Person> cmpFirstName = (p1, p2) -> p1.getFirstName().compareTo(p2.getFirstName()) ;
        Comparator<Person> cmpLastName = (p1, p2) -> p1.getLastName().compareTo(p2.getLastName()) ;
        
        Function<Person, Integer> f1 = p -> p.getAge();
        Function<Person, String> f2 = p -> p.getLastName();
        Function<Person, String> f3 = p -> p.getFirstName();

        Comparator<Person> cmpPersonAge = Comparator.comparing(Person::getAge);
        Comparator<Person> cmpPersonLastName = Comparator.comparing(Person::getLastName);
        
        Comparator<Person> cmp2 = cmpPersonAge.thenComparing(cmpPersonLastName);
      // 既可以传进Comparator又可以传进Function
        
        Comparator<Person> cmp = Comparator.comparing(Person::getLastName)
                                           .thenComparing(Person::getFirstName)
                                           .thenComparing(Person::getAge);
    }
}
```

Method References

An alternative syntax for lambda expr

```Java
Function<Person, Integer> f = person -> person.getAge();

Function<Person, Integer> f = Person::getAge;
```

```java
BinaryOperator<Integer> sum = (i1, i2) -> i1+i2;
														= (i1, i2) -> Integer.sum(i1, i2);
BinaryOperator<Integer> sum = Integer::sum;
```

```java
Consumer<String> printer = s -> System.out.println(s);
												 = System.out::println;
```









