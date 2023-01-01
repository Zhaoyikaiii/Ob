Go error 就是一个普通的接口，普通的值
```go
type error interface {
	Error() string
}
```

我们经常使用 errors. New () 来返回一个 error 对象

```go
type errorString struct {
	s string
}

func (e *errorString) Error() string {
	return e.s
}
```

## Error VS Exception

Go 的处理异常逻辑时不引入 exception，支持 i 多参数返回，所以你很容易在函数签名中带上实现了 error interface 的对象，交由调用者来判断。

> 如果一个函数返回了（value, error), 你不能对这个 value 做任何假设，必须先判定 error。唯一可以忽略 error 的是，如果你连 value 也不关心。

Go 中有 panic 的机制，如果你认为和其他语言的 exception 一样，那你就错了。让我们抛出异常的时候，相当于你把 exception 扔给了调用者来处理。

对于真正意外的清空，那些表示不可恢复的程序错误，例如索引越界，不可恢复的环境问题，栈溢出，我们才使用 panic。对于其他的错误清空，我们应该是期望是使用 error 来进行判定。