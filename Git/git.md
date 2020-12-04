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

## リモートリポジトリ(複数登録可)
  - `$ git remote add <リモート名(origin)> <URL>` - リモートリポジトリを新規作成
  - `$ git push <リモート名(origin)> <ブランチ名>` - `-u`を付けると、今後`git push`のみでokとなる
  - `$ git remote` - リモート情報の表示
  - `$ git remote -v` - 対応するURLを表示
  - `$ git remote show <リモート名>` - リモートの詳細情報を表示
  
#### ＊リモートから取得(fetch)
  - `$ git fetch <リモート名(origin)>` - リモートリポジトリからローカルリポジトリに取得(ワークツリーには未反映)
    - ↑`remotes/<リモート名>/<ブランチ名>`に保存
  - `$ git merge` - ローカルリポジトリに取得したものをワークツリーにコピー
  - `$ git branch -a` - ブランチの情報を表示

#### リモートから取得(pull)
  - `$ git pull <リモート名> <ブランチ名>` - `git fetch` + `git merge` = `git pull`
  
#### リモートを変更or削除
  - `$ git remote rename <旧リモート名> <新リモート名>` - リモート名の変更
  - `$ git remote rm <リモート名>` - リモートの削除
  
## 変更を元に戻す
- ファイルへの変更を取り消す（ワークツリーを作業前の状態に戻す）
  - `$ git checkout -- <ファイル名>` - 特定のファイル
  - `$ git checkout -- <ディレクトリ名>` - 特定のディレクトリ全て
  - `$ git checkout -- .` - 全て
- ステージに追加した変更を元に戻す(ワークツリーはそのままで、直前のコミットの内容をステージに上書き)
  - `$ git reset HEAD <ファイル名>` - 特定のファイル
  - `$ git reset HEAD <ディレクトリ名>` - 特定のディレクトリ
  - `$ git reset HEAD .` - 全て
- 直前のコミットを修正する
  - `$ git commit --amend` - 修正内容を再度コミットし、直前のコミットを上書きできる（リモートリポジトリにpushしたコミットは修正しない）

## ブランチとマージ

  
## .gitignore
  ```.gitignore
  # #から始まる行はコメント行
  
  # 指定したファイルを除外
  index.html
  
  # ルートディレクトリを指定
  /root.html
  
  # ディレクトリ以下を除外
  dir/
  
  # ワイルドカード「*」
  /*/*.css
  ```
