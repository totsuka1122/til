## windowsターミナルにgitのブランチ名を表示する方法
以下のコードを.bashrcに追記すればok
```
# setup git branch view
PS1='\[\033[1;32m\]\u@\h\[\033[00m\]:\[\033[1;34m\]\w\[\033[00m\]\[\033[1;31m\]$(__git_ps1)\[\033[00m\]\$ '
```
[Gitのブランチ名をコンソールに表示する方法](https://qiita.com/daijinload/items/2bb0031a706ce347aca6)

## wslのディレクトリをエクスプローラーで開く
```
explorer.exe .
```
## httpをhttpsにリダイレクトする方法
[参考ページ](https://keywordfinder.jp/blog/seo/http-https-redirect/#htaccess)  
サイト全体で移行する場合
ではまず、サイト全体で“https”にリダイレクトさせる場合の記述方法です。
これは、以下のように.htaccessファイルに記述しておくことで全てのページがリダイレクトされます。
※“https”ではない場合、“https”へ301リダイレクトするという指示。
```
.htaccess
```
ファイルを作成し、以下を記載()
```
RewriteEngine on
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
```
