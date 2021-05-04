# React

## js vs ts

### ts の Pros
- 静的型付け言語
- 型推論
- Null安全性
  - NullPointerException(Java)の回避
- IDEでコンパイル前にエラーを確認できる
- [Slackブログ](https://www.infoq.com/jp/news/2017/04/going-typescript-slack/)
- [Airbnb](https://www.infoq.com/jp/news/2020/02/airbnb-graphql-migration/)

### ts の Cons

- 学習コスト
- コンパイルに時間がかかる


## Create React App
- JS
```
$ npx create-react-app {app名}
```
- TypeScript
```
$ npx create-react-app {app名} --template=typescript
```

- <React.StrictMode> とは
バージョンが進んで非推奨になった API の使用や意図しない副作用といった、アプリケーションの潜在的な問題点を見つけて
warning で教えてくれる React の『Strict モード42』という機能を有効にするためのラッパー。
アプリケーションをより安全なものにするために Create React App がバージョン 3.4.1 からデフォルトで追加してくれるようになった

- コンパイル
CRA で作成したプロジェクトでは、ソースコードファイルはコンパイラである Babel によってコンパイルされて、
それがバンドラである webpack によって適切な形にまとめられ、さらにそれらが相互に関連付けられる

- pkgのupdate
package.jsonに記述してある各パッケージのバージョン範囲に従って、全てのインストール済みのパッケージを一括アップデートする

```
yarn upgrade-interactive --latest
```
- webpack
  - [最新版で学ぶwebpack 5入門JavaScriptのモジュールバンドラ](https://ics.media/entry/12140/)

## tsconfig.json

- "jsx": "react-jsx"
  - これが有効になっていると、`import React from 'react'`文が省略できる
  - `"jsx": "react"`にすると`import`文が必要になる
  - [新しいJSXトランスフォーム](https://ja.reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)
  

## JSXの記述

- トップレベルの要素は1つでなければいけない
- ReactElement オブジェクトを生成するためのシンタックスシュガー

```ts
// Error
const elems = (
  <div>foo</div>
  <div>bar</div>
  <div>baz</div>
)

// Valid
// フラグメント(空のタグ)を使うことで、不要なノードを追加することなくまとめて扱える
const elems = (
  <>
    <div>foo</div>
    <div>bar</div>
    <div>baz</div>
  </>
)
```

## props

- Properties（プロパティ）』の略で、コンポーネントを関数として考えたとき、その引数に相当するもの
- 基本的にはコンポーネントが呼ばれるときに外から与えられる読み取り専用の変数グループをオブジェクトにまとめたもの

## コンポーネント

- ユーザー定義コンポーネント
  - 必ず大文字から始めること(小文字から始めると、呼び出すことができなくなる)

- 組み込みコンポーネント
  - htmlと記述が異なる主なもの
    - `class` -> `className`
    - `for` -> `htmlFor`

## Linter

- [npm trends of linter](https://www.npmtrends.com/eslint-vs-tslint-vs-awesome-typescript-loader-vs-ts-loader-vs-prettier-vs-standard-vs-eslint-config-airbnb-vs-beautify-vs-jshint-vs-eslint-config-google)
- 主流は`ESLint`

### ESLintの環境構築

- バージョン確認
```sh
yarn list eslint
```

- 各種パッケージを最新にする(必要に応じて)
```sh
yarn upgrade-interactive --latest
yarn upgrade typescript@latest
```

- TypeScript ESLint が提供しているパッケージ群をインストール
  - `typescript-eslint-parser` と `eslint-plugin-typescript`もあるが、これらは非推奨
  - ESLint本体を除くエコシステムのパッケージは主に3種類
    - パーサ(Parser) ... ソースコードを特定の言語仕様に沿って解析してくれるライブラリ。ESLint には JavaScript のパーサが組み込まれているが、標準では TypeScript には対応していないので、TypeScript のパーサを導入する
    - プラグイン（Plugin）... ESLint の組み込みルール以外に独自のルールを追加するもの。それらを適用した推奨の共有設定とパッケージングして提供されることが多い
    - 共有設定（Shareable Config）... 複数のルールの適用をまとめて設定するもの。ESLint に同梱される eslint:recommended や Airbnb 社が提供している eslint-config-airbnb14 が有名
  
```sh
 # まずはESLintの設定ファイルを作成
$ yarn eslint --init
? How would you like to use ESLint?
》To check syntax, find problems, and enforce code style

? What type of modules does your project use? JavaScript modules (import/export)
》JavaScript modules (import/export)

? Which framework does your project use?
》React

? Does your project use TypeScript?
》Yes

? Where does your code run?
》Browser

? How would you like to define a style for your project?
》Use a popular style guide

? Which style guide do you want to follow?
》Airbnb: https://github.com/airbnb/javascript

? What format do you want your config file to be in?
》JavaScript

The config that you've selected requires the following dependencies:

eslint-plugin-react@^VERSION @typescript-eslint/eslint-plugin@latest eslint-config-airbnb@latest eslint@^VERSION eslint-plugin-import@^VERSION eslint-plugin-jsx-a11y@^VERSION eslint-plugin-react-hooks@^VERSION @typescript-eslint/parser@latest
? Would you like to install them now with npm?
》No

# 最後にNoと答えるのは、yarnでパッケージ管理を統一するため。
# Yesにすると、npmでインストールされる

```

- 最後にNoと答えたので、拡張ルールセットやプラグインはまだインストールされていないため、以下のコマンドでインストールする
```sh
$ yarn add -D eslint-plugin-react @typescript-eslint/eslint-plugin \
 eslint-config-airbnb eslint-plugin-import eslint-plugin-jsx-a11y \
 eslint-plugin-react-hooks @typescript-eslint/parser
$ typesync
$ yarn
```

- eslint.jsの設定
```js
extends: [
  'plugin:react/recommended',
  'airbnb',
  'airbnb/hooks',
  'plugin:import/errors',
  'plugin:import/warnings',
  'plugin:import/typescript',
  'plugin:@typescript-eslint/recommended',
  'plugin:@typescript-eslint/recommended-requiring-type-checking',
],
```
その他多くの設定があるので、必要に応じて書籍で確認すること。

## フォーマッタ

- Prettier
  - ESLintとバッティングすることがあるから、設定に注意が必要
  - `eslint-plugin-prettir`は公式で非推奨となった

設定については、必要に応じて書籍で確認すること。

## CSS Linter

- stylelint

設定については、必要に応じて書籍で確認すること。

