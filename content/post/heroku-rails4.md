+++
title = "Ruby2.0.0+Rails4.0+MySQLなrailsアプリをherokuにデプロイ(2013/7月版)"
date = "2013-07-28"
published = false
categories = ["rails", "heroku"]
+++

久しぶりにheroku触ってみるとassetでハマることも無く、ruby2.0.0使うのも一行。
すごい簡単になってるけど、結構変わってるしメモ。

MySQL使ってるのはこだわりも無いけど、cleardb簡単そうだし、デメリットは初期設定ぐらいっぽいので慣れてる方にしただけ。
heroku標準postgresの制限に行数があるけど、こちらは容量だけってのも微妙に気になった。

## heroku基本設定
https://toolbelt.heroku.com/ からダウンロード

gem i herokuは非推奨らしい

基本以下参照

https://devcenter.heroku.com/articles/rails3

## Gemfile

``` sh
# herokuに2.0.0使用を指示
ruby '2.0.0'

# ローカルでもsqlite3よりmysqlの方が好き、cleardb使うので全てmysql2
gem 'mysql2'
```

## herokuアプリ作成
```
heroku login
heroku create hoge
git push heroku master
```

## cleardb(MySQLプラグイン) 設定

以下のドキュメントに従う
https://devcenter.heroku.com/articles/cleardb

- addon追加、無課金範囲でもherokuにクレジットカード登録しないとエラーになるので注意
```
heroku addons:add cleardb:ignite
```

- herokuから作成されたcleardbのurlを取得
```
heroku config | grep CLEARDB_DATABASE_URL
```

- 上記で調べたURLをmysql://→mysql2://として設定[^1]
[^1]:上記コマンドでもドキュメントにもmysql://...と書いてあるが、mysql2://...にしないとエラーになった。
```
heroku config:set DATABASE_URL='mysql2://hoge:fuga@us-cdbr-east.cleardb.com/heroku_db?reconnect=true'
heroku run rake db:migrate
```

## 動作確認

```
heroku open
```

