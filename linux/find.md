# findコマンド

- 現在の階層以下のファイルを検索する（ファイル名は正規表現可）

```sh
find -name <ファイル名>

# e.g.
find -name app.go
find -name *.go
```

- 特定のディレクトリ以下のファイルを検索する（ディレクトリ名はカレントディレクトリの階層から）

```sh
find <ディレクトリ名> -name <ファイル名>

# e.g.
find app -name app.go
find app -name *.go
```
