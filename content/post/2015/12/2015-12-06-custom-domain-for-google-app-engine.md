+++
date = "2015-12-06T00:41:44+09:00"
slug = "custom-domain-for-google-app-engine"
tags = ["GoogleAppEngine"]
title = "Google App Engineで独自ドメインを設定する"

+++

S3 + 独自ドメインで公開していた静的なWebサイトをGoogle App Engineに移行しました。
その時のGAEでのドメイン設定のメモです。

## ドメインの追加

デベロッパーコンソールの[設定] - [カスタムドメイン]の画面

追加したいドメインを入力して確認します。

![](/post/2015/12/gae-domain1.jpg)

### ドメインの確認

ドメインは`Value Domain`でとりましたが、`ドメインレジストラまたはプロバイダを選択`のところになかったので、`その他`を選択

![](/post/2015/12/gae-domain3.jpg)

画像はcnameで設定する場合です。txtレコードで設定しようとしたら、Google Apps用のtxtレコードが邪魔したようでNGになりました。

![](/post/2015/12/gae-domain2.jpg)

これらをValueDomainのDNS設定画面で設定します(**ホストとターゲットの最後にドットが必要**)

DNS設定が完了して設定が浸透した頃に確認ボタンを押すと確認OKになるはずです。

![](/post/2015/12/gae-domain4.jpg)

確認OKの場合はドメイン選択に追加したドメインが表示されます。

![](/post/2015/12/gae-domain5.jpg)

GAEの場合ネイキッド・サブのドメインが選択できます。S3は多少面倒なことをしないといけなかったので助かります。

# Aレコードの設定

再度ValueDomainでDNSの設定をします。

![](/post/2015/12/gae-domain6.jpg)

あとは、DNSの浸透をまては完了
