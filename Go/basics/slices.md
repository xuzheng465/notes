# Slices

## basics



You can't assign elements to a **nil slice**.



The zero value of slices is `nil`.



```go
games := []string{"kokemon", "sims"}

```

```go
var nums [] int
```

nums is `nil`

`len(nums)` is equal to 0.

It doesn't contain any Elements yet.



:exclamation: A slice's length is **unrelated** of its **type**.

*slice的长度与它的元素的类型无关*

:exclamation: A `slice` can only be compared to a **nil value**.

*slice只能与nil相比较*

:exclamation: `Slices` with the same **element type** can be **assigned** to each other; no matter what their lengths are.



:exclamation: Go won't execute a **range loop** if the given `slice` is nil. It's a nil-operation (no-operation)

:exclamation: 

An `Empty Slice` is not a `Nil Slice`.

An `Empty Slice` is an *initialized* slice.

A `Nil Slice` is an *uninitialized* slice.

Their lengths are the same: Zero



不要检查slice是否是`nil`

要去检查它的长度。

empty slice和 nil slice 的长度都是 Zero



### append

appends an element to a slice and returns a new slice



```go
nums := []int{1,2,3}
nums = append(nums, 4)
// saving it back to
nums = append(nums, 4, 9)

tens:= []int{12, 13}
nums = append(nums, tens...)
```

append returns a new slice.



It can append multiple elements -- append is a variadic function