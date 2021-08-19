#### Router

`routers/router.go`

```go
func init() {
    web.Router("/", &controllers.MainController{})
    
    web.Router("/", &Ctrl{}, "*:Index") // Any to func Index
    web.Router("/user", &Ctrl{}, "get:ListUser")
    web.Router("/user", &Ctrl{}, "post:StoreUser")
    
    web.Router("/api", &Ctrl{}, "get,post:ApiFunc")
    web.Router("/api", &Ctrl{}, "get:List;post:Create")
    
    web.AutoRouter(&controller.ObjController{})
    // /obj/login to ObjController@Login
    // /obj/blog/21/19 to ObjCtrl@Blog 参数 map[0:21 1:19]
    
    // /ctrl/user.json  this.Ctx.Input.Param(":ext") => json
}
```

> `Get`, `Post`, `Put`, `Patch`, `Head`, `Options`, `Delete`, `Any`

```go
web.Router("/api/:id", &RController{}) // {id}
web.Router("/api/?:id", &C{}) // {id?}
web.Router("/api/:id([0-9]+)") // 正则
web.Router("/d/*.*") // /d/f/a.xml  :path=f/a.xml :etx=xml 
web.Router("/d/c/*") // /d/c/f/a.json :splat=f/a.json
web.Router("/:id:int") // :id 整形，([0-9]+)
web.Router("/:hi:string") // :hi 字符 ([\w]+)
web.Router("/cd_:id([0-9]+).html") // cd_12.html :id=12
```

```go
// 自定义handler
s := rpc.NewServer()
s.RegisterCodec(json.NewCodec(), "application/json")
s.RegisterService(new(HelloService), "")
web.Handler("/rpc", s)
```



#### Request

```go
this.Ctx.Input.Param(":id")  // /api/:id
this.GetString("name")

// Stop
this.StopRun()
```







#### Controller

`controllers/main.go`

```go
type MainController struct {
    web.Controller
}

func (c *MainController) Get() {
    c.Data["site"] = "local",
    c.Data["name"] = "nick",
    c.Layout = "admin/layout.html"
    c.TplName = "admin/index.tpl"
}

func (c *MainController) URLMapping() {
    c.Mapping("Static", c.Static)
    c.Mapping("AllBlock", c.AllBlock)
}

// @router /static/:key [get]
func (c *MainController) Static() {
    
}

// @router /all/:key [get]
func (c *MainController) AllBlock() {
    
}
```

`router.go`

```go
web.Include(&MainController{})
// like:
web.Router("/static/:key", &MainController{}, "get:Static")
web.Router("/all/:key", &MainController{}, "get:AllBlock")
```

**Other**

> `web.CtrlGet(router, pkg.controller.method)`
>
> `Ctrl-`: `Post`, `Put`, `Patch`, `Head`, `Option`, `Delete`, `Any`

```go
//Controller
type BaseCtrl struct {
    web.Controller
}

func (b BaseCtrl) Ping() {
    b.Data["json"] = "pong"
    b.ServeJSON()
}

func (b *BaseCtrl) PingPointer() {
    b.Data["json"] = "pong_pointer"
    b.ServeJSON()
}

// Router
web.CtrlGet("/ping", BaseCtrl.Ping)
web.CtrlGet("/ping_pointer", (*BaseCtrl).PingPointer)
web.Run()
```

#### NameSpace

```go
ns := 
web.NewNamespace("/v1",
    web.NSCond(func(ctx *context.Context) bool {
        if ctx.Input.Domain() == "api.beego.me" {
            return true
        }
        return false
    }),
    web.NSBefore(auth),
    // GET /v1/notallowed
    web.NSGet("/notallowed", func(ctx *context.Context) {
        ctx.Output.Body([]byte("notAllowed"))
    }),
    // GET /v1/version
    web.NSRouter("/version", &AdminCtrl{}, "get:ShowVersion"),
    // GET /v1/changePwd
    // POST /v1/changePwd
    web.NSRouter("/changePwd", &UserCtrl{}),
    // GET /v1/shop/123
    web.NSNamespace("/shop",
        web.NSBefore(sentry),
        web.NSGet("/:id", func(ctx *context.Context) {
            ctx.Output.Body([]byte("notAllowed"))
        }),
    ),
    // GET /v1/cms/  注解路由
    web.NSNamespace("/cms",
        web.NSInclude(
            &controllers.MainController{},
            &controllers.CMSController{},
            &controllers.BlockController{},
        ),
    ),
)
//注册 namespace
web.AddNamespace(ns)
```

