# Go基础语法学习总结

## 一、基础语法

### 1、常见数据类型

> 数据类型包括有：布尔类型、字符串类型、数字类型（整型、浮点型、复数等）、派生类型（数组类型、`slice` 切片类型、`map` 集合类型、`struct` 结构体类型、`interface` 接口类型、`channel` 通道类型、指针类型等）
>
> 注意：这里的 `slice` 切片类型，相当于没有指定长度的数组，可以自动扩容。

### 2、int 整型

```go
package main

import (
	"fmt"
	"math"
	"unsafe"
)

func main() {
	// 整数
	// 一个字节
	var i8 int8
	// 两个字节
	var i16 int16
	// 四个字节
	var i32 int32
	// 八个字节
	var i64 int64
	// 无符号，一个字节
	var ui8 uint8
	// 无符号，两个字节
	var ui16 uint16
	// 无符号，四个字节
	var ui32 uint32
	// 无符号，八个字节
	var ui64 uint64
    
	// 打印类型、字节大小、范围
	fmt.Printf("%T %dB %v~%v\n", i8, unsafe.Sizeof(i8), math.MinInt8, math.MaxInt8)
	fmt.Printf("%T %dB %v~%v\n", i16, unsafe.Sizeof(i16), math.MinInt16, math.MaxInt16)
	fmt.Printf("%T %dB %v~%v\n", i32, unsafe.Sizeof(i32), math.MinInt32, math.MaxInt32)
	fmt.Printf("%T %dB %v~%v\n", i64, unsafe.Sizeof(i64), math.MinInt64, math.MaxInt64)
	fmt.Printf("%T %dB %v~%v\n", ui8, unsafe.Sizeof(ui8), 0, math.MaxUint8)
	fmt.Printf("%T %dB %v~%v\n", ui16, unsafe.Sizeof(ui16), 0, math.MaxUint16)
	fmt.Printf("%T %dB %v~%v\n", ui32, unsafe.Sizeof(ui32), 0, math.MaxUint32)
	fmt.Printf("%T %dB %v~%v\n", ui64, unsafe.Sizeof(ui64), 0, uint64(math.MaxUint64))
    
    // 无符号整型（默认是8个字节，相当于java中的Long类型）
	var ui uint
	fmt.Printf("%T %dB\n", ui, unsafe.Sizeof(ui))
    
	// 有符号整型（默认是8个字节，相当于java中的Long类型）
	var ii int
	fmt.Printf("%T %dB\n", ii, unsafe.Sizeof(ii))
}
```

```go
int8 1B -128~127
int16 2B -32768~32767
int32 4B -2147483648~2147483647
int64 8B -9223372036854775808~9223372036854775807
uint8 1B 0~255
uint16 2B 0~65535
uint32 4B 0~4294967295
uint64 8B 0~18446744073709551615

uint 8B
int 8B
```

### 3、float 浮点数

```go
package main

import (
	"fmt"
	"math"
	"unsafe"
)

func main() {
	// 浮点数
	// 四个字节
	var f32 float32
	// 八个字节
	var f64 float64
	fmt.Printf("%T %dB %v~%v\n", f32, unsafe.Sizeof(f32), -math.MaxFloat32, math.MaxFloat32)
	fmt.Printf("%T %dB %v~%v\n", f64, unsafe.Sizeof(f64), -math.MaxFloat64, math.MaxFloat64)
}
```

```go
float32 4B -3.4028234663852886e+38~3.4028234663852886e+38
float64 8B -1.7976931348623157e+308~1.7976931348623157e+308
```

### 4、字符串类型

```go
package main

import (
	"bytes"
	"fmt"
)

func main() {
	s1 := "hello1"
	var s2 string = "hello2"
	var s3 = "hello3"
	fmt.Printf("s1=%v\n", s1)
	fmt.Printf("s2=%v\n", s2)
	fmt.Printf("s3=%v\n", s3)

	fmt.Println("--------------------")

	// 字符串连接
	// 直接+号连接
	s4 := s1 + s2
	fmt.Printf("s4=%v\n", s4)

	fmt.Println("--------------------")

	// 使用方法进行格式化连接
	msg := fmt.Sprintf("[%s, %s]", s1, s2)
	fmt.Printf("msg=%v\n", msg)

	fmt.Println("--------------------")

	// 使用buffer进行生成
	var buffer bytes.Buffer
	buffer.WriteString("(tom")
	buffer.WriteString("=")
	buffer.WriteString("张三)")
	fmt.Printf("buffer_string=%v\n", buffer.String())

	fmt.Println("--------------------")

	// 字符串切片
	s := "hello world"
	fmt.Println(s[0:5])
	fmt.Println(s[:5])
	fmt.Println(s[1:])
}
```

```go
s1=hello1
s2=hello2
s3=hello3
--------------------
s4=hello1hello2
--------------------
msg=[hello1, hello2]
--------------------
buffer_string=(tom=张三)
--------------------
hello
hello
ello world
```

### 5、进制数

```go
package main

import "fmt"

func main() {
	// 定义一个十进制的数
	num := 10
	// 输出十进制
	fmt.Printf("%d\n", num)
	// 输出二进制
	fmt.Printf("%b\n", num)
	// 输出八进制
	fmt.Printf("%o\n", num)
	// 输出十六进制
	fmt.Printf("%x\n", num)
	fmt.Printf("%X\n", num)
}
```

```go
10
1010
12
a
A
```

### 6、数组类型

```go
package main

import "fmt"

func main() {
	// 一、创建数组，并显示声明数组的大小
	arr1 := [2]int{1, 2}
	fmt.Printf("arr1: %v\n", arr1)
	fmt.Printf("arr1: %T\n", arr1)

	// int类型的初始值为0
	arr2 := [2]int{}
	fmt.Printf("arr2: %v\n", arr2)

	// 二、创建数组，并自动计算数组大小（...表示任意大小，会自动计算数组大小）
	arr3 := [...]int{1, 2, 3}
	fmt.Printf("arr3: %v\n", arr3)

	// 三、创建切片类型（不指定数组大小，即为切片类型，切片类型相当于会自动扩容的数组）
	arr4 := []int{1, 2, 3}
	// 切片类型可以添加元素，但数组不能添加，数组是固定长度。
	arr4 = append(arr4, 100)
	fmt.Printf("arr4: %v\n", arr4)
}
```

```go
arr1: [1 2]
arr1: [2]int

arr2: [0 0]
arr3: [1 2 3]

arr4: [1 2 3 100]
```

### 7、slice 切片类型

```go
package main

import "fmt"

func main() {
	// 一、直接创建切片（不指定数组的大小，即为切片类型，切片类型相当于会自动扩容的数组）
	arr1 := []int{1, 2, 3}
	// 切片类型可以添加元素，但数组不能添加，数组是固定长度。
	// append函数只能接收切片类型的数据，不能接收数组类型的数据。
	arr1 = append(arr1, 100)
	fmt.Printf("arr1: %v\n", arr1)
	fmt.Printf("arr1: %T\n", arr1)

	fmt.Println("------------------------")

	// 二、使用make来创建切片，并指定初始长度为4，会赋初始值为0。此时容量也默认为4。
	arr2 := make([]int, 4)
	fmt.Printf("arr2: %v\n", arr2)
	fmt.Printf("len(arr2): %v\n", len(arr2))
	fmt.Printf("cap(arr2): %v\n", cap(arr2))
	fmt.Println("======添加一个元素后，触发扩容======")
	// 添加元素
	// append函数只能接收切片类型的数据，不能接收数组类型的数据。
	arr2 = append(arr2, 10)
	fmt.Printf("arr2: %v\n", arr2)
	fmt.Printf("len(arr2): %v\n", len(arr2))
	fmt.Printf("cap(arr2): %v\n", cap(arr2))

	fmt.Println("------------------------")

	// 三、使用make创建切片，指定初始长度为0，容量为10
	arr3 := make([]int, 0, 10)
	fmt.Printf("arr3: %v\n", arr3)
	fmt.Printf("len(arr3): %v\n", len(arr3))
	fmt.Printf("cap(arr3): %v\n", cap(arr3))
	fmt.Println("======添加一个元素后======")
	// 添加一个元素
	arr3 = append(arr3, 10)
	fmt.Printf("arr3: %v\n", arr3)
	fmt.Printf("len(arr3): %v\n", len(arr3))
	fmt.Printf("cap(arr3): %v\n", cap(arr3))

	fmt.Println("------------------------")

	// 删除切片中指定下标的元素
	arr4 := []int{1, 2, 3, 4}
	fmt.Printf("arr4: %v\n", arr4)
	// 删除3这个元素，索引为2
	arr4 = append(arr4[:2], arr4[2+1:]...)
	fmt.Printf("arr4: %v\n", arr4)
}
```

```go
arr1: [1 2 3 100]
arr1: []int
------------------------

arr2: [0 0 0 0]
len(arr2): 4
cap(arr2): 4
======添加一个元素后，触发扩容======
arr2: [0 0 0 0 10]
len(arr2): 5
cap(arr2): 8
------------------------

arr3: []
len(arr3): 0
cap(arr3): 10
======添加一个元素后======
arr3: [10]
len(arr3): 1
cap(arr3): 10

------------------------
arr4: [1 2 3 4]
arr4: [1 2 4]
```

### 8、map 类型

```go
package main

import "fmt"

func test1() {
	// 创建map的方式一：使用make创建map类型[key的类型]value的类型
	m := make(map[string]string)
	m["name"] = "张三"
	m["age"] = "20"
	m["mail"] = "312885991@qq.com"
	// i为索引，v为值
	for k, v := range m {
		fmt.Printf("key: %v, value: %v\n", k, v)
	}
}

func test2() {
	// 创建map的方式二：直接声明并赋值
	m := map[string]string{
		"name": "张三",
		"age":  "20",
		"mail": "312885991@qq.com",
	}
	// 输出map的类型
	fmt.Printf("m: %T\n", m)
	// i为索引，v为值
	for k, v := range m {
		fmt.Printf("key: %v, value: %v\n", k, v)
	}
}

func test3() {
	// interface{}表示一个没有方法的空接口类型，相当于其他语言中的Object类型。
	// go语言中的所有类型都属于interface{}接口类型，因此interface{}可以表示任意类型。
	m := map[string]interface{}{
		"name":      "张三",
		"age":       20,
		"isMarried": false,
	}

	// 遍历字典中的数据
	for k, v := range m {
		fmt.Printf("%v=%v\n", k, v)
	}
}

func main() {
	test1()
	test2()
	test3()
}
```

```go
// test1
key: name, value: 张三
key: age, value: 20
key: mail, value: 312885991@qq.com

// test2
m: map[string]string
key: name, value: 张三
key: age, value: 20
key: mail, value: 312885991@qq.com

// test3
name=张三
age=20
isMarried=false
```

### 9、指针类型

```go
package main

import "fmt"

type Person struct {
	id   int
	name string
}

func main() {
	// 简单类型的指针
	x := 10
	// 声明一个指针变量p
	var p *int
	// 将x的地址赋值给指针变量p
	p = &x
	// 打印p的类型
	fmt.Printf("p: %T\n", p)
	// p表示指向x变量的地址指针，*p可以取出该地址存储的值
	fmt.Printf("p: %v\n", p)
	fmt.Printf("p: %v\n", *p)

	fmt.Println("----------------")

	// 定义一个结构体对象
	person := Person{
		id:   1001,
		name: "张三",
	}
	// 定义一个结构体指针
	var per *Person
	per = &person
	fmt.Printf("p: %T\n", per)
	fmt.Printf("p: %v\n", per)
	fmt.Printf("p: %v\n", *per)

	fmt.Println("----------------")

	// 指针数组使用
	arr := [3]int{1, 2, 3}
	// 声明一个指针数组
	var arrp [3]*int
	// 将数组的各个元素地址赋值给指针数组
	for i := 0; i < len(arr); i++ {
		arrp[i] = &arr[i]
	}

	// 打印指针数组
	fmt.Printf("arrp: %v\n", arrp)
	// 遍历指针数组，并取指针所指向地址的数值
	for i := 0; i < len(arrp); i++ {
		fmt.Printf("*arrp[%v]: %v\n", i, *arrp[i])
	}
}
```

```go
p: *int
p: 0xc000016088
p: 10
----------------
p: *main.Person
p: &{1001 张三}
p: {1001 张三}
----------------
arrp: [0xc000018138 0xc000018140 0xc000018148]
*arrp[0]: 1
*arrp[1]: 2
*arrp[2]: 3
```



### 10、if 语句

```go
package main

import (
	"fmt"
)

func main() {
	age := 18
	if age < 18 {
		fmt.Println("未成年")
	} else if age >= 18 && age <= 50 {
		fmt.Println("成年了")
	} else {
		fmt.Println("老年了")
	}
}
```

```go
成年了
```

### 11、switch 语句

```go
package main

import "fmt"

func main() {
	season := "秋天"
	switch season {
	case "春天":
		fmt.Println("秋天来了")
	case "夏天":
		fmt.Println("夏天来了")
	case "秋天":
		fmt.Println("秋天来了")
	case "冬天":
		fmt.Println("冬天来了")
	default:
		fmt.Println("未知季节")
	}
}
```

```go
秋天来了
```

### 12、for 遍历语句

```go
package main

import "fmt"

func main() {
	// 普通for循环
	for i := 0; i < 3; i++ {
		fmt.Printf("i=%v\n", i)
	}
	fmt.Println("----------------------")

	// 利用range遍历数组
	// 案例一(其中...表示自动计算数组大小)
	arr1 := [...]int{1, 2, 3, 4, 5}
	// i表示索引，v表示值
	for i, v := range arr1 {
		fmt.Printf("%v=%v\n", i, v)
	}

	fmt.Println("----------------------")

	// 案例二（不指定大小，也不设置... 则表示切片类型，切片类型即能自动扩容的数组）
	arr2 := []int{1, 2, 3, 4}
	for i, v := range arr2 {
		fmt.Printf("%v=%v\n", i, v)
	}

	// for {
	// 	fmt.Println("我是死循环...")
	// }
}
```

```go
i=0
i=1
i=2
----------------------
0=1
1=2
2=3
3=4
4=5
----------------------
0=1
1=2
2=3
3=4
```

### 13、continue 和 break

> 这两个与其他语言的用法一致，这里略过

### 14、goto 语句

```go
package main

import "fmt"

func main() {
	// 多层循环
	for i := 0; i < 5; i++ {
		for j := 0; j < 2; j++ {
			if i > 2 {
				// 跳转到标签处
				goto MYLABEL
			}
			fmt.Printf("i: %v\n", i)
		}
		fmt.Println("---------------")
	}
	// 自定义标签
MYLABEL:
	fmt.Println("进入goto目的地")
	x := 10
	y := 20
	// 求两数之和
	sum := x + y
	fmt.Printf("(x+y)=%v", sum)
}
```

```go
i: 0
i: 0
---------------
i: 1
i: 1
---------------
i: 2
i: 2
---------------
进入goto目的地
(x+y)=30
```

13、defer 语句

```go
package main

import "fmt"

func main() {
	fmt.Println("开始执行...")
	// defer 语句会延迟到最后执行。
	// 当同时有多个defer语句时，是按栈的先进后出，后进先出顺序进行执行。
	// 即先定义的后执行，后定义的先执行。
	defer fmt.Println("清理程序中的进程1...")
	defer fmt.Println("清理程序中的进程2...")
	fmt.Println("程序即将结束...")
}
```

```go
开始执行...
程序即将结束...
清理程序中的进程2...
清理程序中的进程1...
```



### 15、make 和 new

```go
package main

import (
	"fmt"
)

// new和make的区别:
// 1. make只能用来分配及初始化类型为slice, map , chan的数据; new可以分配任意类型的数据
// 2. new分配返回的是指针，即类型*T ; make返回的是引用，即T;
// 3. new分配的空间被清零，make分配后，会进行初始化。

func main() {
	arr1 := make([]int, 10)
	fmt.Printf("arr1: %T\n", arr1)
	fmt.Printf("arr1: %v\n", arr1)

	fmt.Println("------------------------")

	arr2 := new([]int)
	fmt.Printf("arr2: %T\n", arr2)
	fmt.Printf("arr2: %v\n", arr2)
	fmt.Printf("arr2: %v\n", *arr2)
}
```

```go
arr1: []int
arr1: [0 0 0 0 0 0 0 0 0 0]
------------------------
arr2: *[]int
arr2: &[]
arr2: []
```

### 16、测试数据类型

```go
package main

import "fmt"

func main() {
	var name = "张三"
	age := 20
	is_married := false
	// 打印数据类型
	fmt.Printf("%v %T\n", name, name)
	fmt.Printf("%v %T\n", age, age)
	fmt.Printf("%v %T\n", is_married, is_married)

	fmt.Println("----------------------")

	b := &age
	// b是指针类型，其中b表示age的地址，*p表示取地址存储的值
	fmt.Printf("%v %v %T\n", b, *b, b)

	arr := []int{1, 2, 3, 4, 5}
	// 数组类型
	fmt.Printf("%v %T\n", arr, arr)
	fmt.Println("数组的长度：", len(arr))

}
```

```go
张三 string
20 int
false bool
----------------------
0xc000016088 20 *int
[1 2 3 4 5] []int
数组的长度： 5
```

### 17、格式化操作

```go
package main

import (
	"fmt"
	"time"
)

type WebSite struct {
	Name string
	Age  int
}

func main() {
	// 结构体类型
	site := WebSite{Name: "张三", Age: 20}
	fmt.Printf("site: %v\n", site)
	fmt.Printf("site: %#v\n", site)
	fmt.Printf("site: %T\n", site)

	fmt.Println("---------------------")

	// 布尔类型
	b := true
	fmt.Printf("b: %v\n", b)
	fmt.Printf("b: %T\n", b)

	fmt.Println("---------------------")

	// 指针类型
	p := &b
	fmt.Printf("p: %v\n", p)
	fmt.Printf("p: %T\n", p)

	fmt.Println("---------------------")

	// 科学计数法
	fmt.Printf("a: %e\n", 100.2)

	fmt.Println("---------------------")

	// 开始时间
	start := time.Now()
	// 求和
	sum := 0
	for i := 0; i < 1000000; i++ {
		sum += i
	}
	// 结束时间
	end := time.Now()
	// 间隔时间
	duration := end.Sub(start)
	fmt.Printf("sum: %v, use_time=%v\n", sum, duration.Microseconds())
}
```

```go
site: {张三 20}
site: main.WebSite{Name:"张三", Age:20}
site: main.WebSite
---------------------
b: true
b: bool
---------------------
p: 0xc0000160d0
p: *bool
---------------------
a: 1.002000e+02
---------------------
sum: 499999500000, use_time=653
```



## 二、函数

### 1、函数用法

函数的形式如下：

```go
func 函数名(参数名 参数类型, 参数名 参数类型...) (返回值类型, 返回值类型...){
    函数体
}

func 函数名(参数名 参数类型, 参数名 参数类型...) (返回值变量 返回值类型, 返回值变量 返回值类型...){
    函数体
}
```

```go
package main

import "fmt"

func test1() {
	fmt.Println("测试函数")
	fmt.Println("---------------")
}

func test2(name string, age int) {
	fmt.Printf("name: %v\n", name)
	fmt.Printf("age: %v\n", age)
	fmt.Println("---------------")
}

// 声明返回值类型
func add(a int, b int) int {
	// 声明c变量进行接收
	c := a + b
	return c
}

// 声明返回值类型，并声明对应的变量
func add2(a int, b int) (c int) {
	// 返回值声明了c变量，因此不用再声明，可以直接使用
	c = a + b
	return c
}

func main() {
	test1()
	test2("张三", 20)

	i := add(1, 10)
	fmt.Printf("i: %v\n", i)

	fmt.Println("---------------")

	i = add2(1, 20)
	fmt.Printf("i: %v\n", i)
}
```

```go
测试函数
---------------
name: 张三
age: 20
---------------
i: 11
---------------
i: 21
```

### 2、函数高级

```go
package main

import "fmt"

// 返回一个求和的函数
func f1() func(a int, b int) int {
	// 声明一个求和的匿名函数
	var addFn = func(a int, b int) int {
		return a + b
	}
	return addFn
}

// 声明一个函数，并传递一个函数(指定了函数的具体类型)
func f2(fn func(a int, b int) int) {
	fmt.Println("执行传递的函数...")
	i := fn(1, 2)
	fmt.Printf("i: %v\n", i)
}

// 相减函数
func sub(a int, b int) int {
	return a - b
}

func main() {
	// 返回一个求和的函数
	f := f1()
	fmt.Printf("f(10, 20): %v\n", f(10, 20))

	// 调用f2函数，并传递一个求差的函数
	f2(sub)
}
```

```go
f(10, 20): 30
执行传递的函数...
i: -1
```

### 3、值传递和引用传递

```go
package main

import "fmt"

// 测试值传递和引用传递
type Person struct {
	name string
}

// 值传递
func ff1(person Person) {
	person.name = "值传递"
}

// 引用传递（形参为指针类型）
func ff2(person *Person) {
	person.name = "引用传递"
}

func main() {
	// 测试值传递和引用传递
	person := Person{
		name: "张三",
	}

	fmt.Printf("person.name: %v\n", person.name)
	// 值传递，传递的是副本，因此函数内修改对象的值，并不会改变原对象的值
	ff1(person)
	fmt.Printf("person.name: %v\n", person.name)

	fmt.Println("-------------------------------")

	// 引用传递，传递的是地址，函数内修改对象的值等价于修改原对象
	var p *Person
	p = &person
	ff2(p)
	// ff2(&person)
	fmt.Printf("person.name: %v\n", person.name)
}
```

```go
person.name: 张三
person.name: 张三
-------------------------------
person.name: 张三
person.name: 引用传递
```

### 4、初始化函数

go语言中，名称为 `init` 的函数表示初始化函数，无须显示调用，会自动被调用。

```go
package main

import "fmt"

func init() {
	fmt.Println("这是初始化的内容...")
}

func main() {
    // init函数被自动调用
}
```

```go
这是初始化的内容...
```



## 三、结构体

> go 语言中没有类的概念，只有结构体，等同于类。并且结构体不支持继承，但可以通过组合的方式实现继承的效果。

### 1、声明

结构体的形式如下：

**注意**：在结构体中，如果属性名首字母大写，则表示公有属性；如果属性名首字母小写，则表示私有属性。私有属性外部包不能使用，同时也不能进行序列化。

```go
type name struct{
    属性名 属性类型
    属性名 属性类型
}
```

### 2、例子

```go
package main

import "fmt"

// 定义Person结构体
// 结构体中，属性名首字母大写表示公有属性，首字母小写表示私有属性。
type Student struct {
	// 同类型的可以写在一块
	id, age int
	name    string
}

// 定义Student结构体所属的方法
func (stu Student) eat() {
	fmt.Printf("%v正在吃...\n", stu.name)
}

func (stu Student) sleep() {
	fmt.Printf("%v正在睡...\n", stu.name)
}

func main() {
	// 第一种赋值
	var stu Student
	stu.id = 1001
	stu.age = 20
	stu.name = "张三"
	fmt.Printf("stu: %v\n", stu)

	// 第二种赋值
	st := Student{
		id:   1001,
		name: "李四",
		age:  20,
	}
	fmt.Printf("st: %v\n", st)

	// 测试方法
	st.eat()
	st.sleep()
}
```

```go
stu: {1001 20 张三}
st: {1001 20 李四}

李四正在吃...
李四正在睡...
```

## 四、接口

### 1、声明

接口的形式如下：接口中声明方法的形式，与声明函数是一样的。

**注意**：`interface{}` 表示不包含方法的接口，属于空接口类型，它相当于其他语言中的Object类型，可以表示任何类型。

```go
type name interface{
    方法名()
    方法名(参数名 参数类型, 参数名 参数类型...) (返回值类型, 返回值类型...)
}
```

### 2、例子

```go
package main

import (
	"fmt"
)

// 定义接口
type Animal interface {
	// 动物描述
	descript()
}

// 定义Cat结构体
type Cat struct {
}

// Cat结构体实现Animal接口（方法名称及参数相同即代表实现这个接口）
func (cat Cat) descript() {
	fmt.Println("这是猫的描述内容...")
}

// 定义Dog结构体
type Dog struct {
}

// Dog结构体实现Animal接口（方法名称及参数相同即代表实现这个接口）
func (dog Dog) descript() {
	fmt.Println("这是狗的描述内容...")
}

// 定义一个普通方法，传递Animal接口对象（多态）
func test(animal Animal) {
	animal.descript()
}

func main() {
	// 创建dog和cat对象
	dog := Dog{}
	cat := Cat{}

	// 调用test方法（多态）
	test(dog)
	test(cat)

	fmt.Println("---------------------")

	// 声明变量p为Animal接口类型（多态）
	var p Animal
	// Dog是Animal的实现，所以可以这样赋值
	p = Dog{}
	test(p)
	// Cat也是Animal的实现，所以可以这样赋值
	p = Cat{}
	test(p)
}
```

```go
这是狗的描述内容...
这是猫的描述内容...
---------------------
这是狗的描述内容...
这是猫的描述内容...
```

## 五、并发编程

### 1、协程

> go语言中的协程，相当于系统中的线程一样。

```go
package main

import (
	"fmt"
	"time"
)

// 定义一个方法，循环打印5次名称
func ShowMsg(name string) {
	for i := 0; i < 5; i++ {
		fmt.Printf("name: %v\n", name)
		// 打印一次睡眠1s
		time.Sleep(time.Second * 1)
	}
}

func main() {
	fmt.Println("start...")
	ShowMsg("java") // 主线程调用
	fmt.Println("end...")

	fmt.Println("-----------------")

	fmt.Println("start...")
	go ShowMsg("python") // 开启一个协线程来调用执行，主线程会继续往下执行
	// 让主线程睡眠3s，否则主线程往下执行结束后，协线程也会自动结束。
	// 那么观察不到协线程调用print的效果
	time.Sleep(time.Second * 3)
	fmt.Println("end...")
}
```

```go
start...
name: java
name: java
name: java
name: java
name: java
end...
-----------------
start...
name: python
name: python
name: python
end...
```

### 2、channel

>  channel 主要是负责协程之间进行通信的。
>
> Go语言中有两种channel，一种是无缓冲的channel，相当于其容量为1，一次只能接受一条消息，需要消息消费完成之后，才能接收下一条消息，其是同步阻塞的方式，如果消息不消费，会阻塞住当前线程。
>
> 另一种是带缓冲的channel，相当于其容量为n，可以同时接收多条消息，其是异步非阻塞的方式，即使消息不消费，也不会阻塞当前线程。

#### 2.1、基本使用

```go
package main

import (
	"fmt"
)

func main() {
	// 声明一个channel类型的变量ch
	var ch chan int
    ch := make*
	fmt.Printf("ch: %T\n", ch)

	// 1、创建无缓冲的channel，无缓冲的channel是同步的实现方式
	// （channel接收一个消息后，会阻塞住线程，直至有其他线程消费这个消息，才会结束）
	Unbuffed := make(chan string)

	// 开启一个协程，去消费channel中的消息
	go func() {
		// 从channel中取消息
		msg := <-Unbuffed
		fmt.Printf("msg: %v\n", msg)
	}()
	// 向channel发送消息，如果没有其他线程消费这条消息，则会阻塞在这里
	Unbuffed <- "Hello World"
	// 由于是不带缓冲的channel，其是同步的方式，所以如果没有上方协程取消息的操作，
	// 则上面一步会阻塞住，这条打印语句将不会执行。但现在由于协程消费了消息，所以这里会打印。
	fmt.Println("不带缓冲的channel: 我打印了吗？")

	fmt.Println("-------------------------------------")

	// 2、创建带缓冲的channel，异步实现的方式
	Buffed := make(chan string, 5)
	// 往带缓冲的channel中发送一条消息
	Buffed <- "带缓冲的Buffed消息"
	// 由于是带缓冲的channel，即使消息没有被消费，也不会阻塞住。因此会打印下方语句
	fmt.Println("带缓冲的channel: 我打印了吗？")
}
```

```go
ch: chan int
msg: Hello World
不带缓冲的channel: 我打印了吗？
-------------------------------------
带缓冲的channel: 我打印了吗？
```

#### 2.2、遍历

```go
package main

import "fmt"

func main() {
	// 声明无缓冲的channel类型
	ch := make(chan int)

	// 开启协程往channel中发送数据
	go func() {
		for i := 0; i < 5; i++ {
			// 往channel中添加数据
			ch <- i
		}
		// 关闭channel
		close(ch)
	}()

	// 主线程消费数据
	for v := range ch {
		fmt.Printf("接收到消息: %v\n", v)
	}
}
```

```go
接收到消息: 0
接收到消息: 1
接收到消息: 2
接收到消息: 3
接收到消息: 4
```

### 3、WaitGroup

> WaitGroup 相当于Java语言中的倒计时锁，用于等待各线程全部执行结束。

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

// 声明一个waitgroup类型变量
var wg sync.WaitGroup

// 执行任务的函数
func task() {
	for i := 0; i < 5; i++ {
		fmt.Printf("i: %v\n", i)
		// 每打印一次就睡眠1s
		time.Sleep(time.Second)
		// 执行完一次，就将wg其内部值进行-1操作
		wg.Done()
	}
}

func main() {

	// 设置起始的信号量为5
	wg.Add(5)

	// 异步执行任务
	go task()

	// 主线程等待协程执行完毕，否则协程也会随着主线程结束而结束。
	// 1、以前的做法(主线程主动睡眠5s等待)
	// time.Sleep(time.Second * 5)

	// 2、现在的做法
	wg.Wait() // 当其内部值不为0时，会阻塞住。
	fmt.Println("程序结束...")
}
```

```go
// 每隔1s打印的
i: 0
i: 1
i: 2
i: 3
i: 4
程序结束...
```

### 4、互斥锁

> 当多个协程对同一个变量进行读写操作时，可能会出现数据异常的问题，因此需要使用同步机制来控制多个协程对同一个变量的读写操作，保证数据的一致性。

```go
package main

import (
	"fmt"
	"sync"
)

// 声明一个全局变量count，用于加减操作
var count int = 100

// 声明一个类似倒计时锁的对象，用来控制主线程的结束
var wgg sync.WaitGroup

// 声明一个互斥锁对象
var lock sync.Mutex

func addCount() {
	lock.Lock() // 对共享变量的读写操作进行加锁,保证同一时刻,只能有一个协程进行访问
	// 执行自增操作
	count++
	lock.Unlock()

	// 执行完一次，就将wgg其内部值进行-1操作
	wgg.Done()
}

func subCount() {
	lock.Lock() // 对共享变量的读写操作进行加锁,保证同一时刻,只能有一个协程进行访问
	// 执行自减操作
	count--
	lock.Unlock()

	// 执行完一次，就将wgg其内部值进行-1操作
	wgg.Done()
}

// 测试互斥锁
func main() {

	// 进行100次的自增和自减（多个协程去完成）
	for i := 0; i < 100; i++ {
		// 将wgg其内部值+1，因为即将要开启一个协程去执行add操作
		wgg.Add(1)
		go addCount()

		// 将wgg其内部值+1，因为即将要开启一个协程去执行sub操作
		wgg.Add(1)
		go subCount()
	}

	// 等待所有协程执行完毕
	wgg.Wait()
	// 输出最终的count结果(如果不加锁的情况下，可能会出现count不等于100的情况)
	fmt.Printf("count: %v\n", count)
}
```

```go
count: 100
```

### 5、原子变量

> 原子变量也是用于同步机制，控制多线程的正确访问，相比于互斥锁，其效率较高。它和Java语言中的原子变量一样，都是采用了 `CAS` 操作来实现的。

#### 5.1、使用案例

```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
)

// 初始化全局变量
// var number int = 100  // 默认的int是8个字节、64位
var number int32 = 100

var wg sync.WaitGroup

func addNumber() {
	// number++
	// 使用原子操作+1，保证同步
	atomic.AddInt32(&number, 1)
    // 执行完一次，就将wg其内部值进行-1操作
	wg.Done()
}

func subNumber() {
	// number--
	// 使用原子操作-1，保证同步
	atomic.AddInt32(&number, -1)
    // 执行完一次，就将wg其内部值进行-1操作
	wg.Done()
}

// 原子操作
func main() {
	// 进行100次的自增和自减
	for i := 0; i < 100; i++ {
		// 将wg其内部值+1，因为即将要开启一个协程去执行add操作
		wg.Add(1)
		go addNumber()

		// 将wg其内部值+1，因为即将要开启一个协程去执行sub操作
		wg.Add(1)
		go subNumber()
	}
    // 等待所有协程执行完毕
	wg.Wait()
    // 输出最终的number结果(如果使用原子变量的情况下，可能会出现number不等于100的情况)
	fmt.Printf("number: %v\n", number)
}
```

```go
number: 100
```

#### 5.2、常见操作

```go
package main

import (
	"fmt"
	"sync/atomic"
)

func main() {
	// atomic中的一些操作
	var n int64 = 32

	// 在n的基础上+10
	atomic.AddInt64(&n, 10)
	fmt.Printf("n: %v\n", n)

	fmt.Println("--------------------")

	// 读取n的值
	i := atomic.LoadInt64(&n)
	fmt.Printf("i: %v\n", i)

	fmt.Println("--------------------")

	// 存储n的值为100
	atomic.StoreInt64(&n, 100)
	fmt.Printf("n: %v\n", n)

	fmt.Println("--------------------")

	// cas操作 compare and swap 是保证原子性的基础
	// 在此之前，n的值为100，传入的old值也为100，所以此时能够修改成功
	b := atomic.CompareAndSwapInt64(&n, 100, 200)
	fmt.Printf("是否更改成功？: %v\n", b)

	fmt.Println("--------------------")

	// 在执行这行时，n已经是200了，而传入的old值为100，两者不相等，所以这里会修改失败
	b = atomic.CompareAndSwapInt64(&n, 100, 150)
	fmt.Printf("是否更改成功？: %v\n", b)
}
```

```go
n: 42
--------------------
i: 42
--------------------
n: 100
--------------------
是否更改成功？: true
--------------------
是否更改成功？: false
```



### 6、select

> select 是Go语言中的一个控制结构，类似于switch语句，用于处理异步IO事件。select 会监听 channel 中的读写操作。select 中的 case 语句必须是一个channel操作。
>
> 1、如果有多个case语句可以运行，select 会随机的选出一个case语句进行执行。
>
> 2、如果没有可运行的case语句，且有default语句，则会执行default语句。
>
> 3、如果没有可运行的case语句，且没有default语句，则select会阻塞住，直到某个case可以运行。

```go
package main

import (
	"fmt"
	"time"
)

var chanInt = make(chan int)
var chanStr = make(chan string)

func main() {
	fmt.Println("start...")

	// 开启一个协程，1s后往chanInt发送一条消息。
	go func() {
		fmt.Println("协程: 1s后往chanInt发送消息")
		// 让协程睡眠1s
		time.Sleep(time.Second * 1)
		// 向通道发送数据
		chanInt <- 10
	}()

	// 开启一个协程，2s后往chanStr发送一条消息。
	go func() {
		fmt.Println("协程: 2s后往chanStr发送消息")
		// 让协程睡眠2s
		time.Sleep(time.Second * 2)
		// 向通道发送数据
		chanStr <- "hello world!"
	}()

	// 死循环
	for {
		// 监听channel的读写操作
		select {
		case i := <-chanInt:
			fmt.Printf("i: %v\n", i)
		case s := <-chanStr:
			fmt.Printf("s: %v\n", s)
			// default:
			// 	fmt.Println("default...")
		}
	}
    
    // 上面select阻塞住了，这里不会打印
	fmt.Println("end...")
}
```

```go
start...
协程: 2s后往chanStr发送消息
协程: 1s后往chanInt发送消息

// 1s后通道chanInt接收了消息，所以select监听到了该事件，打印了i=10
i: 10
// 2s后通道chanStr接收了消息，所以select监听到了该事件，打印了s=hello world!
s: hello world!

// 由于两个协程已经结束，chanInt和chanStr不会再触发读写事件了，而select中也没有default语句，所以程序会一直阻塞，造成了死锁。
fatal error: all goroutines are asleep - deadlock!
```

### 7、Timer

> Timer 是一个定时功能，表示多少秒后执行某个动作。

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	fmt.Printf("before: %v\n", time.Now())

	// 创建timer对象，指定2s延迟，2s后执行
	timer := time.NewTimer(time.Second * 2)

	// timer对象中的C属性，是一个channel类型，且是无缓冲，同步的方式。
	// 通过同步获取channel中的数据，实现2s阻塞，从而达到2s延迟的效果。
	// 具体为: 当获取channel中的数据时，由于暂时没有数据，所以会阻塞住。
	// 当2s过后，channel中发送数据了，所以继续往下运行。
	<-timer.C

	// Reset()表示重置时间
	// timer.Reset(time.Second * 1) // 再间隔1s
	// <-timer.C

	// 2s后执行
	fmt.Printf("after: %v\n", time.Now())

	// Stop() 表示停止timer
	// timer.Stop()

}
```

```go
before: 2022-11-16 14:05:53.6631316 +0800 CST m=+0.004601401
// 间隔2s
after: 2022-11-16 14:05:55.6991968 +0800 CST m=+2.040666601
```



### 8、Ticker

> Ticker 也是一个定时功能，表示每隔多少秒循环执行某个动作。

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	// 创建ticker对象，指定每隔2s执行
	ticker := time.NewTicker(time.Second * 2)

	// ticker.C 是一个channel，并且其接收的消息类型是Time类型。
	fmt.Printf("ticker.C: %T\n", ticker.C)

	// 对ticker.C进行遍历，实现每隔2s打印一次消息(时间)
	for v := range ticker.C {
		fmt.Printf("v: %v\n", v)
	}
}
```

```go
ticker.C: <-chan time.Time

v: 2022-11-16 13:59:28.7116516 +0800 CST m=+2.011244101
v: 2022-11-16 13:59:30.7120745 +0800 CST m=+4.011667001
v: 2022-11-16 13:59:32.7178907 +0800 CST m=+6.017483201
```

### 9、runtime 

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	// 打印cpu核心数
	fmt.Printf("NumCPU: %v\n", runtime.NumCPU())

	// Goexit的作用是终止当前线程的执行，但在终止前会执行完defer语句
	// runtime.Goexit()

	// 开启一个协程，执行匿名函数
	go func() {
		for i := 0; i < 5; i++ {
			fmt.Printf("协程打印的内容%v...\n", i)
		}
	}()

	// 主线程执行，主线程执行太快，导致协程内的语句来不及执行。
	// 加上这句代码，可以让当前线程(主线程)让出cpu的使用权，协程就可以执行了。
	runtime.Gosched()
	fmt.Println("主线程打印的内容...")
}
```

```go
NumCPU: 4
协程打印的内容0...
协程打印的内容1...
协程打印的内容2...
协程打印的内容3...
协程打印的内容4...
主线程打印的内容...
```



## 六、文件操作

### 1、读文件

文件内容：

```
你好，world!
```

代码：

```go
package main

import (
	"fmt"
	"io"
	"log"
	"os"
)

// 读取文件
func readFile() {
	// 打开文件，返回File对象，实现了Reader，Writer接口
	// 第一个参数：文件名
	// 第二个参数：操作文件的类型，这里是只读
	// 第三个参数：文件的权限
	f, err := os.OpenFile("a.txt", os.O_RDONLY, os.ModePerm)
	if err != nil {
		log.Fatal(err)
	}
	// 创建字节切片：大小为3个字节
	buf := make([]byte, 3)
	// 循环读取内容
	for {
		// 3个字节读，n为实际读取的字节数
		n, err2 := f.Read(buf)
		// 如果读取到末尾，则结束
		if err2 == io.EOF {
			break
		}
		// 输出读取的内容
		fmt.Printf("读取的字节内容: %v\n", buf[0:n])
		fmt.Printf("转换为字符串的内容: %v\n", string(buf[0:n]))
		fmt.Println("------------------------")
	}
}

func main() {
	// 读取文件内容
	readFile()
}
```

```go
// 一个中文字符，占3个字节。
读取的字节内容: [228 189 160]
转换为字符串的内容: 你
------------------------
读取的字节内容: [229 165 189]
转换为字符串的内容: 好
------------------------
读取的字节内容: [239 188 140]
转换为字符串的内容: ，
------------------------
读取的字节内容: [119 111 114]
转换为字符串的内容: wor
------------------------
读取的字节内容: [108 100 33]
转换为字符串的内容: ld!
------------------------
```



### 2、写文件

```go
package main

import (
	"fmt"
	"io"
	"log"
	"os"
)

// 写入文件内容
func writeFile(msg string) {
	// 打开文件，返回File对象，实现了Reader，Writer接口
	// 第一个参数：文件名
	// 第二个参数：操作文件的类型，这里是只写、可创建
	// 第三个参数：文件的权限
	// 如果文件不存在，则创建；如果文件存在，则从头开始覆盖写(默认)
	f, err := os.OpenFile("b.txt", os.O_WRONLY|os.O_CREATE, 0666)

	// 如果文件不存在，则创建；如果文件存在，则全覆盖写(os.O_TRUNC)。
	// f, err := os.OpenFile("b.txt", os.O_WRONLY|os.O_CREATE|os.O_TRUNC, 0666)

	// 如果文件不存在，则创建；如果文件存在，则追加写(os.O_APPEND)。
	// f, err := os.OpenFile("b.txt", os.O_WRONLY|os.O_CREATE|os.O_APPEND, 0666)

	if err != nil {
		log.Fatal(err)
	}
	// 写入文件内容
	f.Write([]byte(msg)) // 写入字节
	// f.WriteString(msg) // 写入字符串
	f.Close()
}

func main() {
	// 写入文件内容
	writeFile("你好, world!")
	// 默认的写操作类型，对同一个文件是从头开始覆盖写。
	// 比如这里是先写入"你好, world!"，然后再写入"哈哈"，则最后内容变成"哈哈, world!"。
	writeFile("哈哈")

	// 如果要全部覆盖写,则加上os.O_TRUNC操作类型。
	// 如果要在原基础上追加写,则加上os.O_TRUNC操作类型。
}
```

```go
// 先是写入
你好, world!
// 然后变为
哈哈, world!
```



### 3、复制文件

```go
package main

import (
	"fmt"
	"io"
	"log"
	"os"
)

func copyFile(srcFile string, destFile string) {
	// 读取源文件
	f, _ := os.OpenFile(srcFile, os.O_RDONLY, os.ModePerm)
	defer f.Close() // 延迟关闭文件
	// 写入的目的文件
	dest, _ := os.OpenFile(destFile, os.O_WRONLY|os.O_CREATE|os.O_TRUNC, os.ModePerm)
	defer dest.Close() // 延迟关闭文件

	// 创建buf来读取内容
	buf := make([]byte, 10)

	// 循环读取
	for {
		// n是实际读取的字节数
		n, err := f.Read(buf)
		// 读取结束
		if err == io.EOF {
			break
		}
		// 将实际读取的内容写入目的文件
		dest.Write(buf[0:n])
	}
	fmt.Println("复制完毕!")
}

func main() {
	// 复制文件
	copyFile("a.txt", "a_copy.txt")
}
```

```
复制完毕!
```



### 4、带缓冲IO

> 带缓冲的 `bufio` 可以提高读写的速度。

```go
package main

import (
	"bufio"
	"fmt"
	"io"
	"os"
	"time"
)

func copyFileByIO(srcFile string, destFile string) {
	// 记录开始时间
	startTime := time.Now()

	// 读取源文件
	f, _ := os.OpenFile(srcFile, os.O_RDONLY, os.ModePerm)
	defer f.Close() // 延迟关闭文件
	// 写入的目的文件
	dest, _ := os.OpenFile(destFile, os.O_WRONLY|os.O_CREATE|os.O_TRUNC, os.ModePerm)
	defer dest.Close() // 延迟关闭文件

	// 创建buf来读取内容
	buf := make([]byte, 1024)

	// 循环读取
	for {
		// n是实际读取的字节数
		n, err := f.Read(buf)
		// 读取结束
		if err == io.EOF {
			break
		}
		// 将实际读取的内容写入目的文件
		dest.Write(buf[0:n])
	}
	// 记录花费时间
	costTime := time.Now().Sub(startTime)
	fmt.Printf("costTime: %v\n", costTime)
}

func copyFileByBufIO(srcFile string, destFile string) {
	// 记录开始时间
	startTime := time.Now()

	// 读取源文件
	f, _ := os.OpenFile(srcFile, os.O_RDONLY, os.ModePerm)
	defer f.Close() // 延迟关闭文件
	// 写入的目的文件
	dest, _ := os.OpenFile(destFile, os.O_WRONLY|os.O_CREATE|os.O_TRUNC, os.ModePerm)
	defer dest.Close() // 延迟关闭文件

	// 创建buf来读取内容
	buf := make([]byte, 1024)

	// 将f的Reader转成带缓冲的Reader类型，速度更快（区别）
	r := bufio.NewReader(f)

	// 循环读取
	for {
		// n是实际读取的字节数
		n, err := r.Read(buf)
		// 读取结束
		if err == io.EOF {
			break
		}
		// 将实际读取的内容写入目的文件
		dest.Write(buf[0:n])
	}
	// 记录花费时间
	costTime := time.Now().Sub(startTime)
	fmt.Printf("costTime: %v\n", costTime)
}

func main() {
	srcFile := "D:/test.mp4"
	destFile := "D:/test_copy.mp4"
	// 使用不带缓冲的IO复制大文件 4.28s
	copyFileByIO(srcFile, destFile)
	// 使用带缓冲的BufIO复制大文件 3.26s
	copyFileByBufIO(srcFile, destFile)
}
```

```go
costTime: 4.28s
costTime: 3.26s
```

## 七、常用模块

### 1、os 模块

#### 1.1、获取工作目录

```go
package main

import (
	"fmt"
	"log"
	"os"
)

func GetWorkDir() {
	// 获取当前工作目录
	dir, err := os.Getwd()
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("dir: %v\n", dir)
}

func main() {
	// 获取当前工作目录
	GetWorkDir()
}
```

```go
dir: c:\Users\LIULUSHENG\Desktop\go
```



#### 1.2、创建文件

```go
package main

import (
	"fmt"
	"log"
	"os"
)

func createFile() {
	f, err := os.Create("a.txt")
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("f.Name(): %v\n", f.Name())
}

func createFileByOrigin() {
	// OpenFile() 根据传递的参数，既可以创建文件，也可以读取文件
	// 第一个参数：文件名 第二个参数：文件的操作类型（只写、可以创建） 第三个参数：文件的权限，类似linux
	f, err := os.OpenFile("b.txt", os.O_WRONLY|os.O_CREATE, 0777)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("f.Name(): %v\n", f.Name())
}

func main() {
	// 创建文件
	createFile()

	// 通过最原始的方法创建文件
	createFileByOrigin()
}
```

```go
f.Name(): a.txt
f.Name(): b.txt
```



#### 1.3、创建目录

```go
package main

import (
	"fmt"
	"log"
	"os"
)

func createDir() {
	// 创建目录，指定目录的权限
	err := os.Mkdir("a", os.ModePerm)
	if err != nil {
		log.Fatal(err)
	}
}

func createManyDir() {
	// 创建多级目录
	err := os.MkdirAll("b/c/d", os.ModePerm)
	if err != nil {
		log.Fatal(err)
	}
}

func main() {
	// 创建目录
	createDir()

	// 创建多级目录
	createManyDir()
}
```



#### 1.4、删除文件

```go
package main

import (
	"fmt"
	"log"
	"os"
)

func removeFile() {
	// 删除文件
	err := os.Remove("a.txt")
	if err != nil {
		log.Fatal(err)
	}
}

func main() {
	// 删除文件
	removeFile()
}
```



#### 1.5、删除目录

```go
package main

import (
	"fmt"
	"log"
	"os"
)

func removeDir() {
	// 删除单个目录
	err := os.Remove("a")
	if err != nil {
		log.Fatal(err)
	}
}

func removeAllDir() {
	// 删除多个目录（包括其子目录）
	err := os.RemoveAll("b")
	if err != nil {
		log.Fatal(err)
	}
}

func main() {
	// 删除单个目录
	removeDir()

	// 删除多个目录（包括目录下的子目录）
	removeAllDir()
}
```



#### 1.6、创建临时目录

```go
package main

import (
	"fmt"
	"log"
	"os"
)

func createTempDir() {
	// 创建临时目录
	// 第一个参数：指定创建临时目录的根目录位置，若留空，则默认是操作系统的临时根目录下
	// 第二个参数：临时目录的前缀
	s, err := os.MkdirTemp("", "temp")
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("创建的临时目录: %v\n", s)
}

func main() {
	// 创建临时目录
	// 创建的临时目录: C:\Users\LIULUS~1\AppData\Local\Temp\temp3343246708
	createTempDir()
}
```

```go
创建的临时目录: C:\Users\LIULUS~1\AppData\Local\Temp\temp3343246708
```

#### 1.7、读取文件

```go
package main

import (
	"fmt"
	"log"
	"os"
)

func readFile() {
	// 读取文件内容，返回的是字节切片
	b, err := os.ReadFile("a.txt")
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("读取的文件内容为：%v\n", string(b))
}

func main() {
	// 读取文件
	readFile()
}
```

```go
读取的文件内容为：你好，world!
```



#### 1.8、读取目录

```go
package main

import (
	"fmt"
	"log"
	"os"
)

func readDir() {
	// 读取目录内容，返回的是目录下的所有内容（文件和目录）的结点切片[]DirEntry
	de, err := os.ReadDir("b")
	if err != nil {
		log.Fatal(err)
	}
	// 遍历结点切片
	for _, v := range de {
		// 打印每一个内容结点
		fmt.Printf("name: %v \t is_dir: %v\n", v.Name(), v.IsDir())
	}
}

func main() {
	// 读取目录信息
	readDir()
}
```

```go
name: c 			 is_dir: true
name: hello.txt 	 is_dir: false
```

#### 1.9、环境变量

```go
package main

import (
	"fmt"
	"log"
	"os"
)

func readOSEnv() {
	// 读取系统环境
	s := os.Environ()
	for _, v := range s {
		fmt.Printf("v: %v\n", v)
	}
}

func main() {
	// 读取系统环境
	// readOSEnv()

    // 获取系统变量JAVA_HOME
	s := os.Getenv("JAVA_HOME")
	fmt.Printf("s: %v\n", s)

	// 查不到对应的环境变量时，只会返回空串
	s = os.Getenv("GO_PATH")
	fmt.Printf("s: %v\n", s)

	// LookupEnv相对于Getenv，会返回是否存在该变量的提示
	s2, b := os.LookupEnv("GO_PATH")
	if !b {
		fmt.Println("未找到该环境变量")
	} else {
		fmt.Printf("s2: %v\n", s2)
	}

	// 设置环境变量
	err := os.Setenv("key", "value")
	if err != nil {
		log.Fatal(err)
	}

	// 移除某个环境变量
	err = os.Unsetenv("key")
	if err != nil {
		log.Fatal(err)
	}

	// 清除环境变量
	// os.Clearenv()
}
```

```go
s: D:\JDK\jdk1.8
// 空串
s: 
// 有提示
未找到该环境变量
```



### 2、io 模块

```go
package main

import (
	"fmt"
	"io"
	"os"
)

func testReadAll() {
	f, _ := os.OpenFile("a.txt", os.O_RDONLY, 777)
	defer f.Close()
	// 读取文件的所有字节
	b, _ := io.ReadAll(f)
	fmt.Printf("string(b): %v\n", string(b))
}

func testWrite() {
	// 追加写
	f, _ := os.OpenFile("a.txt", os.O_WRONLY|os.O_APPEND, 777)
	defer f.Close()

	// n是实际写入的字节数
	n, _ := io.WriteString(f, "\n")
	fmt.Printf("n: %v\n", n)

	// n是实际写入的字节数
	n, _ = io.WriteString(f, "你好！")
	fmt.Printf("n: %v\n", n)
}

func main() {
	// 读取文件的所有内容
	testReadAll()
	
    fmt.Println("----------------------")
    
	// 往文件写内容
	testWrite()
}
```

```go
string(b): 你好，world!
----------------------
n: 1
n: 9
```

### 3、bytes 模块

```go
package main

import (
	"bytes"
	"fmt"
)

func main() {
	// 将字符串转成带有缓冲的字节切片
	b := bytes.NewBufferString("你好, world!")
	fmt.Printf("b: %T\n", b)
	fmt.Printf("b: %v\n", b)

	fmt.Println("-------------------------------")

	s1 := []byte("hello world!")
	s2 := []byte("hello")

	// 判断s1字节切片中是否包含s2字节切片
	// 相等于判断s1中的字符串是否包含s2的字符串
	fmt.Printf("bytes.Contains(s1, s2): %v\n", bytes.Contains(s1, s2))

	fmt.Println("-------------------------------")

	// 统计s1字节切片中出现h的次数
	fmt.Printf("bytes.Count(s1, []byte('h')): %v\n", bytes.Count(s1, []byte("h")))
	// 统计s1字节切片中出现l的次数
	fmt.Printf("bytes.Count(s1, []byte('l')): %v\n", bytes.Count(s1, []byte("l")))

	fmt.Println("-------------------------------")

	// 将s1字节切片重复多次
	fmt.Printf("string(bytes.Repeat(s1, 1)): %v\n", string(bytes.Repeat(s1, 1)))
	fmt.Printf("string(bytes.Repeat(s1, 3)): %v\n", string(bytes.Repeat(s1, 3)))
}
```

```go
b: *bytes.Buffer
b: 你好, world!
-------------------------------
bytes.Contains(s1, s2): true
-------------------------------
bytes.Count(s1, []byte('h')): 1
bytes.Count(s1, []byte('l')): 3
-------------------------------
string(bytes.Repeat(s1, 1)): hello world!
string(bytes.Repeat(s1, 3)): hello world!hello world!hello world!
```



### 4、log 模块

> log 模块是专门用来记录日志的，可以写入日志文件。

#### 4.1、普通打印

```go
package main

import (
	"fmt"
	"log"
	"os"
)

// 可以自定义打印日志的格式
// 这个是初始化方法，自动调用
func init() {
	// 设置日志格式: 日期 时间 文件
	log.SetFlags(log.Ldate | log.Ltime | log.Lshortfile)
	// 设置日志的前缀
	log.SetPrefix("MyLog: ")

	// 日志文件
	f, _ := os.OpenFile("log.txt", os.O_WRONLY|os.O_CREATE|os.O_APPEND, os.ModePerm)
	// 设置日志的输出位置，默认是控制台，也可以是文件
	log.SetOutput(f)
}

func main() {
	// 普通打印
	log.Println("我是普通日志")
}
```

```go
// 文件中记录了日志
MyLog: 2022/11/16 15:20:44 test_log.go:24: 我是普通日志
```



#### 4.2、Panic 打印

```go
package main

import (
	"fmt"
	"log"
	"os"
)

// 可以自定义打印日志的格式
// 这个是初始化方法，自动调用
func init() {
	// 设置日志格式: 日期 时间 文件
	log.SetFlags(log.Ldate | log.Ltime | log.Lshortfile)
	// 设置日志的前缀
	log.SetPrefix("MyLog: ")

	// 日志文件
	f, _ := os.OpenFile("log.txt", os.O_WRONLY|os.O_CREATE|os.O_APPEND, os.ModePerm)
	// 设置日志的输出位置，默认是控制台，也可以是文件
	log.SetOutput(f)
}

func main() {

	defer fmt.Println("我是延迟打印的内容...")

	// Panic打印，会抛出异常，程序会中断，但defer语句还是会执行。
	log.Panic("出错了...")
	// 这行语句不会打印，因为上面已经抛异常了，程序中断。
	fmt.Println("我是panic打印后的内容...")
}
```

```go
我是延迟打印的内容...
panic: 出错了...

goroutine 1 [running]:
log.Panic({0xc00007bf30?, 0x60?, 0x302d00?})
	D:/go/src/log/log.go:388 +0x65
main.main()
	c:/Users/LIULUSHENG/Desktop/go/test_log.go:30 +0x94
exit status 2

// 同时文件中写入了日志
MyLog: 2022/11/16 15:22:03 test_log.go:30: 出错了...
```



#### 4.3、Fatal 打印

```go
package main

import (
	"fmt"
	"log"
	"os"
)

// 可以自定义打印日志的格式
// 这个是初始化方法，自动调用
func init() {
	// 设置日志格式: 日期 时间 文件
	log.SetFlags(log.Ldate | log.Ltime | log.Lshortfile)
	// 设置日志的前缀
	log.SetPrefix("MyLog: ")

	// 日志文件
	f, _ := os.OpenFile("log.txt", os.O_WRONLY|os.O_CREATE|os.O_APPEND, os.ModePerm)
	// 设置日志的输出位置，默认是控制台，也可以是文件
	log.SetOutput(f)
}

func main() {
	defer fmt.Println("我是延迟打印的内容...")

	// Fatal打印，会导致程序中断，而且defer语句也执行不了。
	log.Fatalln("出大错了...")
	// 这行语句不会打印，因为上面程序已经中断了。
	fmt.Println("我是fatal打印后的内容...")
}
```

```go
// 程序直接中断
exit status 1

// 同时文件中记录了该日志
MyLog: 2022/11/16 15:24:26 test_log.go:35: 出大错了...
```



### 5、errors 模块

> errors 模块可以快速的自定义异常信息。

```go
package main

import (
	"errors"
	"fmt"
	"log"
)

func check(msg string) error {
	if msg == "" {
		return errors.New("消息不能为空")
	} else {
		return nil
	}
}

// 自定义错误类型
type MyError struct {
	msg string
}

// 自定义错误类型需要实现error接口
func (e *MyError) Error() string {
	return e.msg
}

func testMyError() error {
	return &MyError{"我是自定义的错误类型消息..."}
}

func main() {
	// 测试错误
	// 校验传入的参数是否为空串
	err := check("")
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println("验证通过")
	}

	fmt.Println("----------------------")

	err = check("java")
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println("验证通过")
	}

	fmt.Println("----------------------")

	// 测试自定义的错误
	err = testMyError()
	if err != nil {
		log.Println(err)
	}
}
```

```go
消息不能为空
----------------------
验证通过
----------------------
2022/11/16 15:34:26 我是自定义的错误类型消息...
```



### 6、sort 模块

> sort 模块可以对Go语言中的数据类型进行排序，同时也能够自定义数据类型排序。参照 sort 模块内的代码逻辑。

#### 6.1、基本类型排序

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	// 待排序整型切片
	arr := []int{1, 3, 1, 2, 5, 4}
	fmt.Printf("arr: %v\n", arr)
	// 排序
	sort.Ints(arr)
	fmt.Printf("arr: %v\n", arr)

	fmt.Println("-----------------")

	// 待排序浮点数切片
	arr2 := []float64{1.1, 2.2, 0.4, 1.5, 3.4}
	fmt.Printf("arr2: %v\n", arr2)
	// 排序
	sort.Float64s(arr2)
	fmt.Printf("arr2: %v\n", arr2)

	fmt.Println("-----------------")

	// 查看是否已经排好序
	b := sort.Float64sAreSorted(arr2)
	fmt.Printf("b: %v\n", b)

	// 查看是否已经排好序
	arr3 := []float64{1.1, 2.2, 0.4, 1.5, 3.4}
	b = sort.Float64sAreSorted(arr3)
	fmt.Printf("b: %v\n", b)
}
```

```go
arr: [1 3 1 2 5 4]
arr: [1 1 2 3 4 5]
-----------------
arr2: [1.1 2.2 0.4 1.5 3.4]
arr2: [0.4 1.1 1.5 2.2 3.4]
-----------------
b: true
b: false
```



#### 6.2、自定义类型排序

```go
package main

import (
	"fmt"
	"sort"
)

// 1、自定义类型排序(二维切片)
type MultiArr [][]int

// 自定义的类型要实现排序功能，必须实现sort中的Interface接口
func (a MultiArr) Len() int {
	return len(a)
}
func (a MultiArr) Less(i, j int) bool {
	// 对切片中i、j元素中的第一个元素大小进行比较
	return a[i][0] < a[j][0]
}
func (a MultiArr) Swap(i, j int) {
	// 交换切片中i、j两个元素
	a[i], a[j] = a[j], a[i]
}

// 2、定义一个学生结构体
type Student struct {
	name string
	age  int
}

// 2、定义一个学生切片类型
type StudentSlice []Student

// 实现sort中的Interface接口，从而实现排序功能
func (s StudentSlice) Len() int {
	return len(s)
}

func (s StudentSlice) Less(i, j int) bool {
	// 比较切片中i、j元素的age值大小
	return s[i].age < s[j].age
}

func (s StudentSlice) Swap(i, j int) {
	// 交换两个元素
	s[i], s[j] = s[j], s[i]
}

func main() {
	// 1、自定义排序一
	// 创建一个二维的切片
	arr := [][]int{[]int{1, 2}, []int{4, 3}, []int{2, 4}}
	// 将二维的切片类型转换成MultiArr自定义类型
	r := MultiArr(arr)
	fmt.Printf("r: %v\n", r)
	// 排序（按照每个切片中的第一个元素大小进行排序）
	sort.Sort(r)
	fmt.Printf("r: %v\n", r)

	fmt.Println("--------------------------")

	// 2、自定义排序二
	// 创建结构体切片
	studentArr := []Student{
		Student{"张三", 18},
		Student{"李四", 16},
		Student{"王五", 20},
	}
	// 转成StudentSlice类型
	students := StudentSlice(studentArr)
	fmt.Printf("students: %v\n", students)
	// 对切片中的student结构体按照age进行排序
	sort.Sort(students)
	fmt.Printf("students: %v\n", students)
}
```

```go
// 按切片中每个元素中的第一个元素进行排序
r: [[1 2] [4 3] [2 4]]
r: [[1 2] [2 4] [4 3]]
--------------------------
// 按切片中每个student元素的年龄进行排序
students: [{张三 18} {李四 16} {王五 20}]
students: [{李四 16} {张三 18} {王五 20}]
```



### 7、time 模块

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	// 原样输出
	fmt.Printf("t: %v\n", t)

	// 普通格式化输出
	fmt.Printf("%d-%02d-%02d %02d:%02d:%02d\n", t.Year(), t.Month(),
		t.Day(), t.Hour(), t.Minute(), t.Second())

	fmt.Println("---------------------------------")

	// 时间戳
	fmt.Printf("time.Now().UnixMilli(): %v\n", time.Now().UnixMilli())

	fmt.Println("---------------------------------")

	// 1、日期格式化
	// go语言中的格式化和其他语言不同，是采用具体的日期形式进行格式化。
	// 采用的是go的开发日期。2006年1月2日 15时04分05秒 (记忆方式 2006 1 2 3 4 5)
	fmt.Printf("t.Format(): %v\n", t.Format("2006-01-02 15:04:05")) // 24小时制

	fmt.Println("---------------------------------")

	// 2、字符串解析为日期
	// 第一个参数：字符串格式
	// 第二个参数：需要解析的字符串日期
	t2, _ := time.Parse("2006-01-02 15:04:05", "2022-11-10 12:33:10")
	// 带时区的解析，第三个参数为时区，默认是UTC
	location, _ := time.LoadLocation("Asia/Shanghai")
	t3, _ := time.ParseInLocation("2006-01-02 15:04:05", "2022-11-10 12:33:10", location)
	fmt.Printf("t2: %v\n", t2)
	fmt.Printf("t3: %v\n", t3)
}
```

```go
t: 2022-11-16 16:37:08.7704565 +0800 CST m=+0.005225401
2022-11-16 16:37:08
---------------------------------
time.Now().UnixMilli(): 1668587828805
---------------------------------
t.Format(): 2022-11-16 16:37:08
---------------------------------
t2: 2022-11-10 12:33:10 +0000 UTC
t3: 2022-11-10 12:33:10 +0800 CST
```



### 8、json 模块

> 用于对象的序列化，将内存中的对象转换为 `json` 字符串，也能够将 `json` 字符串转换为内存中的对象。

```go
package main

import (
	"encoding/json"
	"fmt"
	"os"
)

// 定义结构体
// 结构体的字段名首字母必须大写，因为首字母小写代表私有字段，不能被json转换或者解析
type Student struct {
	Id   int    `json:"id"` // 这里可以指定json格式化的字段名称
	Name string `json:"name"`
	Age  int    `json:"age"`
}

func structToJsonstring() {
	student := Student{
		Id:   1001,
		Name: "张三",
		Age:  20,
	}
	// 将结构体转换成json字符串，返回的是字节切片
	b, _ := json.Marshal(student)
	fmt.Printf("string(b): %v\n", string(b))

	// 转换成json字符串，并且指定缩进格式
	b, _ = json.MarshalIndent(student, "", "    ")
	fmt.Printf("string(b): %v\n", string(b))
}

func jsonstringToStruct() {
	// json字符串
	json_str := `{"id":1001,"name":"张三","age":20}`
	// 转换成字节切片
	b := []byte(json_str)

	// 声明student对象
	var student Student
	fmt.Printf("student: %v\n", student)

	// 将json字符串转换成结构体对象，赋值给student
	json.Unmarshal(b, &student)
	fmt.Printf("student: %v\n", student)
}

func writeToJsonFile() {
	student := Student{
		Id:   1001,
		Name: "张三",
		Age:  20,
	}
	// 将结构体转换成json字符串
	// b, _ := json.Marshal(student)
	b, _ := json.MarshalIndent(student, "", "    ")

	// 将json字符串写入文件
	os.WriteFile("test.json", b, os.ModePerm)
}

func readFromJsonFile() {
	// 读取json文件内容
	b, _ := os.ReadFile("test.json")

	var student Student
	fmt.Printf("student: %v\n", student)

	// 将json字符串解析成结构体，赋值给student
	json.Unmarshal(b, &student)
	fmt.Printf("student: %v\n", student)
}

func main() {
	// 结构体转成json字符串
	structToJsonstring()

	// json字符串转换成结构体对象
	jsonstringToStruct()

	// 将结构体转成json字符串，写入文件
	writeToJsonFile()

	// 从json文件中读取内容，解析成结构体
	readFromJsonFile()
}
```

控制台输出：

```go
// 结构体对象转成json字符串
string(b): {"id":1001,"name":"张三","age":20}
string(b): {
    "id": 1001,
    "name": "张三",
    "age": 20
}
// json字符串转成结构体对象
student: {0  0}
student: {1001 张三 20}

// 从json文件中读取json字符串，转成结构体对象
student: {0  0}
student: {1001 张三 20}
```

写入文件的内容：

```json
{
    "id": 1001,
    "name": "张三",
    "age": 20
}
```



### 9、xml 模块

> 用于对象的序列化，将内存中的对象转换为 `xml` 格式字符串，也能够将 `xml` 格式字符串转换为内存中的对象。

```go
package main

import (
	"encoding/xml"
	"fmt"
	"os"
)

// 定义结构体
// 结构体的字段名首字母必须大写，因为首字母小写代表私有字段，不能被xml转换或者解析
type Student struct {
	XMLName xml.Name `xml:"student"` // 指定xml格式化的根结点名称
	Id      int      `xml:"id"`      // 这里可以指定xml格式化的字段名称
	Name    string   `xml:"name"`
	Age     int      `xml:"age"`
}

func structToXmlstring() {
	student := Student{
		Id: 1001, Name: "张三", Age: 20}

	// 将结构体转换成为xml字符串
	b, _ := xml.Marshal(student)
	fmt.Printf("string(b): %v\n", string(b))

	// 将结构体转换成为xml字符串（带格式的）
	b, _ = xml.MarshalIndent(student, "", "    ")
	fmt.Printf("string(b): %v\n", string(b))
}

func xmlstringToStruct() {
	// xml字符串
	xml_str := `<student><id>1001</id><name>张三</name><age>20</age></student>`
    // 字符串转成字节切片
	b := []byte(xml_str)

	// 声明student结构体
	var student Student
	fmt.Printf("student: %v\n", student)

	// 将xml字符串解析成student结构体对象
	xml.Unmarshal(b, &student)
	fmt.Printf("student: %v\n", student)
}

func writeToXmlFile() {
	student := Student{
		Id: 1001, Name: "张三", Age: 20}

	// 将结构体转换成为xml字符串（带格式的）
	b, _ := xml.MarshalIndent(student, "", "    ")
    
	// 将xml字符串写入文件
	os.WriteFile("test.xml", b, os.ModePerm)
}

func readFromXmlFile() {
	// 读取xml字符串内容
	b, _ := os.ReadFile("test.xml")

	// 声明student结构体
	var student Student
	fmt.Printf("student: %v\n", student)

	// 将xml字符串解析成student结构体对象
	xml.Unmarshal(b, &student)
	fmt.Printf("student: %v\n", student)
}

func main() {
	// 将结构体转成xml字符串
	structToXmlstring()

	// 将xml字符串转成结构体
	xmlstringToStruct()

	// 将结构体转成xml字符串，写入文件
	writeToXmlFile()

	// 从文件中读取xml内容，解析成结构体
	readFromXmlFile()
}
```

控制台输出：

```go
// 结构体对象转成xml字符串
string(b): <student><id>1001</id><name>张三</name><age>20</age></student>
string(b): 
<student>
    <id>1001</id>
    <name>张三</name>
    <age>20</age>
</student>

// xml字符串转成结构体对象
student: {{ } 0  0}
student: {{ student} 1001 张三 20}

// 从xml文件中读取xml字符串，转成结构体对象
student: {{ } 0  0}
student: {{ student} 1001 张三 20}
```

写入文件的内容：

```xml
<student>
    <id>1001</id>
    <name>张三</name>
    <age>20</age>
</student>
```

### 10、math 模块

```go
package main

import (
	"fmt"
	"math"
	"math/rand"
)

func main() {
	// 常规使用
	fmt.Printf("math.Pi: %v\n", math.Pi)
	fmt.Printf("math.MaxInt16: %v\n", math.MaxInt16)

	fmt.Println("------------------------")

	// 求余数
	fmt.Printf("math.Mod(10, 3): %v\n", math.Mod(10, 3))
	// 返回一个数的整数部分和小数部分
	int2, frac := math.Modf(12.8)
	fmt.Printf("int2: %v\n", int2)
	fmt.Printf("frac: %v\n", frac)

	fmt.Println("------------------------")

	// 开平方根
	fmt.Printf("math.Sqrt(16): %v\n", math.Sqrt(16))
	// 开三次方根
	fmt.Printf("math.Cbrt(27): %v\n", math.Cbrt(27))
	// 求10的n次方
	fmt.Printf("math.Pow10(2): %v\n", math.Pow10(2))
	// 求x的y次方
	fmt.Printf("math.Pow(2, 3): %v\n", math.Pow(2, 3))

	fmt.Println("------------------------")

	// 取上整
	fmt.Printf("math.Ceil(10.2): %v\n", math.Ceil(10.2))
	// 取下整
	fmt.Printf("math.Floor(10.2): %v\n", math.Floor(10.2))
	// 取整数部分
	fmt.Printf("math.Trunc(10.2): %v\n", math.Trunc(10.2))

	fmt.Println("------------------------")

	// 设置随机种子
	rand.Seed(10)
	// 生成10以内的随机整数
	fmt.Printf("rand.Intn(10): %v\n", rand.Intn(10))
	// 生成0-1以内的随机浮点数
	fmt.Printf("rand.Float32(): %v\n", rand.Float32())
}
```

```go
math.Pi: 3.141592653589793
math.MaxInt16: 32767
------------------------
math.Mod(10, 3): 1
int2: 12
frac: 0.8000000000000007
------------------------
math.Sqrt(16): 4
math.Cbrt(27): 3
math.Pow10(2): 100
math.Pow(2, 3): 8
------------------------
math.Ceil(10.2): 11
math.Floor(10.2): 10
math.Trunc(10.2): 10
------------------------
rand.Intn(10): 4
rand.Float32(): 0.417652
```



## 八、补充

> Go语言中原生操作 `MySQL` 数据库的方法如下。另外，`GORM` 封装了 `MySQL` 等多种数据库的快捷操作方式。这里主要学习了原生操作，`ORM` 框架以后有需要再学习。

操作前，需要先下载 `MySQL` 驱动，这里去 [官网](https://pkg.go.dev/) 下载驱动。

```go
// 初始化项目模块
go mod init 项目名

// 删除项目中不需要的依赖，同时加入项目中需要的依赖到go.mod文件中
go mod tidy

// 下载MySQL驱动
go get -u github.com/go-sql-driver/mysql
```

### 1 、测试连接

```go
package main

import (
	"database/sql"
	"fmt"
	"log"

	_ "github.com/go-sql-driver/mysql"
)

func main() {
	// "用户名:密码@tcp(ip:port)/数据库名"
	// db, err := sql.Open("mysql", "root:root123@tcp(localhost:3306)/flea")
	db, err := sql.Open("mysql", "root:root123@/test") // 可简写使用默认值
	if err != nil {
		log.Fatal(err)
	}

	// fmt.Printf("db: %v\n", db)
	// db能够ping通，才表示连接没有问题
	fmt.Printf("db.Ping(): %v\n", db.Ping())
	if db.Ping() == nil {
		log.Println("连接成功")
	}
}
```

```go
db.Ping(): <nil>
2022/11/16 14:20:19 连接成功
```

### 2 、插入数据

```go
package main

import (
	"database/sql"
	"fmt"
	"log"
	"time"

	_ "github.com/go-sql-driver/mysql"
)

// 声明db指针变量
var db *sql.DB

// 初始化数据库连接
func initDB() (err error) {
	// 尝试打开
	db, err = sql.Open("mysql", "root:root123@/test")
	if err != nil {
		return err
	}
	// 尝试与数据库建立连接，检测url是否错误
	err = db.Ping()
	if err != nil {
		return err
	}
	// 设置连接的时长
	db.SetConnMaxLifetime(time.Minute * 5)
	return nil
}

func insert(name string, age int) {
	sql := "insert into user(id, name, age) values(default, ?, ?)"
	// 执行插入语句，同时设置执行参数
	r, err := db.Exec(sql, name, age)
	if err != nil {
		log.Fatal(err)
	}
	// 返回插入记录的ID
	insertId, _ := r.LastInsertId()
	fmt.Printf("insertId: %v\n", insertId)
	// 返回受影响的行数
	affectRows, _ := r.RowsAffected()
	fmt.Printf("affectRows: %v\n", affectRows)
	fmt.Println("-------------------------------")
}

func main() {
	// 初始化数据库连接
	err := initDB()
	if err != nil {
		log.Fatal(err)
	}

	// 执行插入语句
	insert("李四", 21)
	insert("王五", 20)
}
```

```
insertId: 8
affectRows: 1
-------------------------------

insertId: 9
affectRows: 1
-------------------------------
```



### 3 、查询数据

```go
package main

import (
	"database/sql"
	"fmt"
	"log"
	"time"

	_ "github.com/go-sql-driver/mysql"
)

// 声明db变量
var db *sql.DB

// 初始化数据库连接
func initDB() (err error) {
	// 尝试打开数据库
	// 如果数据库中涉及到时间类型，则这里必须指定parseTime=True，否则不能映射到结构体中
	db, err = sql.Open("mysql", "root:root123@/test?parseTime=True")
	if err != nil {
		return err
	}
	// 尝试与数据库建立连接，检测url是否错误
	err = db.Ping()
	if err != nil {
		return err
	}
	// 设置连接的时长
	db.SetConnMaxLifetime(time.Minute * 5)
	return nil
}

// 声明一个和数据库表对应的结构体
type User struct {
	id          int
	name        string
	age         int
	create_time time.Time
}

func queryById(id int) {
	sql := "select * from user where id = ?"
	// 执行查询语句，同时设置查询参数
	r, err := db.Query(sql, id)
	if err != nil {
		log.Fatal(err)
	}
	// 声明user结构体对象，用来接收查询的数据
	var user User
	// 如果有数据，则循环，否则结束循环。按每一行数据来循环
	for r.Next() {
		// 将查询结果赋值给user结构体对象
		err2 := r.Scan(&user.id, &user.name, &user.age, &user.create_time)
		if err2 != nil {
			log.Fatal(err2)
		}
		fmt.Printf("user: %v\n", user)
		// 打印查询到的时间（格式化）
		fmt.Printf("user.create_time: %v\n", user.create_time.Format("2006-01-02 15:04:05"))
	}
	fmt.Println("------------------------------------------")
}

func queryAll() {
	sql := "select * from user"
	// 执行查询语句
	r, err := db.Query(sql)
	if err != nil {
		log.Fatal(err)
	}
	// 声明user结构体对象切片，用来接收查询的数据
	userList := make([]User, 0)
	// 如果有数据，则循环，否则结束循环。按每一行数据来循环
	for r.Next() {
		// 声明user结构体对象
		var user User
		// 将查询结果赋值给user结构体对象
		err2 := r.Scan(&user.id, &user.name, &user.age, &user.create_time)
		if err2 != nil {
			log.Fatal(err2)
		}
		// 将结果添加至切片中
		userList = append(userList, user)
	}
	fmt.Printf("userList: %v\n", userList)
	fmt.Println("------------------------------------------")
}

func main() {
	// 初始化数据库连接
	err := initDB()
	if err != nil {
		log.Fatal(err)
	}

	// 根据ID查询记录
	queryById(3)

	// 查询全部记录
	queryAll()
}

```

```go
user: {3 张三 20 {0 63803868061 <nil>}}
user.create_time: 2022-11-12 16:41:01
------------------------------------------
userList: [{3 张三 20 {0 63803868061 <nil>}} {4 测试名称 20 {0 63803869193 <nil>}} {5 王五 20 {0 63803869193 <nil>}} {6 李四 21 {0 63803869256 <nil>}} {8 李四 21 {0 63804205485 <nil>}} {9 王五 20 {0 63804205485 <nil>}}]
------------------------------------------
```



### 4 、更新数据

```go
package main

import (
	"database/sql"
	"fmt"
	"log"
	"time"

	_ "github.com/go-sql-driver/mysql"
)

// 声明db变量
var db *sql.DB

// 初始化数据库连接
func initDB() (err error) {
	// 尝试打开
	db, err = sql.Open("mysql", "root:root123@/test")
	if err != nil {
		return err
	}
	// 尝试与数据库建立连接，检测url是否错误
	err = db.Ping()
	if err != nil {
		return err
	}
	// 设置连接的时长
	db.SetConnMaxLifetime(time.Minute * 5)
	return nil
}

func updateById(id int, name string, age int) {
	sql := "update user set name = ?, age = ? where id = ?"
	// 执行更新语句，同时设置执行参数
	r, err := db.Exec(sql, name, age, id)
	if err != nil {
		log.Fatal(err)
	}
	// 返回受影响的行数
	affectRows, _ := r.RowsAffected()
	fmt.Printf("affectRows: %v\n", affectRows)
}

func main() {
	// 初始化数据库连接
	err := initDB()
	if err != nil {
		log.Fatal(err)
	}

	// 根据ID执行更新语句
	updateById(4, "测试名称", 20)
}
```

```go
affectRows: 1
```



### 5、 删除数据

```go
package main

import (
	"database/sql"
	"fmt"
	"log"
	"time"

	_ "github.com/go-sql-driver/mysql"
)

// 声明db变量
var db *sql.DB

// 初始化数据库连接
func initDB() (err error) {
	// 尝试打开
	db, err = sql.Open("mysql", "root:root123@/test")
	if err != nil {
		return err
	}
	// 尝试与数据库建立连接，检测url是否错误
	err = db.Ping()
	if err != nil {
		return err
	}
	// 设置连接的时长
	db.SetConnMaxLifetime(time.Minute * 5)
	return nil
}

func deleteById(id int) {
	sql := "delete from user where id = ?"
	// 执行删除语句，同时设置执行参数
	r, err := db.Exec(sql, id)
	if err != nil {
		log.Fatal(err)
	}
	// 返回受影响的行数
	affectRows, _ := r.RowsAffected()
	fmt.Printf("affectRows: %v\n", affectRows)
}

func main() {
	// 初始化数据库连接
	err := initDB()
	if err != nil {
		log.Fatal(err)
	}

	// 根据ID删除对应的记录
	deleteById(9)
}
```

```go
affectRows: 1
```

 <!-- 已于 {docsify-updated} 修改 -->

