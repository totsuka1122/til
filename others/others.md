## windowsターミナルにgitのブランチ名を表示する方法
以下のコードを.bashrcに追記すればok
```
# setup git branch view
PS1='\[\033[1;32m\]\u@\h\[\033[00m\]:\[\033[1;34m\]\w\[\033[00m\]\[\033[1;31m\]$(__git_ps1)\[\033[00m\]\$ '
```
https://qiita.com/daijinload/items/2bb0031a706ce347aca6
