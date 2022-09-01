#### Files

```
.
|-- app
|    |-- controllers
|    |    |-- authController.go
|    |    |-- baseController.go
|    |    |-- indexController.go
|    |    |-- userController.go
|    |-- middlewares
|    |    |-- verifyLogin.go
|    |-- models
|    |    |-- user.go
|-- config
|    |-- database.go
|-- core
|    |-- array.go
|    |-- query.go
|    |-- view.go
|-- public
|    |-- assets
|    |    |-- css
|    |    |    |-- frame.css
|    |    |    |-- style.css
|    |    |-- image
|    |    |    |-- 1.jpg
|    |    |    |-- 2.jpg
|    |    |-- js
|    |    |    |-- jquery.js
|-- routers
|    |-- webRouter.go
|-- views
|    |-- auth
|    |    |-- login.html
|    |-- layouts
|    |    |-- body.html
|    |    |-- header.html
|    |    |-- script.html
|    |    |-- sidebar.html
|    |-- user
|    |    |-- create.html
|    |    |-- edit.html
|    |    |-- list.html
|    |-- index.html
|-- access.log
|-- go.mod
|-- main.go
```

#### App

**Controller**

```go
type IndexController struct {
	BaseController
}

func (c *IndexController) Get() mvc.Result {
	return mvc.View{
		Name:   "index.html",
		Layout: "layouts/body.html",
		Data:   iris.Map{"userInfo": c.GetAdmin()},
	}
}

// Redirect
return mvc.Response{
    Path: url,
}
```

**Middleware**

```go
var ignore = []string{
	"GET /auth/login",
	"POST /auth/login",
}

func VerifyLogin(ctx iris.Context) {
	u := ctx.GetCurrentRoute()
	path := u.Method() + " " + u.Path()
	if core.InArray(path, ignore) { // Ignore
		ctx.Next()
		return
	}
	s := sessions.Get(ctx)
	if info := s.Get("userInfo"); info == nil {
		ctx.Redirect("/auth/login", iris.StatusFound)
		return
	}
	ctx.Next()
}
```

**Model**

```go
type User struct {
	ID       uint
	Username string `json:"username"`
	Password string `json:"password"`
	Flag     string `json:"flag"`
}

func (User) TableName() string { // Set table name
	return "admin"
}

db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
db.Model(&User{}).Find(&users)
```

#### Config

**database.go**

```go
type Database struct {
	Host     string
	Port     string
	Dbname   string
	Username string
	Password string
}

var defConn = "mysql-1"

var connMap = map[string]Database{
	"mysql-1": {Host: "127.0.0.1", Port: "3306",
		Dbname: "blog", Username: "root", Password: "1111"},
}

func (*Database) GetConf(conn string) Database {
	return connMap[conn]
}

func (db *Database) GetDefaultConf() Database {
	return db.GetConf(defConn)
}
```

#### Routers

**webRouter.go**

```go
var sessManager = sessions.New(sessions.Config{
	Cookie:  "my_session",
	Expires: 24 * time.Hour,
})

func (r *WebRouter) LoadRouters() {
	mvc.Configure(r.App.Party("/"), func(m *mvc.Application) {
		m.Router.Use(sessManager.Handler())
		m.Router.Use(middlewares.VerifyLogin)
		m.Handle(new(controllers.IndexController))
		m.Party("/auth").Handle(new(controllers.AuthController))
		m.Party("/user").Handle(new(controllers.UserController))
	})
}
```

#### Main

**main.go**

```go]
app := iris.Default()

tmpl := iris.HTML("./views", ".html")
tmpl.Reload(true)
app.RegisterView(tmpl)

app.HandleDir("/public/assets", iris.Dir("./public/assets"))

router := routers.WebRouter{App: app}
router.LoadRouters()

app.Run(iris.Addr(":9999"))
```



