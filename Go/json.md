## JSONを整形して出力する

```go
package main

import (
	"bytes"
	"encoding/json"
	"fmt"
)

type Hoge struct {
	Name    string
	Value   int
	Content struct {
		Name  string
		Value int
	}
}

func main() {
	h := Hoge{}
	h.Name = "Taro"
	h.Value = 3
	h.Content.Name = "pen"
	h.Content.Value = 5
	j, _ := json.Marshal(h)

	PrintJSON(j)
}

func PrintJSON(b []byte) {
	var buf bytes.Buffer
	json.Indent(&buf, b, "", "  ")

	fmt.Println(buf.String())
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
