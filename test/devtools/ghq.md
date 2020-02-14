# ghq関連の導入

参考：[ghq, peco, hubで快適Gitライフを手に入れよう！](https://qiita.com/itkrt2y/items/0671d1f48e66f21241e2)


## インストール

```
$ brew install ghq
$ brew install peco
$ brew install hub
```

## セットアップ

### ghq

* `ghq look` は廃止され、 `ghq get --look` になったらしい。  
  ただ、サブシェルが起動されてしまうため、基本的には素直にcdした方が良いとのこと。  
  https://songmu.jp/riji/ 

### g, gh alias

~/.zshrc に以下を追記
```
alias g='cd $(ghq root)/$(ghq list | peco)'
alias gh='hub browse $(ghq list | peco | cut -d "/" -f 2,3)'
```
