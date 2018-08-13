# rails チュートリアル 2   8/9(fri)~8/13(mon)

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

### MVCの挙動


ユーザーからリクエストされたURLを、Usersリソースで使うコントローラのアクションに割り当てるためのコード
↓
config/routes.rb

```ruby
Rails.application.routes.draw do
  resources :users
  root 'users#index'
end
```

`root 'users#index'`
コントローラー名#アクション名


Railsではconfig/routes.rbで、URLとアクションの組み合わせ (表 2.1) を効率よく設定していく。

:usersという一見奇妙な記法は、Ruby言語特有の「シンボル」と呼ばれるもの


#### REpresentational State Transfer (REST)

RESTは、インターネットそのものやWebアプリケーションなどの、分散・ネットワーク化されたシステムやアプリケーションを構築するためのアーキテクチャのスタイルの1つ。

RailsアプリケーションにおけるRESTとは、アプリケーションを構成するコンポーネント (ユーザーやマイクロポストなど) を「リソース」としてモデル化することを指します。


#### indexアクションを整理

 Toyアプリケーションの簡潔なユーザーindexアクション
 
 1. indexアクションに@users = User.allという行があります 。これによって、Userモデルからすべてのユーザーの一覧を取り出し、@usersという変数に保存
 
 app/controllers/users_controller.rb
```ruby
class UsersController < ApplicationController
  before_action :set_user, only: [:show, :edit, :update, :destroy]

  # GET /users
  # GET /users.json
  def index
    @users = User.all #Userモデルからすべてのユーザーの一覧を取り出し@usersという変数に保存
  end
  .
  .
  .
end

```


 ToyアプリケーションのUserモデル
 app/models/user.rb
```ruby
class User < ApplicationRecord
end
```
2. @users変数にユーザー一覧が保存されると、コントローラはビュー を呼び出す

*記号で始まる変数をRubyではインスタンス変数と呼び、Railsのコントローラ内で宣言したインスタンス変数はビューでも使えるようになる*

 indexアクションに対応しているビュー
 app/views/users/index.html.erb
```ruby
<h1>Listing users</h1>

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th colspan="3"></th>
    </tr>
  </thead>

<% @users.each do |user| %>
  <tr>
    <td><%= user.name %></td>
    <td><%= user.email %></td>
    <td><%= link_to 'Show', user %></td>
    <td><%= link_to 'Edit', edit_user_path(user) %></td>
    <td><%= link_to 'Destroy', user, method: :delete,
                                     data: { confirm: 'Are you sure?' } %></td>
  </tr>
<% end %>
```

ビューはその内容をHTMLに変換、コントローラがブラウザにHTMLを送信して、ブラウザでHTMLが表示される。


#### before_action

アクションの前に処理を差し込むことができます。一般的に、複数のアクションで共通して必要になる処理などをbefore_actionで定義することが多い。
[rails "before_action"で記述を簡素化しよう！ - bryankawaの日記](http://bryankawa.hatenablog.com/entry/2016/12/25/105210)

#### findメソッド

modelの検索機能の1つ
findは「id」によってデータを取得する。

#### params
Railsでは、リクエスト情報をひとまとめにして、params[:パラメータ名]という形式で取得できる。


#### 演習
1.図 2.11を参考にしながら、/users/1/edit というURLにアクセスしたときの振る舞いについて図を書いてみてください。

a. 

2.図示した振る舞いを見ながら、Scaffoldで生成されたコードの中でデータベースからユーザー情報を取得しているコードを探してみてください。

a. app/controllers/users_controller.rb
66~68行目付近
```ruby
def set_user
    @user = User.find(params[:id])
end
```

3.ユーザーの情報を編集するページのファイル名は何でしょうか?

a. app/views/users/edit.html.erb

### Usersリソースの欠点

scaffoldで作成したUsersリソースは、Railsの概要を手っ取り早く説明するには良いのですが、次のようなさまざまな問題点を抱えている

## Micropostsリソース

### マイクロポストを探検する

rails generate scaffoldコマンドを使って、図 2.3のデータモデルを実装

`rails generate scaffold Micropost content:text user_id:integer`

`rails db:migrate`

config/routes.rbにresources :micropostsが追加される
```ruby
Rails.application.routes.draw do
  resources :microposts
  resources :users
  root 'users#index'
end
```

#### 演習

1.CSSを知っている読者へ: 新しいマイクロポストを作成し、ブラウザのHTMLインスペクター機能を使って「Micropost was successfully created.」の箇所を調べてみてください。ブラウザをリロードすると、その箇所はどうなるでしょうか?

a. 消える

2.マイクロポストの作成画面で、ContentもUserも空にして作成しようとするどうなるでしょうか?

a.空の状態で作成される

3.マイクロポストの作成画面で、ContentもUserも空にして作成しようとするどうなるでしょうか?

a. 作成される

4.上記の演習で作成したマイクロポストを削除してみましょう。

a. 削除される

### マイクロポストをマイクロにする

文字数制限を与える
バリデーション (validation) を使って簡単に制限を加えることができる。

`validates :content, length: { maximum: 140 }`
を追加

app/models/micropost.rb
```ruby
class Micropost < ApplicationRecord
  validates :content, length: { maximum: 140 }
end
```

#### 演習

1.先ほど2.3.1.1の演習でやったように、もう一度Contentに141文字以上を入力してみましょう。どのように振る舞いが変わったでしょうか?

a.エラーメッセージが表示される

2.CSSを知っている読者へ: ブラウザのHTMLインスペクター機能を使って、表示されたエラーメッセージを調べてみてください。

id="error_explanation"とclass="field_with_errors"が追加される

#### 異なるデータモデル同士の関連付け

app/models/user.rb

1人のユーザーに複数のマイクロポストがある。
```ruby
class User < ApplicationRecord
  has_many :microposts
end
```

1つのマイクロポストは1人のユーザーにのみ属する。

app/models/micropost.rb

```ruby
class Micropost < ApplicationRecord
  belongs_to :user
  validates :content, length: { maximum: 140 }
end
```

has_many

belongs_to


User.firstと打ち込んでデータベースから1人目のユーザー情報を取り出し、first_user変数に保存

first_user.micropostsというコードを実行すると、そのユーザーに関連付けられているマイクロポストにアクセスできる


#### 演習

1.ユーザーのshowページを編集し、ユーザーの最初のマイクロポストを表示してみましょう。同ファイル内の他のコードから文法を推測してみてください (コラム 1.1で紹介した技術の出番です)。うまく表示できたかどうか、/users/1 にアクセスして確認してみましょう。

a.<%= @user.microposts.first.content %>

2.マイクロポストのContentが存在しているかどうかを検証するバリデーションです。マイクロポストが空でないことを検証できているかどうか、実際に試してみましょう

a.
```ruby
class Micropost < ApplicationRecord
  belongs_to :user
  validates :content, length: { maximum: 140 },
                      presence: true
end
```

3.FILL_INとなっている箇所を書き換えて、Userモデルのnameとemailが存在していることを検証してみてください
a.
```ruby
class User < ApplicationRecord
  has_many :microposts
  validates :name, presence: true
  validates :email, presence: true
end
```