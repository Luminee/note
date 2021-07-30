### CURD

#### 1. Create

```go
user := User{Name: "Liu", Age: 20}
result := db.Create(&user)

user.ID             // returns inserted data's primary key
result.Error        // returns error
result.RowsAffected // returns inserted records count
```

```go
// 为指定字段赋值
db.Select("Name", "Age").Create(&user)
// INSERT INTO `users` (`name`,`age`) VALUES ("jinzhu", 18)、

// 创建并忽略字段
db.Omit("Name", "Age", "CreatedAt").Create(&user)
// INSERT INTO `users` (`birthday`,`updated_at`) VALUES ("2020-01-01 00:00:00.000", "2020-07-04 11:05:21.775")
```

批量插入

```go
var users = []User{{Name: "A"}, {Name: "B"}, {Name: "C"}}
db.Create(&users)

for _, user := range users {
  user.ID // 1,2,3
}

// 分批次插入
var users = []User{{Name: "A_1"}, ...., {Name: "A_10000"}}
// batch size 100
db.CreateInBatches(users, 100)
```

钩子**Hooks** : `BeforeSave`, `BeforeCreate`, `AfterSave`, `AfterCreate`

```go
func (u *User) BeforCreate(tx *grom.DB) {
    u.Name = "New"
    return
}
```

如果想跳过钩子，使用`SkipHooks`

```go
DB.Session(&gorm.Session{SkipHooks: true}).Create(&user)
```

Create 支持 `map[string]interface{}` 和 `[]map[string]interface{}{}`

```go
db.Model(&User{}).Create(map[string]interface{}{
    "Name": "AA", "Age": 18,
})
db.Model(&User{}).Create([]map[string]interface{}{
    {"Name": "AA_1", "Age": 18},
    {"Name": "AA_2", "Age": 20},
})
```

关联创建

```go
type CreditCard struct {
    gorm.Model
    Number   string
    UserID   uint
}

type User struct {
    gorm.Model
    Name       string
    CreditCard CreditCard
}

db.Create(&User{
    Name: "AA",
    CreditCard: CreditCard{Number: "222"}
})
// INSERT INTO `users` ...
// INSERT INTO `credit_cards` ...

// 使用 `Select`, `Omit` 来忽略关联
db.Omit("CreditCard").Create(&user)

// 忽略所有关联
db.Omit(clause.Associations).Create(&user)
```

默认值

> 对于设置默认值的字段 `0`, `''`, `false` 都不会插入，可以使用指针类型来规避，数据库中设置默认值了，则必须设置 `default` 标记

```go
type User struct {
    Name   string `gorm:"default:AA"`
    // 指针规避
    Age    *int `gorm:"default:18"`
    Active sql.NullBool `gorm:"default:true"`
}
```

#### 2. Update

**Save** 更新全部字段

```go
db.First(&user) // Name: "BB", Age: 20

user.Name = "AA"
db.Save(&user)
// UPDATE users SET name='AA', age=20 WHERE id=12;
```

**Update** 更新 单/多 字段 (当使用 `struct` 更新时，只更新非零字段，可使用`map` 或`Select` 来规避)

```go
// Update users SET name = 'bb' WHERE id = 10
db.Model(&User{}).Where("id = ?", 10).Update("name", "bb")
// or
db.Model(&user).Update("name", "bb") // id = 10

// Update multiple columns
db.Model(&user).Updates(User{Name: "bb", Active: false})
// Update name, updated_at WHERE id = 10
db.Model(&user).Updates(map[string]interface{}{"name": "bb", "active": false})
// Update name, active, updated_at WHERE id = 10

// Where 批量更新
db.Model(User{}).Where("role = ?", "admin").Updates()
db.Table("users").Where("id IN ?", []int{10, 11}).Updates()

// Select 的使用(Omit 同理)
db.Model(&user).Select("name").Updates(map[string]interface{}{"name": "bb", "age": 18, "active": false})
// Update name WHERE id = 10
db.Model(&user).Select("*").Omit("Role").Update()
```

钩子 **Hooks** : `BeforeSave`, `BeforeUpdate`, `AfterSave`, `AfterUpdate`

默认不添加条件则无法全量更新，请添加 `Where` 或使用原生SQL语句

```go
result := db.Model(User{}).Where("role = ?", "admin").Updates()

result.RowsAffected // returns updated records count
result.Error        // returns updating error
```

通过SQL 表达式更新

```go
.Update("price", gorm.Expr("price * ? + ?", 2, 100))
// SET "price" = price * 2 + 100
.UpdateColumn("qty", gorm.Expr("qyt - ?", 1))
// SET "qty" = qty - 1
```

子查询更新

```
.Update("c_name", db.Model().Select("name").Where)
```

使用 `UpdateColumn` 和 `UpdateColumns` 跳过 `Hooks` 跟踪和 **time** 更新

**Changed** 方法返回字段是否有改变，在 **Before Update Hooks** 中生效

```go
func (u *User) BeforeUpdate(tx *gorm.DB) {
    // if Name or Admin changed
    if tx.Statement.Changed("Name", "Admin") { 
        tx.Statement.SetColumn("Age", 18)
    }
    
    // if any fields changed
    if tx.Statement.Changed() {
        // 改变当前值
        tx.Statement.SetColumn("RefreshedAt", time.Now())
    }
}
```

#### 3. Delete

```go
// DELETE FROM users WHERE id = 10;
db.Delete(&User{}, 10)
// DELETE FROM users WHERE id IN (1,2,3);
db.Delete(&users, []int{1,2,3})

// DELETE from users WHERE name like "%aa%"
db.Where("name like ?", "%aa%").Delete(User{})
db.Delete(User{}, "name like ?", "%aa%")
```

钩子 **Hooks** : `BeforeDelete`, `AfterDelete`

默认不添加条件则无法全量更新，请添加 `Where` 或使用原生SQL语句

软删除 : 模型包含 `gorm.DeletedAt` (或在 `gorm.Model` 中)

```go
db.Unscoped().Where().Find() // 忽略软删 (查找全部)
db.Unscoped().Delete(&order) // 硬删除 (真删)
```



#### 4. Query

**Single Object**

```go
// SELECT * FROM users ORDER BY id LIMIT 1
db.First(&user)
// SELECT ... LIMIT 1
db.Take(&user)
//SELECT ... ORDER BY id DESC LIMIT 1
db.Last(&user)

result := db.First(&user)
result.RowsAffected // returns count of records found
result.Error        // returns error or nil

// check error ErrRecordNotFound
errors.Is(result.Error, gorm.ErrRecordNotFound)
```

`First` 和 `Last` 只在 **db.Model()** 中可用， 若没有设置主键，则使用第一个字段

> **db.Table("users").First(&res)** 不可用

