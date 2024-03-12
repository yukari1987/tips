#gitの設定
```
//初期設定
git config --global user.name 'ユーザー名'
git config --global user.email 'email'
git config --list

//SSH設定
sshkeygen -t ed25519 -C 'email'
[参照](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

//設定確認
git config --list
```

#GitHubにアップロード
```
//リポジトリの初期化
git init

//ステージングを上げ待ち状態にする
git add .
//ステージにあるファイルをリスト化
git ls-files --stage
//ステータスを確認
git status

//コミットメッセージを入力
git commit -m "メッセージ"

//ブランチの名前をmainに変更
git branch -M main

//接続する
git remote add origin 接続コード(GitHubから取得)

//プッシュ
git push -u origin main
```

#GitHubからインストール
```
git clone SSHキー
```

#リモート接続先を変更する
```
//現在のリモートURLを確認
origin アドレス.git

//新しいURLに変更
git remote set-url origin 新しいURL
```

#リモートリポジトリの変更を取得
```
git pull リモート名 ブランチ名
```