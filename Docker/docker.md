# Docker

- build

```sh
docker build -t <image名> <Dockerfileのディレクトリ>

# 例
docker build -t gin-image .
```

- conteiner

```sh
docker run -p 8080:8080 gin-image
```
