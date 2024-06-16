---
nocite: |
 @ObsidianFlavoredMarkdown, @macfarlanePandocUserGuide2023
---

-tx-
| | Obsidian | Pandoc |
| --- | --- | --- |
| Lists | Unordered: `- `, `* `, or `+ `; Ordered: `1. ` | (Enable `lists_without_preceding_blankline` extension for exact correspondence)
| Wikilinks | `[[name|alias]]`  | (Enable `wikilinks_title_after_pipe` extension) |
| ^^ | `[[name#heading|alias]]` | – |
| Embeds | `![[name#heading]]` | – |
| Block references | `^id` on end | – |
| Comments | Inline: `%  %`; Block: `%%  %%` | – |
| ^^ | HTML block: `<!--  -->` ||
| Strikethrough | `~~  ~~` ||
| Highlighting | `​==  ==` | (Enable `mark` extension) |
| Inline code | `` `  ` `` ||
| Code blocks | ` ```  ``` ` ||
| Task lists | Incomplete: `- [ ]`; Complete: `- [x]` ||
| Callouts | `> [!title]` | – |
| Tables | Pipe tables ||
| ^^ | – | Simple, Multiline, Grid tables |
| ^^ | – | Captions |
| Citations | (Enable [Pandoc Reference List](https://github.com/mgmeyers/obsidian-pandoc-reference-list) plugin) | `@citekey` et al. |
