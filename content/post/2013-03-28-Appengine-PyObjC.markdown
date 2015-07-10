---

title: "App EngineでPyObjCがないと警告が出た"
date: 2013-03-28
comments: true
tags: ["AppEngine", "Python"]
---
# 

App Engine/Pで開発用サーバを起動したら警告が出るようになりました。

<!--more-->

```
INFO     2013-03-27 15:45:42,742 api_server.py:152] Starting API server at: http://localhost:60355
/Applications/GoogleAppEngineLauncher.app/Contents/Resources/GoogleAppEngine-default.bundle/Contents/Resources/google_appengine/google/appengine/tools/devappserver2/file_watcher.py:97: UserWarning: Detecting source code changes is not supported because your Python version does not include PyObjC (http://pyobjc.sourceforge.net/).
Please install PyObjC or, if that is not practical, file a bug at http://code.google.com/p/appengine-devappserver2-experiment/issues/list.
  warnings.warn('Detecting source code changes is not supported because '
```

「PyObjCがないからソースの変更が監視できないよ」ってなかんじだと思います。
確かに、ソースを変更しても変更が反映されていませんでした。

最近pyenvで環境を作りなおしたし、SDKも更新したのでどっちが原因かはわかりませんが、警告がでているのでPyObjCをインストールして解決しました。

```
$ pip install PyObjC
```

インストール完了まで少し時間がかかりました。

