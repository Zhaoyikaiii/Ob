Go语言原生自带测试

```go
import "testing"

func TestIncrease(t *testing.T) {
	t.Log("stating testing")
	increase(1,2)
}
```