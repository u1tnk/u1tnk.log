---
title: "WSL2で rails test:system を動かす為の設定"
date: 2021-06-28T11:17:59+09:00
draft: false
categories: ["wsl2", "docker"]
---
## 2023/10/15 修正

**状況だいぶ変わっていてubuntuも22.04で問題無いし、chromedriverも入れてくれるgemがあるのでchromeのインストール以外ほぼ不要となっている。**
## 前提
Railsはインストールして systemテスト以外は動いている前提。

## イメージ
ubuntu22.04

~~ubuntu18.04。 20.04だと 2021/1/4当時動かないspecが存在したので18.04を利用。~~

## ライブラリ
**22.04でデフォルトで大体入ってるので不要そう。**


```
sudo apt update
 sudo apt install curl build-essential unzip wget
```

## chromeのインストール

```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install ./google-chrome-stable_current_amd64.deb -y
```

## chromedriverのインストール
**selenium-webdriver gemが入っていれば不要。** 参考: [RSpecを実行するとWebdrivers::VersionErrorが発生する場合の対処方法 - Qiita](https://qiita.com/jnchito/items/f994dd3ac2cdc39bff8c)

```
export LATEST_VERSION=`curl https://chromedriver.storage.googleapis.com/LATEST_RELEASE`
wget https://chromedriver.storage.googleapis.com/$LATEST_VERSION/chromedriver_linux64.zip
unzip chromedriver_linux64.zip
sudo mv chromedriver /usr/local/bin/
```

## systemテストの設定
**現状不要。**

[System specを動かすのにはまった - Qiita](https://qiita.com/jhanyu/items/e2f467684873d806ad00) から引用。
headlessで動作するよう設定する。
 画面を表示したい場合はwindows側に X serverインストール、転送の設定などが必要になる。

rails_helper.rb (rails関係のtestをするファイルからrequireしておく)
```
Capybara.default_driver = :selenium_chrome_headless
Capybara.register_driver :selenium_chrome_headless do |app|
  options = Selenium::WebDriver::Chrome::Options.new
  options.add_argument('--headless')
  options.add_argument('--no-sandbox')
  options.add_argument('--disable-gpu')
  options.add_argument('--window-size=1280,1024')

  Capybara::Selenium::Driver.new(app, browser: :chrome, options: options)
end
```

spec_helper.rb
```
  config.before(:each, type: :system) do
    driven_by :selenium_chrome_headless
  end
```
