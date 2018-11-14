# Config & Storage
* `ConfigMap` 設定ファイル
* `Secret` パスワード
* `PresistentVolumeClaim` 永続化ボリューム


### 設定
* yamlに設定されたものが、OSの環境設定に明記される
* `spec.containers[].env` に `name` と `value` で登録
* `spec.containers[].env.valueFrom` 以下を設定することで、起動しているノードなどの情報も得られる
* 機密情報はSecretリソース場で参照することが推奨

### Secret
* 基本的にはsecretを定義しているマニフェストはbase64でエンコードされているだけ
* 暗号化することも可能（kubesec）
* Generic, TLS, Docker Registry, Service Account
* 環境変数として渡す方法とVolumeとしてマウントして渡す方法がある

### Generic
* kubectl でファイルから値を参照して作成する `--from-file`
* kubectlでenvfileから値を参照して作成する `--from-env-file`
* kubectlで直接値を渡して作成する `--from-literal`
* マニフェストから作成 `-f`

#### --from-file
* `kubectl create secret generic --save-config sample-db-auth --from-file=username --from-file=password`
* `kubectl get secrets sample-db-auth -o json`
* ただし、base64であるので、復元可能であることに注意
* `kubectl get secrets sample-db-auth -o json | jq -r .data.password | base64 --decode` の結果は `password` が帰ってくる
* 要するに、Genericはあんまし使えない

### TLS
* 秘密鍵と証明書のファイルから作成する
* `kubectl create secret tls --save-config tls-sample --key {path_to_key} --cert {path_to_cert}`

### Docker Registory
* dockerの認証情報を用いる

### kubesec
* Secretのマニフェストを暗号化するので、安全だが、若干めんどい

## ConfigMap
* GenericのSecretタイプと同じ方法で作成

## PersistentVolumeClaim
### Volume
* マニフェスト経由で利用可能なボリュームを指定する
* 削除や追加はできないが、参照が可能
### PersistentVolume
* マニフェスト経由で新規ボリュームの作成や削除が可能。
### PersistentVolumeClaim
* Persistent Volume リソースの中からアサインするためのリソース

### Volume
#### emptyDir
* Pod上の一時的なディスク領域
* Podがterminatedになると、削除される
* `spec.volumeMount.mounthPath=/cache`

#### hostPath
* host上の任意の領域をマウントする
* 信用できないコンテナが乗る場合は利用を控える
* セキュリティは甘い

### downwardAPI
* Podの情報などをファイルとして配置するためのプラグイン

### projected
* volumemountを一箇所のディレクトリに集約するプラグイン

## PersistentVolume
* network越しにディスクをアタッチするタイプのディスク
* labelを付与する際にはどのようなボリュームであるかを気をつける

## PersistentVolumeClaim
* manifestファイルの要求に応じて、ヴォリュームが払い出される。
* 基本的にはnfs形式
