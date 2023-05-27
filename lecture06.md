
## 第６回課題

#  CloudTrail のイベントの記録

  ![cloudtrail](image/0606.png)

イベント名 - これは通常、イベントに関連付けられた API 名 (CreateTrail など) です。  

イベント時間 - イベントが発生した現地時間。  

ユーザー名 - イベントをトリガーしたユーザーの名前。  

イベントソース - イベントが発生した AWS API エンドポイント。たとえば、ec2.amazonaws.com と指定します。  

リソースタイプ - イベント内のリソースのタイプ。例えば、リソースタイプは、EC2 の場合は Instance となります。  

リソース名 - EC2 インスタンス用の i-1234567 など、イベントによって参照されるリソースの名前または ID。  

→今回の画像はRDSを５月２７日の１０時３８分にkawafuchiRT（ユーザー名）が停止したことが確認できました。  

#  CloudWatch アラーム

1. CloudWatch アラームの作成

　![0604](image/0604.png)

2. nginxを停止した際にメールを受信

　![0601](image/0601.png)

　![0602](image/0602.png)

3. nginxを再起動するとOKの通知を受信

　![0603](image/0603.png) 


# AWS 利用料の見積を作成

[AWS　利用料金見積](https://calculator.aws/#/estimate?sc_channel=cfm-blog&sc_campaign=la-get-cost-estimates-faster-with-aws-pricing-calculator-bulk-import&sc_medium=plan-and-evaluate&sc_content=cfm-blog&sc_detail=link&sc_outcome=ad&sc_publisher=cfm-adoption&trk=la-get-cost-estimates-faster-with-aws-pricing-calculator-bulk-import_cfm-blog_link&id=43280ae3c64d95a6e6bbdb8a072afa02e1fb1588
)

# マネジメントコンソールから、現在の利用料を確認

　![0605](image/0605.png)

AWSのアカウントを2021年に作成していたため無料枠での利用がありませんでした。

## 感想
・今回の課題課題を通して一番大事だと思ったところはlog(履歴)を残すことの重要さについての部分です。
（どんなに技術があるエンジニアでもlogがないために現状を把握できないとトラブルが発生した際にトラブルシューティングができない）当たり前のことなのですが改めてその重要さに気付くいい機会になりました。
<br>
・AWSの利用履歴を確認してcloud9のときは終了後３０分でインスタンスが自動停止してくれていましたがEC２はオンデマンドで常に課金されている状態となっておりコストがかかっていました。
<br>
また使わなくなったリソース等も削除してコストをかからないように心がけていく必要があることに気づきました。
RDSのスナップショットは見逃しがちなところなので気を付けたいです。


