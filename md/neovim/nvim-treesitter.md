# nvim-treesitter
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

