# tabline
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

