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

### 基于 struct 的反射

```go
//struct
myStruct := T{A:"a"}
v1 := reflect.ValueOf(myStruct)
for i := 0 ; i < v1.NumField(); i ++ {
	fmt.Printf("Field: %d : %v\n",i,v1.Field(i))
} 
for i := 0; i < v1.NumMethod();i++ {
	fmt.Printf("Method: %d : %v",i,v1.Method(i))
}

// 需要注意receive 是struct还是指针
result := v1.Method(0).Call(nil)
fmt.Println("result",result)
```

## Go 语言中的面向对象编程

- 可见性控制
	- public - 常量，变量，类型，接口，结构，函数等的名称大写
	- Private - 非大写就只能在包内使用
- 继承
	- 通过组合实现，内嵌一个或多个 struct 
- 多态
	- 通过接口实现，通过接口定义方法集，编写多套实现

## JSON 编解码

- Unmarshal: 从 string 转换至 struct 
```go
func unmarshal2Struct(humanStr string)Human {
	h := HUman{}
	err := json.Unmarshal([]byte(humanStr),&h)
	if err != nil {
		println(err)
	}
	return h
}
```

- Marshal: 从 struct 转换至 string 
```go
func marshal2JsonString(h Human) string {
	h.Age = 30
	updatedBytes,err := json.Marshal(&h)
	if err != nil {
		println(err)
	}
	return string(updatedBytes )
}
```

- Json 包使用 map[stirng]interface{}和[]interface{}类型保存任意对象
- 可通过如下逻辑解析任意 json
```go
var obj interface{}
err := json.Unmarshal([]byte(humanStr),&obj)
objMap,ok := obj.(map[string]interface{})
for k,v := range objMap {
	switch value := v.(type) {
		case string:
			fmt.Printf("type of %s is string,value is %v\n",k,value)
		case interface{}:
			fmt.Pintf("type of %s is interface{},value is %v\n",k,value)
		default:
			fmt.Print("type of %s is wrong,value %v\n",k,value)
	}
}
```

## 错误处理

- Go 语言无内置 `exception` 机制，只提供 error 接口供定义错误
```go
type eror interface {
	Error() string
}
```
- 可通过 `errors.New` 或 `fmt.Errorf` 创建新的 `error`
	- `var errNotFound error = errors.New("NotFount")`
- 通常应用程序对 `error` 的处理大部分是判断 `error` 是否为 `nil`
如需将 `error` 归类，通常交给应用程序自定义，比如 kubernetes 自定义了与 apiserver 狡猾的不同类型错误

```go
type StatusError struct {
	ErrorStatus metav1.Status
}
```

## Defer 
- 函数返回之前执行某个语句或函数
- 常见的 defer 使用场景：
	- defer file. Close ()
	- `defer mu.Unlock()`
	- `defer println(""）`
```go
func loopFunc() {
	lock := sync.Mutex{}
	for i := 0 ; i < 3 ; i ++ {
		lock.lock()
		defer lock.Unlock() //defer语句是在程序退出时执行，并不是在当前循环结束时执行
		//可以通过闭包来解决
		fmt.Println("loopfunc:",i)
	}
}
```

```go
func loopFunc() {
	lock := sync.Mutex{}
	for i := 0 ; i < 3 ; i ++ {
		go func(i int) {
			lock.lock()
			defer lock.Unlock()
			fmt.Println("loopFunc:"，i)
		}(i)
	}
}
```

## Panic 和 recover 
- Panic: 可在系统出现不可恢复错误时主动调用 panic, panic 会使当前线程直接 crash 
- Defer: 保证执行并把控制权交还给接收到 panic 的函数调用者
- recover: 函数从 panic 或错误场景中恢复

```go
defer func() {
	fmt.Println("defer func is called")
	if err := recover();err != nil {
		fmt.Println(err)
	}
}()
panic("a panic is triggered")
```

-  可以通过 defer 和 recover 组合实现类似于 `try...catch...` 的效果
	- defer 保证的是在 crash 的时候执行一部分代码
	- recover  是在当前的 panic 状态中恢复过来

## 多线程

### 并发和并行

- 并发（concurrency)
	- 两个或多个事件在同一时间间隔发生
- 并行 (parallellism)
	- 两个或者多个事件在同一时刻发生。

### 协程

- 进程：
	- 分配系统资源 (cpu 时间，内存等) 基本单位
	- 有独立的内存空间，切换开销大
- 线程：
	- 进程的一个执行流，是 CPU 调度并能独立运行的基本单位
	- 统一进程中的多线程共享内存空间，线程切换代价小
	- 多线程通信方便
	- 从内核层面来看线程其实也是一种特殊的进程，它跟父进程共享了打开的文件和文件系统信息，共享了地址空间和信号处理函数
- 协程
	- Go 语言中轻量级线程实现
	- Golang 在 runtime, 系统调用等多方面对 goroutine 调度进行了封装和处理，当遇到长时间执行或者进行系统调度，会主动把当前 goroutine 的 CPU (P) 转让出去，让其他 goroutine 能被调度并执行，也就是 Golang 从语言层面支持了协程。

### Communicating Sequential Processing 

- CSP
	- 描述两个独立的并发实体通过共享的通讯 channel 并行通信的并发模型。
- Go 协程 goroutine 
	- 是一种轻量线程，它不是操作系统的线程，而是将一个操作系统线程分段使用，通过调度器实现协作式调度。
	- 是一种绿色线程，微线程，它与 Coroutine 协程也有区别，能够在发现堵塞后启动新的微线程。
- 通道 channel 
	- 类似 Unix 的 pipe, 用于协程之间通讯和同步。协程之间虽然解耦，但是它们和 Channel 有着耦合。

## 线程和协程的差异

- 每个 goroutine (协程) 默认占用内存比 Java, C 的先后曾少很多
	- goroutine:2kb
	- 线程：8MB
- 线程/goroutine 切换开销方面，goroutine 远比线程小
	- 线程: 设计模式切换 (从用户态切换到内核态)，16 个寄存器，PC, SP.... 等寄存器的刷新
	- goroutine: 只有三个寄存器的值修改 -PC/SP/DX。
- GOMAXPROCS
	- 控制并行线程数量

### 协程示例

- 启动新协程：go functionName ()

```go
for i:= 0 ; i < 10 ; i ++ {
	go fmt.Println(i)
}

time.Sleep(time.Second)
```

## Channel 多线程通信

- Channel 是多个协程之间通讯的管道
	- 一端发送数据，一段接收数据
	- 同一时间只有一个协程可以访问数据，无共享内存模式可能出现内存竞争
	- 协调协程的执行顺序
- 声明方式
	- `var identifier chan datatype`
	- 操作符 `<-`
- 示例：
```go
ch := make(chan int)
go func() {
	fmt.Println("hello from goroutine")
	ch <- 0 //数据写入channel
}()

i := <- ch //从Channel中获取数据并赋值
```

## 通道缓存

- 基于 Channel 的通信时同步的
- 当缓冲区满时，数据的发送时阻塞的
- 通过 make 关键字创建时可定义缓冲区容量，湖人缓冲区容量为 0

- 下面两个定义的区别？
	- ch := make (chan int) //这个没有给定默认的长度，默认为 0。
		- channel 默认有一个行为当缓冲区满时，这个 channel 的阻塞的 
	- ch := make (chan int, 1)

### 遍历通道缓冲区

```go
ch := make(chan int,10)
go func() {
	for i:= 0 ; i < 10 ; i ++ {
		rand.Seed(time.Now().UnixNano())
		n := rand.Intn(10)//n will be between 0 and 10
		fmt.Println("putting:",n)
		ch <- n
	}
	close(ch)
}()

fmt.Println("hello from main")
for v:= range ch {
	fmt.Println("receiving:",v)
}
```

## 单向通道

- 只发送通道
	- `var sendOnly chen <- int`
- 只接收通道
	- `var readOnly <- chen int`
- Istio webhook controller 
	- `func (w *WebhookCerPatcher) runWebhookController(stopChan <- chan struct{}){}`
- 如何用：双向通道转换
```go 
var c = make(chan int)
go prod(c)
go consume(c)
func prod(ch chan<- int) {
	for{
		ch <- 1
	}
}
func consume(ch <- chan int) {
	for{
		<-ch
	}
}
```

### 关闭通道

- 通道无需每次关闭
- 关闭的作用时告诉接收者该通道再无新数据发送
- 只有发送方需要关闭通道
```go
ch := make(chan int)
defer close(ch)
if v,notClosed := <- ch ; notClosed {
	fmt.Println(v)
}
```

### Select

- 当多个协程同时运行时，可通过 select 轮询多个通道
	- 如果所有通道都阻塞则等待，如定义了 default 则执行 Default 
	- 如多个通道就绪则随机选择
```go
select {
	case v:= <- ch1:
		...
	case v:= <- ch2:
		...
	default:
		...
}
```

定时器 Timer

- `time.Ticker` 以指定的时间间隔重复的向通道 C 发送时间值
- 使用场景
	- 为协程设定超时时间
```go
timer := time.NewTimer(time.Second)
select {
	//check normal channel
	case <- ch:
		fmt.Println('reveived from ch')
	case <- timer.C:
		fmt.Println("timeout waiting from channel ch")
}
```

## 上下文 Context

- 查找是，取消操作或者一些异常情况，往往需要进行抢占操作胡总和中断后续操作。
- Context 是设置截止日期，同步信号，传递请求相关值的结构体
```go
type Context interface {
	Deadline(){deadline time.Time,ok bool}
	Done() <- chan struct{}
	Err() error 
	Value(key interface{}) interface{}
}
```
- 用法
	- context. Background
		- Background 通常被用于主函数, 初始化以及测试中。作为一个顶层的 context, 也就是说一般我们创建的 context 都是基于 Background 
	- `context.TODO`
		- TODO 是在不确定使用什么 context 的时候才会使用
	- `context.WithDeadline`
		- 超时时间
	- `context.Withvalue`
		- 向 `context` 添加键值对
	- `context.WithCancel`
		- 创建一个可取消的 `context`

### 如何停止一个子协程
```go
done := make(chan bool)
fo func() {
	for {
		select {
			//在这里select监听这个channel如果channel停止会打印
			case <- done:
				fmt.Println("done channel is triggerred,exit child go routine")
				return
		}
	}
}()
close(done)
```

### 基于 Context 停止子协程

- Context 是 Go 语言对 go routine 和 timer 的封装
```go
ctx,cancel := context.WithTimeout(context.Background(),time.Second)
defer cancel()
go process(ctx,100*time.Millisecond)
<- ctx.Done()
fmt.Println("main:",ctx.Err())
```

```go
func main() {
	baseCtx := context.Background() // 这里创建了一个最顶层的context
	ctx := context.WithValue(baseCtx,"a","b") //这里往baseCtx中放入了一个key为a,value为b的值
	go func(c context.Context) {
		fmt.Println(c.Value("a"))
	}(ctx)//向这个goroutine放入了刚刚创建的context
	timeoutCtx,cancel := context.WithTimeout(baseCtx,time.Second)
	defer cancel()
	go func(xtx context.Context) {
		ticker := time.NewTicker(1 * time.Second)
		for _ = range ticker.C {
			select {
				case <- ctx.Done():
					fmt.Println("child process interrupt...")
					return 
				default:
					fmt.Println("enter default")
			}
		}
	}(timeoutCtx)
	select {
		case <- timeoutCtx.Done():
			time.Sleep(1 * time.Second)
			fmt.Prinln("main process exit")
	}

}
```