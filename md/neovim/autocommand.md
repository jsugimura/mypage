# コマンド補完機能
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

