---
nocite: |
 @emilianoBidirectionalMarkdownDocuments2022, @LinuxLibertine2023, @maclennanLibertinus2024
---

# Font support

In LaTeX, documents must be compiled using a font that includes the character set of any scripts used.
Unfortunately LaTeX’s default font families (Computer Modern and Latin Modern), though beautifully-designed, do not support scripts other than Latin.
Fortunately the major TeX distributions include an alternative font family called Libertinus that can be invoked in several ways.
The [Libertinus Fonts](https://github.com/alerque/libertinus) project includes serif, sans-serif, monospaced, math, serif display (for large sizes), serif initials (outlined variants of capital letters suitable for drop caps, et al.), and keyboard font families.
All typefaces support standard OpenType Font (OTF) features.

### Scripts

The Libertinus Serif, Sans, Serif Display, and Math families support Latin, Latin Extended, IPA, diacritics, Greek and Coptic, Cyrillic, Hebrew, phonetic extensions, Greek extended, and many other symbols.
Libertinus Mono supports only Latin, Latin Extended, diacritics, and basic symbols.
The Libertinus Serif Initials and Keyboard typefaces are for special use, thus Initials only supports the most basic Latin characters, while Keyboard supports Latin, Latin Extended, Hebrew, and limited symbols.

### Weights and styles

The Libertinus Serif family includes typefaces covering three weights in each of the two standard styles (regular and italic).
Libertinus Sans includes the standard minimal set of three typefaces covering two weights and two styles (i.e., regular, bold, and italic).
Libertinus Keyboard, Serif Display, Serif Initials, Mono, and Math are individual typefaces.

### History

Libertinus was originally forked from and is now the de facto continuation of the [Libertine Open Fonts](https://libertine-fonts.org/) project, a popular and attractive open-source font family whose development stalled around 2012.
Libertinus fixed issues in Linux Libertine that had accumulated over the years, unified the typeface names, and (most importantly) added a matching mathematical typeface.
It was the original Libertine Fonts project’s Linux Libertine (serif) and Linux Biolinum (san-serif) typefaces that initially added support for “the western languages” and “Western Latin, Greek, Cyrillic (with their specific enhancements), Hebrew, IPA and many more.”[@LibertineFonts2018]

### Usage

To use the Libertinus font family specify the package in the LaTeX document header or append it using Pandoc’s `-H` option:
```latex
\usepackage{libertinus}
```

To specify the package in Markdown document YAML metadata instead, use one of the following Pandoc metadata variables. Note that this does not adhere to the principle of separation of content and formatting:
```yaml
header-includes: \usepackage{libertinus}
fontfamily: libertinus
```

# Bidirectional lines

To mark inline RTL language text so that words are ordered correctly in Obsidian and by Pandoc, use HTML spans, which are hidden in Obsidian’s Live Preview mode:
```markdown
This is some text with <span lang=he>טקסט בעברית</span> in the middle.
```

Pandoc spans can also be used, but they will always be visible in Obsidian:
```markdown
This is some text with [טקסט בעברית]{lang=he} in the middle.
```

Both of these can be done in reverse if inserting LTR text in a RTL line.

## In metadata

To mark RTL text in YAML metadata, use Pandoc spans:
```yaml
title: "Title with [טקסט בעברית]{lang=he} inline"
```
