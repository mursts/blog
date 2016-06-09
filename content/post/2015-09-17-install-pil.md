+++
date = "2015-09-17T23:35:17+09:00"
slug = "install pil"
tags = ["Python"]
title = "PILをインストールする"

+++

Python2.7の環境にpipでPILをインストールしようとすると↓のエラーがでました。

```bash
Could not find a version that satisfies the requirement PIL (from versions: )
Some externally hosted files were ignored as access to them may be unreliable (use --allow-external PIL to allow).
No matching distribution found for PIL
```

<!--more-->

`--allow-external PIL` を付けろとのこと

```bash
$ pip install PIL --allow-external PIL
```

今度は

```
Could not find a version that satisfies the requirement PIL (from versions: )
Some insecure and unverifiable files were ignored (use --allow-unverified PIL to allow).
No matching distribution found for PIL
```

`--allow-unverified PIL` も付けろとのこと

```bash
$ pip install PIL --allow-external PIL --allow-unverified PIL
```

両方つけると今度はfreetypeのヘッダーが見つからないみたい。

```bash
_imagingft.c:73:10: fatal error: 'freetype/fterrors.h' file not found
#include <freetype/fterrors.h>
         ^
1 error generated.
error: command 'clang' failed with exit status 1
```

freetypeはインストール済みのはず。
しかしヘッダーファイルをみると`freetype2`となっていた

```
$ brew search freetype
freetype (installed)
```

```
freetype2 -> ../Cellar/freetype/2.6_1/include/freetype2
```

なのでシンボリックリンクをはって、再度インストール

```
$ ln -s /usr/local/Cellar/freetype/2.6_1/include/freetype2 /usr/local/include/freetype

$ ll /usr/local/include/

freetype -> /usr/local/Cellar/freetype/2.6_1/include/freetype2

$ pip install PIL --allow-external PIL --allow-unverified PIL
Successfully installed PIL-1.1.7
```

インストール成功

ぐぐっても皆さん同じような方法でインストールしているっぽい。

[Error installing Python Image Library using pip on Mac OS X 10.9](http://stackoverflow.com/questions/20325473/error-installing-python-image-library-using-pip-on-mac-os-x-10-9)

ちなみに、pipでオプションがいるのは [pyenvでvirtualenvしててPILがインストール出来ない件](http://qiita.com/noblejasper/items/ee29e06ccb82ce97af5a) を参照
