---
title: "WSL2でのweb開発環境構築tips"
date: 2021-06-30T19:11:23+09:00
draft: false
categories: ["wsl2", "docker"]
---

tipsというかWSL2に移行して開発していてハマったことや、定着した運用方法についてのメモ。

## 前提
- 主にRails開発をしていて、vim/tmuxとほぼターミナルとブラウザだけで開発作業を行っているのでその範囲。
- ubuntu-18.04利用。
- linuxの知識はある前提で書いている。
- WindowsTerminalを使用している


## Linuxインストールまで
[WSL2でLinuxを使おう | FJORD BOOT CAMP（フィヨルドブートキャンプ）](https://bootcamp.fjord.jp/articles/6)

僕がメンターをしているフィヨルドブートキャンプのこの資料で大丈夫です(ちょっと宣伝)

## ubuntuのバージョンについて
<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">WSL2/ubuntu20.04で capybara動かんのか… <br>NotImplementedError (fork() function is unimplemented on this machine)<br>が回避できぬ。<br>WSL2でも ubuntu18.04なら動くっぽいんだが…</p>&mdash; ゆーいち@イカX (@u1tnk) <a href="https://twitter.com/u1tnk/status/1345901647885529088?ref_src=twsrc%5Etfw">January 4, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

という事があり、20.04をやめて18.04を利用している。この時点なので今は不明。
GUIも無いし、WSL2だとデバイスドライバとかも関係無いので新しいメリットも無いので安定バージョンで良い。

## 起動と終了
ubuntuアプリを起動したり、wslコマンドで起動する。WindowsTerminalを使えば内部では wslコマンドを使っているので勝手に起動する。
終了するときは `wsl --shutdown` をpowershellなりで実行すれば良い。


## systemdが動いていない
本物のubuntuと違いsystemdが動いていないので、サービスの自動起動ができない。よってcrondとかも起動していないので注意。

自動起動はできないが動かないわけでは無いので、zshrc(shellによって調整してください)に以下のようなscriptをしこんでWindowsTerminalから開いたときに自動で起動するようにしている。
ここではdockerの起動とsmbサーバへのmountをやっている
```
#! /usr/bin/env ruby
exit unless `ps aux | grep docker | grep -v grep`.empty?

`sudo service docker start`

`sudo mount -t cifs -o username=xxxx,password=xxxx,iocharset=utf8 //192.168.100.1/share /mnt/share`
```

## dockerについて
docker for windowsを入れれば自動起動という点では問題無いが詳細記録し忘れたが何度かトラブルがあったし、自動起動の仕組みを整えたので使っていない。
インストールも通常のubuntu用の手順で問題無いし起動以外にdocker for windowsを使う理由は特に無いので。

## バックアップ/リストア
[「WSL」ディストリビューションのインポート・エクスポートはこんなに簡単！ - やじうまの杜 - 窓の杜](https://forest.watch.impress.co.jp/docs/serial/yajiuma/1220926.html)

のように wslコマンドを使えば簡単にできる。VirtualBox等のイメージと同じように使える。windowsはmacよりバックアップ関係が弱いので引越し時にとても助かる。

ただ、復旧した後にログインユーザがrootになり、起動ディレクトリがおかしくなる場合がある。対応は以下。

### ログインユーザを修正する。
2021年にもなって恐縮だが registry editで修正する。

[WSL2 starting as root when starting with wsl.exe · Issue #4276 · microsoft/WSL](https://github.com/microsoft/WSL/issues/4276)

を参考に

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss\{MY-UUID}` の `DefaultUid `の値をlinux上のuidを調べて入力する。

### 起動ディレクトリをhomeディレクトリにする
WinsowTerminalのプロファイル設定で、通常

`wsl.exe -d Ubuntu18.04`

のようになっているところを

`wsl.exe ~ -d Ubuntu18.04`

と修正する。

## その他
- 先に書いたtips
    - [WSL2でvim/tmuxでクリップボード共有できるようにする](https://u1tnk.github.io/blog/2021/06/28/wsl2_clipboard_config/)
    - [WSL2で rails test:system を動かす為の設定](https://u1tnk.github.io/blog/2021/06/28/wsl2_rails_test_system_config/)
- localhost/127.0.0.1でwslで立ちあげたサーバにアクセスできるが、実際は別のIPが振られていて、windows側で制御してくれている。
    - windowsのhostsに 127.0.0.1にマッピングしたhost名だと動かない現象が起きていたが、現在再現しないので改善されたと思われる。上述の起動スクリプトで自動更新するバッチを書いたばかりなのでちょっと悲しい :sob:
- クリップボード共有のところでも書いたが、WSL2からwindowsのexeが実行できる。不思議。
- 元はVirtualBoxでRLoginというsshクライアントを使っていた事もあり、当初はWindowsTerminalを使わずにsshで開発していたが、windows上のXServerがあまり安定しない事やフォントなどの扱いにクセがある事でいくつか不満点があったが、WindowsTerminalに移行してまったく問題無くなった。

## 雑感
VirtualBoxのような純粋なVMともmacのようにそのまま動いてるとも違うので最初は色々戸惑ったが、Windows+VirtualBoxに比べると起動は軽いし、安定していて明らかに改善された。

もちろんローカルで動いてるmacよりも重いはずだが、バックアップや環境のリセットが簡単なVMならではの利点があるし、macにはdocker for macという足枷があるので動作は軽い気がする。

m1 macより遅い可能性はあるが、私がmacを捨てた理由の大きな一つはハードウェアの選択肢が少ない事で、現状はコロナ禍もあって8コア32GB、nvme ssdと盛ったマシンパワーもあって、パフォーマンス面での不満はまったく無い。

欠点は困ったときに情報が少ない事なので、macに不満のある方は是非移行して情報を増やしましょう！
