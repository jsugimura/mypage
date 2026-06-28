# Neovim環境構築

## ディレクトリ構成
```
> cd ~/.config/nvim
> tree
.
├── init.lua
├── lazy-lock.json
├── lua
│   ├── config
│   │   ├── keymaps.lua
│   │   ├── lazy.lua
│   │   └── options.lua
│   └── plugins
│       ├── bufferline.lua
│       ├── cmp.lua
│       ├── colorscheme.lua
│       ├── indent.lua
│       ├── lsp.lua
│       ├── lualine.lua
│       ├── luasnip.lua
│       ├── oil.lua
│       ├── telescope-fzf.lua
│       ├── telescope.lua
│       ├── tex.lua
│       ├── treesitter-context.lua
│       └── treesitter.lua
└── snippets
    ├── fortran.lua
    └── tex.lua
```

## インストール
- brewによるインストール（[GitHub](https://lazy.folke.io/installation)）。
```
> brew install neovim

（中略）

> nvim --version
NVIM v0.11.5
Build type: Release
LuaJIT 2.1.1765007043
Run "nvim -V1 -v" for more info
```

- 設定ファイルやプラグインを入れるためのディレクトリ作成。
```
> cd .config
> mkdir nvim
> cd nvim
> mkdir -p lua/config
> mkdir -p lua/plugins
```

## 設定ファイル作成
### init.lua
Neovimの設定ファイル。
```
> vi init.lua
```
```
vim.g.mapleader = " "
vim.g.maplocalleader = " "

require("config.keymaps")
require("config.options")
require("config.lazy")

-- vim.opt.termguicolors = true

-- Tabキーによるインデントの表示
vim.opt.shiftwidth = 2
vim.opt.tabstop = 2
vim.opt.softtabstop = 2

vim.opt.clipboard = "unnamedplus"

-- 行番号の色指定
vim.api.nvim_set_hl(0, "LineNr", { fg = "#6c4ae0" })
vim.api.nvim_set_hl(0, "LineNrAbove", { fg = "#6c4ae0" })
vim.api.nvim_set_hl(0, "LineNrBelow", { fg = "#6c4ae0" })
vim.api.nvim_set_hl(0, "CursorLineNr", { fg = "#a9a1e1", bold = true })

-- 前回ファイルを閉じた位置にカーソルを戻す
vim.api.nvim_create_autocmd("BufReadPost", {
  pattern = "*",
  callback = function()
    local mark = vim.api.nvim_buf_get_mark(0, '"')
    local lcount = vim.api.nvim_buf_line_count(0)
    if mark[1] > 0 and mark[1] <= lcount then
      pcall(vim.api.nvim_win_set_cursor, 0, mark)
    end
  end,
})
```

### lazy.lua
プラグインマネージャ。
pluginsディレクトリにあるluaファイルを読み込んでプラグインが追加可能。
```
> vi lua/config/lazy.lua
```
```
-- Bootstrap lazy.nvim
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not (vim.uv or vim.loop).fs_stat(lazypath) then
  local lazyrepo = "https://github.com/folke/lazy.nvim.git"
  local out = vim.fn.system({ "git", "clone", "--filter=blob:none", "--branch=stable", lazyrepo, lazypath })
  if vim.v.shell_error ~= 0 then
    vim.api.nvim_echo({
      { "Failed to clone lazy.nvim:\n", "ErrorMsg" },
      { out, "WarningMsg" },
      { "\nPress any key to exit..." },
    }, true, {})
    vim.fn.getchar()
    os.exit(1)
  end
end
vim.opt.rtp:prepend(lazypath)

-- Make sure to setup `mapleader` and `maplocalleader` before
-- loading lazy.nvim so that mappings are correct.
-- This is also a good place to setup other settings (vim.opt)
vim.g.mapleader = " "
vim.g.maplocalleader = "\\"

-- Setup lazy.nvim
require("lazy").setup({
  spec = {
    -- import your plugins
    { import = "plugins" },
  },
  -- Configure any other settings here. See the documentation for more details.
  -- colorscheme that will be used when installing plugins.
  install = { colorscheme = { "habamax" } },
  -- automatically check for plugin updates
  checker = { enabled = true, notify = false },
})
```

## プラグイン
- [カラースキーム](colorscheme.html)
- [lualine](lualine.html)
- [tabline](tabline.html)
- [nvim-treesitter](nvim-treesitter.html)
- [nvim-treesitter-context](nvim-treesitter-context.html)
- [oil](oil.html)
- [telescope](telescope.html)
- [コマンド補完機能](autocommand.html)
- [indent-blackline](indent.html)
- [luasnip](luasnip.html)

## スニペット
- [LaTeX](latex.html)
- [Fortran](fortran.html)

## latexmkrc
以下のページ参照（[latexmkrcを用いたtexの自動コンパイル](../autotexcompile.html)）。




