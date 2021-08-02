#### Api

```go
func main() {
    // Default iris:
    // Default "debug" Logger.
    // AccessLog on "./access.log",
    // Recovery & Request ID middleware
    app := iris.Default()

    app.Get("/someGet", getting)
    app.Post("/somePost", posting)
    app.Put("/somePut", putting)
    app.Delete("/someDelete", deleting)
    app.Patch("/somePatch", patching)
    app.Header("/someHead", head)
    app.Options("/someOptions", options)

    app.Listen(":8080")
}
```

#### Params In Path

```go
// match /user/abc
app.Get("/user/{name}", func(ctx iris.Context) {
    name := ctx.Params().Get("name")
    ctx.Writef("Hello %s", name)
    
    ctx.GetCurrentRoute().Tmpl().Src == "/user/{name}" // true
})

// match /user/abc/ or /user/abc/send | lower than /user/{name}
app.Get("/user/{name}/{action:path}", func(ctx iris.Context) {
    name := ctx.Params().Get("name")
    action := ctx.Params().Get("action")
    message := name + " is " + action
    ctx.WriteString(message)
})

// --------

// Get & Get default
firstname := ctx.URLParamDefault("firstname", "Guest")
lastname := ctx.URLParam("lastname")

// Post & Post default
message := ctx.PostValue("message")
nick := ctx.PostValueDefault("nick", "aa")
ctx.JSON(iris.Map{
    "status":  "posted",
    "message": message,
    "nick":    nick,
})
```

> POST  /post?**id=a&id=b&id=c**&**name=john&name=doe&name=kataras**

```go
ids := ctx.URLParamSlice("id")
names, err := ctx.PostValues("name")
```

**Upload File**

```go
const maxSize = 8 * iris.MB

ctx.SetMaxRequestBodySize(maxSize)
// OR app.Use(iris.LimitRequestBodySize(maxSize))
// OR iris.WithPostMaxMemory(maxSize)

file, fileHeader, err:= ctx.FormFile("file")
dest := filepath.Join("./uploads", fileHeader.Filename)
ctx.SaveFormFile(file, dest)

// Upload Files
files, n, err := ctx.UploadFormFiles("./uploads")
```

#### Cookie

```go
ctx.GetCookie("my_cookie") // Get cookie
ctx.SetCookieKV("my_cookie", value) // Set cookie
ctx.SetCookieKV(name, value, iris.CookiePath("/path/to/store"))
ctx.SetCookie("", "test", 3600, "/", "localhost", false, true)
```

#### JWT

```go
signer := jwt.NewSigner(jwt.HS256, sigKey, 10*time.Minute)
verifier := jwt.NewVerifier(jwt.HS256, sigKey)
```

