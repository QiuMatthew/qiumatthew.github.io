---
title: Neovim Configuration - Zero to your Personalized Modern IDE
date: 2024-08-16
tags:
  - engineering
  - vim
---

## What is Neovim

I won't talk too much about this since the most important thing is get started. As far as I understand, it is still vim basically, but neovim allows you to configure it with Lua (a programming language that is super easy to learn if you don't know) instead of Vim script. 

## Configuration Location

`~/.config/nvim` if you are using Linux/MacOS. If the folder does not exist, create one.

## Configuration File Structure

You can have your own preferred file structure, but mine looks like this to make me feel organized.

```
.
├── init.lua
└── lua
    ├── core
    └── plugins
```

`init.lua` is the entry point.

```lua
-- Core configurations
require("core.options") -- line number, indentation, etc
require("core.keymaps") -- core keymaps, excluding plugins
require("core.autocommands") -- user configured autocommands
require("core.misc") -- other settings, language

-- Plugins
require("plugins.lazy") -- use lazy to manage all plugins
```

Let's do these configs one by one.

## Core Configurations

Create files in `lua/core` folder, filename should match `init.lua`. 

> For options, we create `lua/core/options.lua`.

### Options

```lua
-- line number
vim.opt.number = true
vim.opt.relativenumber= true

-- indentation
vim.opt.tabstop = 4
vim.opt.shiftwidth = 4
vim.opt.expandtab = true
vim.opt.autoindent = true

-- allow contents exceed the window border
vim.opt.wrap = false

-- highlight cursor line
vim.opt.cursorline = true

-- enable mouse operations, e.g. resizing splits
vim.opt.mouse = "a"

-- enable vim to use system clipboard
vim.opt.clipboard = "unnamedplus"

-- color
vim.opt.termguicolors = true

-- one more column on the left, reserve for debug, etc.
vim.opt.signcolumn = "yes"

-- do not show mode, since it is already in status bar
vim.opt.showmode = false

-- save undo history
vim.opt.undofile = true

-- position of new splits
vim.opt.splitright = true
vim.opt.splitbelow = true
```

### Keymaps

```lua
vim.g.mapleader = " "

-- visual mode
vim.keymap.set("v", "J", ":m '>+1<CR>gv=gv") -- move selected line downward
vim.keymap.set("v", "K", ":m '<-2<CR>gv=gv") -- move selected line upward

-- normal mode
vim.keymap.set("n", "<leader>sv", "<C-w>v") -- split window vertically
vim.keymap.set("n", "<leader>sh", "<C-w>s") -- split window horizontally

-- diagnostics
vim.keymap.set("n", "[d", vim.diagnostic.goto_prev, { desc = "go to previous [D]iagnostic message" })
vim.keymap.set("n", "]d", vim.diagnostic.goto_next, { desc = "go to next [D]iagnostic message" })
```

### Misc

```lua
vim.cmd("language en_US")
```

### Autocommands

Autocommands gives us great flexibility to nicely configure our neovim just as the way we want. For example, if I'm writing an independent C++ program and I need to test the output along the way, I have to stop writing, and type compile command in terminal, and run the executable. This entire procedure can be automated using autocommand. Another useful case for me is when I code in Python or Go, I want to run tests every time I save the file and quickly find out which test failed. And this can also be solved by autocommand.

But this is rather large topic, check [[Neovim Autocommands]] for more details.

## Plugins

Similar to VSCode, Neovim community is really active and you can almost find a plugin for whatever you need.

### Lazy

We use [lazy.nvim](https://github.com/folke/lazy.nvim) All the plugins is listed in `lua/plugins/lazy.lua`

At the moment, my `lazy.lua` file looks like this

```lua
-- install lazy if not exist
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not (vim.uv or vim.loop).fs_stat(lazypath) then
	vim.fn.system({
		"git",
		"clone",
		"--filter=blob:none",
		"https://github.com/folke/lazy.nvim.git",
		"--branch=stable", -- latest stable release
		lazypath,
	})
end
vim.opt.rtp:prepend(lazypath)

local function load_plugins()
	local plugins = {}
	vim.list_extend(plugins, require("plugins.theme"))
	vim.list_extend(plugins, require("plugins.todo-comments"))
	vim.list_extend(plugins, require("plugins.neo-tree"))
	vim.list_extend(plugins, require("plugins.comment"))
	vim.list_extend(plugins, require("plugins.gitsigns"))
	vim.list_extend(plugins, require("plugins.tmux-navigation"))
	vim.list_extend(plugins, require("plugins.which-key"))
	vim.list_extend(plugins, require("plugins.treesitter"))
	vim.list_extend(plugins, require("plugins.telescope"))
	vim.list_extend(plugins, require("plugins.lsp"))
	vim.list_extend(plugins, require("plugins.cmp"))
	vim.list_extend(plugins, require("plugins.conform"))
	vim.list_extend(plugins, require("plugins.leetcode"))
	vim.list_extend(plugins, require("plugins.autopairs"))
	vim.list_extend(plugins, require("plugins.obsidian"))
	vim.list_extend(plugins, require("plugins.competitest"))
	return plugins
end

-- install plugins
require("lazy").setup(load_plugins())
```

We can see there are many plugins including `theme`, `telescope` etc. Let's look at them one by one.

> The installation and configuration for each plugin is included in a separate file.
> For example, `~/.config/nvim/lua/plugins/theme.lua` is for theme plugins.

### Theme

There are several options for theme plugin, you can search for it on the internet or GitHub. The one used by [LazyVim](https://www.lazyvim.org/) is [tokyonight.nvim](https://github.com/folke/tokyonight.nvim). But I personally like [catppuccin](https://github.com/catppuccin/nvim). And you can also edit color palette.

```lua
return {
	-- {
	-- 	"folke/tokyonight.nvim",
	-- 	lazy = false, -- load this plugin immediately when neovim starts
	-- 	priority = 1000, -- start early since other plugins might depend on it
	-- 	init = function() -- run this function when the plugin is loaded
	-- 		vim.cmd([[colorscheme tokyonight-night]])
	-- 	end,
	-- },
	{
		"catppuccin/nvim",
		name = "catppuccin",
		priority = 1000,
		init = function()
			vim.cmd.colorscheme("catppuccin-mocha")
		end,
		config = function()
			require("catppuccin").setup({
				transparent_background = false,
				integrations = {
					telescope = true,
					harpoon = true,
					mason = true,
					neotest = true,
				},
				color_overrides = {
					mocha = {
						rosewater = "#ffc9c9",
						flamingo = "#ff9f9a",
						pink = "#ffa9c9",
						mauve = "#df95cf",
						lavender = "#a990c9",
						red = "#ff6960",
						maroon = "#f98080",
						peach = "#f9905f",
						yellow = "#f9bd69",
						green = "#b0d080",
						teal = "#a0dfa0",
						sky = "#a0d0c0",
						sapphire = "#95b9d0",
						blue = "#89a0e0",
						text = "#e0d0b0",
						subtext1 = "#d5c4a1",
						subtext0 = "#bdae93",
						overlay2 = "#928374",
						overlay1 = "#7c6f64",
						overlay0 = "#665c54",
						surface2 = "#504844",
						surface1 = "#3a3634",
						surface0 = "#252525",
						base = "#151515",
						mantle = "#0e0e0e",
						crust = "#080808",
					},
				},
			})
		end,
	},
}
```

### Special Comments

I use [todo-comments.nvim](https://github.com/folke/todo-comments.nvim) to highlight some special comments.

```lua
return {
	-- Highlight todo, notes, etc in comments
	{
		"folke/todo-comments.nvim",
		event = "VimEnter",
		dependencies = { "nvim-lua/plenary.nvim" },
		opts = { signs = false },
	},
}
```

### File Tree

Like we have in many editors, a file tree is frequently used (although I prefer search for files in telescope, another plugin). The plugin here I use is [nvim-neo-tree](https://github.com/nvim-neo-tree/neo-tree.nvim).

```lua
return {
	-- file tree
	{
		"nvim-neo-tree/neo-tree.nvim",
		branch = "v3.x",
		dependencies = {
			"nvim-lua/plenary.nvim",
			"nvim-tree/nvim-web-devicons",
			"MunifTanjim/nui.nvim",
		},
		configure = {
			vim.keymap.set("n", "<leader>e", ":Neotree reveal=true toggle=true position=left <CR>"),
		},
	},
}
```

> Leader key is defined as `<SPACE>` in `~/.config/nvim/lua/core/keymaps.lua`.

Note that there is a keybinding for `<SPACE>e`. The function is to open the left side file tree and move cursor onto current file in the file tree window. And if it is opened already, close it.

You can set a different keybinding logic that meets your need, just check readme for `nvim-neo-tree`.

### Navigation Between Windows

We often use split windows in vim. For example, the file tree window is also a split window. 

> If you don't know what is window, check [[Tab, Window and Buffer in Vim]].

And we might need to navigate between our code window and file tree window with mouse. But we are using vim and vimmers don't use mouse. So we need to navigate between windows with keyboard and to do this, try [nvim-tmux-navigation](https://github.com/alexghergh/nvim-tmux-navigation).

```lua
return {
	-- navigate between splits
	{
		"alexghergh/nvim-tmux-navigation",
		config = function()
			local nvim_tmux_nav = require("nvim-tmux-navigation")
			nvim_tmux_nav.setup({
				disable_when_zoomed = true,
			})
			vim.keymap.set("n", "<C-h>", nvim_tmux_nav.NvimTmuxNavigateLeft)
			vim.keymap.set("n", "<C-j>", nvim_tmux_nav.NvimTmuxNavigateDown)
			vim.keymap.set("n", "<C-k>", nvim_tmux_nav.NvimTmuxNavigateUp)
			vim.keymap.set("n", "<C-l>", nvim_tmux_nav.NvimTmuxNavigateRight)
			vim.keymap.set("n", "<C-\\>", nvim_tmux_nav.NvimTmuxNavigateLastActive)
		end,
	},
}
```

This allows us to use `<CTRL-h/j/k/l>` to navigate between windows.

### Comment and Uncomment

This one is simple, use [Comment.nvim](https://github.com/numToStr/Comment.nvim) to get shortcuts for commenting and uncommenting lines.

```lua
return {
	-- "gc" to comment/uncomment visual lines
	{ "numToStr/Comment.nvim", opts = {} },
}
```

To comment/uncomment:
- For single line, `gcc`
- For multiple lines, select the lines in visual mode
	- `gc` to apply single line comment for each line
	- `gb` to apply block comment

Take C/C++ as example language.

```c++
return 0;
```

In normal mode, `gcc` will turn this line into

```c++
// return 0;
```

For multiple lines

```c++
i  = 0x5f3759df - ( i >> 1 );
y  = * ( float * ) &i;
```

select these two lines in visual mode, `gc` will give us

```c++
// i  = 0x5f3759df - ( i >> 1 );
// y  = * ( float * ) &i;
```

while `gb` gives

```c++
/* i  = 0x5f3759df - ( i >> 1 );
y  = * ( float * ) &i; */
```

### Git Signs

When our code is in a git repository, we would like to see an indicator telling us which line is added or removed or modified. And [gitsigns.nvim](https://github.com/lewis6991/gitsigns.nvim) is designed for this use case.

```lua
return {
	-- Adds git related signs to the gutter, as well as utilities for managing changes
	{
		"lewis6991/gitsigns.nvim",
		opts = {
			signs = {
				add = { text = "+" },
				change = { text = "~" },
				delete = { text = "_" },
				topdelete = { text = "‾" },
				changedelete = { text = "~" },
			},
		},
	},
}
```

And leaving the `opts` blank will use default settings. And to see blame for current line, run vim command `:Gitsigns toggle_current_line_blame`. And you will see who last edited the current line.

See more detail on their GitHub page.

### Which-key

[which-key.nvim](https://github.com/folke/which-key.nvim) is helpful when you are not yet familiar with your keybindings. It shows possible follow-up keys in a popup.

```lua
return {
	-- Useful plugin to show you pending keybinds.
	{
		"folke/which-key.nvim",
		event = "VimEnter", -- Sets the loading event to 'VimEnter'
		config = function() -- This is the function that runs, AFTER loading
			require("which-key").setup()

			-- Document existing key chains
			require("which-key").add({
				{ "<leader>c", group = "[C]ode" },
				{ "<leader>c_", hidden = true },
				{ "<leader>d", group = "[D]ocument" },
				{ "<leader>d_", hidden = true },
				{ "<leader>h", group = "Git [H]unk" },
				{ "<leader>h_", hidden = true },
				{ "<leader>r", group = "[R]ename" },
				{ "<leader>r_", hidden = true },
				{ "<leader>s", group = "[S]earch" },
				{ "<leader>s_", hidden = true },
				{ "<leader>t", group = "[T]oggle" },
				{ "<leader>t_", hidden = true },
				{ "<leader>w", group = "[W]orkspace" },
				{ "<leader>w_", hidden = true },
			})
			-- visual mode
			require("which-key").add({
				{ "<leader>h", desc = "Git [H]unk", mode = "v" },
			}, { mode = "v" })
		end,
	},
}
```

### Treesitter for Syntax Highlighting

We need different color and highlights for variables, functions and many others as we get for free in many modern IDEs. In neovim, we will use [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter) for syntax highlighting.

```lua
return {
	-- treesitter for syntax highlight
	{
		"nvim-treesitter/nvim-treesitter",
		build = ":TSUpdate",
		config = function()
			require("nvim-treesitter.configs").setup({
				ensure_installed = { "vim", "bash", "c", "cpp", "javascript", "json", "lua", "python" },
				auto_install = true,

				highlight = { enable = true },
				indent = { enable = true },
			})
		end,
	},
}
```

> You may be seeing some warning from diagnostics for missing some fields like `sync_install` or `ignore_install`. It's okay to ignore that warning and the plugin will work just fine.

### Telescope

Telescope is probably the most frequently used plugin. It's a fuzzy finder that helps me find files and content inside files.

I just copied this part from [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim). Or maybe I did some minor modifications.

```lua
return {
	-- Fuzzy Finder (files, lsp, etc)
	{
		"nvim-telescope/telescope.nvim",
		event = "VimEnter",
		branch = "0.1.x",
		dependencies = {
			"nvim-lua/plenary.nvim",
			{
				"nvim-telescope/telescope-fzf-native.nvim",
				build = "make",

				-- `cond` is a condition used to determine whether this plugin should be installed and loaded.
				cond = function()
					return vim.fn.executable("make") == 1
				end,
			},
			{ "nvim-telescope/telescope-ui-select.nvim" },
			{ "nvim-tree/nvim-web-devicons", enabled = vim.g.have_nerd_font },
		},
		config = function()
			require("telescope").setup({
				-- You can put your default mappings / updates / etc. in here
				--  All the info you're looking for is in `:help telescope.setup()`
				--
				-- defaults = {
				--   mappings = {
				--     i = { ['<c-enter>'] = 'to_fuzzy_refine' },
				--   },
				-- },
				-- pickers = {}
				defaults = {
					-- https://github.com/catppuccin/nvim/discussions/323?sort=top#discussioncomment-8653291
					sorting_strategy = "ascending",
					layout_strategy = "flex",
					layout_config = {
						horizontal = { preview_cutoff = 80, preview_width = 0.55 },
						vertical = { mirror = true, preview_cutoff = 25 },
						prompt_position = "top",
						width = 0.87,
						height = 0.80,
					},
				},
				extensions = {
					["ui-select"] = {
						require("telescope.themes").get_dropdown(),
					},
				},
			})

			-- Enable Telescope extensions if they are installed
			pcall(require("telescope").load_extension, "fzf")
			pcall(require("telescope").load_extension, "ui-select")

			-- See `:help telescope.builtin`
			local builtin = require("telescope.builtin")
			vim.keymap.set("n", "<leader>fh", builtin.help_tags, { desc = "[F]ind [H]elp" })
			vim.keymap.set("n", "<leader>ff", builtin.find_files, { desc = "[F]ind [F]iles" })
			vim.keymap.set("n", "<leader>fw", builtin.grep_string, { desc = "[F]ind current [W]ord" })
			vim.keymap.set("n", "<leader>fp", builtin.live_grep, { desc = "[F]ind in [P]roject" })
			vim.keymap.set("n", "<leader>fd", builtin.diagnostics, { desc = "[F]ind [D]iagnostics" })
			vim.keymap.set("n", "<leader>fr", builtin.resume, { desc = "[F]ind [R]esume" })
			vim.keymap.set("n", "<leader>f.", builtin.oldfiles, { desc = '[F]ind Recent Files ("." for repeat)' })
			vim.keymap.set("n", "<leader>fb", builtin.buffers, { desc = "[F]ind existing buffers" })

			-- Slightly advanced example of overriding default behavior and theme
			vim.keymap.set("n", "<leader>/", function()
				-- You can pass additional configuration to Telescope to change the theme, layout, etc.
				builtin.current_buffer_fuzzy_find(require("telescope.themes").get_dropdown({
					winblend = 10,
					previewer = false,
				}))
			end, { desc = "[/] Fuzzily search in current buffer" })

			vim.keymap.set("n", "<leader>fc", function()
				-- You can pass additional configuration to Telescope to change the theme, layout, etc.
				builtin.current_buffer_fuzzy_find()
			end, { desc = "[F]ind in [C]urrent buffer" })

			-- It's also possible to pass additional configuration options.
			--  See `:help telescope.builtin.live_grep()` for information about particular keys
			vim.keymap.set("n", "<leader>f/", function()
				builtin.live_grep({
					grep_open_files = true,
					prompt_title = "Live Grep in Open Files",
				})
			end, { desc = "[F]ind [/] in Open Files" })

			-- Shortcut for searching your Neovim configuration files
			vim.keymap.set("n", "<leader>fn", function()
				builtin.find_files({ cwd = vim.fn.stdpath("config") })
			end, { desc = "[F]ind [N]eovim files" })
		end,
	},
}
```

Usage:
- `<leader>ff` to find files. Note that you will only find files inside the folder where you opened vim.
- `<leader>fn` to find neovim config files, it search inside `~/.config/nvim`
- `<leader>fc` to grep things in current buffer
- `<leader>fr` to resume previous process
- etc.

### Language Server Protocol (LSP)

LSP is also considered as syntax analyzer. Now you may ask: what is the difference between LSP and Treesitter?

You can check [this question](https://www.reddit.com/r/neovim/comments/1109wgr/treesitter_vs_lsp_differences_ans_overlap/).

Simply speaking, LSP parses your code much more deeper than treesitter. For example

```c++
int foo = bar()
```

Treesitter knows that foo is an int variable and bar is a function. This is enough to have a nice highlighting of your code. But does the function `bar` exist? Maybe it is defined in another header file. Or does the function `bar` returns an integer? 

LSP knows the context for current position, therefore it can also provide information for auto-completion. We'll cover that in next plugin.

Except for auto-completion, the most frequently used feature provided by LSP are `Go to definition/references` and `Hover documentation`.

I map `gd` to `Go to definition` and `gr` as `Go to references`. If there are multiple references (most of the cases, yes there are), it will open a telescope floating window displaying all candidates.

Also, `K` is used to peek for document. Type `K` again to enter the peek window.

Finally, to manage LSP servers, we use [mason.nvim](https://github.com/williamboman/mason.nvim).

Here is the file content of `~/.config/nvim/lua/plugins/lsp.lua`

```lua
return {
	-- lsp
	{
		"neovim/nvim-lspconfig",
		dependencies = {
			"williamboman/mason.nvim",
			"williamboman/mason-lspconfig.nvim",
			"WhoIsSethDaniel/mason-tool-installer.nvim",
			{ "j-hui/fidget.nvim", opts = {} },
			{ "folke/neodev.nvim", opts = {} },
		},
		config = function()
			vim.api.nvim_create_autocmd("LspAttach", {
				group = vim.api.nvim_create_augroup("kickstart-lsp-attach", { clear = true }),
				callback = function(event)
					local map = function(keys, func, desc)
						vim.keymap.set("n", keys, func, { buffer = event.buf, desc = "LSP: " .. desc })
					end
					-- Jump to the definition of the word under your cursor.
					--  This is where a variable was first declared, or where a function is defined, etc.
					--  To jump back, press <C-t>.
					map("gd", require("telescope.builtin").lsp_definitions, "[G]oto [D]efinition")

					-- Find references for the word under your cursor.
					map("gr", require("telescope.builtin").lsp_references, "[G]oto [R]eferences")

					-- Jump to the implementation of the word under your cursor.
					--  Useful when your language has ways of declaring types without an actual implementation.
					map("gI", require("telescope.builtin").lsp_implementations, "[G]oto [I]mplementation")

					-- Jump to the type of the word under your cursor.
					--  Useful when you're not sure what type a variable is and you want to see
					--  the definition of its *type*, not where it was *defined*.
					map("<leader>D", require("telescope.builtin").lsp_type_definitions, "Type [D]efinition")

					-- Fuzzy find all the symbols in your current document.
					--  Symbols are things like variables, functions, types, etc.
					map("<leader>ds", require("telescope.builtin").lsp_document_symbols, "[D]ocument [S]ymbols")

					-- Fuzzy find all the symbols in your current workspace.
					--  Similar to document symbols, except searches over your entire project.
					map(
						"<leader>ws",
						require("telescope.builtin").lsp_dynamic_workspace_symbols,
						"[W]orkspace [S]ymbols"
					)

					-- Rename the variable under your cursor.
					--  Most Language Servers support renaming across files, etc.
					map("<leader>rn", vim.lsp.buf.rename, "[R]e[n]ame")

					-- Execute a code action, usually your cursor needs to be on top of an error
					-- or a suggestion from your LSP for this to activate.
					map("<leader>ca", vim.lsp.buf.code_action, "[C]ode [A]ction")

					-- Opens a popup that displays documentation about the word under your cursor
					--  See `:help K` for why this keymap.
					map("K", vim.lsp.buf.hover, "Hover Documentation")

					-- WARN: This is not Goto Definition, this is Goto Declaration.
					--  For example, in C this would take you to the header.
					map("gD", vim.lsp.buf.declaration, "[G]oto [D]eclaration")

					local client = vim.lsp.get_client_by_id(event.data.client_id)
					if client and client.server_capabilities.documentHighlightProvider then
						local highlight_augroup =
							vim.api.nvim_create_augroup("kickstart-lsp-highlight", { clear = false })
						vim.api.nvim_create_autocmd({ "CursorHold", "CursorHoldI" }, {
							buffer = event.buf,
							group = highlight_augroup,
							callback = vim.lsp.buf.document_highlight,
						})

						vim.api.nvim_create_autocmd({ "CursorMoved", "CursorMovedI" }, {
							buffer = event.buf,
							group = highlight_augroup,
							callback = vim.lsp.buf.clear_references,
						})
					end
				end,
			})

			local capabilities = vim.lsp.protocol.make_client_capabilities()
			capabilities = vim.tbl_deep_extend("force", capabilities, require("cmp_nvim_lsp").default_capabilities())
			local servers = {
				clangd = {},
				gopls = {},
				pyright = {},
				-- rust_analyzer = {},
				-- ... etc. See `:help lspconfig-all` for a list of all the pre-configured LSPs
				--
				-- Some languages (like typescript) have entire language plugins that can be useful:
				--    https://github.com/pmizio/typescript-tools.nvim
				--
				-- But for many setups, the LSP (`tsserver`) will work just fine
				tsserver = {},
				--

				lua_ls = {
					-- cmd = {...},
					-- filetypes = { ...},
					-- capabilities = {},
					settings = {
						Lua = {
							completion = {
								callSnippet = "Replace",
							},
							-- You can toggle below to ignore Lua_LS's noisy `missing-fields` warnings
							-- diagnostics = { disable = { 'missing-fields' } },
						},
					},
				},
			}
			require("mason").setup()
			local ensure_installed = vim.tbl_keys(servers or {})
			vim.list_extend(ensure_installed, {
				"stylua", -- Used to format Lua code
			})
			require("mason-tool-installer").setup({ ensure_installed = ensure_installed })

			require("mason-lspconfig").setup({
				handlers = {
					function(server_name)
						local server = servers[server_name] or {}
						-- This handles overriding only values explicitly passed
						-- by the server configuration above. Useful when disabling
						-- certain features of an LSP (for example, turning off formatting for tsserver)
						server.capabilities = vim.tbl_deep_extend("force", {}, capabilities, server.capabilities or {})
						require("lspconfig")[server_name].setup(server)
					end,
				},
			})
		end,
	},
}
```

### Auto-completion

There are several options for auto-complete plugins, I use [nvim-cmp](https://github.com/hrsh7th/nvim-cmp). 

I use `<Ctrl-n>` and `<Ctrl-p>` for next and previous completion candidate and `<Ctrl-y>` for confirm autocompleting with the selected candidate. You can configure to use `<TAB>`, `<Shift-TAB>`and `<Enter>` if you like. 

```lua
return {
	-- autocomplete
	{
		"hrsh7th/nvim-cmp",
		event = "InsertEnter",
		dependencies = {
			-- Snippet Engine & its associated nvim-cmp source
			{
				"L3MON4D3/LuaSnip",
				build = "make install_jsregexp",
				dependencies = {
					-- `friendly-snippets` contains a variety of premade snippets.
					--    See the README about individual language/framework/plugin snippets:
					--    https://github.com/rafamadriz/friendly-snippets
					{
						"rafamadriz/friendly-snippets",
						config = function()
							require("luasnip.loaders.from_vscode").lazy_load()
						end,
					},
				},
			},
			"saadparwaiz1/cmp_luasnip",

			-- Adds other completion capabilities.
			--  nvim-cmp does not ship with all sources by default. They are split
			--  into multiple repos for maintenance purposes.
			"hrsh7th/cmp-nvim-lsp",
			"hrsh7th/cmp-path",
		},
		config = function()
			local cmp = require("cmp")
			local luasnip = require("luasnip")
			luasnip.config.setup({})

			cmp.setup({
				snippet = {
					expand = function(args)
						luasnip.lsp_expand(args.body)
					end,
				},
				completion = { completeopt = "menu,menuone,noinsert" },

				-- For an understanding of why these mappings were
				-- chosen, you will need to read `:help ins-completion`
				--
				-- No, but seriously. Please read `:help ins-completion`, it is really good!
				mapping = cmp.mapping.preset.insert({
					-- Select the [n]ext item
					["<C-n>"] = cmp.mapping.select_next_item(),
					-- Select the [p]revious item
					["<C-p>"] = cmp.mapping.select_prev_item(),

					-- Scroll the documentation window [b]ack / [f]orward
					["<C-b>"] = cmp.mapping.scroll_docs(-4),
					["<C-f>"] = cmp.mapping.scroll_docs(4),

					-- Accept ([y]es) the completion.
					--  This will auto-import if your LSP supports it.
					--  This will expand snippets if the LSP sent a snippet.
					["<C-y>"] = cmp.mapping.confirm({ select = true }),

					-- If you prefer more traditional completion keymaps,
					-- you can uncomment the following lines
					--['<CR>'] = cmp.mapping.confirm { select = true },
					--['<Tab>'] = cmp.mapping.select_next_item(),
					--['<S-Tab>'] = cmp.mapping.select_prev_item(),

					-- Manually trigger a completion from nvim-cmp.
					--  Generally you don't need this, because nvim-cmp will display
					--  completions whenever it has completion options available.
					["<C-Space>"] = cmp.mapping.complete({}),

					-- Think of <c-l> as moving to the right of your snippet expansion.
					--  So if you have a snippet that's like:
					--  function $name($args)
					--    $body
					--  end
					--
					-- <c-l> will move you to the right of each of the expansion locations.
					-- <c-h> is similar, except moving you backwards.
					["<C-l>"] = cmp.mapping(function()
						if luasnip.expand_or_locally_jumpable() then
							luasnip.expand_or_jump()
						end
					end, { "i", "s" }),
					["<C-h>"] = cmp.mapping(function()
						if luasnip.locally_jumpable(-1) then
							luasnip.jump(-1)
						end
					end, { "i", "s" }),

					-- For more advanced Luasnip keymaps (e.g. selecting choice nodes, expansion) see:
					--    https://github.com/L3MON4D3/LuaSnip?tab=readme-ov-file#keymaps
				}),
				sources = {
					{ name = "nvim_lsp" },
					{ name = "luasnip" },
					{ name = "path" },
				},
			})
		end,
	},
}
```

### Autoformat

In many languages, autoformat is pretty useful, especially when we copy paste code from another source and the format can be a mess. Also one important usage for me is that it helps me to format imports in the convention of my organization.

We use the plugin [conform.nvim](https://github.com/stevearc/conform.nvim) here.

```lua
return {
	-- autoformat
	{
		"stevearc/conform.nvim",
		lazy = false,
		keys = {
			{
				"<leader>f",
				function()
					require("conform").format({ async = true, lsp_fallback = true })
				end,
				mode = "",
				desc = "[F]ormat buffer",
			},
		},
		opts = {
			notify_on_error = false,
			format_on_save = function(bufnr)
				local disable_filetypes = { c = true, cpp = true }
				-- local disable_filetypes = {}
				return {
					timeout_ms = 500,
					lsp_fallback = not disable_filetypes[vim.bo[bufnr].filetype],
				}
			end,
			formatters_by_ft = {
				lua = { "stylua" },
				-- cpp = { "clang-format" },
				go = { "goimports-reviser" },
			},
			-- formatters = {
			-- 	clang_format = {
			-- 		env = {
			-- 			style = "{UseTab: Always, IndentWidth: 4, TabWidth: 4}",
			-- 		},
			-- 	},
			-- },
		},
	},
}
```

### Auto Pair 

When coding, most of the cases we'll expect our editor help us close a parenthesis, bracket, brace, quotation mark, etc.

Here we use [nvim-autopairs](https://github.com/windwp/nvim-autopairs).

```lua
return {
	{
		"windwp/nvim-autopairs",
		event = "InsertEnter",
		config = true,
		-- use opts = {} for passing setup options
		-- this is equalent to setup({}) function
	},
}
```

### LeetCode

This is a fancy plugin that let you solve LeetCode problems using your most familiar editor, neovim.

To access the problems, we need to login with session token.

I'd love it more if it supports problem sets (I haven't finished the `Top Interview 150` problem set yet).

```lua
return {
	-- Leetcode
	{
		"kawre/leetcode.nvim",
		build = ":TSUpdate html",
		dependencies = {
			"nvim-telescope/telescope.nvim",
			"nvim-lua/plenary.nvim", -- required by telescope
			"MunifTanjim/nui.nvim",

			-- optional
			"nvim-treesitter/nvim-treesitter",
			"rcarriga/nvim-notify",
			"nvim-tree/nvim-web-devicons",
		},
		opts = {
			-- configuration goes here
			cn = { -- leetcode.cn
				enabled = false, ---@type boolean
				translator = true, ---@type boolean
				translate_problems = true, ---@type boolean
			},
		},
	},
}
```

### Competitive Programming

[CompetiTest.nvim](https://github.com/xeluxee/competitest.nvim)] is a helper for competitive programming. It supports CodeForce and AtCoder, maybe more but IDK.

It should be used in combination with the browser extension [Competitive Companion](https://github.com/jmerle/competitive-companion).

Mostly used commands:
- `:CompetiTest receive problem`
- `:CompetiTest receive testcases`
- `:CompetiTest receive contest`
- `:CompetiTest run`

The plugin helps me download the test cases, compile and run the test cases and display the result in a neat UI.

Also, I can create a template for each language and it will help me initialize code files for each problem, but we need to tell the plugin where is our template file in configuration.

```lua
return {
	{
		"xeluxee/competitest.nvim",
		dependencies = "MunifTanjim/nui.nvim",
		config = function()
			require("competitest").setup({
				template_file = {
					cpp = "~/Projects/CompetitiveProgramming/Templates/template.cpp",
					py = "~/Projects/CompetitiveProgramming/Templates/template.py",
				},
			})
		end,
	},
}
```

TO BE CONTINUED ...
