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
## github action

![Untitled Diagram](https://user-images.githubusercontent.com/70890327/117373592-7dca3300-af06-11eb-8228-1f12269595f7.png)

## ISR (Incremental Static Regeneration)
> 直訳すると、(段階的な静的サイト生成)となります。
> 簡単に説明すると、リクエストに対して静的にビルドされたページを返す。かつ、有効期限を超えたら非同期で静的ページの再生成をSSRで行うことです。

- [ISR(Incremental Static Regeneration)とは？](https://qiita.com/yoshii0110/items/db707ed61030c01c2353)
