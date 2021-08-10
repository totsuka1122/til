# base64

> BASE64とは、バイナリデータを一定の規則に基づいてテキスト（文字）データに置き換える変換方式の一つで、<br>
> 64種類の英数字のみを用いてデータを表現する方式。<br>
> 電子メールの添付ファイル（MIME）などでよく用いられる。

```go
package main

import (
    "encoding/base64"
    "fmt"
)

func main() {
    // encode
    src := []byte("Hello World")

    enc := base64.StdEncoding.EncodeToString(src)
    fmt.Println(enc) // SGVsbG8gV29ybGQ=

    // decode
    dec, err := base64.StdEncoding.DecodeString(enc)
    if err != nil {
        panic(err)
    }

    fmt.Println(string(dec)) // Hello World
}
```