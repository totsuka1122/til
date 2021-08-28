# 設定
## kubectl インストール
https://kubernetes.io/ja/docs/tasks/tools/install-kubectl/

# コンポーネント
- **コントロールプレーンコンポーネント**
  - kube-apiserver
  - etcd
  - kube-scheduler
  - kube-controller-manager
  - cloud-controller-manager
  
- **ノードコンポーネント**
  - kubelet
  - kube-proxy
  - コンテナランタイム
  
https://kubernetes.io/ja/docs/concepts/overview/components/
![components-of-kubernetes](https://user-images.githubusercontent.com/70890327/108790029-0d766b80-75bf-11eb-8706-23a9d7567038.png)

# リソース
- Node
  - ワーカーノード： コンテナランタイム・kubelet・kube-proxy osを含む
- Pod
  - k8sの最小単位
  - 同じPod内で同じポート番号は使用不可
- ReplicaSet
  - Podのレプリカを作成し、指定した数のPodを維持し続けるリソース
  - 障害時に自動で復旧してくれる
<img width="889" src="https://user-images.githubusercontent.com/70890327/108791428-6562a180-75c2-11eb-95e5-02955ffebc46.png">  

- Deployment
  - 複数のReplicaSetを管理してローリングアップデートやロールバックなどデプロイを管理するリソース
<img width="595" src="https://user-images.githubusercontent.com/70890327/108791579-c1c5c100-75c2-11eb-88b6-e76fa91a87cb.png">  

- Service
  - Podへのエンドポイントを割り当てたり、負荷分散を行うためのリソース
  - ClusterIP
  - NodePort
  - LoadBalancer
