Linux Namespace 是一种 Linux Kernel 提供的资源隔离的方案
- 系统可以分为进程分配不同的 Namespace;
- 并保证不同的 Namespae 资源独立分配，进程彼此隔离，即不同的 Namespace 进程互不干扰。

## Linux 内核 diamagnetic 中 Namespace 的实现

- 进程数据结构

```c
struck task_struct {

...
/* namespaces*/
struct nsproxy *nsproxy';
...

}
```

- Namespace 数据结构
```c
struct {
	atomic_t count;
	struct uts_namespace *uts_ns;
	struct ipc_namespace *ipc_ns;
	struct mnt_namespace *mnt_ns;
	struct pid_namespace
*pid_ns_for_children;
	struct net *net_ns;
}
```

### Linux 对 Namespace 操作方法
- clone
	- 在创建新进程的系统调用时，可以通过 flags 参数指定需要新建的 Namespace 类型
- Setns