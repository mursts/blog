+++
title = "GAE/PyとLINE BOT SDKでLINE BOTを作った"
tags = [
  "appengine", "LINE"
]
isCJKLanguage = true
date = "2016-11-12T00:15:06+09:00"
slug = "line-bot-sdk-gae-py"

+++

今回はGAE/PyでLINE BOT SDKを使ってみました。

普通に考えるとpipでダウンロードして使うだけなんですが、このSDKは外部リクエストに`requests`を使用しているため使うには一工夫いります。

> GAEは外部にリクエストする場合基本URLFetch APIを使う必要があります。ただ、[この記事](http://blog.mursts.jp/entry/2016/09/24/requests-on-gaepy/)で書いたようにrequestsにモンキーパッチを当てればrequestsも使うことはできます

最初はSDKにモンキーパッチを当てようと思いましたが、
SDKのソースをみて`LineBotApi`のコンストラクタににHTTPのクライアントを渡せるのがわかりました。

https://github.com/line/line-BOT-sdk-python/blob/master/lineBOT/api.py#L34

そのクライアントには[HttpClient](https://github.com/line/line-BOT-sdk-python/blob/master/lineBOT/http_client.py#L25)クラスを継承したクラスを作ってわたしてあげればよさそうです。

そして、それを実装して作ったBOTはこちら

https://github.com/mursts/recommend-lunch-BOT

Python東海の発表でAWS Lambdaを使って作ったものをGAEに作り変えたものです

- [Python東海 第31回勉強会](http://connpass.com/event/40191/)
- [AWSでサーバレスなアプリを作った話](https://mursts.github.io/python-tokai-31th/)

位置情報をBOTにわたすと近くでランチやっているお店を教えてくれます。Google Static Mapを使ってぱっと見でどこらへんのお店かがわかるようにしました。

このBOTを使ってみたい場合はこのQRコードから申請してみてください。

![](/post/2016/11/qrcode.png)

ちなみに、先日の[GCPUG Nagoya](http://gcpug-nagoya.connpass.com/)でGAEでLINE BOTを作成するハンズオンも行いました。その時の資料は[こちら](https://mursts.github.io/gcpug-nagoya-02/)

簡単にBOTが作成できるので作ってみてはいかがでしょうか
