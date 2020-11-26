# Error in Go



目录

* Error vs Exception
* Error Type
* Handling Error
* Go 1.13 errors
* Go 2 Error Inspection
* Resources
* 

如何包装error, 在一个地方

### 官方源代码

```go
package errors

type errorString struct {
  s string
}

func New(text string) error {
  return &errorString{text}
}

func (e *errorString) Error() string {
  return e.s
}
```



Go panic 意味着fatal error 就是挂了. 不能假设调用者来解决panic, 意味着代码不能继续运行.



```go
package syn
func Go(x func()) {
  if err:=recover(); err!=nil {
    
  }
  go x()
}
```













### Dave Cheney



### Sentinel errors (不要用)

* Never inspect the output of error.Error
  * 永远不要检测error.Error方法的输出. error接口中的Error方法为了程序员而存在, 而不是代码.

* Sentinel errors become part of your public API
* Sentinel errors create a ==dependency== between two packages



### Error types (也尽量不要用)

* 与错误值相比, 错误类型的一大改进是他们能够包装底层错误以提供更多上下文. 

* 调用者要使用类型断言和类型 *switch*，就要让自定义的 *error* 变为 public。这种模型会导致和调用者产生强耦合，从而导致 API 变得脆弱。结论是尽量避免使用 error types，虽然错误类型比 sentinel errors 更好，因为它们可以捕获关于出错的更多上下文，但是 error types 共享 error values 许多相同的问题, 因此，我的建议是避免错误类型，或者至少避免将它们作为公共 API 的一部分。













## Resources

