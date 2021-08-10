# Docker

- コマンド一覧
  https://qiita.com/zembutsu/items/6e1ad18f0d548ce6c266



## 4. Data Volumeコンテナ

- コンテナ間でディレクトリを共有する
- ディスクに保存された永続化データの一部をVolumeとして別のコンテナに共有できるようにしたもの

# Sample

## golang

Docker Hub: https://hub.docker.com/_/golang  
Qiita: https://qiita.com/uji_/items/8c9eda89526abe0ba900

### Dockerfile for golang

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

## MySQL

```bash
$ docker pull mysql  
$ docker run --name {コンテナ名} -e MYSQL_ROOT_PASSWORD={mysqlのパスワード} -d {イメージ名}:{タグ名}
```

```bash
$ docker exec -it {コンテナ名} bash -p
# mysql -u root -p
```

- `--name`の後ろはコンテナ名
- `MYSQL_ROOT_PASSWORD=`の後ろはパスワード

## node

- Dockerfile

```Dockerfile
FROM node:10.13-alpine

RUN mkdir /working # 作業ディレクトリを作成

WORKDIR /working # 作業ディレクトリを指定

RUN npm update
RUN npm install -g ionic cordova

CMD ["sh"]　# デフォルトで node が起動するので sh を代わりに起動
```

- docker-compose.yml

```docker-compose.yml
version: '3'
services:
  node_app:
    build: . # Dockerfileのパス
    image: watashino-image # イメージ名を指定
    volumes:
    - ./working:/working # マウントするディレクトリを指定(ローカルパス:コンテナのパス)
    ports:
    - "8100:8100"
    tty: true
```

- tree

```
.
├── Dockerfile
├── docker-compose.yml
└── working
    └── test.js
```

- ビルドと実行

```
docker-compose build
docker-compose up -d
```
