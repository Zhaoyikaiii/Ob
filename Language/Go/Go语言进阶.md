## 线程加锁

### 理解线程安全

![[Pasted image 20221127213141.png]]
- CPU 速度相对于内存是非常快的，如果每次去读取内存中的值是效率是非常低的，所以现代的 CPU 架构体系中每一个 CPU 上都有自己的缓存机制，所以每次 CPU 读到内存中的值会先放到 CPU 的缓存中。
- 如果线程 1 先读取到内存中的值缓存到 CPU 中，线程 2 也读取到了进行了缓存。如果线程 1 修改了，会导致线程 1 和线程 2 的值不一致。

## 锁

- Go 语言不仅仅提供基于 CSP 的通讯模型，也支持基于共享内存的多线程数据访问。
- Sync 包提供了锁的基本原语
- `sync.Mutex` 互斥锁
	- `Lock()加锁，Unlock()解锁`
- `sync.RWMutex读写分离锁`
	- 不限制并发读，只限制并发写和并发读写
- `sync.WaitGroup`
	- 等待一组 `goroutine` 返回
- `sync.Once`
	- 保证某段代码只执行一次
- `sync.Cond`
	- 让一组 `goroutine` 在满足特定条件时被唤醒

```go

func safeWrite() {
	s := SafeMap {
		safeMap : map[int]int{},
		Mutex : Sync.Mutex{}
	}
	for i := 0 ; i < 100 ; i++ {
		go func() {
			s.Write(1,2)
		}()
	}
}

func(s *SafeMap*)Read(k int) (int , bool) {
	s.Lock()
	defer s.Unlock()
	result,ok := s.safeMap[k]
	return result,ok
}
```