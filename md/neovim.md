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
### カラースキーム
dogrun（[GitHub](https://github.com/wadackel/vim-dogrun)）。
```
> vi lua/plugins/colorscheme.lua

return {
  {
    "wadackel/vim-dogrun",
    priority = 1000,
    config = function()
      -- カラースキームを読み込む
      vim.cmd("colorscheme dogrun")
    end,
  },
}
```

### lualine
ステータスラインの表示（[GitHub](https://github.com/nvim-lualine/lualine.nvim)）。
```
> vi lua/plugins/lualine.lua
```
```
return {
  {
    "nvim-lualine/lualine.nvim",
    dependencies = { "nvim-tree/nvim-web-devicons" },
    config = function()
      require("lualine").setup({
        options = {
          theme = "auto",  -- カラースキームに応じて自動で合わせる
          icons_enabled = true,
        },
      })
    end,
  },
}
```

### tabline
bufferの表示（[Github](https://github.com/kdheepak/tabline.nvim)）。
```
> vi lua/plugins/lualine.lua
```
```
return {
  {
    "akinsho/bufferline.nvim",
    version = "*",
    dependencies = { "nvim-tree/nvim-web-devicons" },
    config = function()
      require("bufferline").setup({
        options = {
          mode = "buffers",   -- タブではなくバッファ一覧を表示（一般的）
          diagnostics = "nvim_lsp", -- LSP の診断アイコンを表示
          show_buffer_close_icons = true,
          show_close_icon = false,
          separator_style = "slant",  -- タブの区切りをスラントに
        },
      })
    end,
  },
}
```
また、`lua/config/keymaps.lua`を作成し、以下を記述。
```
vim.keymap.set("n", "<C-h>", "<cmd>bprev<CR>")
vim.keymap.set("n", "<C-l>", "<cmd>bnext<CR>")
```
これにより、`<Ctrl> + h/l`でbuffer間を移動可能。

### nvim-treesitter
構文のハイライト（[GitHub](https://github.com/nvim-treesitter/nvim-treesitter)）。
```
> vi lua/plugins/treesitter.lua
```
```
return {
  {
    "nvim-treesitter/nvim-treesitter",
    build = ":TSUpdate",   -- インストール後にパーサを更新
    config = function()
      require("nvim-treesitter.configs").setup({
        -- インストールしたいパーサ（必要なものを追加）
        ensure_installed = {
          "lua",
          "vim",
          "vimdoc",
          "query",
          "bash",
          "python",
          "c",
          "cpp",
          --"fortran",
          "json",
          "markdown",
          "markdown_inline",
          "bibtex",
        },

        highlight = {
          enable = true,        -- Treesitter ハイライトを有効化
	  disable = { "fortran" },
        },

        indent = {
          enable = true,        -- TS ベースの自動インデント
        },
      })
    end,
  },
}
```

### nvim-treesitter-context
スクロール時に画面上部でコードを固定（[GitHub](https://github.com/nvim-treesitter/nvim-treesitter-context)）。
```
> vi lua/plugins/treesitter-context.lua
```
```
return {
  {
    "nvim-treesitter/nvim-treesitter-context",
    config = function()
      require("treesitter-context").setup({
        enable = true,             -- 有効化
	disable = { "fortran" },
        max_lines = 4,             -- コンテキスト行数の最大値（0 で制限なし）
        min_window_height = 10,    -- ウィンドウが小さすぎる時は非表示
        line_numbers = true,       -- コンテキストに行番号表示
        multiline_threshold = 20,  -- 長いブロックに対する閾値
        trim_scope = "outer",      -- コンテキストが多い場合は外側から削る
        mode = "cursor",           -- cursor or topline
        separator = nil,           -- 区切り線（"─" なども可）
      })
    end,
  },
}
```

### oil
ファイルエクスプローラ（[GitHub](https://github.com/stevearc/oil.nvim)）。
```
> vi lua/plugins/oil.lua
```
```
return {
  {
    "stevearc/oil.nvim",
    dependencies = { "nvim-tree/nvim-web-devicons" },
    config = function()
      require("oil").setup({
        default_file_explorer = true,  -- Neovim 標準の netrw を置き換える
        view_options = {
          show_hidden = true,          -- 隠しファイルも表示
        },
        columns = {
          "icon",                      -- ファイルアイコンを表示
        },
				keymaps = {
          ["q"] = "actions.close",       -- デフォルト
          ["<Esc>"] = "actions.close",   -- ←これで ESC でも閉じられる
        },
      })
    end,
  },
}
```
neovimを開いた状態で`<Space> + o`で起動。

### telescope
ファイルサーチ（[GitHub](https://github.com/nvim-telescope/telescope.nvim)）。
```
> vi lua/plugins/telescope.lua
```
```
return {
  {
    "nvim-telescope/telescope.nvim",
    tag = "0.1.5",   -- 安定版
    dependencies = {
      "nvim-lua/plenary.nvim",
    },
    config = function()
      local telescope = require("telescope")

      telescope.setup({
        defaults = {
          layout_strategy = "horizontal",
          layout_config = {
            preview_width = 0.55,
          },
          sorting_strategy = "ascending",
        },
      })

      -- よく使うキーマッピング（任意）
      local builtin = require("telescope.builtin")
      vim.keymap.set("n", "<leader>ff", builtin.find_files, { desc = "Telescope: Find Files" })
      vim.keymap.set("n", "<leader>fg", builtin.live_grep,  { desc = "Telescope: Live Grep" })
      vim.keymap.set("n", "<leader>fb", builtin.buffers,    { desc = "Telescope: Buffers" })
      vim.keymap.set("n", "<leader>fh", builtin.help_tags,  { desc = "Telescope: Help" })
    end,
  },
}
```
telescope-fzf-native（[GitHub](https://github.com/nvim-telescope/telescope-fzf-native.nvim)）も導入しておく。
```
> vi lua/plugins/telescope-fzf.lua
```
```
return {
  {
    "nvim-telescope/telescope-fzf-native.nvim",
    build = "make",   -- fzf-native をコンパイル
    cond = vim.fn.executable("make") == 1,  -- make がある環境でのみ有効
    dependencies = { "nvim-telescope/telescope.nvim" },
    config = function()
      require("telescope").load_extension("fzf")
    end,
  },
}
```
- `Space + ff`: ファイル検索
- `Space + fg`: grep
- `Space + fb`: bufferの内容表示
- `Space + fh`: helpタグの表示

### コマンド補完機能
- nvim-config（[GitHub](https://github.com/hrsh7th/nvim-cmp)）  
コマンドの補完エンジン。
```
> vi lua/plugins/cmp.lua
```
```
return {
  {
    "hrsh7th/nvim-cmp",
    event = "InsertEnter",  -- 挿入モードに入ったとき読み込み
    dependencies = {
      "hrsh7th/cmp-nvim-lsp",   -- LSP 補完
      "hrsh7th/cmp-buffer",     -- バッファ内の単語補完
      "hrsh7th/cmp-path",       -- パス補完
      "saadparwaiz1/cmp_luasnip",
      "L3MON4D3/LuaSnip",       -- スニペットエンジン
      "rafamadriz/friendly-snippets", -- 既製スニペット集
    },
    config = function()
      local cmp = require("cmp")
      local luasnip = require("luasnip")

      -- VSCode形式のスニペットを読み込む
      require("luasnip.loaders.from_vscode").lazy_load()

      cmp.setup({
        snippet = {
          expand = function(args)
            luasnip.lsp_expand(args.body)
          end,
        },
        mapping = cmp.mapping.preset.insert({
          ["<C-Space>"] = cmp.mapping.complete(),         -- 補完開始
          ["<C-e>"]     = cmp.mapping.abort(),            -- 補完キャンセル
--          ["<CR>"]      = cmp.mapping.confirm({ select = true }), -- 決定
					-- Enter は「確定しない」
					["<CR>"] = cmp.mapping({
						 i = function(fallback)
							if cmp.visible() then
								cmp.abort()   -- ← 補完を閉じるだけ
							else
								fallback()    -- ← 普通の Enter
							end
						end,
					}),
					-- Tab で確定
			    ["<Tab>"] = cmp.mapping(function(fallback)
			      if cmp.visible() then
			        cmp.confirm({ select = false })
			      else
			        fallback()
			      end
			    end, { "i", "s" }),
          ["<C-n>"]     = cmp.mapping.select_next_item(), -- 次候補
          ["<C-p>"]     = cmp.mapping.select_prev_item(), -- 前候補
        }),
        sources = cmp.config.sources({
          { name = "nvim_lsp" },
          { name = "luasnip" },
        }, {
          { name = "buffer" },
          { name = "path" },
        }),
      })
    end,
  },
}
```

- mason（[GitHub](https://github.com/mason-org/mason.nvim)）、
  nvim-lspconfig（[GitHub](https://github.com/neovim/nvim-lspconfig)）  
LSPサーバとそのNeovimへの接続。
```
> vi lua/plugins/lsp.lua
```
```
return {
  -- LSP サーバのインストーラ
  {
    "williamboman/mason.nvim",
    config = function()
      require("mason").setup()
    end,
  },

  -- mason と標準 LSP の橋渡し
  {
    "williamboman/mason-lspconfig.nvim",
    dependencies = {
      "williamboman/mason.nvim",
      "neovim/nvim-lspconfig",  -- ここは「設定ファイル集」としてだけ使う
      "hrsh7th/cmp-nvim-lsp",
    },
    config = function()
			-- fortls の「initialization complete」だけを黙らせる
		do
		  local function is_fortls_init_complete(result, ctx)
		    local client = vim.lsp.get_client_by_id(ctx.client_id)
		    if not client or client.name ~= "fortls" then
		      return false
		    end
		    local msg = (result and (result.message or result)) or ""
		    msg = type(msg) == "string" and msg or ""
		    return msg:match("initialization complete") ~= nil
		  end
		
		  local orig_log = vim.lsp.handlers["window/logMessage"]
		  vim.lsp.handlers["window/logMessage"] = function(err, result, ctx, config)
		    if result and ctx and is_fortls_init_complete(result, ctx) then
		      return
		    end
		    return orig_log(err, result, ctx, config)
		  end
		
		  local orig_show = vim.lsp.handlers["window/showMessage"]
		  vim.lsp.handlers["window/showMessage"] = function(err, result, ctx, config)
		    if result and ctx and is_fortls_init_complete(result, ctx) then
		      return
		    end
		    return orig_show(err, result, ctx, config)
		  end
		end

      local mason_lspconfig = require("mason-lspconfig")

      mason_lspconfig.setup({
        ensure_installed = {
          "pyright",   -- Python
          -- "clangd",    -- C/C++
          "lua_ls",    -- Lua（Neovim設定用）
          "fortls",    -- Fortran
          "texlab",    -- LaTeX
        },
        automatic_installation = true,
      })

      local capabilities = require("cmp_nvim_lsp").default_capabilities()

      -- ★ 新しい書き方：vim.lsp.config + vim.lsp.enable

      -- 共通能力を全サーバに適用
      vim.lsp.config("*", {
        capabilities = capabilities,
      })

      -- 必要に応じて個別設定を上書き
      vim.lsp.config("lua_ls", {
        settings = {
          Lua = {
            diagnostics = {
              globals = { "vim" },
            },
          },
        },
      })

      vim.lsp.config("texlab", {
        settings = {
          texlab = {
            build = {
              executable = "latexmk",
              args = { "-pdf", "-interaction=nonstopmode", "-synctex=1", "%f" },
              onSave = false,
            },
          },
        },
      })
			vim.lsp.config("fortls", {
			  cmd = {
			    "fortls",
			    "--hover_signature",
			    "--hover_language=fortran",
					"--lowercase_intrinsics",
			    -- "--use_signature_help",
			  },
			  filetypes = { "fortran" },
			  root_markers = { ".fortls", ".fortlsrc", ".fortls.json", ".git" },
			})

      -- 有効にするサーバ一覧
      vim.lsp.enable({
        "pyright",
        -- "clangd",
        "lua_ls",
        "fortls",
        "texlab",
      })
    vim.lsp.enable('stylua', false)
    end,
  },
}
```

### indent-blackline（[GitHub](https://github.com/lukas-reineke/indent-blankline.nvim)）
インデントの可視化。
```
> vi lua/plugins/indent.lua
```
```
return {
  "lukas-reineke/indent-blankline.nvim",
  main = "ibl",
  opts = {
    indent = {
      char = "│",
      tab_char = "│",
    },
    scope = {
      enabled = false,
      show_start = true,
      show_end = false,
    },
    whitespace = {
      remove_blankline_trail = true,
    },
  },
}
```

### luasnip
スニペット管理（[GitHub](https://github.com/L3MON4D3/LuaSnip)）。
```
> vi lua/plugins/luasnip.lua
```
```
return {
  "L3MON4D3/LuaSnip",
  dependencies = { "saadparwaiz1/cmp_luasnip" },
  config = function()
    require("luasnip.loaders.from_lua").lazy_load({ paths = "~/.config/nvim/snippets" })
  end,
}
```

## スニペット
- LaTeX
```
> vi snippets/tex.lua
```
```
local ls = require("luasnip")
local s  = ls.snippet
local t  = ls.text_node
local i  = ls.insert_node

return {

  s({trig = "eq", priority = 2000}, {
    t("\\begin{equation}"), 
		t({ "", "  " }), 
		i(1),
    t({ "", "\\end{equation}" }),
  }),

  s({trig = "al", priority = 2000}, {
    t("\\begin{align}"), 
		t({ "", "  " }), 
		i(1),
    t({ "", "\\end{align}" }),
  }),

  s({trig = "eqal", priority = 2000}, {
    t("\\begin{equation}"), 
    t({ "", "  \\begin{aligned}" }), 
		t({ "", "    " }), 
		i(1),
    t({ "", "  \\end{aligned}" }),
    t({ "", "\\end{equation}" }),
  }),

  s({trig = "item", priority = 2000}, {
    t("\\begin{itemize}"), 
		t({ "", "  \\item " }), 
		i(1),
    t({ "", "\\end{itemize}" }),
  }),

  s({trig = "enu", priority = 2000}, {
    t("\\begin{enumerate}"), 
		t({ "", "  \\item " }), 
		i(1),
    t({ "", "\\end{enumerate}" }),
  }),

  s({trig = "fig", priority = 2000}, {
    t("\\begin{figure}[H]"), 
		t({ "", "  \\centering" }), 
		t({ "", "  \\caption{}"}), 
		i(1),
    t({ "", "\\end{figure}" }),
  }),

  s({trig = "tab", priority = 2000}, {
    t("\\begin{table}[H]"), 
		t({ "", "  \\centering" }), 
		t({ "", "  \\makeatletter" }), 
		t({ "", "  \\def\\@captype{table}" }), 
		t({ "", "  \\makeatother" }), 
		t({ "", "  \\caption{}"}), 
		t({ "", "  \\begin{tabular}{ccc}" }), 
		i(1),
		t({ "", "    \\hline\\hline" }), 
		t({ "", "    &   &   \\\\\\hline" }), 
		t({ "", "    &   &   \\\\" }), 
		t({ "", "    &   &   \\\\" }), 
		t({ "", "    \\hline\\hline" }), 
		t({ "", "  \\end{tabular}" }), 
    t({ "", "\\end{table}" }),
  }),

  s({trig = "figtab", priority = 2000}, {
    t("\\begin{figure}[H]"), 
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
		t({ "", "    \\centering" }), 
		t({ "", "    \\caption{}" }), 
		i(1),
    t({ "", "  \\end{minipage}"}), 
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
		t({ "", "    \\centering" }), 
		t({ "", "    \\makeatletter" }), 
		t({ "", "    \\def\\@captype{table}" }), 
		t({ "", "    \\makeatother" }), 
		t({ "", "    \\caption{}"}), 
		t({ "", "    \\begin{tabular}{ccc}" }), 
		t({ "", "      \\hline\\hline" }), 
		t({ "", "      &   &   \\\\" }), 
		t({ "", "      &   &   \\\\" }), 
		t({ "", "      &   &   \\\\" }), 
		t({ "", "      \\hline\\hline" }), 
		t({ "", "    \\end{tabular}" }), 
    t({ "", "  \\end{minipage}" }),
    t({ "", "\\end{figure}" }),
  }),

  s({trig = "itemtab", priority = 2000}, {
    t("\\begin{tabular}{cc}"), 
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
		t({ "", "    \\begin{itemize}" }), 
		t({ "", "				\\item " }), 
		i(1),
		t({ "", "    \\end{itemize}" }), 
    t({ "", "  \\end{minipage}"}), 
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
		t({ "", "    \\centering" }), 
		t({ "", "    \\makeatletter" }), 
		t({ "", "    \\def\\@captype{table}" }), 
		t({ "", "    \\makeatother" }), 
		t({ "", "    \\caption{}"}), 
		t({ "", "    \\begin{tabular}{ccc}" }), 
		t({ "", "      \\hline\\hline" }), 
		t({ "", "      &   &   \\\\" }), 
		t({ "", "      &   &   \\\\" }), 
		t({ "", "      &   &   \\\\" }), 
		t({ "", "      \\hline\\hline" }), 
		t({ "", "    \\end{tabular}" }), 
    t({ "", "  \\end{minipage}" }),
    t({ "", "\\end{tabular}" }),
  }),

  s({trig = "itemfig", priority = 2000}, {
    t("\\begin{figure}[H]"), 
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
    t({ "", "    \\begin{itemize}" }),
    t({ "", "      \\item " }),
    t({ "", "    \\end{itemize}" }),
    t({ "", "  \\end{minipage}" }),
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
		t({ "", "    \\centering" }), 
		t({ "", "    \\caption{}" }), 
		i(1),
    t({ "", "  \\end{minipage}"}), 
    t({ "", "\\end{figure}" }),
  }),

  s({trig = "enufig", priority = 2000}, {
    t("\\begin{figure}[H]"), 
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
    t({ "", "    \\begin{enumerate}" }),
    t({ "", "      \\item " }),
    t({ "", "    \\end{enumerate}" }),
    t({ "", "  \\end{minipage}" }),
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
		t({ "", "    \\centering" }), 
		t({ "", "    \\caption{}" }), 
		i(1),
    t({ "", "  \\end{minipage}"}), 
    t({ "", "\\end{figure}" }),
  }),

  s({trig = "vs", priority = 2000}, {
    t(""), 
		t({ "", "\\vspace{-10pt}" }), 
		t({ "", ""}), 
		i(1),
  }),

  s({trig = "col", priority = 2000}, {
    t("\\textcolor{"), 
		i(1),
    t("}{}"), 
  }),

  s({trig = "frac", priority = 2000}, {
    t("\\frac{"), 
		i(1),
    t("}{}"), 
  }),

}
```

- Fortran
```
> vi snippets/fortran.lua
```
```
local ls = require("luasnip")
local s = ls.snippet
local t = ls.text_node
local i = ls.insert_node

return {
  s("fmod", {
    t({ "module " }), i(1, "module_name"),
    t({ "", "  " }),
    t({ "", "  implicit none", "" }),
    t({ "", "contains", "" }),
    t({ "", "", "end module " }), rep(1),
  }),

  s("fsub", {
    t({ "subroutine " }), i(1, "name"), t("("), i(2, "args"), t({ ")", "" }),
    t({ "", "  implicit none", "" }),
    t({ "", "end subroutine " }), rep(1),
  }),

  s("func", {
    t({ "function " }), i(1, "name"), t("("), i(2, "args"), t({ ") "}), t({ "result()", "" }),
    t({ "", "  implicit none", "" }),
    t({ "", "end function " }), rep(1),
  }),

  s("fprog", {
    t({ "program " }), i(1, "main"),
    t({ "", "  " }),
    t({ "", "  implicit none", "" }),
    t({ "", "  " }), i(0),
    t({ "", "", "end program " }), rep(1),
  }),

  s("fdo", {
    t("do "), i(0),
    t({ "", "  " }),
    t({ "", "enddo" }),
  }),

  s("fif", {
    t("if ("), i(1, "condition"), t({ ") then", "" }),
    t("  "), i(0),
    t({ "", "endif" }),
  }),

  s("(", {
    t("(/ "), i(0), t({ " /)"}),
  }),
}
```

## latexmkrc
以下のページ参照（[latexmkrcを用いたtexの自動コンパイル](autotexcompile.md)）。




