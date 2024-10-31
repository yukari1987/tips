# Docker Git を使ったWordPressの構築手順
## はじめに
### Dockerとは
アプリケーションとその動作環境を一緒にパッケージ化し、どこでも同じように動かせるようにする技術です。これにより、「自分のPCでは動くのに、別の環境では動かない」といった問題を解決します。
### Gitとは
いつだれが変更を加えたか追跡できるバージョン管理システム。
## コンテナにするフォルダを作成
### 構成
```
プロジェクトフォルダ
  ∟ db_entypoint (*)
    - sql.sql
  ∟ wp-admin
  ∟ wp-content
  ∟ wp-includes
  Dockerfile(**)
  docker-compose.yml(***)
  .gitignore(****)
  index.php
  wp-***.php
  ...
```

## (*)db_entrypointフォルダの中にインポートしたいSQLを入れる

## (**)Dockerfileの作成
[概要、WordPressのプロジェクトはこちらをコピー](../docker/docker-file.md)<br>
[PHPのみのプロジェクトはこちらをコピー](../docker/php-Dockerfile)<br>
適宜プロジェクトに合った修正

## (***)docker-compose.ymlを作成
[こちらをコピー](../docker/docker-compose.yml)<br>
適宜プロジェクトに合った修正

## (****).gitignoreを作成
[こちらをコピー](../git/.gitignore)
35行目のテーマ名を直す

## dockerでWordPressを立ち上げる
1. コマンドプロンプトからプロジェクトフォルダへ移動（docker-compose.ymlファイルがあるところ）
1. Docker立ち上げコマンドを入力
```
docker compose up -d
```
3. 起動したらVSCodeを立ち上げ、左のタブにある「リモートエクスプローラー」から立ち上げたwordpressを選択
3. 上のタブから「フォルダを開く」を選択し、var/www/htmlを開く
3. VSCodeのエクスプローラにwpの一覧が表示されたら、「wp-config.php」を開く
3. DB_NAME…docker-compose.ymlの「WORDPRESS_DB_NAME」のデータに変更
3. DB_USER…docker-compose.ymlの「WORDPRESS_DB_USER」のデータに変更
3. DB_PASSWORD…docker-compose.ymlの「WORDPRESS_DB_PASSWORD」のデータに変更
3. DB_HOST…docker-compose.ymlの「WORDPRESS_DB_HOST」のデータに変更
3. ブラウザからMySQL(http://localhost:4000)にアクセスし、DBのwp-optionsのoption_id 1と2のoption_valueを「http://localhost:3000」に変更
1. http://localhost:3000　にアクセスし、立ち上がりを確認
1. WordPressのページ内リンクがうまくいってない場合、WPにログインし、「パーマリンク」を変更せず設定だけ押すと直る

## 開発終了前の確認事項
[こちらを参照](../wordpress/final.md)

## 開発が完了したら、GitHubにpush
1. VSCodeのターミナルを開く
1. gitの初期化コマンドを入力
```
git init
```
3. GitHubの画面でリポジトリを作成<br>
注意：privateになっていることを確認する
3. リポジトリができたら、VSCodeでGitHubに接続する
```
git remote add origin 接続コード(GitHubのリポジトリからコピー)
```
5. VSCodeのコマンドでSSHキーを生成
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
6. キーファイルの保存場所、jパスフレーズの設定、パスフレーズの確認を求められる。デフォルトのままでよい場合はエンターキーを押す（３回）
6. SSHキーが作成されたので、以下のコマンドを入力し、公開鍵を表示
```
cat /root/.ssh/id_ed25519.pub
```
8. GitHubの画面で、アカウント全体のSettings画面に移動し、「SSH and GPG keys」を選択
8. 「New SSH key」から任意のタイトルを入力、さきほどの公開鍵をコピーし登録
8. SSH接続が完了したので、VSCodeに戻り、pushする
8. gitの詳しい操作方法については[こちらを参照](../git/git_command.md)

## GitHub上に置いてはいけないデータについて
1. データベースの中身(SQLファイル)<br>
1. wp-config.php<br>
※GitHubに機密情報は載せてはいけないため。<br>
※上記の.gitignore内ではアップロードしないように指定済

## GitHubでリポジトリを確認
privateになっているか等確認

## サーバー側からGitHubに接続しクローンする
1. RLoginでサーバーに接続
1. クローンしたいフォルダに移動
1. SSHキーを発行
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
4. SSHキーを表示
```
cat /root/.ssh/id_ed25519.pub
```
5. GitHubにSSHキーを登録
6. クローンする
```
git clone git@github.com:ユーザー名/リポジトリ名.git .
```
※GitHub上のすべてのデータがクローンされてしまうため、不要なファイルを消す操作が必要。gitコマンドのチェックアウトという機能でできる（コマンド確認中）<br>
7. データベース、wp-config.phpはリポジトリに含まれていないので別途設置

## 開発が終わったら
・開発が終わってコンテナを消すときは、コンテナの他に「images」「volumes」からも関連の名前のものを消す <br>
・WordPressログイン情報等変更があればパスワード管理者に報告<br>
・ローカルにもバックアップをとっておく<br>
・SSH接続をOFFにし、公開鍵、秘密鍵を破棄

## 目論見
1. プロジェクトの過去の履歴が分かる
1. 1により、作業者が変わった時の引き継ぎストレスを軽減
1. バックアップの作成（GitHubでは不足。別途方法を考える）
1. 上記が達成されなければフローを見直す

## 懸念点と対策
* SSH鍵の管理が難しそう…

## うまくいかない時
・docker-compose.ymlではローカルをマウントしない方が良い。（なんかうまくいかない）<br>
・うまくいかなかったり、開発が終われば、コンテナは割と頻繁に消すものと思ってよい。手軽に再構築できるのがDockerの良さ。<br>
・キャッシュのない「シークレットモードブラウザ」で開いてみる<br>
・.htaccessの名前を変えて無効化する<br>
