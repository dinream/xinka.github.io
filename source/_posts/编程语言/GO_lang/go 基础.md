go个人笔记

https://blog.csdn.net/weiguang102/article/details/128501539

go 基础

https://www.runoob.com/go/go-tutorial.html

go 环境安装

https://blog.51cto.com/fish/5734211

## 零、配置

### 环境变量

1.临时添加环境变量 PATH

export PATH=$PATH:/usr/local/go/bin

2.所有用户永久添加环境变量

vim /etc/profile

末尾添加：export PATH:$PATH:/usr/local/go/bin

刷新：sourse /etc/profile

3.golang 工作目录

GOPATH 是一个环境变量，用于指定Go项目的工作目录，默认情况下，GOPATH是 $HOME是当前用户的主目录。(/root 或者 /home/user1)

获取：

go env GOPATH

更改：

编辑 .bashrc 或者 .zshrc

找到 GOPATH 这行，将其修改为 /path/to/your/gopath

  

4.go.sum 与 go.mod

`go.sum` 和 `go.mod` 是 Go 语言中用于管理模块依赖的文件。

- `go.mod` 文件：`go.mod` 文件用于定义和管理项目的模块依赖关系。它位于项目的根目录下。当你使用 `go get` 命令来安装或更新依赖包时，`go.mod` 文件会被更新。它记录了项目所依赖的模块及其版本。你可以手动编辑 `go.mod` 文件来添加、移除或升级依赖包。在构建项目时，Go 会根据 `go.mod` 文件获取所需的依赖包。
    
- `go.sum` 文件：`go.sum` 文件用于记录项目所使用模块的校验和信息。每个模块都有一个唯一的校验和，用于确保下载的模块的完整性和安全性。`go.sum` 文件会自动生成并更新，其中包含了所有依赖模块的校验和。当你构建项目时，Go 会根据 `go.sum` 文件验证模块的完整性，以确保下载的模块与之前的校验和匹配。
    

使用 `go.mod` 和 `go.sum` 文件的基本操作如下：

1. 初始化模块：在项目的根目录下执行 `go mod init` 命令，它会根据项目的路径和名称创建一个新的 `go.mod` 文件。
    
2. 添加依赖包：执行 `go get` 命令来添加所需的依赖包。例如，`go get` `github.com/example/package`。这会自动更新 `go.mod` 文件并下载依赖包。
    
3. 移除依赖包：执行 `go mod tidy` 命令来移除不再使用的依赖包。它会自动更新 `go.mod` 文件并删除不需要的依赖。
    
4. 升级依赖包：执行 `go get -u` 命令来升级依赖包到最新版本。这会自动更新 `go.mod` 文件。
    
5. 构建项目：使用 `go build` 或 `go run` 命令来构建或运行项目。Go 会根据 `go.mod` 文件下载依赖包并构建项目。
    

需要注意的是，`go.mod` 和 `go.sum` 文件对于保证项目的可复现性和依赖包的一致性非常重要。当你与他人共享项目时，确保将 `go.mod` 和 `go.sum` 文件一同共享，以便其他人可以获取相同的依赖包版本。

管理和安装 `go.mod` 和 `go.sum` 中列出的依赖项：

1. **下载依赖项**
    

当你首次克隆一个 Go 项目或者在已有项目中更新了依赖项时，可以使用以下命令来下载 `go.mod` 文件中指定的所有依赖项：

```Bash
go mod download
```

这个命令会将依赖项下载到本地的 Go 模块缓存中，但不会在项目目录中创建任何文件。

2. **同步依赖项**
    

如果你想确保项目目录下的依赖项与 `go.mod` 文件中定义的依赖项完全同步（即添加缺失的模块，删除不需要的模块，并更新 `go.sum`），可以使用：

```Bash
go mod tidy
```

`go mod tidy` 命令会添加缺少的模块，移除未使用的模块，并生成一个新的 `go.sum` 文件。这是确保 `go.mod` 和 `go.sum` 文件准确反映项目依赖项的好方法。

3. **查看依赖项**
    

如果你想查看当前项目的依赖树，可以使用：

```Bash
go mod graph
```

这个命令会打印项目的所有依赖项及其版本，帮助你理解项目依赖的结构。

4. **更新依赖项**
    

要更新项目中的某个依赖项到最新版本，可以使用：

```Bash
go get -u package@version
```

其中 `package` 是依赖项的路径，`version` 是你想要更新到的版本。如果你想更新所有依赖项到最新版本，可以使用：

```Bash
go get -u
```

但请注意，这可能会引入重大更改，因此建议仔细测试更新后的依赖项。

  

修改 proto 文件并运行 `make proto` 来更新 `proto/pkg/xxx/xxx.pb.go` 中相关生成的 go 代码

  

## 一、变量
### 一般变量
1. 变量声明
    
    1. 标准格式
        
    ```C++
    var name type // 注意：声明变量时将变量的类型放在变量的名称之后
    var a,b *int // 将 a b 同时声明为指针类型
    
    // 初始化
    var hp int = 100 // 指定类型
    var hp = 100 // 省略类型，编译器会尝试为变量推到类型
    ```
    
    3. 简短格式
        

```C++
// 名字 := 表达式
/*
1. 定义变量，同时显式初始化。
2. 不能提供数据类型
3. 只能用在函数内部。
*/

// 同时声明+初始化
i, j := 0, 1
func main() {
   x:=100
   a,s:=1, "abc"
}
// 注意  :=  左面必须是新变量
var hp int // 声明 hp 变量
hp := 10  // 再次声明并赋值  报错！ 左面没有新变量出现

// 错误！！！！其实 只要有新变量就行！
var conn net.Conn
var err error
conn, err = net.Dial("tcp", "127.0.0.1:8080")  // 不会报错
```

注意：变量的命名规则遵循骆驼命名法，即首个单词小写，每个新单词的首字母大写，例如：numShips 和 startDate 。

2. 变量类型
    
    1. bool
        
    2. string
        
        1. 反引号可以实现多行
            
            ```C++
            const str = `第一行
            第二行
            第三行
            ` 
            fmt.Println(str);  // 输出上面的内容
            
            // 注意 ！！！！！！
            // 此时所有的 转义都会失效，即 `` 内部的全部都视为 字符串一部分
            ```
            
        2. 字符串长度
            
            1. len(), 判断的是 ASCII 长度 或者是 字节长度
                
                1. 在 go 语言中， 所有字符串都是以 utf-8 的格式保存，因此 汉字 3 字节，字符 1 字节
                    
                2. 若要将汉字作为一个整体统计， 可以使用下面的函数
                    
            2. RuneCountInString() , 判断的是字符的个数，或者说是 Unicode 字符串长度
                
                ```C++
                package main
                
                import (
                        "fmt"
                        "strings"
                )
                
                // 遍历字符串
                // 按ASCII 字符
                theme := "狙击 start"
                for i := 0; i < len(theme); i++ {//使用 for 的数值循环进行遍历--
                    fmt.Printf("ascii: %c  %d\n", theme[i], theme[i])
                }// 这里 汉字为乱码
                
                // 按 Unicode  字符
                theme := "狙击 start"
                for _, s := range theme {
                    fmt.Printf("Unicode: %c  %d\n", s, s)
                }// 这里汉字正常显示
                
                
                // 字符串截取
                newtr := theme[i:]  // 截取部分包括 i
                
                // 字符串搜索
                strings.Index(strings ,substring); // 返回 下标
                strings.LastIndex(strings, substrings); // 反向搜索
                
                // 字符串修改
                        s1 := "abc"
                //     注意：字符串无法修改，只能在复制的基础上进行修改
                //  方法一  转为 []byte()
                        s2 := []byte(s1)
                        s2[1] = 'B'
                        fmt.Println(string(s2)) //aBc
                // 方法二  转为 []runej() 
                        s3 := []rune(s1)
                        s3[1] = 'B'
                        fmt.Println(string(s3)) //aBc
                 // 方法三 字符串替换
                        new_str := "ABC"
                        old_str := "abc"
                        s4 := strings.Replace(s1, old_str , new_str , 2)
                        fmt.Println(s4) //ABC
                
                // 字符串拼接
                    str1 := "hello"
                    str2 := "world"
                 // 方法一 使用 + 直接拼接
                     str3 := str1 + str2
                 // 方法二 使用 bytes.Buffer
                     var stringBuilder bytes.Buffer
                     stringBuilder.WriteString(str1)
                     stringBuilder.WriteString(str2)
                     str4 := stringBuilder.String()
                     
                ```
                
    3. int、int8、int16、int32、int64
        
    4. uint、uint8、uint16、uint32、uint64、uintptr
        
    5. byte // uint8 的别名
        
    6. rune // int32 的别名 代表一个 Unicode 码 不对 是 UTF-8
        
    7. float32、float64
        
    8. complex64、complex128 内建函数
    9. interface 接口类型
	    https://learnku.com/go/t/38843
		```go
			type Animal interface {
			    Speak() string
			}
		```
    10. map\[string\] \*sync.WaitGroup  键值对类型
3. 变量赋值
    
    ```C++
    b, a = a, b  // 这里 可以实现交换操作
    a, _ := a, _ := GetData()  // 这里 匿名变量
    ```
    
      注意：**当一个变量被声明之后，系统自动赋予它该类型的零值：****`int 为 0，float 为 0.0，bool 为 false，string 为空字符串，指针为 nil 等`**
    
      所有的内存在 Go 中都是经过初始化的。
    
4. 匿名变量不占用内存空间，不分配内存
    
5. 作用域
    1. 全局变量声明必须以 `var` 关键字开头，如果想要在外部包中使用全局变量的首字母必须大写。
    2. 特殊变量定义位置
	    ```go
	    if firstIdx, err := Func(); err != nil {
	        log.Fatal(err)
	        return nil
	    }
	    // 这里的 firstIdx 只能在 if 里面的大括号用，一旦出去就使用不了
		```
    
6. 比较
    
      只有同类型的变量才能进行比较
    
      包括 0 1 到 true false 的转换
    
7. 类型转换
    
    1. bool 不能强转为 int
        
        ```C++
        var n bool
        n := (int)n
        //会报错
        ```
        
    2. 不能获取数组某个位置的的地址
        

### 结构体变量
1. 结构体声明
	```go
	// 结构体成员变量
	type MyStruct struct{
		age int     // 注意没有 逗号
		name string
	}
	// 结构体函数
	func (ms *MyStruct) grow(int a) bool{
		ms.age += a
		return true
	}
	
	// 结构体的指针继承
	type MyStruct2 struct{
		*MyStruct  // 通过指针的方式，父亲结构体作为一个无名成员变量，
				   // 赋值时使用 父亲对象 赋值； 
				   // 使用时使用 孩子对象 直接使用（不需要先点出来父亲）
		age int     // 注意没有 逗号
		name string
	}
	```
2. 结构体实例化
	``` go
	// 方法一：var 声明
	var fan MyStruct   // 为 fan 分配内存并且零化值
	fan.age = 26
	fan.name = "fan"
	
	// 方法二：new 关键字
	// 11
	fan := &MyStruct{   // 返回一个结构体指针，带有初始值
		26,
		"fan",
	}
	// 22 
	fan := new(MyStruct) // 返回一个结构体指针，零值
	fan.name = "fan"
	fan.age = 26
	
	// 方法三：赋值初始化
	// 11
	fan := MyStruct{
		name: "fan",
		age: 26,
	}
	// 22：必须要对应结构体定义顺序，并且每一个都必须进行初始化。
	fan := MyStruct{
		26,
		"fan",
	}
	```
3. 匿名结构体
	```go
	profile := &struct {
            name string
            age   int
    }{
            name: "rat",
            age:   150,
    }
    // 打印值
    fmt.Printf("使用'%%+v' %+v\n", profile)
    fmt.Printf("使用'%%#v' %#v\n", profile)
    fmt.Printf("使用'%%T' %T\n", profile)
	```
4. 结构体返回值
	```go
	func newStruct() * MyStruct{
		// 此处替换为三种不同方式
	    var fan MyStruct   // 为 fan 分配内存并且零化值
		fan.age = 26
		fan.name = "fan"
		fmt.Printf("函数内变量 fan 的地址为：%p\n", &fan)
		return &fan
	}
	
	// 测试
	func main() {
	    fan := newStruct()
	    fmt.Printf("函数外变量 fan 的地址为：%p\n", fan)
		fmt.Printf("fan使用'%%+v' %+v\n", fan)
		fan.age = 18
		fmt.Printf("fan使用'%%+v' %+v\n", fan)
		// 方法一：地址相同，可更改 
		// 方法二11：地址相同，可更改
		// 方法二22：地址相同，可更改
		// 方法三11：地址相同，可更改
		// 方法三22：地址相同，可更改
	}
	```
### 键值对类型
map\[string\] \*sync.WaitGroup  键值对类型
1. 键值对变量
```go
package main

import (
        "fmt"
)

type MyStruct struct {
    a, b uint64
}
	
func main() {
	// !! 定义与使用 
    //  初始 正常运行
    stru := make(map[uint64]*MyStruct)
    stru[2] = &MyStruct{
        1,1,
    }
    stru[2].a = 1
    stru[2].b  = 1
    fmt.Printf("值为：%+v",stru[2])
    
    //  这样也可以
    var stru map[uint64]*MyStruct 
    stru = make(map[uint64]*MyStruct) // 没有这行运行不通
    stru[2] = &MyStruct{   // 去掉这行也不行
        1,1,
    }  
    stru[2].a = 1
    stru[2].b  = 1
    fmt.Printf("值为：%+v",stru[2])
    
    // 结论： map 必须通过 make 且 键值对必须 初始化。才能修改
	
	// 判断键值是否存在
	if _, ok := stru[2]; !ok {
		return
	}
	
}
	
```
2. 键值对作为结构体成员变量
```go
package main

import (
        "fmt"
)

type MyStruct struct {
    a, b uint64
}
	
type MS struct{
    stu  map[uint64]*MyStruct
}
func main() {
    var stru MS
    stru.stu = make(map[uint64]*MyStruct) // 1 
       stru.stu[2]  = & MyStruct{ // 2 , 1 2 这两步都是必须的，且可以多次执行不会冲突
           1,1,
       }
    stru.stu[2].a =1
    stru.stu[2].b =1

    fmt.Printf("值为：%+v",stru.stu[2])
    
}
```

### 数组
1. 定义 
	```go
	// 构造特定大小的数组，初始会有 size 的大小，每一个元素会有一个初始值，默认0
	ids := make([]uint64, size)
	// 构造大小为 0 的数组
	var   ids  []uint64
	//! 注意两种 定义方式 都可以用下面的方法增加元素 
	// 增加元素
	ids = append(ids,1)
	```
## 二、输入、输出

1. fmt.Sprintf(格式化样式 ， 参数列表)
    
    1. 和 C 语言类似 printf
        
2. fmt.Println(str)
    

```C++
package main

import (
        "fmt"
)

func main() {
        var progress = 2
        var target = 8
        // 两参数格式化
        title := fmt.Sprintf("已采集%d个药草, 还需要%d个完成任务", progress, target)
        fmt.Println(title)
        pi := 3.14159
        // 按数值本身的格式输出
        variant := fmt.Sprintf("%v %v %v", "月球基地", pi, true) //月球基地 3.14159 true
        fmt.Println(variant)
        // 匿名结构体声明, 并赋予初值
        profile := &struct {
                Name string
                HP   int
        }{
                Name: "rat",
                HP:   150,
        }
        // 双百分号表示转义
        fmt.Printf("使用'%%+v' %+v\n", profile)  // 使用'%+v' &{Name:rat HP:150}
        fmt.Printf("使用'%%#v' %#v\n", profile)  // 使用'%#v' &struct { Name string; HP int }{Name:"rat", HP:150}
        fmt.Printf("使用'%%T' %T\n", profile)  // 使用'%T' *struct { Name string; HP int }
}
```

3. 动 词 功 能
    
      %v 按值的本来值输出
    
      %+v 在 %v 基础上，对结构体字段名和值进行展开
    
      %#v 输出 Go 语言语法格式的值
    
      %T 输出 Go 语言语法格式的类型和值
	
      \%% 输出 % 本体
    
      %b 整型以二进制方式显示
    
      %o 整型以八进制方式显示
    
      %d 整型以十进制方式显示
    
      %x 整型以十六进制方式显示
    
      %X 整型以十六进制、字母大写方式显示
    
      %U Unicode 字符
    
      %f 浮点数
    
      %p 指针，十六进制方式显示
    


## 四、 库

#### flag

功能：Go 语言标准库中的一个包，用于解析命令行参数

关键函数：

flag.type(arg_name,arg_default_value,arg_discription) -> value

flag.Parse() ：解析命令获得各个参数的值

实例：

```Bash
func main() {
        // 定义命令行参数
        num1 := flag.Int("num1", 0, "第一个整数")
        num2 := flag.Int("num2", 0, "第二个整数")

        // 解析命令行参数
        flag.Parse()
        
        // 计算和
        sum := *num1 + *num2

        // 输出结果
        fmt.Println("和:", sum)
}
```

  

## 三、常见技巧
### 常见调用
1. 获取变量类型 
	varb.(type)
2. 获取数组长度
	len(arr)
### 键值对KV

go语言中可以使用`map[string]interface{}`类型来实现，键的索引和值的多种类型

```Bash
package main
import (
        "fmt"
)

func main() {
        // 定义
        data := map[string]interface{}{
                "myname": "电子科技大学@2023",
                "tasks":  []string{"task 1", "task 2", "task 3"},
                "age":    25,
                "isStudent": true,
        }
        // 增加
        data["newKey"] = "New Value"
        
        // 删除
        delete(data, "age") // 删除 data 中键为 “age” 的键值对
        // 查找
        key := "tasks"
        value, found := data[key]
        if found {
                fmt.Printf("Value of key '%s': %v\n", key, value)
        } else {
                fmt.Printf("Key '%s' not found\n", key)
        }
        
        // json 解析为键值对 KV 形式
        str := `{"name": "@2023"}`
        var data map[string]interface{}
        errr := json.Unmarshal([]byte(str), &data)
        if errr != nil {
            // 处理解析错误
        }
        fmt.Println(data["name"])
        
        // 多个键值对支持
            // 局部
            data := make(map[string]interface{})
            // 全局
            var data map[string]interface{} = make(map[string]interface{})
            
        // 获取键值对的 K V
        for k, v := range data {
             existingData[k] = v
        }
        
        // 将 KV对 处理为 json格式
        jsonData, err := json.Marshal(data)
        if err != nil {
                // 处理编码错误
        }
        
        // 将 K V 处理为 json格式
            // 方法 一：
            // fmt.Fprintf(w, "{'%s':'%v'}", param, value)
            
            // 方法 二：
            jsonData, err := json.Marshal(map[string]interface{}{
                param: value,
            })
            if err != nil {
                // 处理编码错误
            }
       
        
        //
}
```

### 数据排序
对数据集合（包括自定义数据类型的集合）排序需要实现 sort.Interface 接口的三个方法，以下为该接口的定义：

```go
type Interface interface {
        // 获取数据集合元素个数
        Len() int
        // 如果 i 索引的数据小于 j 索引的数据，返回 true 且不会调用下面的 Swap()，即数据升序排序。
        Less(i, j int) bool
        // 交换 i 和 j 索引的两个元素的位置
        Swap(i, j int)
}
```
数据集合实现了这三个方法后，即可调用该包的 Sort() 方法进行排序。 Sort() 方法定义如下：
```go
func Sort(data Interface)
```

例子：
```go
// 定义
type messageSlice []pb.Message

func (s messageSlice) Len() int           { return len(s) }
func (s messageSlice) Less(i, j int) bool { return fmt.Sprint(s[i]) < fmt.Sprint(s[j]) }
func (s messageSlice) Swap(i, j int)      { s[i], s[j] = s[j], s[i] }

// 使用
sort.Sort(messageSlice(msgs)) // msgs 为 []pb.Message 类型， 先强转再使用

```


### 数据比较

reflect包中的DeepEqual函数完美的解决了比较问题。

函数签名：

func DeepEqual(a1, a2 interface{}) bool

文档中对该函数的说明：
DeepEqual函数用来判断两个值是否深度一致：除了类型相同；在可以时（主要是基本类型）会使用==；但还会比较array、slice的成员，map的键值对，结构体字段进行深入比对。map的键值对，对键只使用==，但值会继续往深层比对。DeepEqual 函数可以正确处理循环的类型。函数类型只有都会nil时才相等；空切片不等于nil切片；还会考虑array、slice的长度、map键值对数。
示例：

func main() {
	m1 := map[int]interface{}{1: []int{1, 2, 3}, 2: 3, 3: "a"}
	m2 := map[int]interface{}{1: []int{1, 2, 3}, 2: 3, 3: "a"}
	if reflect.DeepEqual(m1, m2) {
		fmt.Println("相等")
	}
}

uestc.leemen.org