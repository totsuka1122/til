## dockerコマンド一覧

https://qiita.com/zembutsu/items/6e1ad18f0d548ce6c266

- `docker run`  
  - `--rm`オプションをつけると、終了時にコンテナを削除する
  - `-d`オプションをつけると、常にバックグラウンドで起動するデタッチモードで起動できる


## golang

https://hub.docker.com/_/golang

### dockerfile for golang

```dockerfile
# Dockerfile
FROM golang:latest

RUN mkdir /go/src/work

WORKDIR /go/src/work

ADD . /go/src/work
```

```yaml
# docker-compose.yml
version: '3' # composeファイルのバーション指定
services:
  app: # service名
    build: . # ビルドに使用するDockerfileがあるディレクトリ指定
    tty: true # コンテナの起動永続化
    volumes:
      - .:/go/src/work # マウントディレクトリ指定
```
