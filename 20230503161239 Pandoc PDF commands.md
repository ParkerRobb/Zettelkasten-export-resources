---
nocite: |
 @jdhaoConvertingMarkdownBeautiful2019, @CustomizingPandocGenerate2020, @tozierPandocLaTeXWorkflow2016, @yaoBoilerplatingPandocAcademic2016, @mortenstarckLeftjustifyTextLaTeX2012, @strawbridgeCreatingPDFsJustified2014, @hgvFirstParagraphIndentation2014, @rodeoFirstlineParagraphIndenting2015, @kohmKOMAScriptGuide2023, @butterickButterickPracticalTypography2023, @macfarlanePandocUserGuide2023
---

# Defaults

Default command:
```terminal
pandoc INPUT.md -o OUTPUT.pdf --pdf-engine=xelatex -C --bibliography=/Users/Parker/Zotero.bib -V papersize:letter
```

Default metadata:
```
documentclass: scrartcl
nocite: |
 @citekey
```


# Markdown extensions


> [!NOTE]
> These extensions were added in Pandoc v3.0.0


Command:
```terminal
--from=markdown+ext1+ext2+ext3
```

Wikilinks: `wikilinks_title_after_pipe`

Highlights: `mark`



# Output formatting

## Document class

```terminal
-V documentclass:scrartcl
```
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

The most legible font size is 10-11pt. Both standard and KOMA-script classes optimize typography for ~80 characters per line and calculate the margins from the `fontsize`; if different margins are desired, they should be adjusted by changing the `fontsize`. The table below gives the margins resulting from a given `fontsize` for KOMA-script classes on letter-size paper. The margins that result from 11pt font work better with A4 paper because A4 is the default `papersize`.

| `fontsize`     | Side margins |
| -------------- | ------- |
| 11pt (default) | 1.83”   |
| 12pt           | 1.6”    |
| 13pt           | 1.3”    |
| 14pt           | 0.82”   | 

### Custom LaTeX template

```terminal
--template=filepath.tex
```

## Fonts

The default LaTeX font is Computer Modern. KOMA-script document classes use the sans-serif typeface for headings.

Georgia preferred settings:
```terminal
-V mainfont:"Georgia" -V linestretch:1.15
```
```yaml
mainfont: "Georgia"
linestretch: 1.1
```


## H1 page breaks in standard classes


> [!NOTE] Note
> This is only needed for standard document classes, as certain KOMA-script classes include heading page breaks in their templates.

> [!WARNING] Warning
> The titlesec package does not work with KOMA-script classes


```terminal
-H /Users/Parker/Zettelkasten/4\ Export\ Resources/H1PageBreak.tex
```

Contents:
```latex
\usepackage{titlesec}
\newcommand{\sectionbreak}{\clearpage}
```


## Paragraph indentation

Indentation only needed if double spacing or if no space added between paragraphs.

```terminal
-H /Users/Parker/Zettelkasten/4\ Export\ Resources/IndentParagraph.tex
```

Contents:
```latex
% Pick one of the options:

\usepackage{indentfirst}

% \setlength{\parindent}{4em}
% \setlength{\parskip}{0em}
```

> [!WARNING]
> Neither of these options is compatible with [[20230503161239 Pandoc PDF commands#Left-align text:|ragged2e]]



## MLA style

Complete command:

```terminal
pandoc INPUT.md -o OUTPUT.pdf --pdf-engine=xelatex -C --bibliography=/Users/Parker/Zettelkasten/Zotero.bib --csl=/Users/Parker/Zotero/styles/modern-language-association.csl -V papersize:letter -V geometry:margin=1in -V linestretch:2 -H /Users/Parker/Zettelkasten/4\ Export\ Resources/DisableHyphenation.tex -H /Users/Parker/Zettelkasten/4\ Export\ Resources/LeftAlignAndIndent.tex
```

### Margins

> [!Warning]
> Using the `geometry` package overrides the typographic calculations described in [[20230503161239 Pandoc PDF commands#Document class|Document class]]. Only use it as a nuclear option.

```terminal
-V geometry:margin=1in
```


### Line spacing
```terminal
-V linestretch:2
```
```yaml
linestretch: 2
```


### Disable hyphenation

> [!Note]
> Use with intention. TeX is known for its good hyphenation algorithm.

```terminal
-H /Users/Parker/Zettelkasten/4\ Export\ Resources/DisableHyphenation.tex
```

Contents:
```latex
\exhyphenpenalty=10000 \hyphenpenalty=10000
```


### Left-align text

> [!Note]
> Use with intention. TeX is known for its beautiful text justification algorithms.

```terminal
-H /Users/Parker/Zettelkasten/4\ Export\ Resources/LeftAlign.tex
```

Contents:
```latex
\usepackage[document]{ragged2e}
```

#### Left-align AND indent paragraphs
```terminal
-H /Users/Parker/Zettelkasten/4\ Export\ Resources/LeftAlignAndIndent.tex
```

Contents:
```latex
\usepackage[document]{ragged2e}

\setlength{\RaggedRightParindent}{4em}

\setlength{\parskip}{0em}
```



# Citations

Citation process:
```terminal
-C --bibliography=/Users/Parker/Zettelkasten/Zotero.bib
```
```yaml
bibliography: filepath.bib
```

## Citation style

Default is Chicago author-date

Chicago notes-bibliography:
```terminal
--csl=/Users/Parker/Zotero/styles/chicago-note-bibliography.csl
--csl=/Users/Parker/Zotero/styles/chicago-fullnote-bibliography.csl
```


MLA:
```terminal
--csl=/Users/Parker/Zotero/styles/modern-language-association.csl
```

APA:
```terminal
--csl=Users/Parker/Zotero/styles/apa.csl
```

Using YAML:
```yaml
csl: filepath.csl
```

Filepath can also be a URL.

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

