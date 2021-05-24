## 練習問題

```
3グループでテストを行いました。
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

import "fmt"

// グループです
type Group struct {
	name       string
	testResult TestResult
}

// テスト結果です
type TestResult struct {
	point   map[string]int
	average int
}

// グループを作成します
func NewGroup(name string, tr TestResult) Group {
	g := Group{}
	g.name = name
	g.testResult = tr

	return g
}

// テスト結果を作成します
func NewTestResult(point map[string]int) TestResult {
	t := TestResult{}
	t.point = point

	return t
}

// 平均点を計算します
func (t *TestResult) CalcAverage() {
	var s int
	for _, v := range t.point {
		s += v
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

	h := NewTestResult(hana)
	hh := NewGroup("花組", h)

	t := NewTestResult(tsuki)
	tt := NewGroup("月組", t)

	y := NewTestResult(yuki)
	yy := NewGroup("雪組", y)

	for _, v := range []Group{hh, tt, yy} {
		v.testResult.CalcAverage()
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

## Fizz Buzz モデリングレッスン

```go
package main

import "fmt"

type Number struct {
	value int
}

type Card struct {
	number Number
}

type Holder struct {
	card []Card
}

type Reader struct {
	holder Holder
}

func NewNumber(i int) Number {
	n := Number{}
	n.value = i

	return n
}

func NewCard(n Number) Card {
	c := Card{}
	c.number = n

	return c
}

func (h *Holder) AddCard(c Card) {
	h.card = append(h.card, c)
}

func (r Reader) Print() {
	for _, v := range r.holder.card {
		switch {
		case v.number.value%3 == 0:
			fmt.Println("Fizz")
		case v.number.value%5 == 0:
			fmt.Println("Buzz")
		case v.number.value%15 == 0:
			fmt.Println("FizzBuzz")
		default:
			fmt.Println(v.number.value)
		}
	}
}

func main() {
	h := Holder{}

	for i := 1; i <= 100; i++ {
		n := NewNumber(i)
		c := NewCard(n)

		h.AddCard(c)
	}

	r := Reader{}
	r.holder = h
	r.Print()
}
```
