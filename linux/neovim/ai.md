# ai

- 配置 ollama
```shell
nvim ~/.config/nvim/lua/plugins/init.lua
{
  "olimorris/codecompanion.nvim",
  dependencies = {
    "nvim-lua/plenary.nvim",
    "nvim-treesitter/nvim-treesitter",
  },
  event = { "BufRead" },
  config = function()
    require("codecompanion").setup {
      strategies = {
        chat = {
          adapter = "ollama",
        },
        inline = {
          adapter = "ollama",
        },
      },
      debug = true,
    }
  end,
},

nvim ~/.config/nvim/lua/plugins/init.lua
{
  "hrsh7th/nvim-cmp",
  dependencies = { "codecompanion.nvim" },
  opts = {
    sources = {
      { name = "codecompanion" },  -- 添加 Ollama 补全源
      { name = "nvim_lsp" },
    },
  },
}


<!--集成到补全引擎-->
nvim ~/.config/nvim/lua/plugins/init.lua
{
  "hrsh7th/nvim-cmp",
  dependencies = { "codecompanion.nvim" },
  opts = {
    sources = {
      { name = "codecompanion" },  -- 添加 Ollama 补全源
      { name = "nvim_lsp" },
    },
  },
}


<!--触发代码生成-->
nvim ~/.config/nvim/lua/keymaps.lua
vim.keymap.set("n", "<leader>cc", function()
  require("codecompanion").generate_code({
    prompt = "Write a Python function to calculate factorial", -- 自定义提示词
    model = "codellama:7b-code", 
  })
end, { desc = "Generate code with Ollama" })


<!--代码实时补全-->
require("codecompanion").setup({
  trigger_characters = { ".", "(", "=" }, -- 输入这些字符时触发建议
  debounce_ms = 300,                      -- 防抖延迟（毫秒）
})


<!--多语言切换-->
require("codecompanion").setup({
  language_config = {
    python = {
      model = "codellama:7b-code",
      parameters = { temperature = 0.5 },
    },
    javascript = {
      model = "llama2:13b",
      parameters = { temperature = 0.8 },
    },
  },
})


<!--开启日志-->
<!--nvim ~/.local/state/nvim/codecompanion.log-->
-- 启用 CodeCompanion 调试日志
require("codecompanion").setup({
  debug = true,
})
```
