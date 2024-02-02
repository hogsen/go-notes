# 开始

```go
package main

import "github.com/gin-gonic/gin"

func main() {
	// gin.Default()不仅创建了Gin引擎实例，还自动附带了两个常用的中间件：Logger 和 Recovery。
	r := gin.Default()
	// 定义一个GET路由，处理路径为"/user"的请求
	// getUser是一个路由处理函数
	r.GET("/user", getUser)
	r.Run(":8080") // 在0.0.0.0:8080上启动服务；要指定端口，可以使用r.Run(":8080")
}

// gin.Context结构体封装了处理单个HTTP请求所需要的所有信息和方法
func getUser(c *gin.Context) {
	// 从URL查询参数中获取名为"name"的值
	// user?name=
	name := c.Query("name")
	// 如果name为空，则使用默认值"Guest"
	if name == "" {
		name = "Guest"
	}
	// 设置响应内容类型为JSON
  // gin.H是map[string]interface{}的缩写，表示字符串键和任意类型的映射，interface{}表示任意类型
	c.JSON(200, gin.H{
		"message": "Hello, " + name + "!",
	})
}

```

# `gin.Context`结构体
