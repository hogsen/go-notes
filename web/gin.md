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

gin.Context 是一个结构体类型，它封装了原生的 Go HTTP 请求和响应对象，
同时还提供了一些便捷的方法，用于获取请求信息（如 URL 参数、查询参数、表单数据、JSON 数据等），
以及设置响应信息（如响应头、响应状态码、响应体等）。
gin.Context 是通过中间件来传递的。
每个中间件都可以对 gin.Context 进行操作，然后将其传递给下一个中间件，直到最终的路由处理函数。
这使得开发者可以在中间件中执行各种任务，如身份验证、日志记录、请求修改等。

## `gin.Context`的常用方法

1. 获取请求信息：

- `c.Request`:获取原始的 `*http.Request`对象。
- `c.Params`：获取路由参数，返回一个参数切片，每个参数都是一个键值对。
- `c.Param(key string)`：根据键名获取单个路由参数的值。
- `c.Query(key string)`：获取 URL 查询参数的值。
- `c.QueryArray(key string)`：获取 URL 查询参数的值，返回字符串切片。
- `c.PostForm(key string)`：获取 POST 请求的表单参数值。
- `c.Cookie(name string)`：获取指定名称的 Cookie 值。
- `c.Header(key string)`：获取请求头的值。

2. 设置响应信息

- `c.Writer`：获取原始的 gin.ResponseWriter 对象，用于直接写入响应。
- `c.JSON(code int, obj interface{})`：将对象序列化为 JSON 格式，并作为响应返回。
- `c.String(code int, format string, values ...interface{})`：返回格式化的字符串响应。
- `c.Redirect(code int, url string)`：重定向到指定的 URL。
- `c.SetCookie(name, value string, maxAge int, path, domain string, secure, httpOnly bool)`：设置 Cookie。
- `c.Header(key, value string)`：设置响应头。
- `c.Status(code int)`：设置响应状态码，不发送响应体。

3. 其它方法

- `c.ShouldBind(obj interface{})`：将请求数据绑定到指定的结构体对象上。
- `c.ShouldBindJSON(obj interface{})`：将 JSON 请求数据绑定到指定的结构体对象上。
- `c.ShouldBindQuery(obj interface{})`：将 URL 查询参数绑定到指定的结构体对象上。
- `c.ShouldBindUri(obj interface{})`：将路由参数绑定到指定的结构体对象上。
- `c.AbortWithStatus(code int)`：中断请求处理，并设置响应状态码。
- `c.AbortWithStatusJSON(code int, jsonObj interface{})`：中断请求处理，并设置 JSON 响应和状态码。
- `c.Next()`：调用处理链中的下一个处理函数（通常用于中间件）。
