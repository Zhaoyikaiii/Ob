Go语言原生自带测试

```go
import "testing"

func TestIncrease(t *testing.T) {
	t.Log("stating testing")
	increase(1,2)
}
```

go test ./... -v 运行测试
go test 命令扫描所有