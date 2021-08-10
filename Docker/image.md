# イメージ

## Dockerfileからイメージをビルド

- イメージを作成することを"ビルド"と言う。

| インストラクション | 説明 |
| ---- | ---- |
| FROM | Dockerイメージのベースを指定 |
| RUN | Dockerイメージビルド時にDockerコンテナ内で実行するコマンドを定義 |
| CMD | コンテナ起動時に1度実行される |
| LABEL | イメージの作者名記入などに使用 |
| ENV | 生成したDockerコンテナ内で使用できる環境変数 |
| ARG | ビルド時に情報を埋め込むために使用し、ビルド時だけ使用できる一時的な環境変数 |

```Dockerfile
# Dockerfile
FROM golang:1.9

RUN mkdir /echo
COPY main.go /echo

CMD ["go", "run", "/echo/main.go"]
```

Dockerfileと同階層で`docker image build`コマンドを実行する。

```bash
$ docker image build -t イメージ名:[タグ名※省略時は"latest"] [Dockerfileのディレクトリパス]

# (例)
$ docker image build -t hoge:latest .
```

#### オプション

- `-f`
    - `docker image build` コマンドは、デフォルトでDockerfileを探しに行くが、Dockerfile以外のファイル名を参照したい場合は`-f`で指定する。

```sh
# 例
$ docker image build -f Dockerfile-test -t example/echo:latest .
```

- `--pull`
    - 一度リモートから取得したFROMで指定したイメージは削除しない限りホストに保持されるが、`--pull=true`を指定すると毎回取得する。

```sh
# 例
$ docker image build --pull=true -t example/echo:latest .
```

---

## Docker Hubからイメージを取得

https://hub.docker.com/

```bash
# 例はmysqlのイメージ取得
$ docker pull mysql
```

---

## イメージの削除

イメージの削除

```
$ docker rmi <イメージID>
```

不要なイメージを一括削除  
※いくつか残ることがあるが、これらはDockerが判断して残している。何かしらの理由がある。

```
$ docker image prune

# 使用していない全てのDockerリソースを削除する場合は、以下を実行
$ docker system prune
```

