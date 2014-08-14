---
layout: post
title: "ansible 勉強会 #1メモ"
date: 2014-06-19 10:35
comments: true
categories: ansible 
---

[Ansible 勉強会 #1 - connpass](http://ansible-users.connpass.com/event/5968/)

に行ってきたのでメモ。資料は↑にほぼあるようです。

### Ansible の基本 + MySQL レプリケーションを設定する事例  by @IanMLewis
まさに基本。普段使ってるので大体押さえられてた。

#### MySQLを構築してみる
- bennojoy/ansible-roles ansibleworksの人のリポジトリらしい。参考になりそう。
- when あんま使いこなしてないので見とこう。
- サンプルyaml見ると、自分のyamlが使いこなして無い感ある…
- mysql_replicationモジュールは便利感あるが、ちょっと簡単そうに見えないw 基本の直後に見るのはキツい印象。


### dynamic inventoryが便利な話: no more host list!  by  @tagomoris

- 某巨大メッセージングアプリ企業で普通に使ってるっぽい
- エージェントいらないからCentOS5でも動くという理由が大きいらしい。納得。
- 日本語情報あんま無いのが弱点は同意。っつーか俺書け。
- Inventryに普通のvars定義できたという恐らくかなり基本に気づいてなかった…
- hoge[1:2].local → hoge1.local…とか使える。
- Dyanamic Inventry存在すら気づいてなかった…
- ec2用のscriptは既存である。
- JSON返すだけなので自作もラク。
- ansible.cfg置けば -i不要という事に気づいた。
- 明日からマネしたい！
- 2000台完走実績あるらしい。さすが某巨大メッセージングアプリ企業

#### おまけ
DynamicInventryでは_meta使って —host はスキップさせるのがオススメ。


### callback pluginを使ったタスク実行時間の可視化について by @rudy

- pluginはmoduleと違ってpythonで本体を拡張。
- -i “localhost, ” とかできる事に気づいて衝撃。
- callback_plugins に入れるだけ。実行ホストに置けば良いのラクだな。
- 実用上は使わない気がするが、bot連携すると面白いかも。


###  AnsibleのJUNOS設定モジュールを書いてみた by @saito_hideki
Ansibleでルータ設定をしているという、まったく参考にはならないが衝撃の発表。

### MSPだってansibleしたい by @LaughK
- MSP(運用、保守代行)
- 客のサーバだからに(ほとんど)何も入れなくて良いという理由。

### 目指せ、サーバー構築1200台 by @myb1126
- 採用はエージェントレスが決め手との事。

### 構成管理ツールAnsibleの構成管理について by @iktakahiro
- playbookだけディレクトリ分けるのアリかも。
- 「冪等性は忘れよう」w
- 発表されてたのは複雑な気がする…とはいえbest practiceは複数サービスになると厳しいので、考えないとまずそう。

### Vagrant × Ansible by @tomohiro_urakawa
- inventoryはvagrantが生成すんのか。べんりー
- Vagrant provisionで実行
- 正直言うとそれでもわざわざVagrantに実行させるメリットが見えない…っつーか小さくしか見えない。

### chefからansibleに乗り換えた話 by @futoase
やはりchefより良いのは以下2点
- 対象に何も入れなくて良い
- 上から順に実行されるから分かりやすい → 「孤独なchef使い]になりにくい


## 所感
- 想像しているより使われてる
- 2000台相手にしても大丈夫というのは安心感ある
- やはり採用にあたり大きいのは以下2点
    - 対象ホストに何も入れなく良い
    - 「上から実行」という分かり易さ
- 個人的にはchef等と違ってpush型が基本なので、全サーバに設定ファイル配布…とか小さいところか試しやすいってのも大きい気がする。
