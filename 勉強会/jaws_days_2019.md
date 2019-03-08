# #1 [EC2からKubernetesへの移行をセキュリティ/モニタリングから考える](https://jawsdays2019.jaws-ug.jp/session/2253/)

freee社のミッションが「スモールビジネスを、世界の主役に。」に変わった
日本の[260信用金庫のうち253とAPI連携](https://tech.nikkeibp.co.jp/atcl/nxt/news/18/04006/)するようになった
Open Platformで[アプリストア](https://www.freee.co.jp/blog/appstore_release.html)を開設した
そして、freeeは[電子決済等代行業者の第1号](https://corp.freee.co.jp/news/denshikessai-8726.html)として認定された
これによって、freeeのプラットフォーム上でお金の送金や振り込みもできるようになった

このようにfreeeでは顧客の生のデータ（会計、労務）を多数扱っているため、セキュリティ対策も怠ってはならないと考えている


## 主な取り組み

- 多重防御するためのアーキテクチャを採用している
![IMG_6621.jpeg](https://qiita-image-store.s3.amazonaws.com/0/30522/7972d7d8-56fc-796c-be8c-2e76f9fe973c.jpeg)

- EC2へのログインはすべてMFA
- SSHログイン時はSlackに通知
- 操作ログはすべてS3に保存
- アウトバウンドも必要最低限に絞る
- AWS WAFでAWSが用意した定義を適用すると何で検出されたのかがわからないため、CSC社の[WafCharm](https://www.wafcharm.com/)を使用している
- GuardDutyには即時性はないが、広範囲での脆弱性の検出には役に立つ
- S3に溜まったログはトリアージ（ログの調査、評価）する
![IMG_6625.jpeg](https://qiita-image-store.s3.amazonaws.com/0/30522/02ce4829-c38e-79cc-24cb-f67aff7b4748.jpeg)



## Kubernetesへ

上記のようにビジネスを拡大してきたこともあり、デプロイ時間がかかるようになった
そのため、マイクロサービス化し、コンテナベースのアーキテクチャに変更することになった
最初はkube-awsを用いて自前で構築したが、求めていたセキュリティ要件（DeepSecurityが使えないとか）を満たせなかった
EKSが利用できるようになったことで現在はEKSで構築している

## すべてAWS

freeeはすべてのサービスをAWSで構築している
ベンチャーだからゆえにスピードを重視し、コストをかけなければならないところで集中的にかけるようにしている
なので、**マネージドでできるものはマネージドで**
**本質的価値のあるものへ開発に集中**
**車輪の再発明はしない**

## 監視のアーキテクチャ
![IMG_6628.JPG](https://qiita-image-store.s3.amazonaws.com/0/30522/84ca0b00-fda6-e3e2-9bb8-5ed700b957e3.jpeg)

資料：https://speakerdeck.com/atk/freeeniokeru-kubernetesjian-shi-ji-pan

## 参考

https://dev.classmethod.jp/cloud/aws/201902_jawsdays2019-freee-security-k8s/


# #2 [PenTesterが知っている危ないAWS環境の共通点 ~攻撃者視点よりお届けする狙われやすいAWSの穴](https://jawsdays2019.jaws-ug.jp/session/1393/)

スピアフィッシングによるセキュリティ被害が増えている
AWSだからと言って安全ではない
そして一番盗まれちゃいけないものは**Credential**である
たとえ盗まれたCredentialに管理者権限がなくても、場合によっては権限昇格するケースがある

## 権限昇格するケース

- iam:AttachUserPolicy
- iam:AttachGroupPolicy
- iam:AttachRolePolicy
- 権限の高いLambda関数の実行できるロールが割り当てられている場合

つまり、取得したCredensialでLambda経由で新たに上位権限を持つユーザを作ったりすることができちゃう

## 対策

- IAMのベストプラクティスに従う
- アクセスキーの発行は最低限、定期的に棚卸し
- ツール（AWS Trusted Advisor、Classmethod insightwatch、Netflix Security Monkey）で確認する


## 発表資料
https://www.slideshare.net/zaki4649/pentesteraws

# #3 [Kubernetes on AWS/EKSベストプラクティス](https://jawsdays2019.jaws-ug.jp/session/1918/)

マイクの音量が小さくてほぼ何も聞き取れなかったので、スライドだけ共有
関連ツールなどがたくさん紹介されているので、見る価値はある

https://speakerdeck.com/mumoshu/eksbesutopurakuteisu2019-dot-2-number-jawsdays

