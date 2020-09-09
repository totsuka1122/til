# rand/math
乱数の作成  
Seedで完全な乱数の作成
```
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())
    fmt.Println("My favarite number is", rand.Intn(10))
}
```

# 引数の省略
関数の２つ以上の引数が同じ型である場合には、最後の型を残して省略可。
```
func add(x, y int) int {
    return x + y
}
```