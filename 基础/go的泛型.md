# go 中的泛型限制

1. 匿名结构体和匿名函数不支持泛型
2. 不支持类型断言
3. 不支持泛型方法，只能通过 receiver 来实现方法的泛型处理
4. ~后面的类型必须为基本类型，不能为接口类型

# 类型约束

使用接口进行类型约束
|表示的是联合类型，~表示支持类型的衍生类型,~int64 表示基础类型为 int64 的所有类型
类型有多行表示取交集

```go
type number interface{
  int8|int32|~int64
}
//这里有两行，取交集T的类型是int16
type T interface{
  int8|int16
  int16
}
```

# 衍生类型与类型别名

```go
// 没有等号基于基础类型创建一个新类型
type uid int64
// 有等号，只是取了一个别名，还是同一种类型
type myint = int64
```

# 内置类型

1. comparable 可比较的类型
   comparable 只支持==和 !=

# 泛型定义

1. 泛型切片

```go
// 定义泛型切片
type List[T any] []T

// 定义泛型Map
type Map[K comparable, V any] map[K]V
```
