# Kubernetesコマンド

## 基本的なコマンド

現在のバージョンを確認

```shell
kubectl version
```

コンテキスト一覧を確認

```shell
kubectl config get-contexts
```

現在のコンテキスト(クラスタ+ユーザー+名前空間)を確認

```shell
kubectl config current-context
```

コンテキストの切り替え

```shell
kubectl config use-context <コンテキスト名>
```

クラスタの切り替え(GKEのクラスタ一覧の右端の「・・・」から「接続」をクリック)  
※自分のクラスタのコマンドラインが表示されるため、以下は一例。

```shell
gcloud container clusters get-credentials <クラスタ名> --region asia-northeast1 --project <プロジェクト名>
```

Nodeの情報を確認

```shell
# -oは--outputでも可
kubectl get node -o wide
```

```shell
# 詳細を出力
kubectl describe node <Node名>
```

Podの一覧を取得

```shell
kubectl get pods
```

```shell
# ラベルを表示する場合
kubectl get pods --show-labels
```

```shell
# 詳細を表示する場合
kubectl get pod -o wide
```

```shell
# ラベルを指定して表示する場合
kubectl get pods -l <ラベル名>=<値1>,<値2>
```

```shell
# YAML形式でPodの詳細情報リストを出力
kubectl get pods -o yaml <Pod名>
```

```shell
# 詳細を出力
kubectl describe pod <Pod名>
```

Pod内のコンテナでコマンドを実行

```shell
kubectl exec -it <Pod名> -- /bin/bash
```

```shell
# 複数のコンテナが入ったPodの特定のコンテナでコマンドを実行
# コンテナを指定しないと、defaultのコンテナ(最初に立ち上げたコンテナ)が選択される
kubectl exec -it <Pod名> -c <コンテナ名> -- /bin/bash
```

レプリカセットの一覧を取得

```shell
kubectl get replicasets -o wide
```

## リソース関係のコマンド

リソースの作成(基本的にはapplyを使用)

```shell
# -f {path}　で対象のマニフェストを指定する。
kubectl create -f <yaml>
```

リソースの更新(リソースが存在しない場合には作成するため、createは基本的に使用せずapplyを使用する)

```shell
kubectl apply -f <yaml>
```

リソースの削除

```shell
# 削除は非同期で行われ、--waitをつけるとリソースの削除完了を待ってからコマンドが終了する
kubectl delete -f <yaml> [--wait]
```

リソース使用量を確認

```shell
# Nodeのリソース使用量を確認
kubectl top node
```

## ログの出力

Pod内のコンテナのログを出力

```shell
kubectl logs <Pod名>
```

```shell
# 複数コンテナが入ったPodでコンテナを指定してログを出力
kubectl logs <Pod名> -c <コンテナ名>
```

## gcloudコマンド

現在のログイン状況を確認

```shell
gcloud config list
```

ログイン

```shell
gcloud auth login
# エラーが出る場合は以下を試す
gcloud auth login --no-launch-browser
```

プロジェクトの変更

```shell
gcloud config set project <プロジェクトID>
```

## 参考サイト

- [フューチャー技術ブログ：GKE Autopilotを触ってみた](https://future-architect.github.io/articles/20210318/)
