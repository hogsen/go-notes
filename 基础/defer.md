# 函数延迟执行

defer 声明的匿名函数会在当前函数/方法退出之前执行，及 return 之前执行

```go
func DoDefer()(res int){
  defer func(){res++}() // defer修改的是要返回的数据
  return 1 //2
}
// 函数返回的是res
// res=1
// res++
// return res
```

```go
func DoDefer()(res int){
  v:=100
  defer func(){v++}() // 虽然v增加了，但是它与res没有任何关系
  return v // 100
}
// 函数返回的始终是res
// v:=100
// res=v
// v++
// return res
```

# panic

程序出现异常就会触发 panic,也可以主动调用 panic()函数
当出现 panic,可以执行 recover()函数，让程序恢复
recover()必须在 defer 中执行
recover 可以捕获异常，如果没有出现 panic,调用 recover 会返回 nil

```go
func TestA() {
	defer func() {
		fmt.Println("TestA:defer")
	}()
	defer func() {
		msg := recover()
		fmt.Println(msg) //TestA:pannic
	}()
	panic("TestA:pannic")
}
func TestB() {
	fmt.Println("TestB")
}

func main() {
	TestA()
	TestB()
	fmt.Println("main:end")
}
```
