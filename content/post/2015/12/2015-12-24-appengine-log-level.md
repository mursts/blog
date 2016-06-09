+++
date = "2015-12-24T00:36:12+09:00"
slug = "google-app-engine-logging-debug"
tags = ["GoogleAppEngine", "Python"]
title = "Google App EngineでDebugログを出力する"

+++

App Engine(Python)で以下のようにした場合

```python
import logging

logging.debug('spam')
logging.info('egg')
```

```
egg
```

のようにdebugログが表示されません。

<!--more-->

Debugレベルのログを出す場合は、サーバー起動のオプションで

```sh
$ dev_appsever.py --log_level=debug .
```

と起動してやるとログが表示されます。

```python
import logging

logging.debug('spam')
logging.info('egg')
```

```
spam
egg
```
