# RPC

通过 RPC 开发者可以在客户端程序上直接调用服务端程序上的方法，且无需额外为此调用过程编写网络通信相关代码
RPC 流程
客户端通过 RPC 发送调用服务的请求——>服务器接收这个请求——>调用服务——>返回结果

# RPC 调用过程

1. 客户端：服务调用方
2. 客户端存根(Client Stub):存放服务端地址信息，将客户端的请求参数数据信息打包成网络消息，再通过网络传输发送给服务端。
3. 网络服务：传输协议，可以是 TCP 或 HTTP。
4. 服务端存根(Serve Stub):接收客户端发送过来的请求消息并进行解包，然后再调用本地服务进行处理。
5. 服务端:服务的真正提供者。
   客户端程序通过网络或者其他 IO 连接调用一个远程对象的公开方法
   在 RPC 服务端，可将一个对象注册为可访问的服务，之后该对象的公开方法就能够以远程的方式提供访问。

# 使用 RPC

符合 RPC 规则的方法要求：
方法只能有两个可序列化的参数，其中第二个参数是指针类型，并且返回一个 error 类型，同时必须是公开的方法。

服务端

```go
type User struct {
	ID   int
	Name string
}

func (u *User) GetUser(id int, user *User) error {
	userMap := map[int]User{
		1: {ID: 1, Name: "Alice"},
		2: {ID: 2, Name: "Bob"},
	}
	if u, ok := userMap[id]; ok {
		*user = u
		return nil
	} else {
		return fmt.Errorf("User not found")
	}
}
func main() {
  // rpc 注册外面可以调用的方法
	_ = rpc.Register(new(User))
  // 注册HTTP处理程序
	rpc.HandleHTTP()
  // 设置监听端口
	listener, _ := net.Listen("tcp", ":8081")
  // 启动HTTP服务
	_ = http.Serve(listener, nil)
}
```

客户端

```go
type User struct {
	ID   int
	Name string
}

func main() {
  // 连接服务
	client, _ := rpc.Dial("tcp", ":8081")
	id := 1
	var user User
  // 使用call方法远程调用
	_ = client.Call("User.GetUser", id, &user)
	// 输出结果
	fmt.Println(user)
	// 关闭连接
	id = 2
	userCall := client.Go("User.GetUser", id, &user, nil)
	replyCall := <-userCall.Done
	if replyCall != nil {
		fmt.Println(user)
	}
}
```
