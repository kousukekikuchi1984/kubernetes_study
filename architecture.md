# Kubernetes Architecture

## etcd
* 分散合意アルゴリズムはraftを使ったOSS Key Value Store.
* Kubernetesクラスタの全てのデータが入る。
* Hadoop基盤の場合、HDFSにZookeeperを組み合わせたようなもの

## kube-apiserver
* Kubernetes APIを提供するcomponent
* kubectlなどでリクエストを送る際にはここにアクセスする

## kube-scheduler
* 起動するノード情報が未割り当てのPodを検知し、kube-apiserverにリクエストを送り、スケジュールを更新する
* 起動するノードを決定する
* 一台だけがleaderとなるように冗長化されている

## kube-controller-manager
* DeploytrollerやReplicaSet Controllerなどを実行するcomponent.
* 実際のPodを登録するのはここが担当し、スケジューリングはkube-schedulerが行う

## kubelet
* 各Kubernetes Nodeで動作するcomponent
* Dockerなどのcontainer runtimeと連携し、コンテナの起動や停止を行う

## kube-proxy + Network Plugin
* Kubernetes Node場で動作するcomponents
* NodePortへのトラフィックを正しくPodに転送されるようにする

## kube-dns
* サービスディスカバリに利用されているDNSサーバー
