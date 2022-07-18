# Hugo-Mathjax
Self-Build Hugo with goldmark-mathjax Extension

## Modification

same as <https://github.com/fastai/hugo-mathjax/blob/master/hugo.patch>

```sh
diff --git a/markup/goldmark/convert.go b/markup/goldmark/convert.go
index ffe9cd45..441774e7 100644
--- a/markup/goldmark/convert.go
+++ b/markup/goldmark/convert.go
@@ -31,6 +31,7 @@ import (
        "github.com/gohugoio/hugo/markup/converter"
        "github.com/gohugoio/hugo/markup/highlight"
        "github.com/gohugoio/hugo/markup/tableofcontents"
+       mathjax "github.com/litao91/goldmark-mathjax"
        "github.com/yuin/goldmark"
        hl "github.com/yuin/goldmark-highlighting"
        "github.com/yuin/goldmark/extension"
@@ -135,6 +136,10 @@ func newMarkdown(pcfg converter.ProviderConfig) goldmark.Markdown {
                extensions = append(extensions, extension.Footnote)
        }

+       if cfg.Extensions.Mathjax {
+               extensions = append(extensions, mathjax.MathJax)
+       }
+
        if cfg.Parser.AutoHeadingID {
                parserOptions = append(parserOptions, parser.WithAutoHeadingID())
        }
diff --git a/markup/goldmark/goldmark_config/config.go b/markup/goldmark/goldmark_config/config.go
index af33e03d..925e324b 100644
--- a/markup/goldmark/goldmark_config/config.go
+++ b/markup/goldmark/goldmark_config/config.go
@@ -30,6 +30,7 @@ var Default = Config{
                Strikethrough:  true,
                Linkify:        true,
                TaskList:       true,
+               Mathjax:        true,
        },
        Renderer: Renderer{
                Unsafe: false,
@@ -58,6 +59,7 @@ type Extensions struct {
        Strikethrough bool
        Linkify       bool
        TaskList      bool
+       Mathjax       bool
 }

 type Renderer struct {
```

## Build Steps

```sh
git clone https://github.com/gohugoio/hugo.git
cd hugo
#modify some files
go get github.com/litao91/goldmark-mathjax
CGO_ENABLED=1 GOOS=windows GOARCH=amd64 go build --tags extended -ldflags="-s -w"
```

## References

- <https://github.com/fastai/hugo-mathjax>
- [为 Hugo 添加 MathJax 支持（重新编译 Hugo）](https://1024th.github.io/blog/2021/04/add-mathjax-support-for-hugo/)
