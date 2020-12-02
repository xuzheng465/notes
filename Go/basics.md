

字符串和数字互转

```go
// int to string
i2s := strconv.Itoa(i)
s2i, err := strconv.Atoi(i2s)
fmt.Println(i2s, s2i, err)
```



## slice

```go
var slice = []string{"a", "b", "c", "d"}
// 追加一个元素
slice2:=append(slice, "e")
// 追加多个元素
slice2 := append(slice2, "f", "g")
// 追加一个切片
slice2 := append(slice2, slice)
```



## String and []byte



```go
s:="Hello飞雪无情"

bs:=[]byte(s)

fmt.Println(bs)

fmt.Println(s[0],s[1],s[15])
fmt.Println(len(s)) // 17
// 一个汉子对应三个字节

fmt.Println(utf8.RuneCountInString(s)) // 9
```

