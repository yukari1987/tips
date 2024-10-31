## 権限を確認する方法
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

## Volumeをエクスポート
```
for volume in $(docker volume ls -q); do docker run --rm -v $volume:/source -v $(pwd):/backup alpine tar -czf /backup/$volume.tar.gz -C /source ./ done
```
これでDocker内のvolumeをtarファイルに保存できる

## volume復元 未確認
```
# 特定のコンテナのボリュームを復元
container_name="your_container_name"
backup_dir="./volume_backups/${container_name}"

# バックアップファイルごとに復元
for backup in $backup_dir/*.tar.gz; do
  volume_name=$(basename $backup .tar.gz)
  echo "Restoring volume: $volume_name"
  docker volume create $volume_name
  docker run --rm -v $volume_name:/target -v $backup_dir:/backup alpine tar -xzf /backup/$(basename $backup) -C /target
done
```