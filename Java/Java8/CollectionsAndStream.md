# Functional Interface

## Main Category

* A consumer **consumes an object, and does not return anything.**

```java 
public interface Consumer<T> {
  public void accet(T t);
}

public interface BiConsumer<T>
```





* A supplier provides an object, **take no parameter**

```java
public interface Supplier<T> {
  public T get();
}

Supplier<Person> personSupplier = () -> new Person();
																= Person::new;
```



* A **funtion** takes an object an returns another object

```java
public interface Function<T, R> {
  public R apply(T t);
}

Function<Person, Integer> ageMapper = person -> person.getAge();
																			Person::getAge;

public interface BiFunction<T, V, R> {
  public R appy(T t, V v);
}
```



* UnaryOperator takes a pararmeter of a type and returns the parameter of the same type

```java
public interface UnaryOperator<T> extends Function<T, T> {
  
}
```

* BinaryOperator takes two objects of the same type and returns one object of the same type

```java
public interface BinaryOperator<T> extends BiFunction<T, T, T> {
  
}
```

* A predicate takes an object and returns a boolean

```java
public interface Predicate<T> {
  public boolean test(T t);
}
```



## Function Interfaces for Primitive Types

* Other functional interfaces have been defined, for instance 
  * IntPredicate
  * IntFunction
  * IntToDoubleFunction



# Collection API



## Iterable & Collection Interfaces

* On Iterable

```java
// On Iterable
boolean forEach(Consumer<? super E> consumer);

// On Collection
boolean removeIf(Predicate<? super E> filter);

// LIst
boolean replaceAll(UnaryOperator<? super E> operator);

// On List
boolean sort(Comparator<? super E> comparator);

List<String> names = ... ;
names.replaceAll(name -> name.toUpperCase());
names.replaceAll(String::toUpperCase);

// On map
void forEach(BiConsumer<? super K, ? super V> consumer);

Map<City, List<Person>> map = ... ;
map.forEach(
	(city, list) -> 
  System.out.println(city + ": " + list.size() + " people")
);

// On Map
v getOrDefault(Object key, V defaultValue);

System.out.println(map.getOrDefault(boston, emptyList()));
```



```java
V putIfAbsent(K key, V value);
```

if exist, return the previews value

```java
// New replace method for Map
V replace(K key, V newValue);
boolean replace (K key, V existingValue, V newValue);

void replaceAll(BiFunction<? super K, ? super V, ? extends V> function);

// On map
void remove(Object key, Object value);

// On Map
V compute( K key, BiFunction<? super K, ? super V, ? extends V> remapping);
// todo example

// 更新hashmap的值. compute()方法试图为指定的键和它的当前映射值计算一个映射（如果没有找到当前映射，则为空）该方法用于原子化地更新HashMap中给定键的值。

// On Map
V computeIfPresent(K, BiFunction);
```



The **computeIfPresent()** version

* Computes a new value from:
  * the key passed as a parameter, that should be in the map
  * the existing value, cannot be null
  * the lambda to compute the remappiing from the key and the existing value

```java
public class GFG {
  public static void main(String[] args) {
    HashMap<String, Integer> wordCount = new HashMap<>();
    wordCount.put("zheng", 1);
    wordCount.put("xu", 2);
    wordCount.put("Oliver",3);
    System.out.println("Hashmap before operation :\n "
                           + wordCount); 
    // provide new value for keys which is present 
    // using computeIfPresent method 
    wordCount.computeIfPresent("Geek", 
                                   (key, val) -> val + 100); 
    // print new mapping 
        System.out.println("HashMap after operation :\n "
                           + wordCount); 
  }
}
```

Output

```
Hashmap before operation :
 {geeks=3, Geeks=1, for=2}
HashMap after operation :
 {geeks=3, Geeks=1, for=2}
```



The **computeIfAbsent()** version

```java
// Java program to demonstrate 
// computeIfAbsent(Key, Function) method. 

import java.util.*; 

public class GFG { 

	// Main method 
	public static void main(String[] args) 
	{ 

		// create a HashMap and add some values 
		HashMap<String, Integer> map 
			= new HashMap<>(); 
		map.put("key1", 10000); 
		map.put("key2", 55000); 
		map.put("key3", 44300); 
		map.put("key4", 53200); 

		// print map details 
		System.out.println("HashMap:\n "
						+ map.toString()); 

		// provide value for new key which is absent 
		// using computeIfAbsent method 
		map.computeIfAbsent("key5", 
							k -> 2000 + 33000); 
		map.computeIfAbsent("key6", 
							k -> 2000 * 34); 

		// print new mapping 
		System.out.println("New HashMap:\n "
						+ map); 
	} 
} 

```

Output:

```java
HashMap:
 {key1=10000, key2=55000, key3=44300, key4=53200}
New HashMap:
 {key1=10000, key2=55000, key5=35000, key6=68000, key3=44300, key4=53200}
```



* All the compute*() methods return the value
  * that has just been computed
  * or that was in the map before
* Useful to build maps of maps

```java
Map<String, Map<String, Person>> map = ...;

//key, newValue
map.computeIfAbsent(
	"one",
  key -> new HashMap<String, Person>()
).put("two", john)
```



