# Hugo-Mathjax
Self-Build Hugo with goldmark-mathjax Extension

## Modification

same as <https://github.com/fastai/hugo-mathjax/blob/master/hugo.patch>, two files are mmodified

- markup/goldmark/convert.go
- markup/goldmark/goldmark_config/config.go

```sh
diff --git a/markup/goldmark/convert.go b/markup/goldmark/convert.go
index ba85831b..bce6d82e 100644
--- a/markup/goldmark/convert.go
+++ b/markup/goldmark/convert.go
@@ -25,6 +25,7 @@ import (

        "github.com/gohugoio/hugo/markup/converter"
        "github.com/gohugoio/hugo/markup/tableofcontents"
+       mathjax "github.com/litao91/goldmark-mathjax"
        "github.com/yuin/goldmark"
        "github.com/yuin/goldmark/extension"
        "github.com/yuin/goldmark/parser"
@@ -136,6 +137,10 @@ func newMarkdown(pcfg converter.ProviderConfig) goldmark.Markdown {
                extensions = append(extensions, attributes.New())
        }

+       if cfg.Extensions.Mathjax {
+               extensions = append(extensions, mathjax.MathJax)
+       }
+
        md := goldmark.New(
                goldmark.WithExtensions(
                        extensions...,
diff --git a/markup/goldmark/goldmark_config/config.go b/markup/goldmark/goldmark_config/config.go    
index a3238091..e5c4bde8 100644
--- a/markup/goldmark/goldmark_config/config.go
+++ b/markup/goldmark/goldmark_config/config.go
@@ -31,6 +31,7 @@ var Default = Config{
                Linkify:         true,
                LinkifyProtocol: "https",
                TaskList:        true,
+               Mathjax:         true,
        },
        Renderer: Renderer{
                Unsafe: false,
@@ -63,6 +64,7 @@ type Extensions struct {
        Linkify         bool
        LinkifyProtocol string
        TaskList        bool
+       Mathjax         bool
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
