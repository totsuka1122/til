## direnvのインストールについて

direnv ••• .envrcを作成し、そのディレクトリ内でのみ有効な環境変数を設定することができる。  

```
$ brew install direnv

# ~/bash_profile に以下を記述
eval "$(direnv hook bash)"

$ exec $SHELL -l
```

## サーバー起動 HelloWorld表示

```go
package main

import (
	"net/http"
)

func main() {
	http.HandleFunc("/", HelloHandler) // ハンドラ登録
	http.ListenAndServe(":8888", nil) //サーバ起動
}

// 処理内容
func HelloHandler(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Hello World"))
}

```

## HTMLファイルの表示

```go
package main

import (
	"fmt"
	"math/rand"
	"net/http"
)

func main() {
	// http.HandleFunc("/", DiceHandler)
	// http.ListenAndServe(":8888", nil)
	fs := http.FileServer(http.Dir("pub"))
	http.Handle("/", fs)
	http.HandleFunc("/dice", DiceHandler)
	http.ListenAndServe(":8888", nil)
}

func DiceHandler(w http.ResponseWriter, r *http.Request) {
	v := rand.Intn(6) + 1
	s := fmt.Sprintf("サイコロの目は、 %d", v)
	w.Write([]byte(s))
}
```
```
.
├── hello_server.go
└── pub
    └── index.html
```
