## 練習問題

```
3クラスでテストを行いました。
以下の結果をもとに、「組の名前」「生徒名とその点数」「平均点」をクラスごとに出力してください。

花組
Aさんは80点、Bさんは90点、Cさんは100点

月組
Dさんは60点、Eさんは100点、Fさんは30点

雪組
Gさんは50点、Hさんは90点
```

```go
package main

// テスト結果です
type TestResult struct {
	homeroomName string
	point        map[string]int
	average      int
}

// テスト結果を作成します
func NewTest(class string, point map[string]int) TestResult {
	t := TestResult{}
	t.homeroomName = class
	t.point = point

	return t
}

// 平均点を計算します
func (t *TestResult) CalcAverage() {
	var sum int
	for _, v := range t.point {
		sum += v
	}

	t.average = s / len(t.point)
}

func main() {
	hana := map[string]int{
		"A": 80,
		"B": 90,
		"C": 100,
	}

	tsuki := map[string]int{
		"D": 60,
		"E": 100,
		"F": 20,
	}

	yuki := map[string]int{
		"G": 50,
		"H": 90,
	}

	h := NewTest("花組", hana)
	t := NewTest("月組", tsuki)
	y := NewTest("雪組", yuki)

	for _, v := range []TestResult{h, t, y} {
		v.CalcAverage()
		fmt.Printf("%+v\n", v)
	}
}
```

```
// 出力結果
{homeroomName:花組 point:map[A:80 B:90 C:100] average:90}
{homeroomName:月組 point:map[D:60 E:100 F:20] average:60}
{homeroomName:雪組 point:map[G:50 H:90] average:70}
```
