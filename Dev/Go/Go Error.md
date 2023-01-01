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

### Sentinel Error 

预定义的特定错误，我们叫为 sentinel error, 这个名字来源于计算机编程中使用一个特定值来表示不可能进行进一步处理的做法。所以对于 Go，我们使用特定的值来表示错误。
```go
if err == ErrSomething {...}
```

类似的 `io.EOF`，更底层的 `system.ENOENT`。

> 使用 sentinel 值是最不灵活的错误处理策略，因为调用方必须使用 == 将结果与预先声明的值进行比较。当您想要提供更多的上下文时，这就出现了一个问题，因为返回一个不同的错误将破坏相等性检查。
> 甚至时一些有意义的 fmt. Errorf 携带一些上下文，也会破坏调用者的==, 调用者将被迫查看 error. Error () 的方法输出，以查看它是否与特定的字符串匹配。

- 不依赖检查 `error.Error` 的输出。
> 不应该依赖检测 `error.Error` 的输出，Error 方法存在于 error 接口主要用于方便程序员使用但不是程序（编写测试可能会依赖这个返回）。这个输出的字符串用于记录日志，输出到 stdout.