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
