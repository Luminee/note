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

#### HTML

```go
tmp, err := template.ParseFiles("xx/hi.html")
if err != nil {}
tmp.Execute(w, "Nick") // w http.ResponseWriter

// or
People := People{
    Name: "Nick",
    Age : 20,
    Male: "男"
}
tmp.Execute(w, People)
```

```html
<p>Hello {{.}}</p>
{{/* or (注释) */}}
<p>姓名 {{.Name}}</p>
<p>年龄 {{.Age}}</p>
<p>性别 {{.Friend.Title}}</p>
<!-- 函数 -->
{{.method "arg1" "arg2"}}
{{.Age|ToString}} // ToString(this.Age)
```

> Hello Nick
>
> ---
>
> 姓名 Nick
>
> 年龄 20
>
> 性别 男

**if**

```html
{{$color := "red"}}
{{if eq .Status "ok"}} {{$color = "green"}} {{end}}
{{if eq .Status "ok"}}<span class="bg-{{$color}}">{{.Num}}</span>
{{else if eq .Status "error"}} bg-{{$color}} {{.Num}}
{{end}}
```

> eq `==`
>
> ne `!=`
>
> lt `<`
>
> le `<=`
>
> gt `>`
>
> ge `>=`

**range**

```html
{{range $index, $user := .}}
<tr>
	<td>{{$index}}</td>
    <td>{{$user.Name}}</td>
    <td>{{$.Var1}}</td>  //使用外部的 Var1 变量
</tr>
{{end}}

{{range pipeline}} T1 {{else}} T0 {{end}} //没有值遍历时，执行else
```

**自定义函数**

```go
bookFunc := func(arg string) (string, error) {
    return arg + " nice", nil
}
tpl := template.New("demo.html")
tpl = tpl.Funcs(template.FuncMap{"book": bookFunc})
tpl, _ = tpl.ParseFiles("demo.html")
```

```html
<p>{{book .Name}}</p>
```

> abc nice

**模板嵌套**

```go
t, err := template.ParseFiles("./index.html", "./test.html")
```

`index.html`

```html
<body>
    {{template "test.html"}}
    {{template "insede.html"}}
</body>

{{ define "inside.html"}}
<h1>
    inside.html
</h1>
{{ end}}
```

`test.html`

```html
<h1>
    test.html
</h1>
```



