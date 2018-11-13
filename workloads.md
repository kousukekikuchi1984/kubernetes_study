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

## Deployment
Rolling UpdateやRollbackを実現するリソース。
* 新しいreplica setを一つ作ったら古いreplica setを一つ減らす
* rollback方法は、 `kubectl rollout history deployment {deployment_name} --revision 1`
* などで確認をしたうち、
* `kubectl rollout undo deployemnt {deployment_name} --to-revision {revision_num}`
* 一時停止時には `kubectl rollout pause deployment {deployment_name}`
* 解除方法は `kubectl rollout resume deployment {deployment_name}`
* 一時停止時にはdeploymentの即時反映はできない

### Deploymentのアップデート戦略
* Recrate
  * すべてのPodを削除してからサイドPodを作成する
  * 余剰のリソースを使わない、切り替えが早い
  * `spec.strategy.type` に `Recreate` を設定
* Rolling Update
  * podを一つ一つ置き換えを行う
  * `spec.strategy.type` に `RollingUpdate` または未設定
  * `spec.strategy.rollingUpdate.maxUnavaiable` と `spec.strategy.rollingUpdate.maxSurge` をよしなに設定

## DaemonSet
* 各Podを各ノードに一つは配置する方法
* nodeSelectorやNode Anti-Affinityで除外することも可能
* UseCaseはログ収集やノードのモニタリングなど

### 作成
* `kind` に `DaemonSet` を設定

### アップデート
* `spec.updateStrategy.type` に `OnDelete` を設定
  * この場合はPodが停止した際にアップデートを行う
  * 緊急時には意識的にpodをkillする必要がある（セルフヒーリングで自動的に上がる）
* `spec.updateStrategy.type` に `RollingUpdate` を設定
  * rolling update

## Stateful Set
* ReplicaSetの特殊な形。データ永続化のための仕組みを有している
* 結構各部分が多い。 `kind` に `StatefulSet` を指定
* `volumeClaimTemplates` を明記
* `spec.podManagementPolicy` に `Parallel` or `OrderedReady` に設定できる

### scaling
* `replicas` の数字を変えるだけだが、減る場合は注意が必要。
* インデックスが大きいものから削除されていく

### Update
* `OnDelete` と `RollingUpdate`
* `spec.updateStrategy.rollingUpdate.partition` はアップデートしないもの

### StatefulSetの削除とPersistentVolumeの削除
* 両方とも削除をする必要がある。
* persistent volumeはそのボリュームが残っていれば、問題なくデータを引き出せる



