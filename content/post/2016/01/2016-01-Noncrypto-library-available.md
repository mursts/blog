+++
date = "2016-02-04T00:46:15+09:00"
slug = "python-no-crypto-library-available"
tags = ["Python"]
title = "No crypto library availableと出た場合"

+++

`google-api-python-client`を使ったAPI認証時に

```sh
Traceback (most recent call last):
  File "xxxxx.py", line 30, in <module>
    main()
  File "xxxxx.py", line 18, in main
    scope)
  File "python3.5/site-packages/oauth2client/util.py", line 140, in positional_wrapper
    return wrapped(*args, **kwargs)
  File "python3.5/site-packages/oauth2client/client.py", line 1630, in __init__
    _RequireCryptoOrDie()
  File "python3.5/site-packages/oauth2client/client.py", line 1581, in _RequireCryptoOrDie
    raise CryptoUnavailableError('No crypto library available')
oauth2client.client.CryptoUnavailableError: No crypto library available
```

<!--more-->

となった場合、`oauth2client/client.py`の`_RequireCryptoOrDie`をみると

> The oauth2client.crypt module requires either PyCrypto or PyOpenSSL
> to be available in order to function, but these are optional
> dependencies.

とあるので、どちらかをインストールします。

```sh
$ pip install PyCrypto
or
$ pip install PyOpenSSL
```

PyOpenSSLは薄いラッパーモジュールのようなのでこっちにしました。

はじめはpipで一緒に入れてくれればと思いましたが、どちらでも大丈夫なようにあえて依存関係をもたせてないようですね。
