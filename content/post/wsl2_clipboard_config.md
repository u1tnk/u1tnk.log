---
title: "WSL2でvim/tmuxでクリップボード共有できるようにする"
date: 2021-06-28T15:26:11+09:00
draft: false
categories: ["wsl2", "docker"]
---


## 概要
vim、tmux、windws側のcopy/pasteを全て連携させる。
WSL2移行前に使っていたVIrtualBox環境からの移行で、結局の所ssh+x転送をやめた結果 xselが使えなくなったのでwindowsのexeを直接叩けばOKだった…というだけ。

# windows側のクリップボードを双方向で使えるアプリをインストール
https://github.com/equalsraf/win32yank  を利用。

windowsにデフォルトに入っている `clipboard.exe` はクリップボードへの書き込みはできるが、逆はできず。
逆は https://docs.microsoft.com/ja-jp/powershell/module/microsoft.powershell.management/get-clipboard?view=powershell-5.1 で可能なようだがWSL側から動かす方向が分からなかったので手軽なコレを採用した。
インストールしてWSL側のPATHに入れておく。

## tmux
以下設定は vim形式のキーバインド前提となっている。

tmux.conf
```
bind-key -T copy-mode-vi y send-keys -X copy-pipe-and-cancel "win32yank.exe -i"
bind p run-shell 'win32yank.exe -o | xargs -0 -I{} tmux set-buffer -- {} && tmux paste-buffer'
```

## vim
tmuxとは関係なくコレだけで共有できる。WSL便利というか不思議というか。
vimをclipboard利用できるようにビルドする必要はあるが、情報はいくらでもあるので割愛。
個人的にはneovimを使っているので最初から使えた。

```
set clipboard&
set clipboard^=unnamedplus
```

## 所感
まとめてみると記事にまとめる意味あんのかコレ。というレベルの内容になったが、WSL2の情報は少なくて、当初うまく動かないときにXServer入れないとvimからクリップボード使えないのでは？とか標準だから clipboard.exeとか使ってハマったりしたので、たったこんだけでできるよ！という事で。

当初これのやり方が分からず、WSL2でもsshサーバ起動してRLoginからsshしていたりしていたが、XServerの不調とかにコピペ失敗して気づいてXServer再起動したり、ひどいときにはXServer再起動に失敗してwindows再起動したり… :sob:  などのイライラ一切無く、超安定
するようになっている。満足。

