kotlin 在编译以后依旧是jvm的一个class



**Java与Kotlin交互的语法变化**

java code `TestMain.class`

kotlin code `TestMain::class.java`



```kotlin
import kotlin.reflect.KClass
fun main(args: Array<String>) {
    println("Hello World!")
    testClass(JavaMain::class.java)
    testClass(KotlinMain::class)
}


fun testClass(clazz: Class<JavaMain>){
    println(clazz.simpleName)
}

fun testClass(clazz: KClass<KotlinMain>) {
    println(clazz.simpleName)
}
```



kt中in是关键字

```kotlin
println(JavaMain.`in`)
```



### if is an expression



