# Cluster and Metadata Resource
* Cluster Resourceはセキュリティ周りの設定やQuota設定
* Metadata Resourceはクラスタ上にコンテナを起動させるのに利用するリソース

## Cluster Resource
### Node
* `kubectl get nodes {nodename} -o yaml` でNodeのリソース状況が確認可能
* `kubectl describe node` で現在のリソース量を確認できる

### Namespace
* Kubernetesクラスタを仮想的に分離する。
### PersistentVolume
### Resource Quota
### Role
### ClusterRole
### RoleBinding
### ClusterRoleBinding
### NetworkPolicy

## Metadata Resource
### LimitRange
### HorizontalPodAutoscaler
### PodDisruptionBudget
### CustomResourceDefinition

