# 概念

中间件本质是一个处理函数，它可以用作单个路由、路由组或全局引擎上。

# 注册及使用

中间件可以通过 Engine、路由组或单个路由进行注册。注册中间件时，需要传递一个或多个`gin.HandlerFunc`类型的处理函数。
在 Gin 中，中间件处理函数和业务处理函数共享一个 gin.Content 对象。这个对象包含了当前请求的所有信息和一些有用的方法。比如获取请求参数、设置响应头、记录日志等。因此，中间件可以通过修改 Context 对象的状态或添加自定义的数据来影响后续的处理过程
通过中间件可以实现像身份验证、访问控制、限流、熔断等。

# 编写一个中间件

中间件的本质是一个符合特定签名的函数，他接收一个`*gin.Context`参数，并通常包含一个对`c.Next()`的调用将请求传递给处理链中的下一个处理程序
**编写 Gin 中间件的步骤**

1. 定义中间件函数
   创建一个函数，它接收`*gin.Context`作为参数
2. 处理请求前逻辑
   在调用`c.Next`之前，执行任何你希望在处理请求之前进行的操作，比如身份验证、日志记录等。
3. 调用`c.Next()`
   调用`c.Next()`以将请求控制权传递个链中的下一个处理程序。如果没有更多的处理程序，请求将终止。
4. 处理请求后的逻辑
   在`c.Next()`之后，你可以执行任何你希望在处理完执行的操作，比如日志记录、统计信息。
5. 将中间件注册到 Gin 引擎或路由中
   使用`Use()`方法将中间件注册到 Gin 引擎或特定的路由中

```go
package main

import (
	"log"
	"time"

	"github.com/gin-gonic/gin"
)

//

// 定义中间件函数
func loggingTimeMiddleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		// 设置开始时间
		start := time.Now()
		c.Next()
		end := time.Now()
		duration := end.Sub(start)
		// 记录日志
		log.Printf("Request to %s took %v\n", c.Request.URL.Path, duration)
	}
}
func main() {
	r := gin.Default()
	// 注册路由到全局引擎
	r.Use(loggingTimeMiddleware())
	// 定义路由和相应的处理函数
	r.GET("/", func(c *gin.Context) {
		c.String(200, "Hello, World!")
	})
	err := r.Run(":8080")
	if err != nil {
		log.Fatalf("Error starting server: %v\n", err)
	}
}

```
