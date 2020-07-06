+++
date = "2020-07-04"
slug = "setting-chromebook"
title = "Chromebookの設定をする"
tags = ["chromebook", "pixelbookgo"]
isCJKLanguage = true
+++

Chromebook(主にCrostini)の設定をしたのでメモ

## シェルの設定

> https://techracho.bpsinc.jp/hachi8833/2019_06_06/66396  
> https://www.blog.v41.me/posts/1d962586-8b1d-49d1-8f4f-dddea4e1db71

を参考に `./bash_profile` を作って、その中で `.profile` と `.bashrc` を読む

## ターミナルの設定

設定画面は `ctrl + shift + p` で開く

### ビープ音を消す 

設定の [サウンド] - [Alert bell sound (URI)] を空白にする  
デフォルトは `lib-resource:hterm/audio/bell`

### 日本語入力に切り替えれるようにする

デフォルトではターミナル上でctrl + space がきかない  
設定の [キーボード] - [Keybaord bindings/shortcuts] に以下を追加

```
{
  "Ctrl-Shift-Space": "PASS",
  "Ctrl-Space": "PASS"
}
```

> https://qiita.com/komde/items/25b4c80598d7e2b679f6#linux-%E3%82%BF%E3%83%BC%E3%83%9F%E3%83%8A%E3%83%AB%E4%B8%8A%E3%81%A7%E8%8B%B1%E8%AA%9E%E3%82%AD%E3%83%BC%E3%83%9C%E3%83%BC%E3%83%89%E3%81%A7%E6%97%A5%E6%9C%AC%E8%AA%9E%E5%85%A5%E5%8A%9B%E3%81%AE%E3%82%AA%E3%83%B3%E3%82%AA%E3%83%95%E3%82%92%E3%81%99%E3%82%8B

### ペースト

設定じゃないけど  
`ctrl + shift + v` 

## Homebrew をインストール

```sh
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

`/home/linuxbrew/.linuxbrew` にインストールされます

その他必要なものをインストール & 設定

```sh
$ sudo apt install build-essential
$ echo 'eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)' >> $HOME/.bash_profile
$ source ~/.bash_profile
```

## Jetbrains Toolbox をインストール

brew cask install はLinux未対応のため手動でインストールする

```sh
$ wget https://download.jetbrains.com/toolbox/jetbrains-toolbox-1.17.7139.tar.gz
$ tar -zxf jetbrains-toolbox-1.17.7139.tar.gz -C ~/
$ ./jetbrains-toolbox-1.17.7139/jetbrains-toolbox
./jetbrains-toolbox: error while loading shared libraries: libnss3.so: cannot open shared object file: No such file or directory

# libnss3.so がないのでインストール
$ sudo apt install -y libnss3

$ ./jetbrains-toolbox-1.17.7139/jetbrains-toolbox
```

