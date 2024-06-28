## コンテナにするフォルダを作成
### 構成
```
プロジェクトフォルダ
  ∟ db_entypoint (*)
    - sql.sql
  ∟ wp-admin
  ∟ wp-content
  ∟ wp-includes
  - Dockerfile(**)
  - docker-compose.yml(***)
  - index.php
  - wp-***.php
  ...
```

## (*)db_entrypointフォルダの中にインポートしたいSQLを入れる

## (**)Dockerfileの作成
[概要](./docker-file.md)

## (***)docker-compose.ymlを作成
[概要](./docker-compose.yml)
## 初回起動時のコマンド
```
1. 「コマンドプロンプト」からフォルダへ移動し次のコマンドを実行
2. docker compose exec wordpress(コンテナ名) bash
3. bashで仮想環境に入り、root****/var/www/html#と表示される
4. llコマンドで一覧表示
5. -rw-r--r--  1 www-data www-data   405 Feb  6  2020 index.phpなど表示されるので、www-dataではないファイルまたはフォルダがないか確認
6. もしwww-dataではないものがあれば現在の権限で操作できないので、次のコマンドで権限を変更する
7. chown -R www-data:www-data フォルダ名１ フォルダ名２
8.これで権限の変更が完了し、大体表示がうまくいく
```
### コマンドの解説 5 llで表示された文字列の意味
```
5. -rw-r--r--  1 www-data www-data   405 Feb  6  2020 index.php

-rw-r--r--(権限)
・一番左がdの場合ディレクトリ、-の場合ファイルであることを明示
・rwxrwxrwx r読み、w書き、x実行の権限があれば表示、なければ-。所有者の権限、所有グループの権限、その他ユーザーの権限の順。

1(ハードリンク数)
・通常は１

www-data(ファイルの所有者)
・www-dataは、Debian系のLinuxディストリビューション(UbuntuやDebian等)におけるApacheウェブサーバーのデフォルトユーザー。Apacheウェブサーバーは、セキュリティ上の理由から、rootユーザーではなく、制限された権限を持つ専用のユーザーとグループで実行され、その専用ユーザーがwww-data。

www-data(ファイルの所有グループ)
・www-dataは上記と同じく、デフォルトのグループ。グループとは、Unix系のユーザー管理の概念のひとつで、複数のユーザーをまとめる際に使用される。

405(ファイルサイズのバイト)

Feb  6  2020(最終更新日)

index.php(ファイル名)
```
### コマンドの解説 7 権限の変更コマンド
```
chown
・チェンジオーナー

-R
・recursive。最後に指定するフォルダの中身全てを含む。このコマンドがないと外側のフォルダだけの権限変更となってしまう。

www-data:www-data
・新しい所有者:新しいグループ
```

## 注意点
・docker-compose.ymlではローカルをマウントしない方が良い。（なんかうまくいかない）<br>
・うまくいかなかったり、開発が終われば、コンテナは割と頻繁に消すものと思ってよい。手軽に再構築できるのがDockerの良さ。<br>
・消すときは、コンテナの他に「images」「volumes」からも関連の名前のものを消す <br>
・初回起動時に、キャッシュのない「シークレットモードブラウザ」で開く。
・.htaccessの名前を変えて無効化する
・wp-optionsテーブル（管理画面からも変更可）のサイトアドレスを変える

<!-- ## phpmyadminに接続し設定を変更
・wp-optionsのsiteurl,homeをhttp://localhost:3000/に変更 <br>
・wordpressのユーザー名、パスワードを入手できない場合は、wp-usersのユーザー名とパスワード（ハッシュ化必要）を追加 <br>
・wp-config.phpのデータベース名・ユーザー名・パスワードの設定を確認 -->


## イメージとは
```
Dockerの文脈で言うと「コンテナイメージ」と言われ、コンテナを起動するために必要な一式が格納されているファイルのこと。通常はGitHub上にある公式データを参照する。独自に設定したい場合はdockerfileでイメージをつくる。
・OSの基盤となるファイル（多くの場合Linux distribusion）
・アプリケーションコード
・実行に必要なライブラリ（言語）
・実行設定ファイル（環境変数、ポート設定、ボリューム設定など）
・メタデータ（Dockerfileの命令履歴など）
```

## dockerfileとは
```
イメージを作成するための設計図。
```

## docker composeとは
```
Docker Compose とは、複数コンテナのアプリケーションを定義・共有するために役立つように、開発されたツールです。Compose があれば、サービスを定義する YAML ファイルを作成し、コマンドを１つ実行するだけで、瞬時にすべて立ち上げたり、すべてを削除したりできます。
```

引用
https://qiita.com/yuya_sega/items/0fb78b064a6d64fe0979