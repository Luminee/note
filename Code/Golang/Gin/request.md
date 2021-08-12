#### Request

**Form 绑定/表单**

```go
type LoginForm struct {
    User     string `form:"user" binding:"required"`
    Password string `form:"password" binding:"required"`
}

var form LoginForm
if c.ShouldBind(&form) == nil {
    if form.User == "admin" && form.Password == "123" {
        c.JSON(200, gin.H{
            "status": "logged in"
        })
    }
}
// if c.ShouldBindQuery(&form) == nil // 仅绑定query !post
if eA := c.ShouldBindBodyWith(&objA, binding.JSON); eA == nil
else if eB := c.ShouldBindBodyWith(&objB, binding.XML);

// c.Bind(&b)

// Form
msg := c.PostForm("message")
name := c.DefaultPostForm("name", "nobody")
```

**自定义验证器** ??

**Query & Post**

```shell
POST /post?id=1234&page=1 HTTP/1.1
Content-Type: application/x-www-form-urlencoded

name=manu&message=this_is_great
```

```go
id := c.Query("id")
page := c.DefaultQuery("page", "0")
name := c.PostForm("name")

// /post?ids[]=12&ids[]=34
// names[]=aa&names[]=bb
ids := c.QueryMap("ids")
names := c.PostFormMap("names")

// 单文件上传
file, _ := c.FormFile("file")
c.String(http.StatusOK, file.Filename)
//多文件
form, _ := c.MultipartForm()
files := form.File["upload[]"]
```

**BasicAuth** _middleware_

```go
var secrets = gin.H{
    "foo": gin.H{"email": "f@b.com", "phone": "123"},
    "len": gin.H{"email": "l@a.com", "phone": "456"},
}

// main()
r := gin.Default()
authorized := r.Group("/admin", gin.BasicAuth(gin.Accounts{
    "foo": "bar",
    "len": "nop",
    "mun": "qut",
}))
// /admin/secrets
authorized.GET("/secrets", func(c *gin.Context) {
    user := c.MustGet(gin.AuthUserKey).(string)
    if secret, ok := secrets[user]; ok {
        c.JSON(http.StatusOK, gin.H{
            "user": user,
            "secret": secret,
        })
    }
})

// custom middleware
func Logger() gin.HandlerFunc {
    return func(c *gin.Context) {
        c.Set("example", "123")
        
        c.Next()
        
        status := c.Writer.Status()
    }
}
r.Use(Logger())
```

**Http**

```go
r := gin.New()
//middleware
r.Use(gin.Logger())
r.Use(gin.Recovery())
r.GET("/abc", MyLogger(), Endpoint)
auth := r.Group("/").Use(AuthRequired())
{
    router.GET("/someGet", getting)
    router.POST("/somePost", posting)
    router.PUT("/somePut", putting)
    router.DELETE("/someDelete", deleting)
    router.PATCH("/somePatch", patching)
    router.HEAD("/someHead", head)
    router.OPTIONS("/someOptions", options)
}

// Http Config
r := gin.Default()
s := &http.Server{
    Addr:           ":8080",
    Handler:        router,
    ReadTimeout:    10 * time.Second,
    WriteTimeout:   10 * time.Second,
    MaxHeaderBytes: 1 << 20,
}
s.ListenAndServe()
```

**中间件中使用 Goroutine** _需要copy gin.Context_

```go
cCp := c.Copy()
go func() {
    path := cCp.Request.URL.Path
}
```

**Cookie**

```go
cookie, err := c.Cookie("gin_c")
c.SetCookie("gin_c", "12", 3600, "/", "localhost", false, true)
```

**Router**

```go
r.GET("/user/:name", func(c *gin.Context) {
    name := c.Param("name")
})
r.GET("/user/:name/*action", func) //{name}/{action?}

// static
r.Static("/assets", "./assets")
r.StaticFS("/static", http.Dir("file_sys"))
r.StaticFile("/favicon.ico", "./res/favicon.ico")

v1 := r.Group("/v1")
{
    v1.GET("/login", loginEndpoint)
    v1.POST("/read", readEndpoint)
}
```

**Redirect**

```go
r.GET("/test", func(c *gin.Context) {
    c.Redirect(http.StatusMovedPermanently, "http://google.com")
    // or
    c.Request.URL.Path = "/test2"
    r.HandleContext(c)
})
```



#### Response

**AsciiJSON**

```go
r.GET("/", func(c *gin.Context) {
    data := map[string]interface{}{
        "lang": "Go语言",
        "tag": "<br>",
    }
    
    // 输出: {"lang":"GO\u8bed\u8a00","tag":"\u003cbr\u003e"}
    c.AsciiJSON(http.StatusOK, data)
})
```

**HTML**

```go
func main() {
    r := gin.Default()
    r.LoadHTMLGlob("templates/*")
    r.GET("/", func(c *gin.Context) {
    	c.HTML(http.StatusOK, "index.tmpl", gin.H{
        	"title": "Main website"
    	})
	})
    
    // or
    r.Delims("{[{", "}]}") // 自定义分隔符
    r.SetFuncMap(template.FuncMap{
        "formatAsDate": formatAsDate,
    }) // 自定义模板函数
    
    r.LoadHTMLGlob("templates/**/*")
    r.GET("/user/index", func(c *gin.Context) {
        c.HTML(http.StatusOK, "user/index.tmpl", gin.H{
            "title": "Users"
        })
    })
    
    r.Run(":8080")
}
```

`index.tmpl`

```html
<html>
    <h1>
        {{ .title }}
    </h1>
</html>
```

`user/index.tmpl`

```html
{{ define "user/index.tmpl" }}
<html>
    <h1>
        {{ .title }}
    </h1>
    Date: {{.now | formatAsDate}}
</html>
{{ end }}
```

**Http2 server 推送** ??

**JSONP** ??

**PurseJSON 字面字符** | **SecureJSON**

```go
c.PurseJSON(200, gin.H{
    "html": "<b>Hello, world!</b>",
    // 大概不转义吧，因json转义为 \uxxxx
})

names := []string{"lena", "austin", "foo"}
// 将输出：while(1);["lena","austin","foo"]
c.SecureJSON(http.StatusOK, names)
```

**Log Writer**

```go
gin.DisableConsoleColor()
f, _ := os.Create("gin.log")
gin.DefaultWriter = io.MultiWriter(f)
gin.DefaultWriter = is.MultiWriter(f, os.Stdout) // 日志和控制台
```

