# zsh設定

## Oh-my-zsh
- [こちら](https://qiita.com/shun198/items/c60ec1cce9c9bf1e8c26)を参考にインストール。
```
> git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```
- 新たな.zshrcが生成され、もともとあったものは、.zshrc.pre-oh-my-zshという名前のファイルに変わっていた。そのため、設定したaliasなどは使えなくなった。
- そこで、[こちら](https://qiita.com/nagisa-afadfadf/items/38eb26f01ba1a0bcc5c8)を参考に、.zshrcの名前を変更した。
```
> mv .zshrc .oh-my-zshrc
> mv .zshrc.pre-oh-my-zsh .zshrc
```

## powerlevel10k
- デザインを[こちら](https://qiita.com/shun198/items/c60ec1cce9c9bf1e8c26)に従ってpowerlevel10kに変更。
再設定したい場合は以下のコマンドを入力。
```
> p10k configure
```

## zsh-autosuggestions
```
> git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-autosuggestions
```
- 以下を.oh-my-zshrcに追記して有効化。
```
plugins=(
    git
    zsh-autosuggestions
)
```
- .oh-my-zshrcに以下を追記すると、suggestされる文字がハイライトされる。
```
ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=blue'
```

## zsh-syntax-highlighting
- [こちら](https://zenn.dev/ring_belle/books/mac-environment/viewer/shell-oh-my-zsh-syntax-highlight)を参考にインストール。
```
> git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
- 入力履歴に基づき、コマンドの予測変換をしてくれる。→を押すことでそのコマンドを選択できる。

## zsh-bat
- [こちら](https://zenn.dev/ring_belle/books/mac-environment/viewer/shell-oh-my-zsh-bat)を参考にインストール。
```
> brew install bat
> git clone https://github.com/fdellwing/zsh-bat.git $ZSH_CUSTOM/plugins/zsh-bat
```
- catコマンドが高度なものとなり、シンタックスのハイライトや、行番号が出力される。

## copypath
- .oh-my-zshのplugin部分に「copypath」を追加。
- 「copypath」と入力することで、カレントパスをコピー。
