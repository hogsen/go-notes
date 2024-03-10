# 序列化

序列化使用 Marshal 将数据结构转为 JSON 字符串

```go
type Person struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
	City string `json:"city"`
}

func main() {
	p := Person{Name: "张三", Age: 18, City: "北京"}

	jsonByts, err := json.Marshal(p)
	if err != nil {
		log.Fatalln(err)
	}
	log.Printf("%s\n", jsonByts)
}
```

# 反序列化

使用 Unmarshal 将 JSON 字符串转为 go 的数据结构

```go
type Person struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
	City string `json:"city"`
}

func main() {
	jsonStr := `{"name":"张三","age":18,"city":"北京"}`
	var p Person
	err := json.Unmarshal([]byte(jsonStr), &p)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("%+v\n", p)
}
```
