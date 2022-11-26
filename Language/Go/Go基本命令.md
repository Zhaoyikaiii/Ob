bug
	start a bug report
build     
	compile packaged and dependencied 
	`go build main.go`会将main.go编译为可执行文件直接执行：`./main`
	也可以通过`GOOS=linux`指定要编译的目标系统:`GOOS=linux go build main.go`
clean     
	remove object files and cached files
doc       
	show documentation for packaged or symbol
env       
	print Go environment information
fix         
	update packages to use new APIs
fmt       
	gofmt(reformat) package sources
	
generate 
	generate go files by processing source
get       
	add dependencies to current module and install them
install
	compile and install packages and dependencies
list 
	lis packages or modules
mod
	module maintenance
run
	compile and run Go program
test
	test package
tool
	run specified go tool
version
	print go version
vet
	report likely mistakes in packages, 代码静态检查，发现可能的 bug 或者可疑的构造
	- Print-format 错误，检查类型不匹配的 print 
		- `str := "hello world!"`
		- `fmt. Printf ("%d\n", str)`
	- Boolean 错误，检查一直为 true, false 或者冗余的表达式
		- `fmt.Println(i != 0 || i != 1)
	- Range 循环，比如如下代码主协程会先退出，go routine 无法被执行 
```go
words := [] stirng{"foo","bar","baz"}
for _,word := range words {
	go func() {
		fmt.Println(word)
	}()
}
```

```go
func main() {
	// 第一个参数表示要取的参数key,第二个参数表示的是这个参数的默认值，第三个参数是一个提示是usage
	name := flag.String("name","world","specify the name you want to say hi")
	flag.Parse()
	fmt.Println("os args is:",os.Args)
	fmt.Println("input parameter is:",*name)
	fullString := fmt.Sprintf("hello %s from Go\n",*name)
	fmt.Println(fullString)
}
```

## 变量与常量

- 变量
	- `const identifier type`
	- `var identifier type`

### 变量定义

- 变量
	- var 语句用于声明一个变量列表，跟函数的参数列表一样，类型在最后
	- Var c, python, java bool
- 变量的初始化
	- 变量声明可以包含初始值，每个变量对应一个。
	- 如果初始化已存在，则可以省略类型；变量会从初始值中推断类型。
-  短变量声明
	- 在函数中，简洁赋值语句 `:=` 可在类型明确的地方代替 `var` 声明
	- 函数外的每个语句都必须以关键字开始 (var, func 等等)，因此 `:=` 结构不能在函数外使用。 


## 类型转换与推导

-  类型转换
	- 表达式 `T(v)` 将值 v 转换为类型 `T`
		- 一些关于数值的转换：
			- `var i int = 42`
			- `var f float64 = float64(i)`
			- `var u uint = uint(f)`
		- 或者，更加简单的形式:
			- `i := 42`
			- `f := float64(i)`
			- `u := uint(f)`
- 类型推导
	- 在声明一个变量而不知道其类型时（即使用不带类型的 `:=` 语法或 `var :=` 表达）
		- `var i int`
		- `j := i //也是一个int`


## 数组


- 相同类型且长度固定连续内存片段
- 以下标访问每个元素
- 定义方法
	- `var identifier [len]type`
- 示例
	- `myArray := [3]int{1,2,3}`

## 切片（slice）

- 切片是对数组一个连续片段的引用
- 数组定义中不指定长度即位切片
	- `var identifier []type`
- 切片在未初始化之前默认为 nil, 长度为 0
```go
func main() {
	myArray :=[5]int{1,2,3,4,5}
	mySlice := myArray[1:3]
	fmt.Printf("mySlice %+v\n",mySlice)
	fullSlice := myArray[:]
	remove3rdItem := deleteItem(fullSlice,2)
	fmt.Printf("remove3rdItem %+v\n",remove3rdItem)
}

func deleteItem(slice []int,index int)[]int {
	return append(slice[:index],slice[index+1:]...)
}
```

## Make 和 New

- New 返回指针地址
- Make 返回第一个元素，可预设内存空间，避免未来的内存拷贝
- 示例:
```go
mySlice1 := new([]int)
mySlice2 := new([]int,0)
mySlice3 := make([]int,10)
mySlice4 := make([]int,10,20)
```

### 关于切片的常见问题

- 切片是 i 连续内存并且可以动态拓展，由此引发的问题？
```go
a := []int
b := []int{1,2,3}
c := a
a = append(b,1)
```

- 修改切片的值？
```go
mySlice :=[]int{10,20,30,40,50}
for _,value := range mySlice {
	value *= 2
}
```