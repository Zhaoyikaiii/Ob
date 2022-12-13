## 标准库启动 Web 服务

Go 语言内置了 `net/http` 库，封装了 HTTP 网络编程的基础接口，我们实现的 `Gee` Web 框架便是基于 `net/http` 的。我们接下来通过一个例子，简单介绍下这个库的使用。

```go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/",indexHandler)
	http.HandleFunc（“/hello",helloHandler）
	log.Fatal(http.ListenAndServe(":9999",nil)
}
func indexHandler(w http.ResponseWriter,req *http.Request) {
	for k,b := range req.Header {
		fmr.Fprintf(w,"Header[%q] = %q\n",k,v)
	}
}
func helloHandler(w http.ResponseWriter,req *http.Request) {
	for k,v := range req.Header {
		fmt.Fprintf(w,"header[%q]=%q\n",k,v)
	}
}
```

我们设置 2 个路由，`/` 和 `/hello`, 分别绑定了 `indexHandler` 和 `helloHandler`，根据不同的 HTTP 请求回调用不同的处理函数，访问 `/`，响应是 `URL.Path=/`, 而 `/hello` 的响应则是请求头 (header) 中的键值对信息。
![[Pasted image 20221213203716.png]]

在 main 函数的最后一行，是用来启动 Web 服务的，第一个参数是地址，`:9999` 表示在 9999 端口监听。而第二个参数则代表处理所有的 HTTP 请求的实例，`nil` 代表使用标准库中的实例处理。第二个参数，则是我们基于 `net/http` 标准库实现 Web 框架的入口.

## 实现 http. Handler 接口

```go
package http

type Handler interface {
	ServeHTTP(w ResponseWriter,r *Request)
}

func ListenAndServe(address string,h Handler) error
```

第二个参数的类型是什么呢？通过查看 `net/http` 的源码可以发现，`Handler` 是一个接口，需要实现方法 `ServeHTTP`, 也就是说只需要传入任何实现了 ServerHTTP 接口的实例，所有的 HTTP 请求，就都交给了该实例处理了。马上来试一试吧。

```go
package main 

import (
	"fmt"
	"log"
	"net/http"
)

type Engine struct{}

func (engine *Engine) ServeHTTP(w http.ResponseWriter,req *http.Request){
	switch req.URL.Path {
		case "/":
			fmt.FPrintf(w,"URL.Path = %q\n",req.URL.Path)
		case "/hello":
			for k,v := range req.Header {
				fmt.Fprintf(w,"Header[%q] = %q\n",k,v)
			}
		default:
			fmt.Fprintf(w,"404 NOT FOUND:%s\n",req.URL)
	}
}

func main() {
	engine := new(Engine)
	log.Fatal(http.ListenAndServe(":9999",engine))
}
```

- 我们定义了一个空的结构体 `Engine`，实现了方法 `ServeHTTP`。这个方法有两个参数，第二个参数是 Request, 该对象包含了该 HTTP 请求的所有的信息，比如请求地址/Header 和 Body 等信息；第一个参数是 ResponseWriter, 利用 ResponseWriter 可以构造针对该请求的响应。
- 在 main 函数中，我们给 ListenAndServe 方法的第二个参数传入了刚才创建的 `engine` 实例。至此，我们走出了实现 Web 框架的第一步，即，将所有的 HTTP 请求转向了我们自己的处理逻辑。还记得吗，在实现 `Engine` 之前，我们调用 http. HandleFunc 实现了路由和 Handler 的映射，也就是只能针对具体的路由处理逻辑。比如 `/hello`. 但是子啊实现 `Engine` 之后，我们拦截了所有的 HTTP 请求，拥有了统一的控制入口。在这里我们可以自由定义路由映射的规则，也可以统一添加一些处理逻辑，例如日志/异常处理等。
- 在吗的运行结果与之前的是一致的。

## Gee 框架的雏形

我们接下来重新组织上面的代码，搭建出整个框架的雏形。
最终的代码目录结构是这样的。
```md
gee/
	|--gee.go
	|--go.mod
main.go
go.mod
```

go. mod
```go
module examplte

go 1.13

require gee v0.0.0

replace gee => /.gee
```

- 在 `go.mod` 中使用 `replace` 将 gee 指向 `/.gee`

main. go

```go
package main

import (
	"fmt"
	"net/http"

	"gee"
)

func main() {
	r := gee.New()
	r.GET("/",func(w http.ResponseWriter,req *http.Request) {
		for k,v := range.Header {
			fmt.Fprintf(w,"Header[%q] = %q",k,v)
		}
	})
	r.Run(":9999")
}
```

看到这里，如果使用过 `gin` 框架的话，肯定会觉得无比的亲切。`gee` 框架的设计以及 API 也参考了 `gin`。使用 `New()` 创建 gee 的实例。使用 `GET()` 方法添加路由。最后使用 `Run()` 启动 Web 服务。这里的路由，知识静态路由，不支持 `/hello/:name` 这样的动态路由，动态路由我们在下次实现。

gee. go
```go
package gee 

import (
	"fmt"
	"net/http"
)

type HandlerFunc func(http.ResponseWriter ,req * http.Request)

type Engine struct {
	router map[sting]HandlerFunc
}

func New() *Engine {
	return &Engine(router: make(map[string]HandlerFunc))
}

func (engine *Engine) addRoute(method string,pattern string,handler HandlerFunc) {
	key := method + "-" + pattern 
	engine.router[key] = handler
}

func (engine *Engine) GET(pattern string,handler HandlerFunc) {
	engine.addRoute("GET",pattern,handler)
}

func (engine *Engine) POST(pattern string,handler HandlerFunc) {
	engine.addRoute("POST",pattern,handler)
}

func (engine *Engine) Run(addr string) (err error) {
	return http.ListenAndServe(addr,engine)
}

func (engine *Engine ServeHTTP(w http.ResponseWriter,req *http.Request) {
	key := req.Method + "-" + req.URL.Path
	if handler,ok := engine.router[key];ok {
		handler(w,req)
	} else {
		fmt.Fprintf(w,"404 NOT FOUND : %s\n",req.URL)
	}
})
```

那么 `gee.go` 就是重头戏了，我们重点介绍一些这部分的实现。
- 首先定义了类型 `HandlerFunc`, 这是提供给框架用户的，用来定义路由映射的处理方法。我们在 `Engine` 中，添加了一张路由映射表 `router`，key 由请求方法和静态路由地址构成，例如 `GET-/`, `GET-/hello`, `POST-/hello`, 这样针对相同的路由，如果请求方法不同可以映射到不同的处理方法 (Handler), value 是由用户映射的处理方法。
- 当用户调用 `(*Engine).GET()` 方法时，会将路由和处理方法注册到映射表 router 中，`（*Engine).Run()` 方法是对 ListenAndServe 的包装。
- Engine 实现的 ServeHTTP 的方法的作用就是，解析请求的路径，查找路由映射表，如果查到，就执行注册的处理方法。如果查不到，就返回 `404 NOT FOUND`
至此，整个 `Gee` 框架的原型以及出来了。实现了路由映射表，提供了用户注册静态路由的方法，包装了启动服务的函数。当然，到目前为止，我们还没有实现比 `net/http` 标准库更强大的能力，不用担心，很快就可以将动态路由，中间件等功能添加上去了。