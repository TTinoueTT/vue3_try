FROM node:18.16-alpine3.17

# docker-compose.yml で指定した、build時に渡す変数のキーで受け取り実行
ARG WORKDIR
ARG CONTAINER_PORT

# コンテナ内で利用する環境変数を定義
ENV HOME=/${WORKDIR} \
    # マシン語の英語を文字コード指定
    LANG=C.UTF-8 \
    TZ=Asia/Tokyo \
    # ブラウザからコンテナに接続する際に開けておく
    HOST=0.0.0.0

# ENV check（このRUN命令は確認のためなので無くても良い）

RUN echo ${HOME}
RUN echo ${CONTAINER_PORT}



WORKDIR ${HOME}

EXPOSE ${CONTAINER_PORT}

RUN npm install -g pnpm
# RUN pnpm --version
# RUN npx pnpm install -g pnpm

# RUN pnpm dlx nuxi init app

# RUN pnpm install



# pnpm を使った高速化を行う為の追加の記述
# RUN npm install -g pnpm　①
# 他にも検討段階のコマンドを上げていく
# RUN pnpm create vite my-vue-app --template vue-ts
# RUN pnpm create nuxt-app .
# RUN pnpm add --dev nuxt-vite
# RUN npx nuxi init nuxt3-app # ②
# RUN npx nuxi init . # ②

# nuxt.config.js に追記
# export default {
# ...
# buildModules: [
# // https://go.nuxtjs.dev/typescript
# '@nuxt/typescript-build',
# 'nuxt-vite' // 追加
# ],
# ...
# }


# 2021.12.13追記
# FROM node:14.15.1-alpine
# node v14.15.1は、$ yarn create nuxt-app appコマンド時に下記エラーが発生するので使用不可
# eslint-plugin-vue@8.2.0: The engine "node" is incompatible with this module. Expected version "^12.22.0 || ^14.17.0 || >=16.0.0". Got "14.15.1"

# create nuxt-appコマンド成功確認済みのnode version
# FROM node:16.13.1-alpine
# or
# FROM node:16-alpine(node v16.13.1)

# 現在のnode推奨版はこちらから => https://nodejs.org/ja/download/
