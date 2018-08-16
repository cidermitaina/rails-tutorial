# rails チュートリアル 5  8/16(Thu)

## ほぼ静的なページの作成

### セットアップ

1. sample_appの作成

`rails _5.1.4_ new sample_app`

2. Gemfileの変更

変更後bundle install
`bundle install --without production`

### 静的ページ

まずはRailsのアクションやビューを使って静的なHTMLのみのページを作成。

app/controllersディレクトリやapp/viewsディレクトリ内で作業を進める。

ブランチを切って作業

`git checkout -b static-pages`

1. generate スクリプトで、コントローラを生成

コントローラ名を「Static Pages」

Homeページ、Helpページ、Aboutページに使うアクションもそれぞれ作成

`rails generate controller <コントローラー名> <アクション名>`

`rails generate controller StaticPages home help`

Gitリポジトリに追加
```
git add -A
git commit -m "Add a Static Pages controller"
git push -u origin static-pages
```

#### 元に戻す方法

rails destroyというコマンドを実行

```
rails generate controller StaticPages home help
rails destroy  controller StaticPages home help
```

#### routesファイル

config/routes.rb
```
Rails.application.routes.draw do
  get 'static_pages/home'
  get 'static_pages/help'
end
```
`get 'static_pages/home'`

static_pages/homeというURLに対するリクエストを、StaticPagesコントローラのhomeアクションと結びつけている。

app/controllers/static_pages_controller.rb
```ruby
class StaticPagesController < ApplicationController #StaticPagesControllerというクラスを定義
  def home
  end

  def help
  end
end
```
↑
StaticPagesControllerというクラスを定義。このようなクラスは、メソッド (関数とも呼なれる) をまとめるときに便利な手法。

defというキーワードを使って、homeアクションやhelpアクションを定義

```ruby
def home
end

def help
end
```

純粋なRuby言語であれば、これらのメソッドは何も実行しない。
ApplicationControllerクラスを継承しているため、StaticPagesControllerのメソッドは (たとえ何も書かれていなくても) Rails特有の振る舞いをする。

/static_pages/homeというURLにアクセスすると、RailsはStaticPagesコントローラを参照し、homeアクションに記述されているコードを実行します。その後、そのアクションに対応するビュー を出力します。homeアクションが空になっているので、/static_pages/homeにアクセスしても、単に対応するビューが出力される

##### 演習

1.Fooというコントローラを生成し、その中にbarとbazアクションを追加してみてください。

a. rails generate controller Foo bar baz

2.コラム 3.1で紹介したテクニックを駆使して、Fooコントローラとそれに関連するアクションを削除してみてください。

a. rails d controller Foo bar baz

### 静的なページの調整

app/views/static_pages/home.html.erb,
app/views/static_pages/help.html.erbを調整

### テスト

何らかの変更を行う際には、常に「自動化テスト」を作成して、機能が正しく実装されたことを確認する習慣をぜひ身に付ける

#### 最初のテスト

テストを先に書く

rails generate controllerを実行した時点でテストファイルがちゃんと作成されている。

test/controllers/static_pages_controller_test.rb

```ruby
require 'test_helper'

class StaticPagesControllerTest < ActionDispatch::IntegrationTest
  test "should get home" do
    get static_pages_home_url
    assert_response :success
  end

  test "should get help" do
    get static_pages_help_url
    assert_response :success
  end

end
```

詳しく↓

```ruby
test "should get home" do
  get static_pages_home_url
  assert_response :success
end
```
Homeページのテスト。GETリクエストをhomeアクションに対して発行 (=送信) せよ。そうすれば、リクエストに対するレスポンスは[成功]になるはず。


現在のテストスイートをそのまま実行して、問題なくパスすることを確認しておく

`rails test`

結果が
```2 runs, 2 assertions, 0 failures, 0 errors, 0 skips```
になる

> テストスイート
ソフトウェアテストの目的や対象ごとに複数のテストケースをまとめたもの。自動化テストにおいては、テストの実行単位となる。

もし以下のようなエラーになったら
```
Running via Spring preloader in process 7716
Run options: --seed 45474

# Running:

Run options: --seed 45474

# Running:

....

Finished in 0.296551s, 6.7442 runs/s, 6.7442 assertions/s.

2 runs, 2 assertions, 0 failures, 0 errors, 0 skips
/usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier/tmux/client.rb:12:in `version': undefined method `[]' for nil:NilClass (NoMethodError)
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier/tmux.rb:69:in `_check_available'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier/base.rb:59:in `initialize'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier/detected.rb:100:in `new'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier/detected.rb:100:in `_add'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier/detected.rb:62:in `block (2 levels) in detect'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier/detected.rb:60:in `each'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier/detected.rb:60:in `detect'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier/detected.rb:60:in `block in detect'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier/detected.rb:59:in `each'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier/detected.rb:59:in `detect'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier.rb:180:in `_detect_or_add_notifiers'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier.rb:198:in `_activate'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier.rb:87:in `initialize'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier.rb:42:in `new'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/notiffany-0.1.1/lib/notiffany/notifier.rb:42:in `connect'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/guard-2.13.0/lib/guard/notifier.rb:11:in `connect'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/guard-2.13.0/lib/guard/notifier.rb:31:in `notify'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/guard-compat-1.2.1/lib/guard/compat/plugin.rb:113:in `notify'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/guard-minitest-2.4.4/lib/guard/minitest/notifier.rb:31:in `notify'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/guard-minitest-2.4.4/lib/guard/minitest/reporter.rb:10:in `report'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/minitest-5.11.3/lib/minitest.rb:808:in `each'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/minitest-5.11.3/lib/minitest.rb:808:in `report'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/minitest-5.11.3/lib/minitest.rb:141:in `run'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/minitest-5.11.3/lib/minitest.rb:63:in `block in autorun'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/spring-2.0.2/lib/spring/application.rb:171:in `fork'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/spring-2.0.2/lib/spring/application.rb:171:in `serve'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/spring-2.0.2/lib/spring/application.rb:141:in `block in run'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/spring-2.0.2/lib/spring/application.rb:135:in `loop'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/spring-2.0.2/lib/spring/application.rb:135:in `run'
        from /usr/local/rvm/gems/ruby-2.4.1/gems/spring-2.0.2/lib/spring/application/boot.rb:19:in `<top (required)>'
        from /usr/local/rvm/rubies/ruby-2.4.1/lib/ruby/site_ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require'
        from /usr/local/rvm/rubies/ruby-2.4.1/lib/ruby/site_ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require'
        from -e:1:in `<main>'
```
下記コマンドを実行

`rails db:migrate`
`sudo yum install -y tmux`


[【NomethodError】Rails Tutorial 3章でエラーが出たので調べてみたらtmuxが原因だった | 大学生Webエンジニアの記録](https://ty-engineer.com/ruby-on-rails/rails-tutorial-3-error-tmux/)
[Ruby on Rails チュートリアル３章のテストのバグとその対応メモ](https://qiita.com/miihon_ani/items/1ab1b5733258374efa33)

##### 失敗するテストを最初に書いてみる

static_pages_controller_test.rbに追加
```ruby
test "should get about" do
    get static_pages_about_url
    assert_response :success
end
```  

```NameError: undefined local variable or method `static_pages_about_url'```

AboutページへのURLが見つからないとあるので追加

```
AbstractController::ActionNotFound:
The action 'about' could not be found for StaticPagesController
```
StaticPagesコントローラにaboutアクションがないとエラー
追加

```
Error:
StaticPagesControllerTest#test_should_get_about:
ActionController::UnknownFormat: StaticPagesController#about is missing a template for this request format and variant.
```

テンプレートがないエラー

touchコマンドで作成

```
touch app/views/static_pages/about.html.erb
```

```
rails test
3 tests, 3 assertions, 0 failures, 0 errors, 0 skips
```

エラーがなくなる


## 少し動的なサイト

「red ・ green ・ REFACTOR」のサイクル
ページタイトルの簡単なテストを書き (red)、3つのページにタイトルを追加し (green)、レイアウトファイルを活用してコードの重複を解決します (REFACTOR)

Homeページ、Helpページ、Aboutページをそれぞれ編集し、最終的にページごとに異なるタイトルを表示

### タイトルをテストする (Red)

assert_selectメソッドでは、特定のHTMLタグが存在するかどうかをテストをする

`assert_select "title", "Home | Ruby on Rails Tutorial Sample App"`


テストを作成すると、テストスイートは redになる