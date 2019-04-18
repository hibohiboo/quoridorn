FROM node:11.14.0

# コンテナ上の作業ディレクトリ作成
WORKDIR /app

# 後で確認出来るようにpackage.jsonを作成
RUN npm init -y

RUN yarn add bcdice-js
RUN yarn add file-saver
RUN yarn add jszip
RUN yarn add skyway-js
RUN yarn add vue
RUN yarn add vuex
RUN yarn add moment
RUN yarn add  @babel/runtime-corejs2

RUN yarn add --dev autoprefixer
RUN yarn add --dev chalk
RUN yarn add --dev chromedriver
RUN yarn add --dev copy-webpack-plugin
RUN yarn add --dev cross-spawn
RUN yarn add --dev css-loader
RUN yarn add --dev eslint
RUN yarn add --dev eslint-config-standard
RUN yarn add --dev eslint-friendly-formatter
RUN yarn add --dev eslint-loader
RUN yarn add --dev eslint-plugin-import
RUN yarn add --dev eslint-plugin-node
RUN yarn add --dev eslint-plugin-promise
RUN yarn add --dev eslint-plugin-standard
RUN yarn add --dev eslint-plugin-vue
RUN yarn add --dev extract-text-webpack-plugin
RUN yarn add --dev file-loader
RUN yarn add --dev friendly-errors-webpack-plugin
RUN yarn add --dev html-webpack-plugin
RUN yarn add --dev jest
RUN yarn add --dev jest-serializer-vue
RUN yarn add --dev json-loader
RUN yarn add --dev nightwatch
RUN yarn add --dev node-notifier
RUN yarn add --dev optimize-css-assets-webpack-plugin
RUN yarn add --dev ora
RUN yarn add --dev portfinder
RUN yarn add --dev postcss-import
RUN yarn add --dev postcss-loader
RUN yarn add --dev postcss-url
RUN yarn add --dev rimraf
RUN yarn add --dev selenium-server
RUN yarn add --dev semver
RUN yarn add --dev shelljs
RUN yarn add --dev uglifyjs-webpack-plugin
RUN yarn add --dev url-loader
RUN yarn add --dev vue-jest
RUN yarn add --dev webpack
RUN yarn add --dev webpack-bundle-analyzer
RUN yarn add --dev webpack-cli
RUN yarn add --dev webpack-merge
RUN yarn add --dev webpack-dev-server
RUN yarn add --dev yaml-loader
# RUN yarn add global babel-upgrade

RUN yarn add --dev babel-eslint
RUN yarn add --dev babel-helper-vue-jsx-merge-props
RUN yarn add --dev babel-jest
RUN yarn add --dev babel-plugin-dynamic-import-node
RUN yarn add --dev babel-cli
RUN yarn add --dev @babel/core
RUN yarn add --dev @babel/plugin-proposal-class-properties
RUN yarn add --dev @babel/plugin-proposal-decorators
RUN yarn add --dev @babel/plugin-proposal-export-namespace-from
RUN yarn add --dev @babel/plugin-proposal-function-sent
RUN yarn add --dev @babel/plugin-proposal-json-strings
RUN yarn add --dev @babel/plugin-proposal-numeric-separator
RUN yarn add --dev @babel/plugin-proposal-throw-expressions
RUN yarn add --dev @babel/plugin-syntax-dynamic-import
RUN yarn add --dev @babel/plugin-syntax-import-meta
RUN yarn add --dev @babel/plugin-syntax-jsx
RUN yarn add --dev @babel/plugin-transform-modules-commonjs
RUN yarn add --dev @babel/plugin-transform-runtime
RUN yarn add --dev @babel/preset-env
RUN yarn add --dev @babel/register

# babel-loaderはデフォルトだとbabel7.x向けの8.xがインストールされる。 このプロジェクトではbabel6.xが使われているので、それに対応したbabel-loaderの7.xを使う
RUN yarn add --dev babel-loader
#
RUN yarn add --dev vue-style-loader
RUN yarn add --dev vue-template-compiler
RUN yarn add --dev babel-plugin-transform-vue-jsx@4.x
RUN yarn add --dev vue-loader@14.x

