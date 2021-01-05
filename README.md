# 目的
kubectlで建てたEKSからrdsにつなぐためのcfn。
EKSが立つ時に、VPCとworkernode用のsubnetができるのその辺を踏まえてrdsのprivatesubnetを作成する

# 作れる物
- privateSubnet x 2(rdsSubnetGroupに紐付け)
- privateRouteTable x 2(privateSubnet用)

- rdsSubnetGroup x 1(RDS用)

- RDSSecurityGroup x 1(RDS用)
- RDS x 1

# 手順
vpc_for_rds.ymlでvpcスタック作成
↓
rdsSubnetG_for_ec2_rds.ymlでrdsSubnetGroupを作成
↓
rds.ymlでrdsスタックを作成