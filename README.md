# Rename Tmux Window to File Opened by Neovim

Here is all of the configuration necessary to make it happen (you can paste
this straight into your ~/.config/nvim/init.lua file or import it from your lua
folder):

```lua
if vim.env.TMUX ~= nil then
  -- Define an autocommand for BufEnter and FocusGained events
  vim.api.nvim_create_autocmd({"BufEnter", "FocusGained"}, {
    pattern = "*",
    callback = function()
      local current_file = vim.fn.expand("%:t")
      local tmux_command = string.format("tmux rename-window '📄 nvim - %s'", current_file)
      vim.fn.system(tmux_command)
    end
  })

  -- Define an autocommand for the VimLeave event
  vim.api.nvim_create_autocmd("VimLeave", {
    pattern = "*",
    callback = function()
      local shell = vim.env.SHELL or "sh"
      vim.fn.system("tmux set-window-option automatic-rename on")
    end
  })
end
```
