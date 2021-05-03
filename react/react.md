# React

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
[最新版で学ぶwebpack 5入門JavaScriptのモジュールバンドラ](https://ics.media/entry/12140/)