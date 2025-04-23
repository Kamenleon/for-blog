+++
date = '2025-04-10T21:22:37+09:00'
draft = false
title = 'WSL + Neovimでクリップボードを使えるようにする'
tags = ["Neovim", "WSL"]
categories = ["エディタ"]
+++

WSLでNeovimにコピペできるようにするまで

<!--more-->

## Version

OS：Windows11

Neovim: v0.10.0

## Neovimの設定

`init.lua`に以下の設定を入れる。
```lua
vim.opt.clipboard = "unnamedplus"
```
未検証：元から設定されている場合もありそう？

Neovimを起動し、コマンドモードで以下の入力を入れる。
```vim
:echo &clipboard
```
出力が`unnamedplus`になっていれば成功。

## OSごとの追加設定

### Windows(WSL)

1. `win32yank`のインストール

以下はバージョン0.1.1の例だが、最新版を確認して適宜修正すること。
```sh
curl -Lo /tmp/win32yank-x64.zip https://github.com/equalsraf/win32yank/releases/download/v0.1.1/win32yank-x64.zip
```

2. 解凍する
```sh
unzip /tmp/win32yank-x64.zip -d /tmp/
```

3. 解凍したファイルを設定フォルダに移動させる。
chmodコマンドで実行権限を付与する。
```sh
sudo mv /tmp/win32yank.exe /usr/local/bin/
sudo chmod +x /usr/local/bin/win32yank.exe
```

4. 動作確認

```sh
# Windowsのクリップボードに`test`を貼り付け
echo "test" | /usr/local/bin/win32yank.exe -i
# Windowsのクリップボードの内容を出力
/usr/local/bin/win32yank.exe -o 
```
これで、`p`, `Ctrl + V`, `Ctrl + Shift + V`などのコマンドで、クリップボードの内容を貼り付けることが出来る。

Neovim -> Windows方向も可能。

## Ctrl + Vが乗っ取られる現象の解決

張り付けられるようになったのはよいのだが、今度はCtrl + Vで自動的に貼り付けが発生してしまう。

これはWindows11でのWindows Terminalの設定に影響されているため、解除する。

### 結論
公式ホームページに全ての答えが載っている。
[設定の JSON ファイル](https://learn.microsoft.com/ja-jp/windows/terminal/install#settings-json-file)

### 方法

1. Windows Terminalの設定を開く

`Ctrl + Shift + Space`、又は画面上の▽のようなボタンを押す。設定を選択する。

2. 左側のハンバーガーメニューから「操作」を選択する。
3. 「貼り付け」というコマンドを探し、Ctrl + Vのキーバインドを削除する。

※Ctrl + Shift + Vの「貼り付け」については、邪魔でないのならば残しておいてもよい。


これで、Neovim起動時のCtrl + Vではビジュアルブロックモードが使えるようになる。

## 備考
会社のPCで導入してみたみたところ、コピー内容の出力に5秒程度かかった。
アンチウィルスソフトのせいかWindows Defenderのせいか、それとも他によるものかは分からず…。
PC更新後にまた試してみる。

## 参考

- [equalsraf/win32yank](https://github.com/equalsraf/win32yank)
- [Windows ターミナルでのカスタム アクションとキーバインド](https://learn.microsoft.com/ja-jp/windows/terminal/customize-settings/actions#unbind-keys-disable-keybindings)
