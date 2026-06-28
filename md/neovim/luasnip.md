# luasnip
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

