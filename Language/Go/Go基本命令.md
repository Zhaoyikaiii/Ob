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
a := []int{}
b := []int{1,2,3}//假设b这片空间已经满了，再次插入会导致重新申请新的空间
c := a//初始化一个c使得它指向和a相同的地址
a = append(b,1) //向b中插入一个元素，会导致b的地址发生变化返回给a的就是一个新的地址
// 会导致c还是指向原来的地址，而a已经指向了新的地址
```

- 修改切片的值？
```go
mySlice :=[]int{10,20,30,40,50}
for _,value := range mySlice {
	//这里的value相当于一个临时变量，指向了这个数组遍历的值，go中是值传递
	value *= 2
}
fmt.Printf("mySlice %+v\n",mySlice)
for index,_ :=range mySlice {
	//需要通过下标去修改值
	mySlice[index] *= 2
}
```

## Map

```go
func main() {
	myMap := make(map[string]string,10)
	myMap["a"] = "b"
	myFuncMap := map[string]func() int {
		"funcA":func() int {return 1}
	}
	fmt.Println(myFuncMap)
	f := myFuncMap["funcA"]
	fmt.Println(f())
	value ,exist := myMap["a"]
	if exist {
		println(value)
	}
	for k,v := range myMap {
		println(k,v)
	}
}
```

## 结构体和指针

- 通过 `type ... struct` 关键字自定义结构体
- Go 语言支持指针，但不支持指针运算
	- 指针变量的值为内存地址
	- 未赋值的指针为 nil
```go
type MyType struct {
	Name string 
}

func printMyType(t *MyType) {
	println(t.Name)
}

func main() {
	t := MyType{Name:"test"}
	printMyType(&t)
}
```

## 结构体标签

- 结构体中的字段除了有名字和类型外，还可以有一个可选的标签 (tag)
- 使用场景：Kubernetes APIServer 对所有资源的的定义都是以 `json tag` 和 `protoBuff tag`
```go
type MyType struct {
	Name string `Json:"name"`
}

func main() {
	mt := MyType{Namse:"test"}
	myType := reflect.TypeOf(mt)
	name := myType.Field(0)
	tag := name.Tag.Get("json")
	println(tag)
}
```

## 类型别名

```go
// Service Type string describe ingress methods for a service 
type ServiceType string 

const (
	//ServiceTypeClusterIP means a service will only be accessible inside the cluster,via the ClusterIP
	ServiceTypeCLusterIPServiceType = "ClusterIP"
)
```

## Main 函数

- 类比 `java` ，`go` 中的关键字时通过 `os.Args` 传递过来的
- 每个 Go 语言程序都应该有个 main package 
- Main package 里的 main 函数是 Go 语言程序入口

## Init 函数

- init 函数：会在包初始化时运行
- 谨慎使用 `init` 函数
	- 当多个依赖项目引用统一项目，且被引用项目的初始化在 `init` 中完成，并且不可重复运行时，会导致启动错误。

## 返回值

- 多值返回
	- 函数可以返回任意数量的返回值
	- 多返回值的应用场景? 错误处理
- 命名返回值
	- Go 的返回值可被命名，它们会被视作定义在函数顶部的变量。
	- 返回值的名称应当具有一定的意义，它可以作为文档使用。
	- 没有参数的 return 语句返回已命名的返回值，会直接返回
- 调用者忽略部分返回值
	- `result,_ = strconv.Atoi(orgA)`

## 传递变长参数

Go 语言中的可变长参数允许调用方传递多个相同类型的参数
- 函数定义
`func append(slice []Type,elems ...Type)[] Type`

- 调用方法
`myArray := []string{}`
`myArray = append(myArray,"a","b","c")`

## 内置函数

- close：管道关闭
- len, cap：返回数组，切片，Map 的长度或容量
- `new`, `make` ： 内存分配
- `copy`, `append` : 操作切片
- `panic`, `recover` : 错误处理
- `print`, `println` : 打印
- `comples`, `real`, `imag` : 操作复数

## 回调函数 (Callback)

- 函数作为参数传入其他函数，并在其他函数内部调用执行
`strings.IndexFunc(line,unicode,IsSpace)`
`Kubernetes controller的leaderLection`

```go
func main() {
	DoOperation(1,increase)
	DoOperation(1,increase)
}

func increase(a,b int) {
	println("increase result is",a + b)
}

func DoOperation(y int,f func(int,int)) {
	f(y,1)
}
func decrease(a,b int) {
	println("decrease result is:",a-b)
}
```

## 闭包

- 匿名函数
	- 不能独立存在
	- 可以赋值给其他变量
		- `x := func(){}`
	- 可以直接调用
		- `func(x,y int){println(x+y)}(1,2)`
	- 可作为函数返回值
		- `func Add()(func(b int) int)`
```go
defer func() {
	if r := recover(); r != nil {
		println("recovered in FuncX")
	}
}()
```

## 方法

- 方法：作用在接收者上的函数
	- Func (recv receiver_type) methodName (parameter_list) (return_value_list)
- 使用场景
	- 很多场景下，函数需要的上下文可以保存在 receiver 属性中，通过定义 receiver 的方法，该方法可以直接访问 receiver 属性，减少参数传递需求
```go
func (s *Server)StartTLC() {
	ifs.URL != "" {
		panic("Server already started")
	}
	if s.client == nil {
		s.client = &http.CLient{Transport: &http.Transport{}}
	}
}
```

## 传值还是传指针

- Go 语言只有一种规则：传值
- 函数内修改参数的值不会影像到函数外原始变量的值
- 可以传递指针参数将变量地址传递给调用函数，Go 语言会复制该指针作为函数内的地址，但指向同一地址

## 接口

- 定义一组方法集合
```go
type IF interface {
	Method1(param_list) return_type
}
```
- 适用场景: Kubernetes 中有大量的接口抽象和多种实现
- Struct 无需显示声明实现 interface, 只需直接实现方法
- Struct 除实现 Interface 定义的接口外，还可以有额外的方法
- 一个类型可以实现多个接口
- Go 语言中接口不接受属性定义
- 接口可以嵌套其他接口

注意事项
- Interface 是可能为 `nil` 的，所以针对 `interface` 的使用一定要预先判空，否则会引起程序 `crash(nil panic)`
- Struct 初始化意味着空间分配，对 struct 的引用不会出现空指针 

## 反射机制

- `reflect.TypeOf()` : 返回被检查对象的类型
- `reflect.ValueOf()` : 返回被检查对象的值
- 示例
```go
myMap := make(map[string]string,10)
myMap["a"] = "b"
t := reflect.TypeOf(myMap)
fmt.Println("type:",t)
v := reflect.ValueOf(myMap)
fmt.Println("value:",v)
```