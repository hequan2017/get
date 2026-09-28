[简体中文](README.md) | [English](README.en.md)

# get

![版本](https://img.shields.io/badge/release-1.0.0-blue.svg)
![语言](https://img.shields.io/badge/language-Go-blue.svg)

> 一个只用 Go 标准库从零实现的轻量级 Web 框架，用于学习 Web 框架（路由、上下文、中间件）的底层原理。

## 📖 项目介绍

get 是一个学习型项目：在 [geektutu/7days-golang](https://github.com/geektutu/7days-golang) 的 gee 框架基础上修改完成（教程：[7天用Go从零实现Web框架](https://geektutu.com/post/gee.html)），用几百行代码实现 gin 风格的核心能力。

代码很短，`get/` 目录下全部源码不到 500 行，适合想搞清楚「Trie 树路由怎么匹配参数」「中间件链是怎么执行的」这类问题的 Go 学习者阅读和动手改造。

## ✨ 功能特性

- **Context 封装**：`Query` / `PostForm` / `Param` 取参数，`String` / `JSON` / `Data` 输出响应
- **Trie 树路由**：支持 `:name` 静态参数和 `*filepath` 通配符匹配
- **路由分组**：`RouterGroup` 支持前缀嵌套，同组路由共享中间件
- **中间件机制**：按注册顺序执行，内置 `Logger`（请求日志）和 `Recovery`（panic 恢复，返回 500）
- **静态文件服务**：`Static(relativePath, root)` 挂载静态目录

## 🛠 技术栈

- Go 1.12+
- 仅使用标准库（`net/http`、`html/template`），零第三方依赖

## 🚀 快速开始

```bash
go run main.go   # 监听 :80
```

`main.go` 是一个完整示例，覆盖了常用场景：

```go
r := get.Default() // 默认带 Logger + Recovery 中间件

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

## 📁 目录结构

```
get/            框架源码
├── get.go      Engine / RouterGroup
├── context.go  Context 封装
├── router.go   路由分发
├── trie.go     Trie 树路由匹配
├── logger.go   日志中间件
├── recovery.go panic 恢复中间件
└── router_test.go  路由单元测试
main.go         使用示例
```

## 🙏 特别感谢

- [geektutu/7days-golang](https://github.com/geektutu/7days-golang)
- [geektutu.com/post/gee.html](https://geektutu.com/post/gee.html)

## 📄 License

[MIT](LICENSE)

## 👤 作者

- 何全
