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
* 利用者が直接操作するものは全部で8種類のWorkloads Resource
  * Pod
  * Replication Controller
  * Replica Set
  * Deployment
  * Daemon Set
  * Stateful Set
  * Job
  * CronJob

### Discovery & LB Resource
* コンテナを外部公開するようなエンドポイントを提供するリソース
* 利用者が直接操作するものはServiceとIngress
  * Service
    * CluterIP
    * External IP
    * NodePort
    * LoadBalancer
    * Headless
    * ExternalName
    * None-Selector
  * Ingress

### Config & Storage Resource
* 設定、機密情報、永続化ボリュームに関するリソースでKey-Valueで保存されている
  * Srcret, 機密データ
  * ConfigMap 設定データ
  * Persistent Volume Chain


### Cluster Resource
* セキュリティやクォータなどに関するリソース
* クラスタ自体の振る舞いを定義する
  * Node
  * Namespace
  * Persistent Volume
  * Resource Quota
  * Service Account
  * Role
  * Cluster Role
  * Role Binding
  * Cluster Role Binding
  * Network Policy

### Metadata Resource
* クラスタ内の他のリソースを操作するためのリソース
* Auto Scaingなどに使われたりする
  * Limit Range
  * Horizontal Pod Autoscaler
  * Pod Disruption Budget
  * Custom Resource Definition

## Namespace
* Kubernetes内に仮想的にKubernetesクラスタを分離させる
* 一つのクラスタをProduction/Staging/Developmentに分割できる
* 利用者ごとにアクセスできる領域を分割する
* 初期条件では3つのNamespace
  * kube-system
    * kubernetesクラスタのコンポーネントやアドオンがdeployされる
  * kube-public
    * 全ユーザーが利用できる
    * ConfigMapを配置する
  * default
    * デフォルト
* RBAC: Role-Based Access ControlとNetwork Policyで利用者や接続できるPodを分ける

### 認証情報とConfig
* kubectlがKubernetes Masterと通信する際には `~/.kube/config` に書かれている情報を使用して接続を行う
* 具体的に設定を行うのは `clusters` `users` `contexts` の三種類
* 接続先のクラスタ、ユーザーの認証情報、clusterとuserのペアとnamespaceを指定したもの

### マニフェスト系の操作
* podの作成
  * `kubectl create -f {manifest.yaml}`
* pod一覧の取得
  * `kubectl get pods`
* podの削除
  * `kubectl delete -f {manifest.yaml}`
  * `kubectl delete -f {manifest.yaml} --grace-period 0 --force`
* マニフェストファイルの差分更新
  * `kubectl apply -f {manifest.yaml}`
  * リソースの作成にはapplyを使った方が好ましい
  * 基本的にcreateでは前回のマニフェストファイルが記憶されない

### マニフェストファイルの設計方法
* `./whole-system` 下にすべてのファイルをおく
* `./whole-system/subsystem-{name}` 下に関係のあるファイルをおく
* `./microservice-{name}` 下に関係のあるファイルをおく
