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
