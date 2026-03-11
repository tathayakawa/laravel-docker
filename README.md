# laravel-docker

## 概要

* 多くのアプリケーションで必要となる環境設定を全て終えた状態のLaravel実行環境。
* PostgreSQL, MySQL 両対応。`docker-compose.yml` はPostgreSQLを想定。
* VSCode Remote Containers と併せて使うことで必要な開発ツールが全てインストールされる。

## 接続（VSCode）

* [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) をインストール
* このリポジトリをワークスペースとして開く
* コマンドパレットより次のコマンドを実行  
  > Dev Containers: **Reopen in Container**  
  > 開発コンテナー: コンテナーで再度開く  

## セットアップ（初回のみ）

> \# *URL* には対象プロジェクトのリポジトリURLを指定  
> *container $* git clone *URL* **./**  
> *container $* composer install  
> *container $* npm install  
> *container $* ./artisan migrate:fresh --seed  
> *container $* npm run build  


## ER図の出力

> *host $* docker compose -f docker-compose.yml -f docker-compose.schemaspy.yml run --rm schemaspy

* *schema/* ディレクトリ配下にER図が出力される

## URL

|アプリケーション|URL|
|-|-|
|Laravel|http://localhost:8000/|
|Mailpit|http://localhost:8025/|

## .env によるカスタマイズ（任意）

|環境変数|意味|デフォルト|
|-|-|-|
|APP_DEBUG|Laravel設定|true|
|PORT_WEB_APP|公開ポート / Laravel|8000|
|PORT_PGSQL|公開ポート / PostgreSQL|0 *非公開*|
|PORT_WEB_MAIL|公開ポート / Mailpit|8025|
|PORT_BROWSERSYNC|公開ポート / Browsersync|3000|

