+++
date = "2015-10-16T00:32:19+09:00"
slug = "git-submodule-deinit"
tags = ["git"]
title = "gitのsubmodulesを削除する"

+++

毎度毎度検索しているのでメモ。

[Gitのsubmoduleをお手軽に削除する](http://raimon49.github.io/2015/04/04/git-submodule-deinit.html)にあるように`git deinit`を実行してコミットする

```sh
$ git deinit xxxxxxx
$ git rm xxxxxxx
$ git commit
```
