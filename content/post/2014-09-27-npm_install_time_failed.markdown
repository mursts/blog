---

title: "npmでtimeモジュールのインストールに失敗"
date: 2014-09-27
slug: "npm-time-module-install-failed"
comments: true
tags: ["npm"]
---


npmでtimeモジュールをインストールしたら以下のようにエラーが発生しました。

<!--more-->

```bash
$ npm install time

time@0.11.0 install /path/to/node-mojules/time
node-gyp rebuild

gyp ERR! configure error
gyp ERR! stack Error: Python executable "python" is v3.4.1, which is not supported by gyp.
gyp ERR! stack You can pass the --python switch to point to Python >= v2.5.0 & < 3.0.0.
gyp ERR! stack     at failPythonVersion (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/configure.js:108:14)
gyp ERR! stack     at /usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/configure.js:97:9
gyp ERR! stack     at ChildProcess.exithandler (child_process.js:646:7)
gyp ERR! stack     at ChildProcess.emit (events.js:98:17)
gyp ERR! stack     at maybeClose (child_process.js:756:16)
gyp ERR! stack     at Socket.<anonymous> (child_process.js:969:11)
gyp ERR! stack     at Socket.emit (events.js:95:17)
gyp ERR! stack     at Pipe.close (net.js:465:12)
```

どうやらモジュールで使用するgypがPython3.4.1は対応していなくて
Python \>= v2.5.0 & \< 3.0.0 のようです。

自分はpyenvで対応しましたが、npm的には以下のように設定するようです。

```bash
$ npm install --python=python2.7
$ npm config set python python2.7
```

### 参考

[http://stackoverflow.com/questions/20454199/how-to-use-a-different-version-of-python-duing-npm-install](http://stackoverflow.com/questions/20454199/how-to-use-a-different-version-of-python-duing-npm-install)
