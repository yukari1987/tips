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
[PHPだけのプロジェクトの場合はこちらをコピー](../docker/php-Dockerfile)<br>
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
1. http://localhost:3000　にアクセスし、立ち上がりを確認
1. WordPressのページ内リンクがうまくいってない場合、WPにログインし、「パーマリンク」を変更せず設定だけ押すと直る

## 開発終了前の確認事項
[こちらを参照](../wordpress/final.md)

## 開発が完了したら、サーバーにGitでpush


## NASにGitでpushし、バックアップを保存

## 開発が終わったら
・開発が終わってコンテナを消すときは、コンテナの他に「images」「volumes」からも関連の名前のものを消す <br>
・WordPressログイン情報等変更があればパスワード管理者に報告<br>
・ローカルにもバックアップをとっておく<br>
・SSH接続をOFFにし、公開鍵、秘密鍵を破棄

## うまくいかない時
・docker-compose.ymlではローカルをマウントしない方が良い。（なんかうまくいかない）<br>
・うまくいかなかったり、開発が終われば、コンテナは割と頻繁に消すものと思ってよい。手軽に再構築できるのがDockerの良さ。<br>
・キャッシュのない「シークレットモードブラウザ」で開いてみる<br>
・.htaccessの名前を変えて無効化する<br>
