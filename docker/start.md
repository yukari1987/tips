#docker-compose.ymlを作成
```
version: '3'
services:
  #先程作成したイメージを指定
  app:
    # Dockerfileを指定することも可能
    # https://qiita.com/kai_kou/items/eaafa3cb15e1496f50ec
    image: ubuntu-nginx
    ports:
      - 8080:80

  # 利用するイメージを追加
  mysql:
    image: mysql:8.0-oracle
    environment:
      - MYSQL_ROOT_PASSWORD=password
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    environment:
      - PMA_ARBITRARY=1
      - PMA_HOST=mysql
      - PMA_USER=root
      - PMA_PASSWORD=password
    ports:
      - 8081:80
```
#イメージとは
```
Dockerの文脈で言うと「コンテナイメージ」と言われ、コンテナを起動するために必要な一式が格納されているファイルのことを指します
```
#dockerfileとは
```
イメージを作成するための設計図。
```
#docker composeとは
```
Docker Compose とは、複数コンテナのアプリケーションを定義・共有するために役立つように、開発されたツールです。Compose があれば、サービスを定義する YAML ファイルを作成し、コマンドを１つ実行するだけで、瞬時にすべて立ち上げたり、すべてを削除したりできます。
```

引用
https://qiita.com/yuya_sega/items/0fb78b064a6d64fe0979