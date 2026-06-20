# latexmkrcを用いたtexの自動コンパイル

1. vimtexのインストール
	- `~/.config/nvim/lua/plugins/tex.lua`に以下を記述。  
	```  
	return {
	  {
	    "lervag/vimtex",
	    lazy = false,
	    init = function()
	      vim.g.vimtex_compiler_method = "latexmk"
	      vim.g.vimtex_view_method = "skim"
	      vim.g.vimtex_quickfix_mode = 0
	
	      vim.g.vimtex_compiler_latexmk = {
	        continuous = 1,
	        options = {
	          "-verbose",
	          "-file-line-error",
	          "-synctex=1",
	          "-interaction=nonstopmode",
	        },
	      }
	
	      vim.api.nvim_create_autocmd("User", {
	        pattern = "VimtexEventCompileSuccess",
	        callback = function()
			  vim.fn.jobstart({ "open", "-a", "Skim" }, { detach = true })
	        end,
	      })
	    end,
	  },
	}
	```


2. .latexmkrcの作成
	- メインのtexファイルがあるディレクトリに`.latexmkrc`を作成し、以下を記述。
	```
	#!/usr/bin/env perl
	
	$latex = 'platex -synctex=1 -interaction=nonstopmode -file-line-error %O %S';
	$bibtex = 'pbibtex %O %B';
	$dvipdf = 'dvipdfmx %O -o %D %S';
	
	$pdf_mode = 3;
	```


3. 他のtexファイルについて
	- 先頭に、そのtexファイルからのメインtexファイルの相対パスを記述しておく。
	```latex
	% !TEX root = ../main.tex
	```


4. Skimのインストール
	- [HP](https://skim-app.sourceforge.io/index.html)からPDF viewerのSkimをダウンロード、インストールする。
	- macのデフォルトのビューアでも良いが、自動コンパイルした後にpdfの先頭に戻ってしまうので、あまりお勧めしない。


5. コンパイル
	- NeovimのNormal modeの状態で、`\ll`をタイプしてEnterを押すと、コンパイルしてくれる。
	- これを一度やっておくと、以降は保存するだけでコンパイルしてくれる。
