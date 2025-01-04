# MarkdownViewer
MarkdownViewer is a no-nonsense minimal markdown rendering server. It comes with a couple custom markdown extensions. It is the successor to my previous project called [MarkWiki](https://github.com/Kraballa/MarkWiki) which attempted to do more but became kind of a mess because of it. MarkdownViewer has a reduced scope. Instead of the terrible `python-markdown` package it also uses the modern python implementation of `markdown-it`.

## Usage
MarkdownViewer uses python. It requires the following packages:
- flask (this is the server)
- markdown-it-py (this renders the markdown)
- markupsafe (handles some security considerations)
- latex2mathml (renders tex amsmath to HTML5 MathML)
install them using pip like any other package.

Put all your markdown in a folder called `text` and link to it from `index.md`. Don't include `text` in the markdown link. `(/page.md)` reads file `text/page.md`, only `(/index.md)` goes to `./index.md`.

## Markdown Features
MarkdownViewer uses the sane CommonMark-based `markdown-it-py` package and therefore follows that standart. Additionally things such as hard linebreaks, tables, strikethrough and html inside md are allowed. The server also uses the footnote, frontmatter and deflist plugins. Ontop of that I've added some custom extensions:

| idea             | HTML result              | markdown syntax             |
| :--------------- | :----------------------- | :-------------------------- |
| superscript      | `<sup>` tag              | `^{superscripted text}`     |
| subscript        | `<sub>` tag              | `_{subscripted text}`       |
| ruby annotation  | `<ruby>` tag and friends | `{some text \| annotation}` |
| amsmath notation | `<math>` (google MathML) | `$ *amsmath notation* $`    |

## some implementation details and thoughts
There are various proposed syntaxes for superscript and subscript. I've seen `^superscript^` and `~subscript~` in the original javascript reference implementation of markdown-it. They haven't been ported to python and I find that syntax ugly and easily conflicting. So instead I chose what is natural to me, a masters graduate who has written plenty of documents in LaTeX. 

There already is a plugin for amsmath in markdown-it-py. It renders the same math syntax but uses KaTeX in its final output which is essentially a mix of css and javascript libraries that get referenced in the header of your html output and do a great job at rendering it. It is a good plugin since KaTeX can work independently and enforce its own web and browser standarts. However math notation is now standart with every modern browser in `MathML` so it seems nonsensical to me having to pull javascript libraries clientside just to render what is essentially just funky text. This is why I wrote this extension as an alternative.

I don't actually know of a use case for ruby notation in western text. It just looks funny and there's something appealing about allowing really spicy syntax for when you just want to do something weird.