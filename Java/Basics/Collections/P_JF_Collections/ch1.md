### Collection Design

| Interfaces                         | Implementation              |
| ---------------------------------- | --------------------------- |
| Multiple data structures           | Specific data structures    |
| Functional characteristics         | Performance characteristics |
| Prefer as variable type            | Concrete and instantiate    |
| Often has a popular implementation |                             |



![collectionofcols](/Users/xuzheng/Projects/notes/Java/Basics/Collections/P_JF_Collections/ch1.assets/collectionofcols.png)

<img src="/Users/xuzheng/Projects/notes/Java/Basics/Collections/P_JF_Collections/ch1.assets/checkcollection.png" alt="checkcollection" style="width:600px;" />

<img src="/Users/xuzheng/Projects/notes/Java/Basics/Collections/P_JF_Collections/ch1.assets/iterable.png" alt="iterable" style="width:500px;" />

```java
size();
isEmpty();
add();
addAll();

remove(ele);
removeAll(collection); // Remove all the elements of the argument collection to this collection.
retainAll(collection); // Remove all the elements of this collection not in the argument collection

contains(ele);	// True if the element is in this collection, false otherwise
containsAll(collection); // True if all the 
clear()		// remove all the eles from the col
```

