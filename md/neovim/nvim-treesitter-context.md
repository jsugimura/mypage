# nvim-treesitter-context
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

