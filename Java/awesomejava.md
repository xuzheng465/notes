https://www.codejava.net/java-core/collections/18-java-collections-and-generics-best-practices

https://www.zhihu.com/question/48990017



[Can't find jpackage](https://stackoverflow.com/questions/61307077/cant-find-jpackage-via-toolprovider)



Multithread:

https://juejin.cn/post/6844904117547057159

https://github.com/jfoenixadmin/JFoenix/issues/955



```
Ok, so according to @seinecle link, adding an open to vm arg should fix it:
--add-opens java.base/java.lang.reflect=ALL-UNNAMED
or
--add-opens java.base/java.lang.reflect=com.jfoenix
@seinecle can you verify the fix?
```

