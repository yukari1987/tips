# Dockerfileの書き方
引用元　https://zenn.dev/ama_c/articles/d40305757fea98
```
書き方は２種類（shell形式、exec形式）、公式の推奨はexec
構成コマンド
・FROM
  ベースとなるイメージを指定。
  FROM [イメージ名]:[バージョン]  バージョン指定しなければlatest(最新版)が採用される
  例）FROM ubuntu:18.04

・RUN
  イメージ作成時に実行するコマンドの指定。&&を使用で１行で書ける。\で改行。
  イメージは１行ごとに層状に構成されtarファイルに圧縮されるため、行数が少なくなることを推奨されている

・WORKDIR
  絶対パスを指定することで、この命令の行以降はそのディレクトリでの操作となる。

・EXPOSE
  コンテナが特定のポートを listen していることを Docker に通知する役割

・ARG、ENV
  環境変数を指定。ARGは同一Dockerfile内でのみ使用可、ENVはコンテナに対しての環境変数。
  例）ARG[変数名]=[値] or ENV [環境変数名]=[値]


・COPY、ADD
  指定したファイルやディレクトリをコンテナ内にコピー（追加） する。
  例）COPY [コピー元] [コピー先] or ADD [追加元] [追加先]
  *コピー元のファイルはビルドコンテキスト内（Dockerfile が置いてあるディレクトリ以下の階層） に存在する必要があり，相対パスで記述
  *リモートファイルや tar 展開する必要がある場合はADD

・CMD、ENTRYPOINT
  コンテナ実行時にデフォルトで実行するコマンドやオプション指定。原則末尾に書く。複数書いても一番最後の命令しか処理されない。

・SHELL
  RUN, CMD, ENTRYPOINT 命令などの shell 形式で記述されている shell の種類を指定。デフォルトでは，Linux は["/bin/sh", "-c"]，windows は["cmd", "/S", "/C"]

・VOLUME
マウントする位置を示す。
```

## コピペ用
```
# FROM wordpress:php7.4-apache

FROM wordpress:latest
# 最新版をインストールしたいときはコメントアウトを外す FROMが重複するためphp7.4-apacheはコメントアウトする

# apt-get updateを実行してパッケージリストを更新し、vim、iputils-ping、iproute2をインストール。これにより、コンテナ内でテキストエディタ、pingコマンド、ネットワーク管理ツールが使用できる

RUN apt-get update -y \
    && apt-get install -y vim iputils-ping iproute2

# ~/.bashrcファイルにalias ll="ls -la"を追記しています。これにより、llコマンドでls -laと同等の動作が行われる

RUN echo 'alias ll="ls -la"' >> ~/.bashrc

# /usr/local/etc/php/php.iniファイルを作成しています。このファイルは、PHPの設定を行うために使用

RUN touch /usr/local/etc/php/php.ini

# /usr/local/etc/php/php.iniファイルにXdebugの設定を追記しています。この設定により、Xdebugがデバッグモードで動作し、ホストマシンのIPアドレスを使用して通信を行う。

RUN echo "[xdebug]\n \
xdebug.mode=debug\n \
xdebug.client_host=host.docker.internal\n \
xdebug.start_with_request=yes\n \
xdebug.client_port=9003" >> /usr/local/etc/php/php.ini


# RUN pecl install xdebug-3.1.6 \
#     && docker-php-ext-enable xdebug
# wordpress:latestを指定したらこっち

RUN pecl install xdebug-3.3.1 \
    && docker-php-ext-enable xdebug
# COPY ./wordpress-6.3.2/ /var/www/html
# phpは7.4を使いたいがwordpressは任意のバージョンを使いたい時用の記述

# ./content/wp-content.tar.gzファイルを/var/www/html/ディレクトリに展開しています。これにより、WordPressのwp-contentディレクトリが追加

ADD ./content/wp-content.tar.gz /var/www/html/
```
