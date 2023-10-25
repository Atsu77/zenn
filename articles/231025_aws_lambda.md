---
title: "Lambdaを使用してS3からRDSにデータを移行する方法について" # 記事のタイトル
emoji: "😸" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["aws", "lambda", "s3"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

# Lambda を使用して S3 から RDS にデータを移行する方法について

## 導入

新人研修の成果物を作る際に、S3 に保存していた画像データを RDS に移行する必要がありました。その際に、Lambda を使用して S3 から RDS にデータを移行する方法を調べたので、その内容をまとめます。

## バックグラウンド

### Lambda とは

AWS が提供しているサーバーレスコンピューティングサービスです。Lambda を使用することで、サーバーの管理やスケーリングなどを気にすることなく、コードを実行することができます。これにより、エンジニアはインフラ側のことを気にすることなく、ビジネスロジックなどのコアな部分に集中することができます。

## 方法

### VPCの作成

LambdaからRDSにアクセスするためには、両方が同じVPC内に配置されている必要があります。そのため、まずはVPCを作成します。
インフラ構成図は以下のようになります。

```mermaid
flowchart LR

%%外部要素のUser
OU1[User]

%%グループとサービス
subgraph GC[AWS]
  subgraph GV[vpc-sc1]
    subgraph GS2[subnet-2]
      CP1("EC2<br>web1")
    end
    subgraph GS1[subnet-1]
      CP2(Lambda)
      DB1[("RDS<br>db1")]
    end
  end
  ST1[("S3<br>xx.com")]
end

%%サービス同士の関係
OU1 --> CP1
CP1 --> DB1
CP2 --> DB1
CP2 --> ST1

%%グループのスタイル
classDef SGC fill:none,color:#345,stroke:#345
class GC SGC

classDef SGV fill:none,color:#0a0,stroke:#0a0
class GV SGV

classDef SGPrS fill:#def,color:#07b,stroke:none
class GS2 SGPrS

classDef SGPuS fill:#efe,color:#092,stroke:none
class GS1 SGPuS

%%サービスのスタイル
classDef SOU fill:#aaa,color:#fff,stroke:#fff
class OU1 SOU

classDef SNW fill:#84d,color:#fff,stroke:none
class NW1 SNW

classDef SCP fill:#e83,color:#fff,stroke:none
class CP1,CP2 SCP

classDef SDB fill:#46d,color:#fff,stroke:#fff
class DB1 SDB

classDef SST fill:#493,color:#fff,stroke:#fff
class ST1 SST
```


### ロールの作成

LambdaからS3とRDSにアクセスするために、それぞれの権限を持ったロールを作成します。

#### 1.S3へのアクセス権限を持ったロールの作成

1. IAM のコンソール画面を開きます。
2. 左側のメニューから「ロール」を選択します。
3. 「ロールの作成」をクリックします。
4. 「AWS サービス」を選択し、「Lambda」を選択します。
5. 「次のステップ: アクセス権限」をクリックします。
6. 「AmazonS3ReadOnlyAccess」を選択します。
7. 「次のステップ: タグ」をクリックします。
8. 「次のステップ: 確認」をクリックします。
9. 「ロール名」に任意の名前を入力し、「ロールの作成」をクリックします。


#### 2.RDSへのアクセス権限を持ったロールの作成

1. IAM のコンソール画面を開きます。
2. 左側のメニューから「ロール」を選択します。
3. 「ロールの作成」をクリックします。
4. 「AWS サービス」を選択し、「Lambda」を選択します。
5. 「次のステップ: アクセス権限」をクリックします。
6. 「AmazonRDSFullAccess」を選択します。
7. 「次のステップ: タグ」をクリックします。
8. 「次のステップ: 確認」をクリックします。
9. 「ロール名」に任意の名前を入力し、「ロールの作成」をクリックします。

#### 3.Lambda関数に作成したロールをアタッチする

Lambdaのコンソールにアクセスし、作成したLambda関数を選択し、その後ロールの編集をクリックします。ロールの編集画面が表示されたら、先ほど作成したロールを選択し、保存をクリックします。

#### 4.VPCの設定

Lambda関数をRDSにアクセスできるようにするために、VPCの設定を行います。
まず、Lambdaのコンソールにアクセスし、VPCの設定をクリックします。VPCの設定画面が表示されるので、VPCを選択し、サブネットを選択します。その後、セキュリティグループを選択し、RDSのセキュリティグループを選択します。最後に、保存をクリックします。

#### 5.Lambda関数の作成

今回はPython3.9を使用してLambda関数を作成します。作成したら、圧縮しzipファイルにします。zipファイルをLambda関数にアップロードします。

#### 6.Lambda関数の実行

Lambda関数を実行すると、S3からRDSにデータが移行されます。


## 結果や影響

以上がS3からRDSにデータを移行する方法です。

## まとめ

Lambdaを使用してS3からRDSにデータを移行する方法についてまとめました。今回は、S3からRDSにデータを移行する方法を調べたので、その内容をまとめました。
