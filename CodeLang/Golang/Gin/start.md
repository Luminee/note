#### Installation

```shell
$ mkdir myapp
$ cd myapp
$ go mod init myapp
$ go get github.com/gin-gonic/gin

$ go env -w GOPROXY=https://goproxy.cn,direct
```

#### Usage

`main.go`

```go
package main

import "github.com/gin-gonic/gin"

func main() {
    r := gin.Default()
    r.GET("/ping", func(c *gin.Context) {
        c.JSON(200, gin.H{
            "message": "pong"
        })
    })
    r.Run()  // :8080
}
```

#### Run

```shell
$ go run main.go
```

