# Git
- https://git-scm.com/

## インストール or アップデート
- 上記HPから、Downloads → os選択
- `$ git version`でバージョン確認

## 初期設定
保存場所：　ホームディレクトリ下の`.gitconfig` - `$ cat ~/.gitconfig`
- 登録
  - ユーザー登録： `$ git config --global user.name "githubに登録してあるユーザー名"`
  - アドレス登録： `$ git config --global user.email 自分のメールアドレス`
  - エディタの登録： `$ git config --global core.editor "エディタ名 --wait"`  
    `$ git commit`で指定のエディタを開く
- 確認
  - 全確認： `$ git config --list`
  - ユーザー確認： `$ git config user.name`
  - アドレス確認： `$ git config user.email`
  - エディタ確認： `$ git config core.editor`

## ローカルリポジトリ
 - `$ git init` → `.git`ディレクトリが作成される（objects-リポジトリの本体, config-設定ファイル, index-ステージングした後）
 
## git clone
- `$ git clone リポジトリ名`
