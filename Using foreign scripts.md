---
nocite: |
 @emilianoBidirectionalMarkdownDocuments2022
---

# Font support

In Pandoc and LaTeX, documents must be exported using a font that includes the character set of any script used.
Unfortunately LaTeX’s default fonts do not support scripts other than Latin.

## Libertine fonts

Linux Libertine (serif) and Linux Biolinum (san-serif) fonts cover “the western languages” and support “Western Latin, Greek, Cyrillic (with their specific enhancements), Hebrew, IPA and many more.”[@LibertineFonts2018];
they can be downloaded and installed from https://libertine-fonts.org/.

To export with the [Obsidian Pandoc](https://github.com/OliverBalfour/obsidian-pandoc) plugin configured with [[Pandoc PDF commands#Defaults|default commands]], specify the fonts in the document’s metadata:
```yaml
mainfont: Linux Libertine O
sansfont: Linux Biolinum O
```

If needed the monospaced font can also be used:
```yaml
monofont: Linux Libertine Mono O
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
