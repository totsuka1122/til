# github action

## runで複数コマンドを実行
```yaml
- run: |
  go vet ./...
  go fmt ./...
  go test ./...
```

## runでworking dirを指定して実行
```yaml
# ワーキングディレクトリを/tmpに変更してコマンド実行
- run: |
  pwd
  ls
 working-directory: /tmp
```

## envを設定
```yaml
name: CI
on: push

jobs:
  unit-test:
    name: Unit Test
    runs-on: ubuntu-latest
    
    # ここでenvを設定すると、このjob内で有効となる
    env:
      ID: abcd0123
    steps:
      - name: Setup Go 1.x
        uses: actions/setup-go@v2
        with:
          go-version: 1.16

      - name: Check out code into the Go module directory
        uses: actions/checkout@v2

      - name: test
        run: go test ./...
```

## 機密情報を取得したい場合
- settings > secrets に保存する

## jobを直列に扱いたい場合
jobは基本的に並列に実行されるが、以下のように書けば実行順序を制御できる

```yaml
jobs:
  unit-test:
    ...
  lint:
    ...
  deploy:
    needs: [unit-test, lint]
```
