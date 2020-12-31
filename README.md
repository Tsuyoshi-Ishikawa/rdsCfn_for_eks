# 目的
localからrdsにつなぐためにbassionEc2を介して接続する。
そのためのVPCとEC2とRDSをcloudformationで作成する

# 作れる物
- VPC x 1
- publicSubnet x 1(EC2紐付け)
- privateSubnet x 2(rdsSubnetGroupに紐付け)
- publicRouteTable x 1(publicSubnet用)
- privateRouteTable x 2(privateSubnet用)
- internetGateWay x 1(publicSubnet用)

- rdsSubnetGroup x 1(RDS用)

- EC2SecurityGroup x 1(ec2用)
- EC2 x 1

- RDSSecurityGroup x 1(RDS用)
- RDS x 1

# 手順
vpc_for_ec2_rds.ymlでvpcスタック作成
↓
rdsSubnetG_for_ec2_rds.ymlでrdsSubnetGroupを作成
↓
ec2_rds.ymlでec2とrdsスタックを作成
↓
ec2にelasticIPを割り当て
↓
ec2に入ってrdsのmysql導入・接続確認、db作成
laravelの場合は以下を参考に
https://noumenon-th.net/programming/2020/04/10/ec2-rds-laravel/
↓
DB接続のためにバックエンドアプリの設定変更
laravelの場合は以下を参考に.envを修正
https://noumenon-th.net/programming/2020/04/10/ec2-rds-laravel/
↓
localからのport forwarding
https://dev.classmethod.jp/articles/rds-portforward/
Ec2のsecurityGがsshのためのport22しか開放していなくてもできる

- 注意
localのdockerコンテナからつなぐ場合はコンテナにawsのキーペアをvolumeしないといけないのでその手間忘れない

# 作ってみた感想
- EC2やRDSを立てる前にネットワーク周りVPCの設定、定義が大切

- rds作成にはrdsSubnetGをsubnetIdを用いて作らなければいけない
vpc作成をした後にdsSubnetGを作成しないといけない。しかも設定する内容も多いので実践ではRDSをcfnで定義しないこともある模様

- インデントの仕方ひとつ違うだけでダメと言われるので注意必要

# cloudformationの基本


# cloudformation参考

- Ec2-rds
https://qiita.com/OPySPGcLYpJE0Tc/items/f0ae690d971e96006b38

- rdsのみ
https://qiita.com/okubot55/items/87d4bd7a3649992bc5f7
https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/quickref-rds.html

- ec2のみ
https://www.udemy.com/course/aws-associate/learn/lecture/13658638#overview

- Cfnの設定方法
https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html
各リソースの定義方法は以下を参考に行う
https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-subnet-route-table-assoc.html