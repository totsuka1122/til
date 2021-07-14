# install
## ubuntuにgolangを入れる方法  
1. ```curl -O https://dl.google.com/go/go1.15.5.linux-amd64.tar.gz```   
		- ubuntu上でこのコマンドを実行し、パッケージを取得する。   
2. ```sudo tar -C /usr/local -xzf go1.15.5.linux-amd64.tar.gz```
		- パッケージを展開して配置する。
3. HOMEディレクトリへ戻り、`.profile`を開き以下のPATHを通す
```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:/usr/local/go/bin
export GOPRIVATE=github.com/artis-inc/*
```

## GOPATH と GOROOT
- GOROOT
  - goのインストールパスのこと(基本的には/usr/local/go)
  - 配下には/binがあり、その下にgo, godoc, gofmtがある
```bash
# GOROOTの取得 
go env GOROOT
```
- GOPATH
  - 外部パッケージのリソースが保存される(基本的には$HOME/go)
  - 配下には/binがあり、その下にGoの実行バイナリが保存される(ソースコードはbinではなくpkg/mod)
```bash
# GOPATHの取得
go env GOPATH
```
- 設定
```bash
export GOPATH=$(go env GOPATH)
export PATH=$PATH:$GOPATH/bin
```

## パッケージをインストールしてもコマンドが実行できない場合
パッケージをインストールして正しくcommandを入力しても実行できない場合、いわゆる「PATHが通っていない」状態かもしれません。
```bash
$ echo $GOPATH
$ echo $PATH
```
で、それぞれの環境変数を確認することができます。  
  
$GOPATHはgoの作業ディレクトリです。ファイルの読み込みなどの起点となります。  
$PATHはインストールしたパッケージのコマンドなどが記述されたバイナリファイルが格納される場所です。 このPATHを明示的に設定する必要があります。  
  
1行目のコマンドを実行した時に空行が返る場合、デフォルト値（go v1.8より）が適用されています。  
デフォルト値は下記コマンドで確認できます。  
```bash
$ go env | grep GOPATH
```
下記コマンドでPATHを通すことで、command not foundの事象は解決するかもしれません。
```bash
export GOPATH=$HOME/go;
export PATH=$PATH:$GOPATH/bin;
```
ターミナルでexportを記述するとターミナルを起動するたびに実行する必要があるので、  
`~/.bash_profile`に環境変数として設定してしまうのが楽です。

## godocを使用する

```sh
go get golang.org/x/tools/cmd/godoc
```
```sh
godoc -http=:6060
```

## バージョンをUpdateする場合
- https://golang.org/dl/
	- ` installation instructions`のリンクでinstall方法のページへ飛ぶ

移動
```sh
cd /usr/local
```
/usr/localで実行
```sh
wget https://dl.google.com/go/go1.16.6.linux-amd64.tar.gz
```
以下はsudoで実行？
```sh
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.16.6.linux-amd64.tar.gz
```
再起動で完了
```sh
go version
```
