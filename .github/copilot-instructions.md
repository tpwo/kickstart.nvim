# Copilot Instructions for Kickstart.nvim

## Overview

This repository is **kickstart.nvim** — a minimal, educational Neovim configuration starter project. It is **not a distribution** but a teaching tool. The primary config is a single `init.lua` file (1268 lines) with optional plugin configurations in `lua/kickstart/plugins/`. Users can extend it via `lua/custom/plugins/`.

## Code Structure

- **`init.lua`**: Main configuration file containing settings, keymaps, autocommands, and lazy.nvim setup
- **`lua/kickstart/plugins/`**: Optional plugins that can be enabled by uncommenting in init.lua
  - `gitsigns.lua` — Git change indicators and navigation (currently enabled)
  - `debug.lua`, `indent_line.lua`, `lint.lua`, `autopairs.lua`, `neo-tree.lua` — Optional plugins
- **`lua/custom/plugins/init.lua`**: User's custom plugins go here
- **`lazy-lock.json`**: Locked dependency versions for lazy.nvim
- **`.stylua.toml`**: Lua formatting config

## Formatting & Linting

**Format check runs on PRs:**
```bash
stylua --check .
```

**To format locally:**
```bash
stylua .
```

Configuration in `.stylua.toml`:
- Column width: 160
- Indentation: 2 spaces
- Quote style: Auto-prefer single quotes
- Line endings: Unix

## Key Conventions

### Plugin Structure
- Plugins are defined as Lazy spec tables in `lua/kickstart/plugins/*.lua`
- Each plugin file returns a table conforming to lazy.nvim spec
- Plugins use `---@module` and `---@type` annotations for LSP clarity (vim.bo, Gitsigns.Config, etc.)
- Opts can include `on_attach` callbacks, keymaps, and module-specific config

### Configuration Patterns

**Autocommands** (defined via `vim.api.nvim_create_autocmd`):
- FileType-specific settings (e.g., 2-space tabs for JS/TS, tabs for Makefiles)
- Markdown/asciidoc: `linebreak = true` for word wrapping
- Text yank highlight on `TextYankPost`

**Options** (set via `vim.o` or `vim.opt`):
- Prefixed with comments referencing `:help` topics
- Structured in logical sections (numbers, mouse, searching, splits, etc.)
- Special handling: `vim.schedule()` for clipboard to avoid startup slowdown

**Keymaps** (set via `vim.keymap.set`):
- Leader key: `<space>`
- Documented with `desc` field for which-key
- Grouped by functionality (split navigation, search behavior, clipboard, etc.)

### Lua Guidelines

- Use `require()` for module imports
- LSP annotations with `---@module` and `---@type` for type hints
- Comments explaining "why" rather than "what"
- Extensive `:help` references for users learning Neovim

## Important Details

- **Neovim version**: Targets latest stable and nightly releases only
- **Leader key**: `<space>` (configured early in init.lua before plugins)
- **Single-file philosophy**: init.lua is intentionally not split into modules for educational clarity; users can modularize via `lua/custom/`
- **Tab size**: 4 spaces default; 2 spaces for JS/TS; tabs for Makefile/Justfile
- **Clipboard sync**: Disabled by default; use `<leader>y` to copy to system clipboard
- **Spell check**: Enabled for `en_us,pl` (English US and Polish)

## Plugin Management

**lazy.nvim** handles all plugins:
- View status: `:Lazy`
- Update: `:Lazy update`
- Check docs: `:help lazy.nvim.txt` or https://github.com/folke/lazy.nvim

**Currently included core plugins** (see init.lua ~line 400-1200):
- LSP: nvim-lspconfig, mason.nvim, mason-lspconfig.nvim
- Completion: blink.cmp, LuaSnip
- Treesitter: nvim-treesitter (auto-install parsers)
- UI: telescope.nvim, which-key.nvim, mini.nvim, todo-comments.nvim
- Formatting: conform.nvim
- Git: gitsigns.nvim (only recommended config; base in init.lua)
- Misc: fidget.nvim (LSP progress), undotree, vim-tmux-navigator

## When Making Changes

- **Modifying init.lua**: Keep comments clear and reference `:help` topics; add inline context for non-obvious settings
- **Adding plugins**: Create files in `lua/kickstart/plugins/` following the lazy.nvim spec; add LuaDoc annotations
- **For users**: Encourage modularizing via `lua/custom/plugins/` rather than editing init.lua directly
- **PR template**: Includes reminder to check base repository on forks; no additional CI beyond stylua
