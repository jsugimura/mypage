# カラースキーム
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

