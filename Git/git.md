# Git
- https://git-scm.com/

## インストール or アップデート
- 上記HPから、Downloads → os選択
- `$ git version`でバージョン確認

## 初期設定
保存場所：　ホームディレクトリ下の`.gitconfig` - `$ cat ~/.gitconfig`
- 登録
  - ユーザー登録： `$ git config --global user.name "<githubに登録してあるユーザー名>"`
  - アドレス登録： `$ git config --global user.email <自分のメールアドレス>`
  - エディタの登録： `$ git config --global core.editor "<エディタ名> --wait"`  
- 確認
  - 全確認： `$ git config --list`
  - ユーザー確認： `$ git config user.name`
  - アドレス確認： `$ git config user.email`
  - エディタ確認： `$ git config core.editor`

## 基礎
  - `$ git init` → `.git`ディレクトリが作成される（objects-リポジトリの本体, config-設定ファイル, index-ステージングした後）
  - `$ git status` → 現在の状態確認
  - `$ git clone <リポジトリ名>`
  - `$ git add <ファイル名/ディレクトリ名/.>`
  - `$ git commit` - 指定のエディタを開く
  - `$ git commit -v` - ファイルの変更内容を確認する
  - `$ git diff` - git addする前の変更分(ワークツリーとステージの差分)
  - `$ git diff <ファイル名>` - git addする前の変更分(ワークツリーとステージの差分)
  - `$ git diff --staged` - git addした後の変更分(ステージとリポジトリの差分)
  - `$ git log` - 変更履歴の表示(1つ前のコミットとの差分を表示)
  - `$ git log --oneline` - 1行で表示する
  - `$ git log -p <ファイル名>` - ファイルの変更差分を表示する
  - `$ git log -n <コミット数>` - 表示するコミット数を制限する
  - `$ git rm <ファイル名>` - コミットされた記録が削除 & ワークツリーも削除
  - `$ git rm -r <ディレクトリ名>` - コミットされた記録が削除 & ワークツリーも削除
  - `$ git rm --cached <ファイル名>` - ワークツリーのファイルは残したい時（gitの記録からだけ削除)
  - `$ git mv <旧ファイル> <新ファイル>` - ワークツリーのファイル名を変更し、ステージにも変更を追加
  - `$ git config --global aloas.<ショートカット> <コマンド>` - そのコマンドのショートカットを作成できる
    -`$ git config --global alias.ci commit`
    -`$ git config --global alias.st status`
    -`$ git config --global alias.br branch`
    -`$ git config --global alias.co checkout`

## リモートリポジトリ
  - `$ git remote add origin <URL>` - リモートリポジトリを新規追加
  - `$ git push origin <ブランチ名>` - `-u`を付けると、今後`git push`のみでokとなる
  
## .gitignore
  ```
  # #から始まる行はコメント行
