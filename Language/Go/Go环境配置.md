- 下载GO https://golang.cn/dl/
- 在网站下载对应的二进制文件并安装
- 环境变量
	- GOROOT:
		- go的安装目录
	- GOPATH:
		- src:存放源代码
		- pkg:存放依赖包
		- bin：存放可执行文件
	- 其他常用变量
		- GOOS,GOARCH,GOPROXY
		- 在国内还需要配置：`goproxy:export GOPROXY=htpps://goproxy.cn`

- 需要注意的是，当前虽然当前已经出现`go mod`为了兼容之前的go程序。我们还是需要配置`GOPATH`

