[简体中文](README.md) | [English](README.en.md)

# get

![Version](https://img.shields.io/badge/release-1.0.0-blue.svg)
![Language](https://img.shields.io/badge/language-Go-blue.svg)

> A lightweight web framework built from scratch with nothing but the Go standard library, created to learn how web frameworks (routing, context, middleware) work under the hood.

## 📖 Introduction

get is a learning-oriented project: it is based on the gee framework from [geektutu/7days-golang](https://github.com/geektutu/7days-golang) (tutorial: [7 Days to Build a Web Framework in Go](https://geektutu.com/post/gee.html)) and implements gin-style core features in a few hundred lines of code.

The code is very short — everything under `get/` is less than 500 lines — making it a good read for Go learners who want to understand questions like "how does a trie router match params" or "how is a middleware chain executed".

## ✨ Features

- **Context wrapper**: read params via `Query` / `PostForm` / `Param`, write responses via `String` / `JSON` / `Data`
- **Trie router**: supports `:name` path params and `*filepath` wildcard matching
- **Route groups**: `RouterGroup` supports nested prefixes; routes in a group share its middleware
- **Middleware mechanism**: executed in registration order; built-in `Logger` (request logging) and `Recovery` (recovers from panic, returns 500)
- **Static file serving**: mount a static directory with `Static(relativePath, root)`

## 🛠 Tech Stack

- Go 1.12+
- Standard library only (`net/http`, `html/template`), zero third-party dependencies

## 🚀 Quick Start

```bash
go run main.go   # listens on :80
```

`main.go` is a complete example covering common use cases:

```go
r := get.Default() // Logger + Recovery middleware by default

r.GET("/", func(c *get.Context) {
    c.String(http.StatusOK, "Hello get \n")
})
r.GET("/get", func(c *get.Context) {
    c.JSON(http.StatusOK, get.H{"name": "get"})
})
v1 := r.Group("/v1")
v1.GET("/hello", func(c *get.Context) {
    c.String(http.StatusOK, "hello %s", c.Query("name")) // /v1/hello?name=get
})
v2 := r.Group("/v2")
v2.GET("/hello/:name", func(c *get.Context) {
    c.String(http.StatusOK, "hello %s", c.Param("name")) // /v2/hello/get
})
```

## 📁 Directory Structure

```
get/            framework source
├── get.go      Engine / RouterGroup
├── context.go  Context wrapper
├── router.go   route dispatch
├── trie.go     trie-based route matching
├── logger.go   logging middleware
├── recovery.go panic recovery middleware
└── router_test.go  router unit tests
main.go         usage example
```

## 🙏 Acknowledgements

- [geektutu/7days-golang](https://github.com/geektutu/7days-golang)
- [geektutu.com/post/gee.html](https://geektutu.com/post/gee.html)

## 📄 License

[MIT](LICENSE)

## 👤 Author

- 何全 (Hequan)
