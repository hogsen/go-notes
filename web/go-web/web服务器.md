# 创建一个 HTTP 服务

有两个过程：首先需要注册路由，即提供 URL 模式和 handler 函数的映射；其次就是实例化一个 server 对象，并开启对客户端的监听。

```go
func IndexHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello, world!")
}
func main() {
	http.HandleFunc("/", IndexHandler)
	http.ListenAndServe(":8080", nil)
}
```

# 访问静态资源

使用 http.FileServer()访问静态资源

```go
http.Handle("/src", http.FileServer(http.Dir(`D:\github\books\go\gotest\go-notes\基础`)))
```

# 路由处理

http.HandleFunc()
Handler 接口的 ServeHTTP()方法，所以第二个参数就是实现了 Handler 接口的 handler 实例。

```go
func HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
	DefaultServeMux.HandleFunc(pattern, handler)
}
```
