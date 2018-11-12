## Pod
* 一つのpodには複数のコンテナを持つ
* 同一Pod内のコンテナは同一IPを持つ

### Podのデザインパターン
* サイドカーパターン
  * メインのコンテナに加えて、補助的な昨日を追加するコンテナを加える
  * Podはデータ領域を共有して持つので、データや設定に関係のあるパターン
* アンバサダーパターン
  * メインのコンテナが外部システムと接続いする際に、Proxy用のコンテナを経由するパターン
  * 疎結合を保つため
* アダプタパターン
  * 外部からのリクエストに対して差分を解消するコンテナを内包したパターン
  * MySQLやRedisのログやmetricsをprometheusの形式に合わせるなど

### DNS
* `spec.dnsPolicy` で設定可能
* 外部のサーバーを指定すると、クラスタ内DNSを利用したサービスでリカバリができなくなる
* dnsPolicyをClusterFirstに設定するか未指定にする

### 静的な名前解決
`spec.hostAliases` で可能

## ReplicaSet
### 作成
* `spec.replicas` の設定をした上で `kubectl apply -f {filename}`
* replicasで設定された数字は最低限確保するサーバー台数。
* レプリカ数を下回る場合は自動的にコンテナが起動される
* `spec.selector.matchLabels` で記入したコンテナにマッチする台数を監視する

### ReplicaSetのスケーリング
* `sed -e s|replicas: 3|replicas: 5 {sampel.replicaset.yaml} | kubectl apply -f -`
* `kubectl scale rs {sample.replicaset.yaml} --replicas 5`
* 基本的にはyamlを書き換えることを推奨。後者はファイルと環境が一致しなくなる可能性がある

