# 用語集
## OS
### Ubuntu
Linuxをベースに作られたOS。

### Linux
Unix系OS。

### UNIX
MacOSやLinuxのベースとなっている

### WSL2(Windows Subsystem for Linux 2)
Windows上でLinuxを動作させるための実行環境。

## Linux（Windows）で使う用語
### カーネル
OSの中核。ソフトウェアとハードウェアを繋げ、ユーザーの指示を反映させる。

### シェル
ユーザーの指示をカーネルへ伝えるためのツール

#### sh
シェルの種類のひとつ。

#### bash
シェルの種類のひとつで、shを元にグレードアップしたもの。

### コマンド
シェルに入力する命令文

## MacOSで使う用語

### ターミナル
LinuxやWindowsで言うところのシェル。

### zsh
ターミナルの種類。

## Webサーバーソフト
### Apache
webサーバー（サーバーのデータをブラウザに送る）のソフト。
### nginx(エンジンエックス)
Apacheより複数のデータ処理が得意。

## ポート番号
コンピュータで実際に通信される出入口の番号で、URLやIPアドレスが住所とすると、ポートは玄関のようなもの。

### パケット
TCP／IPネットワークで通信を行う際、データはIP（Internet Protocol）によって分割される。この分割されたデータのことをパケットという。

## Git
ソースコードの変更履歴を管理するバージョン管理システム。
ソースコードの保存場所は、ローカルリポジトリと、リモートリポジトリ。リモートリポジトリはGitHub以外に、独自のサーバーを設定することもできる。

### GitHub
Gitリポジトリをホスティング（インターネット上で利用可能にすること）するための、ウェブベースのプラットフォーム。

### GitとGitHubの違い
GitはWeb上のプラットフォームに依存しない。GitHubはホスティングすることでコラボレーションを可能にする。

## asdf
プログラミング言語やgitなどのツールを一括でバージョン管理できるコマンドラインツール。

## Node.js
### パッケージマネージャについて

#### npm
パッケージマネージャの種類。パッケージと呼ばれる単位（ビルド済）でソースコードが公開されており、パッケージをインストールするとコードが利用できる。
Node.jsをインストールすると使える。

#### yarn
パッケージマネージャの種類。JavaScript。npmと互換性がある。package.jsonが使える。
npmよりインストールが速く、npmより厳密にモジュールのバージョンを固定でき、互換性がある。


## Vite
フロントエンドのビルドツール。作成したアプリケーションを本番環境にデプロイする際にはビルドする必要がある。複数のファイルをひとつにまとめて、実行可能なファイルを作成する。

従来はWebpackなどのバンドルツールを使用してビルドを行っていたが、Viteはビルドする前に依存関係の解決と最低限のバンドルだけを行い、ESModules（JavaScriptのコードをモジュールという単位に分割し、必要な時に読み込む）のimportを通してソースコードを読み込むため、従来のビルドツールに比べて高速に立ち上げることができる。

## Wi-Fiの接続の裏側(AIより引用)
### 1.インターネットサービスプロバイダ（ISP）から接続
IPSはモデム（アナログ信号とデジタル信号を相互変換する、ネット接続の際に必要な機器）を通じて、インターネット接続を提供。このモデムはケーブル、DSL、光ファイバーなどの技術を利用している。

### 2.モデムとルーターの接続
イーサネットケーブル（LANを構成する機器をつなぐ通信ケーブル）を使用して接続。

### 3.ローカルネットワークの作成
WiFiルーターは、無線（WiFi）および有線（イーサネット）の両方を使用して、ローカルネットワーク（同じエリアで接続）を作成。

### 4.NATとDHCP
WiFiルーターは、ネットワークアドレス変換（NAT）を使用して、単一のパブリックIPアドレスを複数のプライベートIPアドレスに変換します。また、動的ホスト構成プロトコル（DHCP）を使用して、接続されたデバイスにIPアドレス、サブネットマスク、デフォルトゲートウェイ、DNSサーバーなどのネットワーク設定を自動的に割り当てます。

### 5.ファイアウォールとセキュリティ
WiFiルーターには、ファイアウォール機能が組み込まれており、外部からの不正アクセスからローカルネットワークを保護します。また、WPA2などの暗号化プロトコルを使用して、無線通信を保護します。

### 6.帯域幅の共有
接続されたすべてのデバイスは、ISPから提供される帯域幅を共有します。帯域幅を多く消費するアプリケーションやデバイスがある場合、他のデバイスのインターネット速度が低下する可能性があります。

### 7.高度な機能
一部のWiFiルーターには、QoS（Quality of Service）、VPNサーバー、ゲストネットワーク、ペアレンタルコントロールなどの高度な機能が含まれています。これらの機能により、ネットワークのパフォーマンスを最適化し、セキュリティと利便性を向上させることができます。


