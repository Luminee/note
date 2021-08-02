#### Installation

```shell
$ mkdir myapp
$ cd myapp
$ go mod init myapp
$ go get github.com/kataras/iris/v12@master

$ go env -w GOPROXY=https://goproxy.cn,direct
```

#### Usage

`main.go`

```go
package main

import "github.com/kataras/iris/v12"
import "github.com/kataras/iris/v12/mvc"

func main() {
    app := iris.New()
    booksAPI := app.Party("/books")
    {
        booksAPI.Use(iris.Compression)
        
        // GET: http://localhost:8080/books
        booksAPI.Get("/", list) 
        booksAPI.Post("/", create) // POST: /books
    }
    
    // MVC mode
    m := mvc.New(booksAPI)
    m.Handle(new(BookController))
    
    app.Listen(":8080")
}

type Book struct {
    Title string `json:"title"`
}

func list(ctx iris.Context) {
    books := []Book{
        {"Golang"},
        {"JavaScript"},
    }
    
    ctx.JSON(books)
}

func create(ctx iris.Context) {
    println("Creating")
    ctx.StatusCode(iris.StatusCreated)
}

// MVC mode
type BookController struct {
    
}

// GET: /books
func (c *BookController) Get() []Book {
    return []Book{
        {"Golang"},
        {"JavaScript"},
    }
}

// POST: /books
func (c *BookController) Post() int {
    return iris.StatusCreated
}
```

**Run**

```shell
$ go run main.go
> Now listening on: http://localhost:8080
> Application started. Press CTRL+C to shut down.
```



