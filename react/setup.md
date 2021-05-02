# reactのsetup

- https://qiita.com/rspmharada7645/items/25c496aee87973bcc7a5
- nvmのドキュメントを参考にしながら以下のコマンドを実行。
- https://github.com/nvm-sh/nvm#install-script
```
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```
```
$ nvm --version
0.38.0
# command not found の場合はターミナル再起動
```
### node.js LTSのインストール
```
$ nvm install --lts
```
```
$ node -v
v14.16.1
```

### yarnのインストール
```
$npm install --global yarn
```
```
$ yarn --version
1.22.10
```
