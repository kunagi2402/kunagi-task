# AWSコース5回

## EC2の環境構築（Pumaでデプロイ確認）
サンプルアプリが動作するように以下のパッケージなどのインストールを行った。  
・Development Tools、Curlのインストール  
・GPGコマンドでrvm.ioの公開鍵をダウンロード後、CurlでRVMのインストール  
・RVMのPATHを通す。  
・RubyとBundlerのインストール  
・GithubからNVMインストールを行いPATHを通す。  
・Node.jsとYarnのインストール  
・プリインストールされているMariaDB削除  
・MySQLリポジトリのダウンロード後GPGキーもダウンロードしてMySQLインストール  
・サンプルアプリをダウンロード後bin/setupを実行してRailsやMySQL2をインストール  
・database.ymlにhost追加とEC2セキュリティグループにのインバウンドルールにポートを指定して  
追加して無事pumaでのデプロイ成功  

![puma](/img/lecture05/sample-app-puma.jpg)  

## unicornでのデプロイ確認
Curlコマンドでの確認はUnixドメインソケットとしてunicorn.sockを指定することで確認できた。  
ブラウザからもアクセスできるか確認するためにunicorn.rbのlistenにポート番号の追加を行った。  
その際にbin/devの中身を追っていくとforemanを使用してProcfileを読み込みrailsとyarnのデプロイを行って  
いるのが確認できたので、rails startをBundle exec unicornに変更してbin/dev実行でuniconrが  
デプロイするように修正を行いブラウザからアクセスできた。  

![unicorn](/img/lecture05/unicorn-start.jpg) 

![unicorn2](/img/lecture05/unicorn.rb.jpg)  

![unicorn3](/img/lecture05/Procfile.jpg)  


## Nginxとunicornでのデプロイ確認
Nginxをインストールしてwebサーバーとしてデプロイして、リクエストをunicornに送る構成に変更。  
Nginx.confにunicornへリクエストを送る設定を追記して、unicorn.rbのポート番号を記述したlistenを  
コメントアウトを行う。  
Nginxとunicornをデプロイした状態でブラウザからのアクセスに変更。  


![Nginx](/img/lecture05/Nginx.conf.jpg)  

![Nginx2](/img/lecture05/nginx-start.jpg)  
  
![Nginx3](/img/lecture05/unicorn.rb2.jpg) 

![Nginx4](/img/lecture05/NginxtoUnicorn-Start.jpg)  


## ALBの追加
ALB用に新しくセキュリティグループとターゲットグループの作成を行う。  
上記グループを適用して、ALBを作成。  
ALBのDNS名でアプリにアクセスが成功した。  

![ALB](/img/lecture05/ALB1.jpg)  

![ALB2](/img/lecture05/ALB2.jpg)  

![ALB3](/img/lecture05/ALB3.jpg)  

![ALB4](/img/lecture05/ALB4.jpg)  

![ALB5](/img/lecture05/ELBtoAccess.jpg)  


## S3を追加して画像保存先をS3に変更
S3バケットを作成後、EC2からアクセスする際の認証情報用に  
IAMロールとS3への表示、ダウンロード、アップロード、削除のみ許可したポリシーを作成。  
ポリシーを張り付けたロールをEC2にアタッチ。  
development.rbとstorage.ymlを修正してS3に画像を保存するよう変更。    
ブラウザから追加した画像がS3に保存されたことを確認。  

![s31](/img/lecture05/policy.jpg)  

![s32](/img/lecture05/role.jpg)  

![s33](/img/lecture05/development.rb.jpg)  

![s34](/img/lecture05/storage.yml.jpg)  

![s35](/img/lecture05/s3-app.jpg) 

![s36](/img/lecture05/s3-bucket.jpg)  

## アーキテクチャ図の作成
AWSのアーキテクチャ図作成時のガイドラインにのっとり作成を実施。

![arc](/img/lecture05/architecture.jpg)  


## 第5回の感想
今回の課題は大変で、環境構築に必要なパッケージは何か常に調べつつ  
インストールすることから始まって、MySQLのエラーの解決に時間がかかり、  
Nginxのプロキシの設定が間違っているためUnicornへのPOSTが失敗したりなど  
はまりつつも調べたり質問したりで何とか進むことができた。  
その後のALBの作成やS3の追加は曖昧だったルーティングやロールとポリシーについて    
理解を深めることができた。  
アーキテクチャ図もガイドラインでがちがちに規約が決まっているのは意外だった。  