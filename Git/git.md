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
  - `remotes/<リモート名>/<ブランチ名>`に保存
- `$ git merge` - ローカルリポジトリに取得したものをワークツリーにコピー
- `$ git branch -a` - ブランチの情報を表示

#### リモートから取得(pull)
- `$ git pull <リモート名> <ブランチ名>` - `git fetch` + `git merge` = `git pull`
- `$ git pull --rebase <リモート名> <ブランチ名>` - マージコミットが残らない(`git fetch` + `git rebase` = `git pull`)
- `$ git config --global pull.rebase true` - プルをリベース型として設定する
- `$ git config branch.master.rebase true` - masterブランチのみプルをリベース型として設定する
  
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
  - `$ git commit rebase -i <コミットID>` - 複数のコミットをやり直す(-i = --interactive)
  - 例: `$ git commit rebase -i HEAD~3` - 直前３つのコミットのやり直し  
  手順:  
    1. `git rebase -i`
    2. 修正したいコミットをeditにしてエディタ終了
    3. editのコミットのところでコミットの適用が止まる
    4. `git commit --amend`コマンドで修正
    5. `git rebase --continue`で次のコミットへいく
    6. (pickはそのままのコミット内容を適用して次へ行く)

## ブランチとマージ

#### 新規作成
- `$ git branch <ブランチ名>` - 新規ブランチ作成(作成のみで切り替えは行わない)
- `$ git branch` - ブランチの一覧を表示（ローカルリポジトリのみ）
- `$ git branch -a` -  ブランチの一覧を表示（リモートリポジトリも含む）
- `$ git log --oneline --decorate` - どのブランチがどのコミットを指しているのか確認

#### ブランチの切り替え・変更・削除
- `$ git checkout <既存ブランチ名>` - 既存のブランチに切り替える
- `$ git checkout -b <新ブランチ名>` - 新規ブランチを作成して切り替える
- `$ git branch -m <新ブランチ名>` - 現在のブランチ名を変更する
- `$ git branch -d <ブランチ名>` - ブランチを削除する(masterにマージされていない変更が残っている場合は削除しない)
- `$ git branch -D <ブランチ名>` - ブランチを強制削除する

#### マージ
- `$ git merge <ブランチ名>` - 作業中のブランチにマージ
- `$ git merge <リモート名/ブランチ名>`  

#### リベース
変更を統合する際に、履歴を整えるために使用。(他のブランチでの変更分を取り込む)  
!! GitHubにプッシュしたコミットをリベースすることはNG !!
- `$ git rebase <ブランチ名>` - HEADブランチが<ブランチ名>に統合される  
  → その後<ブランチ名>に切り替え、マージを行う
  
## タグ付け
- `$ git tag` - タグの一覧を表示
- `$ git tag -l <文字列>` - <文字列>を含んだタグを表示
- `$ git tag -a <タグ名> -m "<メッセージ>"` - 注釈付きタグを作成(最新のコミットに付与)
- `$ git tag <タグ名>` - 軽量版タグを作成
- `$ git tag <タグ名> <コミット名>` - 後からタグを付与
- `$ git show <タグ名>` - タグのデータと関連づけられたコミット等を表示
- `$ git push <リモート名> <タグ名>` - タグをリモートリポジトリに送信  
    ※ `git push origin master`では、タグは付与されない
- `$ git push <リモート名> --tags` - タグを一斉に送信する

## 一時保存
作業中でコミットしたくないけど、別のブランチで作業する必要がある場合等、一時保存が必要な場合
- `$ git stash` - `git stash save`も同意義
- `$ git stash list` - 避難した作業を一覧表示
- `$ git stash apply` - 最新の避難した作業を復元可能(ステージの内容は復元されない)
- `$ git stash --index` - ステージの内容も復元
- `$ git stash apply <スタッシュ名>` - 特定の作業を復元する(`git stash apply stash@{1}`等)
- `$ git stash drop` - 最新の作業を削除
- `$ git stash drop <スタッシュ名>` - 特定の作業を削除する
- `$ git stash clear` - 全作業を削除する

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
