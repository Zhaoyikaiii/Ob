## Docker 在解决什么问题？

### 传统分层架构 VS 微服务

![[Pasted image 20221130190905.png]]


![[Pasted image 20221130191720.png]]

### 微服务改造

分离微服务的方法建议：
- 审视并发现可以分离的业务逻辑
- 寻找天生隔离的代码模块，可以借助静态代码分析工具
- 不同并发规模，不同内存需求的模块都可以分离出不同的微服务，此方法可提高资源利用率，节省成本
### 微服务间的通讯

- 点对点
	- 多用于系统内部多组件之间的通讯
	- 有大量的重复模块如认证授权
	- 缺少统一规范，如监控，审计等功能
	- 后期维护成本高，服务和服务的依赖管理错综复杂难以管理。 
- API 网关
	- 基于一个轻量级的 message gataway
	- 新 API 通过注册至 Gateway 实现
	- 整合实现 Common function 
	- ![[Pasted image 20221130192647.png]]

## [[Docker]] 
- 基于 Linux 内核的 Cgroup, Namespace, 以及 Union FS 等计数，对进程进行封装隔离，属于操作系统层面的虚拟化计数，由于隔离的进程独立于宿主和其它的隔离的进程，因此也称其为容器。
- 最初实现是基于 LXC, 从 0.7 之后开始去除 LXC, 转而使用自行开发的 Libcontainer, 从 1.11 开始，则进一步演进为使用 runC 和 Containerd。
- Docker 在容器的基础上，进行了进一步的封装，从文件系统，网络互联到进程隔离等待，极大的简化了容器的创建和维护，使得 Docker 技术比虚拟机技术更为轻便，快捷。

### 为什么要用 Docker

- 更高效地利用系统资源
- 更快读的启动时间
- 一致的运行环境
- 持续交付和部署
- 更轻松地迁移
- 更轻松地维护和拓展
- ...

### 虚拟机和容器运行态的对比

![[Pasted image 20221130194042.png]]


### 容器操作

- 启动
	- docker run
		- `-it` 交互
		- `-d`  后台运行 daemon
		- `-p`  端口映射 port
		- `-V`  磁盘挂载 volumn
	- 启动已终止容器 `docker start`
	- 停止容器 `docker stop`
	- 查看容器进程 `docker ps`

### 在运行 docker run 是发生了什么

以 `docker run -it centos bash` 为例
- docker 会现在本本地查看是否有 centos 这个镜像
	- 如果在本地没有这个镜像会运行 `docker pull centos` 从远端的 docker hub 上拉取镜像。(本身是一个 tar 包)
- 如果在本次有这个 Image 会在本地起一个 Container
- 之后执行 `bash`

###  容器操作

- 查看容器细节`docker inspect containerId`
- 进入容器
- Docker attach:
	- 通过 nsenter
		- `PID=$(docker inspect --format "{{.State.Pid}}" <container>`
		- `$nsenter --target $PID --mount -uts -ipc --net --pid `
- 拷贝文件 `docker cp file1 <containerid>:/file-to-path`

### 初识容器

- `cat Dockerfile`
```Dockerfil
FROM ubuntu
ENV MY_SERVICE_PORT = 80
EADD bin/amd64/httpserver /httpserver
ENTRYPOINT /httpserver
```

- 将 Dockerfile 打包成镜像

`docker build -t xxxx/xxxx:${tag}`
`docker push xxxx/xxx:vx.x`

- 运行容器

`docker run -d xxx/xxxx:vx:x`

### 容器标准

- Open Container Initiative (OCI)
	- 轻量级开发式管理组织（项目）
- OCI 主要定义两个规范
	- Runtime Specification
		- 文件系统包如何解压至硬盘，供运行时运行
	- Image Specification 
		- 如何通过构建系统打包，生成镜像菜单 (Manifest), 文件系统序列化文件，镜像配置。
		- 将之前虚拟机整个揉在一起的包，通过 Dockerfile 分层的方式，使得容器变得更有条理。每一层都有自己的 checksum 如果 checksum 是一致的会进行复用，拉取的时候只拉取动态的部分