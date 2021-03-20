# dockerコマンド一覧

https://qiita.com/zembutsu/items/6e1ad18f0d548ce6c266

## 1. イメージの作成
### (a) Dockerfileからイメージ作成
```Dockerfile
# Dockerfile
FROM golang:1.9

RUN mkdir /echo
COPY main.go /echo

CMD ["go", "run", "/echo/main.go"]
```
```bash
$ docker image build -t <イメージ名>:<タグ名※省略時は"latest"になる> <Dockerfileのディレクトリパス>
```
### (b) Docker Hubからイメージを取得
https://hub.docker.com/
```bash
# 例はmysqlのイメージ取得
$ docker pull mysql
```

### (c) イメージの削除
イメージの削除
```bash
$ docker rmi <イメージID>
```

不要なイメージを一括削除  
※いくつか残ることがあるが、これらはDockerが判断して残している。何かしらの理由がある。
```bash
$ docker image prune

# 使用していない全てのDockerリソースを削除する場合は、以下を実行
$ docker system prune
```

## 2. コンテナの起動
### (a) dockerコマンドでコンテナを起動
```bash
# ホスト側ポートにアクセス可
$ docker container run -d -p <ホスト側ポート>:<コンテナ側ポート> <イメージ名>:<タグ名>
# 起動時にシェルに入る(-it)
$ docker container run -it <イメージ名>
```

### (b) docker-composeを使ってコンテナを起動
```yml
# docker-compose.yml
version: "3"
services:
  echo:
    image: example/echo:latest
    ports:
      - 9000:8080
```
```bash
$ docker-compose up -d
```
これは以下で起動するのと同じ
```bash
$ docker container run -d -p 9000:8080 example/echo
```

### (b-1) docker-composeを使って、Dockerfileからコンテナを作成

```yml
# docker-compose.yml
version: "3"
services:
  echo:
    build: . # Dockerfileが存在するディレクトリの相対パスを"build"に記載（imageの代わりにbuild属性を指定）
    ports:
      - 9000:8080
```
```bash
# imageが作成済みの場合はビルドは省略されるが、Dockerfileの中身を書き換えたり、再度ビルドする必要がある場合は--buildをつける
$ docker-compose up -d --build
```

### (c) コンテナの削除
コンテナを削除
```bash
$ docker container rm <コンテナIDまたはコンテナ名>
```

実行していないコンテナを一括削除
```bash
$ docker container prune
```


## 3. オプション
- `-d`オプションをつけると、常にバックグラウンドで起動するデタッチモードで起動できる
- `-a` ― 停止中のコンテナも表示する。
- `-s` ― ファイルサイズを表示する。
- `--name コンテナ名` ― コンテナ名を指定する。
- `-p ホストのポート番号:コンテナのポート番号` ― コンテナのポートをホストに公開する。コンテナ間の通信では不要。
- `--mount type=bind,src=ホストのディレクトリ,dst=コンテナのディレクトリ` ― バインドマウント。ホストのディレクトリをコンテナにマウントする。複数指定可能。
- `--mount type=volume,src=ボリューム,dst=コンテナのディレクトリ` ― ボリュームマウント。ボリュームをコンテナにマウントする。複数指定可能。
- `--volumes-from コンテナ` ― 指定したコンテナと同じボリュームにマウントする
- `-e 環境変数名=値` ― 環境変数の設定
- `-dit` ― コンテナをバックグラウンドで実行させる(デタッチモード)。デタッチ中のログは`docker container logs`で確認できる。
- `-it` ― 標準入出力をコンテナに結びつける(キー入力する場合)。
- `-i` ― 標準入出力をコンテナに結びつける(キー入力しない場合)。
- `--rm` ― コマンドの実行が完了したとき、コンテナを破棄する。
- `-w` ― コンテナ内の作業ディレクトリを指定する。
- `--net` ― 接続するDockerネットワークを指定する。

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
