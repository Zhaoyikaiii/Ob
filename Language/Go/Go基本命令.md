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
	report likely mistakes in packages
	- Print-format 错误，检查类型不匹配的 print 
		- `str := "hello world!"`
		- `fmt. Printf ("%d\n", str)`
	- Boolean 错误，检查一直为 true, false 或者冗余的表达式
		- `fmt.Println(i != 0 || i != 1)
	- Range 循环，比如ru'xia
