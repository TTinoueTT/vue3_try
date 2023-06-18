# vue3_try

Practice repository for vue3 and rails7 experimentation.

## 目次

1. [内容](#内容)
1. [おすすめの実践内容](#おすすめの実践内容)
1. [dockerコンテナのビルドについて](#dockerコンテナのビルドについて)
    1. [rails API の作成](#rails API の作成)
    1. [front ディレクトリに vue.js のプロジェクトを作成する](#front ディレクトリに vue.js のプロジェクトを作成する)
    1. [Vue Router のインストール](#Vue Router のインストール)

## 内容

> 基本的なことは

- vue3の実践
- railsの実践
- それに付随する新機能の実践

## おすすめの実践内容

## dockerコンテナのビルドについて
>
`docker compose build --no-cache`

### rails API の作成

#### rails new

> rails new は皆さんは実際には行わないです
> しかし、このプロジェクトが作られた時以下のコマンドを実行して,
> rails プロジェクトを作成したことを知っておいてください。

```sh
docker compose run --rm api rails new . -f -B --api -d mysql
```

> `rails new . -f -B --api` このコマンドの説明

- `rails new .` には、プロジェクト名を引数にとり、その中にプロジェクトを生成するが、すでに今回作った api ディレクトリにプロジェクトを作る場合、`.`でカレントディレクトリに Rails アプリを作成する
- `-f` はファイルが存在する場合に上書きする、すでにあるGemfileが対象となる
- `-B` は `bundle install` を実行しない
- ここでは実行していないが、`-d データベース名`、例えば、`-d postgresql`などで使用するDBシステムを指定できる、デフォルトが `sqlite3`
- `--api` で、API専用のRailsアプリケーションを作成する
<!-- - `-G` は、.gitignoreの生成をなくす -->

> コマンドを実行することで、Gemfile が Rails専用に書き換わる

#### --api オプションについて

[Rails による API 専用アプリケーション](https://railsguides.jp/api_app.html)
> `ApplicationController` が通常の `ActionController::Base` ではなく、`AcrionController::API` を継承する
> ビュー、ヘルパー、アセットを生成しないようにジェネレータを設定している

#### Gemfile の更新を確認/ Dockerイメージを再ビルド

> `rails new` により、Gemfile が書き換わる
> その為、再度イメージの再ビルドを行う

```sh
docker compose build api
```

#### アプリの起動を確認

> コンテナを起動させて、ブラウザでの接続を確認
`docker compose up api` => .env に書かれているポートでプラウザでの実行状態を確認(mysql のコネクションエラーであれば大丈夫)

#### DBコンテナへの接続

> apiコンテナにDBに必要な値を、環境変数から渡す
> database.yml を mysqlコンテナに接続できるように書き換え
> そして、この設定でコンテナを再度起動
`docker compose run --rm api rails db:create`

### front ディレクトリに vue.js のプロジェクトを作成する

> こちらも皆さんが実際には行わないものですが、vue.jsをインストールするまでの流れを確認しておいてください。
> 先に、docker build でイメージを作成していることが前提で進みます

コンテナ起動:

```sh
# docker compose run --rm front pnpx create-vite
docker compose run --rm front npm init vue@latest

# Project name → vue3_try
# Add Typescript → Yes
# Add JSX Support → No
# Add Vue Router for Single Page Application development? → Yes
# Add Pinia for state management? → Yes
# Add Vitest for Unit Testing? → Yes
# Add an End-to-End Testing Solution? › Cypress
# Add ESLint for code quality? → Yes
# Add Prettier for code formatting? → Yes


# docker compose run --rm front sh -c "cd vue3_try && npm install"

# package.json に追記
# "dependencies" 内に "sass": "^1.63.4"を追記

docker compose run --rm front npm install

docker compose up front

# でコンテナが起動されて、nuxtの起動できているか http://localhost:8030/ で動いているか確認する
```

viteの設定をdockerコンテナからの接続用に設定を行う
`vite.config.ts`を以下のように設定

```ts
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [vue()],
    // ここで、host設定を行う
    server: {
        host: true,
    },
});
```
