## direnvのインストールについて

direnv ••• .envrcを作成すると、そのディレクトリ内でのみ有効な環境変数を設定することができる。  

```
$ brew install direnv

# ~/bash_profile に以下を記述
eval "$(direnv hook bash)"

$ exec $SHELL -l
```
