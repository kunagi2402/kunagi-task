# AWSコース6回

## CloudTrailで証跡の確認
CloudTrailのAWS捜査記録からイベントを取り上げ、内容を確認  
- イベント名：PutMetricAlarm  
![1](/img/lecture06/CloudTrail1.jpg)

- イベント内容  
![2](/img/lecture06/CloudTrail2.jpg)

- okActionとalarmAction内でそれぞれのアクション時にどんな動作をするかが記述されている。  
今回は両方ともAmazon SNS内のトピックを呼び出す。
- metricNameでは今回使用するメトリクスの"HTTPCode_Target_4XX_Count"が記述。  
- dimensions内にアラーム対象のターゲットグループやAZ、ELBの情報がValueに格納されている。  

上記のソースを読めばどのユーザーが何を対象にしてどのようなアラームを設定したかが判断できる。

## Cloudwatchアラームの設定
ALBに対して以下2つのアラームを作成して動作確認
- ALBからEC2に対してのヘルスチェックに失敗してUnHealthyになった場合、アラームを通知 。  
1分毎の収集したデータが5回連続で1台でもEC2のヘルスチェックに失敗していたら  
アラーム通知になるよう設定する。

![3](/img/lecture06/Cloudwatch1.jpg)

![4](/img/lecture06/Cloudwatch2.jpg)

Unicornサーバーのstartやstopを行い、アラーム状態かOK状態に状態偏移したときに、  
Amzon SNSで指定したメールアドレスに通知することを確認。

![5](/img/lecture06/Cloudwatch3.jpg)

![6](/img/lecture06/Cloudwatch4.jpg)

- レスポンスコードが4xxエラーだったリクエストの数をカウントし、1回でも4xxエラーが返ってきたら  
アラームで通知するよう設定する。  
nginxのみstartした状態でアクセスして、404がカウントされアラーム状態になり通知されることを確認。

![7](/img/lecture06/Cloudwatch5.jpg)

![8](/img/lecture06/Cloudwatch6.jpg)

![9](/img/lecture06/Cloudwatch7.jpg)


## AWS見積もり作成
AWS Pricing Calculatorで見積もりを作成。  
[AWS利用料の見積もり](https://calculator.aws/#/estimate?id=b26a7ef3993d1be54d99fb8341c16e07f26512fd)

## AWSの現在の利用料確認
2月は無料枠に収まり、料金がかからなかったが、3月はVPCのPublic IPv4アドレス使用料と、  
EC2にアタッチしているEBSが無料枠を超えているため多少の料金が発生している。

![10](/img/lecture06/cost1.jpg)

![11](/img/lecture06/cost4.jpg)

![12](/img/lecture06/cost2.jpg)

Budgetにて予算を10ドルに設定し、7ドルを超えた場合はアラームを通知するよう設定。

![13](/img/lecture06/cost3.jpg)

## 感想
証跡を残すためにログを取ることの大切さを学んだ。  
適切に保存してすぐに確認できること、ログやメトリクスをトリガーにしてアラームを通知することは  
システムを運用していくうえでとても重要で、セキュリティ的にも大切だと感じました。  
複数の監視ツールを用いるのでそれぞれの役割と使用方法をしっかりわかるように覚えていく。

コストの可視化もまた運用を続けていくのに重要なので、どのサービスがどのくらいかかるかなどを  
把握しておく必要があると感じました。  
また1年間の無料枠があるので良いが、RDSなどは機能をつけ足したり、冗長化すると思った以上に  
コストがかかるものだと思いました。

