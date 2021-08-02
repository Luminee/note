#### Static Files

```go
app := iris.New()
app.Favicon("./resources/favicon.ico")
app.HandleDir("/assets", iris.Dir("./assets"))

// Force Send a file to client
ctx.SendFile("./files/first.zip", "client.zip")

src := "./files/big.zip"
dest := "" 
// 限制 ~50Kb 突发 100Kb
limit := 50.0 * iris.KB
burst := 100 * iris.KB
ctx.SendFileWithRate(src, dest, limit, burst)

ctx.ServeFile("./public/main.js")
```

#### Template Render

| Name   | Parse                                              |
| ------ | -------------------------------------------------- |
| HTML   | [html/template](https://pkg.go.dev/html/template)  |
| Django | [flosch/pongo2](https://github.com/flosch/pongo2)  |
| Jet    | [ CloudyKit/jet](https://github.com/CloudyKit/jet) |

```go
tmpl := iris.HTML("./views", ".html")
app.RegisterView(tmpl)

ctx.ViewData("message", "Hello world!") // Bind data
// {{.message}} usage in html

ctx.View("hi.html") // Render view

// add new template function
tmpl.AddFunc("greet", func(s string) string {
    return "Greetings " + s + "!"
})

tmpl.Reload(true) // reload on every request

adminGroup := app.Party("/admin")
adminGroup.RegisterView(iris.Blocks("./views/admin", ".html"))
```

