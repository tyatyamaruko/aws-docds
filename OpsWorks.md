`Chef`を利用した構成管理サービス
Chefは`レシピ`と呼ばれるファイルにサーバーの構成を定義し、ミドルウェアのインストールや設定を自動化するツール

**レシピ例**
```ruby
package "httpd" do
    action :install
end

service "httpd" do
    action [ :enable, :start ]
end
```

このレシピはApacheのインストール、サーバー起動時の自動プロセス立ち上げ設定、そしてhttpdプロセスの立ち上げを行うもの。

- Chef Clientローカル方式
  - 各サーバーがレシピを持ち、自分自身にレシピを適用する方式
- Chef Server/Client方式
  - マスターサーバーを用意し、レシピそのものや各サーバーへのレシピ適用状況を管理する方式

### OpsWorksスタック
Chef Clientローカル方式でChefを利用する場合はOpsWorksスタックを利用
- スタック
  - OpsWorksのトップエンティティ。スタック単位の構成情報をJSON形式で保持する。スタックの下に複数のレイヤーを定義できる。
- レイヤー
  - インスタンスの特性ごとにレイヤーを用意する。レイヤーに対してChefレシピをマッピングする。レイヤーを指定してEC2インスタンスを起動することで、インスタンスに対してレシピが適用される。

レイヤー内で起動したEC2インスタンスには、`OpsWorksエージェント`がインストールされる。
このOpsWorksエージェントがOpsWorksからレシピを取得し、自身のインスタンスに適用する


### OpsWorks for Chef Automate
Chef Server/Client方式で利用する場合
ChefにはChefAutomateという、Chefを総合的に利用するための機能がある。ChefAutomateでは、レシピが適用される各インスタンスをクライアントと呼び、それらとは別にクライアントを管理するマスターサーバーが存在するアーキテクチャを採用している。

OpsWorks for Chef Automateを利用すると、Chef Automateサービスが自動的に構築される。