# Discovery & LB Resource
クラスタのコンテナに対するエンドポイントの提供、ラベルに一致するコンテナのディスカバリに利用されるリソース

## Kubernetes Cluster Network and Service
 * Kubernetesクラスタはクラスタ構築時に自動的にノードにPodのために内部ネットワークを構成する
 * Container Network Interfaceの中には、NICを介してPodがある。
 * 同一Podはlocalhostとして認識される
 * というより、サービスという機能により、同一PodはLocalhostとして認識される
 * そのために外部ロードバランサが払い出すVIPとクラスタ内で利用可能なVIPがある
 * Cluster IPと行ったら、NCIのこと

### サービス
* 同一Pod内では異なるコンテナもlocalhostとして認識される仕組み
* これを応用して、ロードバランサが簡単に実装できる。
* 内部的には、ワーカーノードで動くkube-proxyがiptablesにエントリを追加している
 
### Cluster IP
* `kind=Service` `spec.type=ClusterIP`
* `spec.ports` 以下に `name`, `protocol`, `port`, `targetPort` を指定する
* 複数可能
* `port` と `targetPort` 対象となるコンテナのポートとノードのポートを結びつけている

### DNS
* 内部的にDNSを持っているために、サービス名と自動的に紐付けがなされる
* `*.cluster.local` 以外のレコードにかんしては、外部DNSに問い合わせ
* また、PodのdnsPolicyの設定で明示的に外部DNSを参照することも可能
