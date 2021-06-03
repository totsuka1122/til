## JSONを整形して出力する

```go
package main

import (
	"bytes"
	"encoding/json"
	"fmt"
)

// サンプルの構造体
type Hoge struct {
	Name    string
	Value   int
	Content struct {
		Name  string
		Value int
	}
}

// JSONを整形して出力
func PrintJSON(b []byte) {
	var buf bytes.Buffer
	json.Indent(&buf, b, "", "  ")

	fmt.Println(buf.String())
}

func main() {
	// 適当に値を入れる
	h := Hoge{}
	h.Name = "Taro"
	h.Value = 3
	h.Content.Name = "pen"
	h.Content.Value = 5
	j, _ := json.Marshal(h)
	
	// 出力
	PrintJSON(j)
}
```

```
{
  "Name": "Taro",
  "Value": 3,
  "Content": {
    "Name": "pen",
    "Value": 5
  }
}
```
