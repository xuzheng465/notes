# Escaping References





encapsulation





```java

```



### Immutable Collections

`Collections.unmodifyableMap`

`Collections.unmodifyableList`











































## Q&A

> Q: I understand `Strings` are immutable, but they're also objects, and if we return a String, we are returning the reference to that object, right? So it is a potential Escaping Reference case, isn't it? I'm a bit confused about this part.

A: Yes, it is escaping reference, but in this particular case it doesn't lead to a vulnerability. The main problem of `escaping references` is that we can change referenced object state without notifying the object from which the reference come from, and that can lead to a vulnerability or bug. In this case referenced object is immutable, and any work with it will not change its state, and so will not change the state of the holder object. So, **we actually can allow references to immutable objects escape out of the holder object without any troubles**.



