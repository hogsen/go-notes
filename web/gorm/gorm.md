# 开始 gorm

1. 导入 gorm 框架
   `go get -u gorm.io/gorm`
2. 下载数据库驱动
   `go get -u gorm.io/driver/mysql`
   `go get -u gorm.io/driver/postgres`
3. 连接数据库

```go
package main

import (
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
)

type Book struct {
	ID        uint `gorm:"primaryKey"`
	Title     string
	Author    string
	Price     float64
	CreatedAt string
	UpdatedAt string
	DeletedAt string
	// gorm:"index" 表示创建一个索引，用于加速查询
	// gorm:"unique" 表示创建一个唯一索引，用于加速查询和避免重复数据
	ISBN string `gorm:"index;unique"`
}

func main() {
	// 创建数据库连接
	dsn := "root:LHS052234a@tcp(127.0.0.1:3306)/booksdb?charset=utf8mb4&parseTime=True&loc=Local"
	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
	if err != nil {
		panic("failed to connect database")
	}
	// 自动迁移模式，将 Book 结构体映射为数据库中的表
	err = db.AutoMigrate(&Book{})
	if err != nil {
		panic("failed to migrate database")
	}
}

```
