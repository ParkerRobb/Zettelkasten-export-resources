---
nocite: |
 @jdhaoConvertingMarkdownBeautiful2019, @CustomizingPandocGenerate2020, @tozierPandocLaTeXWorkflow2016, @yaoBoilerplatingPandocAcademic2016, @mortenstarckLeftjustifyTextLaTeX2012, @strawbridgeCreatingPDFsJustified2014, @hgvFirstParagraphIndentation2014, @rodeoFirstlineParagraphIndenting2015, @kohmKOMAScriptGuide2023, @butterickButterickPracticalTypography2023, @macfarlanePandocUserGuide2023
---

> [!NOTE] Note
> All commands assume this Zettelkasten-export-resources repository has been cloned to `~/Zettelkasten/Export\ Resources/`, the Zotero data directory is located at `~/Zotero/`, and the Zotero database is exported to `~/Zotero.bib`.

# Defaults

Default command:
```sh
pandoc <input>.md -o <output>.pdf --from=markdown+wikilinks_title_after_pipe+mark+lists_without_preceding_blankline-blank_before_blockquote --pdf-engine=xelatex -C --bibliography=~/Zotero.bib -V papersize:letter
```

The options can be copied to “Extra Pandoc arguments” in the settings of the [Obsidian Pandoc](https://github.com/OliverBalfour/obsidian-pandoc) plugin.

Default metadata:
```yaml
documentclass: scrartcl
nocite: |
 @citekey
reference-section-title: Bibliography
```


# Markdown extensions

> [!NOTE]
> Most of these extensions were added in Pandoc v3.0.0


Option to make Pandoc interpret Obsidian Flavored Markdown correctly:
```sh
--from=markdown+wikilinks_title_after_pipe+mark+lists_without_preceding_blankline-blank_before_blockquote
```

Obsidian Flavored Markdown mostly consists of GitHub Flavored Markdown (GFM), which itself is an extension of CommonMark.
`markdown` refers to Pandoc’s implementation of Markdown.
See [[Markdown extension comparison]] for a full analysis of where Obsidian Flavored Markdown and Pandoc Markdown overlap and diverge.

Generic option:
```sh
--from=markdown+<ext1>+<ext2>+...
```

- Wikilinks: `wikilinks_title_after_pipe`
- Highlights: `mark`
- Lists without preceding blank line: `lists_without_preceding_blankline`
- Block quote without preceding blank line: `-blank_before_blockquote`
	- Extension requires blank line before block quotes, and is enabled by default.



# Output formatting

## Document class

Option:
```sh
-V documentclass:scrartcl
```
Metadata:
```yaml
documentclass: scrartcl
```

Standard classes:
- `article`
- `book`: Formats in spreads with appropriate blank pages, title page
- `report`: Like `article`, but separate title page and Bibliography page
- `memoir`: Indistinguishable from `book` to me
- `letter`
Default `fontsize` is 10pt.

KOMA-script classes:
- `scrartcl`
- `scrbook`
- `scrreprt`: Starts each H1 on new page
- `scrlttr2`

Global differences between the KOMA-script classes and the standard classes include:
- Sans-serif font for headers
- H4-6 are set on own line and not run-in
- **A4 default `papersize`**
- `11pt` default `fontsize`
- Type Area aspect ratio set to same proportions as the page[^1]

[^1]: This is desired. On portrait-oriented pages, if the margins are equal on all sides, the text area becomes increasingly elongated with larger margins, and its thinness emphasizes the size of the side margins. This visual disproportion is solved by placing additional margin at the bottom. This practice was followed by medieval book designers.

The most legible font size is 10-11pt.
Both standard and KOMA-script classes optimize typography for ~80 characters per line and calculate the margins from the `fontsize`;
if different margins are desired, they should be adjusted by changing the `fontsize`.
The table below gives the margins resulting from a given `fontsize` for KOMA-script classes on letter-size paper.
The margins that result from 11pt font work better with A4 paper because A4 is the default `papersize`.

| `fontsize`     | Side margins |
| -------------- | ------- |
| 11pt (default) | 1.83”   |
| 12pt           | 1.6”    |
| 13pt           | 1.3”    |
| 14pt           | 0.82”   | 

### Custom LaTeX template

Option:
```sh
--template=<filepath>.tex
```

## Fonts

The default LaTeX font is Computer Modern.
KOMA-script document classes use the sans-serif typeface for headings.

Preferred settings for Georgia:

Option:
```sh
-V mainfont:"Georgia" -V linestretch:1.15
```
Metadata:
```yaml
mainfont: "Georgia"
linestretch: 1.1
```


## H1 page breaks in standard classes


> [!NOTE] Note
> This is only needed for standard document classes, as certain KOMA-script classes include heading page breaks in their templates.

> [!WARNING] Warning
> The `titlesec` package does not work with KOMA-script classes

Option:
```sh
-H ~/Zettelkasten/Export\ Resources/LaTeX\ commands/H1-page-break.tex
```

## Indentation

### First line

Indentation only needed if double spacing or if no space added between paragraphs.

Option:
```sh
-H ~/Zettelkasten/Export\ Resources/LaTeX\ commands/indent.tex
```

> [!WARNING]
> Has no effect when combined with [[Pandoc PDF commands#Left-align text:|ragged2e]].
> See [[Pandoc PDF commands#Left-align + indent paragraphs|Left-align + indent]] if left-alignment is also desired.

#### First line + Left-aligned

Option:
```sh
-H ~/Zettelkasten/Export\ Resources/LaTeX\ commands/indent+left-align.tex
```

### Hanging

Option:
```sh
-H ~/Zettelkasten/Export\ Resources/LaTeX\ commands/hanging-indent.tex
```

> [!Warning]
> This adds extra indentation to any [[Pandoc PDF commands#Citations|generated reference section]].
> To reset the reference section indentation, `\setlength{\leftskip}{0em}` must be added to the end of the document.

^ef6732

> [!WARNING]
> Has no effect when combined with [[Pandoc PDF commands#Left-align text:|ragged2e]].
> See [[Pandoc PDF commands#Hanging + Left-aligned|Hanging + left-aligned]] if left-alignment is also desired.

#### Hanging + Left-aligned

Option:
```sh
-H ~/Zettelkasten/Export\ Resources/LaTeX\ commands/hanging-indent+left-align.tex
```

![[Pandoc PDF commands#^ef6732]]

### First paragraph of section

> [!Note]
> Use with intention.
> Typographically, indentation of the first paragraph of a section is unnecessary.

By default, LaTeX does not apply indentation rules to the first paragraph of a section (i.e. following a section heading).

Option:
```sh
-H ~/Zettelkasten/Export\ Resources/LaTeX\ commands/indent-first-paragraph.tex
```

## MLA style

Complete command:
```sh
pandoc <input>.md -o <output>.pdf --pdf-engine=xelatex -C --bibliography=~/Zotero.bib --csl=~/Zotero/styles/modern-language-association.csl -V papersize:letter -V geometry:margin=1in -V linestretch:2 -H ~/Zettelkasten/Export\ Resources/LaTeX\ commands/disable-hyphenation.tex -H ~/Zettelkasten/Export\ Resources/LaTeX\ commands/indent+left-align.tex
```

### Margins

> [!Note]
> Using the `geometry` package overrides the typographic calculations described in [[Pandoc PDF commands#Document class|Document class]].
> Only use it as a nuclear option.

Option:
```sh
-V geometry:margin=1in
```

### Line spacing

Option:
```sh
-V linestretch:2
```
Metadata:
```yaml
linestretch: 2
```

### Disable hyphenation

> [!Note]
> Use with intention.
> TeX is known for its unsurpassed hyphenation algorithm.

Option:
```sh
-H ~/Zettelkasten/Export\ Resources/LaTeX\ commands/disable-hyphenation.tex
```

### Left-align text

> [!Note]
> Use with intention.
> TeX is known for its beautiful text justification algorithms.

The `ragged2e` package “re-enables” hyphenation in aligned/centered text and keeps the text from looking ridiculously ragged, unlike the default LaTeX commands and environments.

`ragged2e` adds its own skip and indentation parameters.
If the package is used, LaTeX code that changes the default skip and indentation parameters must be adjusted to use `ragged2e`‘s parameters to have any effect (which is the reason for several “Left-align + …” sections in this document).

Option:
```sh
-H ~/Zettelkasten/Export\ Resources/LaTeX\ commands/left-align.tex
```

![[Pandoc PDF commands#First line + Left-aligned]]

# Citations

Option:
```sh
-C
```

## Bibliography

Option:
```sh
 --bibliography=~/Zotero.bib
```
Metadata:
```yaml
bibliography: ~/Zotero.bib
```

## Citation style

Option:
```sh
--csl=<filepath>.csl
```
Metadata:
```yaml
csl: <filepath>.csl
```

Filepath can also be a URL.

Default is Chicago author-date
- Chicago notes-bibliography:
	- `~/Zotero/styles/chicago-note-bibliography.csl`
	- `~/Zotero/styles/chicago-fullnote-bibliography.csl`
- MLA: `~/Zotero/styles/modern-language-association.csl`
- APA: `~/Zotero/styles/apa.csl`


# Additional variables

```yaml
block-headings: true
classoption:
- landscape
- twocolumn
indent: true
pagestyle:
sansfont: 
monofont:
mathfont:
mainfontoptions:
twocolumn: true
lof: true
lot: true
thanks: 
toc: true
toc-depth:
```

# Additional options


# Available metadata

For LaTeX and PDF output

```yaml
title:
subtitle:
author:
date:
keywords: 
subject: 
lang: en-US
dir: rtl
csl:
reference-section-title:
nocite:
documentclass:
```

