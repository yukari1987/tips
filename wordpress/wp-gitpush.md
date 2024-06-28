# WordPressにおけるGit使用手順

## 1. Gitのインストール
- 公式サイトからGitをダウンロードしてインストール

## 2. ローカル環境の設定
- ローカルにWordPressをインストール
- プロジェクトディレクトリに移動し、Gitリポジトリを初期化:
git init
Copy
## 3. .gitignoreファイルの作成
セキュリティやパフォーマンスの観点から、以下のファイルを除外: <br>
wp-config.php <br>
wp-content/uploads/ <br>
.htaccess
## 4. 初回コミット
git add .<br>
git commit -m "Initial commit"

## 5. リモートリポジトリの設定
GitHubなどのサービスでリポジトリを作成し、以下のコマンドでリンク:<br>
git remote add origin [リポジトリURL]<br>
git push -u origin main
## 6. 開発ワークフロー
### 6.1 新機能開発
```
bashCopygit checkout -b feature/new-feature 
# 変更を加える
git add .
git commit -m "Add new feature"
```
### 6.2 メインブランチへのマージ
```
bashCopygit checkout main
git merge feature/new-feature
```
### 6.7 デプロイ
```
サーバーにSSH接続
WordPressディレクトリに移動
Gitをセットアップ（初回のみ）
変更をプル:
bashCopygit pull origin main
```
## 注意事項

セキュリティに関わる情報は.gitignoreに追加し、リポジトリにコミットしないよう注意<br>
大規模な変更は別ブランチで行い、レビュー後にメインブランチへマージ<br>
定期的にバックアップを取ることを忘れずに<br>

## 追加のヒント
ブランチ戦略
```
main: 安定版のコード
develop: 開発用のメインブランチ
feature/*: 新機能開発用
hotfix/*: 緊急のバグ修正用
```
コミットメッセージの規則
```
簡潔で明確なメッセージを心がける
例: "Add user authentication feature", "Fix header styling bug"
```
プラグインとテーマの管理
```
カスタムプラグインやテーマは別のリポジトリで管理することも検討
```
自動デプロイ
```
Git hooksやCIツールを使用して自動デプロイを設定することも可能
```
コラボレーション
```
プルリクエストを活用してコードレビューを行う
イシューを使用してタスク管理を行う
```
セキュリティ
```
wp-config.phpなどの重要ファイルは.gitignoreに追加し、別途管理
本番環境の認証情報はGitで管理せず、環境変数などで扱う
```
パフォーマンス
```
大きなメディアファイルはGitで管理せず、別の方法（例：S3）で同期を検討
```
バージョン管理
```
重要な変更点はCHANGELOG.mdに記録
セマンティックバージョニングを使用してリリースを管理
```

これらの手順とヒントを参考に、効率的なWordPress開発ワークフローを構築してください。