#### Grouping Routes

```go
app := iris.Default()

v1 := app.Party("/v1")
{
    v1.Post("/login", login)
}

v2 := app.Party("/v2")
{
    v2.Post("/login", login)
}
```

#### Middleware

```go
app := iris.New()
app.UseRouter(recover.New())

authorized := app.Party("/")
authorized.Use(AuthRequired())
```

**Custom Middleware**

```go
func Logger() iris.Handler {
    ctx.Values().Set("framework", "iris")
    // Before
    ctx.Next()
    // After
    log.Println(0)
}

app.Use(Logger())
```

_Use Basic Authentication_ ??

_Goroutines in middleware_ ??

**Logger**

```go
f, _ := os.Create("iris.log")
app.Logger().SetOutput(f)

// accessLog
ac := accesslog.File("./access.log")
ac.AddOutput(os.Stdout)
ac.Delim = '|'
ac.TimeFormat = "2006-01-02 15:04:05"

defer ac.Close()

app.UseRouter(ac.Handler)
```

#### [Model binding and validation](https://www.iris-go.com/docs/#/?id=model-binding-and-validation)  _???_

**ReadQuery** **ReadForm**

```go
type Person struct {
    Name    string `url:"name,required"`
    Address string `url:"address"`
}

err := ctx.ReadQuery(&person)
// ReadQuery bind URL params, ReadForm bind post data
ctx.ReadBody(&person) // Bind any client data

err := ctx.ReadParams(&p) // bind /{name}/{age}
err := ctx.ReadHeaders(&hs) // bind header
```

#### Redirects

```go
ctx.Redirect("/login", iris.StatusFound)
// or
r := ctx.Request()
r.URL.Path = "/test2"
ctx.Application().ServeHTTPC(ctx)
// or
ctx.Exec("GET", "/test2")
```

```go
redirects := rewrite.Load("redirects.yml")
app.WrapRouter(redirects)
```

`redirects.yml`

```yaml
RedirectMatch:
  # Redirects /seo/* to /*
  - 301 /seo/(.*) /$1

  # Redirects /docs/v12* to /docs
  - 301 /docs/v12(.*) /docs

  # Redirects /old(.*) to /
  - 301 /old(.*) /

  # Redirects http or https://test.* to http or https://newtest.*
  - 301 ^(http|https)://test.(.*) $1://newtest.$2

  # Handles /*.json or .xml as *?format=json or xml,
  # without redirect. See /users route.
  # When Code is 0 then it does not redirect the request,
  # instead it changes the request URL
  # and leaves a route handle the request.
  - 0 /(.*).(json|xml) /$1?format=$2

# Redirects root domain to www.
# Creation of a www subdomain inside the Application is unnecessary,
# all requests are handled by the root Application itself.
PrimarySubdomain: www
```

#### Http Configuration

```go
http.ListenAndServe(":8080", app)

// Custom
srv := &http.Server{
    Addr:           ":8080",
    Handler:        app,
    ReadTimeout:    10 * time.Second,
    WriteTimeout:   10 * time.Second,
    MaxHeaderBytes: 1 << 20,
}
srv.ListenAndServe()
```

