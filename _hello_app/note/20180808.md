# rails チュートリアル 1   8/8(Wed)


## Ruby on Railsとは

Rubyプログラミング言語で記述された、Web開発フレームワーク

### なぜ多くの人に使われるの？

- 制約の少ないMITライセンスで公開
- 設計が簡潔で美しい
- 最新のWebテクノロジーやフレームワーク設計に素早く適応
- 「REST」という設計思想の重要性をいち早く理解し、対応したフレームワークの１つ

REST?

## Cloud9で開発環境を作成

[クラウドならアマゾン ウェブ サービス 【AWS 公式】](https://aws.amazon.com/jp/)

### Railsをインストールする

railsのインストール
`gem install rails -v 5.1.4`

Railsのインストールにはgemコマンドを使います。


environmentに移動
`cd ~/environment`

## 最初のアプリケーション

rails newを実行する（ファイルとディレクトリが作成される）

`rails _5.1.4_ new hello_app`


### Bundler

Bundlerを実行して、アプリケーションに必要なgemをインストール
Bundlerはrailsによって自動的に実行 (この場合はbundle install) される。

> Bundler
gem同士の互換性を保ちながらパッケージの種類やバージョンを管理してくれる仕組みのことです。
複数人、複数環境で開発を行う際に各環境で扱うパッケージの種類やバージョンを合わせてくれるので非常に便利

Gemfileを以下に変更
```ruby
source 'https://rubygems.org'

gem 'rails',        '5.1.4'
gem 'puma',         '3.9.1'
gem 'sass-rails',   '5.0.6'
gem 'uglifier',     '3.2.0'
gem 'coffee-rails', '4.2.2'
gem 'jquery-rails', '4.3.1'
gem 'turbolinks',   '5.0.1'
gem 'jbuilder',     '2.6.4'

group :development, :test do
  gem 'sqlite3',      '1.3.13'
  gem 'byebug', '9.0.6', platform: :mri
end

group :development do
  gem 'web-console',           '3.5.1'
  gem 'listen',                '3.1.5'
  gem 'spring',                '2.0.2'
  gem 'spring-watcher-listen', '2.0.1'
end

# Windows環境ではtzinfo-dataというgemを含める必要があります
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]
```

`bundle install`を実行してgemをインストール

```
Running `bundle update` will rebuild your snapshot from scratch, using only
the gems in your Gemfile, which may resolve the conflict.
```
っていうエラーが出たので、

`bundle update`

実行後、

`bundle install`


### rails server

Railsには開発マシンでのみブラウズできるローカルWebサーバーを起動するためのコマンドラインプログラムがある。

`rails server`


### MVC

app/ディレクトリの中に
「models」「views」「controllers」という3つのサブディレクトリがある

Webサーバーにリクエスト (request) を送信
　　 　　　　　　↓
Railsのコントローラ (controller) に渡される
（リクエストを処理する役割を担っている）
　 　　　　　　　↓
ビュー (view) を生成してHTMLをブラウザに送り返したり、データベースとの通信を担当しているRubyのオブジェクトであるモデル (model) と対話したり
　 　　　　　　　↓
モデルを呼び出した後、コントローラは、ビューを描画し、完成したWebページをHTMLとしてブラウザに返す。

### Hello, world!

#### コントローラーにアクションを定義

`app/controllers/application_controller.rb`に以下を追加

表示したい文字列を返すアクションを定義

```ruby
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  def hello
    render html: "hello, world!"
  end
end
```

#### ルーティングの設定

ルーティングの文法

`root 'controller_name#action_name'`

`config/routes.rb`にルーティングを設定

```ruby
Rails.application.routes.draw do
  root 'application#hello'
end
```


ルートルーティングから「hello, world!」が返されるようになる。