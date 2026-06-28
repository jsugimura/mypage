# indent-blackline
インデントの可視化（[GitHub](https://github.com/lukas-reineke/indent-blankline.nvim)）。
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

