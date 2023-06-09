# 第5回課題
web３層構造を用いてアプリをデプロイ＋S3を使ってみる

## ・初めにVPC,EC2,RDSをAWS上に作成する
＊EC2のパブリック IP の自動割り当て有効化をしないと紐づけてくれないので注意

## ・EC2にSSHでログイン

## サンプルアプリの動作環境
**ruby**
3.1.2<br>
**Bundler**
2.3.14<br>
**Rails**
7.0.4<br>
**Node**
v17.9.1<br>
**yarn**
1.22.19
<br>
## ・パッケージのインストール
`＄sudo yum update -y`
 (-yはyes)<br>
パッケージ類をインストール
<br>
## ・gitをインストール
`$sudo yum install git　-y`<br>
git cloneをするためにgitをインストール
<br>
## ・rubyのインストール
`$git clone https://github.com/sstephenson/rbenv.git ~/.rbenv`<br>
`$git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build`<br>
・パスの設定<br>
`$echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile`<br>
`$echo 'eval "$(rbenv init -)"' >> ~/.bash_profile`<br>
`$source ~/.bash_profile`<br>
rubyバージョン管理のためにrbenvをインストール
<br>

## ・Node.jsインストール
`$curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -`<br>
`$sudo yum -y install nodejs`<br>
javascriptを使うためにインストール
<br>

## ・yarnインストール
`$curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo`<br>
`$sudo yum -y install yarn`
<br>
## ・サンプルアプリインストール
`$git clone gitのｈｔｔｐURL`
<br>
## ・データベースの接続
mysql install<br>
`$curl -fsSL https://raw.githubusercontent.com/MasatoshiMizumoto/raisetech_documents/main/aws/scripts/mysql_initialize.sh | sh`<br>
エラーMYSQLにつながらない<br>
→セキュリティグループ設定が設定されていなかった
<br>
・mysqlにログイン<br>
`$mysql -h RDSendpoint -u root -p`<br>
RDS作成時に設定したものを使う<br>
config databes yumlを編集<br>
データーベースのソケットの場所確認方法<br>
mysql --help | grep my.cnf
<br>
## ・app環境構築
`bin/setup`<br>
エラー　n error occurred while installing sassc (2.4.0), and Bundler cannot continue.<br>
→gcc-c++がなかったのでインストール<br>
`$yum install -y gcc-c++`<br>
エラーインストールがとまっている<br>
→インストールが進まなかったのでインスタンスタイプを変更<br>
エラー　railsがインストールされていない<br>
`$gem install rails`<br>
## ・railsの起動<br>
`$rails s -b 0.0.0.0`<br>
エラー　コンパイルができない<br>
→手動でプリコンパイル<br>
`$rails assets:precompile` 
<br>
![0501](images/0501.png)

## ・ALBの配置
エラー　config.hosts << "raisetech-977894920.ap-northeast-1.elb.amazonaws.com"<br>
→接続をすべて拒否していたのでconfig/environments/development.rbを編集
<br>
![0502](images/0502.png)
<br>
![0503](images/0503.png)
<br>
## ・nginxインストール
`$sudo amazon-linux-extras install nginx1`
<br>
## ・Nginx を起動
`$sudo systemctl start nginx`
<br>
![0504](images/0504.png)
<br>
## ・Unicorn をインストール
Rails アプリケーションがあるディレクトリの config ディレクトリに unicorn.rb というファイルを新規作成
<br>
Unicorn の起動・停止スクリプトを作成する
`$rails g task unicon`<br>
lib/tasks ディレクトリに unicorn.rake というファイルが生成ファイルを開いて編集
<br>
## ・Unicorn を起動
`$rake unicorn:start`<br>
Unicorn が起動しているかどうかを確認<br>
`$ps -ef | grep unicorn | grep -v grep`
<br>
## ・Nginx の設定ファイルを作成
エラー　権限がないためにviが編集できない<br>
→$sudo~とつける<br>
エラー　サーバーネームが長い<br>
→server name をlocalhostに変更
<br>
## nginx 起動
`$sudo systemctl start nginx`<br>
![0505](images/0505.png)<br>
![0509](images/0509.png)
<br>
## nginx エラーの確認できるコマンド
`$sudo systemctl status nginx -l`<br>
`$nginx -t`<br>
エラーログを見る
<br>
## ・s3にALBのaccess　logを保存
ｓ３バケットを作成して権限を持つユーザーを作成<br>
バケットポリシーを作成<br>
ALBの設定からアクセスログの有効化を選択して適応する
<br>
![0506](images/0506.png)
<br>
![0508](images/0508.png)
今回はwebサーバーをstopして502のエラーが返ってきていました。<br>
304は変更なしの意味でした。
<br>
## ・今回作成した構成図
![0507](images/0507.png) 
<br>
# 感想
今回の課題を通して感じた点として、エラーが出た際に１つの考え方ややり方に固執するのではなく様々な見方や方法た試してやってみることが大切だと感じました。
その際に構成図等のツールをうまく活用できるよう日頃から資格試験等の学習において構成図を書いて考える癖付けをしていきたいです。
またサーバーとの接続の際の使うポート番号や通信の仕組みの理解を深めていくべきだと感じました。










