## nodenv
Node.jsのバージョン管理

### nodenv インストール

```zsh
$ brew install nodenv
```

```:~/.zshrc
# nodenv
[[ -d ~/.nodenv  ]] && \
  export PATH=${HOME}/.nodenv/bin:${PATH} && \
  eval "$(nodenv init -)"
```

```zsh
$ source ~/.zshrc
```

### 

```zsh
# インストール可能なバージョン一覧取得
$ nodenv install -l     
.
.
# バージョンインストール
$ nodenv install {バージョン番号}
# インストール後はrehash
$ nodenv rehash
# グローバル指定
$ nodenv global {バージョン番号}
# ローカル指定
$ nodenv local {バージョン番号}
# インストール済みバージョンの確認
$ nodenv versions
# node のバージョン確認
$ node -v
```
