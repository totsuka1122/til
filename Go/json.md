## JSONを整形して出力する

```go
func PrintJSON(b []byte) {
	var buf bytes.Buffer
	json.Indent(&buf, b, "", "  ")

	fmt.Println(buf.String())
}
```
