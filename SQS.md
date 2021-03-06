AWSが提供するフルマネージドなメッセージキューイングサービス。
キューの管理とメッセージの管理の２つの機能がある。

**キュー**
メッセージを管理するための入れ物のようなもので、基本的には利用開始時に作成すれば管理する必要はなく、エンドポイントと呼ばれるURLを介して利用する形になる。キューの管理機能としてはキューの作成・削除の他に動作属性などの詳細な設定があります。

**メッセージの管理機能**
キューに対するメッセージの送信・取得と処理済みのメッセージの削除がある。それ以外にも複数のキューをまとめるて処理するバッチ用のAPIや、処理中に他のプロセスから取得できなくするための可視性制御のAPIもある

### StandardキューとFIFOキュー
SQSのキューにはStandardキューとFIFOキューがある。SQSではベストエフォートのために、もともとメッセージの配信順序は保証されていなかった。
Standardキューでは、取得のタイミングによって同一のメッセージが２回配信される可能性があった。そのため利用するシステム側で、同一のメッセージを受信しても影響がない作り方(冪等性)を保証する必要があった。
FIFOキューは同一のメッセージの二重取得の問題も解消されている。
使いやすさはFIFOキューの方が優れている。ただし、FIFOキューは配信順序の保証のためにStandardキューより秒当たりの処理件数が劣っている。

> **Warning**
> Standardキューは一秒当たりのトランザクション数がほぼ無制限
> FIFOキューは一秒当たりのトランザクションはバッチ処理なしで３００件、バッチ処理ありで３０００件に制限されている。

### ロングポーリングとショートポーリング
キューのメッセージ取得方法として、ロングポーリングとショートポーリングがある。
両者の違いはSQSのキュー側がリクエストを受けた際の処理にある。
デフォルトのショートポーイングの場合、リクエストを受けるとメッセージの有無に関わらず即レスポンスを返す。
ロングポーリングの場合はメッセージがある場合に即レスポンスを返すのは同じだが、メッセージがない場合は設定されたタイムアウトのギリギリまでレスポンスを返しません。

###　可視性タイムアウト
メッセージは受信しただけでは削除されない。クライアント側から明示的に削除指示を受けた時に削除される。そのため、メッセージの受信中に他のクライアントが取得してしまう可能性がある。それを防ぐために、SQSには同じメッセージの受信を防止する機能として可視性タイムアウトがある。
デフォルト設定は３０秒で、最大１２時間まで延長できる


### 遅延キューとメッセージタイマー
メッセージの配信時間のコントロールとして、遅延キューとメッセージタイマーの２つの機能がある

**遅延キュー**
キューに送られたメッセージを一定時間見えなくする機能。

**メッセージタイマー**
個別のメッセージに対して一定時間見えなくする機能。
メッセージ配信後すぐに処理されると問題がある場合に利用する。

**デッドレターキュー**
処理できないメッセージを別のキューに移動する機能。
指定された回数(１~1000回)処理が失敗したメッセージを通常のキューから除外して、デッドレターキューに移動する。

### メッセージサイズ
キューに格納できるメッセージの最大サイズは２５６KB。SQSでは大きなサイズのデータはS３やDynamoDBに格納し、そこへのパスやキーといったポインター情報を受け渡すことで対応する。

