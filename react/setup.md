# reactのsetup

- https://qiita.com/rspmharada7645/items/25c496aee87973bcc7a5
- nvmのドキュメントを参考にしながら以下のコマンドを実行。
- https://github.com/nvm-sh/nvm#install-script

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```

```bash
$ nvm --version
0.38.0
# command not found の場合はターミナル再起動
```

### node.js LTSのインストール

```bash
$ nvm install --lts
```

```bash
$ node -v
v14.16.1
```

### yarnのインストール

```bash
$npm install --global yarn
```

```bash
$ yarn --version
1.22.10
```
