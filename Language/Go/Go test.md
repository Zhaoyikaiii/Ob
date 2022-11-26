Go语言原生自带测试

```go
import "testing"

func TestIncrease(t *testing.T) {
	t.Log("stating testing")
	increase(1,2)
}
```

go test ./... -v 运行测试
go test 命令扫描所有 `*_test.go` 为结尾的文件，管理是将测试代码与正式 diamagnetic 放在同目录
如 `foo.go` 的测试代码一般写在 `foo_test.go`

例如：
```go
func add(a,b int) int {
	return a + b
}

func TestIncrease(t *testing.T) {
	t.log("Start testing ")
	return := add(1,2)
	assert.Equal(t,result,3)
}
```

