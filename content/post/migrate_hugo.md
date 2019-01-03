---
title: "今さらOctopressからHugoに乗りかえた"
date: 2019-01-02T12:10:55+09:00
categories: ["octopress", "hugo"]
---


Octopressどころか自分でブログをビルドする事自体が乗り遅れてる事は自覚している。

だが、乗りかえに労力を割くのもイヤでそのままにしてたらマシン乗りかえてブログ書く度にgemでハマる。
Rails開発者なのでruby環境自体は苦では無いがOctopressもはやメンテもされてないので見つからないgemとかあって不毛でしかない。

マネージドサービスも変なところもイヤだし金もかけたくないし、アフィリンク入れたページもあるので規約とか確認するのもめんどい。
という事で流行ってたhugoにすることにした。aptで入ったしgoでできたコマンド一個で動くようなのでOctopressよりも過去互換性が高そうなので、延命処置には十分かと。jekyllも考えたがより流行ってそうな方にしただけ。

## ざっくり乗り替え手順

1. 公式の https://gohugo.io/tools/migrations/ にあった https://github.com/codebrane/octohug を実行。ポスト少ないのでvimで頑張ってフォロー。公式だから信用したがたぶんダメツール。
    - slug(titleじゃないaliasのようなもの？) に日付入りのpathが入っていて、permalinksにはfilenameを使うので消した
    - categoriesがCategoriesと間違っていてしかも中身が消えていた。
    - titleがfilenameになっていた。日本語タイトルを元からコピー。
1. themeを選定。参考サイトで使っていた[Hugo Octopress | Hugo Themes](https://themes.gohugo.io/hugo-octopress/) を利用。
    - Octopressにこだわる理由はカケラも無いが、ある程度設定するとthemeごとに挙動が結構変わるので変えるのを諦めた。
    - 以下、公式の設定にあるっぽいが対応してなかったのでforkして修正
        - DateFormatが変えられない
1. 動作確認しつつ、config.toml等を設定
    - themeの方にサンプルがあったので編集していった
1. 固定ページは content/hoge.md に移動。
1. 元のoctopressのデータを削除してhugoのファイルをコピーして以下を submoduleにする
    - themes以下にthemeを配置
    - publicにxxx.github.io.git  (デプロイ用)


### permalinkを合わせる
```
[permalinks]
  post = "/blog/:year/:month/:day/:filename/"
```

### rssのurlを合わせる
octopressのときは atom.xmlで hugoだと index.xmlだったので合わせた。rssuriという情報もあるが deprecated。
また、Hugo-Octopressではindex.xmlが直書きだったので無効にしてメニュー作成時に自力でリンクを作った。どうせHugo-Octopressはforkしたので書き替えても良かった。
```
[outputFormats.RSS]
baseName = "atom"
```

### デプロイ

1. 以下で public を xxx.github.io.git にする。
```
git submodule add -b master git@github.com:<USERNAME>/<USERNAME>.github.io.git public
```

1. サイト生成
```
hugo
```

1. デプロイ実行
```
cd public
git push origin master
```


### ローカルテスト
以下コマンド後 localhost:1313
```
hugo server --buildDrafts
```


### 参考サイト
- [Host on GitHub | Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
- [Migrate to Hugo | Hugo](https://gohugo.io/tools/migrations/)
- [HugoのRSS feedのURIを変更するのにrssuriを使うのはdeprecatedだった](https://devnull.namaraii.com/post/2018-02-17-hugo-rssuri/)
lkjkj
