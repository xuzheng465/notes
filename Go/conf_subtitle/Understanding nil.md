nil is not a keyword



### slice

<img src="./Understanding nil.assets/image-20201208195531880.png" alt="image-20201208195531880" style="width:600px;" />



<img src="./Understanding nil.assets/image-20201208195553183.png" alt="image-20201208195553183" style="width:600px;" />



<img src="./Understanding nil.assets/image-20201208195614030.png" alt="image-20201208195614030" style="width:600px;" />



### channels, maps, and functions

<img src="./Understanding nil.assets/image-20201208195710374.png" alt="image-20201208195710374" style="width:600px" />



<img src="./Understanding nil.assets/image-20201208195724631.png" alt="image-20201208195724631" style="width:600px;" />

### interface

(type, value)



```go
var s fmt.Stringer // Stringer (nil, nil)
fmt.Println(s == nil) // true
```



```go
var p *Person // nil of type *Person
var s fmt.Stringer = p // Stringer (*Person, nil)
fmt.Println(s == nil)	// false
```

(*Person, nil) doesn't equal nil



When is nil not nil?

<img src="./Understanding nil.assets/image-20201208222329760.png" alt="image-20201208222329760" style="width:600px;" />



**Do not declare concrete error vars**

<img src="/Users/xuzheng/Projects/notes/Go/conf_subtitle/Understanding nil.assets/image-20201208223316297.png" alt="image-20201208223316297" style="width:600px" />



**Do not return concrete error types**



### kinds of nil

| type       | nil                                            |
| ---------- | ---------------------------------------------- |
| pointers   | point to nothing                               |
| slice      | have no backing array                          |
| maps       | are not initialized                            |
| channels   | are not initialized                            |
| functions  | are not initialized                            |
| interfaces | have no value assigned, not even a nil pointer |

**nil receivers are useful**



<img src="/Users/xuzheng/Projects/notes/Go/conf_subtitle/Understanding nil.assets/image-20201208224907012.png" alt="image-20201208224907012" style="width:600px" />



<img src="/Users/xuzheng/Projects/notes/Go/conf_subtitle/Understanding nil.assets/image-20201208225053680.png" alt="image-20201208225053680" style="width:600px;" />



Use ==**nil chans**== to disable ==**select**== case



![image-20201208231629561](/Users/xuzheng/Projects/bookcode/Golang/gopkg/image-20201208231629561.png)



nil satisfiy the interface

<img src="/Users/xuzheng/Projects/notes/Go/conf_subtitle/Understanding nil.assets/image-20201208232040995.png" alt="image-20201208232040995" style="width:600px;" />





<img src="/Users/xuzheng/Projects/notes/Go/conf_subtitle/Understanding nil.assets/image-20201208232316147.png" alt="image-20201208232316147" style="width:600px;" />

