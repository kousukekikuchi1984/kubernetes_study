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
 
## Cluster IP
* `kind=Service` `spec.type=ClusterIP`
* `spec.ports` 以下に `name`, `protocol`, `port`, `targetPort` を指定する
* 複数可能
* `port` と `targetPort` 対象となるコンテナのポートとノードのポートを結びつけている
* ノードが異なっていても基本的にはCluster IPは同じ

### DNS
* 内部的にDNSを持っているために、サービス名と自動的に紐付けがなされる
* `*.cluster.local` 以外のレコードにかんしては、外部DNSに問い合わせ
* また、PodのdnsPolicyの設定で明示的に外部DNSを参照することも可能

## External IP
* Kubernetes NodeのIPアドレスのトラフィックをコンテナに転送する際に必要
* クラスタ内DNSが返すIPアドレスはExternal IPではなくClusterIP
* ExternalIPを使っている場合は外部からでも疎通できるらしい

## NodePort Service
* 指定されたKubernetes NodeのポートをNICを介してKubernetesのコンテナと接続する
* kubernetes環境では、30000~32767の範囲のポートを指定する必要がある
* 基本的には同じポートを解放することはできない
* ノードを跨いだロードバランシングをなくすことが可能
* `spec.externalTrafficPolicy=Local` に設定

## LoadBalancer Service
* Load Balancingを行う
* localhostを使用する
* 外部にload balancerを持たせる方法は極めて高い耐障害性を持つ
* Kubernetes Nodesに障害が発生した場合はそのノードにトラフィックを送らないようにする
* 検知までに時間がかかる
* `spec.loadBalancerIP` で仮想IPを静的に指定することが可能
* firewallの設定は `spec.loadBalancerSourceRange` でIPアドレスを指定する
* 上記以外にはNetworkPolicy Resourceで不要なアクセスを削除する方が良い。

