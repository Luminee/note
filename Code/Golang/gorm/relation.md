### Relation

#### Belongs To

```go
// `User` belongs to `Company`, `CompanyID` is the foreign key
type User struct {
    gorm.Model
    Name         string
    CompanyID    int      // 隐式外键
    CompanyRefer int
    Company      Company `gorm:"foreignKey:CompanyRefer"` // 显式
    
    Company      Company `gorm:"references:Code"` // 关联 `Code`
}

type Company struct {
    ID   int
    Code string
    Name string
}
```

#### Has One

```go
// User has one CreditCard, CreditCardID is the foreign key
type User struct {
    gorm.Model
    CreditCard CreditCard `gorm:"foreignKey:UserName"` // 显式
    
    CreditCard CreditCard `gorm:"references:name"` // 关联 `Name`
}

type CreditCard struct {
    gorm.Model
    Number string
    UserID uint  // 隐式外键
}
```

#### Has Many

```go
// User has many CreditCards, UserID is the foreign key
type User struct {
    gorm.Model
    MemberNumber string
    CreditCards  []CreditCard // 切片，一对多
    
    CreditCards  []CreditCard `gorm:"foreignKey:UserNumber"`
    CreditCards  []CreditCard `gorm:"references:MemberNumber"`
}

type CreditCard struct {
    gorm.Model
    Number string
    UserID uint
    
    UserNumber string
}
```

#### Many To Many

```go
// User many to many languages, `user_languages` the join table
type User struct {
    gorm.Model
    // 关联表 user_languages
    Languages []*Language `gorm:"many2many:user_languages;"`
}

type Language struct {
    gorm.Model
    Name  string
    // 反关联
    Users []*User `gorm:"many2many:user_languages;"`
}

//   Join Table: user_languages
//   foreign key: user_id, reference: users.id
//   foreign key: language_id, reference: languages.id
```

使用 `foreignKey`, `references`, `joinForeignKey`, `joinReferences` 设置关联id

**多态关联**

```go
type Cat struct {
    ID    int
    Name  string
    Toy   Toy `gorm:"polymorphic:Owner;"` // type = cats
    // type = master
    Toy  Toy `gorm:"polymorphic:Owner;polymorphicValue:master"`
    // 多态一对多  type = cats | or muster ^
    Toys []Toy `gorm:"polymorphic:Owner;"`
}

type Dog struct {
    ID   int
    Name string
    Toy  Toy `gorm:"polymorphic:Owner;"`
}

type Toy struct {
    ID        int
    Name      string
    OwnerID   int
    OwnerType string
}
```

**自关联**

```go
type User struct {
    gorm.Model
    Name      string
    ManagerID *uint
    Manager   *User // 自关联
    
    // 一对多
    Team      []User `gorm:"foreignkey:ManagerID"`
    // 多对多
    Friends []*User `gorm:"many2many:user_friends"`
}
```



-----

#### Relation Query

**Auto Create/Update**

```go
user := User{
    Name:        "aa",
    ShipAddress: Address{Addr: "aa at bb"},
    Emails:      []Email{
        {Email: "aa@qq.com"},
        {Email: "aa@163.com"},
    }
}

db.Create(&user)
// Insert `users`
// Insert `address`
// Insert `emails`
db.Save(&user)
```

**Skip Auto Create/Update**

```go
db.Select("Name").Create(&user) // Insert `users` (name)
db.Omit("ShipAddress").Create(&user) // Skip create address
db.Omit(clause.Associations).Create(&user) // Skip all relations
```

**Association Mode**

```go
var user User
db.Model(&user).Association("Languages")
// 开启关联模式

db.Model().Association("Languages").Find(&languages)
db.Model().Where().Ass().Find(&lang)
```

**Append Associations** / **Replace** / **Delete** / **Clear** / **Count**

#### Eager Loading

```go
db.Preload("Orders").Preload("Role").Find(&users)
// SELECT * FROM users;
// SELECT * FROM orders WHERE user_id IN (1,2,3,4); // has many
// SELECT * FROM roles WHERE id IN (4,5,6); // belongs to

db.Preload(clause.Associations).Find(&users)
// 预加载所有关联
db.Preload("Orders.OrderItems.Product")
// 预加载下级关联

db.Preload("Orders", "age > ?", "18").Find(&users)
// 预加载带查询
db.Preload("Orders", func(db *gorm.DB) *gorm.DB {
  return db.Order("orders.amount DESC")
}).Find(&users)
```

