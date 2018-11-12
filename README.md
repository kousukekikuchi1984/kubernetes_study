# kubernetes_study

## Kubernetes
### Kubernetes Master
* APIエンドポイントの提供
* コンテナのスケジューリング
* コンテナのスケーリングなど

### Kubernetes Node
* Dockerのホスト＝実際にコンテナが動く部分

### 動き方
* Kubernetesクラスタを操作する際には、CLIツールの `kubectl` とYAML形式のマニフェストファイルを用いてKubernetes Masterにリソースの登録を行う
* Kubernetes MasterにあるAPIリクエストを送り、Kubernetesの操作を行う
* このAPIはREST APIなので、直接扱うことも可能

## Kubernetes Resource
### Workloads Resource
* コンテナの実行に関するリソース

### Discovery & LB Resource
* コンテナを外部公開するようなエンドポイントを提供するリソース

### Config & Storage Resource
* 設定、機密情報、永続化ボリュームに関するリソース

### Cluster Resource
* セキュリティやクォータなどに関するリソース

### Metadata Resource
* クラスタ内の他のリソースを操作するためのリソース



