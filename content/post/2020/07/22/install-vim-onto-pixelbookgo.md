---
title:       "Pixelbool GoにNeoVimをインストールする"
subtitle:    ""
description: ""
date:        2020-07-22
author:      ""
image:       ""
tags:        ["vim", "neovim", "pixelbookgo", "chromebook"]
---

Pixelbook GoのLinux環境にNeoVimの環境を作ったのでやったことの覚書

## 環境

Crostini on Pixelbool Go

```sh
$ uname -a 
Linux penguin 4.19.113-08528-g5803a1c7e9f9 #1 SMP PREEMPT Thu Apr 2 15:21:14 PDT 2020 x86_64 GNU/Linux
```

## NeoVimのインストール

インストールはHomebrewで行いました

```sh
$ brew install neovim

$ nvim --version
NVIM v0.4.3
Build type: Release
LuaJIT 2.0.5
Compilation: /home/linuxbrew/.linuxbrew/Homebrew/Library/Homebrew/shims/linux/super/gcc-5 -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=1 -DNDEBUG -DMIN_LOG_LEVEL=3 -Wall -Wextra -pedantic -Wno-unused-parameter -Wstrict-prototypes -std=gnu99 -Wshadow -Wconversion -Wmissing-prototypes -Wimplicit-fallthrough -Wvla -fstack-protector-strong -fdiagnostics-color=auto -DINCLUDE_GENERATED_DECLARATIONS -D_GNU_SOURCE -DNVIM_MSGPACK_HAS_FLOAT32 -DNVIM_UNIBI_HAS_VAR_FROM -I/tmp/neovim-20200210-7080-2a9sbb/neovim-0.4.3/build/config -I/tmp/neovim-20200210-7080-2a9sbb/neovim-0.4.3/src -I/home/linuxbrew/.linuxbrew/include -I/tmp/neovim-20200210-7080-2a9sbb/neovim-0.4.3/deps-build/include -I/usr/include -I/tmp/neovim-20200210-7080-2a9sbb/neovim-0.4.3/build/src/nvim/auto -I/tmp/neovim-20200210-7080-2a9sbb/neovim-0.4.3/build/include
Compiled by root@55c83797796d

Features: +acl +iconv +tui
See ":help feature-compile"

   system vimrc file: "$VIM/sysinit.vim"
  fall-back for $VIM: "
/home/linuxbrew/.linuxbrew/Cellar/neovim/0.4.3_1/share/nvim"

Run :checkhealth for more info
```

`init.vim` を `$HOME/.config/nvim/` に作成する

## dein.vim をインストール

[dein](https://github.com/Shougo/dein.vim) はプラグインを管理するプラグイン。昔で言うところのNeoBundle  
キャッシュの作成先は標準っぽい `$HOME/.cache` にしたけど、`$HOME/.vim` に作ってる人も見かける

```sh
curl https://raw.githubusercontent.com/Shougo/dein.vim/master/bin/installer.sh > installer.sh
sh ./installer.sh ~/.cache/dein
```

インストールが終わったら画面に出ている設定をinit.vimに書く。ただ、デフォルトの設定にすると管理するプラグインや設定をinit.vimに全部書くことになるので、プラグイン管理はtomlファイルに外出しする

設定ファイルのディレクトリ構造は参考サイト通りにした  
> [fish shell上で快適(個人的感想)なneovim&python環境を構築してみた](https://dev.classmethod.jp/articles/fish_neovim_python_synergy/)

```sh
$ tree $HOME/.config/nvim

├── dein
├── init.vim
├── keymap
├── options.rc.vim
└── plugins
```

init.vim  
```vim
"dein Scripts-----------------------------
if &compatible
  set nocompatible               " Be iMproved
endif

" Required:
set runtimepath+=$HOME/.cache/dein/repos/github.com/Shougo/dein.vim

" Required:
if dein#load_state('~/.cache/dein')
  call dein#begin('~/.cache/dein')

  " TOML を読み込みキャッシュしておく
  let g:rc_dir    = '~/nvim/.config/dein'
  let s:toml      = g:rc_dir . '/dein.toml'
  let s:lazy_toml = g:rc_dir . '/dein_lazy.toml'
  call dein#load_toml(s:toml,      {'lazy': 0})
  call dein#load_toml(s:lazy_toml, {'lazy': 1})

  " Required:
  call dein#end()
  call dein#save_state()
endif

" Required:
filetype plugin indent on
syntax enable

" If you want to install not installed plugins on startup.
if dein#check_install()
  call dein#install()
endif

"End dein Scripts-------------------------
```

## NeoVim用のPython環境を作る

プラグインの中にはPythonが必要なものもあるので専用の環境をつくる  
使用するのは最初から入っているPython3

```
$ which python3
/usr/bin/python3

# venvは入ってないためインストール
$ sudo apt install python3-venv

# ~/.venv に作成する
$ mkdir ~/.venv
$ python3 -m venv ~/.venv/nvim

$ source ~/.venv/nvim/bin/activate
$ pip install pynvim
$ pip list
Package       Version
------------- -------
greenlet      0.4.16 
msgpack       1.0.0  
pip           18.1   
pkg-resources 0.0.0  
pynvim        0.4.1  
setuptools    40.8.0 

$ deactivate
```

init.vimにPythonのパスを追加  

> [neovim で python3 の環境を安定的に運用するために pyenv/virtualenev 化した。](https://takuya-1st.hatenablog.jp/entry/2019/03/12/174443)  

```vim
let g:python3_host_prog = '~/.venv/nvim/bin/python'
```

設定ファイルを空で作ってmvimを起動する

```sh
$ touch $HOME/.config/nvim/dein.toml
$ touch $HOME/.config/nvim/dein_lazy.toml

$ nvim
```

`':CheckHealth`を実行してPython3でエラーになってなければOK。Python2は一旦無視する

あとは良さそうなプラグインを見つけて入れていく
