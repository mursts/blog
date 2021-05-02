+++
date = "2021-05-02"
slug = "install-grive"
title = "Google DriveのクライアントにGrive2を使う"
tags = ["linux", "google-drive"]
isCJKLanguage = true
+++

[Grive2](https://github.com/vitalif/grive2) をLinuxにインストールしてGoogle Driveとファイルを同期するためのメモ。

{{% blogcard "https://github.com/vitalif/grive2" %}}

## 環境

```sh
$ uname -a
Linux HomeMachine 5.10.32-1-MANJARO #1 SMP Wed Apr 21 14:54:10 UTC 2021 x86_64 GNU/Linux
```

## インストール

aurにあるのでyayでインストール

```sh
$ yay -S grive
```

## 設定

同期するディレクトリを作成

```sh
$ mkdir ~/google-drive
$ cd ~/google-drive
```

すべてのファイルを同期したくないので `.griveignore` を作って必要なものを指定する

```sh
$ vi .griveignore
```

以下を参考にファイルを作成しました

https://hirose31.hatenablog.jp/entry/2019/07/11/200926

## 認証情報を作成

デフォルトの認証情報を使うとエラーになるので、公式を参考にクライアントIDとクライアントシークレットを作成する

https://github.com/vitalif/grive2#different-oauth2-client-to-workaround-over-quota-and-google-approval-issues

## 実行

```
$ grive -a -id <クライアントID> --secret <クライアントシークレット>
```

しばらく待つと同期が完了する

## 定期的な同期の設定

https://github.com/vitalif/grive2#scheduled-syncs-and-syncs-on-file-change-events

`inotify-tools` が必要なのでインストールしておく

```sh
$ sudo pacman -S inotify-tools

$ systemctl --user enable grive-timer@$(systemd-escape google-drive).timer
$ systemctl --user start grive-timer@$(systemd-escape google-drive).timer
$ systemctl --user enable grive-changes@$(systemd-escape google-drive).service
$ systemctl --user start grive-changes@$(systemd-escape google-drive).service
```

この設定で5分毎に `/usr/lib/grive/grive-sync.sh` を実行するようになる。
そのままだと、下記のように同期が失敗するのでシェルを一部書き換える

```
Syncing google-drive...
Failed to refresh auth token: HTTP 401, body:
exception: /home/mursts/.cache/yay/grive/src/grive2-0.5.1/libgrive/src/protocol/OAuth2.cc(111): Throw in function void gr::OAuth2::Refresh()
Dynamic exception type: boost::wrapexcept<gr::OAuth2::AuthFailed>
[gr::expt::BacktraceTag*] = #0 0x5585a0f7f9be grive gr::Exception::Exception()
#1 0x5585a0f75b0a grive gr::OAuth2::Refresh()
#2 0x5585a0f76199 grive gr::OAuth2::OAuth2(gr::http::Agent*, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&)
#3 0x5585a0f1d653 grive Main(int, char**)
#4 0x5585a0f1a96d grive main
#5 0x7fd667f5fb25 /usr/lib/libc.so.6 __libc_start_main
#6 0x5585a0f1b43e grive _start


Sync of google-drive done.
```

grive-sync.shはデフォルトの認証情報を使うようになっているので、自分のものを使うように書き換える

```sh
$ diff grive-sync.sh.bk grive-sync.sh
88c88,89
<           grive -p "${_directory}" 2>&1 | grep -v -E "^Reading local directories$|^Reading remote server file list$|^Synchronizing files$|^Finished!$"
---
>           grive -p "${_directory}" --id <自分のクライアントID> --secret <クライアントシークレット> 2>&1 | grep -v -E "^Reading local directories$|^Reading remote server file list$|^Synchronizing files$|^Finished!$"
```

同期時のプロセスに認証情報が出てしまうので自己責任で
