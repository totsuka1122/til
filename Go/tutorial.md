# rand/math
乱数の作成  
Seedで完全な乱数の作成
```
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

# 変数の宣言

`var x int`  
`var x = 1`  
`x := 1`※関数の外で定義できない

# 変数と定数

変数・・・途中で書き換え可  
定数・・・途中で書き換え不可  

```
// 変数
var x int = 11
x = 12
fmt.Println(x) // 12

// 定数
const Pi = 3.14
Pi = 3
fmt.Println(Pi) //Error: cannot assign to Pi
```

# if 条件の前に簡単なステートメントの記述
for同様、条件の前に簡単なステートメントの記述が可能  
※ifスコープ内のみ有効
```
func sum(a, b int) {
    if v := a + b; v < 100 {
        return v
    }
    return v // Error
}
```
