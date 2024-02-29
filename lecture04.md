# AWSコース4回

## VPCの作成
raisetch-vpcという名称で2つのパブリックサブネット、プライベートサブネットを含んだVPCを作成。  
IPv4アドレスの範囲は、RFC1918に準拠して192.168.0.0/16と設定。  
EC2のSSH接続と、RDSのEC2からのみ接続用のセキュリティーグループも作成。

![VPC](/img/lecture04/lecture04-vpc.jpg)  

![EC2-Security](/img/lecture04/lucture04-security-ec2.jpg) 

![RDS-Security](/img/lecture04/lucture04-security-rds.jpg)  

## EC2の作成
パブリックサブネット上に作成。  
新規キーペアを作成。

![EC2](/img/lecture04/lucture04-ec2.jpg) 

![EC2-keypair](/img/lecture04/lecture04-ec2-keypair.jpg)  


## RDSの作成
プライベートサブネット上にMySQLエンジンを使用して作成。  

![RDS](/img/lecture04/lecture04-rds1.jpg)  

![RDS2](/img/lecture04/lecture04-rds2.jpg) 

![RDS3](/img/lecture04/lecture04-subnetgroup.jpg)  

## ローカル環境からEC2を経由してRDSにログイン
Puttyからログイン成功

![putty](/img/lecture04/lecture04-mysql-login.jpg)  


## 第4回課題の感想
ここから本格的に構築が始まったことを実感しました。  
新しいインスタンスの作成などは、ある程度AWSが良い感じに作成してくれるが、  
だからこそ各サービスのドキュメントをよく読んで  
大まかに何が設定できるか理解しておく必要があると感じました。


