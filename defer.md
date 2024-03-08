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
