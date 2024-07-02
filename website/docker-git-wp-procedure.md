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
[概要、こちらをコピー](./docker-file.md)

## (***)docker-compose.ymlを作成
[こちらをコピー](./docker-compose.yml)

## (****).gitignoreを作成
[こちらをコピー](../git/.gitignore)
35行目のテーマ名を直す

## dockerでWordPressを立ち上げる
1. コマンドプロンプトからプロジェクトフォルダへ移動（docker-compose.ymlファイルがあるところ）
1. Docker立ち上げコマンドを入力
```
docker compose up -d
```
3. 起動したらVSCodeを立ち上げ、リモートエクスプローラーからwordpressを選択
1. http://localhost:3000にアクセスし、立ち上がりを確認
1. WordPressのリンクがうまくいってない場合、WPにログインし、「パーマリンク」を変更せず設定だけ押すと直る

## 編集が完了したら、GitHubにpush
1. VSCodeのターミナルを開く
1. gitの初期化コマンドを入力
```
git init
```
3. GitHubにリポジトリを作成<br>
注意：privateになっていることを確認する
3. 接続する
```
git remote add origin 接続コード(GitHubのリポジトリからコピー)
```
5. SSHキーを生成
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
6. キーファイルの保存場所、jパスフレーズの設定、パスフレーズの確認を求められる。デフォルトのままでよい場合はエンターキーを押す（３回）
6. 以下のコマンドを入力し、公開鍵を表示
```
cat /root/.ssh/id_ed25519.pub
```
8. GitHubのアカウント全体のSettings画面に移動し、「SSH and GPG keys」を選択
8. 「New SSH key」から任意のタイトルと、さきほどの公開鍵をコピーし登録
8. pushする
8. gitの詳しい操作方法については[こちらを参照](../git/git_command.md)

## GitHub上に置いてはいけないデータについて
1. データベースの中身(SQLファイル)<br>
1. wp-config.php<br>
※GitHubに機密情報は載せてはいけないため。<br>
※上記の.gitignore内ではアップロードしないように指定済

## GitHubを確認
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
・WordPressログイン情報等変更があれば報告<br>
・ローカルにもバックアップをとっておく<br>


## うまくいかない時
・docker-compose.ymlではローカルをマウントしない方が良い。（なんかうまくいかない）<br>
・うまくいかなかったり、開発が終われば、コンテナは割と頻繁に消すものと思ってよい。手軽に再構築できるのがDockerの良さ。<br>
・キャッシュのない「シークレットモードブラウザ」で開いてみる<br>
・.htaccessの名前を変えて無効化する<br>
