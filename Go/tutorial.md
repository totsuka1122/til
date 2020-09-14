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
func sum(a, b int) int {
    if v := a + b; v < 100 {
        return v
    }
    return v // Error
}
```

# Defer
deferへ渡した関数の実行を、呼び出し元の関数の終わり(returnする)まで遅延させる  
※関数が複数ある場合スタックされる
```
func main() {
    defer fmt.Println("World")
    fmt.Println("Hello")
}
```

# struct
goにはclassが存在しないため、struct(構造体)が使用される  
フィールドはドット(.)を用いてアクセスする
```
type Vertex struct {
    x int
    y int
}

func main() {
    v := Vertex{1, 2}
    v.x = 4
    fmt.Println(v.x)
}
```
以下の書き方もok
```
func main() {
    v := Vertex{x: 4, y: 6}
    fmt.Println(v)
}
```

# 配列
```
func main() {
    var a [2]string
    a[0] = "Hello"
    a[1] = "World"
    fmt.Println(a[0], a[1]) // Hello World
    fmt.Println(a) // [Hello World]
    
    primes := [6]int{2, 3, 5, 7, 11, 13}
    fmt.Println(primes) //[2, 3, 5, 7, 11, 13]
}
```

# for ~ range
1つ目の変数(i)はインデックス、2つ目の変数(v)は配列の値  
不要な変数は"_"にて置き換える
```
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("%d , %d\n", i, v)
	}
}
```

# map
連想配列
```
type Student struct {
    math int
    english int
}

var m map[string]Student

func main() {
    m = make(map[string]Student)
    m["taro"] = Student{60, 70}
    fmt.Println(m["taro"])
    
    // {60 70}
}
```
