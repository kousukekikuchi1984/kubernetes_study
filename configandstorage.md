# Config & Storage
* `ConfigMap` 設定ファイル
* `Secret` パスワード
* `PresistentVolumeClaim` 永続化ボリューム

## ConfigMap

### 設定
* yamlに設定されたものが、OSの環境設定に明記される
* `spec.containers[].env` に `name` と `value` で登録
* `spec.containers[].env.valueFrom` 以下を設定することで、起動しているノードなどの情報も得られる
* 機密情報はSecretリソース場で参照することが推奨

### Secret


