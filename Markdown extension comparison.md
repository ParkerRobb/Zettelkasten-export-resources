---
nocite: |
 @ObsidianFlavoredMarkdown, @macfarlanePandocUserGuide2023
---

-tx-
| | Obsidian | Pandoc |
| --- | --- | --- |
| Lists | Unordered: `-`, `*`, or `+`; Ordered: `1. ` | (Enable `lists_without_preceding_blankline` extension for exact correspondence)
| Wikilinks | `[[name#section|alias]]`  | (Enable `wikilinks_title_after_pipe` extension); Processes links to whole files only |
| Wikilink embeds | `![[name#section]]` | (Handled by [Obsidian Pandoc](https://github.com/OliverBalfour/obsidian-pandoc) plugin) |
| Block references | `^id` on end | |
| Comments | Inline: `% %`; Block: `%% %%` | |
| Strikethrough | `~~ ~~` ||
| Highlighting | `== ==` | (Enable `mark` extension) |
| Code blocks | ` ``` ``` ` ||
| Task lists | Incomplete: `- [ ]`; Complete: `- [x]` ||
| Callouts | `> [!title]` | |
| Tables | Pipe tables | Simple, Multiline, Grid, or Pipe tables; Captions |
| Citations | (Enable [Pandoc Reference List](https://github.com/mgmeyers/obsidian-pandoc-reference-list) plugin) | `@citekey` et al. |
