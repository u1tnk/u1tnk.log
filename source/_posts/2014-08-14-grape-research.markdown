---
layout: post
title: "Grapeが良さそうだったけどやめた"
date: 2014-08-14 11:19
comments: true
categories: ruby rails
---

RailsでAPI作ってるのでGrapeが良さそうと聞いて調べて、おお！やってみよう！
と思って細かく調べたけど結果やめました。

### 参考資料

- https://github.com/intridea/grape
- [Ruby - RailsとGrapeで行う最高のWeb API開発 - Qiita](http://qiita.com/anoworl/items/756f01cc3d188ebad139)
- [Grape - RailsでスピーディにAPIを作成！ - 酒と泪とRubyとRailsと](http://morizyun.github.io/blog/rails-grepe-api-heroku-ruby/)
- [Ruby初心者がRailsとGrapeでREST APIを作る - morishitter blog](http://morishitter.hatenablog.com/entry/2014/03/12/033321)


### 良さそうな点

- routes情報と全controllerが一つにまとまるので見易い
- versioningやapiのprefix等柔軟に設定できる

### 微妙なところ

- filterにonly,except等が無かった
- 1ファイルで見通しが良いけども、増え過ぎて分割したら何の為にやったのか感が凄そう
- 単体で使えるものなのでRailsと重複する機能が多くてもったいない気持ちになる

### まとめ

カバー範囲が違えど、二種のフレームワークを混在して使う事への抵抗が大きかった。大して大きな機能が無いので、だったらRailsのみでシンプルにした方が良いかなと。

Railsと組み合わせて使うなら既存の非APIアプリに追加してシンプルなちょっとしたAPIを作る…とかなら良いのかもという気はしました。

