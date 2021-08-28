# Golandの設定

## IdeaVimでビープ音を鳴らさない方法

Windowsのホームディレクトリに

```
.ideavimrc
```

ファイルを作成し、以下を記述。

```
set visualbell
set noerrorbells
```

## コメントで構造体名から始まらないとエラーが出る場合

- 検索で`comment sentence`で検索すると出てくる
- editor > inspection > Go > エクスポートのコメント のチェックを外す(プロジェクト内だけでなく、Defaultで設定しておく)

## エディタの背景色を変更する方法

```
setting > editor > color scheme > general > text > default text > bachground > 色選択
```