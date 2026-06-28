# lualine
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

