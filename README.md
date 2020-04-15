# base16-chroma

[Base 16](http://chriskempson.com/projects/base16/) templates for [Chroma](https://github.com/alecthomas/chroma)
and static sites builder [Hugo](https://gohugo.io/content-management/syntax-highlighting/) (which uses Chroma for highlight).

## Getting started

### use with Hugo

First you'll need to [configure Hugo's Chroma syntax highlighter](https://gohugo.io/content-management/syntax-highlighting/#configure-syntax-highlighter) to add classes to the generated HTML for your site.

```toml
# config.toml
[markup.highlight]
  noClasses = false
```

Once classes are enabled you'll need download or copy one of the pre-built `hugo-base16-css/*.css` files and incorporate it into your site. This is an ideal use for [Hugo Pipes](https://gohugo.io/hugo-pipes/), as you can use the [SASS/SCSS](https://gohugo.io/hugo-pipes/scss-sass/) pipe to incorporate and minify the styling CSS for you. Once you've got a pipeline working trying out different Base16 themes is as simple as overwriting a file in the **assets** directory and letting `hugo server` do the rest.

The template has been designed to generate the CSS in the same form that [Hugo can generate](https://gohugo.io/content-management/syntax-highlighting/#generate-syntax-highlighter-css), so modifiying the CSS for your own needs should be fairly easy.


### use with Chroma

take `goldmark-highlighting` extension with `chroma html formatter` for example

you need config `chroma html formatter` with `WithClasses(true)`

```go
import (
	"bytes"

	chromahtml "github.com/alecthomas/chroma/formatters/html"
	"github.com/yuin/goldmark"
	highlighting "github.com/yuin/goldmark-highlighting"
	"github.com/yuin/goldmark/extension"
	"github.com/yuin/goldmark/parser"
	"github.com/yuin/goldmark/renderer/html"
)

// FormatText converts text with markdown processor, applies external converters and shortens links
func mdToHtmlWithHighlight(txt string) (res string) {
	markdown := goldmark.New(
		goldmark.WithExtensions(extension.GFM), // Linkify | Table | Strikethrough | TaskList
		goldmark.WithParserOptions(
			parser.WithAutoHeadingID(),
		),
		goldmark.WithRendererOptions(
			html.WithHardWraps(),
		),
		goldmark.WithExtensions(
			highlighting.NewHighlighting(
				highlighting.WithFormatOptions(
					chromahtml.TabWidth(2),
					chromahtml.WithClasses(true),
				),
			),
		),
		goldmark.WithExtensions(extension.Typographer), // substitutes punctuations with typographic entities like smartypants
	)

	var output bytes.Buffer
	if err := markdown.Convert([]byte(txt), &output); err != nil {
		panic(err)
	}
	res = output.String()
    return res
}
```

## Thanks to

Since Chroma uses the same syntax as Pygments, this is basically just a transposition of the work done by [mohd-akram](https://github.com/mohd-akram) on the [pygments template](https://github.com/mohd-akram/base16-pygments); all thanks (and blame for any color decisions that deviate from expectation) where it's due.

And, of course, deep thanks to [chriskempson](https://github.com/chriskempson) for [base16](https://github.com/chriskempson/base16).

most of the job is done by [Michael Morehouse](https://github.com/yawpitch/base16-hugo), I just think the name should be `chroma`, not hugo