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
	case val1: //空分支
	case val2 :
		fallthrough // 执行case3中的f()
	case val3:
		f()
	default://默认分支
		...
}
```
- 与其他语言不同的 Switch 默认自带 `break`。
- 如果想要继续往下面的 case 执行可以使用 `fallthrough ` 关键字。
- 如果每个分支都不匹配，执行 `default` 分支。

## For

Go 只有一种循环结构：for 循环。

 for 初始化语句；条件语句；修饰语句{}
```go
for i := 0 ; i < 10 ; i++ {
	sum += i
}
```

- 初始化语句和后置语句是可选的，此场景与 while 等价 (Go 语言bu'zhi'chi)