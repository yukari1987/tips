## docker-compose.ymlを作成
```
//Compose fileのバージョン
version: '3.7'

services:
  # 利用するイメージを追加
  mysql:
    image: mysql:5.7
    volumes:
      - mysql:/var/lib/mysql
      # - \\wsl.localhost\Ubuntu-22.04\home\ユーザー名\docker\db:/var/lib/mysql
      # 仮想的なvolumeではなく物理的なファイルで情報を残したい場合はコメントアウトを外す wsl内に領域を指定しないと動作が激重になる
      - ./db_entrypoint:/docker-entrypoint-initdb.d
    environment:
      MYSQL_ROOT_PASSWORD: パスワード
      MYSQL_DATABASE: wordpress      
  wordpress:
    #WordPressの公式イメージ(DockerHubにアップロードされているWP公式のDockerイメージ)
    #image: wordpress:latest
    build: ./wp
    volumes:
      - wordpress:/var/www/html
    #   - \\wsl.localhost\Ubuntu-22.04\home\ユーザー名\docker\wp:/var/www/html/wp-content
    # ファイルを編集したい場合はコメントアウトを外す wsl内に領域を指定しないと動作が激重になる
    depends_on:
      - mysql
    ports:
      - 3000:80
    environment:
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_HOST: mysql:3306
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: パスワード

  phpmyadmin:
    # container_name: phpmyadmin
    # コンテナの名前を明示したい場合はコメントアウトを外す。重複するとエラーになるので注意
    #DockerHub公式のphpmyadminイメージ
    image: phpmyadmin:latest
    depends_on:
      - mysql
    ports:
      - 4000:80
    environment:
      PMA_HOST: mysql
      PMA_USER: root
      PMA_PASSWORD: パスワード
volumes:
  mysql:
  wordpress:
```
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

## phpmyadminに接続し設定を変更
・wp-optionsのsiteurl,homeをhttp://localhost:3000/に変更 <br>
・wordpressのユーザー名、パスワードを入手できない場合は、wp-usersのユーザー名とパスワード（ハッシュ化必要）を追加 <br>
・wp-config.phpのデータベース名・ユーザー名・パスワードの設定を確認


## イメージとは
```
Dockerの文脈で言うと「コンテナイメージ」と言われ、コンテナを起動するために必要な一式が格納されているファイルのことを指します。
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