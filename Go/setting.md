# GOPATH と GOROOT
- GOROOT
  - goのインストールパスのこと(基本的には/usr/local/go)
  - 配下には/binがあり、その下にgo, godoc, gofmtがある
```
// GOROOTの取得 
go env GOROOT
```
- GOPATH
  - 外部パッケージのリソースが保存される(基本的には$HOME/go)
  - 配下には/binがあり、その下にGoの実行バイナリが保存される(ソースコードはbinではなくpkg/mod)
```
// GOPATHの取得
go env GOPATH
```
- 設定
```
export GOPATH=$(go env GOPATH)
export PATH=$PATH:$GOPATH/bin
```

# パッケージをインストールしてもコマンドが実行できない場合
パッケージをインストールして正しくcommandを入力しても実行できない場合、いわゆる「PATHが通っていない」状態かもしれません。
```
$ echo $GOPATH
$ echo $PATH
```
で、それぞれの環境変数を確認することができます。  
  
$GOPATHはgoの作業ディレクトリです。ファイルの読み込みなどの起点となります。  
$PATHはインストールしたパッケージのコマンドなどが記述されたバイナリファイルが格納される場所です。 このPATHを明示的に設定する必要があります。  
  
1行目のコマンドを実行した時に空行が返る場合、デフォルト値（go v1.8より）が適用されています。  
デフォルト値は下記コマンドで確認できます。  
```
$ go env | grep GOPATH
```
下記コマンドでPATHを通すことで、command not foundの事象は解決するかもしれません。
```
export GOPATH=$HOME/go;
export PATH=$PATH:$GOPATH/bin;
```
ターミナルでexportを記述するとターミナルを起動するたびに実行する必要があるので、  
`~/.bash_profile`に環境変数として設定してしまうのが楽です。
