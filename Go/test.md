# Goでテストカバレッジを表示する

## 1. オプションを付けてテストを実行
`-coverprofile=cover.out`オプションを付けてテストを実行する
```
# 例
$ go test -coverprofile=cover.out ./domain/...
```

## 2. htmlを作成
- オプションを付けてテストを実行すると、カレントディレクトリに`cover.out`ファイルが作成される
- 以下の`go tool`コマンドで、htmlファイルが作成される

```
$ go tool cover -html=cover.out

HTML output written to /tmp/cover270627159/coverage.html
```
- 該当のHTMLを開くと、未カバーの行が表示される
