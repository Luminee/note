### Start

### 1. install

```shell
go get -u gorm.io/gorm
go get -u gorm.io/driver/mysql
```

### 2. Usage

```go
type Product struct {
    gorm.Model
    Code  String
    Price unit
}

func main() {
    db, err := gorm.Open(mysql.Open("test.db"), &gorm.Config{})
    if err != nil {
        panic("failed to connect database")
    }
    
    db.AutoMigrate(&Product{})
    
    // Create
    db.Create(&Product{Code: "D2", Price: 100})
    
    // Read
    var product Product
    // find product with integer primary key
    db.First(&product, 1) 
    // find product with code D42
    db.First(&product, "code = ?", "D2")
    
    // Update - update product's price to 200
    db.Model(&product).Update("Price", 200)
    // Update - update multiple fields
    db.Model(&product).Updates(Product{Price: 200, Code: "F42"})
    
    // Delete - delete product
    db.Delete(&product, 1)
}
```

### 3. Models

```go
type User struct {
    ID        unit
    Name      string
    Email     *string
    CreatedAt time.Time
    UpdatedAt time.Time
}
```

> `Gorm` 使用 **ID** 作为主键，结构名的 **snake_case** 复数形式作为表名，**snake_case** 作为字段名，**CreatedAt** 和 **UpdatedAt** 追踪时间

使用 `gorm.Model` 默认包含 **ID**, **CraatedAt**, **UpdatedAt**, **DeletedAt** 字段

```go
// gorm.Model definition
type Model struct {
    ID        unit           `gorm:"primaryKey"`
    CreatedAt time.Time
    UpdatedAt time.Time
    DeletedAt gorm.DeletedAt `gorm:"index"`
}
```

允许设置字段访问权限

| Tag  | Desc                                                         |
| ---- | ------------------------------------------------------------ |
| `<-` | set write: `<-` C & U, `<-:create` create-only, `<-:false` no write |
| `->` | set read: `->:false` no read                                 |
| `-`  | ignore this field `-` no read/write                          |

```go
type User struct {
    Name string `gorm:"<-:create"` // Read, Create
    Name string `gorm:"->"`        // R, !W* (unless configured)
    Name string `gorm:"->;<-:create"` // R, C
    Name string `gorm:"-"`  // R&W ignore when use struct
}
```

**CreatedAt** **UpdatedAt** 可以使用标记 `autoCreateTime`, `autoUpdateTime` 自定义字段名

```go
type User struct {
    CreatedAt time.Time // current time on creating
    UpdatedAt int       // current unix seconds on upd or crt
    Updated   int64 `gorm:"autoUpdateTime:nano"` // nano seconds
    Updated   int64 `gorm:"autoUpdateTime:milli"`// milli seconds
    Created   int64 `gorm:"autoCreateTime"`      // unix seconds
}
```

嵌入式结构，以下两种结构等价

```go
type User struct {
    gorm.Model
    Name string
}

// equals
type User struct {
    ID        uint           `gorm:"primaryKey"`
    CreatedAt time.Time
    UpdatedAt time.Time
    DeletedAt gorm.DeletedAt `gorm:"index"`
    Name string
}
```

```go
type Author struct {
    Name  string
    Email string
}

type Blog struct {
    ID      int
    // 嵌入字段
    Author  Author `gorm:"embedded"`
    // 或者嵌入前缀
    Author  Author `gorm:"embedded;embeddedPrefix:author_"`
    Upvotes int32
}

// equals
type Blog struct {
    ID    int64
    // 嵌入字段
    Name  string
    Email string
    // 则加入前缀
    AuthorName  string
    AuthorEmail string
    Upvotes  int32
}
```

字段标签 **Tags** 使用 **camelCase** 形式

```go
type Author struct {
    ID uint `gorm:"primaryKey"
}
```

> **column**, **type**, **size**, **primaryKey**, **unique**, **default**, **precision**, **scale**, **not null**, **autoIncrement**, **autoIncrementIncrement**(控制自增步长), **embedded**, **embeddedPrefix**, **autoCreateTime**, **autoUpdateTime**, **index**, **uniqueIndex**, **check**, **<-**, **->**, **-**, **comment**

### 4. Connection to Database

**MySQL**

```go
import (
    "gorm.io/driver/mysql"
    "gorm.io/gorm"
)

func main() {
    dsn := "user:pwd@tcp(127.0.0.1:3306)/dbname?charset=utf8mb4&parseTime=True&loc=Local"
    db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
}
```

> 为处理 `time.Time`, 需要包含 `parseTime` 参数，使用`charset=uft8mb4` 来全量支持**UTF-8**

- 使用 `mysql.New(mysql.Config{})`来自定配置**MySQL**

- 可以使用 `DriverName` 来自定义驱动
- 使用 `Conn` 来使用现有连接

**PostgreSQL**, **SQLite**, **SQL Server**, **Clickhouse**

