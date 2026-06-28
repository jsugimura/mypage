# telescope
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

