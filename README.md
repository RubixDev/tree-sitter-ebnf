# tree-sitter-ebnf

EBNF grammar for [tree-sitter](https://github.com/tree-sitter/tree-sitter)

## Archival Notice

This parser is now part of the
[`RubixDev/ebnf`](https://github.com/RubixDev/ebnf/tree/main/crates/tree-sitter-ebnf)
again.

## Reference

This parser implements the EBNF syntax as described by the
[ISO/IEC 14977:1996 standard](https://www.iso.org/standard/26153.html) with two
notable differences:

1. The ISO standard does not allow underscores `_` in meta-identifiers
2. The ISO standard only allows characters from the
   [ISO/IEC 646:1991 character set](https://www.iso.org/standard/4777.html)

## Usage in Neovim

### Parser Installation

The [nvim-treesitter plugin](https://github.com/nvim-treesitter/nvim-treesitter)
does not include this parser
[currently](https://github.com/nvim-treesitter/nvim-treesitter/pull/3574). To
use it you must instead manually add it to your tree-sitter config and then
install it with `:TSInstall ebnf` or by adding it to your `ensure_installed`
list:

```lua
require('nvim-treesitter.parsers').get_parser_configs().ebnf = {
    install_info = {
        url = 'https://github.com/RubixDev/tree-sitter-ebnf.git',
        files = { 'src/parser.c' },
        branch = 'main',
    },
}
```

### File type detection

You will likely also have to add the `ebnf` file type:

```lua
vim.filetype.add { extension = { ebnf = 'ebnf' } }
```

### Highlighting

If you want to use this parser for highlighting, you will also have to add the
contents of [`queries/highlights.scm`](./queries/highlights.scm) to a file
called `queries/ebnf/highlights.scm` in your Neovim runtime path (see
`:help rtp`). I also recommend customizing these highlights:

- `@grammar.terminal`: terminal symbols enclosed with `'` or `"`, falls back to
  `@string`
- `@grammar.special`: special sequences enclosed with `?`, falls back to
  `@string.special`
- `@grammar.nonterminal`: non-terminal symbols, i.e., identifiers, falls back to
  `@variable`
  - `@grammar.nonterminal.pascal`: non-terminal symbols in PascalCase
  - `@grammar.nonterminal.camel`: non-terminal symbols in camelCase
  - `@grammar.nonterminal.upper`: non-terminal symbols in UPPERCASE
  - `@grammar.nonterminal.lower`: non-terminal symbols in lowercase

As an example, here is my personal configuration:

```lua
vim.api.nvim_set_hl(0, '@grammar.special', { link = '@string.regex' })
vim.api.nvim_set_hl(0, '@grammar.nonterminal.pascal', { link = '@type' })
vim.api.nvim_set_hl(0, '@grammar.nonterminal.camel', { link = '@property' })
vim.api.nvim_set_hl(0, '@grammar.nonterminal.upper', { link = '@constant' })
vim.api.nvim_set_hl(0, '@grammar.nonterminal.lower', { link = '@parameter' })
```
