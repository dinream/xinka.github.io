
	
# 常见库
## Flag 包
### flag包基本使用
[官方文档](https://studygolang.com/pkgdoc)]
[参考文档](https://www.liwenzhou.com/posts/Go/flag/)


#### 导入flag包

```go
import flag
```

#### flag参数类型

flag包支持的命令行参数类型有`bool`、`int`、`int64`、`uint`、`uint64`、`float` `float64`、`string`、`duration`。

| flag参数     | 有效值                                                                          |
| ---------- | ---------------------------------------------------------------------------- |
| 字符串flag    | 合法字符串                                                                        |
| 整数flag     | 1234、0664、0x1234等类型，也可以是负数。                                                  |
| 浮点数flag    | 合法浮点数                                                                        |
| bool类型flag | 1, 0, t, f, T, F, true, false, TRUE, FALSE, True, False。                     |
| 时间段flag    | 任何合法的时间段字符串。如"300ms"、"-1.5h"、“2h45m”。合法的单位有"ns"、“us” /“µs”、“ms”、“s”、“m”、“h”。 |

#### 定义命令行flag参数

有以下两种常用的定义命令行`flag`参数的方法。

### flag.Type()

基本格式如下：

`flag.Type(flag名, 默认值, 帮助信息)*Type` 例如我们要定义姓名、年龄、婚否三个命令行参数，我们可以按如下方式定义：

```go
name := flag.String("name", "张三", "姓名")
age := flag.Int("age", 18, "年龄")
married := flag.Bool("married", false, "婚否")
delay := flag.Duration("d", 0, "时间间隔")
```

需要注意的是，此时`name`、`age`、`married`、`delay`均为对应类型的指针。

#### flag.TypeVar()

基本格式如下： `flag.TypeVar(Type指针, flag名, 默认值, 帮助信息)` 例如我们要定义姓名、年龄、婚否三个命令行参数，我们可以按如下方式定义：

```go
var name string
var age int
var married bool
var delay time.Duration
flag.StringVar(&name, "name", "张三", "姓名")
flag.IntVar(&age, "age", 18, "年龄")
flag.BoolVar(&married, "married", false, "婚否")
flag.DurationVar(&delay, "d", 0, "时间间隔")
```

#### flag.Parse()

通过以上两种方法定义好命令行flag参数后，需要通过调用`flag.Parse()`来对命令行参数进行解析。

支持的命令行参数格式有以下几种：

- `-flag xxx` （使用空格，一个`-`符号）
- `--flag xxx` （使用空格，两个`-`符号）
- `-flag=xxx` （使用等号，一个`-`符号）
- `--flag=xxx` （使用等号，两个`-`符号）

其中，布尔类型的参数必须使用等号的方式指定。

Flag解析在第一个非flag参数（单个"-“不是flag参数）之前停止，或者在终止符”–“之后停止。

#### flag其他函数

```go
flag.Args()  ////返回命令行参数后的其他参数，以[]string类型
flag.NArg()  //返回命令行参数后的其他参数个数
flag.NFlag() //返回使用的命令行参数个数
```

#### 完整示例

##### 定义

```go
func main() {
	//定义命令行参数方式1
	var name string
	var age int
	var married bool
	var delay time.Duration
	flag.StringVar(&name, "name", "张三", "姓名")
	flag.IntVar(&age, "age", 18, "年龄")
	flag.BoolVar(&married, "married", false, "婚否")
	flag.DurationVar(&delay, "d", 0, "延迟的时间间隔")

	//解析命令行参数
	flag.Parse()
	fmt.Println(name, age, married, delay)
	//返回命令行参数后的其他参数
	fmt.Println(flag.Args())
	//返回命令行参数后的其他参数个数
	fmt.Println(flag.NArg())
	//返回使用的命令行参数个数
	fmt.Println(flag.NFlag())
}
```

##### 使用

命令行参数使用提示：

```bash
$ ./flag_demo -help
Usage of ./flag_demo:
  -age int
        年龄 (default 18)
  -d duration
        时间间隔
  -married
        婚否
  -name string
        姓名 (default "张三")
```

正常使用命令行flag参数：

```bash
$ ./flag_demo -name 沙河娜扎 --age 28 -married=false -d=1h30m
沙河娜扎 28 false 1h30m0s
[]
0
4
```

使用非flag命令行参数：

```bash
$ ./flag_demo a b c
张三 18 false 0s
[a b c]
3
0
```




## Log 包
日志相关，[参考](ttps://blog.csdn.net/cold___play/article/details/130744302)

### 基本结构
常量列表：

- Ldate         日期 年/月/日
- Ltime         时间 时:分:秒
- Lmicroseconds 时间 .毫秒于Ltime之后
- Llongfile     完整文件名:行号
- Lshortfile    文件名，此标志位优先于 Llongfile
- LstdFlags     = Ldate 并且 Ltime

功能说明：标志位常量控制日志格式。

示例：
```go
func main() {
	log.SetFlags(log.Lshortfile | log.LstdFlags)
	log.Println("log:") //2023/05/18 12:09:57 constantsDemo.go:15: log:
}
```

函数列表:
```go
func Fatal(v ...interface{})
func Fatalf(format string, v ...interface{})
func Fatalln(v ...interface{})
func Flags() int
func Panic(v ...interface{})
func Panicf(format string, v ...interface{})
func Panicln(v ...interface{})
func Prefix() string
func Print(v ...interface{})
func Printf(format string, v ...interface{})
func Println(v ...interface{})
func SetFlags(flag int)
func SetOutput(w io.Writer)
func SetPrefix(prefix string)
func New(out io.Writer, prefix string, flag int) *Logger
func (l *Logger) Fatal(v ...interface{})
func (l *Logger) Fatalf(format string, v ...interface{})
func (l *Logger) Fatalln(v ...interface{})
func (l *Logger) Flags() int
func (l *Logger) Output(calldepth int, s string) error
func (l *Logger) Panic(v ...interface{})
func (l *Logger) Panicf(format string, v ...interface{})
func (l *Logger) Panicln(v ...interface{})
func (l *Logger) Prefix() string
func (l *Logger) Print(v ...interface{})
func (l *Logger) Printf(format string, v ...interface{})
func (l *Logger) Println(v ...interface{})
func (l *Logger) SetFlags(flag int)
func (l *Logger) SetPrefix(prefix string)
```
### 自定义 logger:
```go
func New(out io.Writer, prefix string, flag int) *Logger
```
参数列表：
	out 输出目标
	prefix 输出前缀
	flag 格式配置标识值
返回值：
	自定义的logger
功能说明：
	这个方法用来自定义logger，指定输出目标、格式等
示例：
```go
func main() {
	l := log.New(os.Stderr, "logger", log.Ldate)
	l.Println("log to stderr sample")//logger2023/05/18 log to stderr sample
}
```

###  Fatal系列函数
#### 1.1 func Fatal(v …interface{})
参数列表：
	v 待输出参数列表
返回值：
	无
功能说明：
	打印日志并退出。相当于调用Print()并os.Exit(1)
#### 1.2 func Fatalf(format string, v …interface{})
参数列表：
	format 输出格式
	v 带输出参数列表
返回值：
	无
功能说明：
	按格式输出日志，并退出。相当于调用Printf()并调用os.Exit(1)
#### 1.3 func Fatalln(v …interface{})
参数列表：
	v
返回值：
	无
功能说明：
	打印一行日志并退出。相当于调用Println()并os.Exit(1)
#### 1.4 func (l \*Logger) Fatal(v …interface{})
参数列表：
	v 待输出参数列表
返回值：
	无
功能说明：
	打印日志并退出。相当于调用l.Print()并os.Exit(1)
#### 1.5 func Fatalf(format string, v …interface{})
参数列表：
	format 输出格式
	v 带输出参数列表
返回值：
	无
功能说明：
	按格式输出日志，并退出。相当于调用l.Printf()并调用os.Exit(1)
#### 1.6 func (l \*Logger) Fatalln(v …interface{})
参数列表：
	v
返回值：
	无
功能说明：
	打印一行日志并退出。相当于调用l.Println()并os.Exit(1)
#### 示例
```go


func main() {
	age := 25
	log.Fatal("Hi & Bye ! Age = ", age) // this will print "Hi & Bye ! Age = 25"
	log.Println("This will not be called.")

	name := "golang"
	log.Fatalf("%8d,%8s", 23, name) //2013/03/10 16:08:49       23,  golang

	log.Fatalln("bye!") //2013/03/10 16:14:54 bye!\n

	l := log.New(os.Stdout, "", log.LstdFlags)
	age = 25
	l.Fatal("Hi & Bye ! Age = ", age) // this will print "Hi & Bye ! Age = 25"
	l.Println("This will not be called.")

	l = log.New(os.Stdout, "", log.LstdFlags)
	//l.Fatalf("%s", "hello")
	name = "golang"
	l.Fatalf("%8d,%8s", 23, name) //2013/03/10 16:08:49       23,  golang

	l = log.New(os.Stdout, "", log.LstdFlags)
	l.Fatalln("bye!") //2013/03/10 16:14:54 bye!\n
}
```
### Flags系列函数
#### 2.1 func Flags() int
参数列表：
	无
返回值：
	默认logger的配置值
功能说明：
	返回默认logger配置值。
#### 2.2 func (l \*Logger) Flags() int
参数列表：
	无
返回值：
	当前logger的配置值
功能说明：
	返回当前logger配置值。
#### 2.3 func SetFlags(flag int)
参数列表：
	flag logger配置值
返回值：
	无
功能说明：
	这个方法用来设置标准logger的配置，默认为3（logger.LstdFlags）
#### 2.4 func (l \*Logger) SetFlags(flag int)
参数列表：
	flag logger配置值
返回值：
	无
功能说明：
	这个方法用来设置标准logger的配置，默认为3（logger.LstdFlags）
#### 示例
```go
func main() {
	fmt.Println("standard flags :", log.Flags()) //standard flags : 3
	//the flags constants
	fmt.Println(log.Ldate)         //1
	fmt.Println(log.Ltime)         //2
	fmt.Println(log.Lmicroseconds) //4
	fmt.Println(log.Llongfile)     //8
	fmt.Println(log.Lshortfile)    //16
	fmt.Println(log.LstdFlags)     //LstdFlags     = Ldate | Ltime   3

	l := log.New(os.Stdout, "", log.LstdFlags)
	fmt.Println("logger l's flags :", l.Flags()) //logger l's flags : 3

	log.Println(log.Flags()) //2013/03/10 17:46:53 3
	log.SetFlags(log.Ldate)
	log.Println(log.Flags()) //2013/03/10 1
	log.SetFlags(log.LstdFlags | log.Lshortfile)
	log.Println(log.Flags()) //2013/03/10 17:46:53 setflags.go:17: 19

	l = log.New(os.Stdout, "", log.LstdFlags)
	l.Println(l.Flags()) //2013/03/10 17:46:53 3
	l.SetFlags(log.Ldate)
	l.Println(l.Flags()) //2013/03/10 1
	l.SetFlags(log.LstdFlags | log.Lshortfile)
	l.Println(l.Flags()) //2013/03/10 17:46:53 setflags.go:17: 19
}
```

### Panic系列函数
#### 3.1 func (l \*Logger) Panic(v …interface{})
参数列表：
	v 待输出参数列表
返回值：
	无
功能说明：
	这个方法相当于调用l.Print()及panic()
#### 3.2 func (l \*Logger) Panicf(format string, v …interface{})
参数列表：
	format 输出格式
	v 待输出参数列表
返回值：
	无
功能说明：
	相当于调用l.Printf()，之后调用panic()
#### 3.3 func (l \*Logger) Panicln(v …interface{})
参数列表：
	v 待输出参数列表
返回值：
	无
功能说明：
	相当于调用l.Println()并调用panic()
#### 3.4 func Panic(v …interface{})
参数列表：
	v 待输出参数列表
返回值：
	无
功能说明：
	这个方法相当于调用Print()及panic()
#### 3.5 func Panicf(format string, v …interface{})
参数列表：
	format 输出格式
	v 待输出参数列表
返回值：
	- 无
功能说明：
	相当于调用Printf()，之后调用panic()
#### 3.6 func Panicln(v …interface{})
参数列表：
	v 待输出参数列表
返回值：
	无
功能说明：
	相当于调用Println()并调用panic()
#### 示例
```go
func main() {
	l := log.New(os.Stdout, "", log.LstdFlags)
	defer func() {
		if err := recover(); err != nil {
			fmt.Println(err) //output : "call panic and stop"
			handleException()
		}
	}()
	l.Panic("call panic and stop")
	log.Println("this will not be called.")

	defer func() {
		if err := recover(); err != nil {
			if err == "3q" {
				log.Println("you are welcome")
			}
		}
	}()
	l = log.New(os.Stdout, "", log.LstdFlags)
	l.Panicf("%d%s", 3, "q")

	defer func() {
		if err := recover(); err != nil {
			if err == "3q\n" {
				log.Println("you are welcome")
			}
		}
	}()
	l = log.New(os.Stdout, "", log.LstdFlags)
	l.Panicln("3q")
}

func handleException() {
	log.Println("recovering...")
}
```

### Print系列函数
#### 4.1 func (l \*Logger) Print(v …interface{})
参数列表：
	v 待输出参数列表
返回值：
	无
功能说明：
	输出日志到logger。参数处理方式同fmt.Print
#### 4.2 func (l \*Logger) Printf(format string, v …interface{})
参数列表：
	format 输出格式
	v 待输出参数列表
返回值：
	无
功能说明：
	调用l.Output输出日志到logger l。参数处理方式同fmt.Printf
#### 4.3 func (l \*Logger) Println(v …interface{})
参数列表：
	v 待输出参数列表
返回值：
	无
功能说明：
	调用Output打印日志到当前logger，参数处理方式同fmt.Println
####  4.4 func Print(v …interface{})
参数列表：
	v 待输出参数列表
返回值：
	无
功能说明：
	输出日志到标准logger。参数处理方式同fmt.Print
#### 4.5 func Printf(format string, v …interface{})
参数列表：
	format 输出格式
	v 待输出参数列表
返回值：
	无
功能说明：
	调用Output输出日志到标准logger。参数处理方式同fmt.Printf
#### 4.6 func Println(v …interface{})
参数列表：
	v
返回值：
	无
功能说明：
	调用Output打印日志到标准logger，参数处理方式同fmt.Println
#### 示例
```go
func main() {
	l := log.New(os.Stdout, "", log.LstdFlags)
	l.Print("string", 1, 2.3) //2013/03/10 17:26:06 string1 2.3

	l = log.New(os.Stdout, "", log.LstdFlags)
	l.Printf("%s", "hello") //hello
	name := "golang"
	l.Printf("%8d,%8s", 23, name) //2013/03/10 16:08:49       23,  golang

	l = log.New(os.Stdout, "", log.LstdFlags)
	l.Println("hello") //2013/03/10 17:35:28 hello\n

	log.Print("string", 1, 2.3) //2013/03/10 17:26:06 string1 2.3

	log.Printf("%s", "hello") //hello
	name = "golang"
	log.Printf("%8d,%8s", 23, name) //2013/03/10 16:08:49       23,  golang

	log.Println("hello") //2013/03/10 17:35:28 hello\n
}
```

### 其它函数
#### 5.1 func (l \*Logger) Prefix() string
参数列表：
	无
返回值：
	logger前缀，字符串类型
功能说明：
	返回当前logger的输出前缀
#### 5.2 func Prefix() string
参数列表：
	无
返回值：
	标准logger前缀，字符串类型
功能说明：
	返回标准logger的输出前缀
#### 5.3 func SetPrefix(prefix string)
参数列表：
	prefix 前缀
返回值：
	无
功能说明：
	设置logger的输出前缀
#### 5.4 func SetPrefix(prefix string)
参数列表：
	prefix 前缀
返回值：
	无
功能说明：
	设置logger的输出前缀
#### 5.5 func (l \*Logger) Output(calldepth int, s string) error
参数列表：
	calldepth 深度
	s 字符串
返回值：
	error 错误
功能说明：
	输出日志事件。字符串s包含待打印内容，跟在预定义的prefix后面，并且根据flags设置会有区分。如果s末尾没有换行符，这个方法会默认加上一个。calldepth目前预定义均为2,以后会用来支持通用场景，支持其他值配置。（本人注：日志输出不建议直接使用该方法）
#### 5.6 func SetOutput(w io.Writer)
参数列表：
	w 目标流，io.Writer类型
返回值：
	无
功能说明：
	设置标准logger的输出目标
#### 示例
```go
func main() {
	l := log.New(os.Stdout, "", log.LstdFlags)
	fmt.Print(l.Prefix()) //this will print nothing
	l.Println(1)          //2013/03/10 17:02:05 1
	l.SetPrefix("log:")
	fmt.Println(l.Prefix()) //log:
	l.Println(2)            //log:2013/03/10 17:02:05 2

	fmt.Print(log.Prefix()) //this will print nothing
	log.Println(1)          //2013/03/10 17:02:05 1
	log.SetPrefix("log:")
	fmt.Println(log.Prefix()) //log:
	log.Println(2)            //log:2013/03/10 17:02:05 2

	l = log.New(os.Stdout, "", log.LstdFlags)
	fmt.Print(l.Prefix()) //this will print nothing
	l.Println(1)          //2013/03/10 17:02:05 1
	l.SetPrefix("log:")
	fmt.Println(l.Prefix()) //log:

	l.Println(2) //log:2013/03/10 17:02:05 2

	fmt.Print(log.Prefix()) //this will print nothing
	log.Println(1)          //2013/03/10 17:02:05 1
	log.SetPrefix("log:")
	fmt.Println(log.Prefix()) //log:
	log.Println(2)            //log:2013/03/10 17:02:05 2

	l = log.New(os.Stdout, "log->", log.Ldate)
	l.Output(2, "log output")

	file, err := os.OpenFile("sample.txt", os.O_WRONLY, 0666)
	if err != nil {
		panic(err)
	}
	defer file.Close()
	log.SetOutput(file)
	log.Println("log to file")
}
```


## Badger 包
[参考](https://juejin.cn/post/6844903814571491335)
[官方文档](https://pkg.go.dev/github.com/dgraph-io/badger#Txn)
badger是一个纯Go实现的快速的嵌入式K/V数据库，针对LSM tree做了优化。
### 安装

`$ go get github.com/dgraph-io/badger/...`

### 数据库

打开一个数据库

```go
opts := badger.DefaultOptions
opts.Dir = "/tmp/badger" 
opts.ValueDir = "/tmp/badger" 
db, err := badger.Open(opts) 
if err != nil {
	log.Fatal(err) 
} 
defer db.Close()
```
### 存储

#### 存储kv

使用 Txn.Set()方法

```go
err := db.Update(func(txn *badger.Txn) error {   
	err := txn.Set([]byte("answer"), []byte("42"))   
	return err 
})
```
#### 批量设置
```go
wb := db.NewWriteBatch() 
defer wb.Cancel() 
for i := 0; i < N; i++ {
	err := wb.Set(key(i), value(i), 0) // Will create txns as needed. 
	handle(err) 
} 
handle(wb.Flush()) // Wait for all txns to finish.
```
WriteBatch不允许任何读取。对于读-修改-写，应该使用事务API。

##### 设置生存时间 TTL

Badger 允许在键上设置一个可选的生存时间 (TTL) 值。一旦 TTL 结束，KEY 将不再是可检索的，并且将进行垃圾收集。TTL 可以使用 Txn.SetWithTTL() 设置为一个`time.Duration`的值

##### 设置元数据

`Txn.SetWithMeta()` 设置用户元数据

使用 `Txn.SetEntry()` 可以一次性设置 key, value, user metatadata 和 TTL

##### 遍历 keys

要遍历键，我们可以使用迭代器，可以使用 `Txn.NewIterator()`方法获得迭代器。迭代按字节字典排序顺序进行。
```go
err := db.View(func(txn *badger.Txn) error { // badger 的 view 方法创建一个只读事务
	opts := badger.DefaultIteratorOptions   
	opts.PrefetchSize = 10   
	it := txn.NewIterator(opts)   
	defer it.Close()   
	for it.Rewind(); it.Valid(); it.Next() {
	    item := it.Item() 
	    k := item.Key()     
	    err := item.Value(func(v []byte) error {
	        fmt.Printf("key=%s, value=%s\n", k, v)       
	        return nil     
	        })     
	    if err != nil {       
		    return err
		}   
	}   
	return nil 
})
```
###### 前缀扫描

要遍历键前缀，可以将 Seek() 和 ValidForPrefix() 组合使用：（这里的前缀是键值的）
```go
db.View(func(txn *badger.Txn) error {
	it := txn.NewIterator(badger.DefaultIteratorOptions)
	defer it.Close()   
	prefix := []byte("1234")   
	for it.Seek(prefix); it.ValidForPrefix(prefix); it.Next() {
		item := it.Item()    
		k := item.Key()     
		err := item.Value(func(v []byte) error {
			fmt.Printf("key=%s, value=%s\n", k, v)       
			return nil     
		})     
		if err != nil {       
			return err     
		}   
	}   
	return nil 
})
```
###### 键的遍历

Badger支持一种独特的迭代模式，称为只有键的迭代。它比常规迭代快几个数量级，因为它只涉及对 lsm 树的访问，而 lsm 树通常完全驻留在 RAM 中。要启用只有键的迭代，您需要设置 IteratorOptions 。PrefetchValues 字段为 false 。这还可以用于在迭代期间对选定的键执行稀疏读取，只在需要时调用 item.Value() 。(获取键，而不获取对应的值)
```go
err := db.View(func(txn *badger.Txn) error {   
	opts := badger.DefaultIteratorOptions   
	opts.PrefetchValues = false   
	it := txn.NewIterator(opts)   
	defer it.Close()   
	for it.Rewind(); it.Valid(); it.Next() {
		item := it.Item()     
		k := item.Key()     
		fmt.Printf("key=%s\n", k)   
	}   
	return nil 
})
```

### 数据流

Badger 提供了一个流框架，它可以并发地遍历数据库的全部或部分，将数据转换为自定义键值，并连续地将数据流输出，以便通过网络发送、写入磁盘，甚至写入 Badger。这是比使用单个迭代器更快的遍历 Badger 的方法。Stream 在管理模式和正常模式下都支持Badger 。
```go
stream := db.NewStream() 
// db.NewStreamAt(readTs) for managed mode. 
// -- Optional settings 
stream.NumGo = 16          // Set number of goroutines to use for iteration.
stream.Prefix = []byte("some-prefix") // Leave nil for iteration over the whole DB. 
stream.LogPrefix = "Badger.Streaming" // For identifying stream logs. Outputs to Logger. 
// ChooseKey is called concurrently for every key. If left nil, assumes true by default. 
stream.ChooseKey = func(item *badger.Item) bool {   
	return bytes.HasSuffix(item.Key(), []byte("er")) 
	// 这个例子中判断后缀 “er” 
} 
// KeyToList is called concurrently for chosen keys. This can be used to convert 
// Badger data into custom key-values. If nil, uses stream.ToList, a default 
// implementation, which picks all valid key-values. 
stream.KeyToList = nil 
// -- End of optional settings. 
// Send is called serially, while Stream.Orchestrate is running. 用于序列化并处理流操作的结果
stream.Send = func(list *pb.KVList) error {   
	return proto.MarshalText(w, list) // Write to w. 
	} 
// Run the stream 
if err := stream.Orchestrate(context.Background()); err != nil {   
	return err 
} // Done.
```


### 删除一个key

使用`Txn.Delete()` 方法删除一个key

### 获取 key value

通过 txn.Get 获取 value
```go
err := db.View(func(txn *badger.Txn) error {   
	item, err := txn.Get([]byte("answer"))   
	handle(err)   
	var valNot, valCopy []byte   
	err := item.Value(func(val []byte) error {     
	// This func with val would only be called if item.Value encounters no error.     
	// Accessing val here is valid.     
	fmt.Printf("The answer is: %s\n", val)     
	// Copying or parsing val is valid.     
	valCopy = append([]byte{}, val...)     // 三个点（`...`）的语法，表示将切片 `val` 展开，将其中的元素逐个添加到新的切片 `valCopy` 中。
	// Assigning val slice to another variable is NOT OK.     
	valNot = val // Do not do this.     
	return nil   
})   
handle(err)   // DO NOT access val here. It is the most common cause of bugs.
fmt.Printf("NEVER do this. %s\n", valNot)   
// You must copy it to use it outside item.Value(...).   
fmt.Printf("The answer is: %s\n", valCopy)   
// Alternatively, you could also use item.ValueCopy().   
valCopy, err = item.ValueCopy(nil)   
handle(err)   
fmt.Printf("The answer is: %s\n", valCopy)   
return nil 
})
```

如果不存在 `Txn.Get()` 将会返回一个 `ErrKeyNotFound` 错误

请注意，Get()返回的值只在事务打开时有效。如果需要在事务外部使用值，则必须使用copy() 将其复制到另一个字节片。

### 事务

##### 只读事务

只读事务使用 DB.View()方法
```go
err := db.View(func(txn *badger.Txn) error {  
	// Your code here…   
	return nil 
})
```

##### 读写事务锁

读写事务可以使用 DB.Update()方法
```go
err := db.Update(func(txn *badger.Txn) error {   
	// Your code here…   
	return nil 
})
```

##### 手动管理事务

直接使用`DB.NewTransaction()`函数，手动创建和提交事务。它接受一个布尔参数来指定是否需要读写事务。对于读写事务，需要调用`Txn.Commit()`来确保事务已提交。对于只读事务，调用 `txn.reject()`就可以了。`commit()`也在内部调用 `txn .reject()`来清除事务，因此只需调用Txn.Commit()就足以执行读写事务。

但是，如果您的代码由于某种原因(出错)没有调用`Txn.Commit()`。就需要在defer中调用 `txn . reject()`
```go
// Start a writable transaction. 
txn := db.NewTransaction(true) 
defer txn.Discard() 
// Use the transaction... 
err := txn.Set([]byte("answer"), []byte("42")) 
if err != nil {     return err } 
// Commit the transaction and check for error. 
if err := txn.Commit(); err != nil {     
	return err 
}
```



## Error 包
```go
// 创建一个新 的 error 类型 数据
errors.New("dest id is not cur id") 
```

## Server 常用

1. 获取 URL 的路径参数
    
      比如： 前端访问：http://127.0.0.1:8080/haha ， 如何获取 haha 这个字符串？ 看代码：
    
    ```Bash
    package main
    
    import (
            "fmt"
            "net/http"
    )
    
    func handler(w http.ResponseWriter, r *http.Request) {
            // 从请求URL中获取路径参数
            param := r.URL.Path[len("/"):]
    
            // 输出获取到的字符串
            fmt.Println(param)
    
            // 在响应中返回获取到的字符串
            fmt.Fprintf(w, "Received: %s", param)
    }
    
    func main() {
            http.HandleFunc("/", handler)
            http.ListenAndServe(":80", nil)
    }
    ```
    
2. http 状态码返回
    
    ```Bash
    package main
    
    import (
            "fmt"
            "net/http"
    )
    func handleRequest(w http.ResponseWriter, r *http.Request) {
            // 模拟根据请求处理的逻辑
            if r.URL.Path == "/" {
                    // 处理成功，返回HTTP 200
                    w.WriteHeader(http.StatusOK)
                    fmt.Fprintf(w, "Success")
            } else {
                    // 处理失败，返回HTTP 404
                    w.WriteHeader(http.StatusNotFound)
                    fmt.Fprintf(w, "Not Found")
            }
    }
    func main() {
            http.HandleFunc("/", handleRequest)
            http.ListenAndServe(":80", nil)
    }
    ```
    
3. 闭包
    
    ```Bash
    在Go语言中，http.HandleFunc()函数的第二个参数是一个函数类型，它必须是满足http.HandlerFunc函数签名的函数。该函数接收两个参数：http.ResponseWriter和*http.Request。
    如果你想在handleRequest函数中传入其他参数，可以使用闭包（Closure）的方式。
    以下是一个示例代码，展示如何在handleRequest函数中传入其他参数：
    go
    复制
    package main
    import (
            "fmt"
            "net/http"
    )
    
    func handleRequest(customParam string) http.HandlerFunc {
            return func(w http.ResponseWriter, r *http.Request) {
                    // 在这里可以使用 customParam 和 w、r 来处理请求
                    fmt.Println("Custom parameter:", customParam)
                    fmt.Fprintf(w, "Hello, World!")
            }
    }
    
    func main() {
            param := "custom value"
            http.HandleFunc("/", handleRequest(param))
            http.ListenAndServe(":8080", nil)
    }
    在上述代码中，我们定义了一个handleRequest函数，它接收一个类型为string的参数customParam。handleRequest函数返回一个函数，该函数满足http.HandlerFunc函数签名。
    在返回的函数中，我们可以使用闭包的方式访问customParam以及http.ResponseWriter和*http.Request参数，并进行相应的处理。
    在main函数中，我们定义了一个param变量作为自定义参数的值。然后，我们通过handleRequest(param)将param传递给handleRequest函数，并返回一个满足http.HandlerFunc函数签名的函数。
    最后，我们使用http.HandleFunc()来将路径"/"与返回的处理函数进行绑定，并通过http.ListenAndServe()监听HTTP请求
    ```
    
4. net 和 http同时使用
    
      Go 语言中，可以同时使用 `net` 和 `http` 包来监听同一个接口，但是需要小心处理并避免冲突。
    
      `net` 包提供了底层的网络功能，可以通过 `net.Listen` 函数来监听指定的网络地址和端口。而 `http` 包是建立在 `net` 包之上的，提供了更高级的 HTTP 服务器和客户端功能。
    
      以下是一个示例代码，同时使用 `net` 和 `http` 监听同一个接口：
    
      
    
    ```Plaintext
    package main
    import (
            "fmt"
            "net"
            "net/http"
    )
    
    func main() {
            // 使用 net 包监听指定的网络地址和端口
            listener, err := net.Listen("tcp", "localhost:8080")
            if err != nil {
                    fmt.Println("Error listening:", err)
                    return
            }
            defer listener.Close()
            // 启动 HTTP 服务器
            go func() {
                    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
                            fmt.Fprintf(w, "Hello, HTTP!")
                    })
                    err := http.Serve(listener, nil)
                    if err != nil {
                            fmt.Println("Error serving HTTP:", err)
                            return
                    }
            }()
    
            // 其他的网络处理逻辑...
            // ...
            // 等待程序退出
            select {}
    }
    ```
    
      在上述示例中，我们使用 `net.Listen` 函数监听 `localhost:8080`，然后使用 `http` 包启动了一个 HTTP 服务器。通过 `http.HandleFunc` 函数，我们定义了一个简单的处理函数来响应 HTTP 请求。最后，通过 `http.Serve` 函数将监听器与 HTTP 服务器关联起来。
    
      你可以在 `// 其他的网络处理逻辑...` 的部分添加其他网络处理逻辑，如基于 `net` 包的 TCP 或 UDP 服务器。只需确保网络处理逻辑不会与 HTTP 服务器冲突，比如使用不同的端口或处理不同的网络协议。
    
      需要注意的是，当使用 `http.Serve` 函数时，它将阻塞当前的 goroutine，因此我们在示例中使用了 `select {}` 来阻止 `main` 函数退出。这样可以保持服务器的运行，直到显式地退出程序。
    
5. mymux
    
      `myMux := http.NewServeMux()` 这条语句用于创建一个新的 `ServeMux` 对象。
    
      在 Go 的 `http` 包中，`ServeMux` 是一个 HTTP 请求多路复用器（multiplexer），用于将收到的 HTTP 请求分发到相应的处理器。`ServeMux` 类型实现了 `http.Handler` 接口，因此它本身可以作为一个处理器来处理请求。
    
      通过调用 `http.NewServeMux()` 函数，我们可以创建一个新的 `ServeMux` 对象，它将用于注册和管理不同路径的处理器。
    
      例如，下面是一个简单示例，使用 `ServeMux` 对象来管理不同路径的处理器：
    
      
    
    ```Plaintext
    package main
    import (
            "fmt"
            "net/http"
    )
    
    func main() {
            myMux := http.NewServeMux()
    
            myMux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
                    fmt.Fprintf(w, "Hello, World!")
            })
    
            myMux.HandleFunc("/about", func(w http.ResponseWriter, r *http.Request) {
                    fmt.Fprintf(w, "About page")
            })
            server := &http.Server{
                    Addr:    ":8080",
                    Handler: myMux,
            }
            server.ListenAndServe()
    }
    ```
    
      在上述示例中，我们首先使用 `http.NewServeMux()` 创建了一个新的 `ServeMux` 对象 `myMux`。然后，我们使用 `myMux.HandleFunc` 方法来注册处理器函数，每个函数对应一个特定的路径。
    
      最后，我们创建了一个 `http.Server` 对象，并将 `myMux` 对象作为处理器指定给该服务器。这样，当服务器收到请求时，就会使用 `myMux` 对象来根据请求的路径选择相应的处理器函数进行处理。
    
      总结起来，`myMux := http.NewServeMux()` 用于创建一个 `ServeMux` 对象，以便注册和管理不同路径的处理器，并根据路径选择相应的处理函数来处理请求。
    
6. go routine 实现并发执行
    
      Go 的 goroutine 来实现并发执行。以下是一个示例代码，演示了如何同时运行 HTTP 服务、gRPC 服务和连接其他 gRPC 服务器：
    
    
    ```go
    package main
    import (
            "log"
            "net"
            "net/http"
            "google.golang.org/grpc"
            "google.golang.org/grpc/credentials"
    )
    
    func main() {
            // 创建监听器
            lis, err := net.Listen("tcp", ":8080")
            if err != nil {
                    log.Fatalf("Failed to listen: %v", err)
            }
    
            // 启动 HTTP 服务器
            go func() {
                    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
                            w.Write([]byte("Hello, HTTP!"))
                    })
                    if err := http.Serve(lis, nil); err != nil {
                            log.Fatalf("HTTP failed to serve: %v", err)
                    }
            }()
    
            // 启动 gRPC 服务器
            go func() {
                    s := grpc.NewServer()
                    // 注册 gRPC 服务
                    // ...
                    if err := s.Serve(lis); err != nil {
                            log.Fatalf("gRPC failed to serve: %v", err)
                    }
            }()
    
            // 连接其他 gRPC 服务器
            conn, err := grpc.Dial("other_server_address", grpc.WithTransportCredentials(credentials.NewInsecure()))
            if err != nil {
                    log.Fatalf("Failed to connect to other server: %v", err)
            }
            defer conn.Close()
            // 进行其他操作，使用 conn 进行 gRPC 通信
            // ...
            // 等待程序退出
            select {}
    }
    ```
    
      在这个示例中，我们使用 goroutine 启动了 HTTP 服务器和 gRPC 服务器，并在主函数中创建了一个与其他 gRPC 服务器的连接。
    
      注意在连接其他 gRPC 服务器时，我们使用了 `grpc.Dial` 函数，并传递了 `grpc.WithTransportCredentials(credentials.NewInsecure())` 选项来创建一个不安全的连接。你应该根据实际情况选择正确的连接配置，例如使用安全的连接和认证。
    
      最后，使用 `select {}` 阻塞主函数，使程序保持运行状态，直到显式退出。
    
      请根据你的实际需求，将代码中的注释部分替换为适当的 gRPC 服务注册和其他操作代码。
    


7. 连接阻塞
    
    ```Go
            // 主要就是 grpc.WithBlock()
            
            conn2, err2 := grpc.Dial(*addr2, grpc.WithTransportCredentials(insecure.NewCredentials()),grpc.WithBlock())
            
    ```
    
8. 循环变量副本
    
      for i := 1; i <= *rpc_num; i++ {
    
      if i == *rpc_id {
    
      continue // 跳过与 当前相同的 端口号 ，不能和自己连接
    
      }
    
      go func(){
    
      conn[i], err = grpc.Dial(rpc_addr[i], grpc.WithTransportCredentials(insecure.NewCredentials()), grpc.WithBlock())
    
      if err != nil {
    
      log.Fatalf("Failed to connect to server %s: %v", rpc_addr[i], err)
    
      return
    
      }
    
      log.Printf("Connect to %s OK", rpc_addr[i])
    
      }()
    
      }
    
      这里有个提示信息但是没有报错，提示为：loop variable i captured by func litera
    
       "loop variable i captured by func literal" 意味着在你的代码中，匿名函数字面量（func literal）捕获了循环变量 i。
    
      在 Go 语言中，当你在循环体内使用匿名函数时，该匿名函数会形成一个闭包，它可以访问和引用外部作用域中的变量。在这种情况下，匿名函数引用了循环变量 i。然而，循环变量在每次迭代中的值会发生变化，这可能会导致一些意外的行为。
    
      为了避免这种问题，Go 语言引入了循环变量副本的概念。在每次迭代中，会为循环体内的匿名函数创建一个新的副本变量，以确保在闭包内部引用的是正确的值。这样可以避免循环变量值的竞争条件。
    
      解决这个问题的常见方法是在循环体内部创建一个局部变量，将循环变量的值复制给局部变量，然后在匿名函数中使用局部变量。例如：

    ```Plaintext
    for i := 1; i <= *rpc_num; i++ {
        if i == *rpc_id {
            continue
        }
        // 创建局部变量 iCopy 并将循环变量的值赋给它
        iCopy := i
        go func() {
            conn[iCopy], err = grpc.Dial(rpc_addr[iCopy], grpc.WithTransportCredentials(insecure.NewCredentials()), grpc.WithBlock())
            if err != nil {
                log.Fatalf("Failed to connect to server %s: %v", rpc_addr[iCopy], err)
                return
            }
            log.Printf("Connect to %s OK", rpc_addr[iCopy])
        }()
    }
    ```
    
      通过创建局部变量 iCopy 并使用它，你可以确保每个匿名函数引用的是该迭代的正确值，而不会受到循环变量的更改影响。









# Atomic 包
```go
package main

import (
    "log"
    "net/http"
    "runtime"
    "sync/atomic"
)

var count int32

func test(w http.ResponseWriter, r *http.Request) {
    currentCount := atomic.LoadInt32(&count)
    atomic.AddInt32(&count, 1)
    log.Println("count:", currentCount)
}
func main() {
    runtime.GOMAXPROCS(runtime.NumCPU() - 1)
    http.HandleFunc("/", test)
    http.ListenAndServe(":8080", nil)
}
```



# Done Wait 同步
 Go 内存模型中的术语。它说明了在调用 `Done` 方法之前，它会与任何由它解除阻塞的 `Wait` 调用之后的返回之前进行"同步在"（synchronizes before）的关系。

具体解释如下：

1. `Done` 方法是用于通知等待组（WaitGroup）中某个操作的完成。
2. `Wait` 方法是等待等待组中所有操作完成的调用。
3. "同步在"（synchronizes before）是 Go 内存模型中的同步关系，指示在同一个 goroutine 中，一个操作的执行在另一个操作之前进行了同步。

根据这句话的解释，当一个 goroutine 调用 `Done` 方法时，它会在解除等待组中的某个操作的阻塞之前与该操作的返回之前进行同步。

具体来说，如果一个 goroutine 在调用 `Wait` 方法等待等待组中的操作时被阻塞，那么当另一个 goroutine 调用 `Done` 方法通知该操作的完成时，调用 `Done` 的 goroutine 会在被阻塞的 goroutine 返回之前进行同步。这确保了在 `Done` 方法解除阻塞之后，等待的 goroutine 可以安全地继续执行。

总之，这句话的含义是在 Go 内存模型中，`Done` 方法的调用会在解除阻塞的 `Wait` 调用的返回之前进行同步。这个同步关系确保了等待组的协同操作的正确性和可靠性。