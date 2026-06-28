# oil
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

