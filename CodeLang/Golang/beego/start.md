#### Installation

```shell
$ mkdir myapp
$ cd myapp
$ go mod init myapp
$ go get github.com/beego/beego/v2
$ go get -u github.com/beego/bee/v2

$ go env -w GOPROXY=https://goproxy.cn,direct
```

#### New

```shell
$ bee new application
```

```markdown
application
|-- conf
|   |-- app.conf
|-- controllers
|   |-- default.go
|-- models
|-- routers
|   |-- router.go
|-- static
|   |-- css
|   |-- img
|   |-- js
|-- tests
|   |-- default_test.go
|-- views
|   |-- index.tpl
|-- main.go
```

#### Run

```shell
$ bee run
```



#### Configure

`app.conf`

```ini
httpport = 8080
runmode = "dev"
viewspath = "views"

mysqluser = "root"
mysqlpass = "1111"
mysqlurls = "127.0.0.1"
mysqldb = "blog"

[dev]
mysqlurls = "0.0.0.0"
[prod]
mysqlurls = "192.168.0.1"

include "app2.conf"
```

`app2.conf`

```ini
autorender = false
recoverpanic = false
```





```go
web.AppConfig.String("mysqluser")
web.AppConfig.String("dev::mysqluser")
```

