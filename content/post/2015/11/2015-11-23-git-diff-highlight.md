+++
date = "2015-11-23T00:41:30+09:00"
slug = "git-diff-highlight"
tags = ["Git"]
title = "diff-highlightでGitのdiffを見やすくする"

+++

このブログを見てGitのdiffをいい感じに見れるのを知りました。

[Git の diff を美しく表示するために必要なたった 1 つの設定 - 詩と創作・思索のひろば #git](http://motemen.hatenablog.com/entry/2013/11/26/Git_%E3%81%AE_diff_%E3%82%92%E7%BE%8E%E3%81%97%E3%81%8F%E8%A1%A8%E7%A4%BA%E3%81%99%E3%82%8B%E3%81%9F%E3%82%81%E3%81%AB%E5%BF%85%E8%A6%81%E3%81%AA%E3%81%9F%E3%81%A3%E3%81%9F_1_%E3%81%A4%E3%81%AE%E8%A8%AD)

<!--more-->

やり方は

> Git に同梱されている contrib/diff-highlight を使います。

> あとは README に書いてあることの引き写しですが、PATH の通ったディレクトリに置いて、~/.gitconfig に以下のように設定を書く。

のようですが、`diff-highlight`の場所がわからなかったので探しました。

- [diff-highlight](https://github.com/git/git/tree/master/contrib/diff-highlight)

## 環境

- Mac OSX
- GitはHomebrewでインストール(2.6.2)

## diff-highlightを探す

Gitのインストールディレクトリを検索

```sh
$ which git
/usr/local/bin/git

$ ls -la /usr/local/bin/git
/usr/local/bin/git -> ../Cellar/git/2.6.2/bin/git

$ find /usr/local/Cellar/git/2.6.2 -name "diff-highlight"
/usr/local/Cellar/git/2.6.2/share/git-core/contrib/diff-highlight
/usr/local/Cellar/git/2.6.2/share/git-core/contrib/diff-highlight/diff-highlight
```

`share/git-core`の中に`contlib`がありました

## PATHを通す

そのままPAHTを通すとバージョン番号が入っているのでよろしくないです。

homebrewでいじってそうなディレクトリを探したら`/usr/local/share`にシンボリックリンクがはってありました。

```sh
ls -la /usr/local/share/git-core
/usr/local/share/git-core -> ../Cellar/git/2.6.2/share/git-core
```

gitをバージョンアップするとここのリンクを変更してるれるはずなので、最終的に以下のようにPATHを通しました。

```sh
sudo ln -s /usr/local/share/git-core/contrib/diff-highlight/diff-highlight /usr/local/bin/diff-highlight
```

## 結果

PATHを通して.gitconfigを編集すると

これが

![](/post/2015/11/diff-highlight-before.jpg)

こうなりました

![](/post/2015/11/diff-highlight-after.jpg)

ちなみにこのソースはCodeIQの問題に回答した時のものです。

- [Python3移行の準備、出来てますか？](https://codeiq.jp/q/2477)
