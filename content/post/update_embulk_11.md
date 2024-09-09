---
title: "embulk 0.9系から0.11系へのアップデートメモ"
date: 2024-09-08T22:57:30+09:00
draft: false
---

アップデート時に結構ハマったので最低限やったことのメモ。(免責: 環境による)

参考: [Embulk v0.11 でなにが変わるのか: ユーザーの皆様へ](https://zenn.dev/dmikurube/articles/what-changes-in-embulk-v0-11)


# 前提
元の環境でやっていたこと。
- embulk.jarをDLしてpathの通った場所に置く。
- 必要なgemを `embulk gem install` でinstall
- rubyスクリプトで生成した設定をrunに渡して実行

# やったこと

- DLするembulk.jarを 0.11.4にした
- JRubyの最新版のjarをDL
- propertiesに設定
  - `$HOME/.embulk/embulk.properties` に作成
  - JRUBYのjarを指定
    - `jruby=file:///path/to/jruby.jar`
  - gem_home
    - `gem_home=/path/to/bundle`
    - rubyから実行すると GEM_HOMEが設定されているせいかSHELLのときとズレてしまったので設定
- embulkの呼び出し方を `java -jar /path/to/embulk.jar` に変更
- embulkの`gem install` に embulk自体を追加
  - msgpackもinstallする必要があるとdocumentにあったが、他のgem install時に入ったようなのでやっていない