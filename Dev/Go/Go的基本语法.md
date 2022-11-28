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

- 初始化语句和后置语句是可选的，此场景与 while 等价 (Go 语言不支持 while)
```go
for ;sum < 1000； {
	sum += sum
}
```

- 无限循环

```go
for {
	if condition1 {
		break
	}
}
```

## For-range

遍历数组，切片，字符串，Map 等
```go
for index,char := range myString {

}

for key,value := range MyMap {

}

for index,valu := range MyArray {

}
```

需要注意: 如果 for range 遍历指针数组，则 value 去除的指针地址为原指针地址的拷贝

