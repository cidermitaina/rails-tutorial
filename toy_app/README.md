# rails チュートリアル 2   8/9(fri)

## アプリケーションの計画

`rails _5.1.4_ new toy_app`

`cd toy_app/`


Toyアプリケーション用のGemfile
```
source 'https://rubygems.org'

gem 'rails',        '5.1.4'
gem 'puma',         '3.9.1'
gem 'sass-rails',   '5.0.6'
gem 'uglifier',     '3.2.0'
gem 'coffee-rails', '4.2.2'
gem 'jquery-rails', '4.3.1'
gem 'turbolinks',   '5.0.1'
gem 'jbuilder',     '2.7.0'

group :development, :test do
  gem 'sqlite3', '1.3.13'
  gem 'byebug',  '9.0.6', platform: :mri
end

group :development do
  gem 'web-console',           '3.5.1'
  gem 'listen',                '3.1.5'
  gem 'spring',                '2.0.2'
  gem 'spring-watcher-listen', '2.0.1'
end

group :production do
  gem 'pg', '0.20.0'
end

# Windows環境ではtzinfo-dataというgemを含める必要があります
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]
```

--without productionオプションを追加することで、本番用のgemを除いたローカルgemをインストール
` bundle install --without production`



### ユーザーのモデル設計


ユーザーのデータモデル
|users||
|:--:|:--:|
|id|integer型|
|name|string型|
|email|string型|

integer型
変数（プログラミングにおけるデータを入れておく箱）の種類のひとつ

### マイクロポストのモデル設計

マイクロポストのデータモデル
|マイクロポスト||
|:--:|:--:|
|content|text型|
|user_id|integer型|

## Usersリソース

ユーザー用のデータモデルを、そのモデルを表示するためのWebインターフェイスに従って実装する

Railsプロジェクトに標準装備されているscaffoldジェネレータで生成

scaffold
基本的な機能を備えたWebアプリをすぐに作成できる

`rails generate scaffold モデル名 カラム名1:データ型1 カラム名2:データ型 2 …`

以下のコマンドを実行

`rails generate scaffold User name:string email:string`

`rails db:migrate`

でデータベースをマイグレート