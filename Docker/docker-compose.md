# docker-compose

## docker-composeを使ってコンテナを起動

```yaml
# docker-compose.yaml
version: "3"
services:
  echo:
    image: example/echo:latest
    ports:
      - "9000:8080"
```

```bash
$ docker-compose up -d
```

これは以下で起動するのと同じ

```bash
$ docker container run -d -p 9000:8080 example/echo
```

## コンテナの停止

docker-compose.yamlで定義しているコンテナを全て停止する

```shell
$ docke-compose down
```

## docker-composeを使って、Dockerfileからコンテナを作成

```yml
# docker-compose.yaml
version: "3"
services:
  echo:
    build: . # Dockerfileが存在するディレクトリの相対パスを記載（imageの代わりにbuild属性を指定）
    ports:
      - "9000:8080"
```

```bash
# imageが作成済みの場合はビルドは省略されるが、Dockerfileの中身を書き換えたり、再度ビルドする必要がある場合は--buildをつける
$ docker-compose up -d --build
```

