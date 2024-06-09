---
documentclass: scrartcl
title: Multilingual writing in Markdown for Pandoc LaTeX export
author: Parker Robb
---

Writing and typesetting multilingually 
or in languages other than English 
is a complex process
because typesetting must be configured on several levels:
writing direction, script, and specific language.
As Overleaf (“Polyglossia”) puts it, 
“typography and typesetting is so much more 
than just font design and selection: 
they are very language- and culture-specific disciplines.”
When writing in a language other than English,
or when writing multilingual documents,
in Markdown using Obsidian, 
and compiling in LaTeX via Pandoc,
explicitly specifying the language of each portion of text
and the font for each language
reduces the complexity
and provides for consistent typesetting behavior and results.

# Rationale

Specifying the language of each portion of text is important 
for several reasons.

First,
LaTeX (via the Babel package) utilizes language-specific hyphenation patterns. 
If the language is explicitly specified, 
this is done correctly consistently;[^BabelAutodetect] 
if the language is not specified, 
LaTeX defaults to the hyphenation pattern 
of the document’s main language
(which itself defaults to English),
which can [result in awkward hyphenation](https://www.overleaf.com/learn/latex/Multilingual_typesetting_on_Overleaf_using_polyglossia_and_fontspec) 
when applied to other languages.

[^BabelAutodetect]: MacFarlane, 43

	Babel can be configured 
to automatically detect different scripts and text direction without markup,
and automatically switch the hyphenation patterns and associated font;
however it cannot detect and switch between languages
that use the same script.
This also requires using the Lua(La)TeX engine,
and modifying `\babelprovides` in the Pandoc LaTeX template
to use the `onchar=ids fonts` option (babel; Bezos, 31; Emiliano).
See [this GitHub issue](https://github.com/OliverBalfour/obsidian-pandoc/issues/161) for discussion.

Second, 
Obsidian only displays words in right-to-left (RTL) scripts in the correct order 
if the language or direction is explicitly specified.[^XeTeXdir]

[^XeTeXdir]: The Xe(La)TeX typesetting engine also requires 
changes in text direction to be marked
(`bidi=default` is the only bidirectional option available to Babel
when using Xe(La)TeX) (Bezos, 45; Emiliano); 
if they are not, 
Xe(La)TeX will render letters or words in bidirectional lines [in the incorrect order](https://www.overleaf.com/learn/latex/Multilingual_typesetting_on_Overleaf_using_polyglossia_and_fontspec).

	Since Obsidian already requires language markup,
the typesetting engine’s requirements are irrelevant.

A related issue is that
LaTeX’s default font families 
(Computer Modern and Latin Modern) 
do not support scripts other than Latin
(e.g. Greek, Hebrew, Cyrillic, Arabic, and the scripts of most Asian languages).[^12] 
Content in a non-Latin script must be typeset
in a font that contains the given script’s characters.[^OverleafInternational]

Associating alternate fonts with languages/scripts 
not supported by the default fonts, 
and specifying the language of text content, 
tells Obsidian the correct word order,
and tells LaTeX in which font text should be typeset
and how it should be hyphenated.

# Font support

In LaTeX, documents must be compiled 
using a font that includes the character set of any scripts used.[^OverleafInternational]
LaTeX’s default font families (Computer Modern and Latin Modern) do not support scripts other than Latin.[^12]

[^12]: Bezos, 3

## Document font

The easy option is to set the entire document font to a font
that includes glyphs for every language/script utilized in the document,
using the `fontfamily` or `mainfont`, `sansfont`, and `monofont` metadata variables.[^7]
Specify them on the command line using the `-V` Pandoc option
or in a separate options template loaded via the `-d`/`--defaults` Pandoc option.[^13]

[^13]: They can also be specified in document YAML metadata,
but the methods presented are preferred 
because they separate content from formatting.

## Per-language/script fonts

Specifying fonts for each language/script can be done
in one of several ways:
1. Give a map of Babel language names and associated fonts in Pandoc options[^7] in a separate options template loaded via the `-d`/`--defaults` Pandoc option:[^13]
```yaml
babelfonts:
 hebrew: libertinus
 polytonicgreek: libertinus
```
2. Use the `\babelfont` LaTeX command to specify fonts for each language[^6] in a separate `.tex` file loaded with the `-H`/`--include-in-header` Pandoc option.[^14] This method allows control over individual font settings, and allows associating a font with entire scripts, not just individual languages.

[^6]: Bezos, 24; Overleaf
[^7]: MacFarlane, 48
[^14]: They can also be specified in the `header-includes` metadata variable,
but this variable is overwritten by the content of any file 
specified using the `-H` option.

The `babel-header-includes.tex` file in this repository 
already includes serif (`rm`) and sans-serif (`sf`) font associations
for Hebrew and Greek scripts.[^Libertinus]
*Automatic typesetting in Greek requires Pandoc v3.2 and above
due to propagation of a [bugfix](https://github.com/jgm/pandoc/issues/9698) in that version.*

[^Libertinus]: Hebrew and Greek are set to use the Libertinus font family 
(https://github.com/alerque/libertinus; 
https://tug.org/FontCatalogue/libertinusserif/).
Libertinus was originally forked from, 
and is now the de facto continuation of, 
the [Libertine Open Fonts](https://libertine-fonts.org/) project, 
a popular and attractive open-source font family 
whose development stalled around 2012.
Libertinus fixed issues in Linux Libertine 
that had accumulated over the years, 
unified the typeface names, 
and (most importantly) added a matching mathematical typeface.
It was the original Libertine Fonts project’s Linux Libertine (serif) and Linux Biolinum (san-serif) typefaces 
that initially added support for “the western languages” 
and “Western Latin, Greek, Cyrillic (with their specific enhancements), Hebrew, IPA and many more” (“Libertine Fonts”).

# Language/script support

If no explicit specifications or modifications are made,
Pandoc and LaTeX assume all document content is in English (`en`).

Specify other languages
using IETF language tags
(following the BCP 47 standard).[^1]
See pages 20–24 of the [Babel package](https://ctan.org/pkg/babel) documentation for a full list of supported languages and locales.

[^1]: MacFarlane, 43

## Document content

### Document language

The main language of the document can be specified
using the `lang` metadata variable.[^1]

### Multilingual documents

Pandoc automatically converts divs and spans with a `lang` property 
into the appropriate Babel LaTeX commands.

Delineate content in a language other than the main document language
using HTML spans or divs:[^RawHTML]
```markdown
And God said, <span lang=he>יְהִי אֹור</span>, and there was light.


<div lang=el-polyton>
# Η γρήγορη καφέ αλεπού πήδηξε πάνω από το τεμπέλικο σκυλί

ἐν ἀρχῇ ἐποίησεν ὁ θεὸς τὸν οὐρανὸν καὶ τὴν γῆν.
ἡ δὲ γῆ ἦν ἀόρατος καὶ ἀκατασκεύαστος καὶ σκότος ἐπάνω τῆς ἀβύσσου καὶ πνεῦμα θεοῦ ἐπεφέρετο ἐπάνω τοῦ ὕδατος.

καὶ εἶπεν ὁ θεός <span lang=he>יְהִי אֹור</span> καὶ ἐγένετο φῶς.
</div>
```

[^RawHTML]: Emiliano.

	Both Markdown and Pandoc allow insertion of raw HTML (MacFarlane, 91).

This is especially important for right-to-left languages,
as Obsidian will not display them
in the correct word order
without a wrapper.[^XeTeXdir]

Pandoc spans and divs can also be used,[^8]
but unlike HTML tags
they are not hidden in Obsidian’s Live Preview:[^Emiliano]
```markdown
And God said, [יְהִי אֹור]{lang=he}, and there was light.

::: {lang=el-polyton}
# Η γρήγορη καφέ αλεπού πήδηξε πάνω από το τεμπέλικο σκυλί

ἐν ἀρχῇ ἐποίησεν ὁ θεὸς τὸν οὐρανὸν καὶ τὴν γῆν.
ἡ δὲ γῆ ἦν ἀόρατος καὶ ἀκατασκεύαστος καὶ σκότος ἐπάνω τῆς ἀβύσσου καὶ πνεῦμα θεοῦ ἐπεφέρετο ἐπάνω τοῦ ὕδατος.

καὶ εἶπεν ὁ θεός [יְהִי אֹור]{lang=he} καὶ ἐγένετο φῶς.
:::
```

[^8]: MacFarlane, 43; Dervieux

## Metadata

To specify a language different than the document language
within a YAML metadata item,
use Pandoc spans
(HTML spans do not work):
```yaml
title: "Title with [טקסט בעברית]{lang=he} inline"
```

## Zotero references

Specifying the language of bibliography titles in Zotero is important
not only for correct font selection and hyphenation,
but also correct capitalization.[^ZoteroCase]

[^ZoteroCase]: Zotero assumes titles are stored in sentence case.

	Better BibTeX for Zotero (“Markdown/Pandoc”) recommends
exporting bibliography files in CSL format,
not BibTeX format,
to preserve capitalization in a lossless manner.

### Non-English titles

Specify the language of an item’s title
by specifying the language of the item
in Zotero’s `language` field.
All titles should be in sentence case.[^9]

[^9]: MacFarlane, 111; Stillman

If the `language` field is blank,
Zotero assumes the item’s language is English (`en`),
and Pandoc will convert all titles to title case
in styles that call for English titles to be converted.[^9]

### Multilingual titles

Use Pandoc spans to mark portions of a title
that are in a language other than the item’s language:
```
Title with [טקסט בעברית]{lang=he} inline
```

Note that bibliographies created from within Zotero
will show the span marker.

# Bibliography

babel. “What’s New in Babel 3.38,” January 15, 2020. [https://latex3.github.io/babel/news/whats-new-in-babel-3.38.html](https://latex3.github.io/babel/news/whats-new-in-babel-3.38.html).

Better BibTeX for Zotero. “Markdown/Pandoc.” Accessed May 30, 2024. [https://retorque.re/zotero-better-bibtex/exporting/pandoc/index.html](https://retorque.re/zotero-better-bibtex/exporting/pandoc/index.html).

Bezos, Javier. “Babel User Guide,” April 20, 2024. [https://mirrors.rit.edu/CTAN/macros/latex/required/babel/base/babel.pdf](https://mirrors.rit.edu/CTAN/macros/latex/required/babel/base/babel.pdf).

Dervieux, Christophe. “How to Set Multiple Languages with Babel? · Quarto-Dev/Quarto-Cli · Discussion #4000.” _GitHub_, January 8, 2023. [https://github.com/quarto-dev/quarto-cli/discussions/4000#discussioncomment-4716646](https://github.com/quarto-dev/quarto-cli/discussions/4000#discussioncomment-4716646).

Emiliano. “Bidirectional Markdown Documents to PDF.” _Pandoc-Discuss_, March 9, 2022. [https://groups.google.com/g/pandoc-discuss/c/OUCwJtfwzgs](https://groups.google.com/g/pandoc-discuss/c/OUCwJtfwzgs).

[^Emiliano]: Emiliano

“Libertine Fonts,” February 4, 2018. [https://libertine-fonts.org/](https://libertine-fonts.org/).

“Linux Libertine.” In _Wikipedia_, December 18, 2023. [https://en.wikipedia.org/w/index.php?title=Linux_Libertine&oldid=1190469115](https://en.wikipedia.org/w/index.php?title=Linux_Libertine&oldid=1190469115).

MacFarlane, John. “Pandoc User’s Guide,” July 7, 2023. [https://pandoc.org/MANUAL.pdf](https://pandoc.org/MANUAL.pdf).

Maclennan, Caleb. “Libertinus.” Python, February 22, 2024. [https://github.com/alerque/libertinus](https://github.com/alerque/libertinus).

Overleaf. “International Language Support.” Accessed June 6, 2024. [https://www.overleaf.com/learn/latex/International_language_support](https://www.overleaf.com/learn/latex/International_language_support).

[^OverleafInternational]: Overleaf, “International Language Support.”

Overleaf. “Multilingual Typesetting on Overleaf Using Babel and Fontspec.” Accessed September 26, 2023. [https://www.overleaf.com/learn/latex/Multilingual_typesetting_on_Overleaf_using_babel_and_fontspec](https://www.overleaf.com/learn/latex/Multilingual_typesetting_on_Overleaf_using_babel_and_fontspec).

Overleaf. “Multilingual Typesetting on Overleaf Using Polyglossia and Fontspec.” Accessed June 6, 2024. [https://www.overleaf.com/learn/latex/Multilingual_typesetting_on_Overleaf_using_polyglossia_and_fontspec](https://www.overleaf.com/learn/latex/Multilingual_typesetting_on_Overleaf_using_polyglossia_and_fontspec).

Stillman, Dan. “Preventing Title Casing for Non-English Titles.” Zotero Documentation, January 22, 2020. [https://www.zotero.org/support/kb/preventing_title_casing_for_non-english_titles](https://www.zotero.org/support/kb/preventing_title_casing_for_non-english_titles).
