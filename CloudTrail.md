AWSに関する操作ログを自動的に取得するサービス。

### 取得できるログの種類
- 管理イベント
  - マネジメントコンソールへのログイン、EC2インスタンスの作成、S3バケットの作成
- データイベント
  - S3バケット上のデータ操作、Lambda関数の実行など

CloudTrailでは管理イベントの取得のみがデフォルトで有効になっており、過去90日分のログをマネジメントコンソール上で確認できる。過去90日分より前の情報も保持したい場合はS3に証跡をのこすように設定もできる。

