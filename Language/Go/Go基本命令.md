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