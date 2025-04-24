+++
date = '2025-04-24T22:12:04+09:00'
draft = false
title = 'ローカルからSSH鍵を使ってGithubにデプロイする方法'
tags = ["Github"]
categories = ["Git"]
+++

いつまでも`~/.netrc`に登録するのも面倒だったので、Githubに鍵を登録してPushする方法をメモする。

<!--more-->

## 鍵のペアを作成する

```sh
ssh-keygen -t ed25519 -C "example@example.com"
```

- 保存場所は`~/.ssh/id_ed25519`のままにした。
- パスフレーズは必要に応じて設定

## Githubに公開鍵を登録する

1. 公開鍵の内容をコピーする

```sh
cat ~/.ssh/id_ed25519.pub
```

2. Githubへ登録

- Github -> アイコン -> Settings
- 左メニューにある**SSH and GPG Keys** -> New SSH Key
- 適当なタイトルを入力して、コピーした内容を張り付ける -> Add SSH Key

## GitのリモートURLをSSHに変更する

```sh
git remote set-url origin git@github.com:your-username/your-repo.git
```
接続方式を、HTTPSからSSHに変更する

```sh
git remote -v
```
で、接続方式の確認ができる。

## SSH設定ファイル

`~/.ssh/config`に以下を追記する。
今回は無かったので新規作成した。

```sh
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes
```

## パーミッションを設定

```sh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

## Githubに接続できるかテスト

```sh
ssh -T git@github.com
```

成功すると、以下のようなメッセージが現れる。
> Hi Kamenleon! You've successfully authenticated, but GitHub does not provide shell access.

以上.

## 備考

これで、あとはいつも通り`git add hoge`からの流れでpushが出来る。

毎度パスワードを聞かれなくなるので大変楽になった。

