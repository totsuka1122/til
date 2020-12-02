## dockerコマンド一覧

https://qiita.com/zembutsu/items/6e1ad18f0d548ce6c266

- `--rm`オプションをつけると、終了時にコンテナを削除する
- `-d`オプションをつけると、常にバックグラウンドで起動するデタッチモードで起動できる
- `-a` ― 停止中のコンテナも表示する。
- `-s` ― ファイルサイズを表示する。
- `--name コンテナ名` ― コンテナ名を指定する。
- `-p ホストのポート番号:コンテナのポート番号` ― コンテナのポートをホストに公開する。コンテナ間の通信では不要。
- `--mount type=bind,src=ホストのディレクトリ,dst=コンテナのディレクトリ` ― バインドマウント。ホストのディレクトリをコンテナにマウントする。複数指定可能。
- `--mount type=volume,src=ボリューム,dst=コンテナのディレクトリ` ― ボリュームマウント。ボリュームをコンテナにマウントする。複数指定可能。
- `--volumes-from コンテナ` ― 指定したコンテナと同じボリュームにマウントする
- `-e` 環境変数名=値 ― 環境変数の設定
- `-dit` ― コンテナをバックグラウンドで実行させる(デタッチモード)。デタッチ中のログは`docker container logs`で確認できる。
- `-it` ― 標準入出力をコンテナに結びつける(キー入力する場合)。
- `-i` ― 標準入出力をコンテナに結びつける(キー入力しない場合)。
- `--rm` ― コマンドの実行が完了したとき、コンテナを破棄する。
- `-w` ― コンテナ内の作業ディレクトリを指定する。
- `--net` ― 接続するDockerネットワークを指定する。


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
$ docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:latest
```
- `--name`の後ろはコンテナ名
- `MYSQL_ROOT_PASSWORD=`の後ろはパスワード
