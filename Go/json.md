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

## mapを整形して出力する

```go
package main

import (
	"encoding/json"
	"os"
)

func PrintMap(m map[string]interface{}) {
	e := json.NewEncoder(os.Stdout)

	e.SetIndent("", "  ")
	e.Encode(m)
}

func main() {
	m := map[string]interface{}{
		"Name":  "Taro",
		"Value": 3,
		"Content": map[string]interface{}{
			"Name":  "pen",
			"Value": 5,
		},
	}

	PrintMap(m)
}
```

```
{
  "Content": {
    "Name": "pen",
    "Value": 5
  },
  "Name": "Taro",
  "Value": 3
}
```

## 構造体を整形して出力
```go
package main

import (
	"encoding/json"
	"os"
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

// 構造体を整形して出力
func PrintStruct(i interface{}) {
	e := json.NewEncoder(os.Stdout)

	e.SetIndent("", "  ")
	e.Encode(i)
}

func main() {
	// 適当に値を入れる
	h := Hoge{}
	h.Name = "Taro"
	h.Value = 3
	h.Content.Name = "pen"
	h.Content.Value = 5

	PrintStruct(h)
}
```
