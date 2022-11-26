## if

- 基本形式
```go
if condition1 {
 //do something
}else if condition2{
//do something else
}else {
//catch-all or default
}
```
- if 的简短语句
	- 同 for 一样, if 语句可以在条件表达式前执行一个简单的语句
```go
if v:= x - 100 ; v < 0 {
	return v
}
```

## Switch

```go
switch var1 {
	case var1: //空分支
	case var2 :
		fallthrough
}
```