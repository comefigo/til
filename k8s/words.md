# 概要

Kubernetes（以降**k8s**とする）関連の用語と構成などのまとめ

# ツール

## kubectl

- k8sのクライアントツール
- Pod、ReplicaSet、Serviceなどを管理できる

## Minikube

- ローカルでシングルクラスタを構築するためのツール

# 構成関連

![Kubernetes.png](https://qiita-image-store.s3.amazonaws.com/0/30522/8596970e-96f3-8cec-97cf-229a5b9b0adf.png)
引用：https://en.wikipedia.org/wiki/Kubernetes


## [Master](https://kubernetes.io/docs/concepts/overview/components/#master-components)

- クラスタを管理する役割
- 最低3台以上の奇数構成
- コンポーネント
    - kube-apiserver（ユーザがk8sを制御するためのAPIサーバ）
    - kube-scheduler（デプロイの調整）
    - kube-controller-manager（コンテナの複製、稼働状況の確認などの各種処理を担うコントローラ）
    - cloud-controller-manager （クラウドプロバイダーとk8sの融合するための機能）
    - etcd（構成情報をKVSで保存）

## [Node](https://kubernetes.io/docs/concepts/overview/components/#node-components)

- コンテナの稼働する場所
- 1台から使用可能
- コンポーネント(すべてのNode上で稼働する)
    - コンテナランタイム（Docker、rkt、cri-o）
    - Kubelet（Node上のコンテナの制御）
    - Kube-proxy（ネットワークトラフィック、ロードバランサーの制御）


# 用語

## Pod

- コンテナのデプロイ先
- デプロイの最小単位
- Nodeを跨ぐPodはない
- 1つのNode内にPodのすべてのコンテナがデプロイされる
- IP、ポート、ファイルシステム以外の全リソースを共有する
- 同一Pod内のコンテナ同士はlocalhostで通信できる（Pod内のコンテナの数に関係なく1つのIPしか持たない）
- Persistent Volumeを共有できる
- スケールアウトはPod単位

## ReplicaSet

Pod内のプロセス障害、Node障害が発生した際に、Podを自動回復させるためのオートヒーリングを実現する機能

### Labelセレクター

監視対象のPodを特定するセレクター
ReplicaSetがセレクターで検索し、Pod数が不足している場合は、新規Podを追加し、余剰の場合は、Podを削除する

**※新規追加されたPodは既存のPodの状態を引き継ぐことではなく、新たにコンテナを新規起動するだけである**

### replicas

稼働すべきPodの数

### Podテンプレート

新しいPodを起動するための定義テンプレート

### ヘルスチェック（Liveness Probe）

デフォルト（コンテナ内のプロセスの起動）のヘルスチェックに加えて以下のチェック機能があります

- HTTP GET
- TCP Socket
- Exec（コンテナ内でコマンドを発行し、ステータスコードが0であることを確認）

## Service

複数のPodにスケールアウトされたコンテナを通信上の入り口として役割を果たす

### 構成要素

- ClusterIP・・・クラスタ内のみで疎通できるIP
    - 外部公開しないPodはこれでよさそう
- port・・・Serviceを利用する側が使用するポート（外部公開のポート）
- targetPort・・・Podのリッスンポート（内部公開のポート）
- selector・・・Labelセレクター

## Readiness Probe

Podの初期化処理が完了し、サービスが利用できる状態であることをチェックする機能

### チェック項目（ヘルスチェックと同等）

- HTTP GET
- TCP Socket
- Exec（コンテナ内でコマンドを発行し、ステータスコードが0であることを確認）

## External Service

Kubernetesクラスタの外部のサービス（RDSなど）にアクセスする場合で、クラスタ内のサービスと同様に扱うことができる

## Deployment

podのバージョンアップ/ロールバックをする仕組み

### 構成要素

- selector・・・監視用のLabelセレクター
- replicas・・・Podの稼働すべき数
- templates・・・新しいPodを起動するための定義テンプレート
- strategy・・・デプロイ時の動作モード
    - RollingUpdate・・・新旧バージョンが入り混じったサービス提供
    - Recreate・・・新旧バージョンが入り混じらず、同時切り替えになる。サービスダウンタイムが発生する
- revisionHistoryLimit・・・ReplicaSetの履歴数の上限値

## ボリューム

データの永続化を目的とする

### emptyDir

Podと同じライフサイクルを持つボリューム
Pod内の複数のコンテナ間で共有する

### gitRepo

ボリュームの初期化後（**1回のみ**）にGitリポジトリからクローンする機能

### hostPathボリューム

Podが稼働しているNode上のファイルシステムにボリュームを持つ

### PersistentVolume（PV）、PersistentVolumeClaim（PVC）

Pod定義に環境依存するボリュームの情報（接続先IPなど）をPVで定義し、環境依存する情報を含まないPVCをボリュームとして提供する
関連するストレージ容量が変化しても、自動でPVが拡張されることはない

#### ダイナミックプロビジョニング

PVの管理を手動ではなく、自動PVCに応じて必要なPVを生成する
予めStorageClassをクラスタに登録し、PVプロビジョナーを指定する必要がある

### ConfigMap

環境依存情報を含む設定ファイルをKey-Valueのボリュームとしてマウントすることができる（ファイル名がキーとして登録される）
また、環境変数やPod定義パラメータとして利用できる
**※取り込み時に単一ファイルの場合は、Key単位で上書きできるが、ディレクトリ単位で取り込んだ場合は、上書きできない**

##### 使い方

- 環境変数で使う
    - プレフィックスを使うことで環境変数の衝突を防ぐことが可能
    - 動的に更新されない
- Volumeで使う
    - 一定時間ごと(デフォルト60s)に値の更新をする

#### Secret

パスワード、秘密鍵などの情報を扱うための特殊なConfigMap

- データをBase64でエンコードし、1MB以内のバイナリデータとして扱わなければならない
- 特定のPodのみにデータ参照が可能
- ノードのメモリ上(tmpfs)に展開され、ディスクには書き込まれない
- 平文でetcdに書き込まれる（アクセス権限の設定に要注意）

**kubesec**で値を暗号化できる

##### タイプ

- Generic(Opaque)
- TLS(kubernetes.io/tls)
- Docker Registry(kubernetes.io/dockerconfigjson)
- Service Account(kubernetes.io/service-account-token)

##### 使い方

- 環境変数で使う
    - プレフィックスを使うことで環境変数の衝突を防ぐことが可能
    - 動的に更新されない
- Volumeで使う
    - 一定時間ごと(デフォルト60s)に値の更新をする

#### 環境変数

command、argsで指定できる環境変数は**マニュフェストで定義された環境変数のみ**
それ以外の環境変数はcommandでシェルスクリプトを実行し、その中で環境変数を取得するように実装する


### DownwardAPI

Podのメタデータをボリュームとして取得できる
取得可能なメタデータは以下の通りになります

- metadata.annotations
- metadata.labes
- metadata.name
- metadata.namespace
- metadata.uid

### projected

Secret、ConfigMap、downwardAPI、serviceAccountTokenのボリュームを1か所に集約するプラグイン

## DeamonSet

ログ収集や監視エージェントなどシステム全体で必要なコンテナを全Nodeに1Podずつ配置する（１ノードに複数の同一Podを作成できない）
配置先を制限する場合は`template.sepc.nodeSelector`のLabelで制限可能

## StatefulSet

- DBなどのステートフルなワークロードに対応するためのリソース
- Pod作成時は同時に作成されず、1つずつ作成されていく
    - spec.podManagementPolicyを`Parallel`にすることで並列作成する
- Pod削除時はStatefulSetのインデックスの大きい順に削除される

## Job / CronJob

Job・・・ワンショットバッチジョブ
CronJob・・・定期実行バッチジョブ

- Nodeの障害時に別NodeでPodを再実行する
- 1回キリの実行
- 一定数成功するまで並列実行
- 順次実行はできない

## Affinity

## Flannel

ノード間を仮想ネットワーク（オーバーレイネットワーク）として構築することで、異なるノードにあるPodが通信できるようにする

## アノテーション

システムコンポーネントが利用するメタデータ（`kubectl.kubernetes.io/last-applied-configuration`など）
リソースを更新するためにこのメタデータをもとに更新する
システムが利用するもの

## ラベル

リソースを分別するためのメタデータ
ユーザが利用するもの

※ラベル被りでは思わぬ事故につながるため、要注意！


# ネットワーク

- 同Pod内のコンテナはすべて同じIP
- 同Pod内のコンテナ同士はlocalhostで通信
- Podと別PodのコンテナはIPアドレスで通信

## DNSポリシー

クラスタ内DNSになければ、外部DNSに問い合わせることができる