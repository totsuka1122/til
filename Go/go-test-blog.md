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
