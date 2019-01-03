+++
title = "JAWS DAYS 2014に行ってきました"
date = "2014-03-15"
categories = ["aws"]
+++


## Immutable Infrastructure
JAWS DAYS 2014、Immutable Infrastructure について - naoyaのはてなダイアリー http://d.hatena.ne.jp/naoya/20140315/1394851727

めっちゃ人居ました。個人的にrebuild.fm全部聞いてるのでほとんど聞いてたので割愛w

## クックパッドのデプロイとオートスケール
個人的一番勉強になったセッション。

Cookpad's deployment and auto scaling // Speaker Deck https://speakerdeck.com/mirakui/cookpads-deployment-and-auto-scaling

- nameタグ、hostnameは起動時に自動採取
	- →これはすぐにでも真似したい。
- roleタグというのをつけてデプロイ内容をコントロール
	- →ふむふむ
- Nameタグからip収集してpowerdnsで管理。
	- →そうしたいよね…
- ec2からタグ情報取得するApiのImitに到達するらしいw キャッシュしてるとのこと

- Amazon AutoScaling使って無くて自作！
    - CloudWatchが最短一分で秒単位で取得したい
	    - そこまで必要なのか…
    - graceful terminate問題
	    - それは確かに。多少のリクエストなら負荷で落ちるよりマシと思ってやってるが…
    - ELBでは無くhaproxy使いたい。…
	    - haproxyの方が良いそうな。
	    - ブラックボックスで問題起こったらよくわからんってのは確かに。
    -  デプロイとにAutoScaalingと排他制御するため
	  - 動くとやばいので、排他制御してると。ほー。デプロイ多いからこその問題。

- 問題調査のための、staticインスタンスとしてAutoScaleとは別管理のインスタンスがある。
	- 確かに。システムログとか見るヒマ無く勝手に落ちるからな…Amazon Autoscalingでもできるからやりたい


- 蛇足
	- chefじゃなくてpuppet使ってるらしい。なんでだろ。
	- cookpadもhipchatoとのこと

## プロビジョニング＆デプロイ on AWSのキホン

The Basics of Provisioning and Deploy on AWS #jawdays #infra http://www.slideshare.net/Ryuzee/the-basics-of-provisioning-and-deploy-on-aws-jawdays-infra
特段目新しい内容は無かったが、とにかく耳の痛い正論セッションでした。

- 自動化しないリスク、コストを過少評価し過ぎ
- 自動化するなら最初から。デプロイするときに考えるじゃ遅い
- 自動化しない→ミスる→チェック増やす→コスト増える→＼(^o^)／
- デプロイアンチパターン、手作業混合。
	- →これはやっちゃう…
- テスト自動化！小さい粒度でリリース！
- sunsu+graphite良いと。


## カジュアルにVPC作った結果がこれだよ！

http://www.slideshare.net/Yuryu/aws-casual-talks-1-vpc
行ってないけど、資料流れてきたらめっちゃ気になってた事なので勉強になった。
カジュアルには行けない事が良くわかったw

## 20140315 JAWS DAYS 2014 ACEに聞け！ CloudFormation編

http://www.slideshare.net/daisuke_m/20140315-jaws-days-2014acecfn

2番目に勉強になった。

- やっぱ基本は一発作るのがメインのものなんだ
- とはいえ、UPDATE_STACKでメンテしていける。
- UPDATE_STACKちゃんとせず、手でメンテすると詰むと。ウチのは完全詰んでるからどっかで精算せねばw
- BLUE-GREEN deploymentを CludFormationだけで実行するデモ。ここまでできると素晴しいなぁ…
- とはいえjsonの定義見るのキツいわw AWS CloudFormation tempate for PHP Blue-Green Deployment environment https://gist.github.com/miyamoto-daisuke/9544801

# Immutable Infrastracture パネルディスカッション〜オレにも一言言わせろ〜
人大杉…立ち見でも厳しそうだったので断念。

## まとめ

- すぐに手をつけたいのはchef/powerdns/cloudformation/tag利用…あたりかなぁ。山積みすぎる…
- 人の多さと会場の割り当てに疑問を感じる。立ち見2回で足痛い…
- AWS面白い。改善頑張ろう。

