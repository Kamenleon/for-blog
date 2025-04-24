+++
date = '2025-04-07T21:00:00+09:00'
draft = false
title = 'Hugo + Stackでブログを作るまで'
tags = ["Hugo"]
categories = ["雑記"]
+++

ブログを作ったので記念に

<!--more-->

ChatGPTに聞きつつ進めたのだが、それでも苦労するところがあったのでメモとして。
WSL上で構築している。

以下の記事は大変参考にさせていただいた。
[Hugo+Stackテーマの導入メモ](https://yamamoto-yuta.github.io/p/hugo-stack-theme-installation-notes/)

## Hugoのインストールとセットアップ

以下の方法でインストールしたのだが、build日が2023年で最新っぽくなかったため、インストールしなおし。
```sh
sudo apt install hugo
hugo version
```

Hugoを一度消す
```sh
sudo apt remove hugo
```

[Hugoのリポジトリ](https://github.com/gohugoio/hugo/releases)から最新バージョンを指定してインストールして解凍。

テーマは後から決めるかもしれないが、このサイトのようにStackを選ぶなら`extended`と書かれたバージョンをインストールすること。

```sh
wget https://github.com/gohugoio/hugo/releases/download/v0.145.0/hugo_extended_0.145.0_Linux-64bit.tar.gz
tar -xvzf hugo_extended_0.145.0_Linux-64bit.tar.gz
```

Hugoをシステムに配置する。
これでどこからでもHugoコマンドが打てるようになる。

```sh
sudo mv hugo /usr/local/bin/
```

バージョン確認
```sh
hugo version
```

インストールしたバージョンが表示されたら成功、ヨシ！

## Hugoサイトの作成

サイトの作成

```sh
hugo new site for-blog
cd for-blog
```

テーマを適用する（[公式のテーマ一覧](https://themes.gohugo.io/)）

```sh
git init
git submodule add https://github.com/CaiJimmy/hugo-theme-stack.git themes/hugo-theme-stack
```

記事作成
```sh
hugo new content/posts/first-post.md
```

content/posts/first-post.md を編集する

```sh
hugo server -D
```

[localhost:1313](http://localhost:1313)にアクセスして動作確認

## hugo.yamlを調整

以下の記事を参考に、外国語対応などの設定を外して調整

> 参考:
> - [非エンジニアの初心者がHugo(テーマStack)+GitHub Pagesでブログを開設するまで](https://miiitomi.github.io/p/hugo/)
> - [yamamoto-yuta.github.io](https://github.com/yamamoto-yuta/yamamoto-yuta.github.io/blob/main/hugo.yaml)

出来たもの
- [hugo.yaml](https://github.com/Kamenleon/for-blog/blob/main/hugo.yaml)

## 日本語フォント対応

> - [Hugo+Stackテーマの導入メモ](https://yamamoto-yuta.github.io/p/hugo-stack-theme-installation-notes/)


あとはGithubにプッシュして、Pages等の設定を行って公開する。
手順残してなかったので、またもう一回作る時があれば追記する…。
