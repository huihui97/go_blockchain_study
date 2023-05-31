# GO基础知识

## init函数

### 特点

+ **init函数用于在程序执行前对包初始化**，包括包内的变量

+ 一个包内可以拥有多个init函数

+ 一个源文件中同样可以拥有多个init函数

+ **同一个包中的多个init函数执行顺序没有明确定义**

+ init函数不能被其他函数调用，在main函数执行前自动被调用

+ 全局变量先于init函数执行

+ import -> 全局 const -> 全局 var -> init -> main

+ ![image-20230526133515329](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230526133515329.png)

+ ```go
  func init(){
      ....
  }
  ```
  
  ----------------------------
  
  
  
  
  
  

## 辅助知识

### 注释

+ 单行注释 //
+ 多行注释 /* ...*/

### 标识符

​	标识符由字母、数字、下划线组成，首字符不能数字

​	**go语言中还可以使用以下符号：@ $ %**

### 作用域

+ 函数级别:变量声明在函数内部,只有在函数内部才能访问,称变量为局部变量
+ 包级别,在当前包下都可以访问.称变量为全局变量.变量声明在函数外面
+ 应用级别,在整个应用下任何包内都可以访问.通过首字母大小写控制
  + 首字母大写：包外可用可见
  + 首字母小写：包内可用可见

### 进制

+ 二进制 ： 0b
+ 八进制： 0
+ 十六进制：0x/0X



### 自定义类型

+ 类型别名：type 类型别名=go语言数据类型     如：type MyInt=int
  + 类型别名编译后会变成go语言数据类型
+ 自定义类型：type 自定义类型名 go语言数据类型   如 type MyInt = int
  + 自定义类型编译后仍会存在

------------------------





## 数据类型

### 整型

​	uint/int (根据操作系统位数决定) ;       uint/int(8,16,32,64)

### 浮点型

+ 浮点数：float32/float64        在go语言中小数默认采用 float64
+ 复数：complex64/complex128   存在实部和虚部，各占一半

### 布尔类型

+ ture/false

###  byte、rune

+ **byte 相当于 uint8**
+ **rune 相当于 int32**



### 值类型和引用类型

+ 值类型：变量存储的是具体的值 ，==内存通常在栈中分配==
+ **值类型：整型，浮点型，字符串类型，数组类型，结构体类型**
+ 引用类型：变量中存储的是地址，==内存分配在堆上==，具体的值保存在堆上，如果没有变量引用这个地址，该值会被gc回收
+ **引用类型：切片、指针、map、interface、 channel ，==其余的类型均为值类型==**
+ ==注意：值类型数据赋值是拷贝值，引入类型赋值是拷贝地址==





### 常量

+ 常量一般大写表示

+ 常量可以为任意基本数据类型

+ const

+ ```go
  const(
      i=10
      j
      k
  )
  // 注意：i,j,k= 10,10,10  这const的一个特殊用法，在赋值时，如果省略了，则和上一行具有相同的赋值操作。
  ```



----------------





## 运算符

### 算术运算符

![image-20230324212342545](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230324212342545.png)





### 关系运算符

![image-20230324212354145](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230324212354145.png)



### 逻辑运算符

![image-20230324212404803](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230324212404803.png)



### 位运算符

![image-20230324212416142](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230324212416142.png)





### 赋值运算符

![image-20230324212429543](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230324212429543.png)





## 变量声明

### 单变量声明

+ var 变量名 变量类型
  + 变量名=值
+ var 变量名 类型 =  值
+ var 变量名=值       $\rightarrow$   自动识别变量的类型
+ 变量名：=值           $\rightarrow$  函数内SSS可以使用，不需要var



### 多变量声明

+ 自动推断数据类型

  + ```go
    var name1,name2,name3= "zhangsna","lisi","wangwu"
    ```

 + 相同数据类型

   + ```go
     var a,b,c int
     a,b,c=1,2,3
     ```

+ ```GO
  a,b,c:=1,2,3
  ```

### 全局变量声明

```go
var (
	str string
    a float32
    b int
    c uint
)
```



### 局部变量和全局变量定义域

+ ==局部变量：从定义那一行开始到遇到`}`或遇到`return`为止有效，超出该范围则无效==
+ ==全局变量：只要在函数外定义的全局变量，无论在哪里均可以使用==





### 匿名变量

\_   匿名变量用下划线表示，不会分配内存，也不存在重复声



## 字符串

### 字符串常用操作

+ 求长度(内置函数)：len()
+ 拼接字符串：fmt.Sprintf("%s,%s",str1,str2)
+ 可以使用+来拼接字符串，但是不高效。如：fmt.Println("hello"+"_world")  //输出结果为：hello_world
+ 分割字符串：strings.Split()
+ 判断是否包含某个字串：strings.contains()
+ 前后缀判断：strings.HasPrefix()/strings.HasSuffix()
+ 查找字串出现的位置：strings.Index()/strings.LastIndex()
+ 用分隔符连接字符串：strings.Jion(s []string, sep string)

### 字符串特点

+ 作为基本数据类型，值类型，默认采用UTF-8编码字符序列，字符为ASCII码则占用一个字节，其他字符根据需要占2-4个字节，中文通常占用3个字节

+ **字符串元素不可修改，只是一个可读的字节数组**

+ 字符串底层数据结构：

  + ```go
    type StringHeader struct{
        Data uintptr //指向底层字节数组
        Len int //字符串字节的长度
    }
    ```

  + 字符串赋值操作是 reflect.StringHeader结构体的复制过程，并不涉及底层字节数组的复制

+ ==**字符串可以转换成 []byte 或 []rune** ，然后进行修改，会重新分配内存，并复制字节数组，即原始的还是没有发生变化==

+ ==在go语言中，可以通过切片截取一个数组或字符串，但是当截取的字符串是中文时，可能出现问题：由于中文的一个字通常是3个字节组成，直接通过切片获取字符串可能会把一个中文字的编码截成两半，导致最后一个字符是乱码。此时可以通过先将其转换为`[]rune`类型，再进行截取，最后转回字符串。==

### iota用法

#### iota定义

​	iota 作为一种特殊的常量，在每一个const关键字出现后，iota 会重置为0，每出现一次iota，iota会自动加1



# 流程控制

## 条件控制

### if条件语句

+ ```go
  if 布尔表达式{
      
  }
  ```

+ ```go
  if 布尔表达式{
      
  }else if 布尔表达式{
      
  }eles{
      
  }
  ```

+ ```go
  // statement 通常为声明，condition 通常为条件
  if statement;condition{
      
  }
  ```



### switch条件语句

+ ```go
  //对大量值进行判断
  switch 值 {
      case 值1:
      	语句
      case 值2,值3,值4:
      	语句
      ...
      default:
      	表达式
  }
  ```

+ ```go
  //switch语句后不加判断变量
  switch {
      case 布尔表达式1:
      	语句
      ...
      default:
      	语句
  }
  ```

+ 可以在某个case 分支后加上 `fallthrough` 语句，则会继续执行下一条case语句

+ case 后可以放多个不同的值，值不能相同

### goto（跳转指定标签）

==goto语句是通过标签在代码间无条件跳转。 goto 语句可与i快速跳出循环、避免重复退出有一定的帮助。==

==**goto 会直接调至标签对应的语句**==

```go
func gotoDemo2() {
	for i := 0; i < 10; i++ {
		for j := 0; j < 10; j++ {
			if j == 2 {
				// 设置退出标签
				goto breakTag
			}
			fmt.Printf("%v-%v\n", i, j)
		}
	}
	return
	// 标签
breakTag:
	fmt.Println("结束for循环")
}
```



### break（跳出循环）

==`break`语句可以结束`for`、`switch`和`select`的代码块==

==`break`语句还可以在语句后面添加标签，表示退出某个标签对应的代码块，标签要求必须定义在对应的`for`、`switch`和`select`的代码块上。==

```go
func breakDemo1() {
BREAKDEMO1:
	for i := 0; i < 10; i++ {
		for j := 0; j < 10; j++ {
			if j == 2 {
				break BREAKDEMO1  //表示跳出最外层循环，如果没有标签，只能跳出当前循环
			}
			fmt.Printf("%v-%v\n", i, j)
		}
	}
	fmt.Println("...")
}
```



### continue（继续下次循环）

==`continue`语句可以结束当前的循环，开始下一次的循环迭代过程，仅限在`for`循环内使用。==

==在`contine`语句后添加标签时，表示开始标签对应的循环。==

```go
func continueDemo() {
forloop1:
	for i := 0; i < 5; i++ {
		// forloop2:
		for j := 0; j < 5; j++ {
			if i == 2 && j == 2 {
				continue forloop1
			}
			fmt.Printf("%v-%v\n", i, j)
		}
	}
}
```





## 循环控制

### for

+ for循环的基本格式:

  + ```bash
    for 初始语句;条件表达式;结束语句{
        循环体语句
    }
    ```

  + ```go
    func forDemo() {
    	for i := 0; i < 10; i++ {
    		fmt.Println(i)
    	}
    }
    ```

  + ```go
    func forDemo2() {
    	i := 0
    	for ; i < 10; i++ {
    		fmt.Println(i)
    	}
    }
    ```

  + ```go
    func forDemo3() {
    	i := 0
    	for i < 10 {         //相当于while
    		fmt.Println(i)
    		i++
    	}
    }
    ```

  + ```go
    //无限循环
    for {
        循环体语句
    }
    ```

### for range

Go语言中可以**使用`for range`遍历数组、切片、字符串、map 及通道（channel）**。 通过`for range`遍历的返回值有以下规律：

1. 数组、切片、字符串返回索引和值。 索引从0开始。
2. map返回键和值。
3. ==通道（channel）只返回通道内的值。==









# 复合数据类型

## 数组

### 一维数组

#### 定义

数组是同一种数据类型元素的集合，由于数组长度为类型的一部分，所以在数组在声明时就已经确定了

```go
var 变量名 [长度]类型
var array [10]int
```

#### 数组初始化

+ ```go
  //1 初始化列表来设置数组元素的值
  var array = [3]int{1,2,3}
  var array =[3]int{} //此时，数组中的元素为默认值0
  
  //2，编译器自动根据元素个数推断
  var array = [...]int {1,2,3,4}
  
  //3,通过索引来直接赋值
  var array [2]int
  array[0]=1
  array[1]=2
  
  //4,函数内部
  array := [...]int{1,3}
  ```

#### 数组遍历

1，for循环

2，for range



### 二维数组

#### 二维数组定义

```go
var 数组名 [行数][列数]数据类型
var array = [2][3]int{{1,2,3},{4,5,6}}

//自动推断
var array = [...][2]int{{1,2}}
```

==**对于二维数组，行数可以自动推断，但列数必须表示出来**。==



#### 注意

+ ==go 语言中，数组作为值类型，赋值会复制整个数组，因此改变副本的值，不会改变原始数组的值==
+ 数组支持“==”和“！=”操作符，因为内存总是初始化过的
+ ==指针数组== `[n]*T`
+ 数组指针 `*[n]T`



## 切片

### 切片定义

+ ==切片作为一种引用类型，不能直接比较，只能和nil比较，判断是否为空==

+ 切片：是一种拥有相同数据类型元素的可变长度序列，是基于数组类型上的一层封装，灵活，可扩容

+ **切片结构中包含了数组的地址，长度，容量**

### 切片初始化

+ ```go
  //切片格式
  var 变量名 []数据类型
  
  //1，由于是引用类型，单独声明切片，是没有开辟出内存空间的
  	var slice []int          // slice==nil
  //2，声明并初始化才会开辟出内存空间
  	var slice = []int{}
  	slice := []int{1,2}
  ```

+ 可以从数组或切片获取切片

  + ```go
    //数组
    var array = [3]int{1,2,3}
    slice := array[1:]   //左包右不包   0slice 值为 []int{2,3}
    ```

  + ```go
    //切片
    var slice1 = []int{1,2,3,4}
    slice2 := slice1[1:2]
    ```

+ ==注意：在go语言中，可以通过切片截取一个数组或字符串，但是当截取的字符串是中文时，可能出现问题：由于中文的一个字通常是3个字节组成，直接通过切片获取字符串可能会把一个中文字的编码截成两半，导致最后一个字符是乱码。此时可以通过先将其转换为`[]rune`类型，再进行截取，最后转回字符串。==



### 长度和容量

+ 长度： len(slice)
+ 容量：cap(slice)   ==容量和底层数组有关，如果对数组进行切片，则切片的指针会在数组中移动==

### make初始化

+ **如果直接初始化，此时，切片长度和容量是一样的**

+ **采用make函数来初始化可以指定数组长度和容量**

+ ```go
  make(数据类型（切片，map，channel），切片元素数量，容量)
  make(数据类型（切片，map，channel），切片元素数量)  //此时，容量和元素数量一致
  ```

+ ==注意:切片是针对底层数组进行操作的==

+ ==切片只能跟nil比较，一个值为nil 的切片是没有底层数组的，此时切片长度和容量均为0==
+ ==一个长度和容量均为0 的切片不一定是 nil==

+ 判断一个切片是否为空，不能用长度来比较，只能通过切片的值是否为nil

### 切片的赋值拷贝

对切片赋值实际上是对底层数组进行赋值操作，修改切片的值会改变底层数组元素的值

### 切片遍历

+ for 循环遍历
+ for range



### 切片 append 用法

1，**append() 函数是为切片添加元素的。** 每个切片会指向一个底层数组，这个数组能够容纳一定数量的元素，==当底层数组不能容纳新增的元素时，切片就会自动按照一定的策略进行“扩容”，此时该切片指向的数组地址就会更换。==

2，”扩容“往往会发生在append()函数调用，append函数会在切片后面追加元素

3，append用法：s:=append(s,可以为多个元素)

```go
s1 := []string{"上海","北京"}
s1 = append(s1,"杭州")
s2 := []string{"天津","广州"}
s1 = append(s1,s2...)  //...表示将s2拆成多个元素添加到s1切片内
```

==注意：使用append函数，一旦底层数组不能再增加元素后，此时就会发生扩容现象，扩容就伴随着数组的复制，切片会指向新的数组==

4，append 扩容策略

+ 首先判断，如果新申请容量（cap）大于2倍的旧容量（old.cap），最终容量（newcap）就是新申请的容量（cap）
+ 否则判断，如果旧切片的长度小于1024，则最终容量(newcap)就是旧容量(old.cap)的两倍，即（newcap=doublecap）
+ 否则判断，如果旧切片长度大于等于1024，则最终容量（newcap）从旧容量（old.cap）开始循环增加原来的1/4，即（newcap=old.cap,for {newcap += newcap/4}）直到最终容量（newcap）大于等于新申请的容量(cap)，即（newcap >= cap）
+ 如果最终容量（cap）计算值溢出，则最终容量（cap）就是新申请容量（cap）

### 切片 copy函数

由于切片是引用类型，所以当两个切片指向同一块内存地址（同一个数组）修改其中的切片，另一个切片的值也会发生变化

==在go语言中，**使用内建函数copy 可以迅速将一个切片的数据复制到另一个切片内**==

```go
copy(destslice,srcslice[]T) //将源切片srcslice内数据复制到目标切片destslice内
//注意：
//1，目标切片不能为空，且切片长度要大于等于源切片长度，否则无法实现所有元素复制，只会复制对应长度的元素个数
//2，使用copy函数，将一个切片元素拷贝到另一个切片内，即不在共享同一个数组内存。
```

### 切片中删除元素

go语言中，没有删除切片中元素的内建函数或方法，需要使用切片自身的特性来实现元素删除

```go
func main(){
    //从切片中删除元素
    a := []int{1,2,3,4,5,6,7,8}
    //删除索引为2的元素
    a = append(a[:2],a[3:]...)
}
```



## 指针

+ ==go语言中，不存在指针操作，只存在取地址符`&` 和 根据地址取值符号`*`==
+ 开辟内存空间存在两种方式
  + 对于引用类型，使用make函数来开辟内存空间
  + 对于值类型，声明会自动开辟内存空间，或者可以使用new函数来分配空间

+ 指针作为引用类型，需要初始化后才会拥有内存空间

+ new函数开辟内存空间

  + ```go
    func new(Type)*Type
    //new函数只接收一个参数，即需要开辟空间的数据类型，返回一个指针类型
    ```

  + new函数不常用，使用一个new函数得到的是一个类型的指针，并且该指针对应值为该类型的零值

+ new函数用于给基本类型分配内存，对应值为该类型的零值

## map

### map定义

+ map为go语言中提供映射关系的容器，内部是用hash（散列表）实现的，==**map支持自动扩容**==

+ map是一种无序的key-value数据结构

+ map为引用类型，必须经过初始化，开辟内存后才能使用

+ map类型的变量声明时的初始值为nil，需要使用make函数来分配内存

  + ```go
    map[值的数据类型]键的数据类型
    make[map[值类型]键类型,容量]//容量为可选，在初始化是就应该指定一个合适的容量，否则经常出现自动扩容
    ```

### map初始化

==map只能采取以下两种方式进行初始化==

```go
//1,直接初始化
m1 := map[int]string{1:"shanghai",2:"wuhan"}
m2 := map[int]string{} //使用默认值
//2,使用make函数开辟内存
m3 := make(map[int]string,2)
m3[0]="guangzhou"
m3[1]="changsha"
```

### 判断map的某个键是否存在

```go
m := map[string]string{"shanghai":"songjiang","wuhan":"wuchang"}
value,ok := m["guangzhou"] //如果该键不存在，此时value为默认值，ok为false
```

==如果键不存在，编译不会报错，会返回该类型的零值==



### map遍历

==使用for range 遍历map集合，遍历map时，元素顺序与添加键值对的顺序无关==



如何按照指定顺序遍历map集合

```go
//1,先取出map集合的键
//2，对键进行排序
//3，根据键的顺序，输出对应的值


//对切片进行递增排序方法
//sort包提供了排序切片和用户自定义数据集函数
sort.Strings(keys) 
```





### delete函数删除键值对

==使用delete函数可以从map集合中删除一组键值对==

```go
delete(要删除键值对的map集合,要删除的键 key)
delete(m,"wuhan")
```



### 元素为map类型的切片

```go
var slice = make([]map[string]string,3,4)
//赋值有两种方式，注意需要先开辟map内存空间，赋值是将map内存地址赋值给切片了
//第一种，
slice[0] = map[string]string{"wuhan":"dongxihu"}
//第二种
slice[1] = make(map[string]string,2)
slice[1]["shanghai"]="songjiang"
```



### 值为切片类型的map

```go
var sliceMap = make(map[string][]int,2)
sliceMap["shanghai"]=[]int{1,2,3}
sliceMap["wuhan"]=[]int{11,22,33}
```



==切片类型和map类型均为引用类型，一定要进行初始化开辟出内存空间==



# 函数

## 函数

### 定义

```go
func 函数名(参数)(返回值参数){
    函数体
    return
}
//有返回值要写return，没有返回值可以不写
```

+ 在同一个包内，函数名不能重复
+ 返回值由返回值变量和类型组成，也可以只写类型
+ 当连续两个及以上参数类型一致时，可以将前面的变量的类型省略
+ 可变参数 `func f(str string, y... int)`,其中y为可变参数，y可以为多个也可以为0个，==实际y的类型为切片类型==
+ 函数传参，值类型的参数通过值传递，引用类型参数则是地址传递



### defer语句

go语言中的defer语句会将后面的语句延迟处理，==先defer语句最后被执行，后defer语句先执行，类似栈==

defer一般用在：资源清理，文件关闭，解锁等

defer执行时机：==**返回值赋值操作之后，RET指令执行前，defer会将变量的状态都代进去，只是结果最后执行**==

```go
a,b = 1,2
defer calc("1",a,calc("2",a,b)) 
//相当于 defer calc("1",1,3)
```

defer 三个注意点：

+ 先defer后调用，后defer先调用
+ defer将语句放入栈中时，也会将相关值拷贝同时入栈
+ defer跟return的返回时机：先赋值，再defer，最后RET

### 函数作用域

+ 全局变量：在函数外部的变量，程序整个运行期间均有效
+ 局部变量：函数内部的变量，==**如果局部变量和全局变量重名，优先使用局部变量，采用“就近原则”**==
+ 变量名首字母大写，在包外可见
+ 变量名首字母小写，包内可见

### 函数类型和变量

#### 函数类型

```go
//定义了一个函数类型
type caculation func(int ,int)int
//func (int,int)int 为函数类型
```

#### 函数类型变量

```go
func add(a int,b int)int{
    return a+b
}

var c caculation
c = add //其实是函数入口，地址
c(1,2)
```

### 函数作为函数参数

==函数本身存在类型，可以用该类型定义参数，然后将函数名作为参数传递过来==

```go
func add(a int,b int)int{
    return a+b 
}

func f(f1 func(int,int)int){
    
}


//在调用时就可以将add 作为f函数的参数
f(a)
```

```go
func add(x, y int) int {
	return x + y
}
func calc(x, y int, op func(int, int) int) int {
	return op(x, y)
}
func main() {
	ret2 := calc(10, 20, add)
	fmt.Println(ret2) //30
}
```



### 函数作为返回值

```go
func f(int,int)func(int,int)int	
```

```go
func do(s string) (func(int, int) int, error) {
	switch s {
	case "+":
		return add, nil
	case "-":
		return sub, nil
	default:
		err := errors.New("无法识别的操作符")
		return nil, err
	}
}
```



### 匿名函数

在go语言中，**匿名函数用于函数内部**，因为函数内部不能声明有名字的函数

```go
//匿名函数格式：
func (参数)(返回值){
    函数体
}
//匿名函数没有函数名，无法像普通函数那样调用，匿名函数需要保存到某个变量或作为立即执行函数执行
//匿名函数多用于实现回调函数和闭包
```

+ 多次调用匿名函数，可以将匿名函数保存在变量中，实现重复调用

+ 如果为单次调用，可以将其作为立即执行函数

  + ```go
    func (参数)(返回值参数){
        函数体
    }(传入实参)
    ```



### 闭包

闭包：指一个函数与其相关的引用环境组合而成的实体

​			**闭包= 匿名函数+匿名函数外部环境变量**

闭包是一个函数，该函数能够包含外部作用域中的变量，变量查找的顺序为：先自己内部，找不到就去外部找

闭包底层原理：

+ 函数作为返回值
+ 变量查找的顺序为：先自己内部，找不到就去外部找

```go
//闭包示例：
func adder()func(int)int{
    var x int
    return func(y int)int{  //返回一个匿名函数，该匿名函数与变量x有关
        x+=y
        return x
    }
}

func main(){
    var f = adder()  //可以看到f是由这个匿名函数和变量x组成的一个整体，x是初始化一次的
    fmt.Println(f(10))  //10
    fmt.Println(f(20))  //30
    fmt.Println(f(30))  //60
}
```

闭包特点：

+ **可以让变量常驻内存**
+ 可以让变量不全局有效
+ 闭包是指有权访问另一个函数作用域中的变量函数
+ 创建闭包的常见方式，就是在一个函数内部创建另一个函数，内函数可以访问外函数的变量
+ **闭包里作用域返回的局部变量不会立即销毁**

### go语言内置函数

| 内置函数       | 介绍                                                         |
| -------------- | ------------------------------------------------------------ |
| close          | 主要用来关闭**channel**                                      |
| len            | 用来求长度，比如**string、array、slice、map、channel**       |
| new            | 用来分配内存，主要用来分配值类型，比如**int、struct**。返回的是指针 |
| make           | 用来分配内存，主要用来分配引用类型，比如**chan、map、slice** |
| append         | 用来追加元素到**slice对应的底层数组**中                      |
| panic和recover | 用来做错误处理                                               |



### panic和recover

+ go语言中目前没有异常处理机制，而是用panic和recover模式来处理错误
+ ==panic可以在任何地方引用，**但recover只有在的defer调用函数中有效**==
+ 在go语言中，为了防止异常滥用，没有try catch，==常常使用函数的返回值来返回异常，而不是用异常来代替错误==
+ panic是出现重大错误时使用，相当于throw exception，抛出异常
+ recover相当于try catch ，恢复异常





+ recover用法：
  + recover的作用是捕获panic，从而代码恢复，正常执行
  + **recover只有在defer调用函数中使用有效**
  + recover没有传入参数，但是有返回值，返回值就是panic传递的值，即panic报的错误

```go
func funcA() {
	fmt.Println("func A")
}

func funcB() {
	defer func() {       //必须使用这种写法，才能保证recover有效执行，可以用判断来打印出捕获的错误
		err := recover() //该错误就是panic传过来的错误
		//如果程序出出现了panic错误,可以通过recover恢复过来
		if err != nil {
			fmt.Println("recover in B")
		}
	}()
	panic("panic in B")
}

func funcC() {
	fmt.Println("func C")
}
func main() {
	funcA()
	funcB()
	funcC()
}

//这样recover才能恢复错误，而如果直接用defer recover() 无法恢复过来，这是因为defer会将相关值也拷贝入栈
```



## fmt函数

fmt包实现了类似C语言的格式化输入与输出

### 向外输出

+ fmt.print/printf/println

+ fmt.Fprint/Fprintln/Fprintf    ==写入文件==

  + ```go
    func Fprint(w io.Writer, a ...interface{})(n int,err error)
    func Fprintf(w io.Writer,format string,a ...interface{})(n int,err error)
    ```

+ fmt.Sprint/Sprintln/Sprintf  ==传入数据返回一个字符串==

  + ```go
    func Sprint(a ...interface{})
    func Sprintf(format string,a ...interface{})
    ```

+ fmt.Errorf

  + ```go
    func Errorf(format string,a ...interface{})error
    
    //可以用上面函数生成一个错误
    
    //同时可以使用errors包
    errors.New(s string)error
    ```

###  获取输入

+ fmt.Scan/Scanln/Scanf   ==从标准输入中获取用户输入的数据==

  ```go
  func Scan(a ...interface{})(n int,err error) //以空白符作为分隔符，换行符视为空白符
  
  //示例
  var(
  	a int
  	b int
  )
  fmt.Scan(&a,&b)
  
  func Scanf(format string, a ...interface{})(n int,err error) //需要按照指定的输入格式
  
  
  fmt.Scanf("%d,%d",&a,&b)  //严格按照格式在终端上输入
  
  fmt.Scanln(&a,&b)   //在终端上输入，多个数之间用空格隔开，遇到换行结束
  
  ```

+ bufio.NewReader  ==bufio包实现了有缓冲的I/O==

  + ```go
    func bufioDemo(){
        reader := bufio.NewReader(os.stdin) //从标准输入生成读对象
        text,_:=reader.ReadString('\n')
    }
    ```

+ ioutil.ReadFile  ==ioutil包提供了对文件读写的工具函数，通过这些函数可以快速实现文件读写操作==

  + ```GO
    func NopCloser(r io.Reader) io.ReadCloser
    func ReadAll(r io.Reader) ([]byte, error)
    func ReadFile(filename string) ([]byte, error)
    func WriteFile(filename string, data []byte, perm os.FileMode) error
    func ReadDir(dirname string) ([]os.FileInfo, error)  //获取某个文件夹中所有信息的函数
    func TempDir(dir, prefix string) (name string, err error)
    func TempFile(dir, prefix string) (f *os.File, err error)
    ```

  + 注意：在go 1.16开始，ioutil包被弃用，相关功能被移到io和os包中

  + ```go
    ioutil.ReadAll  --> io.ReadAll
    ioutil.ReadFile  -->  os.ReadFile
    ioutil.ReadDir  -->  os.ReadDir
    
    ioutil.NopCloser  --> io.NopCloser
    ioutil.TempDir  -->  os.MkdirTemp
    ioutil.TempFile  -->  os.CreateTemp
    ioutil.WriteFile  --> os.WriteFile
    ```

  + 

+ Fscan系列函数

  + Fscan/Fscanf/Fscanln 函数功能类似于fmt.Scan/fmt.Scanf/fmt.Scanln这三个函数，==不是从标准输入中读取数据，而是从io.Reader中读取数据。==

    ```go
    func Fscan(r io.Reader, a ...interface{})(n int,err error)
    func Fscanln(r io.Reader,a ...interface{})(n int,err error)
    //Fscanf 扫描从 r 读取的文本，将连续的空格分隔值存储到由格式确定的连续参数中
    func Fscanf(r io.reader, format string, a ...interface{})(n int,err error)
    ```

    

+ Sscan系列函数

  + Scanf/Scan/Scanln函数功能类似于Scan/Scanf/Scanln三个函数，==不是从标准输入中读取数据，而是从指定字符串中读取数据。==

    ```go
    func Sscan(str string,a ...interface{})(n int,err error)
    //Sscanf 扫描参数字符串，将连续的空格分隔值存储到由格式确定的连续参数中
    func Sscanf(str string, format string,a ...interface{})(n int,err error)
    func Sscanln(str string,a... interface{})(n int,err error)
    ```

    



# 结构体

结构体作为值类型，而不是引用类型

## 结构体定义

```go
type 自定义类型名 struct{
    字段名 字段类型
    ...
}
```

## 结构体实例化

```go
var 结构体实例 结构体类型
//实例化后，通过.来访问结构体的字段
//只有实例之后才会分配内存
```



## 匿名结构体

匿名结构体即为没有名字的结构体，类似于匿名函数，一般在函数体内临时使用时定义

```go
var person = struct{
    name string
    num int
}{"zhangsan",23}
```



## 指针类型的结构体

可以先定义结构体，然后通过new关键字来生成一个结构体指针

**==go语言中存在一个语法糖：对于指针类型的变量可以直接用`.`来访问结构体中的字段==**

```go
type student struct{
	num int
	name string
}


func main(){
	var p = new(student)
	fmt.Println((*p).num)
	fmt.Println(p)
}

//0
//&{0 }
```



## 对结构体取地址操作

```go
type person struct{
    name string
    num int
}
p := &person{"zhangsan",23} //此时p为结构体类型的指针
```





## 结构体初始化

+ 结构体为零值，只要声明后，就初始化完成了，只是对应字段为对应类型的零值

+ 可以采取**键值对** 初始化

  + ```go
    var p = person{name:"zhangsan",num:23}
    ```



+ 可以采用**值列表** 形式初始化

  + ```go
    var p = person{"zhangsan",23}
    ```



+ 注意：
  + 初始值的填充顺序必须和字段顺序一致
  + 值列表和键值对两种形式不能混用

+ ==结构体布局==
  + 结构体是占用一块连续的内存空间
  + ==空结构体也是占用空间的==



## 构造函数

在go语言中没有面向对象的概念，也没有java形式的构造函数，但我们可以利用结构体来自己实现一个构造函数。

==因为结构体为值类型，如果结构体比较复杂，值拷贝性能开销较大，所以构造函数返回的是结构体指针类型==

==go语言中自己构造的构造函数，**通常使用new开头**==

```go
func newPerson(name string,num int)*person{
    return &person{
        name:name,
        num:num,
    }
}


//此时，可以利用自己构造的构造函数来实例化一个结构体

```



## 方法和接收者

go语言中的方法是一种作用于特定类型变量的函数，特定类型即为接收者，接收者类似其他语言中this和self

格式：

```go
func (接收者 接收者类型)方法名(参数列表)(返回值参数列表){    //接收者名通常用接收者类型的首字母小写表示
    函数体
}
```

方法只有特定类型的变量才能调用，==**只有在自定义类型才能添加方法**==



***接收者***

+ 指针类型的接收者由一个结构体指针组成，由于指针特性，调用方法时修改接收者指针的任意成员变量，在方法结束，修改都是有效的
+ 值类型的接收者，方法作用于值类型接收者时，go语言在代码运行时将接收者值复制一份，在值类型接收者方法中，可以获取接收者成员值，但是修改操作只是针对副本，无法修改接收者变量本身。
+ ==什么时候使用指针类型的接收者==
  + 需要修改接收者中的值
  + 接收者    复杂占用较大内存，此时应该用指针类型的接收者
  + 如果某个方法使用指针类型的接收者，那么其他方法也应该采用指针类型的接收者
+ ==注意==
  + 只有用type自定的类型才能作为接收者
  + 其他包中自定类型不能再本包中定义方法和接收者



==同种类型，值类型和指针类型接收者的方法，值类型和指针类型变量均可以调用该方法==





## 结构体匿名字段



结构体允许其成员字段在声明时没有字段名而只有类型，这种没有名字的字段就称为匿名字段。

==匿名字段并不是代表没有字段名，而是将数据类型作为字段名，结构体要求字段名必须一致==

==**注意：相同数据类型的匿名字段只有一个**==

```go
type person struct{
     string
     int
    num int 
}

func main(){
    var p = person{
        "zhangsan",
        23,
        12,
    }
    fmt.Println(p.string,p.int,p.num)
}
```

+ 同种类型的字段名只有一个
+ 默认采用类型作为字段名



## 嵌套结构体

一个结构体中可以嵌套另一个结构体，将另一个结构体作为一种数据类型

```go
type student struct{
    name string
    age int
    address
}
type address struct{
    city string
    postalCode int 
}
func main(){
	var stu=student{
		"zhangsan",
		23,
		address{"wuhan",55555},
	}
	fmt.Println(stu)
}
```

对于嵌套结构体，如果想访问嵌套结构体中的字段名，需要一层一层查找 。

+ 如果匿名结构体，想要访问最里层，当字段名没有冲突时，可以`stu.city`,这是一种语法糖，如果有冲突，则需要一层一层调用 `stu.addr.city`



## 结构体继承

在go语言中，可以用结构体模拟继承，实现其他编程语言中的面向对象的继承

```go
type animal struct{
    name string
}
func (a animal) move(){
    fmt.Println(a.name,"在移动")
}

type dog struct{
	feet int
    animal    
}
func (d dog)wang(){
    fmt.Println(d.name,"汪汪汪")
}
func main(){
	d1 := dog{
		4,
		animal{"xiaoming"},
	}
    d1.eat()   //这里为什么可以直接调用eat方法，还是因为语法糖缘故，因为采用匿名字段，原本应该是 d1.animal.eat()
	fmt.Println(d1.feet)
}

```

==注意：用结构体嵌套来模拟继承，外层结构体类型 拥有 里层结构体类型的方法==



## 结构体方法重写

方法的重写，**==不能发生在同类中，只能发生在子类中==**。**若子类中的方法与父类中的某一方法具有相同的方法名、返回类型和参数表，则新方法将覆盖原有的方法。**

```go
package main

import "fmt"

type Person struct {
	name string
	age  int
}

func (p *Person) PrintInfo() {   //由于结构体在该包中，所以定义方法也只能在该包，在其他包定义方法会报错
	fmt.Println("这是父类中的方法")
}

type Student struct {
	Person
	score float64
}

func (p *Student) PrintInfo() {
	fmt.Println("这是子类中的方法")
}

func main() {
	var stu Student
    // 如果父类中的方法名称与子类中的方法名称一致，那么通过子类的对象调用的是子类中的方法。方法重写
	stu.PrintInfo()   //输出：这是子类中的方法
	stu.Person.PrintInfo()
}

```

==注意：定义方法，只能在本包中进行，即方法所用到的结构体在哪个包中，则定义方法也只能在那个包中，所以通常我们定义好结构体后，就接着定义方法==



```go
package main

import "fmt"

func main() {
	p := person{"zhangsan", teacher{
		"lisi",
	}}
	fmt.Println(p.name)
	p.eat()
}

/*
	在go语言中，对于嵌套匿名结构体来说：如果当前的结构体字段和嵌套匿名结构体的字段一样，此时语法糖失效，优先显示当前结构		体的字段。
	
	为什么可以实现方法的继承？
	因为匿名字段的语法糖原因，语法糖可以省略了中间匿名字段，导致看起来继承嵌套结构体的方法
	
	为什么实现方法重写？
	还是因为语法糖的原因，因为当前结构体的方法和嵌套结构体的方法一样时，直接使用会优先使用当前结构体的方法，语法糖失效

*/



type student struct {
	name string
}

func (s student) eat() {
	fmt.Println("student eat()")
}

type teacher struct {
	name string
}

func (s teacher) eat() {
	fmt.Println("teacher eat()")
}

type person struct {
	name string
	teacher
}

func (p person) eat() {
	fmt.Println("person eat()")
}
```







## 结构体与JSON序列化

json（javascript object notation）是一种轻量级的数据交换格式，JSON键值对时用来保存JS对象的一种方式，键值对组合中的键名在前，且用双引号包裹，使用冒号分开，接着写值，多个键值对之间用逗号隔开。

```go
type Student struct{
    ID int
    Gender string
    Name string
}
type Class struct{
    Title string
    Students []*Student
}

func main(){
    c :=&Class{
        "101",
        make([]*Student,0,10),
    }
    for i:=0;i<10;i++{
        stu := &Student{
            ID:i,
            Gender:"男",
            Name:fmt.Sprintf("stu%02d",i),
        }
        c.Students = append(c.Students,stu)
    }
    
    //Json序列化：结构体-->Json格式化字符串
    data,_:=json.Marshal(c)
    fmt.Println(data)
    
    //Json反序列化，json格式字符串-->结构体
    str := `{"Title":"101","Students":[{"ID":0,"Gender":"男","Name":"stu00"},{"ID":1,"Gender":"男","Name":"stu01"},{"ID":2,"Gender":"男","Name":"stu02"},{"ID":3,"Gender":"男","Name":"stu03"},{"ID":4,"Gender":"男","Name":"stu04"},{"ID":5,"Gender":"男","Name":"stu05"},{"ID":6,"Gender":"男","Name":"stu06"},{"ID":7,"Gender":"男","Name":"stu07"},{"ID":8,"Gender":"男","Name":"stu08"},{"ID":9,"Gender":"男","Name":"stu09"}]}`
	c1 := &Class{}
	json.Unmarshal([]byte(str),c1)
	fmt.Printf("%d,%s,%s\n",c1.Students[0].ID,c1.Students[0].Gender,c1.Students[0].Name)
    
}

```

![image-20230212200506194](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230212200506194.png)





## 结构体标签

Tag 是结构体元信息，可以在运行式通过反射机制读取出来。Tag在结构体的后方定义，由一对反引号包裹起来

```go
`key1:"value1" key2:"value2"`
```

结构体tag由一个或多个键值对组成，键与值使用冒号分隔，值使用双引号括起来，同一个结构体字段可以设置多个键值对tag，不同键值对之间用空格分隔。

==注意：为结构体编写tag时，需要严格遵守键值对规则。结构体标签容错能力很差，一旦格式写错，编译和运行都不会提示任何错误，通过反射也无法正确取值==

```go
//Student 学生
type Student struct {
	ID     int    `json:"id"` //通过指定tag实现json序列化该字段时的key
	Gender string //json序列化是默认使用字段名作为key
	name   string //私有不能被json包访问
}

func main() {
	s1 := Student{
		ID:     1,
		Gender: "男",
		name:   "沙河娜扎",
	}
    //******注意；结构体字段必须大写，否则无法序列化******
	data, err := json.Marshal(s1)
	if err != nil {
		fmt.Println("json marshal failed!")
		return
	}
	fmt.Printf("json str:%s\n", data) //json str:{"id":1,"Gender":"男"}
}
```





## 结构体和方法补充知识点

因为slice和map这两种数据类型都包含了指向底层数据的指针，因此我们在需要复制它们时要特别注意。我们来看下面的例子：

```go
type Person struct {
	name   string
	age    int8
	dreams []string
}

func (p *Person) SetDreams(dreams []string) {
	p.dreams = dreams
}

func main() {
	p1 := Person{name: "小王子", age: 18}
	data := []string{"吃饭", "睡觉", "打豆豆"}
	p1.SetDreams(data)

	// 你真的想要修改 p1.dreams 吗？
	data[1] = "不睡觉"
	fmt.Println(p1.dreams)  // ?
}
```

正确的做法是在方法中使用传入的slice的拷贝进行结构体赋值。

```go
func (p *Person) SetDreams(dreams []string) {
	p.dreams = make([]string, len(dreams))
	copy(p.dreams, dreams)
}
```

同样的问题也存在于返回值slice和map的情况，在实际编码过程中一定要注意这个问题。







## 结构体和结构体成员字段的跨包导出
+ 如果结构体名称首字母小写，则结构体不会被导出。这时，即使结构体成员字段名首字母大写，也不会被导出。如果结构体名称首字母大写，则结构体可被导出，但只会导出大写首字母的成员字段，那些小写首字母的成员字段不会被导出。
+ 结构体的成员字段是否可被导出，也遵循大小写原则：首字母大写的成员字段可被导出，首字母小写的成员字段不可被导出。
  









# 接口

## 接口定义

在go语言中，接口是一种抽象类型，接口是一种由程序员来定义的类型，一个接口类型就是一组方法的集合，只有实现接口内的所有方法，才能说是接口类型。

+ 接口可以接收值类型和引用类型

+ 每个接口类型都是由一组方法组成

+ 接口格式：

  ```go
  type 接口类型 interface{
      方法名1 (参数列表)(返回值列表)
      方法名2 (参数列表)(返回值列表)
      ...
  }
  ```

  + **在go语言中，接口命名通常以er为结尾**
  + 方法名首字母大写且这个接口类型名也大写时，这个方法就可以被接口所在包之外的代码访问
  + 参数列表、返回值列表中的参数变量名可以省略

+ ==实现接口的条件：一个变量实现了接口内所有方法，则实现了该接口==
+ ==接口类型：引用类型==
+ ==在go语言中，**引用类型：切片、字典、通道、函数、指针、接口**==
+ 任何类型都可以实现接口，对于基本数据类型来说必须要用type 自定义一个类型，而不能直接用基本数据类型



## 值接收者和指针接收者

+ ==值接收者T类型实现接口：则T类型和*T类型均实现了该接口==
+ ==指针接收者*T类型实现了接口：则\*T类型实现了该接口，而T类型没有实现该接口==



## 类型和接口关系

+ 一个类型可以是实现多个接口
+ 多种类型可以实现同一个接口
+ 对于接口中的方法，不一定由一个类型来实现，可以通过在类型中嵌入其他类型，从而实现接口
+ 任何类型都可以实现接口，对于基本数据类型来说必须要用type 自定义一个类型，而不能直接用基本数据类型





## 接口组合



### 接口嵌套接口

接口与接口之间可以通过相互嵌套形成新的接口。

==对于多个接口类型嵌套形成新的接口类型，只要实现该接口中所有的方法就算实现了该接口。==



```go
type Reader interface{
    Read(p []byte)(n int,err error)
}
type Writer interface{
    Write(p []byte)(n int,err error)
}
type Closer interface{
    Close() error
}

type ReadWriter interface{
    Reader   //匿名结构字段
    Writer
}

type WriteClose interface{
    Writer
    Closer
}

```



### 结构体嵌套接口

在结构体中嵌套接口，可以让该结构体 实现该接口类型，并且还可以改写接口的方法

 ```go
 package main
 
 import (
 	"fmt"
 )
 
 //结构体中嵌套接口
 
 
 type student struct{
 	Name string
     Mover    //这里采用匿名字段，那么后面就可以直接stu.f1(),stu.f2()
     //Move Mover //如果这里不采用匿名字段，那么后面只能stu.Move.f1(),stu.Move.f2()
 }
 
 func (s student)f(){
 	fmt.Println("student f")
 }
 
 func (s student)f1(){
     fmt.Println("student f1")
 }
 
 type Mover interface{
 	f1()
 	f2()
 }
 type person struct{
 	name string
 	age int
 }
 func (p person)f1(){
 	fmt.Println("peroson f1")
 }
 func (p person)f2(){
 	fmt.Println("perosn f2")
 }
 
 func main(){
 	var p=person{
 		"zhangsan",
 		23,
 	}
 
 	var stu = student{
 		"lisi",
 		p,
 	}
 
 	
 	stu.f()
 	stu.f1()  //显示 student f1 而不是 person f1  由于外层也实现了该方法，调用时会直接显示外层方法，相当对里层方法重写了
 
 	fmt.Println(stu.Name,stu.Mover)
 	//无法获取Mover里面的字段，因为这是一个接口类型的变量，接口只有方法，没有属性
 }
 ```

+ 结构体嵌套结构体：外层结构体变量可以访问里层结构体的属性和方法，也可以访问自己的方法
+ 结构体嵌套接口；外层结构体变量可以访问本层结构体的属性，不能访问接口的属性（因为接口没有属性），可以访问自己的方法和接口的方法
+ 嵌套结构体中，如果外层结构体和里层结构体具有相同的方法，那么在调用时，优先调用外层方法，可以通过层层调用，调用里层结构体/接口的方法





## 空接口

空接口是指没有定义任何方法的接口类型，因此任何类型均可以视为实现了空接口，因此空接口类型的变量可以存储任意类型的值

通常在使用空接口类型时，不必使用type关键字声明，可以直接用 `interface{}`

+ 空接口作为函数的参数：
  + 使用空接口可以接收任意类型的函数参数

+ 空接口作为map的值
  + 使用空接口可以在map中保存任意值





==**注意：两个空接口类型的数据是不能直接进行比较或算术运算的**==



## 接口值

接口类型的值可以是任意一个实现了该接口的类型值，所以接口除了需要记录具体值外，还需要记录这个值属于类型。接口值是由类型和值组成。

**接口包含两个部分：动态类型和动态值**

```go
var m Mover
//此时 m的type和value均为nil

//可以通过m==nil来判断是否为空接口
fmt.Println(m==nil)

//接口值是可以相互比较的（针对值类型的变量），只有当值类型和值均相等才相等

//对于引用类型的变量，是不支持比较的，否则会产生panic

```

==注意：如果为值类型，则接口值是拷贝过来的，如果为引用类型，那么接口值为地址（改变二者均会发生变化）==



## 类型断言

对于空接口来说，接口值可以为任意类型的值，可以通过类型断言来获取其存储的具体数据

+ 第一种方法：

  ```go
  var m Mover
  
  m = &dog{Name:"旺财"}
  fmt.Printf("%T\n",m)  //*main.dog
  
  m = new(car)
  fmt.Printf("%T\n",m) //*main.car
  ```

+ 使用类型断言

  + ==value,ok := x.(T)  //这是可以在switch语句外使用的==

  ```go
  //类型断言语法格式：
  value,ok := x.(T) //类型断言，如果断言成功，则会将x转换成对应类型的值赋给value，断言失败value则为对应断言类型的默认值
  //x表示接口类型的变量，T表示断言x可能是的类型
  //返回两个参数，第一个参数x转换为T类型后的变量，第二个值是bool值，若是true则表示断言成功，false表示断言失败
  ```

  ```go
  var n Mover = &dog{Name:"旺财"}
  v , ok := n.(*dog)
  if ok {
      fmt.Println("类型断言成功")
  }else {
      fmt.Println("类型断言失败")
  }
  
  
  
  func main() {
  	var i interface{} = int32(32)
  	value, ok := i.(bool)
  	fmt.Println(x, ok)  //断言失败，value为false，ok为false
  }
  ```

+ 需要进行多个类型判断时，可以使用switch

  + ==**v:=x.(type)    获取x可能的类型，这种类型只能在switch语句中可以使用 **==

  ```go
  func justifyType(x interface{}){
      switch v:=x.(type) {     //****获取x可能的类型，这种类型只能在switch语句中可以使用 *****
      case string :
          fmt.Printf("x is a string: value is %v\n",v)
      case int:
          fmt.Printf("x is a int is %v\n",v)
      }
  }
  ```

  

==不能滥用interface{}，只有当有两个或两个以上的具体类型必须以相同的方式进行处理时才需要定义接口，切记不要为了使用接口类型而增加不必要的抽象，导致不必要的运行损耗==







# 泛型



## 泛型定义

泛型是一种允许程序员在强类型程序设计语言中编写代码时使用一些以后才指定的类型，在实例化时作为参数指明这些类型。--即在编写代码或数据结构是先不提供值的类型，而是之后才提供。



**泛型作用：**

+ 泛型可以减少重复代码并提高类型的安全性
+ 当你需要针对不同类型书写同样的逻辑，使用泛型来简化代码是最好的。





## 为什么要引入泛型

如反转切片的函数示例：

```go
func reverse(s []int) []int {
	l := len(s)
	r := make([]int, l)

	for i, e := range s {
		r[l-i-1] = e
	}
	return r
}

fmt.Println(reverse([]int{1, 2, 3, 4}))  // [4 3 2 1]
```

可是这个函数只能接收`[]int`类型的参数，如果我们想支持`[]float64`类型的参数，我们就需要再定义一个`reverseFloat64Slice`函数。

```go
func reverseFloat64Slice(s []float64) []float64 {
	l := len(s)
	r := make([]float64, l)

	for i, e := range s {
		r[l-i-1] = e
	}
	return r
}
```

如果要想支持`[]string`类型切片就要定义`reverseStringSlice`函数，如果想支持`[]xxx`就需要定义一个`reverseXxxSlice`…

一遍一遍地编写相同的功能是低效的，实际上这个反转切片的函数并不需要知道切片中元素的类型，但为了适用不同的类型我们把一段代码重复了很多遍。

Go1.18之前我们可以尝试使用反射去解决上述问题，但是使用反射在运行期间获取变量类型会降低代码的执行效率并且失去编译期的类型检查，同时大量的反射代码也会让程序变得晦涩难懂。

类似这样的场景就非常适合使用泛型。从Go1.18开始，使用泛型就能够编写出适用所有元素类型的“普适版”`reverse`函数。

```go
func reverseWithGenerics[T any](s []T) []T {
	l := len(s)
	r := make([]T, l)

	for i, e := range s {
		r[l-i-1] = e
	}
	return r
}
```



## 泛型语法

**泛型为Go语言添加了三个新的重要特性:**

1. 函数和类型的类型参数。
2. 将接口类型定义为类型集，包括没有方法的类型。
3. 类型推断，它允许在调用函数时在许多情况下省略类型参数



### 类型参数

#### 类型形参和类型实参

我们之前已经知道函数定义时可以指定形参，函数调用时需要传入实参。

![image-20230216164504311](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230216164504311.png)



现在，Go语言中的函数和类型支持添加类型参数。==类型参数列表看起来像普通的参数列表，只不过它使用方括号（`[]`）而不是圆括号（`()`）。==

![image-20230216164538997](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230216164538997.png)

借助泛型，我们可以声明一个适用于**一组类型**的`min`函数。

```go
func min[T int | float64](a, b T) T {
	if a <= b {
		return a
	}
	return b
}
```





#### 类型实例化

这次定义的`min`函数就同时支持`int`和`float64`两种类型，也就是说当调用`min`函数时，我们既可以传入`int`类型的参数。

```go
m1 := min[int](1,2)

m1 := min[float64](-0.1,-0.2)
```

向`min`函数提供类型参数称为实例化，即调用函数时指定类型参数，如上 指定 int, float64

类型实例化分为两步：

+ 首先，编译器在整个泛型函数或类型中将所有类型形参替换为它们各自的类型实参
+ 其次，编译器验证每个类型参数是否满足相应的约束。

在成功实例化后，我们将得到一个非泛型函数，它可以像任何其他函数一样被调用。例如：

```go
fmin := min[float64] //类型实例化，编译器生成 T=float64的min函数
m2 = fmin(1.2,3.2)
```





#### 类型参数的使用

==**除了函数中支持使用类型参数列表外，类型也可以使用类型参数列表**==

```go
type Slice [T int|string] []T

type Map [K int|string,V float32|float64]  map[K] V

type Tree [T interface{}] struct{
    left,right *Tree[T]
    value T
}
```

在上述泛型类型中，T、K、V  都属于类型形参，类型形参后面是类型约束，类型实参需要满足对应的类型约束。



==泛型类型可以有方法，例如为上面的`Tree`实现一个查找元素的`Lookup`方法。==

```go
func (t *Tree[]) Lookup(x T) *Tree[T] {...}
```

要使用泛型类型，必须进行实例化。`Tree[string]`是使用类型实参`string`实例化`Tree`的示例。

```go
var stringTree Tree[string]
```





#### 类型约束

普通函数中的每个参数都有一个类型; 该类型定义一系列值的集合。例如，我们上面定义的非泛型函数`minFloat64`那样，声明了参数的类型为`float64`，那么在函数调用时允许传入的实际参数就必须是可以用`float64`类型表示的浮点数值。

类似于参数列表中每个参数都有对应的参数类型，==类型参数列表中每个类型参数都有一个**类型约束**。类型约束定义了一个类型集——只有在这个类型集中的类型才能用作类型实参。==

==**Go语言中的类型约束是接口类型。**==

就以上面提到的`min`函数为例，我们来看一下类型约束常见的两种方式。

==类型约束接口可以直接在类型参数列表中使用。==

```go
//********** 类型约束字面量，通常外层interface{}可省略  ***********
func min[T interface{ int | float64 }](a, b T) T {
	if a <= b {
		return a
	}
	return b
}
```

==作为类型约束使用的接口类型可以事先定义并支持复用。==

```go
// 事先定义好的类型约束类型
type Value interface {
    int | float64         //就是不省略接口interface{}，可以看到在定义类型约束时，里面还是使用 | 符号
}
func min[T Value](a, b T) T {
	if a <= b {
		return a
	}
	return b
}
```

在使用类型约束时，如果省略了外层的`interface{}`会引起歧义，那么就不能省略。例如：

```go
type IntPtrSlice [T *int] []T  // T*int ?

type IntPtrSlice[T *int,] []T  // 只有一个类型约束时可以添加`,`
type IntPtrSlice[T interface{ *int }] []T // 使用interface{}包裹
```









### 类型集

Go语言扩展了接口类型的语法，让我们能够向接口中添加类型。例如

```go
type V interface {
	int | string | bool
}
```

上面的代码就定义了一个包含 `int`、 `string` 和 `bool` 类型的类型集。



==在 Go 1.18中，接口既可以像以前一样包含方法和嵌入式接口，但也可以嵌入非接口类型、类型并集和基础类型的集合。==

**当用作类型约束时**，由接口定义的类型集精确地指定允许作为相应类型参数的类型。

- `|`符号

  `T1 | T2`表示类型约束为T1和T2这两个类型的并集，例如下面的`Integer`类型表示由`Signed`和`Unsigned`组成。

  ```go
  type Integer interface {
      Signed | Unsigned
  }
  ```

- `~`符号

  `~T`表示所以底层类型是T的类型，例如`~string`表示所有底层类型是`string`的类型集合。

  ```go
  type MyString ~string  // MyString的底层类型是string
  ```

  **注意：==`~`符号后面只能是基本类型。==

接口作为类型集是一种强大的新机制，是使类型约束能够生效的关键。目前，使用新语法表的接口只能用作类型约束。



#### any

空接口在类型参数列表中很常见，在Go 1.18引入了一个新的预声明标识符，作为空接口类型的别名。

```go
// src/builtin/builtin.go

type any = interface{}
```

由此，我们可以使用如下代码：

```go
func foo[S ~[]E, E any]() {
	// ...
}
```





#### comparable 

`comparable` 表示go里面所有内置可比较类型：int  uint  float  bool  struct  指针 等一切可以比较的类型。用法和`any`一样 



### 类型推断

最后一个新的主要语言特征是类型推断。从某些方面来说，这是语言中最复杂的变化，但它很重要，因为它能让人们在编写调用泛型函数的代码时更自然。



#### 函数参数类型推断

对于类型参数，需要传递类型参数，这可能导致代码冗长。回到我们通用的 `min`函数：

```go
func min[T int | float64](a, b T) T {
	if a <= b {
		return a
	}
	return b
}
```

类型形参`T`用于指定`a`和`b`的类型。我们可以使用**显式类型实参调用**它：

```go
var a, b, m float64
m = min[float64](a, b) // 显式指定类型实参
```

在许多情况下，编译器可以从普通参数推断 `T` 的类型实参。这使得代码更短，同时保持清晰。

```go
var a, b, m float64

m = min(a, b) // 无需指定类型实参
```

这种从实参的类型推断出函数的类型实参的推断称为函数实参类型推断。==**函数实参类型推断只适用于函数参数中使用的类型参数，而不适用于仅在函数结果中或仅在函数体中使用的类型参数。**==例如，它不适用于像 `MakeT [ T any ]() T` 这样的函数，因为它只使用 `T` 表示结果。



#### 约束类型推断

Go 语言支持另一种类型推断，即**约束类型推断**。接下来我们从下面这个缩放整数的例子开始：

```go
// Scale 返回切片中每个元素都乘c的副本切片
func Scale[E constraints.Integer](s []E, c E) []E {
    r := make([]E, len(s))
    for i, v := range s {
        r[i] = v * c
    }
    return r
}
```

这是一个泛型函数适用于任何整数类型的切片。

现在假设我们有一个多维坐标的 `Point` 类型，其中每个 `Point` 只是一个给出点坐标的整数列表。这种类型通常会实现一些业务方法，这里假设它有一个`String`方法。

```go
type Point []int32

func (p Point) String() string {
    b, _ := json.Marshal(p)
    return string(b)
}
```

由于一个`Point`其实就是一个整数切片，我们可以使用前面编写的`Scale`函数：

```go
func ScaleAndPrint(p Point) {
    r := Scale(p, 2)
    fmt.Println(r.String()) // 编译失败
}
```

不幸的是，这代码会编译失败，输出`r.String undefined (type []int32 has no field or method String`的错误。

问题是`Scale`函数返回类型为`[]E`的值，其中`E`是参数切片的元素类型。当我们使用`Point`类型的值调用`Scale`（其基础类型为[]int32）时，我们返回的是`[]int32`类型的值，而不是`Point`类型。这源于泛型代码的编写方式，但这不是我们想要的。

为了解决这个问题，我们必须更改 `Scale` 函数，以便为切片类型使用类型参数。

```go
func Scale[S ~[]E, E constraints.Integer](s S, c E) S {
    r := make(S, len(s))
    for i, v := range s {
        r[i] = v * c
    }
    return r
}
```

我们引入了一个新的类型参数`S`，它是切片参数的类型。我们对它进行了约束，使得基础类型是`S`而不是`[]E`，函数返回的结果类型现在是`S`。由于`E`被约束为整数，因此效果与之前相同：第一个参数必须是某个整数类型的切片。对函数体的唯一更改是，现在我们在调用`make`时传递`S`，而不是`[]E`。

现在这个`Scale`函数，不仅支持传入普通整数切片参数，也支持传入`Point`类型参数。

这里需要思考的是，为什么不传递显式类型参数就可以写入 `Scale` 调用？也就是说，为什么我们可以写 `Scale(p, 2)`，没有类型参数，而不是必须写 `Scale[Point, int32](p, 2)` ？

新 `Scale` 函数有两个类型参数——`S` 和 `E`。在不传递任何类型参数的 `Scale(p, 2)` 调用中，如上所述，函数参数类型推断让编译器推断 `S` 的类型参数是 `Point`。但是这个函数也有一个类型参数 `E`，它是乘法因子 `c` 的类型。相应的函数参数是`2`，因为`2`是一个非类型化的常量，函数参数类型推断不能推断出 `E` 的正确类型(最好的情况是它可以推断出`2`的默认类型是 `int`，而这是错误的，因为Point 的基础类型是`[]int32`)。相反，编译器推断 `E` 的类型参数是切片的元素类型的过程称为**约束类型推断**。

约束类型推断从类型参数约束推导类型参数。当一个类型参数具有根据另一个类型参数定义的约束时使用。当其中一个类型参数的类型参数已知时，约束用于推断另一个类型参数的类型参数。

通常的情况是，当一个约束对某种类型使用 *~type* 形式时，该类型是使用其他类型参数编写的。我们在 `Scale` 的例子中看到了这一点。`S` 是 `~[]E`，后面跟着一个用另一个类型参数写的类型`[]E`。如果我们知道了 `S` 的类型实参，我们就可以推断出`E`的类型实参。`S` 是一个切片类型，而 `E`是该切片的元素类型。





## 使用泛型

### 定义泛型

**用泛型来定义类型，主要就是以下几种方式：**

```go
//一个泛型类型的切片
type Slice[T string | int | bool] []T

//一个泛型类型的map
type MyMap[K int | string, V string | int | []string] map[K]V

//一个泛型类型的结构体
type Student[T string | int] struct {
	name T
	age  T
}

//一个泛型类型的接口
type Speaker[T string | int] interface {
	speak(T) T
}

//一个泛型类型的通道
type MyChan[T string | int | bool] chan T

```

> 特殊类型的泛型

```go
type Wow[T int|string] int  //这么定义，其实实际没有意义

var a Wow[int] =123 //编译成功
var b Wow[string] =123 //编译成功
var c Wow[string] = "hello" //编译错误，因为"hello"不能赋值给底层类型int
```





### 泛型函数和方法

注意：

+ **方法中不能定义类型参数，除非是对接收者定义了泛型，方法中可以使用接收者的泛型T**
+ **定义类型泛型，在使用时也会对泛型T进行类型具体化的**
+ **泛型类型自动推导适用于函数和方法**

**定义方法：**

```go
//定义泛型方法

//记住一点，用type 来定义的新类型都可以有方法

type Slice[T string | int] []T


//可以看到，由于接收者已经对T进行了约束，所以方法的形参和返回值参数中用到类型T，可以不用再进行约束
func (s Slice[T]) f1() T {  
	var sum T
	for _, v := range s {
		sum = sum + v
	}
	return sum
}

func (s Slice[T]) f2(x, y T) T {   //共用泛型 T，由于在接收者中已经约束了，就不用再约束
	return x + y   //这种形式也是可以的，因为T的类型时string/int，这两种类型都能相加，如果T是bool则会报错，不能相加
}


//注意：在方法中不能定义类型参数
func (s Slice[T]) f3[V int | string](x, y V) V {  //这里会报错，因为：方法不能有类型参数
	return x + y
}

func main(){
    s := Slice[int]{1, 2}  //这里必须要type定义的类型，才能调用刚才定义的泛型方法
	s.f1()  
	s.f2(1, 2)
    
    
    var s1 = Slice[string]{"hello","world"}
	s1.f1()
	
	var s2 Slice[int] = []int{1,2}  //如果采用这种写法，必须保证前后类型一致
	s2.f1()
}

```





**定义函数：**

```go
//定义泛型函数

func Add[T string|int|float64](x ,y T)T{
	return x+y
}


func main(){
    Add[int](1,2)
    
    Add(1,2)  //golang中支持类型实参自动推导
 
    
	//Add(1.0,2)  //这种情况也是不行，实际上还是要保证参数类型一致，这种会报错
}
```





### 自定义泛型约束

当我们泛型的约束太多了，如何实现泛型快速重复使用？

==我们可以使用interface来定义泛型约束==

==**但需注意：**==

+ ==**interface里面是方法的话，可以用来定义接口**==
+ ==**interface里面是类型的话，可以用来自定义约束**==



```go
//自定义泛型约束
type mystring string
type MyInt interface{
	int|int8|int16|int32|int64|uint|uint8|mystring  //可能后面还有很多  |uint16|...
}

//定义好的约束可以直接拿来使用，还可以重复使用
func Sum[T MyInt](x,y T)T{
	return x+y
}

//还可以用来定义类型，都可以使用
type MySlice[T MyInt] []T  



//可以自定义类型，进行嵌套



func main(){
    var myslice MySlice[int] = []int{1,2}  //这样使用就可以了
}
```



**`~` 该特殊符号的使用：**

`~T`表示所以底层类型是T的类型，例如`~string`表示所有底层类型是`string`的类型集合。

```go
// ~ 用来给某个前缀进行类型衍生

type int000 int
type int8uuu int8

type MyInt interface{
    ~int|int8|int16    //~int 表示底层是int类型都适用
    //int|~int8|int16   //~int8 表示底层是int8类型都适用  
}

Sum [T MyInt] (x,y T)

func main(){
    Sum(int000(1), int000(2))  // int000的底层就是int类型，所以能用
    
    //这个会报错
    //Sum(int8uuu(1), int8uuu(2))  //int8uuu 底层是int8类型，我们在约束时只将 ~ 加到了int前面
}
```



==`any` 其实就是 `interface{}`，`any`是内置的类型，如果在约束时加上`any` ，表示所有类型都能用==

==`comparable` 表示go里面所有内置可比较类型：int  uint  float  bool  struct  指针 等一切可以比较的类型。用法和`any`一样==，==该比较是`==`或`!=`==

```go
func print[T any](arr []T){
	for _,v:=range arr{
		fmt.Println(v)
	}
}


//注意：
// 不支持泛型类型直接进行运算
func Add[T any](a, b T) T {  //这种用法会报错：Invalid operation: x+y (the operator + is not defined on T)
	return a + b
}

//报错：Invalid operation: x + y (the operator + is not defined on T)
func fff[T comparable](x, y T) T {
	return x+y
}

//报错：Invalid operation: x + y (the operator + is not defined on T)
func ffff[T any](x, y T) T {
	return x+y
}

结论：用any和comparable进行约束时，是不能进行算术、比较等运算的
```

==**注意：两个空接口类型的数据是不能直接进行比较或算术运算的**==

==**结论：用any和comparable进行约束时，是不能进行算术、比较等运算的**==





## 不应该使用泛型

### 不要用类型参数替换接口类型

众所周知，Go有接口类型。接口类型允许一种通用编程。

例如，广泛使用的`io.Reader`接口提供了一种通用机制，用于从包含信息（例如文件）或产生信息（例如随机数生成器）的任何值读取数据。如果对某个类型的值只需要调用该值的方法，则使用接口类型，而不是类型参数。读卡器易于阅读、高效且有效。不需要使用类型参数通过调用read方法从值中读取数据。

例如，你可能会尝试将这里的第一个函数签名（仅使用接口类型）更改为第二个版本（使用类型参数）。

```go
func ReadSome(r io.Reader) ([]byte, error)

func ReadSome[T io.Reader](r T) ([]byte, error)
```

不要做出那种改变。省略type参数使函数更容易编写，更容易读取，并且执行时间可能相同。

最后一点值得强调。虽然可以用几种不同的方式实现泛型，而且随着时间的推移，实现也会发生变化和改进，但在许多情况下，Go 1.18中使用的实现将处理类型为类型参数的值，就像处理类型为接口类型的值一样。这意味着使用类型参数通常不会比使用接口类型快。因此，不要为了速度而从接口类型更改为类型参数，因为它可能不会运行得更快。

### 如果方法实现不同，不要使用类型参数

==在决定是否使用类型参数或接口类型时，请考虑方法的实现。前面我们说过，如果一个方法的实现对于所有类型都是相同的，那么就使用一个类型参数。相反，如果每种类型的实现都不同，则使用接口类型并编写不同的方法实现，不要使用类型参数。==

例如，从文件读取的实现与从随机数生成器读取的实现完全不同。这意味着我们应该编写两个不同的Read方法，并使用像io.Reader这样的接口类型。

### 在适当的地方使用反射

Go具有运行时反射。反射允许一种泛型编程，因为它允许你编写适用于任何类型的代码。

==如果某些操作甚至必须支持没有方法的类型（不能使用接口类型），并且每个类型的操作都不同（不能使用类型参数），请使用反射。==

encoding/json包就是一个例子。我们不想要求我们编码的每个类型都有MarshalJSON方法，所以我们不能使用接口类型。但对接口类型的编码与对结构类型的编码不同，因此我们不应该使用类型参数。相反，该包使用反射。代码不简单，但它有效。有关详细信息，请参阅[源代码](https://go.dev/src/encoding/json/encode.go)。









# 包



## 包定义

+ 在go语言中，每个`.go`文件都必须在第一行声明文件归属的包

+ ==一个文件夹下的文件只能归属同一个包，通常包名和文件夹名一致==

+ go语言中，标识符首字母大写表示包外可见，首字母小写表示包内可见

+ 结构体可导出的字段的字符段名必须首字母大写，否则不可导出

+ 包别名

  ```go
  import importname "path/to/package"
  ```

+ 包匿名引入

  ```go
  import _ "github.com/go-sql-driver/mysql"
  ```

+ ==包名和目录名不一致：**建议包名和目录名一致，如果不一致，在import时要写目录名，引用时要写包名=**=





## go module 模式

在go1.11版本后，采用go module模式，不用gopath模式。

### go module 相关命令

![image-20221122200249031](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221122200249031.png)



### goproxy

goproxy 用于设置go模块代理，用于使go在后续拉去模块版本时能脱离传统的VCS方式，直接通过镜像站点快速拉取

```go
//goproxy 默认值：
https://proxy.golang.org,direct

//由于国内无法访问外网，通过配置中间代理
//目前社区采用以下两种：
https://goproxy.cn
https://goproxy.io

//设置goproxy命令：
go env -w GOPROXY=https://goproxy.cn,direct

//“direct” 为特殊指示符，用于指示 Go 回源到模块版本的源地址去抓取(比如 GitHub 等)
```



### goprivate

设置GOPROXY之后，go命令会从配置代理地址中拉取和校验依赖包。如果在项目中引入非公开包（公司内部git仓库或github私有仓库等），此时便无法正常从代理拉取这些非公开的依赖包，这个时候需要配置GOPRIVATE环境变量。

==GOPRIVATE告诉go命令哪些仓库属于私有仓库，不必通过代理服务器拉取和校验==

```go
//设置GOPRIVATE命令；
go env -w GOPRIVATE="git.mycompany.com"
//这样在拉取以git.mycompany.com为路径前缀的依赖包时就能正常拉取了。
```

如果公司内部自建了GOPROXY服务，那么可以通过设置`GONOPROXY=none`,允许通过内部代理拉取私有仓库的包。





### go module 引入包

+ 第一种方式，在项目目录下执行 `go get` 命令手动下载依赖的包：

  ```go
  //这种形式表示下载最新发布的版本 相当于 go get -u github.com/qimi/hello@latest
  holiday $ go get -u github.com/q1mi/hello   //u 表示强制更新现有的依赖
  go get: added github.com/q1mi/hello v0.1.1
  
  //或者可以指定版本下载
  go get -u github.com/qimi/hello@v0.1.1
  ```

  

+ 第二种方式，通过编辑go.mod文件，将依赖包和版本信息写入该文件总，然后在项目目录下执行`go mod download`下载依赖包

  ```go
  module holiday
  
  go 1.16
  
  require github.com/qimi/hello latest //或 require github.com/qimi/hello v0.1.1
  ```

  ```go
  //在命令行下，执行该命令：
   go mod download //此时就会下载go.mod文件中的依赖
  ```

  

+ go module 模式引入该项目下自己写的包

  ```go
  //可以直接以module名为项目路径为前缀 
  ```

  

+ go module模式导入其他项目下的包

  ```go
  //在go.mod文件中看，使用replace语句将这个依赖替换为使用相对路径表示的本地包
  
  module holiday
  
  go 1.16
  
  require github.com/q1mi/hello v0.1.1
  require liwenzhou.com/overtime v0.0.0
  
  replace liwenzhou.com/overtime  => ../overtime
  ```

  



# go module 模式

## bin、pkg、src

```go
bin  用于存放go可执行文件

pkg  用于存放编译后的文件，存放.a文件，a文件是编译过程中生成的,每个package都会生成对应的.a文件,Go在编译的时候先判断package的源码是否有改动,如果没有的话,就不再重新编译.a文件,这样可以加快速度。

src  用于存放源文件，在没有开启go module模式，相关依赖必须下载到src下

pkg mod 在开启go module模式后，相关依赖会下载到mod目录下
```





## GO111MODULE

go module 是Go1.11版本后推出的版本管理工具，go module是go语言默认的依赖管理工具。



GO111MODULE 有三个可选项：off、on、auto，默认值为auto

+ off禁用模块支持，编译时会从GOPATH和vendor文件夹中查找包
+ on启用模块支持，编译时会忽略GOPATH和vendor文件夹，只根据go.mod下载依赖
+ auto，，当项目在GOPATH/src外且项目根目录有go.mod文件时，开启模块支持。

```go
// 在windows系统中可以使用set命令配置临时环境变量。注：临时环境变量只对当前窗口有效
set GO111MODULE=on   

//永久改变环境变量
go env -w GO111MODULE=on

```





## GOPROXY

goproxy 用于设置go模块代理，用于使go在后续拉去模块版本时能脱离传统的VCS方式，直接通过镜像站点快速拉取

```go
//goproxy 默认值：
https://proxy.golang.org,direct

//由于国内无法访问外网，通过配置中间代理
//目前社区采用以下两种：
https://goproxy.cn
https://goproxy.io

//设置goproxy命令：
go env -w GOPROXY=https://goproxy.cn,direct

//“direct” 为特殊指示符，用于指示 Go 回源到模块版本的源地址去抓取(比如 GitHub 等)
```



## go mod命令



```
go mod download    下载依赖的module到本地cache（默认为$GOPATH/pkg/mod目录）
go mod edit        编辑go.mod文件
go mod graph       打印模块依赖图
go mod init        初始化当前文件夹, 创建go.mod文件
go mod tidy        增加缺少的module，删除无用的module，清除未使用到的依赖
go mod vendor      将依赖复制到vendor下
go mod verify      校验依赖
go mod why         解释为什么需要依赖
```



```go
go get 模块名 ----获取远程模块
go get 默认为 go get -d 其中d代表下载
一般采用go get -u 其中u表示强制更新现有依赖

go get 相当于 git clone + go install
//自Go 1.18起，go get 命令不再有编译包的功能。将只有添加，更新，移除 go.mod 文件中的依赖项的功能。侧重应用依赖项管理。

go install 相当于 go build + 把编译后的可执行文件放在GOPATH/Bin目录下
//go install: 在操作系统中安装 Go 生态的第三方命令行应用。不更改项目 go.mod 文件。侧重可执行文件的编译和安装。

go build 编译指定的源文件以及相关依赖


go list -m all ----查看当前依赖模块
go mod tidy ----清除未使用到的依赖
```



## go.sum

```go
go.sum文件和go.mod文件是go modules版本管理的指导文件，记录了所有依赖的module的校验信息，以防下载的依赖被恶意篡改，主要用于安全校验。

<module> <version>/go.mod <hash>
表示：模块路径，模块版本，哈希检验值，其中哈希检验值是用来保证当前缓存的模块不会被篡改。hash是以h1:开头的字符串，sha256


```



## go build和 go run 

### `go build`

`go build` 用来启动编译，将go语言程序和相关依赖编译成一个可执行文件。

+ 有参数时：
  + 如果当前目录下存在main包，则会生成一个与当前目录名同名的可执行文件
  + 如果不存在main包，则只会对当前目录下的程序源码进行语法检查，不生成可执行文件
+ 无参数
  + 如果参数为同一个main包下的源文件名，编译器将生成一个与第一个参数同名的可执行文件
  + 如果为非main包下的源文件，编译器只进行语法检查，不生成可执行文件



### `go run` 

`go run` 命令将编译和执行指令合二为一，会在编译后立即执行程序，生成一个临时文件，并不会生成可执行文件。

### 二者区别

+ 如果使用`go build`生成了可执行文件，那么不论当前环境有没有go 开发语言环境，都可以执行该可执行文件
+ 如果使用`go run`命令执行，只能在安装go开发环境的机器上执行
+ 在`go build`生成可执行文件时，会将当前的库文件以及依赖包含到可执行文件中，会导致可执行文件变大。





# 文件操作

计算机文件是存储在外部介质（通常为磁盘）上的数据集合，文件分为文本文件和二进制文件



## 文件打开和关闭



```go
//打开文件两种方式：
//打开文件，只能读
1，os.Open(filenme string)(*os.File,error) 打开一个文件，返回一个*File和一个err

//打开文件，能读能写，通过flag进行设置
2,os.OpenFile(name string,flag int,perm FileMode)(*File,error){...}


//关闭文件
通过文件对象中的Close()方法
func (*os.File)Close()error

```

![image-20221123181832810](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221123181832810.png)

## 读取文件



### Read()方法

```go
func (f *File)Read(b []byte)(n int,err error) 
//表示将文件中的内容读取到切片中，并返回一个读取的字节数和可能的错误
//读取到文件末尾后，error为io.EOF //io.EOF很多时候并不能算一个错误，更重要表示一个输入流结束了。

package main

import (
	"fmt"
	"io"
	"os"
)

func main() {
	fileObj, err := os.Open("./main.go")
	if err != nil {
		fmt.Println("file open failed,err:", err)
		return
	}
	defer fileObj.Close()
	var b = make([]byte, 1024)
	for {
		n, err := fileObj.Read(b)
		if err == io.EOF {                 //表示读取到文件末尾了
			fmt.Println(string(b[:n]))
			fmt.Println("文件读取完毕！")
			break
		}
		if err != nil {
			fmt.Println("file read failed,err:", err)
			return
		}
		fmt.Println(string(b[:n]))
	}
}
```



### bufio读取文件

bufio包实在file的基础上封装一层API，以支持更多的功能

bufio.Reader 结构包装了一个 io.Reader 对象，提供缓存功能，同时实现了 io.Reader 接口。

bufio包中缓冲区大小为4096字节，即4KB

```go
package main

import (
	"bufio"
	"fmt"
	"io"
	"os"
)

func main() {
	fileObj, err := os.Open("./main.go")
	if err != nil {
		fmt.Println("file open failed,err:", err)
		return
	}
	defer fileObj.Close()

	reader := bufio.NewReader(fileObj)  //新建一个bufio的读取对象
	for {
		str, err := reader.ReadString('\n') //表示读到换行符，即一行一行读取文件内容
		if err==io.EOF {
			fmt.Println(str)
			fmt.Println("文件读取完毕")
			break
		}
		if err != nil {
			fmt.Println("read file failed,err:", err)
			return
		}
		fmt.Print(str)
	}
}
```





### ioutil读取文件

ioutil包可以读取整个文件内容

```go
package main

import (
	"fmt"
	"io/ioutil"
	"os"
)

func main() {
	fileObj, err := os.Open("./main.go")
	if err != nil {
		fmt.Println("file open failed,err:", err)
		return
	}
	defer fileObj.Close()

	b,err:=ioutil.ReadAll(fileObj)
	if err!=nil {
		fmt.Println("文件读取失败")
		return
	}
	fmt.Println(string(b))
}
```

```go
import (
	"fmt"
	"io/ioutil"
)

func main() {
    //调用ReadFile()方法可以直接从文件中读取全部内容，可以不用创建一个fileObj文件对象
	b,err:=ioutil.ReadFile("./main.go")
	if err!=nil {
		fmt.Println("file read failed,err:",err)
		return
	}
	fmt.Println(string(b))
}
```







## 写入文件

针对向文件中写入内容，需要使用`os.OpenFile()`函数能以指定模式打开文件，从而实现了文件写入相关功能。

### Write和WriteString写入文件

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	fileObj,err:=os.OpenFile("./xx.txt",os.O_CREATE|os.O_RDWR|os.O_APPEND,0633)
	if err!=nil {
		fmt.Println("file open failed,err:",err)
		return
	}
	defer fileObj.Close()

    //使用write需要将字符串转换为切片类型
	n,err:=fileObj.Write([]byte("hello world!\n"))
	if err!=nil {
		fmt.Println("file write failed,err",err)
		return
	}
	fmt.Println("写入字节数：",n)

    //使用WriteString可以直接写入字符串
	n,err=fileObj.WriteString("我爱中国\n")
	if err!=nil {
		fmt.Println("file writeString failed,err:",err)
		return
	}
	fmt.Println("写入字节数：",n)
}
```





### bufio文件写入

**==注意：利用bufio包写入时，最后一定要Flush，因为bufio会将内容先写入缓冲区内==**

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	fileObj,err:=os.OpenFile("./xx.txt",os.O_CREATE|os.O_RDWR|os.O_APPEND,0633)
	if err!=nil {
		fmt.Println("file open failed,err:",err)
		return
	}
	defer fileObj.Close()
	
    //默认缓冲区大小为4096字节
	writer:=bufio.NewWriter(fileObj)
	nn,err:=writer.Write([]byte("hello world!\n"))
	if err!=nil {
		fmt.Println("file write failed,err：",err)
		return
	}
    //将缓冲区中内容刷新到底层的io.Writer对象中，如果不使用Flush方法，那么只有缓冲区满了才会将其内容刷新到底层对象中。
	writer.Flush()
	fmt.Println("写入字节数：",nn)
}
```





### ioutil文件写入



```go
package main

import (
	"fmt"
	"io/ioutil"
)

func main() {
    //调用WriteFile()方法可以直接向文件中写入内容
    //如果文件不存在，会自动创建文件，如果文件存在，则会先清空文件再写入。
    //最后一个参数表示文件权限
	err:=ioutil.WriteFile("./xx.txt",[]byte("hello world!\n"),0433)
	if err!=nil {
		fmt.Println("file write failed,err:",err)
		return
	}
}
```



## 文件拷贝

借助**`io.Copy()`方法实现一个拷贝文件函数**

`io.Copy(dst io.writer,src io.Reader)(written int64,err error)` 将src数据拷贝到dst，直到在src上达到EOF或发生错误。返回拷贝的字节数和遇到的第一个错误。，对成功的调用，返回值为nil而非EOF，因为copy定义为src读取到EOF，它不会将读取到EOF视为应报告的错误。

```go
//CopyFile 拷贝文件函数
func CopyFile(dstName,srcName string)(written int64,err error){
    //以读方式打开源文件
    src,err:=os.Open(srcName)
    if err!=nil{
        fmt.Printf("open %s failed,err:%w\n",srcName,err)
        return
    }
    defer src.Close()
    
    //以写|创建的方式打开目标文件
    dst,err:=os.OpenFile(dstName,os.O_WRONLY|os.O_CREATE,0644)
    if err！=nil{
        fmt.Printf("open %s failed,err:%w\n",dstName,err)
        return
    }
    defer dst.Close()
    return io.Copy(dst,src) //调用io.Copy()拷贝内容
}

func main() {
	_, err := CopyFile("dst.txt", "src.txt")
	if err != nil {
		fmt.Println("copy file failed, err:", err)
		return
	}
	fmt.Println("copy done!")
}
```







## 使用文件操作，模拟实现linux平台`cat`命令功能



```go
package main

import (
	"bufio"
	"flag"
	"fmt"
	"io"
	"os"
)

//cat命令实现
func cat(r *bufio.Reader){
    for {
        buf,err:=r.ReadBytes('\n')
        if err==io.EOF{
            //退出之前将已读到的内容输出
            fmt.Fprintf(os.Stdout,"%s",buf)
            break
        }
        fmt.Fprintf(os.Stdout,"%s",buf)
    }
}

func main(){
    flag.Parse() //解析命令行参数
    if flag.NArg()==0{
        //如果没有参数默认从标准输入读取内容
        cat(bufio.NewReader(os.Stdin))
    }
    //依次读取每个指定文件的内容并打印到终端
    for i:=0;i<flag.NArg();i++{
        f,err:=os.Open(flag.Arg(i))
        if err!=nil{
            fmt.Fprintf(os.Stdout,"reading from %s failed,err:%w\n",flag.Arg(i),err)
            continue
        }
        cat(bufio.NewReader(f))
    }
    
}
```



















# time包

## 时间类型

go语言中，使用time.Time类型表示时间，可以通过`time.Now()`来获取当前的时间对象，然后从时间对象中获取 年、月、日、时、分、秒等信息

```go
func timeDemo(){
    //获取当前时间对象
    now := time.Now() 
    
    //从当前时间对象中获取年月日时分秒
    year := now.Year()
    month := now.Month()
    day := now.Day()
    hour := now.Hour()
    minute := now.Minute()
    second := now.Second()
    
}
```

==注意：`time.Now()`获取CST中国时间，而不是UTC时间==







## Location和time Zone

go语言中使用location来映射具体的时区。

时区是根据世界各个国家与地区不同的经度而划分的时间定义，全球共划分24个时区，中国差不多跨5个时区，为了方便，用东八区即北京时间

```go
func timezoneDemo(){
	//中国没有夏令时，使用一个固定的8小时的UTC时差
	//对于很多其他国家需要考虑夏令时
	secondEastOfUTC := int((8*time.Hour).Seconds()) //偏移量
	//FixedZone 返回始终使用给定区域名称和偏移量的时区（Loaction）
	beijing := time.FixedZone("beijing",secondEastOfUTC)

	//如果当前系统又时区数据库，则可以加载一个位置得到对应的时区
	//例如；加载纽约所在的时区
	newYork,err:=time.LoadLocation("America/New_York")
	if err!=nil {
		fmt.Println("load American/New_York location failed",err)
		return
	}

	//加载上海对应的时区
//	Shanghai := time.LoadLocation("Asia/Shanghai")


	//创建时间对象必须要指定位置（时区），常用位置为time.Local(本地时间)和time.UTC（UTC）时间
	timeInUTC := time.Date(2009,1,1,12,0,0,0,time.UTC)
	timeInLocal:= time.Date(2009,1,1,12,0,0,0,time.Local)
	timeInNewYork := time.Date(2009,1,1,12,0,0,0,newYork)
	timeInBeijing := time.Date(2009,1,1,12,0,0,0,beijing)

	fmt.Println(timeInLocal,timeInNewYork,timeInBeijing,timeInUTC)


	//北京时间（东八区）比UTC早8小时，所以上面两个时间看似差了8小时，但表示是同一个时间
	timeAreEqual := timeInUTC.Equal(timeInBeijing)
	fmt.Println(timeAreEqual)

} 
```





## Unix Time

unix time 表示从1970年1月1日00：00：00 UTC到当前时间经历过的总秒数。

```go
//timestampDemo 时间戳

//将事件对象转换为时间戳
func timestampDemo(){
    now := time.Now() //获取当前时间对象
    timestamp := now.Unix() //秒级时间戳
    milli := now.UnixMilli() //毫秒时间戳
    micro := now.UnixMicro()  //微秒时间戳
    nano :=now.UnixNano() //纳秒时间戳
    fmt.Println(timestamp,milli,micro,nano)
}


//将时间戳转换为时间对象
func timeStampToTimeObjection(){
    //获取北京时间所在东八区时区对象
    secondEastOfUTC := int((8*time.Hour).Seconds())
    beijng:=time.FixedZone("beijing",secondEastofUTC)
    
    //北京时间 2022-02-22 22：22：22
    t := time.Date(2022,02,22,22,22,22,beijing)
    
    var (
        sec = t.Unix()
        msec = t.UnixMilli()
        usec = t.UnixMicro()
    )
    
    //直接用time包中的函数将时间戳转为时间对象
    //将秒级时间戳转为时间对象
    //第二个参数为不足1秒的纳秒数
    timeobj1:=time.Unix(sec,22)   //2022-02-22 22:22:22.0000000022 +0800 CST
    timeobj2:=time.UnixMilli(msec) //将毫秒转换为事件对象
    timeobj3:=time.UnixMicro(usec) //将微秒转换为时间对象
}

```





## 时间间隔

`time.Duration`是`time`包定义的一个类型，表示两个时间点之间经过的时间，以纳秒为单位。`time.Duration`表示一段时间间隔，可表示最长时间段大约为290年。

time包中定义的时间间隔类型的常量如下：

```go
const(
	Nanosecond Duration = 1
    Microsecond = 1000*Nanosecond
    Millisecond =1000*Microsecond
    Second = 1000*Millisecond
    Minute = 60*Second
    Hour = 60*Minute
)
//time.Duration 表示为1纳秒，time.Second表示为1秒
```







## 时间操作

go语言中提供了对时间操作的方法

```go
//对某个时间加上一个时间间隔
func (t Time)Add(d Duration)Time

//两个时间相减的差值
func (t Time)Sub(u Time)Duration

//判断两个时间是否相同，考虑时区的影响，不同时区标准时间可以正确比较。
//本方法和t==u不同，该方法会比较地点和时区信息
func (t Time)Equal(u Time)bool

//判断时间t是否在u之前，若是返回true，否则返回false
func (t Time)Before(u Time)bool

//判断时间t是否在u之后，若是返回true，否则返回false
func (t Time)After(u Time)bool

//Since返回从t到现在经过的时间，等价于time.Now().Sub(t)。
func Since(t Time) Duration
```





## 定时器

使用`time.Tick(时间间隔)`来设置定时器，定时器的本质是一个通道（channel）

```go
func tickDemo(){
    ticker := time.Tick(time.Second) //定义一个1秒间隔的定时器
    for i:=range ticker{
        fmt.Println(i) //每秒会执行一次任务
    }
}
```



## 时间格式化

利用`time.Format`函数将一个时间对象格式化输出为指定布局的文本表示形式**，在go语言中，采用2006-01-02 15：04：05.000**

==注意：如果想格式化 为12小时格式，需要在格式化布局中添加PM==

​		==小数部分想要保留指定位数就写0，如果想省略末尾可能的0就写9==

```go
func formatDemo(){
    now := time.Now()
    
    //24小时制
    fmt.Println(now.Format("2006-01-02 15:04:05.000 Mon Jan"))
    //12小时制
    fmt.Println(now.Format("2006-01-02 15:04:05.000 PM Mon Jan"))
    //小数点后写0，因为有3个0所以格式化输出的结果也保留3位小数
    fmt.Println(now.Format("2006/01/02 15:04:05.000"))
    //小数点后写9，会省略末尾可能出现的0
    fmt.Println(now.Format("2006/01/02 15:04:05.999"))
    //只格式化时分秒
    fmt.Println(now.Format("15:04:05"))
    //只格式化日期
    fmt.Println(now.Format("2006.01.02"))
}
```



## 解析字符串格式的时间

从文本的时间表示中解析出时间对象，time包中包括了`time.Parse`和`time.ParseInLocation`两个函数。

+ **`time.Parse`在解析时不需要额外指定时区信息**

```go
func parseDemo(){
    //在没有时区指示符的情况下，time.Parse返回UTC时间
    timeObj,_:=time.Parse("2006/01/02 15:04:05","2022/10/15 11:13:20")
    fmt.Println(timeObj) //2022-10-15 11:13:20 +0000 UTC 
    
    //在有时区指示符的情况下，time.Parse 返回对应的时区时间表示
    //RFC3339 = “2006-01-02T15:04:05Z07:00”
    timeObj,_=time.Parse(time.RFC3339,"2022-10-05T11:25:20+08:00")
    fmt.Println(timeObj)   //2022-19-05 11:25:20 +0800 CST
}
```



+ **`time.ParseInLocation`在解析时需要指定时区信息**

```go
func parseDemo(){
    now := time.Now()
    
    loc,_:=time.LoadLocation("Asia/Shanghai")
    timeObj,_:=time.ParseInLocation("2006/01/02 15:04:05","2022/10/04 11:23:20",loc)
    fmt.Println(timeObj.Sub(now))
}
```











# 日志

  go语言内置的log包实现了简单的日志服务。

## 使用Logger

log包定义了Logger类型，该类型提供一系列格式化输出的方法。可以通过调用函数`Print系列(Print|Printf|Println)`、`Fatal系列(Fatal|Fatalf|Fatalln)`和`Panic系列(Panic|Panicf|Panicln)`来使用



可以直接通过log包来调用上面提到的函数，==**默认它们会将日志信息打印到终端界面**==：

```go
package main

import "log"

func main() {
	//print/fatal/panic
	log.Println("这是一条普通日志")
	
	log.Panic("这是一个panic日志")
	log.Fatalln("这是一个fatal日志")
}


```

+ Fatal系列函数
  + 打印输出内容
  + 退出应用程序
  + 不执行defer方法
+ Panic系列函数
  + 当前函数立刻停止执行（不是主程序）
  + 执行defer方法
  + 返回给调用者caller
  + 调用函数收到了panic方法，从而它们会执行以上操作
  + 递归执行，直到最上层函数，如果都没有函数处理这个异常，应用程序就会停止。
+ 二者区别：
  + 第一种在报错之后会立即终止整个程序
  + 第二种在报错之后会终止当前报错的协程，然后返回到调用此函数的入口处继续执行

==注意：panic可以recover，fatal不能recover==



## 配置Logger

默认情况下，logger只会提供日志的时间信息，无法记录该日志的文件名和行号等。

log包中Flags函数会返回标准logger的输出配置，而SetFlags函数设置标准库logger的输出配置

```go
func Flags()int
func SetFlags(flag int)
```



### flag选项

log标准库中提供了flag选项，是一系列定义好的常量

```go
const (
    // 控制输出日志信息的细节，不能控制输出的顺序和格式。
    // 输出的日志在每一项后会有一个冒号分隔：例如2009/01/23 01:23:23.123123 /a/b/c/d.go:23: message
    Ldate         = 1 << iota     // 日期：2009/01/23
    Ltime                         // 时间：01:23:23
    Lmicroseconds                 // 微秒级别的时间：01:23:23.123123（用于增强Ltime位）
    Llongfile                     // 文件全路径名+行号： /a/b/c/d.go:23
    Lshortfile                    // 文件名+行号：d.go:23（会覆盖掉Llongfile）
    LUTC                          // 使用UTC时间
    LstdFlags     = Ldate | Ltime // 标准logger的初始值
)
```



### 配置日志前缀

log包中提供了关于日志信息前缀的两个方法

```go
//用于查看标准logger的输出前缀
func Prefix()string

//用于设置输出前缀
func SetPrefixs(prefix string)
```



```go
func main(){
    log.SetFlags(log.Llongfile|log.Lmicroseconds|log.Ldate)  //设置日志的flag项
    log.Println("这是一条普通日志")
    log.SetPrefix("[王者]")
    log.Println("这是一条普通日志")
}
//2022/11/25 16:13:22.321632 D:/code/GOPATH_code/src/github.com/ghost/fuxi/01/main.go:9: 这是一条普通日志
//[王者]2022/11/25 16:13:22.332850 D:/code/GOPATH_code/src/github.com/ghost/fuxi/01/main.go:11: 这是一条普通日志
```



### 配置日志输出位置

```go
//SetOutput函数用来设置标准logger的输出目的地，默认输出到终端 os.Stdout
func SetOutput(w io.writer)
```



可以通过调用SetOutput函数来设置输出位置

```go
func main(){
    logFile,_:=os.OpenFile("./xx.log",os.O_CREAT|os.O_WRONLY|os.O_APPEND,0644)
    log.SetOutput(logfile)
    log.SetFlags(log.Llongfile|log.Lmicroseconds|log.Ldate)
    log.Println("这是一条普通日志")
    log.SetPrefix("[日志级别]")
    log.Println("这是一条普通日志")
}
```



==在使用标准的logger，通常将日志配置卸载init函数中==

```go
func init() {
	logFile, err := os.OpenFile("./xx.log", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0644)
	if err != nil {
		fmt.Println("open log file failed, err:", err)
		return
	}
	log.SetOutput(logFile)
	log.SetFlags(log.Llongfile | log.Lmicroseconds | log.Ldate)
}
```



## 创建Logger

log标准库中提供一种创建新的logger对象的构造函数—New，支持创建自己的logger示例

```go
func New(out io.Writer,prefix string,flag int)*Logger
```

New创建一个Logger对象，其中out为日志写入地方，prefix日志前缀，flag定义日志属性

```go
//示例
func main() {
	logger := log.New(os.Stdout, "<New>", log.Lshortfile|log.Ldate|log.Ltime)
	logger.Println("这是自定义的logger记录的日志。")
}
```



==GO内置的log库功能有限，无法满足记录不同级别日志情况，在实际项目中根据自己的需要选择使用第三方的日志库，如logrus、zap==









## 日志库设计

+ 实际项目中，记录日志，首先要对日志的需求分析；
  + 支持向不同地方输出日志
  + 日志级别：debug，trace，info，warning，error，fatal
  + 日志支持开关控制
  + 日志要有时间、行号、文件名、日志级别、日志信息
  + 日志文件切割：如按日志大小切割，按日期切割

 

















# Error接口和错误处理

go语言的错误处理与其他语言不一样，go语言将错误当成一种错误来处理，强调判断错误、处理错误，而不是catch捕获异常。

## Error接口

go语言中使用一个名为`error`接口来表示错误类型

```go
type error interface{
    Error()string
}
```

当一个函数或方法需要返回错误时，通常将错误作为最后一个返回值。

由于error是接口类型，默认值为nil。所以通常将调用函数返回的错误与nil进行比较，以此来判断函数是否返回错误。

```go
file,err:=os.Open("./xx.txt")
if err!=nil{
    //当我们使用fmt包打印错误时会自动调用error类型的Error方法，也就是会打印错误的描述信息
    fmt.Println("打开文件夹失败，err：",err)
    return
}
```





## 创建错误

+ 可以根据需求自定义error，最简单的方式为errors包提供的New函数创建一个错误。

  ```go
  func New(text string)error
  //接收一个字符串参数返回包含该字符串的错误
  
  //其中，标准库io.EOF错误定义如下；
  var EOF = errors.New("EOF")
  ```

  

+ 当我需要传入格式化的错误描述信息时，可以使用fmt.Errorf

  ```go
  fmt.Errorf("查询数据库失败，err:%v\n",err)
  //这种方式会丢失原有的错误类型，只拿到错误描述的文本信息
  
  
  //为了不丢失函数调用的错误链，使用fmt.Errorf时搭配格式化动词 %w，可以实现基于已有的错误在包装得到一个新的错误
  fmt.Errorf("查询数据库失败，err：%w\n",err)
  //对于这种二次包装的错误，errors包中提供了以下三个方法
  func Unwrap(err error) error                 // 获取被包装的原error
  func Is(err, target error) bool              // 判断err是否包含target
  func As(err error, target interface{}) bool  // 判断err是否为target类型
  ```

  



## 错误结构体类型

此外，可以通过自定义一个结构体类型，实现error接口

```go
// OpError 自定义结构体类型
type OpError struct {
	Op string
}

// Error OpError 类型实现error接口
func (e *OpError) Error() string {
	return fmt.Sprintf("无权执行%s操作", e.Op)
}
```





# 反射

## 变量内在机制

go语言变量分为两部分：

+ 类型信息：预先定义的元信息
+ 值信息：程序运行过程中可动态变化的



## 反射 

Go语言提供了一种机制在运行时更新和检查变量的值、调用变量的方法和变量支持的内在操作，但是在编译时并不知道这些变量的具体类型，这种机制被称为反射。

反射是指在程序运行期间对程序本身进行访问和修改的能力。程序在编译时，变量会转换为内存地址，变量名不会编译器写入可执行部分。在程序运行期间时，程序无法获取自身的信息。

==支持反射的语言可以在程序编译期间将变量的反射信息，如字段名、类型信息、结构体信息等整合到可执行文件中，并给程序提供接口访问反射信息，这样就可以在程序运行期间获取类型的反射信息，并且有能力修改它们。==

Go程序在运行期间使用reflect包访问程序的反射信息。



## reflect包

任何`接口值`都是`一个具体类型`和`具体类型的值`两部分组成。reflect包提供了反射功能，==**任意接口值在反射中都可以理解为`reflect.Type和reflect.Value`两部分组成，并且reflect包提供了reflect.TypeOf和refect.ValueOf两个函数来获取任意对象的Value和Type**==。

### TypeOf

在go语言中，使用`reflect.TypeOf()`函数可以获得任意值的类型对象`(reflect.Type)`，程序通过类型对象可以访问任意值的类型信息。

```go
package main
import(
    "fmt"
    "reflect"
)
func reflectType(x interface{}){
    v:=reflect.TypeOf(x)         //获取x的类型信息
    fmt.Printf("type:%v\n",v)
}
func main(){
    var a float32 =3.14
    reflectType(a)
    var b int64 = 100
    reflectType(b)
}

```



在反射中，**类型还分为两类：类型（Type）和 种类（Kind）**。因为go语言中可以使用type关键字来构造很多自定义类型，而种类（Kind）就是指底层类型。在反射中，区分指针、结构体等大品种的类型时，就会用到中种类（Kind）。

==**即在类型信息中，又包含了 类型（Type）和 种类（Kind）**==

```go
package main

import (
	"fmt"
	"reflect"
)

func reflectType(x interface{}){
	v := reflect.TypeOf(x)
	fmt.Println("type:",v.Name(),"kind:",v.Kind())
}
type myInt int64

func main(){
	var a *float32  //指针
	var b myInt  //自定义类型
	var c rune  //类型别名
    var cc [2]int32  //数组
	reflectType(a)   //type:  kind: ptr
	reflectType(b)  //type: myInt kind: int64
	reflectType(c)  //type: int32 kind: int32
    reflectType(cc) //type:  kind: array

	type person struct{
		name string
		age int
	}
	type book struct{
		title string
	}
	var d = person{
		name :"zhangsan",
		age :23,
	}
	var e = book{"golang"}
	reflectType(d) //type: person kind: struct
	reflectType(e)  //type: book kind: struct
}
```

==注意：在反射中，数组、切片、Map、指针等类型变量，它们的`.Name()`都是返回 `空` 。==



reflect包中定义的Kind类型如下：

```go
type Kind uint
const (
    Invalid Kind = iota  // 非法类型
    Bool                 // 布尔型
    Int                  // 有符号整型
    Int8                 // 有符号8位整型
    Int16                // 有符号16位整型
    Int32                // 有符号32位整型
    Int64                // 有符号64位整型
    Uint                 // 无符号整型
    Uint8                // 无符号8位整型
    Uint16               // 无符号16位整型
    Uint32               // 无符号32位整型
    Uint64               // 无符号64位整型
    Uintptr              // 指针
    Float32              // 单精度浮点数
    Float64              // 双精度浮点数
    Complex64            // 64位复数类型
    Complex128           // 128位复数类型
    Array                // 数组
    Chan                 // 通道
    Func                 // 函数
    Interface            // 接口
    Map                  // 映射
    Ptr                  // 指针
    Slice                // 切片
    String               // 字符串
    Struct               // 结构体
    UnsafePointer        // 底层指针
)
```



### ValueOf

`reflect.ValueOf()`返回的是`reflect.Value`类型，其中包含了原始值的信息。

**==`reflect.Value`与原始值之间可以互相转换。==**

`reflect.Value`类型提供的获取原始值的方法如下：

| 方法                     | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ |
| Interface() interface {} | 将值以 interface{} 类型返回，可以通过类型断言转换为指定类型  |
| Int() int64              | 将值以 int 类型返回，所有有符号整型均可以此方式返回          |
| Uint() uint64            | 将值以 uint 类型返回，所有无符号整型均可以此方式返回         |
| Float() float64          | 将值以双精度（float64）类型返回，所有浮点数（float32、float64）均可以此方式返回 |
| Bool() bool              | 将值以 bool 类型返回                                         |
| Bytes() []bytes          | 将值以字节数组 []bytes 类型返回                              |
| String() string          | 将值以字符串类型返回                                         |



#### 通过反射获取值

```go
func reflectValue(x interface{}){
    v := reflect.ValueOf(x)
    k := v.Kind()         //reflect.Type和reflect.Value均实现了Kind方法
    switch k{
        case reflect.Int64:
        	fmt.Printf("type is int64,value is %d\n",int64(v.Int())
        case reflect.Float32:
            fmt.Printf("type is float32,value is %d\n",float32(v.Float()))
    }  
}
func main(){
   var a float32 = 3.14
    var b int64 = 100
    reflectValue(a) //type is float32,value is 3.140000
    reflectValue(b)  //type is int64,value is 100
    
    //将int类型的原始值转换为reflect.Value类型
    c := reflect.ValueOf(10)
    fmt.Println("type c:%T\n",c)  //type c:reflect.Value
}
```



#### 通过反射设置变量的值

在函数中通过反射修改变量的值，需要注意函数参数传递是值传递还是引用传递。对于值传递，**必须传递变量的地址才能修改变量的值**，==在反射中使用专有的`Elem()方法`来获取指针对应的值==

==通过反射来设置值，必须通过SetInt/SetFloat....等reflect包中方法来设置值。==

```go
package main

import(
    "fmt"
    "reflect"
)
func reflectSetValue(x interface{}){
    v := reflect.ValueOf(x)
    if v.Kind() == reflect.Int64{
        //通过反射设置值，必须通过Set***方法来进行设置
        v.SetInt(200) //修改的是副本，reflect包会引发panic
    }
}
func reflectSetValue1(x interface{}){
    v := reflect.ValueOf(x)
    //反射中使用Elem()方法来获取指针对应的值
    if v.Elem().Kind()==reflect.Int64{
        v.Elem().SetInt(200)
    }
}

func main(){
    var a int64 = 100
    //reflectSetValue(a) //panic:reflect:reflect.Value.SetInt using unaddressable value  
    reflectSetValue1(&a)
    fmt.Println(a)
}
```



#### isNil()和isValid()

`reflect.Value`类型实现了`IsNil()`和`IsValid()`方法

+ isNil()

  ```go
  func (v Value)IsNil()bool
  IsNil()报告v持有的值是否为nil。v持有的值分类必须是通道、函数、接口、映射、指针、切片之一；否则IsNil函数会导致panic
  ```

+ isValid()

  ```go
  func (v Value)IsValid()bool
  IsValid()返回v是否持有一个值，如果v是Value零值则会返回假，此时v除了IsValid、String、Kind之外方法都会导致panic
  ```

  

+ 示例：

  `IsNil()`常被用于判断指针是否为空；`IsValue()` 常被用于判定返回值是否有效

  ```go
  func main() {
  	// *int类型空指针
  	var a *int
  	fmt.Println("var a *int IsNil:", reflect.ValueOf(a).IsNil())
  	// nil值
  	fmt.Println("nil IsValid:", reflect.ValueOf(nil).IsValid())
  	// 实例化一个匿名结构体
  	b := struct{}{}
  	// 尝试从结构体中查找"abc"字段
  	fmt.Println("不存在的结构体成员:", reflect.ValueOf(b).FieldByName("abc").IsValid())
  	// 尝试从结构体中查找"abc"方法
  	fmt.Println("不存在的结构体方法:", reflect.ValueOf(b).MethodByName("abc").IsValid())
  	// map
  	c := map[string]int{}
  	// 尝试从map中查找一个不存在的键
  	fmt.Println("map中不存在的键：", reflect.ValueOf(c).MapIndex(reflect.ValueOf("娜扎")).IsValid())
  }
  ```

  

## 结构体反射

### 与结构体相关方法

任意值通过`reflect.TypeOf()`获得反射对象信息后，如果它的类型是结构体，可以通过反射值对象`(reflect.Type)`的`NumFiled()`和`Filed()`方法获得结构体成员的详细信息。

`reflect.Type`和`reflect.Value`中与获取结构体成员的相关的方法如下：

| 方法                                                        | 说明                                                         |
| ----------------------------------------------------------- | ------------------------------------------------------------ |
| Field(i int) StructField                                    | 根据索引，返回索引对应的结构体字段的信息。**从0开始，跟数组下标一样** |
| NumField() int                                              | 返回结构体成员字段数量。                                     |
| FieldByName(name string) (StructField, bool)                | 根据给定字符串返回字符串对应的结构体字段的信息。             |
| FieldByIndex(index []int) StructField                       | 多层成员访问时，根据 []int 提供的每个结构体的字段索引，返回字段的信息。 |
| FieldByNameFunc(match func(string) bool) (StructField,bool) | 根据传入的匹配函数匹配需要的字段。                           |
| NumMethod() int                                             | 返回该类型的方法集中方法的数目                               |
| Method(int) Method                                          | 返回该类型方法集中的第i个方法                                |
| MethodByName(string)(Method, bool)                          | 根据方法名返回该类型方法集中的方法                           |



==`reflect.Type`和`reflect.Value`在对结构体操作时有相同的方法，也有各自不同的方法，注意使用==





### StructField和Method类型

`StructField`类型用来描述结构体中的一个字段的信息。

`StructField`的定义如下：

```go
type StructField struct {  //结构体中的每个字段包含的信息
    // Name是字段的名字。PkgPath是非导出字段的包路径，对导出字段该字段为""。
    // 参见http://golang.org/ref/spec#Uniqueness_of_identifiers
    Name    string
    PkgPath string
    Type      Type      // 字段的类型
    Tag       StructTag // 字段的标签
    Offset    uintptr   // 字段在结构体中的字节偏移量
    Index     []int     // 用于Type.FieldByIndex时的索引切片
    Anonymous bool      // 是否匿名字段
}
```



`Mehtod`的定义如下：

```go
type Method struct{
    Name string //方法名
    PkgPath string // 是非导出方法的包路径，对导出方法该字段为""。
    Type Type //方法类型
    Func value //方法入口地址
    Index int  //用于Type。Method时的索引
}
```









### 结构体反射示例

当我们使用反射得到一个结构体数据之后可以通过索引依次获取其字段信息，也可以通过字段名去获取指定的字段信息。

```go
package main

import(
    "fmt"
    "reflect"
)

type student struct{
    Name string `json:"name"`
    Score int `json:"score"`
}

//匿名结构体嵌套
type studentInformation struct{
	school string
	student
}

func main(){
    stu1 := student{
        Name:"小王子",
        Score:88,
    }
	t := reflect.TypeOf(stu1)
	fmt.Println(t.Name(),t.Kind())

	for i:=0;i<t.NumField();i++{
		field := t.Field(i)
		fmt.Printf("name:%s,index:%v,type:%v,json tag:%v\n",field.Name,field.Index,field.Type,field.Tag.Get("json"))   //因为标签是键值对形式，肯定存在这个方法
	}


	//通过字段名来获取指定结构体字段信息
	if scoreFiled,ok:=t.FieldByName("score");ok{
		fmt.Printf("name:%s,index:%v,type:%v,json tag:%v\n",scoreFiled.Name,scoreFiled.Index,scoreFiled.Type,scoreFiled.Tag.Get("json"))
	}

	var stu2 = studentInformation{
		"第一中学",
		stu1,
	}
	t1 := reflect.TypeOf(stu2)

	//通过字段名获取指定结构体字段信息
	if field1,ok:= t1.FieldByName("student");ok{
		fmt.Printf("name:%s,index:%v,type:%v,json tag:%v\n",field1.Name,field1.Index,field1.Type,field1.Tag.Get("json"))
		fmt.Printf("Anonymous:%t\n",field1.Anonymous)
	}

	//通过索引切片实现多层成员访问
	field2:=t1.FieldByIndex([]int{1,1})
	fmt.Println(field2)
	
}
```





接下来编写一个函数`printMethod(s interface{})`来遍历打印s包含的方法

```go
// 给student添加两个方法 Study和Sleep(注意首字母大写)
func (s student) Study() string {
	msg := "好好学习，天天向上。"
	fmt.Println(msg)
	return msg
}

func (s student) Sleep() string {
	msg := "好好睡觉，快快长大。"
	fmt.Println(msg)
	return msg
}

func printMethod(x interface{}) {
	t := reflect.TypeOf(x)
	v := reflect.ValueOf(x)

	fmt.Println(t.NumMethod())
	for i := 0; i < v.NumMethod(); i++ {
		methodType := v.Method(i).Type()
		fmt.Printf("method name:%s\n", t.Method(i).Name)
		fmt.Printf("method:%s\n", methodType)
		
        // 通过反射调用方法传递的参数必须是 []reflect.Value 类型
		var args = []reflect.Value{}
        
        //通过反射来调用方法，使用call
		v.Method(i).Call(args)
	}
}


/*
执行结果如下：
	2
	method name:Sleep
	method:func() string
	好好睡觉，快快长大。
	method name:Study
	method:func() string
	好好学习，天天向上。

*/
```



## 反射是把双刃剑

反射是一个强大并富有表现力的工具，能让我们写出灵活代码。但是反射不应该滥用，原因有以下三个：

+ 基于反射的代码是极其脆弱的，反射中的类型错误会在真正运行时候引发panic，那可能是在代码完成后很长时间内
+ 大量使用反射的代码难以理解
+ 反射的性能低下，基于反射实现的代码通常比正常代码运行速度慢一到两个数量级







# strcnov包

在go语言中，`strconv`包实现了基本数据类型和字符串表示的相互转换，主要有以下函数：`Atoi()  Itoa()  pare系列  format系列  append系列`.

## string与int类型转换

### Atoi()

`Atoi()`函数用于将字符串类型的整数转换为int类型

```go
func Atoi(s string)(i int,err error)
```

如果传入的字符串参数无法转换为int类型，就会返回错误

```go
s1:="100"
i1,err:=strconv.Atoi(s1)
if err!=nil{
    fmt.Println("can not convert to int")
}else{
    fmt.Printf("type:%T value:%v\n",i1,i1)  //type:int value:100
}
```



### Itoa()

`Itoa()`函数将int类型的数据转换为对应的字符串表示

```go
func Itoa(i int)string
```

示例：

```go
i2 :=200
s2 :=strconv.Itoa(i2)
fmt.Printf("type:%T,value:%#v\n",s2,s2)
```



### a的典故

在c语言中没有string类型而用字符数组（array）表示字符串，所以`Itoa`对很多C系的程序员好理解。



## Parse系列函数

Parse系列函数用于将字符串转为给定类型的值：ParseBool()  ParseFloat()  ParseInt()  ParseUint()

### ParseBool()

```go
func ParseBool(str string)(value bool,err error)
//返回字符串表示的bool值。
//它接受1、0、t、f、T、F、true、false、True、False、TRUE、FALSE；否则返回错误
```

### ParseInt()

```go
func ParseInt(s string,base int,bitSize int)(i int64,err error)
//返回字符串表示的整数值，接受正负号。
//base指定进制（2-36），如果base为0，则会从字符串前缀判断，”0X“是16进制，”o”是8进制，否则是10进制
//注意：base是指数字字符串的进制，比如二进制，八进制，十六进制
//bitSize指定结果必须能无溢出赋值的整数类型，0，8，16，32，64分别代表int，int8，int16，int32，int64
//返回的err是*NumErr类型的，如果语法有误，err.Error=ErrSyntax；如果结果超出类型范围err.Error=ErrRange。
```

### ParseUint()

```go
func ParseUint(s string,base int,bitSize int)(n uint64,err error)
//ParseUint类似ParseInt，但不接受正负号，用于无符号类型
```

### ParseFloat()

```go
func ParseFloat(s string,bitSize int)(f float64,err error)
//解析一个表示浮点数的字符串并返回其值
//如果s合乎语法规则，函数返回最为接近s表示值的一个浮点数
//bitSize指定期望接收类型，32是float32，64是float64
//返回的err是*NumErr类型的，如果语法有误，err.Error=ErrSyntax；如果结果超出类型范围err.Error=ErrRange。
```

### 示例

```go
b, err := strconv.ParseBool("true")
f, err := strconv.ParseFloat("3.1415", 64)
i, err := strconv.ParseInt("-2", 10, 64)
u, err := strconv.ParseUint("2", 10, 64)
```



## Format系列函数

Format系列函数实现了将给定数据格式化为string类型数据的功能

### FormatBool()

```go
func FormatBool(b bool) string
//根据b的值返回”true”或”false”。
```

### FormatInt()

```go
func FormatInt(i int64, base int) string
//返回i的base进制的字符串表示。base 必须在2到36之间，结果中会使用小写字母’a’到’z’表示大于10的数字。
```

### FormatUint()

```go
func FormatUint(i uint64, base int) string
//是FormatInt的无符号整数版本。
```

### FormatFloat()

```go
func FormatFloat(f float64, fmt byte, prec, bitSize int) string
//函数将浮点数表示为字符串并返回。
//bitSize表示f的来源类型（32：float32、64：float64），会据此进行舍入。

//fmt表示格式：’f’（-ddd.dddd）、’b’（-ddddp±ddd，指数为二进制）、’e’（-d.dddde±dd，十进制指数）、’E’（-d.ddddE±dd，十进制指数）、’g’（指数很大时用’e’格式，否则’f’格式）、’G’（指数很大时用’E’格式，否则’f’格式）。

//prec控制精度（排除指数部分）：对’f’、’e’、’E’，它表示小数点后的数字个数；对’g’、’G’，它控制总的数字个数。如果prec 为-1，则代表使用最少数量的、但又必需的数字来表示f。
```

### 示例

```go
s1 := strconv.FormatBool(true)
s2 := strconv.FormatFloat(3.1415, 'E', -1, 64)
s3 := strconv.FormatInt(-2, 16)
s4 := strconv.FormatUint(2, 16)
```



## 其他

### isPrint()

```go
func IsPrint(r rune)bool
//返回一个字符是否是可打印的，和unicode.IsPrint一样，r必须是：字母（广义）、数字、标点、符号、ASCII空格
```

### CanBackquote()

```go
func CanBackquote(s string)bool
//返回字符串s是否可以不被修改的表示为一个单行的、没有空格和tab之外控制符的反引号字符串。=
```

### 其他

除上文列出的函数外，`strconv`包中还有Append系列、Quote系列等函数。具体用法可查看[官方文档](https://golang.org/pkg/strconv/)。





# 并发

go语言在语言层面天生支持并发，充分利用CPU的多核优势。同时实现了自动垃圾回收机制。

go语言在并发机制上运用起来非常简单，在启动并发的方式上直接添加语言级的关键字go就可以实现，相比其它编程语言更轻量级。

## 基本概念

### 串行、并发和并行

+ 串行：只有干完一件事才能干另一件事
+ 并发：同一段时间内执行多个任务
+ 并行：同一时刻执行多个任务

### 进程、线程和协程

+ 进程：程序在操作系统中的一次执行过程，系统进行资源分配和调度的一个独立单位
+ 线程：操作系统基于进程开启的轻量级进程，是操作系统调度执行的最小单位
+ 协程：非操作系统提供而是由用户自行创建和控制的用户态“线程”，比线程更轻量级

### 并发模型

常见的并发模型：

+ 线程&锁模型
+ Actor模型
+ CSP模型
+ Fork&Join模型

==**go语言中并发程序主要是通过基于CSP的goroutine和channel来是实现**，也支持使用传统的多线程共享内存的并发方式==



## goroutine

​	goroutine是go语言支持并发的核心，在一个go程序中同时创建成百上千个goroutine是非常普遍的，一个goroutine会以一个很小的栈开始其生命周期，==一般只需要2KB==。==区别于操作系统的线程是由系统内核进行调度，goroutine是go运行机制（runtime）负责调度。==

​	==**go运行机制（runtime）会智能地将m个groutine合理分配给n个操作系统的线程，实现类似m：n的调度机制，不需要go开发者自行在代码层面维护一个线程池**。==

​	==goroutine是go程序中最基本的并发执行单元。每一个go程序都至少包含一个goroutine--main goroutine，当go程序启动时它会自动创建。==

​	==在go语言中不需要自己写进程、线程、协程，当你需要某个任务并发执行的时候，只需要把这个任务包装成一个函数，开启一个goroutine去执行这个函数就可以了，简单粗暴。==



### go关键字

​	go语言中执行goroutine非常简单，只需要在函数或方法调用前加上`go`关键字就可以创建一个goroutine，从而让该函数或方法子在新建的goroutine中执行。

```go
go f() //创建一个新的goroutine运行函数f
```

​	匿名函数也支持使用`go`关键字创建goroutine去执行

```go
go func(){
    ...
}()
```

一个goroutine必定对应一个函数或方法，可以创建多个goroutine去执行相同的函数或方法



### 启动单个goroutine

启动goroutine方式：只需要在调用函数（普通函数和匿名函数）前加上一个`go`关键字。

```go
package main

import (
	"fmt"
)

func hello() {
	fmt.Println("hello")
}

func main() {
	hello()
	fmt.Println("你好")
}
//代码执行后的结果为：
//hello
//你好
```

代码中hello函数和其后面的打印语句是串行的

![image-20221127142151769](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221127142151769.png)

在调用函数hello前加上关键字go，即启动一个共goroutine去执行hello这个函数。

```go
func main() {
	go hello() // 启动另外一个goroutine去执行hello函数
	fmt.Println("main goroutine done!")
}
//打印结果：你好
//这一次执行结果只在终端打印了“你好”，并没有打印“hello”。
```

**在go程序启动时，go程序会为main函数创建一个默认的goroutine**。在上面代码中，在main函数中使用go关键字创建了一个goroutine去执行hello函数，而此时main goroutine还在继续往下执行，程序中存在两个并发执行的goroutine。当main函数结束时整个程序就结束了，同时main goroutine也结束了，所有由main goroutine创建的goroutine也会一起结束。即main函数结束太快了，另一个goroutine中的函数还未执行完成，程序就退出了。

最简单的方式：在main函数中“time.Sleep”一秒钟。

```go
package main

import (
	"fmt"
	"time"
)

func hello() {
	fmt.Println("hello")
}

func main() {
	go hello()
	fmt.Println("你好")
	time.Sleep(time.Second)
}
//打印结果：
//你好
//hello
```



为啥先打印你好？

​	因为：在程序中创建goroutine执行函数需要一定的开销，于此同时main函数所在的goroutine是继续执行的。

![image-20221127143219435](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221127143219435.png)

在上面程序中使用`time.Sleep`让main goroutine等待hello goroutine执行结束不优雅，也不准确。





go语言中通过`sync`包为我们提供了常用的并发原语。

​	正常情况下，新建的goroutine的结束过程是不可控制的，唯一可以保证终止goroutine行为是main goroutine的终止。即，我们不知道哪个goroutine什么时候结束。但是，在很多情况下，我们需要直到goroutine是否完成。此时，需要借助sync包的WaitGroup来是实现。



```go
package main

import(
	"fmt"
    "sync"
)

//声明全局等待组变量
var wg sync.WaitGroup

func hello(){
    fmt.Println("hello")
    wg.Done() //告知当前goroutine执行完成
}
func main(){
    wg.Add(1) //登记一个goroutine
    go hello()
    fmt.Println("你好")
    wg.Wait()  //阻塞，等待登记的goroutine完成
}
```





### 启动多个goroutine

在go语言中，可以启动多个goroutine。同样可以使用`sync.WaitGroup`来实现goroutine的同步。

```go
package main

import(
	"fmt"
    "sync"
)

var wg sync.WaitGroup

func hello(i int){
    defer wg.Done() //goroutine结束就登记-1
    fmt.Println("hello",i)
}
func main(){
    for i:=0;i<10;i++{
        wg.Add(1) //启动一个goroutine就登记+1
        go hello(i)
    }
    wg.Wait() //等待所有登记的goroutine都结束
}
```

多次执行上面代码发现每次终端打印数字的顺序都不一致。这是因为10个goroutine是并发执行的，而goroutine的调度是随机的。



### 动态栈

​	==操作系统的线程一般都有固定的栈内存（通常为2MB），而go语言中的goroutine非常轻量级，一个goroutine的初始栈空间很小（一般为2KB），所以在go语言中一次创建数万个goroutine也是可能的。并且goroutine的栈不是固定的，可以根据需要动态地增大或缩小，**go的runtime会自动为goroutine分配合适的栈空间**。==



### goroutine调度

+ 操作系统内核在调度时会挂起当前正在执行的线程并将寄存器中的内容保存到内存中，然后选出接下来要执行的线程并内存中恢复该线程的寄存器信息，然后恢复执行该线程的现场并开始执行线程。从一个线程切换到另一个线程需要完整的上下文切换。因为可能需要多次内存访问，索引这个切换上下文的操作开销较大，会增加运行的cpu周期。
+ 区别于操作系统内核调度操作系统线程，goroutine的调度是go语言运行时（runtime）层面的实现，是完全由go语言本身实现的一套调度系统——go scheduler。==它的作用是按照一定规则将所有的goroutine调度到操作系统线程上执行==。



+ **go语言当前调度器采用`GMP`调度模型**

  ![image-20221127152309812](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221127152309812.png)

  + G：表示goroutine，每执行一次`go f()`就创建一个G，包含要执行的函数和上下文信息。
  + 全局队列（Global Queue）：存放等待的运行的G
  + P：表示goroutine执行所需的资源，最多有GOMAXPROCS个。
  + P的本地队列：同全局队列类似，存放的也是等待运行的G，存的数量有限，不超过256个。新建G时，G优先加入P的本地队列，如果本地队列满了会批量移动部分G到全局队列中。
  + M：线程想运行任务就得获取P，从P的本地队列获取G，当P的本地队列为空时，M也会尝试从全局队列或其他P的本地队列获取G。M运行G，G执行完成后，M会从P中获取下一个G，不断重复下去。
  + Goroutine调度器和操作系统调度器是通过M结合起来的，每个M都代表了1个内核线程，操作系统调度器负责把内核线程分配到CPU的核上执行。

单从线程调度讲，go语言相比其他语言的优势在于OS线程是由OS内核来调度的，==goroutine则是由go运行时（runtime）自己的调度器调度的，完全是在用户态下完成的，不涉及内核态与用户态之间的频繁切换，包括内存的分配与释放，都是在用户态维护着一块大的内存池，不直接调用系统的malloc函数（除非内存池需要改变），成本比调度OS线程低很多==。另一方面充分利用了多核的硬件资源，近似的把若干goroutine均分在物理线程上，在加上本身goroutine的超轻量级，以上种种特性保证了goroutine调度方面的性能。



### GOMAXPROCS

go运行时调度器使用`GOMAXPROCS`参数来确定需要使用多少个OS线程来同时执行go代码。默认值是机器上的CPU核心数。例如在一个8核心的机器上，GOMAXPROCS默认为8。Go语言可以通过`runtime.GOMAXPROCS`函数来设置当前程序并发时占用的CPU逻辑核心数。（go1.5版本前默认使用单核心执行，go1.5后版本默认使用全部的CPU逻辑核心数）



### goroutine退出机制

goroutine的调度是由 golang 运行时进行管理的，同一个程序中的所有 goroutine 共享同一个地址空间。goroutine退出机制是由goroutine自己退出的，不能在外部强制结束一个正在执行的goroutine，==只有在一种情况下正在运行的goroutine会因为其他goroutine结束而被终止，就是main函数退出或程序结束停止执行==



==goroutine退出方式==：

+ main函数结束退出
+ **通过向channel中发送信息，来通知结束goroutine结束**
+ **通过context来通知结束goroutine**
+ goroutine自己结束



### 了解

8核cpu：是指一个cpu里面封装了8个逻辑核心（计算单元），8个逻辑核心协同工作可以提高cpu处理能力。

16线程：本来一个逻辑核心只能跑一个线程，由于英特尔发明的超线程技术，一个逻辑核心可以跑两个线程



## channel

单纯地将函数并发执行是没有意义地，函数与函数之间需要交换数据才能体现并发执行函数地意义。

虽然可以使用共享内存方式进行数据交换（如：定义一个全局变量，多个goroutine去访问该变量），但是共享内存在不同地goroutine中容易发生竞态问题。为保证数据交换地正确性，很多并发模型中必须使用互斥量对内存加锁，但这种做法势必造成性能问题。

竞态问题：

​		用户短时间内重复地触发同一个动作产生多个异步请求，而由于请求的响应时延是不稳定的，可能会出现早发起的请求反而比晚发起的请求响应慢，导致界面呈现效果出现混乱、重复、覆盖等异常。



**go语言采用的并发模型是`CSP(Commuicating Sequential Processes)`，提倡通过==通信共享内存==而不是通过==共享内存而实现通信==。**



如果说goroutine是go程序并发的执行体，channel就是它们之间的连接。==channel是可以让一个goroutine发送特定值到另一个goroutine的通信机制。==



go语言中通道（channel）是一种特殊的类型。通道像一个传送带或者队列，总是遵循==先入先出==的规则，保证收发数据的顺序。每一个通道都是一个具体类型的导管，即声明通道channel时需要为其指定元素类型。



### channel类型

==channel是go语言中一种特有的类型**，也是一种引用类型**。==

```go
var 变量名称 chan 元素类型
//其中：
//chan：关键字
//元素类型：是指通道中传递元素的类型

//示例
var ch1 chan int //声明一个传递整型的通道
var ch2 chan bool //声明一个传递布尔类型的通道
var ch3 chan []int //声明一个传递int切片的通道

```





### channel零值

==未初始化的通道类型变量其默认值的零值为nil==

```go
var ch chan int
fmt.Println(ch) //<nil>
```



### 初始化channel

声明通道类型变量需要使用内置的`make`函数初始化之后才能使用。==**即channel初始化需要用make函数**==

```go
make(chan 数据类型，[缓冲区大小])
//channel缓冲区大小为可选

ch4 := make(chan int)
ch5 := make(chan bool,1) //声明一个缓冲区大小为1的缓冲区
```





###  channel操作

通道共有发送，接收，关闭（close）  三个操作。其中，发送和接收操作都使用`<-`符号

```go
ch := make(chan int)

//发送
ch <- 10 //把10发送到ch中

//接收
x := <- ch //从ch中接收值并赋值给变量x
<-ch //从ch中接收值，忽略结果

//关闭，通过close函数来关闭通道
Close(ch)

```



==注意：一个通道值是可以被垃圾回收掉的**。通道通常由发送方执行关闭操作，并且只有在接收方明确等待通道关闭的信号时才需要执行关闭操作**。它和关闭文件不一样，通常在结束操作之后关闭文件是必须做的，但关闭通道不是必须的，因为通道是可以被垃圾回收机制回收的。==

关闭后的通道有以下特点：

+ 对一个关闭的通道再发送值就会导致panic
+ **对一个关闭的通道进行接收会一直获取值直到通道为空**
+ **对一个关闭且没有值的通道执行接收操作，会得到对应类型的零值**
+ 关闭一个已经关闭的通道会导致panic



### 无缓冲的通道

无缓冲区的通道又被称为阻塞的通道。

```go
func main() {
	ch := make(chan int)
	ch <- 10
	fmt.Println("发送成功")
}
//程序报错
//fatal error: all goroutines are asleep - deadlock!
//goroutine 1 [chan send]:
//main.main()
//        .../main.go:8 +0x54

//程序出现deadlock（死锁）
```



==对于无缓冲区的通道只有在接收方能够接收值的时候才能发送成功，否则会一直等待发送的阶段。同理，如果对一个无缓冲区通道执行接收操作时，没有任何向通道中发送值的操作也会导致接收操作阻塞。即无缓冲区的通道必须至少有一个接收方才能发送成功。==



理解：无缓冲区通道即没有地方可以临时存放值，必须至少有一个人在旁边候着，一有值就立马接收，否则就会出现goroutine被挂起



==使用无缓冲区通道进行通信将导致发送和接收的goroutine同步化。因此，`无缓冲区通道`也被称为`同步通道`==



### 有缓冲的通道

有缓冲区的通道可以解决无缓冲区通道的死锁问题。在对通道声明时，可以使用make函数初始化通道并指定容量。

```go
func main(){
    ch := make(chan int,1) //创建一个容量为1的有缓冲区通道
    ch <- 10
    fmt.Println("发送成功")
}
```

==通道的容量表示通道最大能存放的元素数量。**当通道内已有元素数量达到最大容量后，再向通道内发送数据就会阻塞**，除非有从通道中接收数据。==

==**可以使用内置的`len`函数来获取通道内元素的数量，使用`cap`函数来获取通道的容量，一般用于有缓冲区的通道。**==



### 多返回值模式

==当向通道中发送完数据后，可以通过`close`函数来关闭通道。当一个通道被关闭后，再向通道内发送数据会引起panic。可以从关闭的通道中取值，如果通道的值被取完了，还继续从通道中取值，那么会得到对应类型的零值。==



如何判断通道关闭后，通道内是否还有值？

```go
//对一个通道执行接收操作时支持使用如下多返回值模式

value,ok := <-ch

//value:从通道中取出的值，如果通道被关闭且没有值了则返回对应类型的零值
//ok：用来判断通道内是否有值，即使通道关闭后，如果通道内还有发送方发送的值，那么ok为true，如果通道内没有发送方发送的值，那么ok为		false

//注意：对于通道关闭后，如果通道内还有值，那么ok为true，如果通道内值已取完，此时ok为false
//对于通道没有关闭，如果通道内有值，那么ok为true，如果通道没有值了，那么继续取值，goroutine会挂起，等待通道内有值
```





### for range 接收值

==通常我们**选择`for range`循环从通道中接收值，当通道被关闭后，会在通道内所有值接收完毕后自动退出循环**。==

```go
func f3(ch chan int){
    for v:=range ch{
        fmt.Println(v)
    }
}
```

==目前go语言中没有提供一种不对通道进行读取操作就能判断通道是否被关闭的方法。不能简单通过`len(ch)`操作来判断通道是否被关闭，因为这只能判断通道内没有值了==



### 单向通道

在某些场景下，可能会将通道作为参数在多个任务函数间进行传递，通常会选择不同任务函数中对通道的使用进行限制，比如限制通道在某个函数中只能执行发送或只能执行接收操作。

==上面定义的通道都是双向通道，即发送方和接收方都可以向通道中发送值和接收值。==



为保证发送者只能向通道中发送数据，接收者只能从通道中取数据，go语言提供了==单向通道==



```go
<- chan int //只接收通道，只能接收不能发送
chan <- int //只发送通道，只能发送不能接收
```

其中，箭头`<-`和关键字`chan`的相对位置表明了当前通道允许的操作，这种限制在编译阶段会进行检测。

==对一个只接收通道执行`close`也是不允许的，因为默认通道的关闭操作应该由发送方来完成。==

==在函数传参或赋值操作中，`全向通道`（正常通道）可以转换为`单向通道`，但是无法反向转换。==





***示例***：

```go
// Producer2 返回一个接收通道
func Producer2() <-chan int {
	ch := make(chan int, 2)
	// 创建一个新的goroutine执行发送数据的任务
	go func() {
		for i := 0; i < 10; i++ {
			if i%2 == 1 {
				ch <- i
			}
		}
		close(ch) // 任务完成后关闭通道
	}()

	return ch
}

// Consumer2 参数为接收通道
func Consumer2(ch <-chan int) int {
	sum := 0
	for v := range ch {
		sum += v
	}
	return sum
}

func main() {
	ch2 := Producer2()
  
	res2 := Consumer2(ch2)
	fmt.Println(res2) // 25
}
```





### 总结



+ 有缓冲区的通道：

![image-20221127194443937](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221127194443937.png)

+ 无缓冲区的通道：
  + 接收者和发送者必须都存在，因为没有内存可以临时存放元素，只能一手交钱一手交货。





## select多路复用

在某些情况下，可能需要同时从多个通道中接收数据。通道在接收数据时，如果没有数据可以被接收那么当前的goroutine将会发生阻塞。

第一种方式：

```go
for{
    // 尝试从ch1接收值
    data, ok := <-ch1
    // 尝试从ch2接收值
    data, ok := <-ch2
    …
}
//这种方式虽然可以实现从多个通道接收值，但是程序的运行性能会差很多。不建议采取这种方式
```



==第二种方式：==

==go语言内置`select`关键字，使用该关键字可以实现同时响应多个通道的操作。==

select使用方式类似switch语句，有一系列case分支和一个默认的分支。每个case分支对应一个通道的通信（发送或接收）过程。select会一直等待，直到其中的某个case的通信操作完成时，就会执行case分支对应的语句。

```go
select{
    case <-ch1:
    	//...
    case data:=<-ch2:
        //...
    case ch3<-10:
    	
    default:
    	//默认操作
}
//default语句可以有也可以没有，跟switch类似，但是select上没有表达式
//如果其他分支都没有准备好，那么就会执行default分支，为了非阻塞的发送或者接收，可使用 default 分支
//case分支后可以没有语句，即不进行任何操作
```



select支持default，如果select没有一条语句可以执行，即所有的通道都被阻塞，那么有两种可能的情况：

+ 如果给出了default语句，执行default语句，同时程序性的执行会从select语句后的语句中恢复
+ 如果没有default语句，那么select语句将被阻塞，直到至少有一个通信可以进行下去
+ 所以一般不写default语句



==***select语句具有以下特点***==

+ ==可以处理一个或多个channel的发送/接收操作==
+ ==如果多个case同时满足，select会随机选择一个执行==
+ 对于没有case的select会一直阻塞，可用于阻塞main函数，防止退出。

+ **select多路复用一般结合for循环（一般时for无限循环）一起使用**。



示例：

```go
package main

import "fmt"

func main(){
    ch := make(chan int,1)
    for i:=0;i<10;i++{
        select{
            case x:=<-ch:
            	fmt.Println(x)
            case ch<-i：
        }
    }
}
//从上述代码中可以看出，select支持对一个通道或多个通道进行发送或者接收操作
```



==**select机制：**==

+ select+case是用于阻塞监听goroutine的，**==如果没有case，就单单一个select{}，则为阻塞当前程序中的goroutine==，此时注意，需要有真实的goroutine在跑，否则select{}会报panic**

+ select底下有多个可执行的case，则随机执行一个。

+ select常配合for循环来监听channel有没有故事发生。需要注意的是在这个场景下，break只是退出当前select而不会退出for，需要用break 标签 / goto 标签  的方式。

+ 无缓冲的通道，则传值后立马close，则会在close之前阻塞，有缓冲的通道则即使close了也会继续让接收后面的值

+ 同个通道多个goroutine进行关闭，可用recover panic的方式来判断通道关闭问题





## 通道误用示例

```go
//demo1 通道误用导致bug
func demo1(){
    wg := sync.WaitGroup{}
    
    ch := make(chan int,10)
    for i:=0;i<10;i++{
        ch<- i
    }
    close(ch)
    
    wg.Add(3)
    for j:=0;j<3;j++{
        go func(){
            for {
                task := <-ch
                //这里假设对接收的数据执行某些操作
                fmt.Println(task)
            }
            wg.Done()  
        }()
    }
    wg.Wait() 
}

//上述代码编译通过后，匿名函数所在的goroutine并不会按照预期在通道关闭后退出。因为task:=<-ch的接收操作在通道关闭后会一直接收到零值，而不会退出。此处的接收操作应该使用task,ok:=<-ch， 通过判断布尔值ok为false时退出；或者使用select来处理通道。
```



```go
//demo2 通道误用导致的bug
func demo2(){
    ch := make(chan string)
    go func(){
        //这里假设执行一些耗时操作
        time.Sleep(3*time.Second)
        ch <- "job result"
    }()
    
    select{
        case result:=<-ch:
        	fmt.Println(result)
        case <-time.After(time.Second): //较小的超时时间
        	return
    }
}
//上述代码可能导致goroutine泄露（goroutine并未按照预期退出并销毁）。由于select命中超时逻辑，导致通道没有消费者（无接收操作），而其定义的通道未无缓冲区的通道，因此goroutine中的ch<-"job result"操作会一直阻塞，最终导致goroutine泄露。
```







## 并发安全和锁

有时候，代码中可能存在多个goroutine同时操作一个资源（临界区）的情况，在这种情况下就会发生 ”竞态问题“ （数据竟态）。类似于：十字路口被各个方向的汽车竞争，火车上的卫生间被车厢里的人竞争。



数据竞争示例：

```go
package main

import(
	"fmt"
    "sync"
)

var (
	x int64
    wg sync.WaitGroup //等待组
)

//add 对全局变量x执行5000次加1操作
func add(){
    for i:=0;i<5000;i++{
        x = x+1
    }
    wg.Done()
}

func main(){
    wg.Add(2)
    go add()
    go add()
    wg.Wait()
    fmt.Println(x)
}
//从上面代码中可以看出，开启了两个goroutine分别执行add函数，这两个goroutine在访问和修改全局的x变量时就会存在数据竞争，某个goroutine中对全局变量x的修改可能会覆盖另一个goroutine中的操作，所以导致最后的结果与预期不符。
```



### 互斥锁

==互斥锁是一种常用的控制共享资源访问的方法，它能够保证同一时间内只有一个goroutine可以访问共享资源。==go语言中使用`sync`包中提供的`Mutex`类型来实现互斥锁。

`sync.Mutex`提供了两个方法：

| 方法名                  | 功能       |
| ----------------------- | ---------- |
| func (m *Mutex)Lock()   | 获取互斥锁 |
| func (m *Mutex)Unlock() | 释放互斥锁 |


```go
//在下面代码中，使用互斥锁限制每次只能一个goroutine才能修改全局变量x，从而修复上述问题
package main

import(
	"fmt"
    "sync"
)

//sync.Mutex

var (
	x int64
    wg sync.WaitGroup //等待组
    m sync.Mutex //互斥锁
)

//add 对全局变量x执行5000次操作
func add(){
    for i:=0;i<5000;i++{
        m.Lock() //修改x前加锁
        x=x+1
        m.Unlock() //修改完成后解锁
    }
    wg.Done()
}

func main(){
    wg.Add(2)

	go add()
	go add()

	wg.Wait()
	fmt.Println(x)
}
//此时，可以保证每一次都会得到预期的结果 --10000
```

==使用互斥锁能够保证同一时间内有且仅有一个goroutine进入临界区，其他的goroutine则等待锁；当互斥锁释放后，等待的goroutine才能获取锁进入临界区，多个goroutine同时等待一个锁时，唤醒的策略是随机的。==



### 读写互斥锁

互斥锁是完全互斥的，公共资源在同一时间内有且仅有一个goroutine能够访问，实际场景中存在读多写少，当我们并发的去读取一个资源而不涉及资源修改的时候是没有必要加互斥锁的，在这种场景下，使用读写锁是一种更好的选择。读写锁在go语言中使用`sync`包中的`RWMutex`类型。

`sync.RWMutex`提供了以下5种方法：

| 方法名                             | 功能                           |
| ---------------------------------- | ------------------------------ |
| func (rw *RWMutex)Lock()           | 获取写锁                       |
| func (rw *RWMutex)Unlock()         | 释放写锁                       |
| func (rw *RWMutex)RLock()          | 获取读锁                       |
| func (rw *RWMutex)RUnlock()        | 释放读锁                       |
| func (rw *RWMutex)RLocker() Locker | 返回一个实现Locker接口的读写锁 |



==**读写锁分为两种：读锁和写锁**==

+ 当一个goroutine获取到读锁后，其他的goroutine可以继续获取读锁，若是获取写锁则需等待。

+ 当一个goroutine获取写锁后，其他goroutine获取读锁、写锁都需要等待。





```go
//读多写少的场景，使用互斥锁和读写互斥锁来查看它们的性能
var (
	x int64
    wg sync.WaitGroup  //等待组
    mutex sync.Mutex //互斥锁
    rwMutex sync.RWMutex //读写互斥锁
)

//writeWithLock 使用互斥锁的写操作
func writeWithMutex(){
    mutex.Lock()  //加互斥锁
    x = x+1
    time.Sleep(10*time.Millisecond) //假设操作耗时
    mutex.Unlock()  //释放互斥锁
    wg.Done()
}

//readWithLock 使用互斥锁读操作
func readWithMutex(){
    mutex.Lock()  //加互斥锁
    time.Sleep(time.Millisecond) //假设读操作耗时1毫秒
    mutex.Unlock() //释放互斥锁
    wg.Done()
}

//writeWithRWLock 使用读写互斥锁的写操作
func writeWithRWLock(){
    rwMutex.Lock() //加写锁
    x = x+1
    time.Sleep(10*time.Millisecond) //假设读操作耗时10毫秒
    rwMutex.Unlock() //释放写锁
    wg.Done()
}

//readWithRWMutex 使用读写互斥锁的读操作
func readWithRWLock(){
    rwMutex.RLock() //加读锁
    time.Sleep(time.Millisecond) //假设读操作耗时1毫秒
    rwMutex.RUnlock() //释放读锁
    wg.Done()
}

func do(wf,rf func(),wc,rc int){
    start := time.Now()
    //wc个并发写操作
    for i:=0;i<wc;i++{
        wg.Add(1)
        go wf()
    }
    
    //rc个并发读操作
    for i:=0;i<rc;i++{
        wg.Add(1)
        go rf()
    }
    
    wg.Wait()
    cost := time.Since(start)  //返回从t到现在经过的时间
    fmt.Printf("x:%v cost:%v\n",x,cost)
}
```

我们假设每一次读操作都会耗时1ms，而每次写操作会耗时10ms，我们分别测试使用互斥锁和读写互斥锁执行10次并发写和1000次并发读的耗时数据：

```go
//使用互斥锁，10次并发写，1000次并发读
do(writeWithMutex,readWithMutex,10,1000) //x:10 cost:10.8608302s
//使用读写互斥锁，10次并发写，1000次并发读
do(writeWithRWLock,readWithRWLock,10,1000) //x:20 cost:160.2827ms
```

从结果种可以看出，使用读写互斥锁在读多写少的场景下能够极大地提高程序地性能。不过需要注意：如果一个程序种的读操作和写操作数量级差别不大，那么读写互斥锁的优势很难发挥出来。



### sync.WaitGroup

go语言中，可以使用`sync.WaitGroup`来实现并发任务的同步。

`sync.WaitGroup`有以下方法：

| 方法名                             | 功能              |
| ---------------------------------- | ----------------- |
| func (wg *WaitGroup)Add(delta int) | 计数器+delta      |
| func (wg *WaitGroup)Done()         | 计数器-1          |
| func (wg *WaitGroup)Wait()         | 阻塞直到计数器为0 |

`sync.WaitGroup`内部维护了一个计数器，计数器的值可以增加/减少。当我们启动N个任务时，就将计算器值增加N。每个任务完成时通过调用Done方法将计数器减1.通过调用Wait来等待并发任务执行完，当计数器值为0时，表示所有并发任务已经完成。

**==注意：`sync.WaitGroup`是一个结构体，是值类型，在进行参数传递时需要传递指针。==**



### sync.Once

在某些场景下，==需要保证某些操作即使在高并发场景下也只需执行一次==，例如只加载一次配置文件等。

 go语言中，`sync`包中提供了一个针对只执行一次场景的解决方案——`sync.Once`，`sync.Once`只有一个`Do`方法。

```go
func (o *Once)Do(f func())  
//相当于加了互斥锁和标签，如果其中某个goroutine已经执行f函数一次，此时将标签置为true，其他goroutine通过检查标签来判断f是否执行了，如果标签为true，表示已经执行了，那么将跳过该函数f，执行其他内容。
```

==注意：如果要执行的函数f需要传递参数就需要搭配闭包来使用==



#### 加载配置文件示例

延迟一个开销很大的初始化操作到真正用到它的时候再执行是一个很好的实践。因为预先初始化一个变量（比如：在init函数完成初始化）会增加程序启动的耗时，而且有可能实际执行过程中这个变量没有用上，那么这个初始化操作就不是必须要做的。

```go
var icons map[string]image.Image
func loadIcons(){
    icons = map[string]image.Image{
        "left":loadIcon("left.png"),
        "up":loadIcon("up.png")
        "right":loadIcon("right.png")
        "down":loadIcon("down.png")
    }
}

//Icon被多个goroutine调用时不是并发安全的
func Icon(name string)image.Image{
    if icons == nil{
        loadIcons()
    }
    return icons[name]
}
```

多个goroutine并发调用Icon函数时不是并发安全的，现代编译器和CPU可能会在保证每个goroutine都满足串行一致的基础上自由地访问内存地顺序。loadIcons函数可能会被重排为以下结果：

```go
func loadIcons(){
    icons = make(map[string]image.Image) //此时icons ！=nil
    icons["left"] = loadIcon("left.png")
    icons["up"] = loadIcon("up.png")
    icons["right"] = loadIcon("right.png")
    icons["down"] = loadIcon("down.png")
}
```

在这种情况下，会出现即使判断了icons不是nil也不意味着变量初始化完成了。考虑到这种情况，我们可以添加互斥锁，保证初始化icons的时候不会被其他的goroutine操作，但是这样又会引发性能问题。



使用`sync.Once`改造的示

```go
var icons map[string]image.Image

var loadIconsOnce sync.Once

func loadIcons() {
	icons = map[string]image.Image{
		"left":  loadIcon("left.png"),
		"up":    loadIcon("up.png"),
		"right": loadIcon("right.png"),
		"down":  loadIcon("down.png"),
	}
}

// Icon 是并发安全的
func Icon(name string) image.Image { 
	loadIconsOnce.Do(loadIcons)   //在多个goroutine中，允许其中一个goroutine执行一次
	return icons[name]
}
```





#### 并发安全的单例模式

```go
//借助sync.Once实现并发安全的单例模式
package main

import (
	"sync"
)

type singleton struct{}

var instance *singleton
var once sync.Once

func GetInstance()*singleton{
    once.Do(func(){
        instance = &singleton{}
    })
    return instance
}
//sync.Once 内部包含了一个互斥锁和一个布尔值，互斥锁保证布尔值和数据安全，而布尔值用来记录初始化是否完成。目的：保证初始化操作的时候是并发安全的并且初始化操作也不会被执行多次。
```





### sync.Map

==go语言内置的map不是并发安全的。==

示例：

```go
package  main
import(
	"fmt"
    "sync"
    "strconv"
)

var m = make(map[string]int)

func get(key string)int{
    return m[key]
}
func set(key string,value int){
    m[key] = value
}

func main(){
    wg := sync.WaitGroup{}
    for i:=0;i<10;i++{
        wg.Add(1)
        go func(n int){
            key := strconv.Itoa(n)
            set(key,n)
            fmt.Printf("k=%v,v=%v\n",key,get(key))
            wg.Done()
        }(i)
    }
    wg.Wait()
}

//代码编译执行后，报错fatal error: concurrent map writes，不能在多个goroutine中并发对内置的map进行读写操作，否则会存在数据竞争问题。
```



==最多只允许7个goroutine同时访问go语言内置的map，超过7个就会出现报错`fatal error: concurrent map writes`==



针对内置map不是并发安全的，此时需要加锁才能保证内置map是并发安全的。然而，go语言中`sync`包中提供了一种开箱即用的并发安全版map——`sync.Map`。==开箱即用表示其不用像内置map一样使用make函数初始化就能直接使用==。同时`sync.Map`内置了诸如`Store`、`Load`、`LoadOrStore`、`Delete`、`Range`等操作方法。

==其实：内置的map为引用类型，需要使用make开辟内存空间，而`sync.Map`为sync包中自定义的结构体，是值类型，因此不用使用make函数来开辟空间==

```go
//1, 向sync.Map类型的变量中，存储key-value数据
func (m *Map)Store(key,value interface{})

//2,查询key对应的value
func (m *Map)Load(key interface{})(value interface{},ok bool)

//3，先查询，如果存在，则返回已经存在的值和true，如果不存在，则返回新存进去的值和false
func (m *Map)LoadOrStore(key,value interface{})(actual interface{},loaded bool)

//4，先查询是否存在，若存在，则返回已经存在的值，并从Map中删除返回true，如果不存在，则返回 <nil> false
func (m *Map)LoadAndDelete(key interface{})(value interface{},loaded bool)

//5,删除key
func (m *Map)Delete(key interface{})

//6，对Map中的每个key-value依次调用f函数
func (m *Map)Range(f func(key,value interface{})bool)
```



`sync.Map`并发读写示例：

```go
package main

import(
	"fmt"
    "strconv"
    "sync"
)

//并发安全的map
var m = sync.Map{}

func main(){
    wg:=sync.WaitGroup{}
    //对m执行20个并发的读写操作
    for i:=0;i<20;i++{
        wg.Add(1)
        go func(n int){
            key:=strconv.Itoa(n)
            m.Store(key,n)
            value,_:=m.Load(key)
            fmt.Printf("k:=%v,v:=%v\n",key,value)
            wg.Done()
        }(i)
    }
    wg.Wait()
}
```





## 原子操作

针对整数数据类型（int32，uint32，int64，uint64）可以使用原子操作来保证并发安全，通常使用原子操作比使用锁操作效率更高。

go语言中的原子操作内置标准库`sync/atomic`提供。

### atomic包

| 方法                                                         | 功能           |
| ------------------------------------------------------------ | -------------- |
| func LoadInt32(addr *int32) (val int32)<br/>func LoadInt64(addr *int64) (val int64)<br/>func LoadUint32(addr *uint32) (val uint32)<br/>func LoadUint64(addr *uint64) (val uint64)<br/>func LoadUintptr(addr *uintptr) (val uintptr)<br/>func LoadPointer(addr *unsafe.Pointer) (val unsafe.Pointer) | 读取操作       |
| func StoreInt32(addr *int32, val int32)<br/>func StoreInt64(addr *int64, val int64)<br/>func StoreUint32(addr *uint32, val uint32)<br/>func StoreUint64(addr *uint64, val uint64)<br/>func StoreUintptr(addr *uintptr, val uintptr)<br/>func StorePointer(addr *unsafe.Pointer, val unsafe.Pointer) | 写入操作       |
| func AddInt32(addr *int32, delta int32) (new int32)<br/>func AddInt64(addr *int64, delta int64) (new int64)<br/>func AddUint32(addr *uint32, delta uint32) (new uint32)<br/>func AddUint64(addr *uint64, delta uint64) (new uint64)<br/>func AddUintptr(addr *uintptr, delta uintptr) (new uintptr) | 修改操作       |
| func SwapInt32(addr *int32, new int32) (old int32)<br/>func SwapInt64(addr *int64, new int64) (old int64)<br/>func SwapUint32(addr *uint32, new uint32) (old uint32)<br/>func SwapUint64(addr *uint64, new uint64) (old uint64)<br/>func SwapUintptr(addr *uintptr, new uintptr) (old uintptr)<br/>func SwapPointer(addr *unsafe.Pointer, new unsafe.Pointer) (old unsafe.Pointer) | 交换操作       |
| func CompareAndSwapInt32(addr *int32, old, new int32) (swapped bool)<br/>func CompareAndSwapInt64(addr *int64, old, new int64) (swapped bool)<br/>func CompareAndSwapUint32(addr *uint32, old, new uint32) (swapped bool)<br/>func CompareAndSwapUint64(addr *uint64, old, new uint64) (swapped bool)<br/>func CompareAndSwapUintptr(addr *uintptr, old, new uintptr) (swapped bool)<br/>func CompareAndSwapPointer(addr *unsafe.Pointer, old, new unsafe.Pointer) (swapped bool) | 比较并交换操作 |





### 示例

比较下互斥锁和原子操作的性能。

```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
	"time"
)

type Counter interface {
	Inc()
	Load() int64
}

// 普通版
type CommonCounter struct {
	counter int64
}

func (c CommonCounter) Inc() {
	c.counter++
}

func (c CommonCounter) Load() int64 {
	return c.counter
}

// 互斥锁版
type MutexCounter struct {
	counter int64
	lock    sync.Mutex
}

func (m *MutexCounter) Inc() {
	m.lock.Lock()
	defer m.lock.Unlock()
	m.counter++
}

func (m *MutexCounter) Load() int64 {
	m.lock.Lock()
	defer m.lock.Unlock()
	return m.counter
}

// 原子操作版
type AtomicCounter struct {
	counter int64
}

func (a *AtomicCounter) Inc() {
	atomic.AddInt64(&a.counter, 1)
}

func (a *AtomicCounter) Load() int64 {
	return atomic.LoadInt64(&a.counter)
}

func test(c Counter) {
	var wg sync.WaitGroup
	start := time.Now()
	for i := 0; i < 1000; i++ {
		wg.Add(1)
		go func() {
			c.Inc()
			wg.Done()
		}()
	}
	wg.Wait()
	end := time.Now()
	fmt.Println(c.Load(), end.Sub(start))
}

func main() {
	c1 := CommonCounter{} // 非并发安全
	test(c1)
	c2 := MutexCounter{} // 使用互斥锁实现并发安全
	test(&c2)
	c3 := AtomicCounter{} // 并发安全且比互斥锁效率更高
	test(&c3)
}
```



`atomic`包提供了底层的原子级内存操作，对于同步算法的实现很有用。这些函数必须谨慎地保证正确使用。除了某些特殊的底层应用，使用通道或者 sync 包的函数/类型实现同步更好。





# 处理并发错误

在go语言中，通过开启goroutine可以执行并发任务，如何有效处理并发过程中的错误则是一个棘手的问题。



## recover gorourtine中的panic

==通常，我们在代码中使用recover来恢复程序中的panic，而panic只会触发当前goroutine中的defer操作。==



示例：无法在main函数中recover另一个goroutine中引发的panic

```go
func f1(){
    defer func(){
        if e:=recover();e!=nil{
            fmt.Printf("recover panic:%v\n",e)
        }
    }()
    
    //开启一个goroutine执行任务
    go func(){
        fmt.Println("in goroutine...")
        //只能触发当前goroutine中的defer
        panic("panic in goroutine")
    }()
    time.Sleep(time.Second)
    fmt.Println("exit")
    
}

//D:\code\GOPATH_code\src\github.com\ghost\fuxi\01>go run main.go
//in goroutine...
//panic: panic in goroutine

//goroutine 19 [running]:
//main.f1.func2()
//        D:/code/GOPATH_code/src/github.com/ghost/fuxi/01/main.go:20 +0x65
//created by main.f1
//        D:/code/GOPATH_code/src/github.com/ghost/fuxi/01/main.go:17 +0x48
//exit status 2


从输出结果中可以看到程序没有正常退出，而是由于panic异常退出了
```

在启用goroutine去执行任务的场景下，如果想要recover goroutine中可能出现的panic就需要在goroutine中使用recover。

```go
func f2(){
    defer func(){
        if r:=recover();r!=nil{
            fmt.Println("recover outer panic:%v\n",r)
        }
    }()
    //开启一个goroutine执行任务
    go func(){
        defer func(){
            if r:=recover();r!=nil{
                fmt.Printf("recover inner panic:%v\n",r)
            }
        }()
        fmt.Println("in goroutine...")
        panic("panic in goroutine")
    }()
    time.Sleep(time.Second)
    fmt.Println("exit")
}
//in goroutine....
//recover inner panic:panic in goroutine
//exit
 程序中的panic被recover成功捕获，程序正常退出。
```



**==如果某个goroutine出现panic，那么在该goroutine中必须包含`defer func(){recover()}`才能捕获该panic，不能捕获其他goroutine中的panic==**



## errgroup

在以往演示的并发示例中，通常如下代码中的那样，直接在go关键字后调用一个函数或匿名函数。

```go
go func(){
    ...
}

go f()
```

这种代码示例我们默认被并发的函数都不会返回错误，但真实情况并不是这样。



当我们将一个任务拆分为多个子任务交给多个goroutine去运行，此时应该如何获取到子任务可能返回的错误尼？

示例：

```go
//假设我们有多个网址并发去获取它们的内容。

//fetchUrlDemo 并发获取url内容
func fetchUrlDemo(){
    wg := sync.WaitGroup{}
    var urls = []string{
        "http://pkg.go.dev",
        "http://www.liwenzhou.com",
        "http//www.yixiqitawangzhi.com"
    }
    
    for _,url:=range urls{
        wg.Add(1)
        go func (url string){
            defer wg.Done()
            resp,err:=http.Get(url)
            if err!=nil{
                fmt.Printf("获取%s成功\n",url)
                resp.Body.Close()
            }
            return //如何将错误返回呢？
        }(urls)
    }
    wg.Wait()
    //如何获取goroutine中可能出现的错误？
}

//在上面代码中，开启了3个goroutine分别去执行获取3个url的内容。类似这种将任务分为若干个子任务的场景很多，如何获取子任务中可能出现的错误呢？
```



==在实际项目中，子任务goroutine的执行并不是顺风顺水，也许会产生error，而WaitGroup并没有告诉我们在子任务goroutine发生错误时，如何将该错误抛给主任务goroutine。==



errgroup包可以解决这类问题，它能为处理公共任务的子任务而开启的一组goroutine提供同步、error传播和基于context的取消功能。

errgroup包定义的Group类型，包含了若干个不可导出的字段。

```go
type Group struct{
    cancel func()
    
    wg sync.WaitGroup
    
    errOnce sync.Once
    err error
}

errgroup.Group提供了Go和Wait两个方法:

func (g *Group)Go(f func()error)
//Go函数会在新的goroutine中调用传入的函数f
//注意：该方法其实可以看作：go f()error{}()   ，实际上就是启动了一个可以返回goroutine错误的go协程
//第一个返回非nil错误的调用将取消该Group；下面的Wait方法会返回该错误

func (g *Group)Wait()error
//Wait会阻塞直至由上述Go方法调用的所有函数都返回，然后从它们返回第一个非nil的错误（如果有）。
```





**示例：如何使用errgroup包来处理多个子任务goroutine中可能返回的error。**

```go
//fetchUrlDemo2使用errgroup并发获取url内容
func fetchUrlDemo2()error{
    g := new(errgroup.Group) //创建等待组（类似sync.WaitGroup）
    var urls = []string{
        "http://pkg.go.dev",
		"http://www.liwenzhou.com",
		"http://www.yixieqitawangzhi.com",
    }
    for _,url:=range urls{
        url := url //注意此处声明新的变量
        //启动一个goroutine去获取url内容
        g.Go(func()error{
            resp,err := http.Get(url)
            if err == nil{
                fmt.Printf("获取%s成功\n",url)
                resp.Body.Close()
            }
            return err //返回错误
        })
    }
    if err:=g.Wait();err!=nil{  //这里会阻塞，直到所有goroutine都成功执行，或某个goroutine出现错误
        //处理可能出现的错误
        fmt.Println(err)
        return err
    }
    fmt.Println("所有goroutine均成功")
    return nil
}

//获取http://pkg.go.dev成功
//获取http://www.liwenzhou.com成功
//Get "http://www.yixieqitawangzhi.com": dial tcp: lookup www.yixieqitawangzhi.com: no such host

当子任务的goroutine中对http://www.yixieqitawangzhi.com发起http请求时会返回一个错误，这个错误会由errgroup.Group的Wait方法返回。
```







可以看到当任意一个函数f返回错误时，会通过`g.errOnce.Do`只将第一个返回的错误记录，并且如果存在cancel方法则会调用cancel。

```go
func (g *Group)Go(f func()error){
    g.wg.Add(1)
    
    go func(){
        defer g.Done()
        if err:=f();err!=nil{
            g.errOnce.Do(func()){
                g.err=err
                if g.cancel!=nil{
                    g.cancel()
                }
            }
        }
    }()
}
```



如何创建带有cancel方法的errgroup.Group尼？

​	可以通过errgroup包提供的WithContext函数。

```go
func WithContext(ctx context.Context)(*Group,context.Context)
//WithContext函数接收一个父context，返回一个新的Group对象和一个关联的子context对象。
```



示例：

```go
package main

import(
	"context"
    "crypto/md5"
    "fmt"
    "log"
    "os"
    "path/filepath"
    "golang.org/x/sync/errgroup"
)
```









# 网络编程

互联网的核心是一系列协议，总称为“互联网协议“（Internet Protocal Suite），这些协议规定了电脑如何连接和组网。

## 互联网分层模型

![image-20221129164815490](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221129164815490.png)



### 物理层

电脑与外界互联网通信，需要先将电脑连接网络，常用方式：双绞线、光纤、无线电波等方式。即就是将电脑连接起来的物理手段，主要规定了网络的一些电气特性，负责传送0和1的电信号。



### 数据链路层

规定了解读电信号的方式。

以太网规定：一组电信号构成一个数据包，叫做”帧“，每一帧分成两个部分：标头（Head）和数据（Data）。其中 ”标头“包含数据包的一些说明项，比如发送者、接收者、数据类型等；”数据“则是数据包的具体内容。”标头“的固定长度为18个字节，”数据“的长度：最长为1500字节，最短46字节；因此，整个帧最短为64个字节，最长为1518个字节。如果数据很长，则必须分割成多个帧进行发送。

发送者和接收者是如何识别的？==以太网规定，连入网络的所有设备必须具有”网卡“接口。数据包必须从一块网卡，传送到另一块网卡。====网卡的地址，就是数据包发送地址和接收地址，即MAC地址==。每块网卡出厂都有一个全世界独一无二的MAC地址，长度为48个二进制位，通常用12个十六进制数表示。前6个十六进制数是厂商编号，后6个是该厂商的网卡流水号。有了MAC地址，就可以定位网卡和数据包的路径了。

我们通过ARP协议来获取接收方的MAC地址，有了MAC地址，如何将数据准确发送到接收方？以太网采用了一种”原始“的方式，它不是把数据包准确发送到接收方，而是发送给网络中的所有计算机，让每台计算机读取这个包的”标头“，找到接收方的MAC地址，然后与自身的MAC地址相比较，如果二者相同，就接收这个包，否则就丢弃这个包。这种发送方式称为“广播”。



### 网络层

以太网协议可以依靠MAC地址向外发送数据。但是这种方式局限在两台计算机处于同一个子网络层，在不同的网络层是广播不出去的。

因此，必须找到一种方法区分哪些MAC地址属于同一个子网络。如果是同一个子网络采用广播方式发送，否则采用”路由“方式发送。故产生了”网络层“，主要作用为：==引进一套新的地址，使得我们能够区分不同的计算机是否属于同一个子网络==。这套地址被称为”网络地址“，即网址。

“网络层”出现后，每台计算机有两种地址，一种MAC地址，另一种是网络地址。两个地址无联系，MAC地址是绑定在网卡上的，网络地址则是网络管理员分配的。网络地址帮助我们确定计算机所在的子网络，MAC地址则将数据包送到子网络中的目标网卡。因此，**数据传输：先处理网络地址，再处理MAC地址。**

规定网络地址协议为IP协议，因此该协议定义的地址又称为”IP“地址。目前，广泛采用IP协议第四版，即IPv4。该网络地址由32个二进制位组成，通常分成四段的十进制数表示IP地址，从0.0.0.0到255.255.255.255.

根据IP协议发送的数据被称为IP数据包。IP数据包也分为”标头“和”数据“：”标头“包括版本，长度，ip地址等信息，”数据“包括ip数据包的具体内容。IP数据包的”标头“部分长度位20到60字节，整个数据包的总长度最大为65535字节。



### 传输层

有了MAC和IP地址后，可以在互联网上任意两台主机上建立通信。但同一台主机上会有许多程序都需要用网络收发数据。如何区分某个数据包是哪个程序的？此时，==引入”端口“：表示：每一个使用网卡的程序编号。每个数据包都发到主机的特定端口，所以不同的程序就能取到自己需要的数据。==

”端口“是从0到65535之间一个整数，正好16个二进制位。其中0到1023的端口被系统占用，用户只能选用大于1023的端口。有了IP和端口就能实现唯一确定互联网程序上的一个程序，进而实现网络间的程序通信。

必须在数据包中加入端口信息，故需要新的协议。最简单的实现叫做UDP协议，它的格式几乎就是在数据前面加上端口号。UDP数据包也是由”标头“和”数据“两部分组成，其中，标头：定义了发出端口和接收端口，数据：具体内容。UDP数据包简单，“标头”部分一共只有8个字节，总长度不超过65535字节，正好放进一个IP数据包。

UDP协议简单易是实现，但是可靠性较差，一旦数据发出，无法知道对方是否收到。为了解决这个问题，提高网络可靠性，诞生了TCP协议。TCP协议能够保证数据不会丢失，但是过程复杂、实现困难、消耗较多的资源。TCP数据包没有长度限制，理论上可以无限长，为了保证网络的效率，通常TCP数据包长度不会超过IP数据包长度，以确保单个TCP数据包不再分割。



### 应用层

应用程序收到“传输层”的数据，需要对数据进行解包。由于数据来源复杂，需要实现规定好通信的数据格式，否则接收方根本无法获得真正发送的数据内容。==“应用层”的作用就是规定应用程序使用的数据格式==，如TCP协议之上常见的Email、HTTP、FTP等协议，这些协议组成了互联网协议的应用层。

如下图示，发送方的HTTP数据经过互联网的传输过程中会依次添加各层协议的标头信息，接收方接收到数据包后再依次根据协议解包得到数据。

![image-20221129181012085](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221129181012085.png)





## socket编程

Socket是BSD UNIX的进程通信机制，**通常也被称为”套接字“，用于描述IP地址和端口**，是一个通信链的句柄。Socket可以理解为TCP/IP网络的API，它定义了许多函数或例程，程序员可以用它们来开发TCP/IP网络上的应用程序。电脑运行的应用程序通常通过”套接字“向网络发出请求或应答网络请求。



### socket图解

`Socket`是应用层与TCP/IP协议族通信的中间软件抽象层。在设计模式中，`Socket`其实就是一个门面模式，它将复杂的TCP/IP协议族隐藏在`Socket`后面，对用户来说只需要调用`Socket`规定的相关函数，让`Socket`去组织符合指定的协议数据然后进行通信。

![image-20221129193920896](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221129193920896.png)

### go语言实现tcp通信

#### tcp协议

TCP/IP为传输控制协议/网间协议，==是一种面向连接的、可靠的、基于字节流的传输层通信协议，因为面向连接的协议，数据向流水一样传输，会存在黏包问题==。



#### tcp服务端

一个TCP服务端可以同时连接多个客户端，如世界各地用户可以使用自己电脑上的浏览器访问淘宝网。go语言中创建多个goroutine实现并发非常高效，所以可以每建立一个连接就创建一个goroutine去处理。

TCP服务端程序处理流程：

+ 监听端口
+ 接收客户端请求建立链接
+ 创建goroutine处理链接

go语言net包实现了TCP服务端代码：

```go 
package main

import (
	"bufio"
	"fmt"
	"io"
	"net"
	"os"
	"strings"
)

func process(conn net.Conn) {
	defer conn.Close()  //记得关闭连接
	reader := bufio.NewReader(os.Stdin)
	for {
		//接收来客户端的数据
        var buf [128]byte
		n, err := conn.Read(buf[:])
		if err == io.EOF {
			fmt.Println("收到client端发来的数据：", string(buf[:n]))
			return
		}
		if err != nil {
			fmt.Println("read from client failed,err:", err)
			return
		}
		fmt.Println("收到client端发来的数据：", string(buf[:n]))

		//从服务端向客户端发送数据
		str,err:=reader.ReadString('\n')
		if err!=nil {
			fmt.Println("read from stdin failed,err:",err)
			return
		}
		str=strings.TrimSpace(str)
		if str=="exit" {
			break
		}
		conn.Write([]byte(str))
	}
}

func main() {
	//本地端口
	listen, err := net.Listen("tcp", "127.0.0.1:20000")
	if err != nil {
		fmt.Println("listen failed,err:", err)
		return
	}
	//建立链接
	for {
		conn, err := listen.Accept()
		if err != nil {
			fmt.Println("accept failed,err:", err)
			continue
		}
		go process(conn)
	}

}

```





#### tcp客户端

一个TCP客户端进行TCP通信的流程：

+ 建立与服务端的链接
+ 进行数据收发
+ 关闭链接

go语言的net包实现了TCP客户端代码：

```go
package main

import (
	"bufio"
	"fmt"
	"net"
	"os"
	"strings"
)

//TCP client

func main() {
	conn, err := net.Dial("tcp", "127.0.0.1:20000")
	if err != nil {
		fmt.Println("dial failed,err:", err)
		return
	}
	defer conn.Close()
	inputReader := bufio.NewReader(os.Stdin)
	for {
		input, _ := inputReader.ReadString('\n')
		inputInfo := strings.TrimSpace(input)
		if strings.ToUpper(inputInfo) == "exit" {
			//如果输入exit就退出
			return
		}
		_, err = conn.Write([]byte(inputInfo)) //发送数据
		if err != nil {
			fmt.Println("write connn failed,err:", err)
			return
		}
		buf := [512]byte{}
		n, err := conn.Read(buf[:])
		if err != nil {
			fmt.Println("recv failed,err:", err)
		 	return
		 }
		 fmt.Println("从客户端收到数据：",string(buf[:n]))
	}

}

```





### tcp黏包

#### 黏包示例

```go
//服务端代码：

package main

import (
	"fmt"
	"io"
	"net"
)

func process(conn net.Conn) {
	defer conn.Close()
	var buf [1024]byte
	for {
		n, err := conn.Read(buf[:])
		if err == io.EOF {
			fmt.Println("收到客户端数据：",string(buf[:n]))
			break
		}
		if err != nil {
			fmt.Println("read from conn failed,err:", err)
			return
		}
		fmt.Println(string(buf[:n]))
	}
}
func main(){
	listen,err:=net.Listen("tcp","127.0.0.1:20000")
	if err!=nil{
		fmt.Println("listen failed,err:",err)
		return
	}
	defer listen.Close()

	for{
		conn,err:=listen.Accept()
		if err!=nil {
			fmt.Println("accept failed,err:",err)
			continue
		}
		go process(conn)
	}
}


```

```go
//客户端代码：
package main

import (
	"fmt"
	"net"
)

func main(){
	conn,err:=net.Dial("tcp","127.0.0.1:20000")
	if err!=nil{
		fmt.Println("dial failed,err:",err)
		return
	}
	defer conn.Close()
	for i:=0;i<20;i++{
		msg:=`hello,Hello,how are you`
		conn.Write([]byte(msg))
	}
}

```

```go
//先启动服务端，再启动客户端，可以在服务端输出如下结果：
收到client发来的数据： Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?
收到client发来的数据： Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?
收到client发来的数据： Hello, Hello. How are you?Hello, Hello. How are you?
收到client发来的数据： Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?
收到client发来的数据： Hello, Hello. How are you?Hello, Hello. How are you?

//可以看出，客户端分20次发送数据，然而，在客户端上，并没有输出20次，而是多条数据黏到一起
```



#### 为什么出现黏包

==主要原因：tcp数据传输的传递模式为：流模式，在保持长连接的时候，可以进行多次收发==

==“黏包”可发生在发送端也可发生在接收端：==

+ ==由Nagle算法造成的发送端黏包：==Nagle算法是一种改善网络传输效率的算法。简答来说：当我们提交一段数据给TCP发送时，TCP并不立刻发送此段数据，而是等待一小段时间看看等待期内是否还有要发送的数据，若是有，则一次把两段数据发送出去。
+ ==接收端接收不及时造成的接收端黏包：==TCP会把接收到的数据存在自己的缓冲区内，然后通知应用层取数据。当应用层由于某些原因不能及时把TCP数据取出来，就会造成TCP缓冲区内存放了几段数据。



#### 如何解决黏包

==出现黏包的关键在于接收方不确定将要传输的数据包大小，因此可以对数据包进行封包和拆包操作。==

“封包”：就是给一段数据加上包头，这样一来，数据包就分为包头和包体（过滤非法包时封包会加入“包尾”内容）。包头部分长度固定，存储包体的长度，根据包头长度固定以及包头中含有包体长度的变量就能正确拆分出一个完整的数据包。



```go
//如可以定义一个协议，如数据包前四个字节为包头，里面存储发送的数据长度
package proto

import (
	"bufio"
	"bytes"
	"encoding/binary"
)

//将消息编码
func Encode(message string)([]byte,error){
	//读取消息的长度，转换为int32类型（占4个字节）
	var length = int32(len(message))
	var pkg = new(bytes.Buffer)

	//写入消息头
	err:=binary.Write(pkg,binary.LittleEndian,length)
	if err!=nil{
		return nil,err
	}
	//写入消息实体
	err=binary.Write(pkg,binary.LittleEndian,[]byte(message))
	if err!=nil {
		return nil,err
	}
	return pkg.Bytes(),nil
}

//Decode 解码
func Decode(reader *bufio.Reader)(string,error){
	//读取消息长度
	lengthByte,_:=reader.Peek(4)//读取前4个字节
	lengthBuff:=bytes.NewBuffer(lengthByte)
	var length int32
	err:=binary.Read(lengthBuff,binary.LittleEndian,&length)
	if err!=nil{
		return "",nil
	}
	//Buffer返回缓冲区中现有的可读取的字节数
	if int32(reader.Buffered())<length+4{
		return "",err
	}
	//读取真正的消息
	pack:=make([]byte,int(4+length))
	_,err=reader.Read(pack)
	if  err!=nil {
		return "",nil
	}
	return string(pack[4:]),nil

}
//在服务端和客户端分别使用上面定义的proto包的Decode和Encode函数处理数据。
```



```go
//服务端代码：

func process(conn net.Conn) {
	defer conn.Close()
	reader := bufio.NewReader(conn)
	for {
		msg, err := proto.Decode(reader)
		if err == io.EOF {
			return
		}
		if err != nil {
			fmt.Println("decode msg failed, err:", err)
			return
		}
		fmt.Println("收到client发来的数据：", msg)
	}
}

func main() {

	listen, err := net.Listen("tcp", "127.0.0.1:30000")
	if err != nil {
		fmt.Println("listen failed, err:", err)
		return
	}
	defer listen.Close()
	for {
		conn, err := listen.Accept()
		if err != nil {
			fmt.Println("accept failed, err:", err)
			continue
		}
		go process(conn)
	}
}
```



```go
//客户端代码：

func main() {
	conn, err := net.Dial("tcp", "127.0.0.1:30000")
	if err != nil {
		fmt.Println("dial failed, err", err)
		return
	}
	defer conn.Close()
	for i := 0; i < 20; i++ {
		msg := `Hello, Hello. How are you?`
		data, err := proto.Encode(msg)
		if err != nil {
			fmt.Println("encode msg failed, err:", err)
			return
		}
		conn.Write(data)
	}
}
```



### go语言实现udp通信

#### udp协议

==UDP协议又被称为数据报协议==，是OSI（Open System Interconnection，开放式系统互联）参考模型中==一种无连接的传输协议，不需要建立连接就能直接进行数据发送和接收，属于不可靠的、没有时序的通信，但是UDP协议的实时性比较好，通常用于视频直播相关领域。==

#### udp服务端

```go
//udp服务端代码：

package main

import (
	"fmt"
	"net"
)

func main() {
	listen, err := net.ListenUDP("udp", &net.UDPAddr{IP: net.IPv4(127, 0, 0, 1), Port: 30000})
	if err != nil {
		fmt.Println("listen failed,err:", err)
		return
	}
	defer listen.Close()
	for {
		var data [1024]byte
        //不需要建立连接
		n, addr, err := listen.ReadFromUDP(data[:]) //接收数据
		if err != nil {
			fmt.Println("read udp failed,err:", err)
			continue
		}
		fmt.Printf("data:%v,addr:%v,count:%v\n", string(data[:n]), addr, n) //addr 为数据发送方的地址
		_, err = listen.WriteToUDP(data[:n], addr)                          //发送数据
		if err != nil {
			fmt.Println("write to udp failed,err:", err)
			continue
		}
	}
}
```



#### udp客户端

```go
//udp客户端代码：

package main

import (
	"fmt"
	"net"
)

func main() {
	udpConn, err := net.DialUDP("udp", nil, &net.UDPAddr{IP: net.IPv4(127, 0, 0, 1), Port: 30000})
	if err!=nil {
		fmt.Println("dial failed,err:",err)
		return 
	}
	defer udpConn.Close()
	sendData:=[]byte("hello world")
	_,err=udpConn.Write(sendData)
	if err!=nil {
		fmt.Println("发送数据失败,err:",err)
		return
	}
	data := make([]byte,4096)
	n,remoteAddr,err:=udpConn.ReadFromUDP(data)
	if err!=nil {
		fmt.Println("接收数据失败,err:",err)
		return
	}
	fmt.Printf("recv:%v,addr:%v,count:%v\n",string(data[:n]),remoteAddr,n)
}
```





# net/http包

go语言内置的net/http包提供了HTTP客户端和服务端的实现。



## HTTP协议

超文本传输协议（HTTP，HyperText Transfer Protocol）是互联网上应用广泛的一种网络传输协议，所有的WWW文件都是必须遵循该标准。HTTP最初目的：提供一种发布和接收HTML页面的方法。

==HTTP协议是基于TCP/IP协议的关于数据如何在万维网中通信的协议。**即HTTP的底层是TCP/IP**。**HTTP协议对应于应用层**，**TCP协议对应于传输层**。==



## HTTP客户端

### 基本HTTP/HTTPS请求

==Get、Head、Post和PostForm函数发出HTTP/HTTPS请求。==

```go
resp,err:=http.Get("http://example.com/")
...
resp,err:=http.Post("http://example.com/upload","image/jpeg",&buf)
...
resp,err:=http.PostForm("http://example.com/form",url.Values{"key":{"value"},"id":{"123"}})
```

程序在使用完response后必须关闭回复的主题。

```go
resp,err:=http.Get("http://example.com/")
if err!=nil{
    //handle error
}
defer resp.Body.Close()
body,err:=ioutil.ReadAll(resp.Body)
```



### 不带参数的GET请求示例

```go
//简单的发送HTTP请求的client端

package main

import(
	"fmt"
    "net/http"
    "io/ioutil"
)
func main(){
    resp,err:=http.Get("https://www.liwenzhou.com/")
    if err!=nil{
        fmt.Printf("get failed,err:%v\n",err)
        return
    }
    defer resp.Body.Close()
    body,err:=ioutil.ReadAll(resp.Body)
    if err!=nil{
        fmt.Printf("read from resp.Body failed,err:%v\n",err)
        return 
    }
    fmt.Print(string(body))
}
//执行之后，可以在终端打印www.liwenzhou.com网站首先的内容。
//浏览器其实就是一个发送和接收HTTP协议数据的客户端，平时浏览器访问网页就是从网站的服务器接收HTTP数据，然后浏览器按照HTML、CSS等规则将网页渲染展示
```



### 带参数的GET请求示例

Get请求的参数需要使用go语言内置的`net/url`这个库来处理。

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
	"net/url"
)

func main() {
	apiUrl := "http://127.0.0.1:9090/get"
	//URL param
	data := url.Values{}
	data.Set("name","小王子")
	data.Set("age","18")
	//将原始url解析为一个URL结构体
	u,err:=url.ParseRequestURI(apiUrl)
	if err!=nil {
		fmt.Printf("parse url requestUrl failed,err:%v\n",err)
		return
	}
	//将参数编码赋值到结构体中的RawQuery字段
	u.RawQuery=data.Encode()
	//将URL重新组合为有效的URL字符串
	fmt.Println(u.String())
	resp,err:=http.Get(u.String())
	if err!=nil{
		fmt.Printf("post failed,err:%v\n",err)
		return
	}
	defer resp.Body.Close()
	b,err:=ioutil.ReadAll(resp.Body)
	if err!=nil{
		fmt.Printf("get resp failed,err:%v\n",err)
		return
	}
	fmt.Println(string(b))
}
```

==对应server端HandlerFunc如下：==

```go
func getHandler(w http.ResponseWriter,r *http.Request){
    defer r.Body.Close()
    data := r.URL.Query()
    fmt.Println(data.Get("name"))
    fmt.Println(data.Get("age"))
    answer:=`{"status":"ok"}`
    w.Write([]byte(answer))
}
```



### Post请求示例

使用`net/http`包发送`Post`请求示例：

```go
package main

import (
	// "bytes"
	"fmt"
	"io/ioutil"
	"net/http"
	"strings"
)

func main() {
	url := "http://127.0.0.1:9090/post"
	//表单数据
	//contentType:="application/x-www-form-urlencoded"
	//data:="name=小王子&age=18"

	//json
	contentType := "application/json"
	data := `{"Name":"小王子","Age":18}`
    
    
	//strings.NewReader(s string)*Reader //创建一个从s读取数据的Reader。本函数类似bytes.NewBufferString,但是更高效，且为只读。
	resp, err := http.Post(url,contentType,strings.NewReader(data))
	// resp,err:=http.Post(url,contentType,bytes.NewReader([]byte(data)))
    
    
	if err!=nil{
		fmt.Printf("post failed,err:%v\n",err)
		return
	}
	defer resp.Body.Close()
	b,err:=ioutil.ReadAll(resp.Body)
	if err!=nil{
		fmt.Printf("get resp failed,err:%v\n",err)
		return
	}
	fmt.Println(string(b))
}
```

==对应的Server端HandlerFunc如下：==

```go
func postHandler(w http.ResponseWriter,r *http.Request){
    defer r.Body.Close()
    //1,请求类型是application/x-www-form-urlencoded时解析form数据
    r.ParseForm()
    fmt.Println(r.PostForm)  //打印form数据
    fmt.Println(r.PostForm.Get("name"),r.PostForm.Get("age"))
    //2,请求类型为application/json时从r.Body读取数据
    b,err:=ioutil.ReadAll(r.Body)  //此时，b是序列化的数据，需要反序列化才行
    if err!=nil{
        fmt.Printf("read request.Body failed,err:%v\n",err)
        return
    }
    
    var stu struct {
		Name string
		Age  int
	}
	json.Unmarshal(b, &stu)   //json序列化
    
    answer:=`{"status":"ok"}`
    w.Write([]byte(answer))
}
```



### 自定义Client

要管理HTTP客户端的头域、重定向策略和其他设置，创建一个Client

```go
client:=&http.Client{
    checkRedirect:redirectPolicyFunc,
}
resp,err:client.Get("http://example.com")
//...
req,err:=http.NewRequest("GET","http://example.com",nil)
//...
req.Header.Add("If-None-Match",`W/"wyzzy"`)
resp,err:=client.Do(req)
//...
```



### 自定义Transport

要管理代理、TLS配置、keep-alive、压缩和其他设置，创建一个Transport：

```go
tr:=&http.Transport{
    TLSClientConfig:&tls.Config{RootCAs:poot},
    DisableCompression:true,
}
client:=&http.Client{Transport:tr}
resp,err:=client.Get("https://example.com")
```

==Client和Transport类型都可以安全的被多个goroutine同时使用。处于效率考虑，应一次建立、尽量重复使用。==



## 服务端

![image-20221201153917516](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221201153917516.png)

### 默认的Server

ListenAndServe使用指定的监听地址和处理器启动一个HTTP服务端。处理器参数通常为nil，表示采用包变量DefultServeMux作为处理器。

Handle和HandleFunc函数可以向DefaultServeMux添加处理器。

```go
//第一个参数为请求路径，第二个参数为接口类型
func Handle(pattern string,handler Handler){
    DefualtServeMux.Handle(pattern,handler)
}
type Handler interface{
    ServeHTTP(ResponseWriter,*Request)
}
//如果使用Handle方法，需要自定义一个结构来实现该接口
//如：
type httpServer struct{}
func(h httpServer)ServeHTTP(w http.ResponseWriter,r *http.Request){
    w.Write([]byte("hello world!"))
}
//因此采用第一种处理函数，需要定义一个结构体来实现该接口，即我们需要实现ServeHTTP方法


//第二种方法：第一个参数为请求路径，第二个参数为函数，同样需要定义一个该类型的函数
func HandleFunc(pattern string,handler func(ResponseWriter,*Request)){
    DefaultServeMux.HandleFunc(pattern,handler)
}






//"/foo"表示请求路径，fooHandler表示：对该路径处理函数
http.Handle("/foo",fooHandler)
http.HandFunc("/bar",func(w http.ResponseWriter,r *http.Rquest){
    fmt.Fprintf(w,"Hello,%q",html.EscapeString(r.URL.Path))
})
log.Fatal(http.ListenAndServe(":8080",nil))
```



### 默认的Server示例

使用go语言中的`net/http`包编写一个简单的接收HTTP请求的Serve端示例，`net/http`包是对net包的进一步封装，专用来处理HTTP协议的数据。

```go
package main

import "net/http"

func f(w http.ResponseWriter,r *http.Request){
	w.Write([]byte("hello world!"))
}

type httpServe struct{

}
func (h httpServe)ServeHTTP(w http.ResponseWriter,r *http.Request){
	w.Write([]byte("你好，中国"))
}

func main() {

	var h httpServe
	http.HandleFunc("/xx/",f)	
	http.Handle("/xxx/",h)
	//监听TCP网络地址addr，然后调用Serve with handler来处理传入连接上的请求。
	//处理器参数为nil表示采用包变量DefultServeMux作为处理器
	//监听0.0.0.0的端口，就是监听本机中所有IP的端口。
	//0.0.0.0 是对外开放，说明80端口外面可以访问；127.0.0.1 是只能本机访问，外面访问不了此端口
	http.ListenAndServe("0.0.0.0:8080",nil) 
}
```





### 自定义Server

要管理服务端的行为，可以创建一个自定义的Serve：

```go
s:=&http.Serve{
    Addr:":8080",
    Handler:myHandler,
    ReadTimeout:10*time.Second,
    WriteTimeout:10*time.Second,
    MaxHeaderBytes:1<<20,
}

//ListenAndServe监听srv.Addr指定的TCP地址，并且调用Serve方法接收到的连接。如果srv.Addr为空字符串，会使用":http"。
func (srv *Server)ListenAndServe()error



log.Fatal(s.ListenAndServe())
```















































# Flag包

go语言内置`flag`包实现了**命令行参数的解析**，`flag`包使开发命令行工具更为简单。



## os.Args

==`os.Args`可以简单获取命令行参数，`os.Args`是一个`[]string`，并且切片中的第一个元素为执行文件的名称==

```go
package main

import "os"
import "fmt"

func main() {
	if len(os.Args)>0{
		//os.Args为[]string，保管了命令行参数，其中第一个参数是程序名
		for index,arg:=range os.Args{
			fmt.Printf("args[%d]=%v\n",index,arg)
		}
	}
}
/*
D:\code\GOPATH_code\src\github.com\ghost\fuxi\07>go run main.go a b c d
args[0]=C:\Users\18390\AppData\Local\Temp\go-build3277888204\b001\exe\main.exe
args[1]=a
args[2]=b
args[3]=c
args[4]=d
*/

/*
D:\code\GOPATH_code\src\github.com\ghost\fuxi\07>go build
D:\code\GOPATH_code\src\github.com\ghost\fuxi\07>07.exe 1 2 3
args[0]=07.exe
args[1]=1
args[2]=2
args[3]=3
*/
```





## flag包基本使用

本文介绍了flag包的常用函数和基本用法，更详细的内容请查看[官方文档](https://studygolang.com/pkgdoc)。

### 导入flag包

```go
import "flag"
```



### flag参数类型

**==flag包支持的命令行参数类型有`bool`、`int`、`int64`、`uint`、`uint64`、`float` `float64`、`string`、`duration`。==**

| flag参数     | 有效值                                                       |
| ------------ | ------------------------------------------------------------ |
| 字符串flag   | 合法字符串                                                   |
| 整数flag     | 1234、0664、0x123等类型，也可以为负数                        |
| 浮点数flag   | 合法浮点数                                                   |
| bool类型flag | 1、0、t、f、T、F、true、false、TRUE、FALSE、True、False      |
| 时间段flag   | 合法的时间段字符串。如“200ms”、“-1.4h”、“3h45m”。合法单位：ns，us，$\mu$s, ms，s，m，h |



### 定义命令行flag参数

常用定义命令行`flag`参数的方法。

#### flag.Type()

基本格式：

```go
//flag名其实就是标签，在命令行中是通过该参数来指定值
flag.Type(flag名，默认值，帮助信息)*Tpye

//示例
//如定义姓名、年龄、婚否三个命令行参数，定义方式如下：
name := flag.String("name","张三","姓名") //"张三"为默认值
age := flag.Int("age",18,"年龄")
married := flag.Bool("married",false,"婚否")
delay := flag.Duration("delay",0,"时间间隔")

//注意：此时name、age、married、delay均为对应类型的指针。
```



#### flag.TypeVar()

基本格式：

```go
flag.TypeVar(Type指针，flag名，默认值，帮助信息)

//如定义姓名、年龄、婚否三个命令参数：
var name string
var age int
var married bool
var delay time.Duration
flag.StringVar(&name,"name","张三","姓名")
flag.IntVar(&age,"age",18,"年龄")
flag.BoolVar(&married,"married",false,"婚否")
flag.DurationVar(&delay,"delay",0,"时间间隔")
```



### flag.Parse()

==通过以上两种方法定义好命令行flag参数后，**需要通过调用`flag.Parse()`来对命令行参数进行解析**，这个函数的主要是把用户传递的命令行参数解析为对应变量的值。==

```go
//从os.Args[1:]中解析注册的flag。必须在所有flag都注册好而未访问其值时执行。未注册却使用flag -help时，会返回ErrHelp。
flag.Parse()
```



支持的命令行参数格式为：

+ `-flag xxx`
+ `--flag xxx`
+ `-flag=xxx`
+ `--flag=xxx`

==**注意：布尔类型的参数必须使用等号的方式指定**。==

Flag解析在第一个非flag参数（单个”-“不是flag参数）之前停止，或者在终止符”–“之后停止。



### flag其它函数

```go
flag.Args() //返回解析之后剩下的非flag参数。（不包括命令名），以[]string类型返回
flag.NArg() //NArg返回解析flag之后剩余参数的个数。
flag.NFlag() //返回进行了设置的flag的数量
```



### 完整示例

```go
package main

import (
	"flag"
	"fmt"
	"time"
)

func main() {
	//定义命令行参数方式1
	//flag.Type()*Type
	//返回指针类型
	name:=flag.String("name","张三","姓名")
	age:=flag.Int("age",18,"年龄")
	married:=flag.Bool("married",false,"婚否")
	delay:=flag.Duration("delay",0,"间隔期间")
	
	//定义命令行参数方式2
	var (
		name1 string
		age1 int
		married1 bool
		delay1 time.Duration
	)
	flag.StringVar(&name1,"name1","王五","姓名")
	flag.IntVar(&age1,"age1",20,"年龄")
	flag.BoolVar(&married1,"married1",false,"婚否")
	flag.DurationVar(&delay1,"delay1",1,"时间间隔")

	//解析os.Arg[1:]中注册的flag。必须在所有flag都注册好而未访问其值时执行。
	flag.Parse()
	
	fmt.Println("方法1",*name,*age,*married,*delay)
	fmt.Println("方法2",name1,age1,married1,delay1)
    
    //返回解析之后非flag参数
	fmt.Println("返回flag.Parse解析之后非flag参数，以字符串切片返回：",flag.Args())
	//返回解析之后非flag参数个数
	fmt.Println("flag.Parse 解析之后非flag参数个数：",flag.NArg())
	//返回进行设置了flag参数个数
	fmt.Println("在命令行中设置了flag参数个数：",flag.NFlag())
}

------------------------------------------------------------------------------------------------------

/*
D:\code\GOPATH_code\src\github.com\ghost\fuxi\07>go run main.go -name=amos -age 100 -married=t -delay 22h33m12s23ms55us199ns -name1=huihui -age1=25 -delay1=11s a b c d f
方法1 amos 100 true 22h33m12.023055199s
方法2 huihui 25 false 11s
返回flag.Parse解析之后非flag参数，以字符串切片返回： [a b c d f]
flag.Parse 解析之后非flag参数个数： 5
在命令行中设置了flag参数个数： 7

D:\code\GOPATH_code\src\github.com\ghost\fuxi\07>go run main.go -flag=help                                                           flag provided but not defined: -flag
Usage of C:\Users\18390\AppData\Local\Temp\go-build3010434552\b001\exe\main.exe:
  -age int
        年龄 (default 18)
  -age1 int
        年龄 (default 20)
  -delay duration
        间隔期间
  -delay1 duration
        时间间隔 (default 1ns)
  -married
        婚否
  -married1
        婚否
  -name string
        姓名 (default "张三")
  -name1 string
        姓名 (default "王五")
exit status 2
*/
```





# 单元测试

## 测试函数

### go语言测试

#### go test 工具

==go语言中测试依赖 `go test`命令。在包目录内，所有以`_test.go`为后缀名的源代码文件都是 `go test`测试的一部分，不会被 `go build` 编译到可执行文件中。==



**以`*_test.go`为后缀的文件中有三种类型的函数，单元测试函数、基准测试函数和示例函数**。

| 类型     | 格式                  | 作用                           |
| -------- | --------------------- | ------------------------------ |
| 测试函数 | 函数名前缀为Test      | 测试程序的一些逻辑行为是否正确 |
| 基准函数 | 函数名前缀为Benchmark | 测试函数的性能                 |
| 示例函数 | 函数名为Example       | 为文档提供示例文档             |

`go test`命令会遍历所有的`*_test.go`文件中符合上述命名规则的函数，然后生成一个临时main包用于调用相应的测试函数，然后构建并运行、报告测试结果，最后清理测试中生成的临时文件。



#### 单元测试函数

##### 格式

每个测试函数必须导入`testing`包，测试函数的基本格式如下：

```go
func TestName(t *testing.T){
    //...
}
```

==**测试函数的名字必须以`Test`开头**，可选的**后缀名必须以大写字母开头**。==

```go
func TestAdd(t *testing.T){...}
func TestSum(t *testing.T){...}
```

其中参数`t`用于报告测试失败和附加的日志信息。`testing.T`拥有的方法如下：

```go
func (c *T)Cleanup(func())
func (c *T)Error(args ...interface{})
func (c *T)Errorf(format string,args ...interface{})
func (c *T)Fail()
func (c *T)FailNow()
func (c *T)Failed()bool
func (c *T)Fatal(args ...interface{})
func (c *T)Fatalf(format string,args ...interface{})
func (c *T)Helper()
func (c *T)Log(args ...interface{})
func (c *T)Logf(format string,args ...interface{})
func (c *T)Name()string
func (c *T)Skip(args ...interface{})
func (c *T)SkipNow()
func (c *T)Skipf(format string,args...interface{})
func (c *T)Skipped()bool
func (c *T)TempDir()string
```



##### 单元测试示例

一个程序是由很多单元组件构成的，单元组件可以是函数、结构体、方法和用户可能依赖的任意东西。单元测试是利用各种方法测试单元组件的程序，判断其结果是否符合预期的结果。

```go
//base_demo/split.go

package split

import "strings"
func Split(s,sep string)(result []string){
    i:=strings.Index(s,sep)  //获取下标，从0开始的下标
    for i>-1{
        result=append(result,s[:i])
        s=s[i+1:]
        i=string.Index(s,sep)
    }
    result=append(result,s)
    return 
}
```

创建一个`split_test.go`的测试文件，并定义一个测试函数如下：

```go
//split/split_test.go

package split

import(
	"reflect"
    "testing"
)
func TestSplit(t *testing.T){
    got := Split("a:b:c",":")  //程序输出结果
    want := []string{a,b,c} //期望得到的结果
    if !reflect.DeepEqual(want,got){      //reflect.DeepEqual 针对引用类数据进行比较
        t.Errorf("expected:%v,got:%v",want,got)
    }
}
```

运行结果为：

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -v
=== RUN   TestSplit
--- PASS: TestSplit (0.00s)
PASS
ok      github.com/ghost/fuxi/08        0.407s

/*
TestSplit 表示测试函数
github.com/ghost/fuxi/08 表示文件路径
0.407s 表示测试运行时间
*/
```



#### go test -v

```go
func TestMoreSplit(t *testing.T){
    got:=split("abcd","bc")
    want:=[]string{"a","d"}
    if !reflect.DeepEqual(got,want){
        t.Errorf("want:%v,but got:%v\n",want,got)
    }
}
```



```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -v
=== RUN   TestSplit
--- PASS: TestSplit (0.00s)
=== RUN   TestMoreSplit
    split_test.go:20: expected:[a d],got:[a cd]
--- FAIL: TestMoreSplit (0.00s)
FAIL
exit status 1
FAIL    github.com/ghost/fuxi/08        0.025s
```



`go test -v`可以查看测试函数名称和运行时间，可以看到第二个测试用例`FALL`



#### go test -run

从上面测试结果可以看出，`split`函数实现并不可靠，没有考虑传入的sep参数是多个字符的情况。

```go
package main

import "strings"

func split(s,sep string)(result []string){
    i:=strings.Index(s,sep)
    for i>-1{
        result=append(result,s[:i])
        s=s[i+len(sep):]
        i=strings.Index(s,sep)
    }
    result=append(result,s)
    return
}
```

**`go test`命令添加`-run`参数**，对应一个正则表达式，==**只有被函数名匹配的测试函数才会被`go test`命令执行**==

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -v -run=More
=== RUN   TestMoreSplit
--- PASS: TestMoreSplit (0.00s)
PASS
ok      github.com/ghost/fuxi/08        0.033s
```



#### 回归测试

代码修改后，仅仅执行失败的测试用例或新引入的测试用例是错误且危险的，正确做法：完整运行所有的测试用例，保证不会因为修改代码而引入新的问题

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -v
=== RUN   TestSplit
--- PASS: TestSplit (0.00s)
=== RUN   TestMoreSplit
--- PASS: TestMoreSplit (0.00s)
PASS
ok      github.com/ghost/fuxi/08        0.030s
```

测试结果表明单元测试全部通过，通过单元测试可以在代码改动后快速进行回归测试，提高了开发效率和代码质量。



#### 跳过某些测试用例

为了节省时间，支持在单元测试中跳过某些耗时的测试用例。

```go
func TestTimeConsuming(t *testing.T){
    if testing.Short(){      //Short reports whether the -test.short flag is set
        t.Skip("Short 模式下会跳过该测试用例")
    }
    ...
}
```

示例：

```go
package split

import (
	"reflect"
	"testing"
)

func TestSplit(t *testing.T){
	got := split("a:b:c",":")
	want := []string{"a","b","c"}
	if !reflect.DeepEqual(want,got){
		t.Errorf("want:%v,but got:%v\n",want,got)
	}
}

func TestMoreSplit(t *testing.T) {
	if testing.Short(){
		t.Skip("跳过该测试用例")   //相当于到这里就已经结束了
	}
	got:=split("abcd","bc")
	want:=[]string{"a","d"}
	if !reflect.DeepEqual(got,want){
		t.Errorf("expected:%v,got:%v\n",want,got)
	}
}
```

当执行`go test -short`时就不会执行上面的`TestTimeConsuming`测试用例。

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -short -v
=== RUN   TestSplit
--- PASS: TestSplit (0.00s)
=== RUN   TestMoreSplit
    split_test.go:18: 跳过该测试用例
--- SKIP: TestMoreSplit (0.00s)
PASS
ok      github.com/ghost/fuxi/08        0.025s
```



#### 测试组

可以将多个测试用例合到一起，**即利用结构体或map包装多个测试实例，然后将其放在数组或切片内，利用循环语句，一次测试多个测试用例**。

```go
func TestSplit(t *testing.T){
   //定义一个测试用例类型
    type test struct{
        input string
        sep string
        want []string
    }
    //定义一个存储测试用例的切片
    tests:=[]test{
        {input:"a:b:c",sep:":",want:[]string{"a","b","c"}},
        {input:"a:b:c",sep:",",want:[]string{"a","b","c"}},
        {input:"abcd",sep:"bc",want:[]string{"a","d"}},
        {input:"沙河有沙又有河",sep:"沙",want:[]string{"河有","又有河"}},
    }
    for _,tc:=range tests{
        got:=split(tc.input,tc.sep)
        if !reflect.DeepEqual(got,tc.want){
            t.Errorf("want:%#v,but got:#%v\n",tc.want,got)
        }
    }
}
```

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -v -run=Split1
=== RUN   TestSplit1
    split_test.go:44: want:[]string{"a", "b", "c"},but got:[]string{"a:b:c"}
    split_test.go:44: want:[]string{"河有", "又有河"},but got:[]string{"", "河有", "又有河"}
--- FAIL: TestSplit1 (0.00s)
FAIL
exit status 1
FAIL    github.com/ghost/fuxi/08        0.031s
```

==可以看出，测试组采用切片，测试组中只有一些列测试数据，如果测试数据较多，则需要仔细查找，没有标签标明。==





#### 子测试

在上面示例中，我们为每个测试数据编写一个测试函数，==通常在单元测试中需要测试多组测试数据保证测试的效果。Go1.7+中增加了子测试，支持在测试函数中使用`t.Run`执行一组测试用例==，这样不需要定义多个测试函数。 

```go
func TestXXX(t *testing.T){
    t.Run("case1",func(t *testing.T){...})
    t.Run("case2",func(t *testing.T){...})
    t.Run("case3",func(t *testing.T){...})
}

func TestSplit2(t *testing.T){
    type test struct{
        input string
        sep string
        want []string
    }
    //采用map存储测试用例，利用map存储多个测试用例，然后利用for循环，一次测试多个
    tests:=map[string]test{
        "Simple":{input:"a:b:c",sep:":",want:[]string{"a","b","c"}},
        "wrong sep":{input:"a:b:c",sep:",",want:[]string{"a","b","c"}},
        "more sep":{input:"abcd",sep:"bc",want:[]string{"a","d"}},
        "leading sep":{input:"沙河有沙又有河",sep:"沙",want:[]string{"河有","又有河"}},
    }
    for name,tc:=range tests{
        t.Run(name,func(t *testing.T){  //t.Run 用来执行子测试
            got:=split(tc.input,tc.sep)
        if !reflect.DeepEqual(got,tc.want){
            t.Errorf("want:%#v,but got:%#v\n",tc.want,got)
        }
        })
    }
}
```

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -run=Split2 -v
=== RUN   TestSplit2
=== RUN   TestSplit2/leading_sep
    split_test.go:67: want:[]string{"河有", "又有河"},but got:[]string{"", "河有", "又有河"}
=== RUN   TestSplit2/Simple
=== RUN   TestSplit2/wrong_sep
    split_test.go:67: want:[]string{"a", "b", "c"},but got:[]string{"a:b:c"}
=== RUN   TestSplit2/more_sep
--- FAIL: TestSplit2 (0.00s)
    --- FAIL: TestSplit2/leading_sep (0.00s)
    --- PASS: TestSplit2/Simple (0.00s)
    --- FAIL: TestSplit2/wrong_sep (0.00s)
    --- PASS: TestSplit2/more_sep (0.00s)
FAIL
exit status 1
FAIL    github.com/ghost/fuxi/08        0.035s
```



==可以通过`-run=xxx`来指定运行测试用例，还可以通过`/`来指定运行的子测试用例，如：`go test -v -run=Split2/Simple`只会运行`Simple`对应的子测试用例。==

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -v -run=Split2/Simple 
=== RUN   TestSplit2
=== RUN   TestSplit2/Simple
--- PASS: TestSplit2 (0.00s)
    --- PASS: TestSplit2/Simple (0.00s)
PASS
ok      github.com/ghost/fuxi/08        0.028s
```



#### 表格驱动测试

##### 介绍

表格驱动测试不是工具、包或其他任何东西，只是编写一种更清晰的测试方式。

表格驱动测试涵盖很多方面：表格里的每一个条目都是完整的测试用例，包含输入和预期结果，有时还包含测试名称等附加信息，使测试输出易于阅读。

表格驱动测试可以方便维护多个测试用例，可以避免在编写单元测试时频繁复制粘贴。

表格驱动测试的步骤通常是定义一个测试用例表格，然后遍历表格，并使用`t.Run(name string, f func(t *T))`对每个条目执行必要的测试。

##### 示例

```go
var flagtests=[]struct{
    in string
    out string
}{
    {"%a", "[%a]"},
	{"%-a", "[%-a]"},
	{"%+a", "[%+a]"},
	{"%#a", "[%#a]"},
	{"% a", "[% a]"},
	{"%0a", "[%0a]"},
	{"%1.2a", "[%1.2a]"},
	{"%-1.2a", "[%-1.2a]"},
	{"%+1.2a", "[%+1.2a]"},
	{"%-+1.2a", "[%+-1.2a]"},
	{"%-+1.2abc", "[%+-1.2a]bc"},
	{"%-1.2abc", "[%-1.2a]bc"},
}

func TestFlagParse(t *testing.T){
    var flagprinter flagPrinter
    for _,tt:=range flagtests{
        t.Run(tt.in,func(t *testing.T){
            s:=Sprintf(tt.in,&flagprinter)
            if s!=tt.out{
                //%q 表示：双引号包裹的字符串
                t.Errorf("got:%q,want:%q",s,tt.out)
            }
        })
    }
}
```

==通常表格是匿名结构体切片==，可以定义结构体或使用已经存在的结构进行结构体数组声明。`name`属性用来描述特定的测试用例。

```go
func TestSplitAll(t *testing.T){
    //定义测试表格
    //使用匿名结构体定义若干测试用例
    //为每个测试用例设置一个名称
    tests:=[]struct{
        name string
        input string
        sep string
        want []string
    }{
        {"base case","a:b:c",":",[]string{"a","b","c"}},
        {"wrong sep","a:b:c",",",[]string{"a:b:c"}},
        {"more sep","abcd","bc",[]string{"a","d"}},
        {"leading sep","沙河有沙又有河","沙",[]string{"","河有","又有河"}}
    }
    //遍历测试用例
    for _,tt:=range tests{
        t.Run(tt.name,func(t *testing.T){
            got := split(tt.input,tt.sep)
            if !reflect.DeepEqual(got,tt.want){
                t.Errorf("want:%#v,but got:%#v\n",tt.want,got)
            }
        })
    }
}
```

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -v -run=All
=== RUN   TestSplitAll
=== RUN   TestSplitAll/base_case
=== RUN   TestSplitAll/wrong_sep
=== RUN   TestSplitAll/more_sep
=== RUN   TestSplitAll/leading_sep
--- PASS: TestSplitAll (0.00s)
    --- PASS: TestSplitAll/base_case (0.00s)
    --- PASS: TestSplitAll/wrong_sep (0.00s)
    --- PASS: TestSplitAll/more_sep (0.00s)
    --- PASS: TestSplitAll/leading_sep (0.00s)
PASS
ok      github.com/ghost/fuxi/08        0.025s
```



##### 并行测试

表格驱动测试中通常会定义比较多的测试用例，而go语言又天生支持并发，所以很容易发挥自身并发优势将表格驱动测试并行化。想要在单元测试过程中使用并行测试，可以像如下代码中添加`t.Parallel()`来实现。

```go
func TestSplitAll(t *testing.T){
    t.Parallel() //将TLog标记为能够与其他测试并行运行
    //定义测试表格
    //使用匿名结构体定义若干测试用例
    //为每个测试用例设置一个名称
    tests:=[]struct{
        name string
        input string
        sep string
        want []string
    }{
        {"base case","a:b:c",":",[]string{"a","b","c"}},
        {"wrong sep","a:b:c",",",[]string{"a:b:c"}},
        {"more sep","abcd","bc",[]string{"a","d"}},
        {"leading sep","沙河有沙又有河","沙",[]string{"","河有","又有河"}}
    }
    //遍历测试用例
    for _,tt:=range tests{
        //这里必须重新声明，如果tt发生变化，某个goroutine还没来得及在tt变化前处理完，就会产生竟态问题
         tt:=tt //注意：这里重新声明tt变量（避免多个goroutine中使用了相同的变量）
        t.Run(tt.name,func(t *testing.T){  //使用t.Run()执行子测试
            t.Parallel()  //将每个测试用例标记为能够彼此并行运行
            got := split(tt.input,tt.sep)
            if !reflect.DeepEqual(got,tt.want){
                t.Errorf("want:%#v,but got:%#v\n",tt.want,got)
            }
        })
    }
}
```

```go
=== RUN   TestSplitAll
=== PAUSE TestSplitAll
=== CONT  TestSplitAll
=== RUN   TestSplitAll/base_case
=== PAUSE TestSplitAll/base_case
=== RUN   TestSplitAll/wrong_sep
=== PAUSE TestSplitAll/wrong_sep
=== RUN   TestSplitAll/more_sep
=== PAUSE TestSplitAll/more_sep
=== RUN   TestSplitAll/leading_sep
=== PAUSE TestSplitAll/leading_sep
=== CONT  TestSplitAll/base_case
=== CONT  TestSplitAll/leading_sep
=== CONT  TestSplitAll/more_sep
=== CONT  TestSplitAll/wrong_sep
--- PASS: TestSplitAll (0.00s)
    --- PASS: TestSplitAll/base_case (0.00s)
    --- PASS: TestSplitAll/leading_sep (0.00s)
    --- PASS: TestSplitAll/more_sep (0.00s)
    --- PASS: TestSplitAll/wrong_sep (0.00s)
PASS
ok      github.com/ghost/fuxi/08        0.026s
```

执行测试后，可以发现：每个测试用例并不是按照我们定义的顺序执行，而是相互并行。

##### 实用工具生成测试代码

社区中有很多自动生成表格驱动测试函数的工具，比如[gotests](https://github.com/cweill/gotests)等，很多编辑器如Goland也支持快速生成测试文件。这里简单演示一下`gotests`的使用。

安装

```go
go get -u github.com/cweill/gotests/...
```

执行

```go
gotests -all -w split.go
```

```go
  -all                  generate tests for all functions and methods

  -excl                 regexp. generate tests for functions and methods that don't
                         match. Takes precedence over -only, -exported, and -all

  -exported             generate tests for exported functions and methods. Takes
                         precedence over -only and -all

  -i                    print test inputs in error messages

  -only                 regexp. generate tests for functions and methods that match only.
                         Takes precedence over -all

  -nosubtests           disable subtest generation when >= Go 1.7

  -parallel             enable parallel subtest generation when >= Go 1.7.

  -w                    write output to (test) files instead of stdout

  -template_dir         Path to a directory containing custom test code templates. Takes
                         precedence over -template. This can also be set via environment
                         variable GOTESTS_TEMPLATE_DIR

  -template             Specify custom test code templates, e.g. testify. This can also
                         be set via environment variable GOTESTS_TEMPLATE

  -template_params_file read external parameters to template by json with file

  -template_params      read external parameters to template by json with stdin
```



上述命令表示：为`split.go`文件的所有函数生成测试代码至`split_test.go`文件（目录下如果事先存在这个文件就不再生成）

生成的测试代码如下：

```go
package base_demo
import(
	"reflect"
    "testing"
)
func TestSplit(t *testing.T){
    type args struct{
        s string
        sep string
    }
    tests:=[]struct{
        name string
        args args
        wantResult []string
    }{
        //TODO:Add test cases.
    }
    for _,tt:=range tests{
        t.Run(tt.name,func(t *testing.T){
            if gotReuslt:=Split(tt.args.s,tt.args.sep);!reflect.DeepEqual(gotResult,tt.wantResult){
                t.Errorf("Split()=%v,want:%v",gotResult,tt.wantResult)
            }
        })
    }
}
```

代码格式与上面类似，只需要在TODO位置添加我们的测试逻辑就可以了。





#### SetUp与TearDown

测试程序有时需要在测试之前进行额外的设置（setup）或在测试之后进行拆卸（teardown）。



##### TestMain

通过在`*_test.go`文件中定义`TestMain`函数，可以在测试之前进行额外的设置或在测试之后进行拆卸操作。

如果测试文件包含函数：`func TestMain(m *testing.M)`那么生成的测试会先调用`TestMain(m)`，然后再运行具体测试。`TestMian`运行在主`goroutine`中，可以在调用`m.Run`前后做任何设置和拆卸。退出测试的时候应该使用`m.Run`的返回值作为参数调用`os.Exit`。



示例：

```go
func TestMain(m *testing.M){
    fmt.Println("write setup code here...") //测试之前做一些设置
    //如果TestMain使用了flags，这里应该加上flag.Parse()
    retCode:=m.Run() //执行测试
    fmt.Println("write teardown code here...") //测试之后做一些拆卸工作
    os.Exit(retCode)  //退出测试
}
```



示例：

```go
package TestMain
 
import (
	"fmt"
	"os"
	"testing"
)
 
func setup()  {
	fmt.Println("setup all tests")
}
func tearDown()  {
	fmt.Println("tearDown all tests")
}
func Test1(t *testing.T)  {
	fmt.Println("i'm test1")
}
func Test2(t *testing.T)  {
	fmt.Println("i'm test2")
}
func TestMain(m *testing.M)  {
	setup()
	run := m.Run()
	tearDown()
	os.Exit(run)
}
```

测试结果：

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\10>go test -v
setup all tests
=== RUN   Test1
i'm test1
--- PASS: Test1 (0.00s)
=== RUN   Test2
i'm test2
--- PASS: Test2 (0.00s)
PASS
tearDown all tests
ok      github.com/ghost/fuxi/10        0.024s
```





注意：在调用`TestMain`时，`flag.Parse`并没有被调用。所以如果`TestMain`依赖于command-line标志（包括testing包的标记），则应该显示调用`flag.Parse`。



##### 子测试的Setup与Teardown

有时候需要为每个测试集设置setup与teardown，也可能需要为每个子测试设置setup与teardown。

```go
//测试集的setup和teardown
func setupTestCase(t *testing.T) func(t *testing.T){
    t.Log("如有需要在此执行：测试之前的setup")
    return func(t *testing.T){
        t.Log("如有需要在此执行：子测试之后的teardown")
    }
}


//子测试的setup与teardown
func setupSubTest(t *testing.T)func(t *testing.T){
    t.Log("如有需要在此执行：子测试之前的setup")
    return func(t *testing.T){
        t.Log("如有需要在此执行：子测试之后的teardown")
    }
}
```

使用方式：

```go
func TestSplit(t *testing.T){
    type test struct { // 定义test结构体
		input string
		sep   string
		want  []string
	}
	tests := map[string]test{ // 测试用例使用map存储
		"simple":      {input: "a:b:c", sep: ":", want: []string{"a", "b", "c"}},
		"wrong sep":   {input: "a:b:c", sep: ",", want: []string{"a:b:c"}},
		"more sep":    {input: "abcd", sep: "bc", want: []string{"a", "d"}},
		"leading sep": {input: "沙河有沙又有河", sep: "沙", want: []string{"", "河有", "又有河"}},
	}
    
    
    teardownTestCase:=setupTestCase(t) //测试之前执行setup操作
    defer teardownTestCase(t)  //测试之后执行testdown操作
    
    
    
    for name, tc := range tests {
		t.Run(name, func(t *testing.T) { // 使用t.Run()执行子测试
            
            
			teardownSubTest := setupSubTest(t) // 子测试之前执行setup操作
			defer teardownSubTest(t)           // 测试之后执行testdoen操作
            
            
			got := Split(tc.input, tc.sep)
			if !reflect.DeepEqual(got, tc.want) {
				t.Errorf("expected:%#v, got:%#v", tc.want, got)
			}
		})
	}
}
```

测试结果为：

```bash
split $ go test -v
=== RUN   TestSplit
=== RUN   TestSplit/simple
=== RUN   TestSplit/wrong_sep
=== RUN   TestSplit/more_sep
=== RUN   TestSplit/leading_sep
--- PASS: TestSplit (0.00s)
    split_test.go:71: 如有需要在此执行:测试之前的setup
    --- PASS: TestSplit/simple (0.00s)
        split_test.go:79: 如有需要在此执行:子测试之前的setup
        split_test.go:81: 如有需要在此执行:子测试之后的teardown
    --- PASS: TestSplit/wrong_sep (0.00s)
        split_test.go:79: 如有需要在此执行:子测试之前的setup
        split_test.go:81: 如有需要在此执行:子测试之后的teardown
    --- PASS: TestSplit/more_sep (0.00s)
        split_test.go:79: 如有需要在此执行:子测试之前的setup
        split_test.go:81: 如有需要在此执行:子测试之后的teardown
    --- PASS: TestSplit/leading_sep (0.00s)
        split_test.go:79: 如有需要在此执行:子测试之前的setup
        split_test.go:81: 如有需要在此执行:子测试之后的teardown
    split_test.go:73: 如有需要在此执行:测试之后的teardown
=== RUN   ExampleSplit
--- PASS: ExampleSplit (0.00s)
PASS
ok      github.com/Q1mi/studygo/code_demo/test_demo/split       0.006s
```







#### 测试覆盖率

测试覆盖率：指代码被测试套件覆盖的百分比。通常为使用的语句覆盖率，就是在测试中至少被运行一次的代码占总代码的比例。一般公司内部要求测试覆盖率达到80%左右。

**go语言内置功能来检查代码覆盖率，即使用`go test -cover`来查看测试覆盖率**。

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -run=All -cover    
PASS
coverage: 100.0% of statements
ok      github.com/ghost/fuxi/08        0.029s
```

从上述结果中，可以看出我们测试用例覆盖了100%的代码。



**go语言还提供了一个额外的`-coverprofile`参数，用来将覆盖率相关的记录信息输出到一个文件中。**

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -run=All -coverprofile=c.out
PASS
coverage: 100.0% of statements
ok      github.com/ghost/fuxi/08        0.029s
```



上述命令会将覆盖率相关信息输出到当前文件夹下的`c.out`文件中

![image-20221215185534040](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221215185534040.png)



**然后，执行`go tool cover -html=c.out`，使用`cover`工具来处理生成的记录信息，该命令会打开本地浏览器窗口生成一个HTML报告。**

![image-20221215190059694](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221215190059694.png)

上图中绿色标记的语句块表示被覆盖，红色表示未被覆盖。



### testify/assert

**[testify](https://github.com/stretchr/testify)是一个社区非常流行的Go单元测试工具包**，其中使用最多的功能就是它提供的断言工具——`testify/assert`或`testify/require`。

安装：

```go
go get github.com/stretchr/testify
```

使用示例：

在写单元测试的时候，通常需要使用断言来校验测试结果，但是由于go语言没有提供断言，所以会写出很多`if...else...`语句。而`testify/assert`提供了很多常用的断言函数，并且能够输出友好、易于阅读的错误描述信息。



比如之前在`TestSplit`测试函数中使用了`reflect.DeepEqual`来判断期望结果与实际结果是否一致。

```go
t.Run(tt.name,func(t *testing.T){
    got:=split(tt.input,tt.sep)
    if !reflect.DeepEqual(got,tt.want){
        t.Errorf("expected:%v,got:%v\n",tt.want,got)
    }
})
```

使用`tstify/assert`之后就能将上述判断简化为如下：

```go
t.Run(tt.name,func(t *testing.T){
    got:=split(tt.input,tt.sep)
    assert.Equal(tt.got,tt.want)
})
```

当我们有多个断言语句时，还可以使用`assert:=assert.New(t)`创建一个`assert`对象，它拥有前面所有断言方法，只是不需要再传入`testing.T`参数 了。

```go
func TestSomething(t *testing.T){
    assert:=assert.New(t)
    //assert equality
    assert.Equal(123,123,"they should be equal")
    
    //assert inequality
    assert.NotEqual(123,456,"they should not be equal")
    
    //assert for nil (good for errors)
    assert.Nil(object)
    
    //assert for not nil (good when you expect something)
    if assert.NotNil(object){
    //now we know that object isn't nil, we are safe to makke further assertions without casuing any errors
        assert.Equal("something",object.Value)
    }
}
```

`testify/assert`提供了非常多的断言函数，可以参考[官方文档](https://pkg.go.dev/github.com/stretchr/testify/assert#pkg-functions)了解。



==**`testify/require`拥有`testify/assert`所有断言函数，唯一区别是——`testify/require`遇到失败的用例会立刻终止本测试**。==



==此外，`testify`包还提供了[mock](https://pkg.go.dev/github.com/stretchr/testify/mock)、[http](https://pkg.go.dev/github.com/stretchr/testify/http)等其他测试工具。==





## 基准测试

### 基准测试函数格式

基准测试就是在一定的工作负载之下检测程序性能的一种方法。基准测试的基本格式如下：

```go
func BenchmarkName(b *testing.B){
    //...
}
```



基准测试以`Benchmark`为前缀，需要一个`*testing.B`类型的参数b**，基准测试必须要执行`b.N`次**，这样测试才有对照性。`b.N`的值是系统根据实际情况去调正的，从而保证测试的稳定性。



`testing.B`拥有的方法如下：

```go
func (c *B)Error(args ...interface{})
func (c *B)Errorf(format string,args ...interface{})
func (c *B)Fail()
func (c *B)FailNow()
func (c *B) Failed() bool
func (c *B) Fatal(args ...interface{})
func (c *B) Fatalf(format string, args ...interface{})
func (c *B) Log(args ...interface{})
func (c *B) Logf(format string, args ...interface{})
func (c *B) Name() string
func (b *B) ReportAllocs()
func (b *B) ResetTimer()
func (b *B) Run(name string, f func(b *B)) bool
func (b *B) RunParallel(body func(*PB))
func (b *B) SetBytes(n int64)
func (b *B) SetParallelism(p int)
func (c *B) Skip(args ...interface{})
func (c *B) SkipNow()
func (c *B) Skipf(format string, args ...interface{})
func (c *B) Skipped() bool
func (b *B) StartTimer()
func (b *B) StopTimer()
```



### 基准测试示例

如为`split`包中的`split`函数编写基准测试如下：

```go
func BenchmarkSplit(b *testing.B){
    for i:=0;i<b.N;i++{
        split("沙河有沙又有河","沙")
    }
}
```

==**基准测试并不会默认执行，需要增加`-bench`参数，所以通过执行`go test -bench=split`命令执行基准测试**==，输出结果为：

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -bench=Split
goos: windows
goarch: amd64
pkg: github.com/ghost/fuxi/08
cpu: AMD Ryzen 7 5800H with Radeon Graphics
BenchmarkSplit-16       10477209               114.4 ns/op
PASS
ok      github.com/ghost/fuxi/08        1.344s
```

其中`BenchmarkSplit-16`表示对split函数执行基准测试，数字`16`表示`GOMAXPROCS`的值，这个对于并发基准测试很重要。`10477209`和`114.4ns/op`表示每次调用`split`函数耗时`114.4ns`，这个结果是`10477209`次调用的平均值。`1.344s`表示总的基准测试时间



我们还可以为基准测试==**添加`-benchmem`参数，来获取内存分配的统计数据**==。

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -bench=Split -benchmem
goos: windows
goarch: amd64
pkg: github.com/ghost/fuxi/08
cpu: AMD Ryzen 7 5800H with Radeon Graphics
BenchmarkSplit-16       10469568               111.0 ns/op           112 B/op          3 allocs/op
PASS
ok      github.com/ghost/fuxi/08        1.314s
```

其中，`112B/op`表示每次操作内存分配了112字节，`3 allocs/op`表示每次操作进行了3次内存分配。



优化我们的`split`函数：

```go
func split1(s,sep string)(result []string){
    result=make([]string,0,strings.Count(s,sep)+1)
    i:=strings.Index(s,sep)
    for i>-1{
        result=append(result,s[:i])
        s=s[i+len(sep):] 
        i=strings.Index(s,sep)
    }
    result=append(result,s)
    return
}
```

这一次提前使用make函数将result初始化为一个容量足够大的切片，而不是像之前那样通过调用append函数来追加。性能得到了提升：

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -bench=Split1 -benchmem
goos: windows
goarch: amd64
pkg: github.com/ghost/fuxi/08
cpu: AMD Ryzen 7 5800H with Radeon Graphics
BenchmarkSplit1-16      21664364                51.76 ns/op           48 B/op          1 allocs/op
PASS
ok      github.com/ghost/fuxi/08        1.215s
```

使用make函数提前分配内存的改动，减少了2/3的内存分配次数，并减少了一般的内存分配。



### 性能比较函数

上面基准测试只能给定操作的绝对耗时，然而很多性能问题是发生在两个不同操作之间的相对耗时，比如同一个函数处理1000个元素的耗时与处理10万个元素耗时的差别是多少？或者通过一个任务使用哪种算法性能最佳？通常需要对两个不同算法的实现使用相同输入来进行基准比较测试。



**==性能比较函数通常是一个带有参数的函数，被多个不同的Benchmark函数传入不同的值来调用。==**

```go
func benchmark(b *testing.B,size int){...}
func benchmark10(b *testing.B){benchmark(b,10)}
func benchmark100(b *testing.B){benchmark(b,100)}
func benchmark1000(b *testing.B){benchmark(b,1000)}
```



编写一个计算斐波那契数列的函数如下：

```go
//fib.go

//Fib是一个计算第n个斐波那契数列的函数
func Fib(n int)int{
    if n<2{
        return n
    }
    return Fib(n-1)+Fib(n-2)
}
```

编写性能比较函数如下：

```go
//fib_test.go

func benchmarkFib(b *testing.B,n int){
    for i:=0;i<b.N;i++{
        Fib(n)
    }
}

func BenchmarkFib1(b *testing.B){benchmarkFib(b,1)}
func BenchmarkFib2(b *testing.B){benchmarkFib(b,2)}
func BenchmarkFib3(b *testing.B){benchmarkFib(b,3)}
func BenchmarkFib10(b *testing.B){benchmarkFib(b,10)}
func BenchmarkFib20(b *testing.B){benchmarkFib(b,20)}
func BenchmarkFib40(b *testing.B){benchmarkFib(b,40)}

```

运行基准测试：

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -bench=. -benchmem   
goos: windows
goarch: amd64
pkg: github.com/ghost/fuxi/08
cpu: AMD Ryzen 7 5800H with Radeon Graphics
BenchmarkSplit-16       10358346               116.4 ns/op           112 B/op          3 allocs/op
BenchmarkSplit1-16      20859480                53.59 ns/op           48 B/op          1 allocs/op
BenchmarkFib1-16        860213820                1.417 ns/op           0 B/op          0 allocs/op
BenchmarkFib2-16        286589474                4.238 ns/op           0 B/op          0 allocs/op
BenchmarkFib3-16        171866683                6.931 ns/op           0 B/op          0 allocs/op
BenchmarkFib10-16        5053509               236.5 ns/op             0 B/op          0 allocs/op
BenchmarkFib20-16          39946             30163 ns/op               0 B/op          0 allocs/op
BenchmarkFib40-16              3         464834300 ns/op               0 B/op          0 allocs/op
PASS
ok      github.com/ghost/fuxi/08        13.141s
```

其中：**`-bench=.`表示执行所有基准测试**，==默认情况下，每个基准测试至少运行1秒。如果在Benchmark函数返回时没有执行1秒，则`b.N`的值会按1，2，5，10，20，50，...，增加，并且函数再次运行。==`13.141s`表示所有基准测试运行完成时间。



> BenchmarkFib40运行了3次，每次运行的平均时间只有不到1秒。像这种情况下，我们可以使用`-benchtime`标志增加最小基准时间，即调整默认值，使其能够运行多次（因此多次取平均值，结果更为准确）。



例如：

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -bench=Fib40 -benchmem -benchtime=10s
goos: windows
goarch: amd64
pkg: github.com/ghost/fuxi/08
cpu: AMD Ryzen 7 5800H with Radeon Graphics
BenchmarkFib40-16             25         462868808 ns/op               0 B/op          0 allocs/op
PASS
ok      github.com/ghost/fuxi/08        12.066s
```

此时，`benchmarkFib40`函数运行了25次，结果就会更准确一些。



使用性能比较函数做测试的时候，容易犯错误是：把`b.N`作为输入的大小，如下：

```go
//错误示例1
func BenchmarkFibWrong(b *testing.B){
    for n:=0;n<b.N;n++{
        Fib(n)
    }
}

//错误示例2
func BenchmarkFibWrong2(b *testing.B){
    Fib(b.N)
}
```



### 重置时间

==`b.ResetTimer`之前的处理不会放到执行时间里，也不会输出到报告中，所以可以在之前做一些不计划作为测试报告的操作。==例如：

```go
func BenchmarkSplit(b *testing.B){
    time.Sleep(5*time.Second) //假设需要做一些耗时的无关操作
    b.ResetTimer()
    for i:=0;i<b.N;i++{
        split("沙河有沙又有河","沙")
    }
}
```



### 并行测试

`func (b *B)RunParallel(body func(*testing.PB))`会以并行的方式执行给定的基准测试。

==**`RunParallel`会创建出多个`goroutine`，并将`b.N`分配给这些`goroutine`执行，其中`goroutine`数量的默认值`GOMAXPROCS`**==。用户如果想要增加非CPU受限基准测试的并行性，可以在`RunParallel`之前调用`SetParallelism`。`RunParallel`通常会与`-cpu`标志一起使用。



```go
func BenchmarkSplitParallel(b *testing.B){
    //将 RunParallel 使用的 goroutine 数量设置为 p*GOMAXPROCS ，如果 p 小于 1 ，那么调用将不产生任何效果。
	//CPU受限（CPU-bound）的基准测试通常不需要调用这个方法。
    
    //b.SetParallelism(1) //启动多少goroutine来执行
    b.RunParallel(func(pb *testing.PB){
        for pb.Next(){
             split("沙河有沙又有河","沙")
        }
    })
}
```

执行结果为：

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -bench=Parallel -benchmem
goos: windows
goarch: amd64
pkg: github.com/ghost/fuxi/08
cpu: AMD Ryzen 7 5800H with Radeon Graphics
BenchmarkSplitParallel-16       43181155                25.54 ns/op          112 B/op          3 allocs/op      
PASS
ok      github.com/ghost/fuxi/08        1.173s
```

还可以在测试命令后添加`-cpu`参数，如`go test -bench=Parallel -cpu=1`来指定使用cpu的数量

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -bench=Parallel -benchmem -cpu=1
goos: windows
goarch: amd64
pkg: github.com/ghost/fuxi/08
cpu: AMD Ryzen 7 5800H with Radeon Graphics
BenchmarkSplitParallel  10294322               114.3 ns/op           112 B/op          3 allocs/op
PASS
ok      github.com/ghost/fuxi/08        1.316s

D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -bench=Parallel -benchmem -cpu=2
goos: windows
goarch: amd64
pkg: github.com/ghost/fuxi/08
cpu: AMD Ryzen 7 5800H with Radeon Graphics
BenchmarkSplitParallel-2        18656570                60.66 ns/op          112 B/op          3 allocs/op
PASS
ok      github.com/ghost/fuxi/08        1.241s
```





## 示例函数

### 示例函数格式

被`go test`特殊对待的第三种函数就是示例函数，它们的函数名以`Example`为前缀。它们既没有参数也没有返回值。格式为：

```go
func ExampleName(){
    //...
}
```



### 示例函数示例

为`Split`函数编写一个示例函数：

```go
func ExampleSplit() {
	fmt.Println(split.Split("a:b:c", ":"))
	fmt.Println(split.Split("沙河有沙又有河", "沙"))
	// Output:
	// [a b c]
	// [ 河有 又有河]
}
```

注意：示例函数必须按照以上格式来写（包括// Output等内容，//后面必须要有空格），格式出现差错就会报错。



为代码编写示例函数有三个好处：

+ 示例函数能够作为文档直接使用，例如基于web的godoc中能把示例函数与对应的函数或包相关联。

+ 示例函数只要包含了`// Output:`也是可以通过`go test`运行的可执行测试。

  ```bash
   D:\code\GOPATH_code\src\github.com\ghost\fuxi\08>go test -run=Example
  PASS
  ok      github.com/ghost/fuxi/08        0.031s
  ```

+ 示例函数提供了可以直接运行的示例代码，可以直接在`golang.org`的`godoc`文档服务器上使用`Go Playground`运行示例代码。下图为`strings.ToUpper`函数在Playground的示例函数效果。

  ![image-20221215223323694](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221215223323694.png)



## 网络测试





## MySQL和Redis测试



## mock接口测试





## monkey打桩测试



## goconvey的使用



## 编写可测试的代码











# pprof性能调优

在计算机性能调试领域里，**profiling是指对应程序的画像，画像就是应用程序使用cpu和内存的情况**。go语言是一个对性能特别看重的语言，因此go语言中自带了profiling的库。

## Go性能优化

go语言项目中的性能优化主要有以下几个方面：

+ **cpu profile： 报告程序的cpu使用情况，按照一定频率去采集应用程序在cpu和寄存器上面的数据。**
+ **Memory profile： 报告程序的内存使用情况。**
+ **Block profiling： 报告goroutines不在运行状态情况，可以用来分析和查找死锁等性能瓶颈。**
+ **Goroutine profiling： 报告goroutines的使用情况，有哪些goroutine，它们的调用关系是怎样的。**



### 采集性能数据

go语言内置了获取程序的运行数据的工具，两个标准库：

+ **`runtiem/pprof`：采集工具型应用运行数据进行分析**
+ **`net/http/pprof`： 采集服务型应用运行时数据进行分析**



**pprof开启后，每隔一段时间（10ms）就会收集当前的堆栈信息，获取各个函数占用的cpu以及内存资源**；最后通过对这些采样数据进行分析，形成一个性能分析报告。

==**注意：只应该在性能测试的时候才在代码中引入pprof**==



### 工具型应用

如果应用程序在运行一段时间就结束退出，那么最好就是在应用退出时把profiling的报告保存在文件中，进行分析。针对这种情况，可以使用`runtime/pprof`库。

首先在代码中导入`runtime/pprof`工具：

```go
import "runtime/pprof"
```



#### cpu性能分析



**开启cpu性能分析：**

```go
pprof.StartCPUProfile(w io.Writer)
```

**停止cpu性能分析:**

```go
pprof.StopCPUProfile()
```

应用程序结束后，就会生成一个文件，保存cpu profiling数据。得到采样数据后，**使用`go tool pprof`工具进行cpu性能分析**。



#### 内存性能分析



**记录程序的堆栈信息：**

```go
pprof.WriteHeapProfile(w io.Writer)
```

得到采样数据后，使用`go tool pprof`工具进行内存性能分析。



==`go tool pprof`默认使用`-inuse_space`进行统计，还可以使用`-inuse-objects`查看分配对象数量。==



### 服务型应用

如果应用程序是一直运行的，如web应用，则可以使用`net/http/pprof`库，它能够在提供HTTP服务进行分析。

如果使用了默认的`http.DefaultServeMux`（通常是代码直接使用`http.ListenAndServe("0.0.0.0:8000",nil)`），只需要在web server端代码中按如下方式导入`net/http/pprof`

```go
import _ "net/http/pprof"
```

如果使用自定义的Mux，则需要手动注册一些路由规则：

```go
r.HandleFunc("/debug/pprof/",pprof.Index)
r.HandleFunc("/debug/pprof/cmdline",pprof.Cmdline)
r.HandleFunc("/debug/pprof/profile",pprof.Profile)
r.HandleFunc("/debug/pprof/symbol",pprof.Symbol)
r.HandleFunc("/debug/pprof/trace",pprof.Trace)
```

==**如果使用的是gin框架，那么推荐使用[github.com/gin-contrib/pprof](https://github.com/gin-contrib/pprof)，在代码中通过以下命令注册pprof相关路由**。==

```go
pprof.Register(router)
```

不管哪种方式，HTTP服务都会多出`/debug/pprof` endpoint，访问它会得到类似下面的内容：

![image-20221216150824678](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221216150824678.png)





**这个路径下还有几个子页面：**

- **/debug/pprof/profile：访问这个链接会自动进行 CPU profiling，持续 30s，并生成一个文件供下载**
- **/debug/pprof/heap： Memory Profiling 的路径，访问这个链接会得到一个内存 Profiling 结果的文件**
- **/debug/pprof/block：block Profiling 的路径**
- **/debug/pprof/goroutines：运行的 goroutines 列表，以及调用关系**





### go tool pprof命令

不管是工具型应用还是服务型应用，使用相应的pprof库获取数据之后，需要对这些数据进行分析，可以使用`go tool pprof`命令工具。

`go tool pprof`最简单的使用方式：

```go
go tool pprof [binary] [source]
```

其中：

+ binary：是应用的二进制文件，用来解析各种符号
+ source：表示profile数据的来源，可以是本地文件，也可以是http地址

==注意：获取的Profiling数据是动态的，想要获取有效的数据，得保证应用处于较大负载（如：正在生成中运行得服务，或者通过其他工具模拟访问压力）。否则如果应用处于空闲状态，得到的结果可能没有任何意义。==



### 具体示例

```go
package main

import (
	"flag"
	"fmt"
	"os"
	"runtime/pprof"
	"time"
)

//一段有问题的代码
func logicCode(){
	var c chan int  //nil
	for {
		select{
		case v:=<-c:
			fmt.Printf("recv from chan,value:%v\n",v)
		default:

		}
	}
}

func main(){
	var isCPUPprof bool
	var isMemPprof bool

	flag.BoolVar(&isCPUPprof,"cpu",false,"turn cpu pprof on")
	flag.BoolVar(&isMemPprof,"mem",false,"turn mem pprof on")
	flag.Parse()

	if isCPUPprof{
		file,err:=os.Create("./cpu.pprof")
		if err!=nil{
			fmt.Printf("create cpu pprof failed,err:%v\n",err)
			return
		}
        defer file.Close()
		pprof.StartCPUProfile(file)
		defer pprof.StopCPUProfile()
		
	}
	for i:=0;i<8;i++{
		go logicCode()
	}
	time.Sleep(20*time.Second)
	if isMemPprof{
		file,err:=os.Create("./mem.pprof")
		if err!=nil{
			fmt.Printf("create mem pprof failed,err:%v\n",err)
			return
		}
		pprof.WriteHeapProfile(file)
		file.Close()
	}
}
```

通过flag，可以在命令行控制是否开启CPU和Mem的性能分析。可以将上述代码保存并编译成`runtime_pprof`可执行文件，执行时加上`-cpu`命令行参数如下：

```go
./runtime_pprof -cpu=true

//或
D:\code\GOPATH_code\src\github.com\ghost\fuxi\10>go run main.go -cpu=true
```

等待30秒后会在当前目录下生成一个`cpu.pprof`文件。



#### 命令行交互界面



##### cpu pprof

使用go工具链里的`pprof`来分析：

```go
go tool pprof cpu.pprof
```

执行结果为：

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\10>go tool pprof cpu.pprof
Type: cpu
Time: Dec 16, 2022 at 4:12pm (CST)
Duration: 20.20s, Total samples = 101.04s (500.26%)
Entering interactive mode (type "help" for commands, "o" for options)
(pprof) 
```

可以在交互界面输入`top3`来查看程序中占用cpu前3位函数：

```go
(pprof) top3
Showing nodes accounting for 100.63s, 99.59% of 101.04s total
Dropped 40 nodes (cum <= 0.51s)
      flat  flat%   sum%        cum   cum%
    73.73s 72.97% 72.97%     73.73s 72.97%  runtime.chanrecv
    17.62s 17.44% 90.41%     91.35s 90.41%  runtime.selectnbrecv
     9.28s  9.18% 99.59%    100.63s 99.59%  main.logicCode
(pprof)
```

其中：

+ flat：当前函数占用cpu的耗时
+ flat%：当前函数占用cpu的耗时百分比
+ sum%：函数占用cpu的耗时累计百分比
+ cum：当前函数加上调用该当前函数的函数占用cpu的总耗时
+ cum%：当前函数加上调用当前函数的函数占用cpu的总耗时百分比
+ 最后一列：函数名称



大多数情况下，可以通过分析这五列得出一个应用程序的运行情况，并对程序进行优化。

还可以使用`list 函数名`命令查看具体的函数分析，如执行`list logicCode`查看我们编写的函数的详细分析。

```go
(pprof) list logicCode 
Total: 101.04s
ROUTINE ======================== main.logicCode in D:\code\GOPATH_code\src\github.com\ghost\fuxi\10\main.go
     9.28s    100.63s (flat, cum) 99.59% of Total
         .          .     11://一段有问题的代码
         .          .     12:func logicCode(){
         .          .     13:   var c chan int
         .          .     14:   for {
         .          .     15:           select{
     9.28s    100.63s     16:           case v:=<-c:
         .          .     17:                   fmt.Printf("recv from chan,value:%v\n",v)
         .          .     18:           default:
         .          .     19:
         .          .     20:           }
         .          .     21:   }
(pprof) 
```

通过分析发现大部分cpu资源被16行占用，我们分析出select语句中的default没有内容导致上面的`case v:=<-c:`一直执行。我们在default分支添加一行`time.Sleep(time.Second)`即可。



##### memory pprof

生成mem.pprof文件：

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\10>go run main.go -mem=true
```

对mem.pprof数据进行分析：

```go
D:\code\GOPATH_code\src\github.com\ghost\fuxi\10>go tool pprof mem.pprof
Type: inuse_space
Time: Dec 16, 2022 at 9:20pm (CST)
Entering interactive mode (type "help" for commands, "o" for options)
```

消耗内存占比前五：

```go
(pprof) top5
Showing nodes accounting for 4099.70kB, 100% of 4099.70kB total
Showing top 5 nodes out of 15
      flat  flat%   sum%        cum   cum%
 3587.50kB 87.51% 87.51%  3587.50kB 87.51%  runtime.allocm
  512.20kB 12.49%   100%   512.20kB 12.49%  runtime.malg
         0     0%   100%   512.50kB 12.50%  runtime.mcall
         0     0%   100%     3075kB 75.01%  runtime.mstart
         0     0%   100%     3075kB 75.01%  runtime.mstart0
(pprof) 
```

mem用法和cpu用法相同。



#### 图形化

或者可以直接输入web，通过svg图的方式查看程序中详细的cpu占用情况。想要查看图形化界面首先需要安装[graphviz](https://graphviz.gitlab.io/)图形化工具。

Mac：

```go
brew install graphviz
```



Windows：下载[graphviz](https://graphviz.gitlab.io/)将`graphviz`安装目录下的bin文件夹添加到Path环境变量中。在终端输入`dot -version`查看是否安装成功。

![image-20221216202345126](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221216202345126.png)

图形说明：每个框代表一个函数，理论上框的越大表示占用的cpu资源越多。方框之间的线条代表函数之间的调用关系。线条上的数字表示函数调用的次数。方框中的第一行数字表示当前函数占用的cpu百分比，第二行数字表示当前函数累计占用cpu的百分比。



除了分析cpu性能数据，pprof也支持分析内存性能数据。比如，使用下面命令分析http服务的heap性能数据，查看当前程序的内存占用以及热点内存对象使用情况。

```go
# 查看内存占用数据
go tool pprof -inuse_space http://127.0.0.1:8080/debug/pprof/heap
go tool pprof -inuse_objects http://127.0.0.1:8080/debug/pprof/heap
# 查看临时内存分配数据
go tool pprof -alloc_space http://127.0.0.1:8080/debug/pprof/heap
go tool pprof -alloc_objects http://127.0.0.1:8080/debug/pprof/heap
```





### go-torch和火焰图

火焰图（Flame Graph）是Bredan Gregg创建的一种性能分析图表，因为样子近似🔥而得名。可以将上述的profiling结果转换成火焰图。

通过工具`go-torch`（这是uber开源的一个工具）直接读取golang profiling数据，并生成一个火焰图的svg文件。



#### 安装go-torch

```go
go get -v github.com/uber/go-torch
```

火焰图svg文件可以通过浏览器打开，它对于调用图的有点就是动态的：可以通过点击每个方块来zoom in 分析它上面的内容。

火焰图的调用顺序从下到上，每个方块代表一个函数，它上面一层表示这个函数会调用哪些函数，方块大小代表占用cpu使用的长短。火焰图的配色并没有特殊的意义，默认的红、黄配色是为了更像火焰而已。

go-torch工具使用非常简单，没有任何参数的话，它会尝试从`http://localhost:8080/debug/pprof/profile`获取profiling数据。它有三个常用的参数可以调正：

+ -u -url：要访问的URL，这里只是主机和端口部分
+ -s -suffix：pprof profile的路径，默认为：/debug/pprof/profile
+ -seconds：要执行profiling的时间长度，默认为30s



#### 安装FlameGraph

要生成火焰图，需事先安装FlameGraph工具，安装简单（需要perl环境支持），只要把对应的可执行文件加入到环境变量中即可。

 1. 下载安装perl：[https://www.perl.org/get.heml](https://www.perl.org/get.html)

 2. 下载FlameGraph：`git clone https://github.com/brendangregg/FlameGraph.git`

 3. 将`FlameGraph`目录加入操作系统的环境变量中

 4. Windows平台，需要把`go-torch/render/flamegraph.go`文件中的`GenerateFlameGraph`按如下方式修改，然后在`go-torch`目录下执行`go install`即可。

    ```go
    // GenerateFlameGraph runs the flamegraph script to generate a flame graph SVG. func GenerateFlameGraph(graphInput []byte, args ...string) ([]byte, error) {
    flameGraph := findInPath(flameGraphScripts)
    if flameGraph == "" {
    	return nil, errNoPerlScript
    }
    if runtime.GOOS == "windows" {
    	return runScript("perl", append([]string{flameGraph}, args...), graphInput)
    }
      return runScript(flameGraph, args, graphInput)
    }
    ```

    

#### 压测工具wrk

**推荐使用[https://github.com/wg/wrk](https://github.com/wg/wrk)或[https://github.com/adjust/go-wrk](https://github.com/adjust/go-wrk)**



#### 使用go-torch

使用wrk进行压测：

```go
go-wrk -n 50000 http://127.0.0.1:8080/book/list
```

在上面压测进行的同时，打开另一个终端执行：

```go
go-torch -u http://127.0.0.1:8080 -t 30
```

30秒后，终端会出现提示：`Writing svg to torch.svg`

然后使用浏览器打开`torch.svg`就能看到如图所示的火焰图了。

![image-20221216211618701](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221216211618701.png)

火焰图的y轴表示cpu调用方法的先后，x轴表示在每个采样调用时间内，方法所占时间百分比，越宽代表占据cpu时间越多。通过火焰图就可以更加清楚的找出耗时长的函数调用，然后不断修正代码，重新采样，不断优化。



此外，可以借助火焰图分析内存性能数据：

```go
go-torch -inuse_space http://127.0.0.1:8080/debug/pprof/heap
go-torch -inuse_objects http://127.0.0.1:8080/debug/pprof/heap
go-torch -alloc_space http://127.0.0.1:8080/debug/pprof/heap
go-torch -alloc_objects http://127.0.0.1:8080/debug/pprof/heap
```



### pprof与基准测试结合

**`go test`命令有两个参数和pprof相关，它们分别指定生成的CPU和Memory profiling 保存的文件**：

+ -cpuprofile： cpu profiling 数据要保存的文件地址
+ -memprofile： memory profiling数据要保存的文件地址



可以选择将pprof与基准测试相结合，如：

==**在执行测试的同时，也会执行cpu profiling，并把结果保存在cpu.pprof文件中：**==

```go
go test -bench . -cpuprofile=./cpu.pprof
```

==**在执行测试的同时，也会执行Mem profiling，并把结果保存在mem.pprof文件中：**==

```go
go test -bench . -memprofile=./mem.pprof
```

==注意：profiling一般和基准测试一起使用，原因：只有在应用负载高的情况下，profiling才有意义。==





# context

在go http包的server中，每一个请求在都有一个对应的goroutine去处理。请求处理函数通常会启动额外的goroutine来访问后端服务器，比如数据库和RPC服务。==用来处理一个请求的goroutine通常需要访问一些与请求特定的数据，比如终端用户的身份认证信息、验证相关的token、请求截止时间。当一个请求被取消后或超时后，所有用来处理该请求的goroutine都应该迅速退出，然后系统才能释放这些goroutine占用的资源。==



## 为什么需要context

### 基本示例

```go
package main

import (
	"fmt"
    "sync"
    "time"
)


var wg sync.WaitGroup

func f(){
    for {
    	fmt.Println("worker")
    	time.Sleep(time.Second)
        }
    wg.Done()
}


func main(){
    wg.Add(1)
    go f()
    time.Sleep(10*time.Second)  
    //如何优雅地退出子goroutine？
    wg.Wait()
    fmt.Println("over")
}
```



### 全局变量方式

```go
package main

import (
	"fmt"
    "sync"
    "time"
)


var wg sync.WaitGroup
var exit bool

func f(){
    for {
        fmt.Println("worker")
    	time.Sleep(time.Second)
        if exit{
            break
        }
    }
    wg.Done()
}


func main(){
    wg.Add(1)
    go f()
    time.Sleep(10*time.Second)
    //如何优雅地退出子goroutine？
    exit = true
    wg.Wait()
    fmt.Println("over")
}
```





### 通道方式

```go
package main

import (
	"fmt"
    "sync"
    "time"
)


var wg sync.WaitGroup

//注意：管道方式存在的问题：使用全局变量在跨包调用时不容易实现规范统一，需要维护一个共用的channel

func f(exitChan chan bool){
    Loop:
    	for {
       
    		fmt.Println("worker")
    		time.Sleep(time.Second)
        	select(
           		 case <-exitChan:
            		break Loop
             	default:
       		 )
            }
   	wg.Done()
}


func main(){
    var exitChan = make(chan bool,1)
    wg.Add(1)
    go f(exitChan)
    time.Sleep(10*time.Second)
    //如何优雅地退出子goroutine？
   	exitChan<-true  //给管道发送信号
    wg.Wait()
    fmt.Println("over")
}
```



### 官方版的方案

```go
package main

import (
	"fmt"
    "sync"
    "time"
)


var wg sync.WaitGroup


func f(ctx context.Context){
    
 Loop:
    for (
    	fmt.Println("worker")
    	time.Sleep(time.Second)
        select {
            case <-ctx.Done()://等待上级通知
            	break Loop
            default:
        }
    )
    wg.Done()
}


func main(){
    ctx,cancel := context.WithCancel(context.Background())
    wg.Add(1)
    go f(ctx)
    time.Sleep(10*time.Second)
    //如何优雅地退出子goroutine？
    cancel() //通知子goroutine结束
    wg.Wait()
    fmt.Println("over")
}
```



当一个子goroutine又开启另外一个goroutine时，只需要将ctx传入即可：

```go
package main

import(
	"context"
    "fmt"
    "sync"
    "time"
)

var wg WaitGroup

func f(ctx context.Context){
    go f1(ctx)
    Loop:
    for{
        fmt.Println("worker")
        time.Sleep(time.Second)
        select{
            case <-ctx.Done():
            	break Loop
            default:
        }
    }
    wg.Done()
}

func f1(ctx context.Context){
    Loop:
    for{
        fmt.Println("worker2")
        time.Sleep(time.Second)
        select{
            case <-ctx.Done():
            	break Loop
            default:
        }
    }
}

func main(){
    ctx,cancel:=context.WithCancel(context.Background())
    wg.Add(1)
    go f(ctx)
    time.Sleep(time.Second*3)
    cancel() //通知子goroutine结束
    wg.Wait()
    fmt.Print("over")
}
```





## context初识

Go1.7 加入一个新的标准库`context`，它定义了`Context`类型，**专门用来简化对于处理单个请求的多个goroutine之间与请求域的数据、取消信号、截止时间等相关操作**，这些操作可能涉及多个API调用。



==“Go 1.7 标准库引入 context,中文译作“上下文”,准确说它是 goroutine 的上下文,包含 goroutine 的运行状态、环境、现场等信息。 context 主要用来在 goroutine 之间传递上下文信息,包括:取消信号、超时时间、截止时间、k-v 等。==



![image-20230211174645988](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230211174645988.png)



对于服务器传入的请求应该创建`Context`，而对服务器的传出调用应该接收`Context`。它们之间的函数调用链必须传递`Context`，或者可以使用`WithCancel`，`WithDeadline`，`WithTimeout`，`WithValue`创建派生上下文。==**当一个`Context`被取消时，它派生的所有`Context`也被取消**==。



## context接口

`context.Context`是一个接口，该接口定义了四个需要实现的方法。具体签名如下：

```go
type Context interface{
    Deadline()(deadline time.Time,ok bool)
    Done() <-chan struct{}
    Err() error
    Value( key interface{} ) interface{}
}
```

其中：

+ `Deadline`方法需要**返回当前`Context`被取消的时间**，也就是完成工作的截止时间（deadline）
+ `Done`方法需要返回一个通知子goroutine退出的`Channel`，这个Channel会在当前工作完成或者上下文被取消之后关闭，多次调用`Done`方法返回同一个Channel
+ `Err`方法**返回当前`Context`结束的原因**，它只会在`Done`返回的Channel被关闭时才会返回非空的值
  + 如果当前`Context`被取消就会返回`Canceled`错误；
  + 如果当前`Context`超时就会返回`DeadlineExceeded`错误；
+ `Value`方法会**从`Context`中返回键对应的值**，对于同一个上下文来说，多次调用`Value`并传入相同的`key`会返回相同的结果，该方法仅用于传递API和进程间跟请求域的数据；



### Background()和TODO()

Go context 内置两个方法：`Background()`和`TODO()`，这两个函数分别返回一个实现了`Context`接口的`background`和`todo`。我们代码中最开始都是以这两个内置上下文对象作为最顶层的`parent context`，衍生出更多的子上下文对象。

+ `Background()`主要用于main函数，初始化以及测试代码中，作为Context这个树结构的最顶层的Context，也就是根Context
+ `TODO()`，它目前还不知道具体的使用场景，如果我们不知道该使用什么Context的时候，可以使用这个。
+ `background`和`todo`本质上都是`emptCtx`结构体类型，是一个不可取消，没有设置截止时间，没有携带任何值的Context



## With系列函数

`context`中还定义四个With系列函数

### WithCancel

`WithCancel`的函数签名如下：

```go
func WithCancel(parent Context)(ctx Context,cancel CancelFunc)
```

`WithCancel`返回一个带有新Done通道的父节点副本。当调用返回的cancel函数或当关闭父上下文的Done通道时，将关闭返回上下文的Done通道，无论先发生什么情况。



取消此上下文将释放与其关联的资源，因此代码中应该在此上下文中进行操作完成后立即调用cancel。

```go
func gen(ctx context.Context)<-chan int{
    dst:=make(chan int)
    n:=1
    go func(){
        for {
            select {
                case <-ctx.Done():
                	return //结束该goroutine，防止泄露
                case dst<-n:
                	n++
            }
        }
    }()
    return dst
}


func main(){
    ctx,cancel:=context.WithCancel(context.Background())
    defer cancel() //当我们取完整的整数后调用cancel
    
    for n:=range gen(ctx){
        fmt.Println(n)
        if n==5{
            break
        }
    }
}
```

上面的示例代码中，`gen`函数在单独的goroutine中生成整数并将它们发送到返回的通道。`gen`的调用者在使用生成的整数之后取消上下文，以免`gen`启动的内部`goroutine`发生泄露。





### WithDeadline

`WithDeadline`的函数签名如下：

```go
func WithDeadline(parent Context,deadline time.Time)(Context,CancelFunc)

//两种方式：
//1，等到deadline时间时，ctx会自动c过期，ctx.Done()中就有值了
//2，可以手动调用cancel函数

//即：可以利用cancle()手动退出，也可以利用deadline的时间设置，到deadline时间会自动退出
```

返回父上下文的副本，并将deadline调正为不迟于d。如果父上下文的deadline已经早于d，则WithDeadline(parent,d)在语义上等同于父上下文。当截止日期过期时，当调用返回的cancel函数时，或者当父上下文的Done通道关闭时，返回上下文的Done通道将被关闭，以最先发生的情况为准。



取消此上下文将释放与其关联的资源，因此代码应该在上下文中运行的操作完成后立即调用cancel。

```go
func main(){
    d:=time.Now().Add(50*time.Millisecond)
    ctx,cancel:=context.WithDeadline(context.Background(),d)
    //尽管ctx会过期，但是在任何情况下调用它的cancel函数都是很好的实践
    //如果不这样做，可能会使上下文及其父类存活的时间超过必要的时间
    defer cancel()
    
    select{
        case <-time.After(1*time.Second):  //time.After(time.Duration)只向通道发送一个值
	        fmt.Println("overslept")
        case <-ctx.Done():
    	    fmt.Println(ctx.Err())
    }
}

```

上面的代码中，定义了一个50毫秒之后过期的deadline，然后我们调用`context.WithDeadline(context.Background(),d)`得到一个`ctx,cancel`然后使用一个select让主程序陷入等待：等待1秒后打印`overslept`退出或者等待ctx过期后退出。

在上面的示例代码中，因为ctx 50毫秒后就会过期，所以`ctx.Done()`会接收到context到期通知，并会打印`ctx.Err()`的内容。





### WithTimeout

`WithTimeout`的函数签名如下：

```go
func WithTimeout(parent Context,timeout time.Duration)(Context,CancelFunc)

//说明；
//1，可以利用cancel()手动退出
//2，可以等过了timeout时间后，子goroutine会自动退出。

//withDeadline：是设置退出时间点
//withTimeout：是设置需要多长时间退出，是时间周期
```

`WithTimeout`返回`WithDeadline(parent,time.Now().Add(timeout))`。

取消上下文将释放与其相关资源，因为代码应该在此上下文中运行的操作完成后立即调用cancel，通常用于数据库或者网络连接超时控制。

```go
package main

import(
	"context"
    "fmt"
    "sync"
    "time"
)

var wg sync.WaitGroup

func worker(ctx context.Context){
    Loop:
    for{
        fmt.Println('db connecting ...')
        time.Sleep(time.Millisecond *10) //假设正常连接数据库耗时10毫秒
        select{
            case <-ctx.Done(): //50秒后自动调用
          	  break Loop
            default:
        }
    }
    fmt.Println("worker done!")
    wg.Done()
}


func main(){
    //设置一个50毫秒的超时
    ctx,cancel:=context.WithTimeout(context.Background(),time.Millisecond*50)
    wg.Add(1)
    go worker(ctx)
    time.Sleep(time.Second*5)
    cancel()  //通知子goroutine结束
    wg.Wait()
    fmt.Println("over")
}
```





### WithValue

`WithValue`函数能够将请求作用域的数据与Context对象建立关系。声明如下：

```go
func WithValue(parent Context,key ,value interface{}) Context

//说明：
//1，该函数没有cancel(),无法手动退出
//2，该函数目的是利用context，传递值，即在context保存了一个键值对，子goroutine中的context可以获取该值
```

`WithValue`返回父节点的副本，其中与key关联的值为val。



仅对API和进程间传递请求域的数据使用上下文值，而不是使用它来传递可选参数给函数。

所提供的键必须是可比较的，并且不应该是`string`类型或任何其他内置类型，以避免使用上下文在包之间发生冲突。`WithValue`的用户应该为键定义自己的类型。为了避免在分配给`interface{}`时进行分配，上下文键通常具有具体类型`struct{}`。或者，导出的上下文关键变量的静态类型应该是指针或接口。

```go
package main

import(
	"context"
    "fmt"
    "sync"
    "time"
)
//context.WithValue

type TraceCode string  //造一个类型
var wg sync.WaitGroup

func worker(ctx context.Context){
    key:=TraceCode("TRACE_CODE")
    traceCode,ok:=ctx.Value(key).(string) //在子goroutine中获取trace code
    if !ok{
        fmt.Println("worker,trace code:%s\n",traceCode)
    }
    Loop:
    for{
        fmt.Printf("worker,trace code:%s\n",traceCode)
        time.Sleep(time.Millisecond*10)
        select{
            case <-ctx.Done(): //假设正常连接数据库耗时10毫秒
            	break Loop
            default:
        }
    }
    fmt.Println("worker done!")
    wg.Done()
}

func main(){
    //设置一个50毫秒的超时
    ctx,cancel:=context.WithTimeout(context.Background(),time.Millisecond*50)
    //在系统的入口中设置trace code 传递给后续启动的goroutine实现日志数据聚合
    ctx=context.WithValue(ctx,TraceCode("TRACE_CODE"),"12512312234")
    wg.Add(1)
    go worker(ctx)
    time.Sleep(time.Second*5)
    cancel() //通知子goroutine结束
    wg.Wait()
    fmt.Println("over")
}
```







## 使用context的注意事项

+ 推荐以参数的方式显示传递Context
+ **以Context作为参数的函数方法，应该把Context作为第一个参数**
+ **给一个函数方法传递Context的时候，不要传递nil，如果不知道传递什么，就使用context.TODO()**
+ Context的Value相关方法应该传递请求域的必要数据，不应该用于传递可选参数
+ **Context是线程安全的，可以放心在多个goroutine中传递**









## 客户端超时取消示例

调用服务器API时如何在客户端实现超时控制？

### server端

```go
package main

import(
	"fmt"
    "math/rand"
    "net/http"
    "time"
)

//server端，随机出现慢响应

func indexHandler(w http.ResponseWriter,r *http.Request){
    number:=rand.Intn(2)
    if number ==0{
        time.Sleep(time.Second*20)
        fmt.Fprintf(w,"slow response")
        return
    }
    fmt.Println(w,"quick response")
}

func main(){
    http.HandFunc("/",indexHandler)
    err:=http.ListenAndServer(":8080",nil)
    if err!=nil{
        panic(err)
    }
}
```





### client端

```go
package main

import(
	"context"
    "fmt"
    "io/ioutil"
    "net/http"
    "sync"
    "time"
)

//client

type respData struct{
    resp *http.Response
    err error
}

func doCall(ctx context.Context){
    transport := http.Transport{
        //请求频繁可定义全局的client对象并启用长链接
        //请求不频繁使用的短链接
        DisableKeepAlives:true,
    }
    
    client:=http.Client{
        Transport: &transport,
    }
    
    respChan := make(chan *respData,1)
    req,err:=http.NewRequest("GET","http://127.0.0.1:8080/",nil)
    if err!=nil{
        fmt.Printf("new request failed,err:%v\n",err)
        return
    }
    req = req.WithContext(ctx) //使用带超时的ctx创建一个新的client request
    var wg sync.WaitGroup
    wg.Add(1)
    defer wg.Wait()
    go func(){
        resp,err:=client.Do(req)
        fmt.Printf("client.do resp:%v,err:%v\n",resp,err)
        rd:=&respData{
            resp:resp,
            err:err,
        }
        respChan <- rd
    	wg.Done()
    }()
    select{
        case <-ctx.Done():
        fmt.Println("call api timeout")
    case result:=<-respChan:
        fmt.Println("call server api success")
        if result.err!=nil{
            fmt.Printf("call server api failed,err:%v\n",err)
            return
        }
        defer result.resp.Body.Close()
        data,_:=ioutil.ReadAll(result.resp.Body)
        fmt.Printf("resp:%v\n",string(data))
    }
}

```





# 数据库相关

数据库：它其实一个软件，包含了查询数据、存储数据等功能。数据库可以从磁盘上执行一些读取数据、更新数据等操作。

常见的数据库：SQLlite，MySql，SQLServer，postgreSQl，Oracle



## Go操作MySQL———database/sql使用指南

### 连接

Go语言中的`database/sql`包提供了保证SQL或类SQL数据库的泛接口，并不提供具体的数据库驱动。使用`data/sql`包时必须注入（至少）一个数据库驱动。`data/sql`库原生支持连接池，是并发安全的，该库没有具体的实现，只是列出了一些需要第三方库实现的具体内容。

我们常用的数据库基本都有完整的第三方实现。例如：[MySQL驱动](https://github.com/go-sql-driver/mysql)

#### 下载依赖

```go
go get -u github.com/go-sql-driver/mysql

//go get 包的路径就是下载第三方的依赖，将第三方依赖默认保存在$GOPATH/src/
```



#### 使用MySQL驱动

```go
package main

import (
	"database/sql"
	"fmt"

	_ "github.com/go-sql-driver/mysql"  //即执行了该包中的init函数
)

//go 连接mysql数据库

func main(){
	//数据库信息  DSN:Data Source Name
	dsn:="root:123456@tcp(127.0.0.1:3306)/bjpowernode"
	//连接数据库
	//Open函数可能只是验证其参数格式是否正确，实际上并不创建与数据库的连接。
	//如果要检查数据源的名称是否真实有效，应该调用Ping方法。

	db,err:=sql.Open("mysql",dsn)  //不会校验用户名和密码是否正确
	if err!=nil {  //dsn格式不正确时会报错
		fmt.Printf("dsn:%s invalid,err:%v\n",dsn,err)
		return
	}

	//返回的DB对象可以安全地被多个goroutine并发使用，并且维护其自己的空闲连接池。
	//因此，Open函数应该仅被调用一次，很少需要关闭这个DB对象。

	defer db.Close()

	//使用Ping方法来检查数据源的名称是否真实有效
	err=db.Ping()  //尝试连接数据库
	if err!=nil{
		fmt.Printf("open %s failed,err:%v\n",dsn,err) 
		return
	}
	fmt.Println("连接数据库成功...")
}
```



第三方依赖`github.com/go-driver-sql/mysql`中的init函数

![image-20221229184043618](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221229184043618.png)



#### 初始化连接

==**注意事项**==：

+ `sql.Open()`：该函数只是验证参数格式是否正确，实际上并不会创建与数据库的连接。

+ `sql.Ping()`：该函数用来检查用户名和密码是否正确，来尝试连接数据库

+ 返回的`db`对象可以安全的被多个goroutine并发使用，并且维护其自己的空闲连接池。因此，Open函数应该被使用一次，很少要关闭这个db对象。

  



可以通过定义一个全局变量`db`，用来保存数据库连接对象。可以创建一个`initDB()error`函数，在程序启动时调用一次该函数完成全局变量的db的初始化，其他函数就可以直接使用全局变量`db`。

```go
func initDB()(err error) {
	//数据库信息  DSN:Data Source Name
	dsn := "root:123456@tcp(127.0.0.1:3306)/bjpowernode"
	//连接数据库
	//Open函数可能只是验证其参数格式是否正确，实际上并不创建与数据库的连接。
	//如果要检查数据源的名称是否真实有效，应该调用Ping方法。

	db, err = sql.Open("mysql", dsn) //不会校验用户名和密码是否正确
	if err != nil {                   //dsn格式不正确时会报错
		return
	}

	//使用Ping方法来检查数据源的名称是否真实有效
	err = db.Ping() //尝试连接数据库
	if err != nil {
		return
	}
	return 
}
```

==注意：`sql.DB`是表示连接的数据库对象，保存了连接数据库相关的所有信息。它内部维护了一个具有零到多个底层连接的连接池，它可以安全地被多个goroutine同时使用。==



#### SetMaxOpenConns

```go
func (db *DB)SetMaxOpenConns(n int)
//设置与数据库建立连接的最大数目。如果 0<n<最大闲置连接数，此时会将最大闲置数减少到匹配最大开启连接数的限制。 如果 n<=0, 不会限制最大开启连接数，默认为0（无限制）。
```



#### SetMaxIdleConns

```go
func (db *DB)SetMaxIdleConns(n int)
//SetMaxIdleConns设置连接池中的最大闲置连接数。如果 最大开启连接数<n ，则新的最大闲置连接数会减少到匹配到最大开启连接数的限制。如果 n<=0，不会保留闲置连接。
```



### CRUD

#### 键库建表

```go
//mysql中创建数据库
create database sql_test;
//使用该数据库
use sql_test;
//创建表
create table user(
    id bigint(20) not null auto_increment,
    name varchar(32) default '',
    age int(11) default 10,
    primary key(id)
    )engine = InnoDB auto_increment =1 default charset = utf8mb4;
```



#### 查询

```go
type user struct{
    id int
    age int
    name string
}
```

##### 单行查询

单行查询`db.QueryRow()`执行一次查询，并期望返回最多一行结果（即Row）。QueryRow总是返回非nil的值，知道返回值的Scan方法被调用时，才会返回被延迟的错误。（如未找到结果）

```go
func (db *DB)QueryRow(query string,args ...interface{})*Row
```



```go
func querOne(id int) {
	//1，写查询sql语句
	sqlStr := `select id,name,age from user where id = ?` //?占位符
	//2，执行
	rowObj := db.QueryRow(sqlStr, id) //从连接池中取出一个连接到数据库中查询单挑记录
	//3，获取结果
	var u1 user
	//func (*sql.Row).Scan(dest ...interface{}) error 该方法中包含了释放连接数据库操作
	err := rowObj.Scan(&u1.id, &u1.name, &u1.age)
	if err != nil {
		fmt.Printf("err:%v\n", err)
		return
	}
	//4，打印结果
	fmt.Printf("u1:%#v\n", u1)
}
```





##### 多行查询

多行查询`db.Query()`执行一次查询，返回多行结果（即Rows），一般用于执行select命令。参数args表示query中的占位参数。

```go
func (db *DB)Query (query string,args ...interface{})(*Rows,error)
```



```go
func queryMore(n int) {
	//1，sql语句
	sqlStr := `select id,name,age from user where id > ?;`
	//2，执行
	rows, err := db.Query(sqlStr, n)
	if err != nil {
		fmt.Printf("exec %s query failed,err:%v\n", sqlStr, err)
		return
	}
	//3,关闭rows
	defer rows.Close()

	//4,循环取值
	for rows.Next() {
		var u1 user
		//注意：func (*sql.Rows).Scan(dest ...interface{}) error 该方法中没有释放连接数据库操作，需手动
		err := rows.Scan(&u1.id, &u1.name, &u1.age)
		if err != nil {
			fmt.Printf("scan failed,err:%v\n", err)
			return
		}
		fmt.Printf("u1:%#v\n", u1)
	}
}
```





#### 插入数据

插入、更新和删除操作都是使用`Exec`方法，`Exec`方法执行一次命令（包括 删除、更新、插入等），返回的Result是对已执行的SQL命令的总结。参数args表示query中的占位参数。

```go
func (db *DB)Exec(query string,args ...interface{})(Result,error)
```



```go
func insert() {
	//写sql语句
	sqlStr := `insert into user(name,age) values('wangwu',22);`
	ret,err:=db.Exec(sqlStr)
	if err!=nil {
		fmt.Printf("insert failed,err:%v\n",err)
		return
	}
	//如果是插入数据操作，则可以拿到插入数据的id
	id,err:=ret.LastInsertId()  //返回一个数据库生成的回应命令的整数。  // 当插入新行时，一般来自一个"自增"列。
	if err!=nil {
		fmt.Printf("get id failed,err:%v\n",err)
		return
	}
	fmt.Println("id:",id)
}
```





#### 更新数据

```go
func updateRow(newAge int,id int){
	sqlStr:=`update user set age = ? where id= ? ;`
	ret,err:=db.Exec(sqlStr,newAge,id)
	if err!=nil {
		fmt.Printf("update failed,err:%v\n",err)
		return
	}
	n,err:=ret.RowsAffected() //返回被update、insert或delete命令影响的行数
	if err!=nil {
		fmt.Printf("rowsAffected,err:%v\n",err)
		return
	}
	fmt.Printf("更新了%v行\n",n)
}
```





#### 删除数据

```go
func deleteRow(id int){
	sqlStr := `delete from user where id = ?`
	ret,err:=db.Exec(sqlStr,id)
	if err != nil {
		fmt.Printf("delete failed,err:%v\n",err)
		return
	}
	n,err:=ret.RowsAffected()
	if err!=nil {
		fmt.Printf("rowsAffected,err:%v\n",err)
		return
	}
	fmt.Printf("删除了%d\n",n)
}
```



#### 完整实例

```go
package main

import (
	"database/sql"
	"fmt"

	_ "github.com/go-sql-driver/mysql"
)

var db *sql.DB

func initDB() (err error) {
	//数据库信息  DSN:Data Source Name
	dsn := "root:123456@tcp(127.0.0.1:3306)/sql_test"
	//连接数据库
	//Open函数可能只是验证其参数格式是否正确，实际上并不创建与数据库的连接。
	//如果要检查数据源的名称是否真实有效，应该调用Ping方法。

	db, err = sql.Open("mysql", dsn) //不会校验用户名和密码是否正确
	if err != nil {                  //dsn格式不正确时会报错
		return
	}

	//使用Ping方法来检查数据源的名称是否真实有效
	err = db.Ping() //尝试连接数据库
	if err != nil {
		return
	}
	return
}

type user struct {
	id   int
	name string
	age  int
}

func querOne(id int) {
	//1，写查询sql语句
	sqlStr := `select id,name,age from user where id = ?` //?占位符
	//2，执行
	rowObj := db.QueryRow(sqlStr, id) //从连接池中取出一个连接到数据库中查询单挑记录
	//3，获取结果
	var u1 user
	//func (*sql.Row).Scan(dest ...interface{}) error 该方法中包含了释放连接数据库操作
	err := rowObj.Scan(&u1.id, &u1.name, &u1.age)
	if err != nil {
		fmt.Printf("err:%v\n", err)
		return
	}
	//4，打印结果
	fmt.Printf("u1:%#v\n", u1)
}

func queryMore(n int) {
	//1，sql语句
	sqlStr := `select id,name,age from user where id > ?;`
	//2，执行
	rows, err := db.Query(sqlStr, n)
	if err != nil {
		fmt.Printf("exec %s query failed,err:%v\n", sqlStr, err)
		return
	}
	//3,关闭rows
	defer rows.Close()

	//4,循环取值
	for rows.Next() {
		var u1 user
		//注意：func (*sql.Rows).Scan(dest ...interface{}) error 该方法中没有释放连接数据库操作，需手动
		err := rows.Scan(&u1.id, &u1.name, &u1.age)
		if err != nil {
			fmt.Printf("scan failed,err:%v\n", err)
			return
		}
		fmt.Printf("u1:%#v\n", u1)
	}
}

func insert() {
	//写sql语句
	sqlStr := `insert into user(name,age) values('wangwu',22);`
	ret,err:=db.Exec(sqlStr)
	if err!=nil {
		fmt.Printf("insert failed,err:%v\n",err)
		return
	}
	//如果是插入数据操作，则可以拿到插入数据的id
	id,err:=ret.LastInsertId()
	if err!=nil {
		fmt.Printf("get id failed,err:%v\n",err)
		return
	}
	fmt.Println("id:",id)
}

func updateRow(newAge int,id int){
	sqlStr:=`update user set age = ? where id = ?;`
	ret,err:=db.Exec(sqlStr,newAge,id)
	if err!=nil {
		fmt.Printf("update failed,err:%v\n",err)
		return
	}
	n,err:=ret.RowsAffected()
	if err!=nil {
		fmt.Printf("rowsAffected,err:%v\n",err)
		return
	}
	fmt.Printf("更新了%v行\n",n)
}

func deleteRow(id int){
	sqlStr := `delete from user where id = ?`
	ret,err:=db.Exec(sqlStr,id)
	if err != nil {
		fmt.Printf("delete failed,err:%v\n",err)
		return
	}
	n,err:=ret.RowsAffected()
	if err!=nil {
		fmt.Printf("rowsAffected,err:%v\n",err)
		return
	}
	fmt.Printf("删除了%d\n",n)
}

func main() {
	err := initDB()
	if err != nil {
		fmt.Printf("init DB failed,err:%v\n", err)
	}
	fmt.Println("数据库连接成功！")
	// querOne(1)
	// queryMore(0)
	// insert()
	// updateRow(9000,2)
	deleteRow(3)
}
```







### MySQL预处理

#### 什么是预处理

**普通SQL语句执行过程**

+ 客户端对SQL语句进行占位符替换得到完整的SQL语句。
+ 客户端发送完整SQL语句到MySQL服务端
+ MySQL服务端执行完整的SQL语句并将结果返回客户端



**预处理执行过程**

+ 把SQL语句分成两部分，命令部分和数据部分
+ 先把命令部分发送给MySQL服务端，MySQL服务端进行SQL预处理
+ 然后把数据部分发送给MySQL服务端，MySQL服务端对SQL语句进行占位符替换
+ MySQL服务端执行完整的SQL语句并将结果返回客户端



#### 为什么要预处理

+ 优化MySQL服务器==重复执行SQL方法==，可以提升服务器的性能，提前让服务器编译，一次编译多次执行，节省后续编译的成本。
+ 避免SQL注入问题。



#### Go实现MySQL预处理

`database/sql`中使用下面的`Prepare`方法来实现预处理操作。

```go
//Prepare方法会先将sql语句发送给MySQL服务端，返回一个准备好的状态用于之后的查询和命令。返回值可以同时执行多个查询和命令

func (db *DB)Prepare(query string)(*Stmt,error)
```



```go
//预处理查询示例

func prepareQureyDemo(){
    sqlStr:=`select id,name,age from user where id > ?;`
    stmt,err:=db.Prepare(sqlStr)
    if err!=nil{
        fmt.Printf("prepare failed,err:%v\n",err)
        return
    }
    defer stmt.Close()
    rows,err:=stmt.Query(0)
    if err!=nil{
        fmt.Printf("query failed,err:%v\n",err)
        return
    }
    defer rows.Close()
    for rows.Next(){
        var u user
        err:= rows.Scan(&u.id,&u.name,&u.age)
        if err!=nil{
            fmt.Printf("scan failed,err:%v\n",err)
            return
        }
        fmt.Printf("u:%#v\n",u)
    } 
}
```



插入、更新和删除操作的预处理十分类似。



```go
//插入预处理示例
func prepareInsertDemo(){
    sqlStr:=`insert into user(name,age) values(?,?);`
    stmt.err:=db.Prepare(sqlStr)
    if err!=nil{
        fmt.Printf("prepare failed,err:%v\n",err)
        return
    }
    defer stmt.Close()
    ret,err := stmt.Exec("小王子",123)
    if err!=nil{
        return
    }
    id,err:=ret.LastInsertId()
    if err!=nil{
        fmt.Println("get id failed,err:",err)
        return
    }
    fmt.Println("id:",id)
    ret,err = stmt.Exec("娜扎",19)
    if err!=nil{
        fmt.Printf("insert failed,err:%v\n",err)
        return
    }
    n,err:=ret.RowsAffected()
    if err!=nil{
        fmt.Println("rowsAffected,err:",err)
        return
    }
    fmt.Printf("有%n行发生了变化\n",n)
}
```



#### SQL注入问题

==***注意：任何时候，我们都不应该自己拼接SQL语句***==



```go
//自行拼接sql语句示例，编写一个根据name字段查询user表的函数：
func sqlInjectDemo(name string){
    sqlStr:=fmt.Sprintf("select id,name,age from user where name = %s;",name)
    fmt.Printf("SQL:%s\n",sqlStr)
    var u user
    err:=db.QueryRow(sqlStr).Scan(&u.id,&u.name,&u.age)
    if err!=nil{
        fmt.Printf("exec failed,err:%v\n",err)
        return
    }
    fmt.Printf("user:%#v\n",u)
}
```





**在不同的数据库中，SQL语句使用的占位符语法不尽相同。**

| 数据库     | 占位符用法    |
| ---------- | ------------- |
| MySQL      | `？`          |
| PostgreSQL | `$1` ，`$2`等 |
| SQLite·    | `？`和`$1`    |
| Oracle     | `:name`       |



### Go实现MySQL事务

#### 什么是事务

事务：一个最小的不可再分的工作单元；通常一个事务对应一个完整的业务（例如：银行账户转账业务），同时这个完整的业务需要执行多次DML语句共同联合完成。A转账B，这里就需要执行两次update操作。



在MySQL中只有使用了`InnoDB`数据库引擎的数据库或表才支持事务。事务处理可以用来维护数据库的完整性，保证成批的SQL语句要么全部执行，要么全部不执行。



#### 事务ACID

事务通常要满足四个条件（ACID）：原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）、持久性（Durability）。

|  条件  |                             解释                             |
| :----: | :----------------------------------------------------------: |
| 原子性 | 一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。 |
| 一致性 | 在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。 |
| 隔离性 | 数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）。 |
| 持久性 | 事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。 |



#### 事务相关方法

go语言中使用以下三个方法来实现MySQL中的事务操作。

```go
//开始事务
func (db *DB)Begin()(*Tx,error)
//提交事务
func (tx *Tx)Commit()error
//事务回滚
func (tx *Tx)Rollback()error
```



#### 事务实例

下面代码演示一个简单的事务操作，该事务操作能够保证两次更新操作要么同时成功，要么同时失败，不存在中间状态。

```go
//事务操作示例

func transactionDemo(){
    tx ,err := db.Begin()
    if err!=nil{
        if tx!=nil{
            tx.Rolback()
        }
        fmt.Printf("begin transaction failed,err:%v\n",err)
        return
    }
    sqlStr1:="update user set age = 30 where id = ?;"
    ret1,err:=tx.Exec(sqlStr1,2)
    if err!=nil{
        tx.Rollback()
        fmt.Printf("exec sql1 failed,err:%v\n",err)
        return
    }
    affRow1,err:=ret1.RowsAffected()
    if err!=nil{
        tx.Rollback()
        fmt.Printf("exec ret1.RowAffected() failed,err:%v\n",err)
        return
    }
    sqlStr2 := "update user set age = 40 where id = ?;"
    ret2,err:=tx.Exec(sqlStr2,4)
    if err!=nil{
        tx.Rollback()
        fmt.Printf("exec sql2 failed,err:%v\n",err)
        return
    }
    affRow2,err:=ret2.RowsAffected()
    if err!=nil{
        tx.Rollback()
        fmt.Printf("exec ret2.RowAffected() failed,err:%v\n",err)
        return
    }
    fmt.Println(affRow1,affRow2)
    if affRow1==1&&affRow2==1{
        fmt.Println("事务提交...")
        tx.Commit()
    }else{
        tx.Rollback()
        fmt.Println("事务回滚...")
    }
    fmt.Println("exec trans success!")
}
```





## 更强大、更好用的sqlx库使用指南

在项目中，通常可能会使用`database/sql`连接MySQL数据库。本文介绍`sqlx`实现批量插入数据的例子，并介绍了`sqlx.In`和`DB.NameExec`方法。



### sqlx介绍

在项目中，通常使用`database/sql`连接MySQL数据库。`sqlx`可以认为是Go语言内置的`database/sql`的超集，它在`database/sql`基础上提供了一组扩展。这些扩展中除了常用的查询`Get(dest interface{}, ...)error`和`Select(dest,interface{}, ...)error`外还有很多强大的功能。

### 安装sqlx

```go
go get github.com/jmoiron/sqlx
```



### sqlx基本使用

#### 连接数据库

```go
var db *sqlx.DB

func initDB()(err error){
    dsn:="root:123456@tcp(127.0.0.1)/sql_test?charset=utf8mb4&parseTime=True"
    //Connect方法会检测用户名和密码是否正确
    db,err:=sqlx.Connect("mysql",dsn)
    if err!=nil{
        fmt.Printf("connect DB failed,err:%v\n",err)
        return
    }
    //设置数据库连接池最大连接数
    db.SetMaxOpenConns(20)
    //设置最大空闲连接数
    db.SetMaxIdleConns(10)
}
```



#### 查询

```go
//单行查询
func queryRowDemo(){
    sqlStr:=`select id,name,age from user where id = ?;`
    var u user
    //Get方法用于单行查询
    err:=db.Get(&u,sqlStr,1)
    if err!=nil{
        fmt.Printf("get failed,err:%v\n",err)
        return
    }
    fmt.Printf("u:%#v\n",u)
}

//多行查询
func queryMultiRowDemo(){
    sqlStr:=`select id,name,age from user where id > ?;`
    var users []user
    err:= db.Select(&users,sqlStr,0)
    if err!=nil{
        fmt.Printf("query failed,err:%v\n",err)
        return
    }
    fmt.Printf("users:%#v\n",users)
}
```



#### 插入、更新和删除

sqlx中的exec方法与原生的sql中的exec方法使用基本一致

```go
//插入数据
func insertRowDemo(){
    sqlStr:="insert into user(name,age)values(?,?);"
    ret,err:=db.Exec(sqlStr,"二哈",2)
    if err!=nil{
        fmt.Printf("insert failed,err:%v\n",err)
        return
    }
    theID,err:= ret.LastInsertId() //获取新插入数据的id
    if err!=nil{
        fmt.Printf("get lastinsert ID failed,err:%v\n",err)
        return
    }
    fmt.Printf("insert success,the id is %d.\n",theID)
}

//更新数据
func updateDemo(){
    sqlStr:="update user set age = ? where id = ?"
    ret ,err:=db.Exec(sqlStr,39,1)
    if err!=nil{
        fmt.Printf("update failed,err:%v\n",err)
        return
    }
    n,err:=ret.RowAffected() //操作影响的行数
    if err!=nil{
        fmt.Printf("get RowsAffected failed,err:%v\n",n)
    }
}

//删除数据
func deleteRowDemo(){
    sqlStr:="delete from user where id = ?"
    ret ,err:= db.Exec(sqlStr,0)   //增删改都是使用Exec方法
    if err!=nil{
        fmt.Printf("delete failed,err:%v\n",err)
        return
    }
    n,err:=ret.RowsAffected() 
    if err!=nil{
        fmt.Printf("get RowsAffected failed,err:%v\n",err)
        return
    }
    fmt.Printf("delete success,affected rows:%d\n",n)
}
```



#### NameExec

`DB.NameExec`方法用来绑定SQL语句与结构体或map中的同名字段

```go
func insertUserDemo()(err error){
    sqlStr:="insert into user(name,age)values(:name,:age)"
    _,err:=db.NameExec(sqlStr,map[string]interface{}{
        "name":"柯基",
        "age":28,
    })
    return
}
```



#### NamedQuery

与`DB.NameExec`同理，支持查询

```go
func nameQuery(){
    sqlStr:=`select * from user where name = :name`
    //使用map做命名查询
    rows,err:=db.NameQuery(sqlStr,map[string]interface{}{"name":"qimi"})
    if err!=nil{
        fmt.Printf("db.NameQuery failed,err:%v\n",err)
        return
    }
    defer rows.Close()
    
    for rows.Next(){
        var u user
        err:=rows.StructScan(&u)
        if err!=nil{
            fmt.Printf("scan failed,err:%v\n",err)
            continue
        }
        fmt.Printf("user:%#v\n",u)
    }
    u:=user{
        Name:"七米",
    }
    //使用结构体命名查询，根据结构体字段的db tag 进行映射
    rows,err = db.NameQuery(sqlStr,u)
    if err!=nil{
        fmt.Printf("db.NameQuery failed,err:%v\n",err)
        return
    }
    defer rows.Close()
    for rows.Next(){
        var u user
        err:=rows.StructScan(&u)
        if err!=nil{
            fmt.Printf("scan failed,err:%v\n",err)
            continue
        }
        fmt.Printf("user:%#v\n",u)
    }
}
```



#### 事务

对于事务操作，我们可以使用`sqlx`中提供的`db.Beginx()`和`tx.Exec()`方法。示例代码如下：

```go
func transactionDemo2()(err error) {
	tx, err := db.Beginx() // 开启事务
	if err != nil {
		fmt.Printf("begin trans failed, err:%v\n", err)
		return err
	}
	defer func() {
		if p := recover(); p != nil {
			tx.Rollback()
			panic(p) // re-throw panic after Rollback
		} else if err != nil {
			fmt.Println("rollback")
			tx.Rollback() // err is non-nil; don't change it
		} else {
			err = tx.Commit() // err is nil; if Commit returns error update err
			fmt.Println("commit")
		}
	}()

	sqlStr1 := "Update user set age=20 where id=?"

	rs, err := tx.Exec(sqlStr1, 1)
	if err!= nil{
		return err
	}
	n, err := rs.RowsAffected()
	if err != nil {
		return err
	}
	if n != 1 {
		return errors.New("exec sqlStr1 failed")
	}
	sqlStr2 := "Update user set age=50 where i=?"
	rs, err = tx.Exec(sqlStr2, 5)
	if err!=nil{
		return err
	}
	n, err = rs.RowsAffected()
	if err != nil {
		return err
	}
	if n != 1 {
		return errors.New("exec sqlStr1 failed")
	}
	return err
}
```



### sqlx.In

`sqlx.In`是`sqlx`提供的一个非常方便的函数。



#### 前置条件



##### 表结构

为了方便演示插入数据操作，这里创建一个`user`表，表结构如下：

```sql
CREATE TABLE `user` (
    `id` BIGINT(20) NOT NULL AUTO_INCREMENT,
    `name` VARCHAR(20) DEFAULT '',
    `age` INT(11) DEFAULT '0',
    PRIMARY KEY(`id`)
)ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4;
```



##### 结构体

定义一个`user`结构体，字段通过tag与数据库中user表的列一致。

```go
type User struct {
	Name string `db:"name"`
	Age  int    `db:"age"`
}
```



##### bindvars（绑定变量）

查询占位符`?`在内部称为**bindvars（查询占位符）**,它非常重要。你应该始终使用它们向数据库发送值，因为它们可以防止SQL注入攻击。`database/sq`不尝试对查询文本进行任何验证；它与编码的参数一起按原样发送到服务器。除非驱动程序实现一个特殊的接口，否则在执行之前，查询是在服务器上准备的。因此`bindvars`是特定于数据库的:

- MySQL中使用`?`
- PostgreSQL使用枚举的`$1`、`$2`等bindvar语法
- SQLite中`?`和`$1`的语法都支持
- Oracle中使用`:name`的语法

其他数据库可能有所不同。可以使用`sqlx.DB.Rebind(string) string`函数和`?`的bindvar语法去获取适合在当前数据库类型上执行的查询语句。

`bindvars`的一个常见误解是，它们用于在语句中插入值。它们仅用于参数化，不允许更改SQL语句的结构。例如，使用`bindvars`尝试参数化列或表名将不起作用：

```go
// ？不能用来插入表名（做SQL语句中表名的占位符）
db.Query("SELECT * FROM ?", "mytable")
 
// ？也不能用来插入列名（做SQL语句中列名的占位符）
db.Query("SELECT ?, ? FROM people", "name", "location")
```



#### 实现批量插入



##### 自己拼接语句实现批量插入

比较笨，但是很好理解。就是有多少个User就拼接多少个`(?, ?)`。

```go
// BatchInsertUsers 自行构造批量插入的语句
func BatchInsertUsers(users []*User) error {
	// 存放 (?, ?) 的slice
	valueStrings := make([]string, 0, len(users))
	// 存放values的slice
	valueArgs := make([]interface{}, 0, len(users) * 2)
	// 遍历users准备相关数据
	for _, u := range users {
		// 此处占位符要与插入值的个数对应
		valueStrings = append(valueStrings, "(?, ?)")
		valueArgs = append(valueArgs, u.Name)
		valueArgs = append(valueArgs, u.Age)
	}
	// 自行拼接要执行的具体语句
	stmt := fmt.Sprintf("INSERT INTO user (name, age) VALUES %s",
		strings.Join(valueStrings, ","))
	_, err := DB.Exec(stmt, valueArgs...)
	return err
}
```



##### 使用sqlx.In实现批量插入

前提是需要我们的结构体实现`driver.Valuer`接口：

```go
func (u User) Value() (driver.Value, error) {
	return []interface{}{u.Name, u.Age}, nil
}
```

使用`sqlx.In`实现批量插入代码如下：

```go
// BatchInsertUsers2 使用sqlx.In帮我们拼接语句和参数, 注意传入的参数是[]interface{}
func BatchInsertUsers2(users []interface{}) error {
	query, args, _ := sqlx.In(
		"INSERT INTO user (name, age) VALUES (?), (?), (?)",
		users..., // 如果arg实现了 driver.Valuer, sqlx.In 会通过调用 Value()来展开它
	)
	fmt.Println(query) // 查看生成的querystring
	fmt.Println(args)  // 查看生成的args
	_, err := DB.Exec(query, args...)
	return err
}
```



##### 使用NameExec实现批量插入

**==注意==** ：该功能目前有人已经推了[#285 PR](https://github.com/jmoiron/sqlx/pull/285)，但是作者还没有发`release`，所以想要使用下面的方法实现批量插入需要暂时使用`master`分支的代码：

在项目目录下执行以下命令下载并使用`master`分支代码：

```bash
go get github.com/jmoiron/sqlx@master
```

使用`NamedExec`实现批量插入的代码如下：

```go
// BatchInsertUsers3 使用NamedExec实现批量插入
func BatchInsertUsers3(users []*User) error {
	_, err := DB.NamedExec("INSERT INTO user (name, age) VALUES (:name, :age)", users)
	return err
}
```

把上面三种方法综合起来试一下：

```go
func main() {
	err := initDB()
	if err != nil {
		panic(err)
	}
	defer DB.Close()
	u1 := User{Name: "七米", Age: 18}
	u2 := User{Name: "q1mi", Age: 28}
	u3 := User{Name: "小王子", Age: 38}

	// 方法1
	users := []*User{&u1, &u2, &u3}
	err = BatchInsertUsers(users)
	if err != nil {
		fmt.Printf("BatchInsertUsers failed, err:%v\n", err)
	}

	// 方法2
	users2 := []interface{}{u1, u2, u3}
	err = BatchInsertUsers2(users2)
	if err != nil {
		fmt.Printf("BatchInsertUsers2 failed, err:%v\n", err)
	}

	// 方法3
	users3 := []*User{&u1, &u2, &u3}
	err = BatchInsertUsers3(users3)
	if err != nil {
		fmt.Printf("BatchInsertUsers3 failed, err:%v\n", err)
	}
}
```



#### sqlx.In的查询示例

关于`sqlx.In`这里再补充一个用法，在`sqlx`查询语句中实现In查询和FIND_IN_SET函数。即实现`SELECT * FROM user WHERE id in (3, 2, 1);`和`SELECT * FROM user WHERE id in (3, 2, 1) ORDER BY FIND_IN_SET(id, '3,2,1');`。



##### in查询

查询id在给定id集合中的数据。

```go
// QueryByIDs 根据给定ID查询
func QueryByIDs(ids []int)(users []User, err error){
	// 动态填充id
	query, args, err := sqlx.In("SELECT name, age FROM user WHERE id IN (?)", ids)
	if err != nil {
		return
	}
	// sqlx.In 返回带 `?` bindvar的查询语句, 我们使用Rebind()重新绑定它
	query = DB.Rebind(query)

	err = DB.Select(&users, query, args...)
	return
}
```



##### in查询和FIND_IN-SET函数

查询id在给定id集合的数据并维持给定id集合的顺序。

```go
// QueryAndOrderByIDs 按照指定id查询并维护顺序
func QueryAndOrderByIDs(ids []int)(users []User, err error){
	// 动态填充id
	strIDs := make([]string, 0, len(ids))
	for _, id := range ids {
		strIDs = append(strIDs, fmt.Sprintf("%d", id))
	}
	query, args, err := sqlx.In("SELECT name, age FROM user WHERE id IN (?) ORDER BY FIND_IN_SET(id, ?)", ids, strings.Join(strIDs, ","))
	if err != nil {
		return
	}

	// sqlx.In 返回带 `?` bindvar的查询语句, 我们使用Rebind()重新绑定它
	query = DB.Rebind(query)

	err = DB.Select(&users, query, args...)
	return
}
```

当然，在这个例子里面你也可以先使用`IN`查询，然后通过代码按给定的ids对查询结果进行排序。

参考链接：

[Illustrated guide to SQLX](http://jmoiron.github.io/sqlx/)





## Go操作Redis--go-redis库使用指南

### Redis介绍

redis是一种开源的内存数据库，redis提供了多种不同的类型的数据结构，在很多业务场景下可以自然地映射到这些数据结构上。除此之外，通过复制、持久化和客户端分片等特性，可以很方便地将redis 扩展成一个包含数百GB数据、每秒处理上百万次请求地系统。



#### Redis支持的数据类型

Redis支持：字符串string、哈希hash、列表list、集合set、有序结合zset、bitmaps、hyperloglog、带半径范围的地理空间索引（geospatial index）和流（stream）等数据结构。



#### Redis应用场景

+ 缓存系统，减轻主数据库（mysql）的压力
+ 计数场景，比如微博、抖音中的关注数和粉丝数
+ 热门排行榜，需要排序的场景特别适合使用zset
+ 利用list可以实现队列功能
+ 利用hyperloglog统计UV、PV等数据
+ 使用geospatial index 进行地理位置相关查询



#### 准备Redis环境

+ 本机安装redis
+ docker容器安装redis



### go-redis库

#### 安装

Go 社区中目前有很多成熟的 redis client 库，比如[https://github.com/gomodule/redigo 和https://github.com/go-redis/redis，读者可以自行选择适合自己的库。本书使用 go-redis 这个库来操作 Redis 数据库。

使用以下命令安装go-redis库

```go
go get github.com/go-redis/redis/v8
```



#### 连接

##### 普通模式连接

go-redis库中使用redis.NewClient函数连接redis服务器。

```go
rdb := redis.NewClient(&redis.Options{
    Addr:"127.0.0.1:6379",
    Password:"",
    DB:0,
    PoolSize:20,
})
```

除此之外，可以使用redis.ParseURL函数从表示数据源的字符串中解析得到Redis服务器的配置信息。

```go
opt,err:=redis.ParseURL("redis://<user>:<pass>@localhost:6379/<db>")
if err!=nil{
    panic(err)
}
rdb:=redis.NewClient(opt)
```

##### TLS连接模式

使用TLS连接方式，需要使用tls.Config配置。

```go
rdb:=redis.NewClient(&redis.Options{
    TLSConfig:&tls.Config{
        MinVersion:tls.VersionTLS12,
    }
})
```

##### Sentinel模式

使用下面命令连接到由redis sentinel 管理的redis服务器

```go
rdb := redis.NewFailoverClient(&redis.FailoverOptions{
    MasterName:"master-name",
    SentinelAddrs:[]string{":9126",":9127",":9128"}
})
```

##### redis cluster 模式

使用下面命令连接到Redis Cluster，go-redis支持按延迟或随机路由命令

```go
rdb := redis.NewClusterClient(&redis.ClusterOptions{
    Addrs:[]string{":7000",":7001",":7002",":7003",":7004",":7005"},
    //若需要根据延迟或随机路由命令，请启用以下命令之一
    //RouteByLatency:true,
    //RouteRandomly:true,
})
```







### 基本使用

#### 执行命令

下面的示例代码演示了 go-redis 库的基本使用。

```go
func doCommand(){
	ctx,cancel := context.WithTimeout(context.Background(),500*time.Millisecond)
	defer cancel()
	

	//1，执行命令获取结果
	val,err:=rdb.Get(ctx,"name").Result()
	fmt.Println(val,err)

	//2，先获取命令对象，在获取值和错误
	cmder := rdb.Get(ctx,"name")  //获取对象
	fmt.Println(cmder.Val())  //获取值
	fmt.Println(cmder.Err())  //获取错误


	//3，直接执行命令获取错误
	rdb.Set(ctx,"name1","zhangsan",0).Err()

	//4，直接执行命令获取值
	value:=rdb.Get(ctx,"name1").Val()
	fmt.Println(value)
}
```





#### 执行任意命令

go-redis 还提供了一个执行任意命令或自定义命令的 Do 方法，特别是一些 go-redis 库暂时不支持的命令都可以使用该方法执行。具体使用方法如下。

```go
func doDemo(){
	ctx,cancel:=context.WithTimeout(context.Background(),500*time.Millisecond)
	defer cancel()

	//直接执行命令获取错误
	err:=rdb.Do(ctx,"set","name2","wangwu","EX",3600).Err()
	fmt.Println(err)

	//直接执行命令获取结果
	val,err:=rdb.Do(ctx,"get","name2").Result()
	fmt.Println(val,err)
}
```



#### redis.Nil

go-redis库提供了一个redis.Nil错误来表示key不存在的错误。使用go-redis时需要注意对返回错误的判断。在某些场景下，我们应该区别处理redis.Nil和其他不为nil的错误。

```go
//getValueFromRedis  redis.Nil判断

func getValueFromRedis(key,defaultValue string)(string,error){
	ctx,cancel:=context.WithTimeout(context.Background(),500*time.Millisecond)
	defer cancel()
	val,err:=rdb.Get(ctx,key).Result()
	if err!=nil{
		//如果返回的错误是key不存在
		if errors.Is(err,redis.Nil){  //用来判断错误是否为redis.Nil
			return defaultValue,nil
		}
		//其他错误
		return "",err
	}
	return val,nil
}
```



### 其他示例

#### zset示例

示例演示go-redis库操作zset

```go
func zsetDemo() {
	//key
	zsetKey := "language_rank"
	//value
	languages := []redis.Z{
		{Score: 90.0, Member: "golang"},
		{Score: 98.0, Member: "java"},
		{Score: 95.0, Member: "python"},
		{Score: 97.0, Member: "javascript"},
		{Score: 99.0, Member: "c++"},
	}
	ctx, cancel := context.WithTimeout(context.Background(), 500*time.Millisecond)
	defer cancel()

	//zAdd
	err := rdb.ZAdd(ctx, zsetKey, languages...).Err()
	if err != nil {
		fmt.Printf("zadd failed,err:%v\n", err)
		return
	}
	fmt.Println("zadd success")

	//把golang的分数加10
	newScore, err := rdb.ZIncrBy(ctx, zsetKey, 10, "golang").Result()
	if err != nil {
		fmt.Printf("zincrby failed,err:%v\n", err)
		return
	}
	fmt.Printf("golang's score is %f now.\n", newScore)

	//取分数最高的3个
	ret := rdb.ZRevRangeWithScores(ctx, zsetKey, 0, 2).Val()  //按照从大到小的顺序
	for _, z := range ret {
		fmt.Println(z.Member, z.Score)
	}

	//取95~100分的
	op := &redis.ZRangeBy{
		Min: "95",
		Max: "100",
	}
	ret, err = rdb.ZRangeByScoreWithScores(ctx, zsetKey, op).Result() //按照从小到大的顺序
	if err != nil {
		fmt.Printf("zrangebyscore failed,err:%v\n", err)
		return
	}
	for _, z := range ret {
		fmt.Println(z.Member, z.Score)
	}
}


//输出结果：
golang 100
c++ 99
java 98
------------------------
python 95
javascript 97
java 98
c++ 99
golang 100
```



#### 扫描或遍历所有的key

使用`KEYS prefix:*`命令按前缀获取所有的key

```go
vals,err:=rdb.Keys(ctx,"prefix:*").Result()
```

但是如果需要扫描数百万的key，速度就会比较慢。这种场景下，可以使用Scan命令来遍历所有符合要求的key。

```go
//scanKeyDemo1  按前缀查找所有的key示例

func scanKeysDemo1(){
	ctx, cancel := context.WithTimeout(context.Background(), 500*time.Millisecond)
	defer cancel()

	var cursor uint64

	for {
		var keys []string
		var err error
		//按照前缀扫描key
		keys,cursor,err = rdb.Scan(ctx,cursor,"prefix:*",0).Result()
		if err!=nil{
			panic(err)
		}
		for _,key := range keys{
			fmt.Println("key",key)
		}

		if cursor == 0{  //no more keys
			break
		}
	}
}
```

go-redis允许将上面的代码简化为如下示例

```go
//scankeyDemo2 按前缀扫描key示例
func scankeyDemo2(){
	ctx,cancel := context.WithTimeout(context.Background(),500*time.Millisecond)
	defer cancel()

	//按前缀扫描key
	iter:=rdb.Scan(ctx,0,"prefix:*",0).Iterator()
	for iter.Next(ctx){
		fmt.Println("keys",iter.Val())
	}
	if err:=iter.Err();err!=nil{
		panic(err)
	}
}
```

写出一个将所有匹配指定模式的key删除的示例

```go
//按match格式扫描所有的key并删除
func delkeysByMatch(match string,timeout time.Duration){
	ctx,cancel:=context.WithTimeout(context.Background(),timeout)
	defer cancel()

	iter:=rdb.Scan(ctx,0,match,0).Iterator()
	for iter.Next(ctx){
		err:=rdb.Del(ctx,iter.Val()).Err()
		if err!=nil{
			panic(err)
		}
	}

	if err:=iter.Err();err!=nil{
		panic(err)
	}
}
```

此外，对于redis中的set、hash、zset数据类型，go-redis也支持类似的遍历方法

```go
iter:=rdb.SScan(ctx,"set-key",0,"prefix:*",0).Iterator()
iter:=rdb.HScan(ctx,"hash-key",0,"prefix:*",0).Iterator()
iter:=rdb.ZScan(ctx,"sorted-hash-key",0,"prefix:*",0).Iterator()
```



### Pipeline

redis pipeline 允许通过使用单个client-server-client 往返执行多个命令来提高性能。区别于一个接着一个执行100个命令，可以将这些命令放入pipeline中，然后使用1次读写操作像执行一个指令一样执行它们。好处：节省了执行命令的网络往返时间（RTT）。



下面示例代码中演示了使用pipeline通过一个write+read操作来执行多个命令。

```go
pipe := rdb.Pipeline()

incr := pipe.Incr(ctx,"pipeline_counter")
pipe.Expire(ctx,"pipeline_counter",time.Hour)

cmds,err:=pipe.Exec(ctx)
if err!=nil{
    panic(err)
}

//在执行pipe.Exec之后才能获取结果
fmt.Println(incr.Val())
```

上面代码相当于将以下两个命令一次发给redis server 端执行，与不执行pipeline相比减少了一次RTT。

```go
INCR pipeline_counter
EXPIRE pipeline_counts 3600
```

或者，可以使用`Pipelined`方法，它会在函数退出时调用Exec。

```go
var incr *redis.IntCmd

cmds,err:=rdb.Pipelined(ctx,func(pipe redis.Pipeliner)error{
    incr := pipe.Incr(ctx,"pipelined_counter")
    pipe.Expire(ctx,"pipelined_counter",time.Hour)
    return nil
})
if err!=nil{
    panic(err)
}

//在pipeline执行之后获取结果
fmt.Println(incr.Val())
```

可以遍历pipeline命令的返回值一次获取每个命令的结果。下面示例代码使用pipeline一次执行100个Get命令，在pipeline执行之后遍历取出100个命令的结果。

```go
cmds,err:=rdb.Pipelined(ctx,func(pipe redis.Pipeliner)error{
    for i:=0;i<100;i++{
        pipe.Get(ctx,fmt.Spintf("key%d",i))
    }
    return nil
})
if err!=nil{
    panic(err)
}
for _,cmd:=range cmds{
    fmt.Println(cmd.(*redis.StringCmd).Val())
}
```

在我们需要一次执行多个命令的场景下，可以考虑使用pipeline来优化。



### 事务

redis是单线程执行命令，因此单个命令是原子的，但是来自不同的客户端的两个给定的命令可以依次执行，例如在它们之间交替执行。但是，`Multi/exec`能够确保在`multi/exec`两个语句之间的命令之间没有其他客户端正常执行命令。



在这种场景下，需要使用TxPipeline或TxPipelined 方法将pipeline命令使用`MULTI`和`EXEC`包裹起来。

```go
//TxPipeline demo
pipe:=rdb.TxPipeline()
incr:=pipe.Incr(ctx,"tx_pipeline_counter")
pipe.Expire(ctx,"tx_pipeline_counter",time.Hour)
_,err:=pipe.Exec(ctx)


//TxPipelined demo
var incr2 *redis.IntCmd
_,err=rdb.TxPipelined(ctx,func(pipe redis.Pipeliner)error{
    incr2 = pipe.Incr(ctx,"tx_pipeline_counter")
    pipe.Expire(ctx,"tx_pipeline_counter",time.Hour)
    return nil
})
fmt.Println(incr2.Val(),err)
```

上面代码相当于在一个RTT时间下，执行了下面的redis命令

```go
MULTI
INCR pipeline_counter
EXPIRE pipeline_counts 3600
EXEC
```



##### Watch

通常搭配`WATCH`命令来执行事务操作。从使用`WATCH`命令监视某个key开始，直到执行`EXEC`命令的这段时间里，如果有其他用户抢先对被监视的key进行了替换、更新、删除等操作，那么当用户尝试执行`EXEC`的时候，事务将失败并返回一个错误，用户可以根据这个错误来选择重试事务或者放弃事务。



Watch方法接收一个函数和一个或多个key作为参数。

```go
Watch(fn func(*Tx)error,keys ...string)error
```

下面代码片段演示Watch方法搭配下TxPipelined的使用示例

```go
//watchDemo 在key值不变的情况下将其值+1

func watchDemo(ctx context.Context,key string)error{
    return rdb.Watch(ctx,func(tx *redis.Tx)error{
        n,err:=tx.Get(ctx,key).Int()
        if err!=nil&&err!=redis.Nil{
            return err
        }
        
        //假设操作耗时5秒
        //5秒内我们通过其他客户端修改key，当前事务就会失败
        time.Sleep(5*time.Second)
        _,err=tx.TxPipelined(ctx,func(pipe redis.Pipeliner)error{
            pipe.Set(ctx,key,n+1,time.Hour)
            return nil
        })
        return err
    },key)
}
```

将上面的函数执行并打印其返回值，如果在程序运行后的5秒内修改了被watch的key的值，那么该事务操作就会失败，返回`redis:transaction failed`错误。



最后看一个go-redis官方文档中使用的`GET,SET,WATCH`命令实现一个INCR命令的完整示例。

```go
const routineCount = 100

increment := func(key string)error{
    txf := func(tx *redis.Tx)error{
        //获取当前值或零值
        n,err:=tx.Get(key).Int()
        if err!=nil&&err!=redis.Nil{
            return err
        }
        //实际操作（乐观锁定中的本地操作）
        n++
        //仅在监视的key保持不变的情况下执行
        _,err:=tx.Pipelined(func(pipe redis.Pipeliner)error{
            pipe.Set(key,n,0)
            return nil
        })
        return err
    }
    for retrier:=routineCount;retries>0;retries--{
        err:=rdb.Watch(txf,key)
        if err!=redis.TxFailedErr{
            return err
        }
        //乐观丢失
    }
    return errors.New("increment reached maximum number of retries")
}


var wg sync.WaitGroup
wg.Add(routineCount)
for i:=0;i<routineCount;i++{
    go func(){
        defer wg.Done()
        
        if err:=increment("counter3");err!=nil{
            fmt.Println("increment error:",err)
        }
    }()
}
wg.Wait()
n,err:= rdb.Get("counter3").Int()
fmt.Println("ended with",n,err)
```

在这个示例中使用了 `redis.TxFailedErr` 来检查事务是否失败。

更多详情请查阅[文档](https://pkg.go.dev/github.com/go-redis/redis)。





## Go操作MongoDB









# Gin框架

## 关于web

+ web是基于HTTP协议进行交互的应用网络
+ web就是通过使用浏览器/APP访问各种资源



![image-20230204161054089](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230204161054089.png)





## Go语言标准库之http/template

`html/template`包实现了数据驱动的模板，**用于生成可防止代码注入的安全的HTML内容**。它提供了和`text/template`包相同的接口，go语言中输出HTML 场景都应使用`http/template`这个包。



**模板渲染：**在一些前后端不分离的web架构中，我们通常需要在后端将一些数据渲染到HTML文档中，从而实现动态网页（网页的布局和样式大致一样，但展示的内容并不一样）效果。

==我们这里说的模板可以理解为事先定义好的HTML文档文件，模板渲染的作用机制可以简单理解为文本替换操作—使用相应的数据去替换HTML文档中事先准备好的标记。==

很多编程语言的Web框架中都使用模板引擎，比如Python语言中Flask框架中使用 jinja2 模板引擎。



### go语言的模板引擎

Go语言中内置的文本模板引擎`text/template`和用于HTML文档的`html/template`。它们的作用机制可以简单归纳为：

+ ==模板文件通常定义为`.tmpl`和`.tpl`为后缀（也可以使用其他的后缀），必须使用`UTF-8`编码。==
+ ==模板文件中使用`{{` 和 `}}` 包裹和标识需要传入的数据==
+ ==传给模板这样的数据就可以通过点号 （`.`）来访问，如果数据是复杂类型的数据，就可以通过 {{`.FieldName`}}来访问它的字段。==
+ ==除`{{` 和`}}` 包裹的内容外，其他内容均不修改原样输出。==



### 模板引擎的使用

Go语言模板引擎的使用可以分为三部分：==定义模板文件、解析模板文件、模板渲染。==



#### 定义模板文件

其中，定义模板文件时需要我们按照相关语法规则去编写，后文会详细介绍。



#### 解析模板文件

上面定义好了模板文件之后，可以使用下面的常用方法去解析模板文件，得到模板对象：

```go
//方法
func (t *Template)Parse(src string)(*Template,error)     //解析字符串,解析单个模板文件
func (t *Template) ParseFiles(filenames ...string) (*Template, error)
func (t *Template) ParseGlob(pattern string) (*Template, error)

//ParseFiles函数创建一个模板并解析filenames指定的文件里的模板定义。返回的模板的名字是第一个文件的文件名（不含扩展名），内容为解析后的第一个文件的内容。至少要提供一个文件。如果发生错误，会停止解析并返回nil
//当分析不同目录中同名的多个文件时，最后一个文件将是结果。
//例如，ParseFiles（“a/foo”，“b/foo”）将“b/foo”存储为名为“foo”的模板，而“a/foo”不可用


//函数
func ParseFiles(filenames ... string)(*Template,error)   //解析多个模板文件
func ParseGlob(pattern string)(*Template,error)         //解析意图（如：正则规则），通常用来解析某个文件夹下所有模板
```



当然，也可以使用`func New(name string) *Template  //指定名字创建模板`函数创建一个名为`name`的模板，然后对其调用上面的方法去解析模板字符串或模板文件。

```go
//注意：New后面的名字必须和解析的名字要对应上
t, err := template.New("f").Funcs(template.FuncMap{"kua": kua}).ParseFiles("./f.tmpl")   
// 此时，ParseFiles是方法
//可以看到，New创建的模板文件名必须和解析的文件名保持一致（即前f.tmpl，则后f.tmpl），否则报错


//注意：parse的用法
htmlByte, _ := ioutil.ReadFile("./index.tmpl")
template,_ := template.New("index").Funcs(template.FuncMap{"kua": kua}).Parse(string(htmlByte))
```





#### 模板渲染

==渲染模板简单来说就是使用数据去填充模板==，当然实际上可能复杂很多。

```go
//Execute方法将解析好的模板应用到data上，并将输出写入wr。如果执行时出现错误，会停止执行，但有可能已经写入wr部分数据。模板可以安全的并发执行。
//注意：当解析多个模板时，该方法是对解析的默认模板进行渲染
func (t *Template) Execute(wr io.Writer,data interface{})error  


//ExecuteTemplate方法类似Execute，但是使用名为name的t关联的模板产生输出。
//注意：name必须带后缀名，即当解析了多个模板时，我们可以指定某个模板，然后对其进行渲染
func (t *Template) ExecuteTemplate(wr io.Writer,name string,data interface{})error
```

==注意：使用ParseFiles一次指定多个文件或ParseGlob加载多个模板进来，就不可以使用t.Execute()来执行数据融合，可以通过t.ExecuteTemplate()方法指	定模板名称来执行数据融合。==



#### 基本示例

##### 定义模板文件

我们按照Go模板语法定义一个`hello.tmpl`的模板文件，内容如下：

```go
<!DOCTYPE html>
<html lang="zh-CN">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta http-equiv="x-ua-compatible" content="ie=edge">
</head>
<body>
	<p>Hello {{.}}</p>
</body>
</html>
```



##### 解析和渲染模板文件

然后创建一个`main.go`文件，在其中写下HTTP server端代码如下：

```go
//main.go

func sayHello(w http.ResposeWriter,r *http.Request){
    //解析指定文件生成模板对象
    tmpl,err:=template.ParseFiles("./hello.tmpl")
    if err!=nil{
        fmt.Println("create template failed,err:",err)
        return 
    }
    //利用给定数据渲染模板，并将结果写入w
    tmpl.Execute(w,"我的未来：web3.0")
}

func main(){
    http.HandleFunc("/",sayHello)
    err:=http.ListenAndServe(":9090",nil)
    if err!=nil{
        fmt.Println("HTTP server failed,err:",err)
        return 
    }
}
```

将上面的`mian.go`文件编译执行，然后使用浏览器访问`http://127.0.0.1:9090`就能看到页面上显示了"我的未来：web3.0"。这就是一个最简单的模板渲染的示例，Go语言模板引擎详细用法请往下阅读。



### 模板语法

#### {{.}}

模板语法都包含在 `{{` 和 `}}` 中间，==其中`{{.}}` 中的**点**表示当前对象==。

当我们传入一个结构体对象时，我们可以根据`.`来访问结构体的对应字段。例如：

```go
//main.go

type UserInfo struct{
    Name string
    Gender string
    Age int
}

func sayHello (w http.ResponseWriter,r *http.Request){
    //解析指定文件生成模板对象
    tmpl,err:=template.ParseFiles("./hello.tmpl")
    if err!=nil{
        fmt.Println("create template failed,err:",err)
        return 
    }
    
    //利用给定的数据渲染模板，并将结果写入w
    //这里为什么要字段大写，因为底层还是采用反射来读取的
    user := UserInfo{
        Name: "小王子",
        Gender: "男",
        Age : 18,
    }
    tmpl.Execute(w,user)
}
```

模板文件`hello.tmpl`内容如下：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width,initial-scale=1.0">
	<meta http-equiv="x-ua-compatible" content="ie=edge">
	<title>Hello</title>
</head>
<body>
	<!-- 注释-->
	<p>Hello {{.Name}}</p>
	<p>性别：{{.Gender}}</p>
	<p>年龄：{{.Age}}</p>
</body>
</html>
```

同理，**当我们传入的变量是map时，也可以在模板文件中通过`.`根据key来取值。**



#### 注释

```go
方式1：适合go语言
{{/*a comment */}}
注释，执行时会忽略。可以多行注释。但注释不能嵌套，并且必须紧贴分界符始止

方式2：通用
<!-- 注释内容 -->
这种形式注释子在html文件中很常见，也可以使用在tmpl，tpl文件中
```

==注意：`{{/*`和`*/}}`，这三个符号必须紧挨在一起才算注释。单独`/*   */`里面的内容不会注释掉。==





#### pipeline

`pipeline`是指生成数据的操作。比如`{{.}}`、`{{.Name}}`等。==Go的模板语法中支持使用管道符号`|`链接多个命令==，用法和unix下的管道类似：`|` 前面的命令会将运算结果（或返回值）传递给后一个命令的最后一个位置。==（`|`表示管道符，将前一个命令的输出作为下一个命令的输入）==

==注意：并不是只有使用了 `|` 才是pipeline。Go的模板语法中，`pipeline`的概念是传递数据，只要能产生数据的，都是`pipeline`。==



#### 变量

我们还可以在模板中声明变量，用来保存传入模板的数据或其他语句生成的结果。具体语法如下：

```go
//给变量赋值
{{$obj := .}}
{{$obj := 100}}

//使用时
{{$obj}}  //此时在网页上显示出来的是变量的值
```

其中,**`$obj`是变量的名字**，其中$ 是指定obj为变量，相当于var



#### 移除空格

有时候我们在使用模板语法的时候会不可避免地的引入一个空格或换行符，这样模板最终渲染出来的内容可能和我们想的不一样，这个使用可以**使用`{{-`**

**语法去除模板内容左侧的所有空白符号，使用`-}}` 去除模板内容右侧的所有空白符号**。

例如：

```go
{{-.Name-}}  //跟注释一样，必须紧挨左右两边的大括号，否则报错
```

==注意：`-`要紧挨`{{` 、`}}`·，同时与模板值之间需要使用空格分隔。==



#### 条件判断

Go模板语法中的条件判断有以下几种：

```go
{{if pipeline}} T1 {{end}}
{{if pipeline}} T1 {{else}} T0 {{end}}
{{if pipeline}} T1 {{else if pipeline}} T0 {{end}}
{{if pipeline}} T3 {{else if pipeline}} T2 {{else}} T1 {{else}} T0 {{end}}
```



#### range

Go的模板语法中使用`range`关键字进行遍历，有以下两种写法，==**其中`pipeline`的值必须是字符串、数组、切片、map或者管道**。==

```html
{{range pipeline}} T1 {{end}}
如果pipeline的值其长度为0，不会有任何输出

{{range pipleline}} T1 {{else}} T0 {{end}}
如果pipeline的值长度为0，则会执行T0

//例如：某slice := []string{"足球","篮球","双色球"}
{{range $index,$hobby := .}} {{$index}}--{{$hobby}}<br> {{else}} 没啥爱好！ {{end}}
//结果为：
0--足球
1--篮球
2--双色球
```



#### with

**以上两种方式得到的结果是相同的，可以看出with适用于省略大量重复的前缀**

```go
{{with pipleline}} T1 {{end}}
如果pipeline为empty不会产生输出，否则将dot设为pipeline的值并执行T1。不修改外面的dot。

{{with pipeline}} T1 {{else}} T0 {{end}}
如果pipeline为empty，不会改变dot并执行T0，否则dot设为pipeline的值并执行T1


//示例：
原来的是：
{{.m.Age}}
{{.m.Name}}

通过使用with来定义局部 . 代表什么
{{with .m}}
{{.Age}}
{{.Name}}
{{end}}
```



#### 预定义函数

执行模板时，函数从两个函数字典中查找：首先是模板函数字典，然后是全局函数字典。**一般不在模板内定义函数，而是使用Funcs方法添加函数到模板里。**

预定义的全局函数如下：

```go
and 
	函数返回它的第一个empty参数或者最后一个参数；
	就是说"and x y " 等价于 "if x then y else x"；所有参数都会执行；

or 
	返回第一个非empty参数或者最后一个参数；
	即 "or x y" 等价于 "if x then x else y";所有参数都会执行；

not 
	返回它的单个参数的布尔值的否定
	
len
	返回它的参数的字节长度

index
	执行结果为第一个参数以剩下的参数为索引/键指向的值；
	如 "index x 1" 返回x[1]的值；每个被索引的主体必须是数组、切片或者字典。

print
	即fmt.Sprint

printf
	即fmt.Sprintf

println
	即fmt.Sprintln

html
	以适合嵌入到网址查询中的形式返回其参数的文本表示的转义值。
	这个函数在html/template中不可用

js
	返回与其参数的文本表示形式等效的转义JavaScript

call
	执行结果是调用第一个参数的返回值，该参数必须是函数类型，其余参数作为调用该函数的参数；
	如 "call .X.Y 1 2" 等价于go语言中dot.X.Y(1,2)；
	其中，Y是函数类型的字段或者字典的值，或者其他类似情况。
	call的第一个参数的执行结果必须是函数类型的值（和预定义函数如print明显不同）
	该函数类型值必须有1到2个返回值，如果有2个则后一个必须是error接口类型
	如果有2个返回值的方法返回的error非nil，模板执行会中断并返回给调用模板执行者该错误。

```





#### 比较函数

布尔函数会将任何类型的零值视为假，其余视为真

下面是定义为函数的二元比较运算的集合：

```go
eq  如果arg1 == arg2则返回真    //eq   equal
ne  如果arg1 != arg2 则返回真  //ne   not equal
lt  如果arg1 < arg2 则返回真  // lt    less than
le  如果arg1 <= arg2 则返回真  //le   less than or equal
gt  如果arg1 > arg2 则返回真   //gt   greater than
ge  如果arg1 >= arg2 则返回真  //ge   greater than or equal
```

为简化多参数相等检测，eq（只有eq）可以接受2个或者更多个参数，它会将第一个参数和其他参数依次比较，返回下式结果：

```go
{{eq arg1 arg2 arg3}}
```

比较函数只适用于基本类型（或重定义的基本类型，如"type Celsius float32"。但是，整数和浮点数不能相互比较)。



#### 自定义函数

Go的模板支持自定义函数。

```go
func sayHello(w http.ResposeWriter,r *http.Request){
    htmlByte,err:=ioutil.ReadFile("./hello.tmpl")
    if err!=nil{
        fmt.Println("read html failed,err:",err)
        return 
    }
    //自定义一个夸人的模板函数
    //这个自定义函数没有要求，有返回值即可。因为:template.FuncMap[string]any
    kua := func (arg string)(string,error){  
        return arg+”真帅",nil
    }
    
    
	//对于自定义函数，必须在解析模板之前注册，告诉模板引擎我们现在多了一个自定义函数
    //采用链式操作在Parse之前调用Funcs添加自定义的kua函数
    
    //理解：
    //先新建一个模板，然后设置模板函数，然后将其解析到字符串中htmlByte，也就是
    tmpl,err:=template.New("hello").Funcs(template.FuncMap{"kua":kua}).Parse(string(htmlByte))
    if err!=nil{
        fmt.Println("create template failed,err:",err)
        return
    }
    
    user := UserInfo{
        Name:"小王子",
        Gender:"男",
        Age : 19,
    }
    //使用user渲染模板，并将结果写入w
    tmpl.Execute(w,user)
}
```

我们可以在模板文件`hello.tmpl`中按照如下方式使用我们自定义的`kua`函数了。

```go
{{kua .Name}}  //将 .Name 作为kua的参数传入进去的

//预定义函数和自定义函数传参都是如此，将函数后的值作为参数传进去
```



#### 嵌套template

我们可以在template中嵌套其他的template。==**这个template可以是单独的文件，也可以是通过`define`定义的template**==。

举一个例子：`t.tmpl`文件内容如下：

```go
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <meta http-equiv="x-ua-compatible" content="ie=edge">
</head>

<body>
    <h1>测试嵌套</h1>
    <hr>
    {{/* 嵌套一个单独的模板 */}}
	<!--注意：template 模板名 . ,可以在模板名后添加数据，即将数据传递到嵌套的那个模板中，-->
    {{template "ul.tmpl" .}}   
    <hr>
    {{/* 嵌套一个define定义的模板 */}}
    {{template "ol.tmpl"}}

    <div>你好，{{.}}</div>
</body>
</html>

{{/*通过define定义一个模板*/}}
{{define "ol.tmpl"}}
<ol>
    <li>吃饭</li>
    <li>睡觉</li>
    <li>打豆豆</li>
</ol>
{{end}}
```

`ul.tmpl`文件内容如下：

```go
<ul>
	<li>注释</li>
	<li>日志</li>
	<li>测试</li>
</ul>
```

我们注册一个`tmplDemo`路由处理函数

```go
http.HandleFunc("/tmpl",tmplDemo)
```

`tmplDemo`函数的具体内容如下：

```go
func demo1(w http.ResponseWriter, r *http.Request) {
	//定义模板，解析模板，渲染模板
	//解析模板
    //当分析不同目录中同名的多个文件时，最后一个文件将是结果。
	//例如，ParseFiles（“a/foo”，“b/foo”）将“b/foo”存储为名为“foo”的模板，而“a/foo”不可用
	t, err := template.ParseFiles("./t.tmpl", "./ul.tmpl")
	if err != nil {
		fmt.Printf("parse template failed,err:%v\n", err)
		return
	}
	//渲染模板
	t.Execute(w, "小灰灰")
}

func main() {
	http.HandleFunc("/tempdemo1", demo1)
	err := http.ListenAndServe("127.0.0.1:9090", nil)
	if err != nil {
		fmt.Printf("http server start failed,err:%v\n", err)
		return
	}
}
```

==**注意**：**在解析模板时，被嵌套的模板一定要在后面解析，例如上的示例中`t.tmpl`模板中嵌套了`ul.tmpl`，所以`ul.tmpl`要在`t.tmpl`后进行解析**==。



#### block

```go
{{block "name" pipeline}} T1 {{end}}
```

`block`是定义模板`{{define "name"}} T1 {{end}}`和执行`{{template "name" pipeline}}`缩写，==典型的用法是定义一组根模板，然后通过在其中重新定义块模板进行自定义。==



定义一个根模板`template/base.tmpl`，内容如下：

```go
<!DOCTYPE html>
<html lang="zh-CN">
<head>
	<title>Go Templates</title>
</head>
<body>
<div class="container-fluid">
	{{block "content".}}{{end}}  // . 表示executeTemplate中传递的data
</div>
</body>
</html>
```

然后定义一个`template/index.tmpl`，”继承“ `base.tmpl`：

```go
如果多个模板中存在共同的内容，我们可以将共同的内容写进一个公共模板中，其他模板通过继承该模板，从而获取相同内容。
但是，每个模板中还有自己单独的内容，此时，我们可以通过定义block，将自己单独的内容写入block中。
其实，定义block就是相当于{{define "name"}} T {{end}} 和执行 {{template "name" pipeline}}缩写
```

```go
{{template "base.tmpl" .}}  //继承模板，如果想要index.tmpl模板中有数据，必须在继承时将数据传过来
{{define "content"}}        //定义模块
	<div>Hello World!</div>
{{end}}
```

然后使用`template.ParseGlob`按照正则匹配规则解析模板文件，然后通过`ExecuteTemplate`渲染指定的模板：

```go
func index(w http.ResponseWriter,r *http.Request){
    tmpl,err:=template.ParseGlob("templates/*.tmpl")
    if err!=nil{
        fmt.Println("create template failed,err:",err)
        return 
    }
    err = tmpl.ExecuteTemplate(w,"index.tmpl",nil)
    if err!=nil{
        fmt.Println("render template failed,err:",err)
        return 
    }
}
```

如果我们的模板名称冲突了，例如不同业务线下都定义了一个`index.tmpl`模板，我们可以通过下面两种方法来解决：

+ 如果模板文件出现重名，可以在模板文件开头使用`{{define 模板名}}`，相当于给模板文件重新起了一个名字来防止冲突。





#### 修改默认的标识符

go语言标准库的模板引擎使用的花括号`{{`和`}}` ，而许多前端框架（如`vue`和`AngularJS`也是用`{{`和`}}`作为标识符，所以当我们同时使用Go语言模板引擎和以上前端框架时会出现冲突，这个时候我们需要修改标识符，修改前端的或者修改Go语言的。这里演示如何修改Go语言模板引擎默认的标识符：

```go
//在模板解析时修改模板的标识符
template.New("test").Delims("{[","]}").ParseFiles("./t.tmpl")
```

示例：

```go
//这是index.tmpl模板文件
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <title>修改模板引擎的标识符</title>
</head>
<body>
<div>Hello {[.]}</div>
</body>
</html>
```

```go
package main

import (
	"fmt"
	"html/template"
	"net/http"
)

func index(w http.ResponseWriter, r *http.Request) {
	//定义模板，解析模板，渲染模板
	//New后面的参数必须写全index.tmpl而不能index，要跟后面解析的文件名一致
	t, err := template.New("index.tmpl").Delims("{[", "]}").ParseFiles("./index.tmpl")
	if err != nil {
		fmt.Println("parse ")
		return
	}
	t.Execute(w, "小灰灰")
}

func main() {
	http.HandleFunc("/index", index)
	err := http.ListenAndServe(":9090", nil)
	if err != nil {
		fmt.Printf("http server start failed,err:%v\n", err)
		return
	}
}
```





### text/template与html/template区别

`html/template`针对的是需要返回HTML内容的场景，在模板渲染过程中会对一些有风险的内容进行转义，以此来防范跨站脚本攻击。

例如：我们定义下面的模板文件：

```go
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,inital-scale=1.0">
    <meta http-equiv="x-ua-compatible" content="ie=edge">
</head>
<body>
{{. | safe}}  {{/* |是管道符,将.通过管道传递给safe函数 */}}
{{/* 也可以像上面的自定义函数kua,一样用法: */}}
{{safe .}} {{/* 将safe函数写在前面,后面.作为参数传递进去  */}}
</body>
</html>
```

这个时候传入一段JS代码并使用`html/tempalte`去渲染这个文件，会在页面上显示出转义后的JS内容。`<script>alert('嘿嘿嘿')</script>`这就是`html/template`为我们做的事情。

但是在某些场景下，我们如果相信用户输入的内容，不想转义的话，可以自行编写一个safe函数，手动返回一个`template.HTML`类型的内容。示例如下：

```go
func SayHello(w http.ResponseWriter, r *http.Request) {

	//解析模板
	//模板嵌套其实就是在解析模板时给嵌套进去了
	t, err := template.New("x1").Funcs(template.FuncMap{
		"safe": func(str string) template.HTML {
			return template.HTML(str)
		},
	}).ParseFiles("./x1.html")
	if err != nil {
		fmt.Println("err:", err)
		return
	}
    
	//渲染模板
    
    //如果只有一个模板解析，可以使用execute来渲染模板
    t.Execute(w,"<a href='http://liwenzhou.com'>liwenzhou博客") 
    
    
    
   //如果解析了多个模板，此时需要指定模板名，最后再用executeTemplate来渲染
    
   //注意：如果使用了new，一定要executeTemplate来进行渲染，否则会出错
	t.ExecuteTemplate(w,"x1.html", "<a href='http://liwenzhou.com'>liwenzhou博客")
}

func main() {
	http.HandleFunc("/x1", SayHello)
	http.ListenAndServe(":9090", nil)
}
```

![image-20230309212037861](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230309212037861.png)







## gin框架介绍和使用

`Gin`是一个用Go语言编写的web框架。它是一个类似`martini`但拥有更好的性能的API框架，由于使用了`httprouter`，速度提高了近40倍。如果你是性能和高效的追求者，会爱上`Gin`。

### Gin框架介绍

Go语言世界最流行的Web框架，[Github](https://github.com/gin-gonic/gin)上有`32K+`star。 基于[httprouter](https://github.com/julienschmidt/httprouter)开发的Web框架。 [中文文档](https://gin-gonic.com/zh-cn/docs/)齐全，简单易用的轻量级框架。



### Gin框架安装和使用

#### 安装

下载并安装`Gin`：

```go
go get -u github.com/gin-gonic/gin
```



#### 第一个Gin示例

**注意事项**：

+ 在使用gin.Default()创建Gin引擎时，已经自带了Logger()、Recovery()中间件。
  + Logger中间件将日志写入gin.DefaultWriter。
  + Recovery中间件会recover任何panic。如果有panic的话，会写入500响应码。
+ 如果创建Gin引擎时，不想使用默认的中间件。可以使用gin.New()。但是不建议这么做。



```go
package main

import(
	"github.com/gin-gonic/gin"
)

func main(){
    //创建一个默认的路由引擎
    r := gin.Default()
    //GET:请求方式；/hello：请求路径
    //当客户端以GET方法请求/hello路径时，会执行后面的匿名函数
    r.GET("/hello",func(c *gin.Context){
        //c.JSON:返回JSON格式的数据
        c.JSON(200,gin.H{
            "message":"hello world",
        })
    })
    //启动HTTP服务，默认在0.0.0.0:8080启动服务
    r.Run("0.0.0.0:8080")
}
```

将上面的代码保存并编译，然后使用浏览器打开`127.0.0.1:8080/hell0`就能看到一串JSON字符粗。



### RESTful API 

REST与技术无关，代表的是一种软件架构风格，REST是Representational State Transfer的简称，中文翻译为”表征状态转移“或”表现层状态转移“。

推荐阅读[阮一峰 理解RESTful架构](http://www.ruanyifeng.com/blog/2011/09/restful.html)

简单来说，REST的含义就是客户端与Web服务器之间进行交互的时候，使用HTTP协议中的4个请求方式代表不同的动作：

+ **GET 用来获取资源**
+ **POST 用来新建资源**
+ **PUT 用来更新资源**
+ **DELETE 用来删除资源**

只要API程序遵循了REST风格，那就可以称其为RESTful API。目前在前后端分离的架构中，前后端都是通过RESTful API 来进行交互。



例如：我们现在要编写一个管理书籍的系统，我们可以对一本书进行查询、创建、更新、删除等操作，我们在编写程序的时候就要设计客户端浏览器与我们web服务器交互的方式和路径。



按照经验我们通常会设计如下模式：

| 请求方式 | URL          | 含义         |
| -------- | ------------ | ------------ |
| GET      | /book        | 查询书籍信息 |
| POST     | /create_book | 创建书籍信息 |
| POST     | /update_book | 更新书籍信息 |
| POST     | /delete_book | 删除书籍信息 |



**同样的需求我们按照RESTful API 设计如下：**

| 请求方式 | URL          | 含义         |
| -------- | ------------ | ------------ |
| GET      | /book        | 查询书籍信息 |
| POST     | /create_book | 创建书籍信息 |
| PUT      | /update_book | 更新书籍信息 |
| DELETE   | /delete_book | 删除书籍信息 |



Gin 框架支持开发RESTful API 的开发

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	//返回默认的引擎
	r := gin.Default()

	r.GET("/book", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"method": "GET",
		})
	})

	r.POST("/book", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"method": "POST",
		})
	})

	r.PUT("/book", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"method": "PUT",
		})
	})

	r.DELETE("/book", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"method": "DELETE",
		})
	})

	//启动服务
	r.Run("127.0.0.1:9090")
}
```

 



### Gin框架中两个重要概念

#### gin.Engine

Engine是Gin框架最重要的数据结构，它是框架的入口。我们**通过Engine对象来定义服务路由信息，组装插件，运行服务**。Engine如中文意思一致，它就是框架的核心发动机，整个web服务都是由它来驱动的。

Engine 对象很简单，因为引擎最重要的部分 —— 底层的 HTTP 服务器使用的是 Go 语言内置的 http server，Engine 的本质只是对内置的 HTTP 服务器的包装，让它使用起来更加便捷。




#### gin.Context

Context 是gin框架贯穿整个请求的上下文，从请求开始到请求完成无论是在middleware/handle中，我们可以通过context对象获取到请求的信息，响应的信息，可以在context中设置一些自定义参数，在整个请求的过程都可以获取。





### Gin渲染

#### HTML渲染

我们首先定义一个存放模板文件的`templates`文件夹,然后在其内部按照业务分别定义一个`posts`文件夹和一个`users`文件夹.`posts/index.html`文件的内容如下:

```go
{{define "posts/index.html"}}
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width,initial-scale=1.0">
	<meta http-equiv="x-ua-compatible" content="ie=edge">
	<title>posts/index</title>
</head>
<body>
{{.title}}
</body>
</html>
{{end}}
```

`users/index.html`文件内容如下:

```go
{{define "users/index.html"}}
<!DOCTYPE html>
<html lang="en">
<head>
	<meata charset="UTF-8">
	<meta name="viewport" content="width=device-width,initial-scale=1.0">
	<meta http-equiv="x-ua-compatible" content="ie=edge">
	<title>users/index</title>
</head>
<body>
{{.title}}
</body>
<html>
{{end}}
```

Gin框架中使用`LoadHTMLGlob()`或者`LoadHTMLFiles()`方法进行HTML模板渲染.

```go
func main(){
    r:=gin.Default()   //创建一个默认的路由引擎
    r.LoadHTMLGlob("templates/**/*")  //解析模板
    //r.LoadHTMLFiles("templates/posts/index.html","templates/users/index.html")
    r.GET("/posts/index",func(c *gin.Context){
        c.HTML(http.StatusOK,"posts/index.html",gin.H{  //渲染模板
            "title":"posts/index",
        })
    })
    r.Run(":8080")  //启动server
}
```





#### 自定义模板函数

==**必须在模板解析之前添加自定义函数**==

定义一个不转义相应内容的`safe`模板函数如下:

```go
func main(){
    r := gin.Default()
    //在gin框架中可以添加自定义函数，但是必须在解析之前
    r.SetFuncMap(template.FuncMap{
        "safe":func(str string)template.HTML{
            return template.HTML(str)
        },
    })
    r.GET("/index",func(c *gin.Context){
        c.HTML(http.StatusOK,"index.tmpl","<a href='https://liwenzhou.com'>李文周博客</a>")        
    })
    r.Run(":8080")
}

```

在`index.tmpl`中使用定义好的`safe`模板函数:

```go
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<title>修改模板引擎的标识符</title>
</head>
<body>
<div>{{.|safe}}</div>
</body>
</html>
```





#### 静态文件处理

静态文件：html页面上用到的样式文件，通常是`.css文件`、`.js文件`、图片等。



**加载静态文件方式（==必须在解析文件之前加载静态文件==）：**

```go
//第一个参数为相对目录，第二个参数为源目录，引用文件时直接从相对目录下来读取文件，即用相对目录代替了源目录
func (group *RouterGroup) Static(relativePath string, root string)

//第一个参数为相对路径，第二个参数为文件路径，也就是用相对路径来表示源路径，在引用时直接使用相对路径即可
func (group *RouterGroup) StaticFile(relativePath string, filepath string)
```



示例：

```go
//在statics目录下存在一个css文件，/statics/index.css

body{
    background-color: aqua;
}
```

```go
// 在statics目录下存在一个js文件，/statics/index.js

alert(123)
```

```go
// 在users目录存在一个tmpl文件 /users/index.tmpl

{{define "users/index.tmpl"}}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <meta http-equiv="x-ua-compatible" content="ie=edge">
    <title>posts/index</title>

    {{/*css文件通常放在head最后面*/}}
    <link rel="stylesheet" href="/xxx/index.css">     //采用Static
	{{/*<link rel="stylesheet" href="/index.css"*/}}  //采用StaticFile

</head>
<body>
{{. | safe}}

{{/*js文件通常放在body的最下面*/}}
<script src="/xxx/index.js"></script>		//采用Static
{{/*<script src="/index.js"></script>*/}}   //采用StaticFile

</body>
</html>
{{end}}
```



```go
package main

import (
	"github.com/gin-gonic/gin"
	"html/template"
	"net/http"
)

func main() {
	r := gin.Default()
	//模板解析
	//r.LoadHTMLGlob("./*.tmpl")

	//必须在解析文件之前，加载静态文件
	//这个相当于给根目录起了一个别名路径，然后在tmpl文件中根据别名路径来读取.css文件
	r.Static("/xxx", "./statics")
    
    
	//相当于给源路径下的文件起了一个别名，直接根据别名就能读取.css文件
	//r.StaticFile("/index.css", "./statics/index.css")
	//r.StaticFile("/index.js", "./statics/index.js")

	//gin框架中，可以在解析模板之前，自定义函数
	r.SetFuncMap(template.FuncMap{
		"safe": func(str string) template.HTML {
			return template.HTML(str)
		},
	})
	//解析模板
	r.LoadHTMLFiles("./users/index.tmpl", "./posts/index.tmpl")

	r.GET("/posts/index", func(c *gin.Context) {
		//模板渲染
		c.HTML(http.StatusOK, "posts/index.tmpl", gin.H{ //gin.H 是一个map[string]interface{}
			"title": "liwenzhou.com",
		})
	})
	r.GET("/users/index", func(c *gin.Context) {
		//模板渲染
		c.HTML(http.StatusOK, "users/index.tmpl", "<a href='https://liwenzhou.com'>李文周博客</a>")
	})
	//启动server
	r.Run("127.0.0.1:9090")
}

```





#### 使用模板继承

==**Gin框架默认都是使用单模板**==，如果需要使用`block template`功能，可以通过`"github.com/gin-contrib/multitemplate"`库实现，具体示例如下：

首先，假设我们项目目录下的template文件夹下有以下模板文件，其中`home.tmpl`和`index.tmpl`继承了`base.tmpl`：

```go
templates
├── includes
│   ├── home.tmpl
│   └── index.tmpl
├── layouts
│   └── base.tmpl
└── scripts.tmpl
```

然后，我们定义一个`loadTemplates`函数如下：

```go
func loadTemplates(templatesDir string)multitemplate.Renderer{
    r := multitemplate.NewRenderer()
    layouts,err:=filepath.Glob(templateDir + "/layouts/*.tmpl")
    if err!=nil{
        panic(err.Error())
    }
    includes,err:= filepath.Glob(templateDir + "/includes/*.tmpl")
    if err!=nil{
        panic(err.Error())
    }
    //为layouts/和includes/目录生成templates map
    for _,include := range includes{
        layoutCopy := make([]string,len(layouts))
        copy(layoutCopy,layouts)
        files := append(layoutCopy,include)
        r.AddFromFiles(filepath.Base(include),files...)
    }
    return r
}
```

我们在`main`函数中：

```go
func indexFunc(c *gin.Context){
    c.HTML(http.StatusOK,"index.tmpl",nil)
}
func homeFunc(c *gin.Context){
    c.HTML(http.StatusOK,"home.tmpl",nil)
}

func main(){
    r:=gin.Default()
    r.HTMLRender = loadTemplates("./templates")
    r.GET("/index",indexFunc)
    r.GET("./home",homeFunc)
    r.GET("/home",homeFunc)
    r.Run()
}
```





#### 补充文件路径处理

关于模板文件和静态文件的路径，我们需要根据公司和项目的要求进行设置。可以使用下面的函数获取当前执行程序的路径。

```go
func getCurrentPath()string{
    if ex,err:=os.Executable();err!=nil{
        return filepath.Dir(ex)
    }
    return "./"
}
```





#### JSON渲染

```go
func main(){
    r := gin.Default()
    
    //gin.H 是map[string]interface{}的缩写
    r.GET("/someJSON",func(c *gin.Context){
        //方式1，自己拼接JSON
        c.JSON(http.StatusOK,gin.H{     //序列化对map没有影响
            "message":"Hello World!",
        })
    })
    r.GET("/moreJSON",func(c *gin.Context){
        //方法2，使用结构体
        var msg struct{
            Name string `json:"user"`
            Message string
            Age int
        }
        msg.Name = "小王子"
        msg.Message = "hello world"
        msg.Age = 20
        c.JSON(http.StatusOK,msg)    //这里有序列化，所以结构体字段必须大写
    })
    r.Run(":8080")
}
```





#### XML渲染

注意需要使用具体的结构体类型。

```go
func main(){
    r := gin.Default()
    //gin.H 是map[string]interface{}的缩写
    r.GET("/someXML",func(c *gin.Context){
        //方法1，自己拼接JSON
        c.XML(http.StatusOK,gin.H{
            "message":"hello world",
        })
    })
    
    //注意：xml不支持结构体类型
    /*
    r.GET("/moreXML",func(c *gin.Context){
        //方法2，使用结构体
        type MessageRecord struct{
            Name string
            Message string
            Age int
        }
        var msg MessageRecord
        msg.Name = "小王子"
        msg.Message = "Hello world"
        msg.Age = 19
        c.XML(http.StatusOK,msg)  //注意：结构体字段必须大写
    })
    */
    r.Run(":8080")
}
```





#### YMAL渲染

```go
//请求该路径后,会下载一个ymal格式的文件
r.GET("/someYAML",func(c *gin.Context){
    c.YAML(http.StatusOK,gin.H{
        "message":"ok",
        "status":http.StatusOK,
    })
    
    //如果使用结构体，则结构体的字段必须大写
})
```





#### protobuf渲染

```go
r.GET("/someProtoBuf",func(c *gin.Context){
    reps := []int64{int64(1),int64(2)}
    label := "test"
    //protobuf 的具体定义写在 testdata/protoexample 文件中
    data := &protoexample.Test{
        Label : &label,
        Reps:reps,
    }
    //请注意：数据在响应中变为二进制数据
    //将输出被protoexample.Test protobuf 序列化了的数据
    c.ProtoBuf(http.StatusOK,data)
})
```





### 获取参数

#### 获取querystring参数

**`querystring`指的是URL中的`?`后面携带的具体参数**，例如：`/user/search?username=小王子&address=沙河`。获取请求的querystring参数的方法如下：

```go
func main(){
    //Default 返回一个默认的路由引擎
    r := gin.Default()
    
    
    //GET请求 URL ? 后面的是querystring参数，key=value格式，多个key=value可以使用& 连接
    //例如：/web?query=哈哈&age=23
    
    
    r.GET("/user/search",func(c *gin.Context){
        
        //第一种方式：
        //username := c.DefaultQuery("username","小王子") //取不到，就用默认值
        
        
        //第二种方式：
        //username,ok := c.DefualtQuery("username") //会返回两个参数，第二个参数为bool
   		//if !ok{
        //    username = "some body"
        //}
        
        
        //第三种方式：
        address := c.Query("address")
        
        
        //输出json结果给调用方
        c.JSON(http.StatusOK,gin.H{
            "message":"ok",
            "username":username,
            "address":address,
        })
    })
    r.Run()
}
```





#### 获取form参数

```go
//index.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>username: {{.username}} </h1>
<p>password: {{.password}}</p>
</body>
</html>
```

```go
//login.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>login</title>
</head>
<body>
<form action="/login" method="post" novalidate autocomplete="off">
    <div>
        <label for="username">username:</label>
        <input type="text" name="username" id="username">
    </div>
    <div>
        <label for="password">password:</label>
        <input type="password" name="password" id="password">
    </div>
    <div>
        <input type="submit" value="登录">
    </div>

</form>
</body>
</html>
```



**当前端请求的数据通过form表单提交时**，例如向`/login`发送一个POST请求，获取请求数据的方式如下：

```go
package main

import (
	"github.com/gin-gonic/gin"	
	"net/http"
)

//获取form表单提交的参数

func main() {
	r := gin.Default()

	r.LoadHTMLFiles("./login.html", "./index.html")
	r.GET("/login", func(c *gin.Context) {
		c.HTML(http.StatusOK, "login.html", nil)
	})
	//login post

	r.POST("./login", func(c *gin.Context) {
		//第一种方式
		username := c.PostForm("username")
		password := c.PostForm("password")

		//第二种方式
		//username:=c.DefaultPostForm("username","哈哈")
		//password:=c.DefaultPostForm("password","***")  //返回默认值

		//第三种方式
		//username,ok := c.GetPostForm("username")  //返回bool值
		//if !ok{
		//	username = "哈哈"
		//}
		//password ,ok := c.GetPostForm("password")
		//if !ok{
		//	password = "***"
		//}

		c.HTML(http.StatusOK, "index.html", gin.H{
			"username": username,
			"password": password,
		})
	})

	r.Run(":9090")
}
```





#### 获取json参数

**当前端请求的数据通过JSON提交时**，例如向`/json`发送一个POST请求，则获取请求参数的方式如下：

```go
package main

import (
	"encoding/json"
	"fmt"
	"github.com/gin-gonic/gin"
	"net/http"
)

//当前提交的数据是通过json格式，如何从中获取请求参数

func main() {
	r := gin.Default()

	r.POST("/post", func(c *gin.Context) {
		b, err := c.GetRawData() //从c.Request.Body读取请求数据
		if err != nil {
			fmt.Println("get rawData failed,err:", err)
			return
		}
		var m map[string]interface{}

		//反序列化，由于b是经过序列化后的数据
		json.Unmarshal(b, &m)

		c.JSON(http.StatusOK, m)

	})
	r.Run(":9090")
}
```

更便利的获取请求参数的方式，参见下面的 **参数绑定** 小节。





#### 获取path参数

**请求的参数通过URL路径传递，**例如：`/user/qimi/23`。获取请求的URL路径中参数的方式如下：

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

//获取path参数  （获取URI参数），返回参数都是字符串类型
func main() {
	r := gin.Default()

	//注意：此处的 :name  :age 相当于变量，当外部url中格式跟它对应，会自动将其替换
	//例如：请求地址  http://127.0.0.1:9090/user/qimi/23
	//相当于下面的代码变成了 ： r.GET("/user/qimi/23",func(c *gin.Context){...})

	r.GET("/user/:name/:age", func(c *gin.Context) {
		name := c.Param("name") //获取path参数，返回的是字符串类型，或者 name := c.Params.ByName("name")
		age := c.Param("age")   //获取path参数方法，返回的是字符串类型
		c.JSON(http.StatusOK, gin.H{
			"name": name,
			"age":  age,
		})
	})

    //注意：如果获取path参数时，要保证访问路径前缀不一样，否则可能出现错误，原因是，如果第一个路径为：/:name/:age
    //后一个路径为：/blog/:year/:month，当我在发送请求 127.0.0.1:9090/blog/2019/12，此时会出现冲突，会将blog填充到第一	   //个路径中 :name上，这样与我们原意产生了冲突，所以在多个路径时保证不冲突。
    
	r.GET("/blog/:year/:month", func(c *gin.Context) {
		year := c.Param("year")
		month := c.Param("month")
		c.JSON(http.StatusOK, gin.H{
			"year":  year,
			"month": month,
		})
	})
	r.Run(":9090")
}

```





#### 参数绑定

为了能够方便的获取请求相关参数，提高开发效率，我们可以**基于请求的`Context-Type`识别请求数据类型并利用反射机制自动提取请求中`QueryString`、`form表单`、`JSON`、`XML`等参数到结构体中**。下面示例代码演示了`.ShouldBind()`强大的功能，它能够基于请求自动提取`JSON`、`from表单`和`QueryString`类型的数据，并把值绑定到指定的结构体对象。

```go
//Binding from JSON
//用参数绑定,下面的标签最好都要写,binding标签必不可少
type Login struct{
    User string `form:"user" json:"user" binding:"required"` 
    Password string `form:"password" json:"password" binding:"required"`
}

func main(){
    router := gin.Default()
    
    //绑定JSON的示例({"user":"qimi","password":"123456"})
    router.POST("/loginJSON",func(c *gin.Context){
        var login Login
        if err:= c.ShouldBind(&login);err==nil{
            fmt.Printf("login info:%#v\n",login)
            c.JSON(http.StatusOK,gin.H{
                "user":login.User,
                "password":login.Password,
            })
        }else{
            c.JSON(http.StatusBadRequest,gin.H{"error":err.Error()})
        }
    })
    //绑定form表单示例(user=qimi&password=123456)
    router.POST("/loginForm",func(c *gin.Context){
        var login Login
        //ShouldBind()会根据请求的Content-Type自行选择绑定器
        if err:=c.ShouldBind(&login);err==nil{
            c.JSON(http.StatusOK,gin.H{
                "user":login.User,
                "password":login.Password,
            })
        }else{
            c.JSON(http.StatusBadRequest,gin.H{"error":err.Error()})
        }
    })
    // 绑定QueryString示例 (/loginQuery?user=q1mi&password=123456)
	router.GET("/loginForm", func(c *gin.Context) {
		var login Login
		// ShouldBind()会根据请求的Content-Type自行选择绑定器
		if err := c.ShouldBind(&login); err == nil {
			c.JSON(http.StatusOK, gin.H{
				"user":     login.User,
				"password": login.Password,
			})
		} else {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		}
	})

	// Listen and serve on 0.0.0.0:8080
	router.Run(":8080")
}
```

`ShouldBind`会按照下面的顺序解析请求中的数据完成绑定：

1. 如果是 `GET` 请求，只使用 `Form` 绑定引擎（`query`）。
2. 如果是 `POST` 请求，首先检查 `content-type` 是否为 `JSON` 或 `XML`，然后再使用 `Form`（`form-data`）。

==**绑定还有许多方法，Bind开头的方法，还有ShouldBind开头的方法，可以灵活使用，并不局限于ShouldBind方法**==

==**如何实现自动将值赋值给结构体字段上，ShouldBind内部采用反射来获取字段并赋值，因此结构体字段必须要大写，并且还要传递地址，以及设置tag**==

```go
//示例：

package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
	"net/http"
)

type UserInfo struct {
	Username string `json:"user" form:"username"`
	Password string `json:"pwd" form:"password"`
}

func main() {
	r := gin.Default()

	r.GET("/user", func(c *gin.Context) {

		//当少量的参数时，可以一个一个的提取，如果大量参数怎么办？
		//username := c.Query("username")
		//password := c.Query("password")
		//c.JSON(http.StatusOK, gin.H{
		//	"username": username,
		//	"password": password,
		//})

		//大量参数，可以利用ShouldBind方法来自动绑定
		var u UserInfo
		//如何实现自动将值赋值给结构体字段上，ShouldBind内部采用反射来获取字段并赋值，因此结构体字段必须要大写，并且还要传递地址，以及设置tag
		err := c.ShouldBind(&u)
		if err != nil {
			c.JSON(http.StatusBadRequest, gin.H{
				"error": err.Error(),
			})
		} else {
			fmt.Printf("%#v\n", u)
			c.JSON(http.StatusOK, gin.H{
				"status": "ok",
			})
		}

	})

	r.POST("/form", func(c *gin.Context) {
		var u UserInfo
		err := c.ShouldBind(&u)
		if err != nil {
			c.JSON(http.StatusBadRequest, gin.H{
				"error": err.Error(),
			})
		} else {
			fmt.Printf("%#v\n", u)
			c.JSON(http.StatusOK, gin.H{
				"status": "ok",
			})
		}
	})

	//对于发送过来的json格式的数据，shouldbind会根据json的tag来进行比对，如果没有设定json的tag，则会根据form的tag来比对。
	r.POST("/json", func(c *gin.Context) {
		var u UserInfo
		err := c.ShouldBind(&u)
		if err != nil {
			c.JSON(http.StatusBadRequest, gin.H{
				"error": err.Error(),
			})
		} else {
			fmt.Printf("%#v\n", u)
			c.JSON(http.StatusOK, gin.H{
				"status": "ok",
			})
		}
	})

	r.Run(":9090")
}

```





### 文件上传

#### 单个文件上传

文件上传前端页面代码：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <title>上传文件示例</title>
</head>
<body>
    <!--对文件、图片、音频等文件，enctype="multipart/form-data"，必须是这种类型，默认 enctype="x-www-form-urlencoded" 这种不能传二进制文件 -->
<form action="/upload" method="post" enctype="multipart/form-data">
    <input type="file" name="f1">
    <input type="submit" value="上传">
</form>
</body>
</html>
```

后端gin框架部分代码：

```go
func main() {
	r := gin.Default()
	// 处理multipart forms提交文件时默认的内存限制是32 MiB
	// 可以通过下面的方式修改
	// r.MaxMultipartMemory = 8 << 20  // 8 MiB
	r.LoadHTMLFiles("./index.html")
	r.GET("/index", func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.html", nil)
	})

	r.POST("/upload", func(c *gin.Context) {
		//从请求中读取文件
		//f1 是在index.html中定义的,即键
		f, err := c.FormFile("f1") //从请求中获取携带的参数一样的
		if err != nil {
			c.JSON(http.StatusBadRequest, gin.H{
				"status": err.Error(),
			})
		} else {
			//将读取到的文件保存在本地
			//dst := fmt.Sprintf("./%s",f.Filename)  //f.Filename 是上传文件的文件名
			//path.join():方法使用平台特定的分隔符[Unix系统是/，Windows系统是\ ]把全部给定的 path 片段连接到一起，并规范			//化生成的路径。
			dst := path.Join("./", f.Filename)
			c.SaveUploadedFile(f, dst) //gin框架中的保存文件的方法
			c.JSON(http.StatusOK, gin.H{
				"status": "ok",
			})
}
```





#### 多个文件上传

```go
func main() {
	router := gin.Default()
	// 处理multipart forms提交文件时默认的内存限制是32 MiB
	// 可以通过下面的方式修改
	// router.MaxMultipartMemory = 8 << 20  // 8 MiB
	router.POST("/upload", func(c *gin.Context) {
		// Multipart form
        //MultipartForm是已解析的多部分表单，包括文件上载
		form, _ := c.MultipartForm()
       
		files := form.File["file"]

		for index, file := range files {
			log.Println(file.Filename)
			dst := fmt.Sprintf("C:/tmp/%s_%d", file.Filename, index)
			// 上传文件到指定的目录
			c.SaveUploadedFile(file, dst)
		}
		c.JSON(http.StatusOK, gin.H{
			"message": fmt.Sprintf("%d files uploaded!", len(files)),
		})
	})
	router.Run()
}
```





### 重定向

#### HTTP重定向

HTTP 重定向很容易。 内部、外部重定向均支持。

```go
r.GET("/test", func(c *gin.Context) {
    //http 重定向
    //即当请求 127.0.0.1/test 会自动转向 --> http://www.sogo.com
    //注意：一旦重定向了，再将 http://www.sogo.com 修改为 www.baidu.com 时，访问/test路径时，还是会转向www.sogo.com，这是因为浏览器带缓存了
	c.Redirect(http.StatusMovedPermanently, "http://www.sogo.com/")
})
```



#### 路由重定向

路由重定向，使用`HandleContext`：

```go
r.GET("/test", func(c *gin.Context) {
    // 指定重定向的URL
    c.Request.URL.Path = "/test2"
    r.HandleContext(c)
})
r.GET("/test2", func(c *gin.Context) {
    c.JSON(http.StatusOK, gin.H{"hello": "world"})
})
```





```go
//示例：
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

//重定向

func main() {
	r := gin.Default()
	r.GET("/redirect", func(c *gin.Context) {
		//c.JSON(http.StatusOK,gin.H{
		//	"status":"ok",
		//})
		c.Redirect(http.StatusMovedPermanently, "http://www.sogo.com")
	})

	r.GET("/aaa", func(c *gin.Context) {
		
		//路由重定向,跳转到 /b 对应的路由处理函数
		c.Request.URL.Path = "/bbb" //将请求的URI进行修改
		r.HandleContext(c)          //继续后续处理
	})

	r.GET("/bbb", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"message": "bbbb",
		})
	})

	r.Run(":9090")
}

```







### Gin路由

#### 普通路由

```go
r.GET("/index", func(c *gin.Context) {...})
r.GET("/login", func(c *gin.Context) {...})
r.POST("/login", func(c *gin.Context) {...})
```

此外，还有一个可以匹配所有请求方法的`Any`方法如下：

```go
r.Any("/test", func(c *gin.Context) {...})
```



**为没有配置处理函数的路由添加处理程序，默认情况下它返回404代码，**下面的代码为没有匹配到路由的请求都返回`views/404.html`页面。

```go
//访问没有路由的路径，会执行下面代码
r.NoRoute(func(c *gin.Context) {
		c.HTML(http.StatusNotFound, "views/404.html", nil)
})
```





#### 路由组

我们可以将拥有共同URL前缀的路由划分为一个路由组。习惯性一对`{}`包裹同组的路由，这只是为了看着清晰，你用不用`{}`包裹功能上没什么区别。

```go
func main() {
	r := gin.Default()
	userGroup := r.Group("/user")
	{
		userGroup.GET("/index", func(c *gin.Context) {...})
		userGroup.GET("/login", func(c *gin.Context) {...})
		userGroup.POST("/login", func(c *gin.Context) {...})

	}
	shopGroup := r.Group("/shop")
	{
		shopGroup.GET("/index", func(c *gin.Context) {...})
		shopGroup.GET("/cart", func(c *gin.Context) {...})
		shopGroup.POST("/checkout", func(c *gin.Context) {...})
	}
    r.Run(":9090")
}
```

路由组也是支持嵌套的，例如：

```go
shopGroup := r.Group("/shop")
	{
		shopGroup.GET("/index", func(c *gin.Context) {...})
		shopGroup.GET("/cart", func(c *gin.Context) {...})
		shopGroup.POST("/checkout", func(c *gin.Context) {...})
		// 嵌套路由组
        //路由组也是可以嵌套，即在路由组内可以在分组，形成子路由组
		xx := shopGroup.Group("xx")
		xx.GET("/oo", func(c *gin.Context) {...})
	}
```

通常我们将路由分组用在划分业务逻辑或划分API版本时。



```go
//示例：
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	r := gin.Default()

	//访问/index的GET请求会走这一条处理逻辑
	//路由
	r.GET("/index", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"method": "GET",
		})
	})

	r.POST("/index", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"method": "POST",
		})
	})

	//使用Any时，所有请求方式 在请求该路径时都能得到响应
	//Any registers a route that matches all the HTTP methods.
	// GET, POST, PUT, PATCH, HEAD, OPTIONS, DELETE, CONNECT, TRACE.
	r.Any("/user", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"method": "Any",
		})
		//也可以使用下面方式，根据请求方式来进行不同的处理
		//switch c.Request.Method{
		//case "GET":
        
			////http.StatusOK 是http包中定义的常量
        	////同样，http.MethodPost也是事先http包中定义好的常量，尽量使用go语言提供的
        
		//	c.JSON(http.StatusOK,gin.H{"method":"GET"})
		//case http.MethodPost:
		//	c.JSON(http.StatusOK,gin.H{"method":http.MethodPost})
		////....
		//}
	})

	//没有路由的页面，即没有定义的页面
	r.NoRoute(func(c *gin.Context) {
		c.JSON(http.StatusNotFound, gin.H{"message": "liwenzhou.com"})
	})

	//路由组
	//视频的首页和详情页
	//r.GET("/video/index", func(c *gin.Context) {
	//	c.JSON(http.StatusOK, gin.H{"message": "/video/index"})
	//})
	//r.GET("/video/xxx", func(c *gin.Context) {
	//	c.JSON(http.StatusOK, gin.H{"message": "/video/xxx"})
	//})
	//r.GET("/video/ooo", func(c *gin.Context) {
	//	c.JSON(http.StatusOK, gin.H{"message": "/video/ooo"})
	//})

	//普通写法：
	//商城的首页和详情页
	r.GET("/shop/index", func(c *gin.Context) {
        
		c.JSON(http.StatusOK, gin.H{"message": "/shop/index"})
	})
	r.GET("/shop/xxx", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{"message": "/shop/xxx"})
	})
	r.GET("/shop/ooo", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{"message": "/shop/xxx"})
	})

    //路由的组，即路由组，多用于区分不同的业务线或API版本中
    
	//利用路由组写法：
	videoGroup := r.Group("/vedio") //利用公共前缀部分定义一个路由组
    
	{  //该花括号不是必须的，写上会显得结构更加清晰
        
		videoGroup.GET("/index", func(c *gin.Context) {
			c.JSON(http.StatusOK, gin.H{"message": "/video/index"})
		})
		videoGroup.GET("/xxx", func(c *gin.Context) {
			c.JSON(http.StatusOK, gin.H{"message": "/video/xxx"})
		})
		videoGroup.GET("/ooo", func(c *gin.Context) {
			c.JSON(http.StatusOK, gin.H{"message": "/video/ooo"})
		})
	}
	r.Run(":9090")

}

```



#### 路由原理

Gin框架中的路由使用的是[httprouter](https://github.com/julienschmidt/httprouter)这个库。

**其基本原理就是构造一个路由地址的前缀树。**





### Gin中间件

Gin框架==允许**开发者**在处理请求的过程中，**加入用户自己的钩子（Hook）函数**==。这个钩子函数就叫中间件，**中间件适合处理一些公共的业务逻辑，比如登录认证、权限校验、数据分页、记录日志、耗时统计等。**





![image-20230211153400909](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230211153400909.png)









#### 定义中间件

==Gin中的中间件必须是一个`gin.HandlerFunc`类型。==



##### 中间件中主要函数

```go
//调用后续func(*gin.Context){...}函数
func (c *Context) Next()

//阻止调用后续func(*gin.Context){...}函数
func (c *Context) Abort(）
                        
//全局注册中间件,将中间连接到路由器中
func (engine *Engine) Use(middleware ...HandlerFunc) IRoutes     
                        
                        
```

中间件存在的形式:
1, **给每个路由处理注册中间件**
2, **在路由处理之前,全局注册中间件,后续路由都会用到该中间件**
3, **给路由组注册中间件,该路由组内的路由处理函数都会用到该中间件**





##### 记录接口耗时的中间件

例如我们像下面的代码一样定义一个统计请求耗时的中间件。

```go
// StatCost 是一个统计耗时请求耗时的中间件
func StatCost() gin.HandlerFunc {
	return func(c *gin.Context) {
		start := time.Now()
		c.Set("name", "小王子") // 可以通过c.Set在请求上下文中设置值，后续的处理函数能够取到该值
		// 调用该请求的剩余处理程序
		c.Next()
		// 不调用该请求的剩余处理程序
		// c.Abort()
		// 计算耗时
		cost := time.Since(start)
		log.Println(cost)
	}
}
```



##### 记录响应的中间件

我们有时候可能会想要记录下某些情况下返回给客户端的响应数据，这个时候就可以编写一个中间件来搞定。

```go
type bodyLogWriter struct {
	gin.ResponseWriter               // 嵌入gin框架ResponseWriter
	body               *bytes.Buffer // 我们记录用的response
}

// Write 写入响应体数据
func (w bodyLogWriter) Write(b []byte) (int, error) {
	w.body.Write(b)                  // 我们记录一份
	return w.ResponseWriter.Write(b) // 真正写入响应
}

// ginBodyLogMiddleware 一个记录返回给客户端响应体的中间件
// https://stackoverflow.com/questions/38501325/how-to-log-response-body-in-gin
func ginBodyLogMiddleware(c *gin.Context) {
	blw := &bodyLogWriter{body: bytes.NewBuffer([]byte{}), ResponseWriter: c.Writer}
	c.Writer = blw // 使用我们自定义的类型替换默认的

	c.Next() // 执行业务逻辑

	fmt.Println("Response body: " + blw.body.String()) // 事后按需记录返回的响应
}
```





##### 跨域中间件cors

推荐使用社区的https://github.com/gin-contrib/cors 库，一行代码解决前后端分离架构下的跨域问题。

**注意：** 该中间件需要注册在业务处理函数前面。

这个库支持各种常用的配置项，具体使用方法如下。

```go
package main

import (
  "time"

  "github.com/gin-contrib/cors"
  "github.com/gin-gonic/gin"
)

func main() {
  router := gin.Default()
  // CORS for https://foo.com and https://github.com origins, allowing:
  // - PUT and PATCH methods
  // - Origin header
  // - Credentials share
  // - Preflight requests cached for 12 hours
  router.Use(cors.New(cors.Config{
    AllowOrigins:     []string{"https://foo.com"},  // 允许跨域发来请求的网站
    AllowMethods:     []string{"GET", "POST", "PUT", "DELETE",  "OPTIONS"},  // 允许的请求方法
    AllowHeaders:     []string{"Origin", "Authorization", "Content-Type"},
    ExposeHeaders:    []string{"Content-Length"},
    AllowCredentials: true,
    AllowOriginFunc: func(origin string) bool {  // 自定义过滤源站的方法
      return origin == "https://github.com"
    },
    MaxAge: 12 * time.Hour,
  }))
  router.Run()
}
```

当然你可以简单的像下面的示例代码那样使用默认配置，允许所有的跨域请求。

```go
func main() {
  router := gin.Default()
  // same as
  // config := cors.DefaultConfig()
  // config.AllowAllOrigins = true
  // router.Use(cors.New(config))
  router.Use(cors.Default())
  router.Run()
}
```





#### 注册中间件

在gin框架中，我们可以为每个路由添加任意数量的中间件。



##### 为全局路由注册

```go
func main() {
	// 新建一个没有任何默认中间件的路由
	r := gin.New()
	// 注册一个全局中间件
	r.Use(StatCost())
	
	r.GET("/test", func(c *gin.Context) {
		name := c.MustGet("name").(string) // 从上下文取值
		log.Println(name)
		c.JSON(http.StatusOK, gin.H{
			"message": "Hello world!",
		})
	})
	r.Run()
}
```



##### 为某个路由单独注册

```go
// 给/test2路由单独注册中间件（可注册多个）
	r.GET("/test2", StatCost(), func(c *gin.Context) {
		name := c.MustGet("name").(string) // 从上下文取值
		log.Println(name)
		c.JSON(http.StatusOK, gin.H{
			"message": "Hello world!",
		})
	})
```



##### 为路由组注册中间件

为路由组注册中间件有以下两种写法。

**写法1：**

```go
//可以为路由组注册中间件
shopGroup := r.Group("/shop", StatCost())
{
    shopGroup.GET("/index", func(c *gin.Context) {...})
    ...
}
```

**写法2：**

```go
shopGroup := r.Group("/shop")
shopGroup.Use(StatCost())  //这种写法也是可以的，同时要注意：可以为子路由组再添加中间件
{
    shopGroup.GET("/index", func(c *gin.Context) {...})
    ...
}
```

==可以为路由组添加中间件，也可以为子路由组添加中间件==



#### 中间件注意事项

##### gin默认中间件

`gin.Default()`默认使用了`Logger`和`Recovery`中间件，其中：

- `Logger`中间件将日志写入`gin.DefaultWriter`，即使配置了`GIN_MODE=release`。
- `Recovery`中间件会recover任何`panic`。如果有panic的话，会写入500响应码。

如果不想使用上面两个默认的中间件，可以使用`gin.New()`新建一个没有任何默认中间件的路由。



##### gin中间件中使用goroutine

==当在中间件或`handlerFunc`中启动新的`goroutine`时，**不能使用**原始的上下文（c *gin.Context），必须使用其只读副本（`c.Copy()`）。==

==因为，如果直接传入原始的`c`，一旦在goroutine中出现了对`c`的修改，那么会影响整个后续的`c`，为了并发安全，只能用`c`的副本，即只能从`c`中读取消息，而不能修改信息。==



```go
//示例；
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
	"net/http"
	"time"
)

//中间件

//定义一个中间件
func m1(c *gin.Context) {
	fmt.Println("m1")
	start := time.Now()
	c.Next() //调用后续的处理函数
	cost := time.Since(start)
	fmt.Printf("cost:%v\n", cost)

	fmt.Println("退出m1函数")
	//c.Abort() //阻止调用后续的处理函数
}

func m2(c *gin.Context) {

	//Set用于专门为此上下文存储新的键/值对。如果以前没有使用，它也会惰性地初始化c键
	c.Set("name", "qimi") //在上下文中存储值，中间件
}

//在实际中，中间件一般以闭包的形式返回
func authMiddleware(doCheck bool) gin.HandlerFunc {
	//连接数据库
	//或其他一些准备工作
	return func(c *gin.Context) {
		//是否登录的判断
		// if 是登录用户
		// c.Next()
		//esle
		//c.Abort()
	}
}

func main() {
	r := gin.Default() //默认路由引擎

	//这种形式是在每个路由处理时加入中间件，可以加一个全局中间件吗？ 可以
	//r.GET("/index", m1, func(c *gin.Context) {
	//	c.JSON(http.StatusOK, gin.H{
	//		"msg": "index",
	//	})
	//})
	//
	//r.GET("/shop",m1, func(c *gin.Context) {
	//	c.JSON(http.StatusOK,gin.H{
	//		"msg":"shop",
	//	})
	//})

	//全局注册一个中间件函数m1,效果跟上面方式一样
	r.Use(m1, m2, authMiddleware(true))
	r.GET("/index", func(c *gin.Context) {
		fmt.Println("in")

		//Get返回给定键的值，即：（value，true）。如果该值不存在，则返回（nil，false）
		name, ok := c.Get("name")  //从上下文中取值，实现跨中间件取值
		if !ok {
			name = "匿名用户"
		}
		c.JSON(http.StatusOK, gin.H{
			"name": name,
		})
		fmt.Println("out")
	})
	r.GET("/shop", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"msg": "shop",
		})
	})

	r.Run(":9090")
}

```



==特别需要注意多个中间件的执行顺序，还需注意：`func(c *gin.context{})`执行过了就不会再次执行了，那么有`c.next()`：==

```go
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	r := gin.Default()

	r1 := r.Group("/xxx", f())
	{
		r1.GET("/get", f1(), func(c *gin.Context) {
			fmt.Println("f1()")
			c.JSON(http.StatusOK, gin.H{
				"name": "haha",
			})
		})
	}
	r.Run(":9090")
}

func f() func(c *gin.Context) {
	return func(c *gin.Context) {
		fmt.Println("1")
		c.Next()
		fmt.Println("11")
		c.Abort()
		fmt.Println("111")
	}
}

func f1() func(c *gin.Context) {
	return func(c *gin.Context) {
		fmt.Println(2)
		c.Next()
		fmt.Println(22)
		c.Abort()
		fmt.Println(222)
	}
}



/*
1
2
f1()
22
222
11
111
*/
```





### 运行多个服务

我们可以在多个端口启动服务，例如：

```go
package main

import (
	"log"
	"net/http"
	"time"

	"github.com/gin-gonic/gin"
	"golang.org/x/sync/errgroup"
)

var (
	g errgroup.Group   //处理并发错误
)

func router01() http.Handler {
	e := gin.New()
	e.Use(gin.Recovery())
	e.GET("/", func(c *gin.Context) {
		c.JSON(
			http.StatusOK,
			gin.H{
				"code":  http.StatusOK,
				"error": "Welcome server 01",
			},
		)
	})

	return e
}

func router02() http.Handler {
	e := gin.New()
	e.Use(gin.Recovery())
	e.GET("/", func(c *gin.Context) {
		c.JSON(
			http.StatusOK,
			gin.H{
				"code":  http.StatusOK,
				"error": "Welcome server 02",
			},
		)
	})

	return e
}

func main() {
	server01 := &http.Server{
		Addr:         ":8080",
		Handler:      router01(),
		ReadTimeout:  5 * time.Second,
		WriteTimeout: 10 * time.Second,
	}

	server02 := &http.Server{
		Addr:         ":8081",
		Handler:      router02(),
		ReadTimeout:  5 * time.Second,
		WriteTimeout: 10 * time.Second,
	}
   // 借助errgroup.Group或者自行开启两个goroutine分别启动两个服务
	g.Go(func() error {
		return server01.ListenAndServe()
	})

	g.Go(func() error {
		return server02.ListenAndServe()
	})

	if err := g.Wait(); err != nil {
		log.Fatal(err)
	}
}
```





## 小清单项目实现

```go
//项目代码
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
	"gorm.io/gorm/logger"
	"net/http"
	"strconv"
)

type Todo struct {
	ID     int    `json:"id"`
	Title  string `json:"title"`
	Status bool   `json:"status"`
}

var DB *gorm.DB

func initDB() (err error) {
	//初始化，连接数据库
	username := "root"
	password := "kxh19970301"
	dbname := "bubble"
	dsn := fmt.Sprintf("%s:%s@tcp(127.0.0.1:3306)/%s?charset=utf8mb4&parseTime=True&loc=Local", username, password, dbname)
	//dsn:="root:kxh19970301@tcp(127.0.0.1)/sql_test"
	//官方示例 dsn := "user:pass@tcp(127.0.0.1:3306)/dbname?charset=utf8mb4&parseTime=True&loc=Local"
	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{
		//设置一个日志的级别，可以设置也可以不设置
		Logger: logger.Default.LogMode(logger.Info),
	})
	if err != nil {
		fmt.Printf("open mysql failed,err:%v\n", err)
		return
	}
	DB = db
	return
}

//模板定义、模板解析、模板渲染
func main() {
	//创建数据库
	//连接数据库
	err := initDB()
	if err != nil {
		fmt.Println("init mysql failed,err:%v\n", err)
		return
	}
	sqlDB, _ := DB.DB()
	defer sqlDB.Close() //关闭数据库连接
	//模型绑定，自动建表
	DB.AutoMigrate(&Todo{})

	//根据用户请求实现增删改查

	r := gin.Default()
	//在解析模板文件之前，加载静态文件，告诉gin框架的模板文件中引用的静态文件去哪找
	r.Static("./static", "./static")
	r.LoadHTMLFiles("./template/index.html")
	r.GET("/", func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.html", nil)

	})

	//v1
	v1Group := r.Group("/v1")
	{
		//事务待办
		//添加
		v1Group.POST("/todo", func(c *gin.Context) {
			//前端页面填写待办事项，点击提交，发请求到这里
			//1，从请求中读取数据
			var todo Todo
			c.BindJSON(&todo)
			//2，然后保存在数据库中  3，返回响应
			if err := DB.Create(&todo).Error; err != nil {
				c.JSON(http.StatusOK, gin.H{
					"error": err.Error(),
				})
			} else {
				c.JSON(http.StatusOK, todo)
			}

		})
		//查看所有的待办事项
		v1Group.GET("/todo", func(c *gin.Context) {
			//查询todos这张表中的所有数据
			var todoList []Todo
			if err := DB.Find(&todoList).Error; err != nil {
				c.JSON(http.StatusOK, gin.H{
					"error": err.Error(),
				})
			} else {
				c.JSON(http.StatusOK, todoList)
			}
		})
		//查看某一个待办事项
		v1Group.GET("/todo/:id", func(c *gin.Context) {

		})
		//修改某一个待办事项
		v1Group.PUT("/todo/:id", func(c *gin.Context) {
			id := c.Params.ByName("id")
			var todo Todo
			if err := DB.Where("id=?", id).First(&todo).Error; err != nil {
				c.JSON(http.StatusOK, gin.H{
					"error": err.Error(),
				})
				return
			}
			c.BindJSON(&todo)
			if err = DB.Save(&todo).Error; err != nil {
				c.JSON(http.StatusOK, gin.H{
					"error": err.Error(),
				})
			} else {
				c.JSON(http.StatusOK, todo)
			}
		})
		//删除某一个待办事项
		v1Group.DELETE("/todo/:id", func(c *gin.Context) {
			id := c.Param("id")
			id1, _ := strconv.Atoi(id)
			if err := DB.Where("id = ?", id1).Delete(&Todo{}).Error; err != nil {
				c.JSON(http.StatusOK, gin.H{
					"error": err.Error(),
				})
			} else {
				c.JSON(http.StatusOK, gin.H{
					id: "deleted",
				})
			}
		})
	}

	r.Run(":9090")
}

```

根据结构体，利用gorm中的自动建表，表各字段类型如下：

![image-20230214011413918](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230214011413918.png)





## 企业级项目结构拆分















# gRPC教程

## 什么是微服务

### 单体架构

![image-20230215143058808](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230215143058808.png)

**单体架构的缺点：**

+ **一旦某个服务宕机，会引起整个应用不可用**，隔离性差
+ 应用只能整体进行伸缩，浪费资源，可伸缩性差
+ 代码耦合在一起，可维护性差



### 微服务架构

要想解决上述的单体架构存在的问题，就需要将服务拆分出来，单独维护和管理。

![image-20230215143345944](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230215143345944.png)

**通过上述架构，解决了单体架构的弊端。但是又引入了新的问题：**

+ 代码冗余
+ 服务和服务之间存在调用关系



#### 代码冗余问题

> 服务未拆分之前，公共的功能有统一的实现，比如认证，授权，限流等，但是服务拆分之后，每一个服务可能都需要实现一遍

**解决方案：**

1. 由于为了保持对外提供服务的一致性，==引入了网关的概念，由网关根据不同的请求，将其转发到不同的服务（路由功能），由于入口的一致性，可以在网关上实现公共的一些功能。==
2. 可以将公共的功能抽取出来，形成一个新的服务，比如统一认证中心。





#### 服务之间调用

服务拆分后，服务和服务之间发生的是进程和进程之间的调用，服务器和服务器之间的调用。

==那么就需要发起网络调用，网络调用我们能立马想起的就是http，但是在微服务架构中，http虽然便捷方便，但性能较低，这时候就**需要引入RPC（远程过程调用），通过自定义协议发起TCP调用，来加快传输效率**。==

> 每个服务由于可能分布在成千上百台机器上，服务和服务之间的调用，会出现一些问题，比如，如何知道应该调用哪台机器上的服务，调用方可能需要维护被调用方的地址，这个地址可能很多，增加了额外的负担，这时候就需要引入服务治理.

**服务治理**中有一个重要的概念`服务发现`，**服务发现**中有一个重要的概念叫做`注册中心`。

![image-20220422233632635](https://www.mszlu.com/assets/image-20220422233632635.ba57afe4.png)

==每个服务启动的时候，会将自身的服务和ip注册到注册中心，其他服务调用的时候，只需要向注册中心申请地址即可。==

> 当然，服务和服务之间调用会发生一些问题，为了避免产生连锁的雪崩反应，引入了服务容错，为了追踪一个调用所经过的服务，引入了链路追踪，等等这些就构建了一个微服务的生态







## gRPC

> 上面我们讲到，服务和服务之间调用需要使用RPC，`gRPC`是一款**语言中立**、**平台中立**、开源的远程过程调用系统，`gRPC`客户端和服务端可以在多种环境中运行和交互，例如用`java`写一个服务端，可以用`go`语言写客户端调用

数据在进行网络传输的时候，需要进行序列化，序列化协议有很多种，比如xml, json，protobuf等

==gRPC默认使用`protocol buffers`，这是google开源的一套成熟的结构数据序列化机制。==

在学习gRPC之前，需要先了解`protocol buffers`

**序列化**：将数据结构或对象转换成二进制串的过程。

**反序列化**：将在序列化过程中所产生的二进制串转换成数据结构或对象的过程



更详细的资料可以从官网上获取：[Quick start | Go | gRPC](https://grpc.io/docs/languages/go/quickstart/)

中文文档：http://doc.oschina.net/grpc



## protobuf

==protobuf是谷歌开源的一种数据格式，**适合高性能，对响应速度有要求**的数据传输场景==。因为profobuf是二进制数据格式，需要编码和解码。数据本身不具有可读性。因此只能反序列化之后得到真正可读的数据。

**优势：**

1. **序列化后体积相比Json和XML很小，适合网络传输**
2. 支持跨平台多语言
3. 消息格式升级和兼容性还不错
4. **序列化反序列化速度很快**



### 安装

+ 第一步：下载通用编译器，下载地址：[Releases · protocolbuffers/protobuf (github.com)](https://github.com/protocolbuffers/protobuf/releases)，然后根据不同操作系统下载相应的包即可。

  ![image-20230215152116064](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230215152116064.png)

+ 第二步：配置环境变量

  ![image-20230215152024038](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230215152024038.png)

+ 安装go专用的protoc的生成器

  ```go
  go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
  go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
  ```

  然后，可以看到在`GOPATH\bin`目录下由两个可执行文件`protoc-gen-go.exe和protoc-gen-go-grpc.exe`

  

  安装后会在`GOPATH`目录下生成可执行文件，protobuf的编译器插件`protoc-gen-go`，执行`protoc`命令会**自动调用这个插件**

  > 如何使用protobuf呢？

  1. 定义了一种源文件，扩展名为 `.proto`，使用这种源文件，可以定义存储类的内容(消息类型)
  2. protobuf有自己的编译器 `protoc`，可以将 `.proto` 编译成对应语言的文件，就可以进行使用了



### 编写Hello的proto文件

```go
//proto/hello.proto
//proto文件其实是一个约束


//这是在说明我们使用的是proto3语法
syntax = "proto3";

//这部分的内容是关于最后生成的go文件是处在哪个目录哪个包中，.代表在当前目录生成，service代表了生成的go文件的包名是service
option go_package =".;service";

//然后，我们需要定义一个服务，在这个服务中需要有一个方法，这个方法可以接受客户端的参数，再返回服务端响应
//其实很容易看出，我们定义一个service，称为SayHello，这个服务中有一个rpc方法，名为SayHello.
//这个方法会发送一个HelloRequest,然后返回一个HelloResponse.

//这里定义一个服务,这个服务里面有很多方法,方法需要接收请求参数
service SayHello {
  rpc SayHello(HelloRequest) returns (HelloResponse){}
}


//message关键字,其实可以理解为Golang中的结构体
//这里比较特别的是变量后面的"赋值".注意:这里并不是赋值,而是在定义这个变量在message中的位置.
//消息,要传输的对象
message HelloRequest{
  string requestName=1;  //1代表的是标识号,即生成go代码时会将其放在结构体中第一行
//  int64  age = 2;  //2也是代表标识号,即生成go代码是会将其放在结构体中第二行
}
message  HelloResponse{
  string responseMsg = 1;
}
```

编写完成后，可以在该`hello.proto`文件下执行如下命令：

```go
protoc --go_out=. hello.proto        //--go_out=.表示输出位置为当前文件夹，位置可以换，后面的hello.proto 要编译的文件
protoc --go-grpc_out=. hello.proto   //--go-grpc_out=. 也是设置输出的位置，后面hello.proto为要编译的文件名
```



```go
//测试是否可以执行proto的序列化和反序列
package main

import (
	"fmt"
	"google.golang.org/protobuf/proto"
	service "grpc/proto"
)

func main() {
	r := service.HelloRequest{
		RequestName: "哈哈",
	}
    //marshal只能传入地址
	b, err := proto.Marshal(&r)
	if err != nil {
		panic(err)
	}
	u := service.HelloRequest{}
	err = proto.Unmarshal(b, &u)
	if err != nil {
		panic(err)
	}
	fmt.Println(u.String())  //输出结果为：requestName:"哈哈"
}
```





### proto文件介绍

#### message介绍

`message`：`protobuf`中定义一个消息类型是通过关键字`message`字段指定的。

**消息就是需要传输的数据格式的定义**。

message关键字类似于C++中的class，Java中的class，go中的struct

例如：

```protobuf
message User {
  string username = 1; //1为标识号
  int32 age = 2;
}
```

在消息中承载的数据分别对应于每一个字段。

其中每个字段都有一个名字和一种类型 。



#### 字段规则

- `required`:消息体中必填字段，不设置会导致编解码异常。
- `optional`: 消息体中可选字段。
- `repeated`: 消息体中可重复字段，重复的值的顺序会被保留，在go中重复的会被定义为切片。
- ==在protobuf3中没有了required、optional等说明关键字，都默认为optional==

```protobuf
message User {
  string username = 1; 
  int32 age = 2;
  string password = 3;
  repeated string address = 4;
}
```





#### 标识号

`标识号`：在消息体的定义中，==每个字段都必须要有一个**唯一**的标识号==，标识号是[0,2^29-1]范围内的一个整数。

```protobuf
message Person { 
  string name = 1;  // (位置1)
  int32 id = 2;  
  string email = 3;  
  repeated string phones = 4; // (位置4)  //在message中消息号随着字段个数进行累加
}
```

以Person为例，name=1，id=2, email=3, phones=4 中的1-4就是标识号。





#### 字段映射

| **.proto Type** | **Notes**                                                    | **C++ Type** | **Python Type** | **Go Type** |
| --------------- | ------------------------------------------------------------ | ------------ | --------------- | ----------- |
| double          |                                                              | double       | float           | float64     |
| float           |                                                              | float        | float           | float32     |
| int32           | 使用变长编码，对于负值的效率很低，如果你的域有 可能有负值，请使用sint32替代 | int32        | int             | int32       |
| int64           | 使用变长编码，对于负值的效率很低，如果你的域有 可能有负值，请使用sint64替代 | int64        | int             | int64       |
| uint32          | 使用变长编码                                                 | uint32       | int/long        | uint32      |
| uint64          | 使用变长编码                                                 | uint64       | int/long        | uint64      |
| sint32          | 使用变长编码，这些编码在负值时比int32高效的多                | int32        | int             | int32       |
| sint64          | 使用变长编码，有符号的整型值。编码时比通常的 int64高效。     | int64        | int/long        | int64       |
| fixed32         | 总是4个字节，如果数值总是比总是比228大的话，这 个类型会比uint32高效。 | uint32       | int             | uint32      |
| fixed64         | 总是8个字节，如果数值总是比总是比256大的话，这 个类型会比uint64高效。 | uint64       | int/long        | uint64      |
| sfixed32        | 总是4个字节                                                  | int32        | int             | int32       |
| sfixed32        | 总是4个字节                                                  | int32        | int             | int32       |
| sfixed64        | 总是8个字节                                                  | int64        | int/long        | int64       |
| bool            |                                                              | bool         | bool            | bool        |
| string          | 一个字符串必须是UTF-8编码或者7-bit ASCII编码的文 本。        | string       | str/unicode     | string      |
| bytes           | 可能包含任意顺序的字节数据。                                 | string       | str             | []byte      |



#### 默认值

protobuf3 删除了 protobuf2 中用来设置默认值的 default 关键字，取而代之的是protobuf3为各类型定义的默认值，也就是约定的默认值，如下表所示：

| 类型     | 默认值                                                       |
| :------- | :----------------------------------------------------------- |
| bool     | false                                                        |
| 整型     | 0                                                            |
| string   | 空字符串""                                                   |
| 枚举enum | 第一个枚举元素的值，因为Protobuf3强制要求第一个枚举元素的值必须是0，所以枚举的默认值就是0； |
| message  | 不是null，而是DEFAULT_INSTANCE                               |



#### 定义多个消息类型

一个proto文件中可以定义多个消息类型

```go
message UserRequest {
  string username = 1;
  int32 age = 2;
  string password = 3;
  repeated string address = 4;
}

message UserResponse {
  string username = 1;
  int32 age = 2;
  string password = 3;
  repeated string address = 4;
}
```



#### 消息嵌套

==可以在其他消息类型中**定义、使用**消息类型==，在下面的例子中，Person消息就定义在PersonInfo消息内，如 ：

```protobuf
message PersonInfo {
    message Person {  //定义
        string name = 1;
        int32 height = 2;
        repeated int32 weight = 3;
    } 
	repeated Person info = 1;  //使用
}
```

==如果你想在它的父消息类型的外部重用这个消息类型==，你需要以PersonInfo.Person的形式使用它，如：

```protobuf
message PersonMessage {
	PersonInfo.Person info = 1; //对于嵌套类型，必须将父类型也要写出来，即必须写全
}
```

当然，你也可以将消息嵌套任意多层，如 :

```protobuf
message Grandpa { // Level 0
    message Father { // Level 1
        message son { // Level 2
            string name = 1;
            int32 age = 2;
    	}
	} 
    message Uncle { // Level 1
        message Son { // Level 2
            string name = 1;
            int32 age = 2;
        }
    }
}
```



#### 定义服务（service）

==如果想要将消息类型用在RPC系统中，可以在.proto文件中定义一个RPC服务接口==，protocol buffer 编译器将会根据所选择的不同语言生成服务接口代码及存根。

```protobuf
service SearchService {
	//rpc 服务的函数名 （传入参数）返回（返回参数），rpc可以理解为golang中的func
	rpc Search (SearchRequest) returns (SearchResponse){};  //相当于定义一个方法，需要自己实现
	//如果有需求，可以继续在下面定义多个方法
}
```

上述代表表示，定义了一个RPC服务，该方法接收SearchRequest返回SearchResponse





##  gRPC实例

### RPC和gRPC介绍

RPC（Remote Procedure Call）==远程过程调用协议==，一种通过网络从远程计算机上请求服务，而不需要了解底层网络技术的协议。RPC它假定某些协议的存在，例如TCP/UDP等，为通信程序之间携带信息数据。在OSI网络七层模型中，RPC跨越了传输层和应用层，RPC使得开发包括网络分布式多程序在内的应用程序更加容易。

过程是什么？ 过程就是业务处理、计算任务，更直白的说，就是程序，就是像调用本地方法一样调用远程的过程

RPC采用客户端/服务端的模式，通过request-response消息模式实现

![image-20220424111303405](https://www.mszlu.com/assets/image-20220424111303405.ff0c100d.png)

gRPC 里**客户端**应用可以像调用本地对象一样直接调用另一台不同的机器上**服务端**应用的方法，使得您能够更容易地创建分布式应用和服务。与许多 RPC 系统类似，gRPC 也是基于以下理念：定义==**服务**==，指定其能够被远程调用的方法（包含参数和返回类型）。在服务端实现这个接口，并运行一个 gRPC 服务器来处理客户端调用。在客户端拥有一个**存根**能够像服务端一样的方法。

![image-20220424111455580](https://www.mszlu.com/assets/image-20220424111455580.e7637358.png)

官方网站：https://grpc.io/

底层协议：

- HTTP2: https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md
- GRPC-WEB ： https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md





### HTTP2

![image-20220424112609954](https://www.mszlu.com/assets/image-20220424112609954.63e50baa.png)

- HTTP/1里的header对应HTTP/2里的 HEADERS frame
- HTTP/1里的payload对应HTTP/2里的 DATA frame

gRPC把元数据放到HTTP/2 Headers里，请求参数序列化之后放到 DATA frame里

**基于HTTP/2 协议的优点**

1. 公开标准
2. HTTP/2的前身是Google的[SPDYopen in new window](https://en.wikipedia.org/wiki/SPDY) ，有经过实践检验
3. HTTP/2 天然支持物联网、手机、浏览器
4. 基于HTTP/2 多语言客户端实现容易
   1. 每个流行的编程语言都会有成熟的HTTP/2 Client
   2. HTTP/2 Client是经过充分测试，可靠的
   3. 用Client发送HTTP/2请求的难度远低于用socket发送数据包/解析数据包
5. HTTP/2支持Stream和流控
6. 基于HTTP/2 在Gateway/Proxy很容易支持
   1. nginx和envoy都有支持
7. HTTP/2 安全性有保证
   1. HTTP/2 天然支持SSL，当然gRPC可以跑在clear text协议（即不加密）上。
   2. 很多私有协议的rpc可能自己包装了一层TLS支持，使用起来也非常复杂。开发者是否有足够的安全知识？使用者是否配置对了？运维者是否能正确理解？
   3. HTTP/2 在公有网络上的传输上有保证。比如这个[CRIME攻击open in new window](https://en.wikipedia.org/wiki/CRIME)，私有协议很难保证没有这样子的漏洞。
8. HTTP/2 鉴权成熟
   1. 从HTTP/1发展起来的鉴权系统已经很成熟了，可以无缝用在HTTP/2上
   2. 可以从前端到后端完全打通的鉴权，不需要做任何转换适配

**基于HTTP/2 协议的缺点**

- rpc的元数据的传输不够高效

  尽管HPAC可以压缩HTTP Header，但是对于rpc来说，确定一个函数调用，可以简化为一个int，只要两端去协商过一次，后面直接查表就可以了，不需要像HPAC那样编码解码。 可以考虑专门对gRPC做一个优化过的HTTP/2解析器，减少一些通用的处理，感觉可以提升性能。

- HTTP/2 里一次gRPC调用需要解码两次

  一次是HEADERS frame，一次是DATA frame。

- HTTP/2 标准本身是只有一个TCP连接，但是实际在gRPC里是会有多个TCP连接，使用时需要注意。

gRPC选择基于HTTP/2，那么它的性能肯定不会是最顶尖的。但是对于rpc来说中庸的qps可以接受，通用和兼容性才是最重要的事情。

- 官方的benchmark：https://grpc.io/docs/guides/benchmarking.html
- https://github.com/hank-whu/rpc-benchmark



gRPC目前是k8s生态里的事实标准，而Kubernetes又是容器编排的事实标准。gRPC已经广泛应用于Istio体系，包括:

- Envoy与Pilot(现在叫istiod)间的XDS协议
- mixer的handler扩展协议
- MCP(控制面的配置分发协议)

在Cloud Native的潮流下，开放互通的需求必然会产生基于HTTP/2的RPC





### 实例

#### 服务端

+ 创建gRPC Server对象，可以理解为它是Server端的抽象对象
+ 将server（其包含需要被调用的服务端接口）注册到gRPC Server的内部注册中心，这样可以在接收请求时，通过内部的服务发现，发现该服务端接口并转接进行逻辑处理
+ 创建Listen，监听tcp端口
+ gRPC Server开始`lis.Accept`，直到Stop。

```go
package main

import (
	"context"
	"errors"
	"fmt"
	"google.golang.org/grpc"
	"google.golang.org/grpc/credentials/insecure"
	"google.golang.org/grpc/metadata"
	service "grpc/proto" //这里之所以要这么写,就是因为包名和路径名不一致
	"net"
)

type server struct {
	service.UnimplementedSayHelloServer
}

// 方法重写，注意：必须使用指针接收者，因为需要实现接口SayHelloServer
func (s *server) SayHello(ctx context.Context, req *service.HelloRequest) (*service.HelloResponse, error) {

	//获取元数据信息
	md, ok := metadata.FromIncomingContext(ctx)
	if !ok {
		return nil, errors.New("未传输token")
	}
	var appId string
	var appKey string
	//注意：如果在客户端中，appId和appKey是大写，这里必须小写才能行
	if v, ok := md["appid"]; ok {
		appId = v[0]
	}
	if v, ok := md["appkey"]; ok {
		appKey = v[0]
	}
	if appId != "xiao" || appKey != "123" {
		return nil, errors.New("token不正确")
	}

	fmt.Println("server端 SayHello方法")
	return &service.HelloResponse{ResponseMsg: "hello" + req.GetRequestName()}, nil
}

func main() {
	//TSL认证，有两个参数cretFile，keyFile
	//我们需要将刚才生成的自签名证书test.pem 和 私钥文件 test.key 放进去即可
	//creds, _ := credentials.NewServerTLSFromFile("D:\\学习笔记\\后端学习资料\\grpc\\key\\test.pem",
	//	"D:\\学习笔记\\后端学习资料\\grpc\\key\\test.key")

	//开启端口
	listen, err := net.Listen("tcp", ":9090")
	if err != nil {
		fmt.Printf("listen failed,err:%v\n", err)
		return
	}
	//创建grpc服务,将服务暴露供别人使用
	grpcServer := grpc.NewServer(grpc.Creds(insecure.NewCredentials()))

	//在grpc服务端中注册我们自己编写的服务
	//这里必须通过引用注册,即必须是&server{},因为上面方法实现是通过指针实现的,而RegisterSayHelloServer第二个参数
	//是接口类型,指针实现的方法只能传入指针类型才能得到该方法
	service.RegisterSayHelloServer(grpcServer, &server{}) //注册服务和方法

	//启动服务
	err = grpcServer.Serve(listen)
	if err != nil {
		fmt.Println("grpcServer failed,err:", err)
		return
	}
}

```







#### 客户端

+ 创建与给定目标（服务端）的连接交互
+ 创建server的客户端对象
+ 发送RPC请求，等待同步响应，得到回调后返回响应结果。
+ 输出响应结果。

```go
package main

import (
	"context"
	"fmt"
	"google.golang.org/grpc"
	"google.golang.org/grpc/credentials/insecure"
	service "grpc/proto"
)

func main() {
	//连接到server端,此处禁用了安全传输,没有加密和验证
	conn, err := grpc.Dial("127.0.0.1:9090", grpc.WithTransportCredentials(insecure.NewCredentials()))
	if err != nil {
		fmt.Printf("connect failed,err:%v\n", err)
		return
	}
	defer conn.Close()

	//建立连接
	client := service.NewSayHelloClient(conn)

    //执行rpc调用(这个方法在服务器端已经实现了),调用的SayHello方法是服务端实现的方法(即刚才服务端重写的方法)
	resp, err := client.SayHello(context.Background(), &service.HelloRequest{RequestName: "哈哈"})
	if err != nil {
		fmt.Println("client.SayHello failed,err:", err)
		return
	}
	//获取回复的消息
	fmt.Println(resp.GetResponseMsg())
}

```





**上面客户端、服务端用到的`Hello.proto`文件：**

```go
//proto文件其实是一个约束


//这是在说明我们使用的是proto3语法
syntax = "proto3";

//这部分的内容是关于最后生成的go文件是处在哪个目录哪个包中，代表在当前目录生成，service代表了生成的go文件的包名是service
option go_package =".;server";

//然后，我们需要定义一个服务，在这个服务中需要有一个方法，这个方法可以接受客户端的参数，再返回服务端响应
//其实很容易看出，我们定义一个service，称为SayHello，这个服务中有一个rpc方法，名为SayHello.
//这个方法会发送一个HelloRequest,然后返回一个HelloResponse.

//这里定义一个服务,这个服务里面有很多方法,方法需要接收请求参数
service SayHello {  //定义服务主体
  rpc SayHello(HelloRequest) returns (HelloResponse){}
}

//message关键字,其实可以理解为Golang中的结构体
//这里比较特别的是变量后面的"赋值".注意:这里并不是赋值,而是在定义这个变量在message中的位置.
//消息,要传输的对象
message HelloRequest{
  string requestName=1;  //1代表的是标识号,无特别意义, =1 可写可不写.
//  int64  age = 2;  //2也是代表标识号
}
message  HelloResponse{
  string responseMsg = 1;
}
```







## 认证-安全传输

gRPC是一个典型的C/S模型，需要开发客户端和服务端，客户端与服务端需要达成协议，使用某个确定的传输协议来传输数据，==**gRPC通常默认使用protobuf来作为传输协议**==，当然也可以使用其他的自定义协议。

![image-20230216015609271](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230216015609271.png)



那么，客户端与服务端要通信之前，客户端如何知道自己的数据是发给哪一个明确的服务端呢？反过来，服务端是不是也需要一种方式来弄清楚自己的数据要返回给谁？

**那么就不得不提gRPC认证**

此处说到的认证，不是用户的身份认证，而是**指多个server和多个client之间，如何识别对方是谁，并且可以安全的进行数据传输**

+ SSL/TLS认证方式（采用http2协议）
+ 基于token的认证方式（基于安全连接）
+ 不采用任何措施的连接，这是不安全的连接（默认采用http1）
+ 自定义的身份认证



> 客户端和服务端之间调用，我们可以通过加入证书的方式，实现调用的安全性

TLS（Transport Layer Security，安全传输层），TLS是建立在传输层TCP协议之上的协议，服务于应用层，它的前身是SSL（Secure Socket Layer，安全套接字层），它实现了将应用层的报文进行加密后再交由TCP进行传输的功能。

TLS协议主要解决了如下三个网络安全问题：

+ 保密（message privacy），保密通过加密encryption实现，所有信息都加密传输，第三方无法嗅探。
+ 完整性（message intergrity），通过MAC校验机制，一旦被篡改，通信双方会立刻发现。
+ 认证(mutual authentication)，双方认证,双方都可以配备证书，防止身份被冒充





==gRPC将各种认证方式浓缩统一到一个凭证（credentials）上，可以单独使用一个凭证，比如只使用TLS凭证或者只使用自定义的token凭证，也可以将多种凭证结合使用，gRPC提供了统一的API验证机制，使研发人员使用方便，这也是gRPC设计的巧妙之处。==





### 生成自签证书

> 生产环境可以购买证书或者使用一些平台发放的免费证书

- 安装**openssl**

  网站下载：http://slproweb.com/products/Win32OpenSSL.html

  （mac电脑 自行搜索安装）

  + 使用便捷安装包（exe），一直下一步即可
  + 配置环境变量
  + 命令行测试 openssl



```shell
# 1,生成私钥
openssl genrsa -out server.key 2048

# 2,生成证书 全部回车即可，可以不填，crt文件是根据key文件生成的
openssl req -new -x509 -key server.key -out server.crt -days 36500

# 国家名称
Country Name (2 letter code) [AU]:CN

# 省名称
State or Province Name (full name) [Some-State]:GuangDong

# 城市名称
Locality Name (eg, city) []:Meizhou

# 公司组织名称
Organization Name (eg, company) [Internet widgits Pty Ltd]:xuexiangban

# 部门名称
Organizational Unit Name (eg, section) []:go

# 服务器or网站名称
Common Name (eg, server FQDN or YOUR name) []:kuangstudy

# 邮件
Email Address []:1369385554@qq.com


# 3,生成密钥
openssl req -new -key server.key -out server.csr
```



```shell
# 更改openssl.cfg （linux 是 openssl.cnf）
#1）复制一份你安装的openssl的bin目录里面的openssl.cfg 文件到你项目的所在目录
#2）找到 [ CA_default ]，打开 copy_extensions = copy （就是把前面的 # 去掉）
#3）找到 [ req ]，打开 req_extensions = v3_req # The extensions to add to a certificate request
#4）找到 [ v3_req ]，添加subjectAltName = @alt_names
#5）添加新的标签 [ alt_names ]，和标签字段
DNS.1 = *.kuangstudy.com
```



```shell
# 生成证书私钥test.key
openssl genpkey -algorithm RSA -out test.key

# 通过私钥test.key 生成证书请求文件test.crs（注意cfg和cnf）
openssl req -new -nodes -key test.key -out test.csr --day 3650 -subj "/C=cn/OU=myorg/O=mycomp/CN=myname" -config ./openssl.cnf -extensions v3_req

# test.csr 是上面生成的证书请求文件。ca.crt/server.key 是CA证书文件和key，用来对test.csr进行签名认证。这两个文件在第一份生成。

# 生成SAN证书 pem
openssl x509 -req -days 365 -in test.csr -out test.pem -CA server.crt -CAkey server.key -CAcreateserial -extfile ./openssl.cnf -extensions v3_req
```



- **key：** 服务器上的私钥文件，用于对发送给客户端数据的加密，以及对从客户端接收到数据的解密。
- **csr：** 证书签名请求文件，用于提交给证书颁发机构（CA）对证书签名。
- **crt：** 由证书颁发机构（CA）签名后的证书，或者是开发者自签名的证书，包含证书持有人的信息，持有人的公钥，以及签署者的签名等信息。
- **pem：** 是基于Base64编码的证书格式，扩展名包括PEM、CRT和CER。

什么是 SAN？

SAN（Subject Alternative Name）是 SSL 标准 x509 中定义的一个扩展。使用了 SAN 字段的 SSL 证书，可以扩展此证书支持的域名，使得一个证书可以支持多个不同域名的解析



#### 服务端应用证书

将`server.key`和`server.pem` copy到程序中

```go
func main()  {

	//添加证书
	file, err2 := credentials.NewServerTLSFromFile("keys/mszlu.pem", "keys/mszlu.key")
	if err2 != nil {
		log.Fatal("证书生成错误",err2)
	}
	rpcServer := grpc.NewServer(grpc.Creds(file))

	service.RegisterProdServiceServer(rpcServer,service.ProductService)

	listener ,err := net.Listen("tcp",":8002")
	if err != nil {
		log.Fatal("启动监听出错",err)
	}
	err = rpcServer.Serve(listener)
	if err != nil {
		log.Fatal("启动服务出错",err)
	}
	fmt.Println("启动grpc服务端成功")
}
```



#### 客户端认证

公钥copy到客户端

```go
func main()  {
	file, err2 := credentials.NewClientTLSFromFile("client/keys/mszlu.pem", "*.mszlu.com")
	if err2 != nil {
		log.Fatal("证书错误",err2)
	}
	conn, err := grpc.Dial(":8002", grpc.WithTransportCredentials(file))

	if err != nil {
		log.Fatal("服务端出错，连接不上",err)
	}
	defer conn.Close()

	prodClient := service.NewProdServiceClient(conn)

	request := &service.ProductRequest{
		ProdId: 123,
	}
	stockResponse, err := prodClient.GetProductStock(context.Background(), request)
	if err != nil {
		log.Fatal("查询库存出错",err)
	}
	fmt.Println("查询成功",stockResponse.ProdStock)
}
```

上述认证方式为单向认证：

![img](https://www.mszlu.com/assets/1586953-20210625171059706-1447106002-16509094111532.8283e9e7.png)

中间人攻击





#### 双向认证

![img](https://www.mszlu.com/assets/1586953-20210625211235069-195172761-16509094417774.1dfd9a90.png)

上面的server.pem和server.key 是服务端的 公钥和私钥。

如果双向认证，客户端也需要生成对应的公钥和私钥。

私钥：

```bash
openssl genpkey -algorithm RSA -out client.key
```

证书:

```bash
openssl req -new -nodes -key client.key -out client.csr -days 3650 -config ./openssl.cnf -extensions v3_req
```

SAN证书：

```bash
openssl x509 -req -days 365 -in client.csr -out client.pem -CA ca.crt -CAkey ca.key -CAcreateserial -extfile ./openssl.cnf -extensions v3_req
```

服务端：

```go
func main()  {

	//添加证书
	//file, err2 := credentials.NewServerTLSFromFile("keys/mszlu.pem", "keys/mszlu.key")
	//if err2 != nil {
	//	log.Fatal("证书生成错误",err2)
	//}
	// 证书认证-双向认证
	// 从证书相关文件中读取和解析信息，得到证书公钥、密钥对
	cert, err := tls.LoadX509KeyPair("keys/mszlu.pem", "keys/mszlu.key")
	if err != nil {
		log.Fatal("证书读取错误",err)
	}
	// 创建一个新的、空的 CertPool
	certPool := x509.NewCertPool()
	ca, err := ioutil.ReadFile("keys/ca.crt")
	if err != nil {
		log.Fatal("ca证书读取错误",err)
	}
	// 尝试解析所传入的 PEM 编码的证书。如果解析成功会将其加到 CertPool 中，便于后面的使用
	certPool.AppendCertsFromPEM(ca)
	// 构建基于 TLS 的 TransportCredentials 选项
	creds := credentials.NewTLS(&tls.Config{
		// 设置证书链，允许包含一个或多个
		Certificates: []tls.Certificate{cert},
		// 要求必须校验客户端的证书。可以根据实际情况选用以下参数
		ClientAuth: tls.RequireAndVerifyClientCert,
		// 设置根证书的集合，校验方式使用 ClientAuth 中设定的模式
		ClientCAs: certPool,
	})

	rpcServer := grpc.NewServer(grpc.Creds(creds))

	service.RegisterProdServiceServer(rpcServer,service.ProductService)

	listener ,err := net.Listen("tcp",":8002")
	if err != nil {
		log.Fatal("启动监听出错",err)
	}
	err = rpcServer.Serve(listener)
	if err != nil {
		log.Fatal("启动服务出错",err)
	}
	fmt.Println("启动grpc服务端成功")
}

```

客户端：

```go
func main()  {
	//file, err2 := credentials.NewClientTLSFromFile("client/keys/mszlu.pem", "*.mszlu.com")
	//if err2 != nil {
	//	log.Fatal("证书错误",err2)
	//}
	// 证书认证-双向认证
	// 从证书相关文件中读取和解析信息，得到证书公钥、密钥对
	cert, _ := tls.LoadX509KeyPair("client/keys/test.pem", "client/keys/test.key")
	// 创建一个新的、空的 CertPool
	certPool := x509.NewCertPool()
	ca, _ := ioutil.ReadFile("client/keys/ca.crt")
	// 尝试解析所传入的 PEM 编码的证书。如果解析成功会将其加到 CertPool 中，便于后面的使用
	certPool.AppendCertsFromPEM(ca)
	// 构建基于 TLS 的 TransportCredentials 选项
	creds := credentials.NewTLS(&tls.Config{
		// 设置证书链，允许包含一个或多个
		Certificates: []tls.Certificate{cert},
		// 要求必须校验客户端的证书。可以根据实际情况选用以下参数
		ServerName: "*.mszlu.com",
		RootCAs:    certPool,
	})

	conn, err := grpc.Dial(":8002", grpc.WithTransportCredentials(creds))

	if err != nil {
		log.Fatal("服务端出错，连接不上",err)
	}
	defer conn.Close()

	prodClient := service.NewProdServiceClient(conn)

	request := &service.ProductRequest{
		ProdId: 123,
	}
	stockResponse, err := prodClient.GetProductStock(context.Background(), request)
	if err != nil {
		log.Fatal("查询库存出错",err)
	}
	fmt.Println("查询成功",stockResponse.ProdStock)
}
```



### Token认证

gRPC提供了我们一个接口，这个接口中包含了两个方法，接口位于credentials包下，这个接口需要**客户端**来实现

```go
type PerRPCCredentials interface{
    GetRequestMetadata (ctx context.Context,uri ...string)(map[string]string,error)
    RequireTransportSecurity()bool
}
```

**第一个方法作用是获取元数据信息，也就是客户端提供的key-value对，context用于控制超时和取消，uri是请求入口处的uri**

**第二个方法的作用是否需要基于TLS认证进行安全传输，如果返回值是true，则必须加上TLS验证，返回值是false则不用。**





#### 服务端添加用户名密码的校验

```go
func main()  {
	var authInterceptor grpc.UnaryServerInterceptor
	authInterceptor = func(
		ctx context.Context,
		req interface{},
		info *grpc.UnaryServerInfo,
		handler grpc.UnaryHandler,
	) (resp interface{}, err error) {
		//拦截普通方法请求，验证 Token
		err = Auth(ctx)
		if err != nil {
			return
		}
		// 继续处理请求
		return handler(ctx, req)
	}
	server := grpc.NewServer(grpc.UnaryInterceptor(authInterceptor))
	service.RegisterProdServiceServer(server,service.ProductService)

	listener, err := net.Listen("tcp", ":8002")
	if err != nil {
		log.Fatal("服务监听端口失败", err)
	}
	err = server.Serve(listener)
	if err != nil {
		log.Fatal("服务、启动失败", err)
	}
	fmt.Println("启动成功")
}


func Auth(ctx context.Context) error {
	md, ok := metadata.FromIncomingContext(ctx)
	if !ok {
		return fmt.Errorf("missing credentials")
	}
	var user string
	var password string

	if val, ok := md["user"]; ok {
		user = val[0]
	}
	if val, ok := md["password"]; ok {
		password = val[0]
	}

	if user != "admin" || password != "admin" {
		return status.Errorf(codes.Unauthenticated, "token不合法")
	}
	return nil
}
```



#### 客户端实现

客户端需要实现 `PerRPCCredentials` 接口。

```go
type PerRPCCredentials interface {
	// GetRequestMetadata gets the current request metadata, refreshing
	// tokens if required. This should be called by the transport layer on
	// each request, and the data should be populated in headers or other
	// context. If a status code is returned, it will be used as the status
	// for the RPC. uri is the URI of the entry point for the request.
	// When supported by the underlying implementation, ctx can be used for
	// timeout and cancellation. Additionally, RequestInfo data will be
	// available via ctx to this call.
	// TODO(zhaoq): Define the set of the qualified keys instead of leaving
	// it as an arbitrary string.
	GetRequestMetadata(ctx context.Context, uri ...string) (map[string]string, error)
	// RequireTransportSecurity indicates whether the credentials requires
	// transport security.
	RequireTransportSecurity() bool
}
```

`GetRequestMetadata` 方法返回认证需要的必要信息，`RequireTransportSecurity` 方法表示是否启用安全链接，在生产环境中，一般都是启用的，但为了测试方便，暂时这里不启用了。

实现接口：

```go
type Authentication struct {
    User     string   //字段名必须大写
    Password string
}

func (a *Authentication) GetRequestMetadata(context.Context, ...string) (map[string]string, error) {
    return map[string]string{"user": a.User, "password": a.Password}, nil
}

func (a *Authentication) RequireTransportSecurity() bool {
    return false
}
```

应用：

```go
user := &auth.Authentication{
		User: "admin",
		Password: "admin",
	}
	conn, err := grpc.Dial(":8002", grpc.WithTransportCredentials(insecure.NewCredentials()),grpc.WithPerRPCCredentials(user))
	
```







#### 示例

```go
//客户端
package main

import (
	"context"
	"fmt"
	"google.golang.org/grpc"
	"google.golang.org/grpc/credentials/insecure"
	service "grpc/proto"
)

type ClientTokenAuth struct {
}

func (c ClientTokenAuth) GetRequestMetadata(ctx context.Context, uri ...string) (map[string]string, error) {
	return map[string]string{
		"appid":  "xiao",
		"appkey": "123",
	}, nil
}
func (c ClientTokenAuth) RequireTransportSecurity() bool {
	return false
}

func main() {

	//creds, _ := credentials.NewClientTLSFromFile("D:\\学习笔记\\后端学习资料\\grpc\\key\\test.pem",
	//	"*.xiaokk.com")

	//连接到server端,此处禁用了安全传输,没有加密和验证,insecure.NewCredentials()不开启安全传输

	var opts []grpc.DialOption
	opts = append(opts, grpc.WithTransportCredentials(insecure.NewCredentials()))
	opts = append(opts, grpc.WithPerRPCCredentials(new(ClientTokenAuth)))
	
    conn, err := grpc.Dial("127.0.0.1:9090", opts...)
    
    
	if err != nil {
		fmt.Printf("connect failed,err:%v\n", err)
		return
	}
	defer conn.Close()

	//建立连接
	client := service.NewSayHelloClient(conn)

	//执行rpc调用(这个方法在服务器端来实现并返回结果),调用的SayHello方法是服务端实现的方法(即刚才服务端重写的方法)
	resp, err := client.SayHello(context.Background(), &service.HelloRequest{RequestName: "哈哈"})
	if err != nil {
		fmt.Println("client.SayHello failed,err:", err)
		return
	}
	//获取回复的消息
	fmt.Println(resp.GetResponseMsg())
}

```



```go
//服务端
package main

import (
	"context"
	"errors"
	"fmt"
	"google.golang.org/grpc"
	"google.golang.org/grpc/credentials/insecure"
	"google.golang.org/grpc/metadata"
	service "grpc/proto" //这里之所以要这么写,就是因为包名和路径名不一致
	"net"
)

type server struct {
	service.UnimplementedSayHelloServer
}

// 方法重写
func (s *server) SayHello(ctx context.Context, req *service.HelloRequest) (*service.HelloResponse, error) {

	//获取元数据信息
	md, ok := metadata.FromIncomingContext(ctx)
	if !ok {
		return nil, errors.New("未传输token")
	}
	var appId string
	var appKey string
	//注意：如果在客户端中，appId和appKey是大写，这里必须小写才能行
	if v, ok := md["appid"]; ok {
		appId = v[0]
	}
	if v, ok := md["appkey"]; ok {
		appKey = v[0]
	}
	if appId != "xiao" || appKey != "123" {
		return nil, errors.New("token不正确")
	}

	fmt.Println("server端 SayHello方法")
	return &service.HelloResponse{ResponseMsg: "hello" + req.GetRequestName()}, nil
}

func main() {
	//TSL认证，有两个参数cretFile，keyFile
	//我们需要将刚才生成的自签名证书test.pem 和 私钥文件 test.key 放进去即可
	//creds, _ := credentials.NewServerTLSFromFile("D:\\学习笔记\\后端学习资料\\grpc\\key\\test.pem",
	//	"D:\\学习笔记\\后端学习资料\\grpc\\key\\test.key")

	//开启端口
	listen, err := net.Listen("tcp", ":9090")
	if err != nil {
		fmt.Printf("listen failed,err:%v\n", err)
		return
	}
	//创建grpc服务,将服务暴露供别人使用
	grpcServer := grpc.NewServer(grpc.Creds(insecure.NewCredentials()))

	//在grpc服务端中注册我们自己编写的服务
	//这里必须通过引用注册,即必须是&server{},因为上面方法实现是通过指针实现的,而RegisterSayHelloServer第二个参数
	//是接口类型,指针实现的方法只能传入指针类型才能得到该方法
	service.RegisterSayHelloServer(grpcServer, &server{}) //注册服务和方法

	//启动服务
	err = grpcServer.Serve(listen)
	if err != nil {
		fmt.Println("grpcServer failed,err:", err)
		return
	}
}

```









## 流和import讲解

### import使用

> 用于导入其他proto文件

```go
// 这个就是protobuf的中间文件

// 指定的当前proto语法的版本，有2和3
syntax = "proto3";
//从执行 protoc这个命令的当前目录开始算起，
import "pbfile/user.proto";

option go_package="../service";

// 指定等会文件生成出来的package
package service;

// 定义request model
message ProductRequest{
  int32 prod_id = 1; // 1代表顺序
}

// 定义response model
message ProductResponse{
  int32 prod_stock = 1; // 1代表顺序
  User user = 2;
}

// 定义服务主体
service ProdService{
  // 定义方法
    rpc GetProductStock(ProductRequest) returns(ProductResponse){};
}
```



### stream

> 任意类型

```protobuf
// 使用any类型，需要导入这个
import "google/protobuf/any.proto";

// 定义入参消息
message HelloParam{
  // any，代表可以是任何类型
  google.protobuf.Any data = 1;
}
```



```go
// 这个就是protobuf的中间文件

// 指定的当前proto语法的版本，有2和3
syntax = "proto3";
//从执行 protoc这个命令的当前目录开始算起，
import "user.proto";
// 使用any类型，需要导入这个
import "google/protobuf/any.proto";

option go_package="../service";

// 指定等会文件生成出来的package
package service;

// 定义request model
message ProductRequest{
  int32 prod_id = 1; // 1代表顺序
}

message Content {
  string msg = 1;
}
// 定义response model
message ProductResponse{
  int32 prod_stock = 1; // 1代表顺序
  User user = 2;
  google.protobuf.Any data = 3;
}

// 定义服务主体
service ProdService{
  // 定义方法
    rpc GetProductStock(ProductRequest) returns(ProductResponse){};
}

```

```go
func (p *productService) GetProductStock(context context.Context, request *ProductRequest) (*ProductResponse, error) {
	//实现具体的业务逻辑
	stock := p.GetStockById(request.ProdId)
	user := User{Username: "mszlu"}
	content := Content{Msg: "mszlu msg..."}
	//转换成any类型
	any, _ := anypb.New(&content)
	return &ProductResponse{ProdStock: stock, User: &user, Data: any}, nil
}
```



#### 定义

在 HTTP/1.1 的时代，同一个时刻只能对一个请求进行处理或者响应，换句话说，下一个请求必须要等当前请求处理完才能继续进行。

> HTTP/1.1需要注意的是，在服务端没有response的时候，客户端是可以发起多个request的，但服务端依旧是顺序对请求进行处理, 并按照收到请求的次序予以返回。

HTTP/2 的时代，多路复用的特性让一次同时处理多个请求成为了现实，并且同一个 TCP 通道中的请求不分先后、不会阻塞，HTTP/2 中引入了流(Stream) 和 帧(Frame) 的概念，当 TCP 通道建立以后，后续的所有操作都是以流的方式发送的，而二进制帧则是组成流的最小单位，属于协议层上的流式传输。

> HTTP/2 在一个 TCP 连接的基础上虚拟出多个 Stream, Stream 之间可以并发的请求和处理, 并且 HTTP/2 以二进制帧 (frame) 的方式进行数据传送, 并引入了头部压缩 (HPACK), 大大提升了交互效率



#### 客户端流

```protobuf
// 普通 RPC
 2  rpc SimplePing(PingRequest) returns (PingReply);
 3
 4  // 客户端流式 RPC
 5  rpc ClientStreamPing(stream PingRequest) returns (PingReply);
 6
 7  // 服务器端流式 RPC
 8  rpc ServerStreamPing(PingRequest) returns (stream PingReply);
 9
10  // 双向流式 RPC
11  rpc BothStreamPing(stream PingRequest) returns (stream PingReply);
```

`stream`关键字，当该关键字修饰参数时，表示这是一个客户端流式的 gRPC 接口；当该参数修饰返回值时，表示这是一个服务器端流式的 gRPC 接口；当该关键字同时修饰参数和返回值时，表示这是一个双向流式的 gRPC 接口。



#### 服务端流

定义：

```protobuf
rpc UpdateStockClientStream(stream ProductRequest) returns(ProductResponse);
```



```go
//....	
stream, err := prodClient.UpdateProductStockClientStream(context.Background())
	if err != nil {
		log.Fatal("获取流出错", err)
	}
	rsp := make(chan struct{}, 1)
	go prodRequest(stream, rsp)
	select {
	case <-rsp:
		recv, err := stream.CloseAndRecv()
		if err != nil {
			log.Fatal(err)
		}
		stock := recv.ProdStock
		fmt.Println("客户端收到响应：", stock)
```




```go
func prodRequest(stream service.ProdService_UpdateProductStockClientStreamClient, rsp chan struct{}) {
	count := 0
	for {
		request := &service.ProductRequest{
			ProdId: 123,
		}
		err := stream.Send(request)
		if err != nil {
			log.Fatal(err)
		}
		time.Sleep(time.Second)
		count++
		if count > 10 {
			rsp <- struct{}{}
			break
		}
	}
}
```




```go
func (p *productService) UpdateProductStockClientStream(stream ProdService_UpdateProductStockClientStreamServer) error {
	count := 0
	for {
		//源源不断的去接收客户端发来的信息
		recv, err := stream.Recv()
		if err != nil {
			if err == io.EOF {
				return nil
			}
			return err
		}
		fmt.Println("服务端接收到的流", recv.ProdId, count)
		count++
		if count > 10 {
			rsp := &ProductResponse{ProdStock: recv.ProdId}
			err := stream.SendAndClose(rsp)
			if err != nil {
				return err
			}
			return nil
		}
	}
}
```





#### 双向流

```go
stream, err := prodClient.SayHelloStream(context.Background())

	for {
		request := &service.ProductRequest{
			ProdId: 123,
		}
		err = stream.Send(request)
		if err != nil {
			log.Fatal(err)
		}
		time.Sleep(time.Second)
		recv, err := stream.Recv()
		if err != nil {
			log.Fatal(err)
		}
		//websocket
		fmt.Println("客户端收到的流信息", recv.ProdStock)
	}
```



```go
func (p *productService) SayHelloStream(stream ProdService_SayHelloStreamServer) error {
	for {
		recv, err := stream.Recv()
		if err != nil {
			return nil
		}
		fmt.Println("服务端收到客户端的消息", recv.ProdId)
		time.Sleep(time.Second)
		rsp := &ProductResponse{ProdStock: recv.ProdId}
		err = stream.Send(rsp)
		if err != nil {
			return nil
		}
	}
}
```









# GORM框架

gorm是一个使用Go语言编写的ORM框架。它文档齐全，对开发者友好，支持主流数据库。

gorm官方文档：[GORM 指南 | GORM - The fantastic ORM library for Golang, aims to be developer friendly.](https://gorm.io/zh_CN/docs/)



## 定义

**ORM（ Object Relational Mapping）：**是对象关系映射，是一种程序技术,用于实现面向对象编程语言里不同类型系统的数据之间的转换。



![image-20230211200240125](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230211200240125.png)

![image-20230211200642119](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230211200642119.png)



## ORM优缺点

**优点：**

+ 提高开发效率

**缺点：**

+ 牺牲执行性能
+ 牺牲灵活性
+ 弱化SQL能力





## 安装

```go
go get -u gorm.io/gorm
//装驱动
//go get -u gorm.io/driver/mysql
//go get -u gorm.io/driver/sqlite
```



## 官方--连接数据库

连接不同的数据库都需要导入对应的数据驱动程序，`GORM`已经贴心的为我们包装了一些驱动程序，只需要导入对应驱动即可。



### 连接MySQL

```go
import (
  "gorm.io/driver/mysql"  //导入对应数据库的驱动
  "gorm.io/gorm"
)

func main() {
  // 参考 https://github.com/go-sql-driver/mysql#dsn-data-source-name 获取详情
    //dbname 为数据库名
  dsn := "username:password@tcp(127.0.0.1:3306)/dbname?charset=utf8mb4&parseTime=True&loc=Local"
  db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
}
```

**注意：**想要正确的处理 `time.Time` ，您需要带上 `parseTime` 参数， ([更多参数](https://github.com/go-sql-driver/mysql#parameters)) 要支持完整的 UTF-8 编码，您需要将 `charset=utf8` 更改为 `charset=utf8mb4` 查看 [此文章](https://mathiasbynens.be/notes/mysql-utf8mb4) 获取详情





### 连接PostgreSQL

基本代码同上，注意引入对应`postgres`驱动并正确指定`gorm.Open()`参数

```go
import (  
    "gorm.io/driver/postgres"  
    "gorm.io/gorm"
)

func main(){
    
	dsn := "host=localhost user=gorm password=gorm dbname=gorm port=9920 sslmode=disable TimeZone=Asia/Shanghai"
	db, err := gorm.Open(postgres.Open(dsn), &gorm.Config{})

}
```



### Sqlite3

基本代码同上，注意引入对应`Sqlite3`驱动并正确指定`gorm.Open()`参数

```go
import (
  "gorm.io/driver/sqlite" // 基于 GGO 的 Sqlite 驱动
  // "github.com/glebarez/sqlite" // 纯 Go 实现的 SQLite 驱动, 详情参考： https://github.com/glebarez/sqlite
  "gorm.io/gorm"
)

// github.com/mattn/go-sqlite3
db, err := gorm.Open(sqlite.Open("gorm.db"), &gorm.Config{})
```





### 连接SQL Server

基本代码同上，注意引入对应`sqlserver`驱动并正确指定`gorm.Open()`参数

```go
import (
  "gorm.io/driver/sqlserver"
  "gorm.io/gorm"
)

// github.com/denisenkom/go-mssqldb
dsn := "sqlserver://gorm:LoremIpsum86@localhost:9930?database=gorm"
db, err := gorm.Open(sqlserver.Open(dsn), &gorm.Config{})
```





**注意：**

![image-20230214002152220](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230214002152220.png)



## GORM基本示例

注意：

+ 本文以MySQL数据库为例，讲解了GORM各项功能的主要使用方法。
+ 往下阅读本文前，需要有一个能够成功连接上的MySQL数据库实例。



### 创建数据库

在使用GORM前手动创建数据库`db1`

```go
CREATE DATABASE db1;
```



### 数据定义语句DDL

#### Migrator 接口中方法

```go
// Migrator migrator interface
type Migrator interface {
	// AutoMigrate
	AutoMigrate(dst ...interface{}) error

	// Database
	CurrentDatabase() string
	FullDataTypeOf(*schema.Field) clause.Expr

	// Tables
	CreateTable(dst ...interface{}) error
	DropTable(dst ...interface{}) error
	HasTable(dst interface{}) bool
	RenameTable(oldName, newName interface{}) error
	GetTables() (tableList []string, err error)

	// Columns
	AddColumn(dst interface{}, field string) error
	DropColumn(dst interface{}, field string) error
	AlterColumn(dst interface{}, field string) error
	MigrateColumn(dst interface{}, field *schema.Field, columnType ColumnType) error
	HasColumn(dst interface{}, field string) bool
	RenameColumn(dst interface{}, oldName, field string) error
	ColumnTypes(dst interface{}) ([]ColumnType, error)

	// Views
	CreateView(name string, option ViewOption) error
	DropView(name string) error

	// Constraints
	CreateConstraint(dst interface{}, name string) error
	DropConstraint(dst interface{}, name string) error
	HasConstraint(dst interface{}, name string) bool

	// Indexes
	CreateIndex(dst interface{}, name string) error
	DropIndex(dst interface{}, name string) error
	HasIndex(dst interface{}, name string) bool
	RenameIndex(dst interface{}, oldName, newName string) error
}

```



#### 自动建表

**通过AutoMigrate函数可以快速建表，如果表已经存在不会重复创建**

```go
// 根据User结构体，自动创建表结构.
db.AutoMigrate(&User{}) 
db.AutoMigrate(User{})  //这种也是可以的

// 一次创建User、Product、Order三个结构体对应的表结构
db.AutoMigrate(&User{}, &Product{}, &Order{})

// 可以通过Set设置附加参数，下面设置表的存储引擎为InnoDB
db.Set("gorm:table_options", "ENGINE=InnoDB").AutoMigrate(&User{})

```

通过 `AutoMigrate` 方法传入要迁移的模型类实例即可，GORM 会自动创建对应的数据表，表名规则是模型类名小写的复数形式。

```go
//自动建表内部其实就是调用接口中AutoMigrate方法
func (db *DB) AutoMigrate(dst ...interface{}) error {
	return db.Migrator().AutoMigrate(dst...)  
}

//通过自动建表，会将id作为主键，并且添加auto_increment
```



#### 手动建表

```go
m:=db.Migrator().CreateTable(&User{})  //db.Migrator()
```



#### 查看表是否存在

```go
//以结构体的方式查
m.HasTable(&User{})  // m 是 db.Migrator()

//以表名的方式查
fmt.Println(m.HasTable("users"))  //表名
```



#### 删除表 

```go
err = m.DropTable(&User{})
if err != nil {
	fmt.Println(err)
}

err = m.DropTable("gva_users")  //表名
if err != nil {
	fmt.Println(err)
}

if m.HasTable(&User{}) {
	m.DropTable(&User{})
} else {
	m.CreateTable(&User{})
}

```



#### 表 重命名

将gva_users表改为gva_users_two表

```go
m := db.Migrator()
m.RenameTable(&User{}, "gva_users_two")  //表名
```

这个时候再通过原来的结构体查询就查不到了

```go
fmt.Println( m.HasTable(&User{}))//false
```

**所以一般建议这样写**

```go
type User struct {
	Name string
}
m := db.Migrator()
type UserTwo struct {
	Name string
}
//给表重命名，第一个参数为旧表名，第二个参数为新表名
//第一种方式：
m.RenameTable(&User{},&UserTwo{})   //m := db.Migrator()


//第二种方式：
m.RenameTable(&User{},"usersTwo") //usersTwo 是新表名
```









## GORM 操作 MySQL 示例

注意：该示例主要关注三点：

+ 在没有进行自动迁移，如何将结构体字段映射到数据库表字段
+ 在没有进行自动迁移，如何根据该数据结构找到需要插入的数据库表
+ 在没有自动迁移，在插入数据、查找数据、更新数据时如何找到对应的数据库表

```go
package main

import (
	"fmt"
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
	"gorm.io/gorm/logger"
)

var DB *gorm.DB

type UserInfo struct {
	//如何将字段映射到数据库表中对应的列尼？
	//通过在字段后面的标签说明，定义golang字段和数据库表字段的关系
	ID         int64
	Name       string `gorm:"column:username"`
	Gender     string `gorm:"column:gender"`
	CreateTime int64  `gorm:"column:createtime"`
}

func init() {
	//初始化，连接数据库
	username := "root"
	password := "kxh19970301"
	dbname := "gorm"
	dsn := fmt.Sprintf("%s:%s@tcp(127.0.0.1:3306)/%s?charset=utf8mb4&parseTime=True&loc=Local", username, password, dbname)
	//dsn:="root:kxh19970301@tcp(127.0.0.1)/sql_test"
	//官方示例 dsn := "user:pass@tcp(127.0.0.1:3306)/dbname?charset=utf8mb4&parseTime=True&loc=Local"
	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{
		//设置一个日志的级别，可以设置也可以不设置
		Logger: logger.Default.LogMode(logger.Info),
	})
	if err != nil {
		fmt.Printf("open mysql failed,err:%v\n", err)
		return
	}
	DB = db
	fmt.Println("mysql connect success!")
}

func (u UserInfo) TableName() string {
	//***注意***
	//绑定mysql数据库中的表名为users这张表
	//将UserInfo结构体与数据库表users这张表进行绑定，之所以需要手动绑定是因为：UserInfo默认会数据库表user_infos对应，
	//但是，数据库表中明显没有该表，我们需要将userinfo信息填入的是users表中，自动绑定无法进行，因此，需要手动绑定。
	return "users"
}

//插入
func Saveuser(user *UserInfo) {
	//将值保存在users表中
	//创建插入值，将插入数据的主键返回到值的id中
	err := DB.Create(user).Error
	if err != nil {
		fmt.Printf("insert user failed,err:%v\n", err)
		return
	}
	fmt.Println("insert user success!")

}

//单条查询
func GetById(id int64) UserInfo {
	var user UserInfo
	err := DB.Where("id=?", id).First(&user).Error
	if err != nil {
		fmt.Printf("get user failed,err:%v\n", err)
		return UserInfo{}
	}
	return user
}

//查询全部数据
func GetAll() []UserInfo {
	var users []UserInfo
	//查找与给定条件条件匹配的所有记录
	err := DB.Find(&users).Error
	if err != nil {
		fmt.Println("find users failed,err:", err)
		return nil
	}
	return users
}

//更新数据
func UpdateUser(id int64) {
	err := DB.Model(&UserInfo{}).Where("id=?", id).Update("username", "娜扎").Error
	if err != nil {
		fmt.Printf("update user information failed,err:%v\n", err)
		return
	}
	fmt.Println("update user information success!")
}

//删除数据
func DeleteUser(id int64) {
	err := DB.Where("id=?", id).Delete(&UserInfo{}).Error
	if err != nil {
		fmt.Printf("delete user information failed,err:%v\n", err)
		return
	}
	fmt.Println("delete user information success!")
}

func main() {
	//u1 := UserInfo{
	//
	//	Name:       "哈哈",
	//	Gender:     "男",
	//	CreateTime: time.Now().UnixMilli(),
	//}
	////插入
	//Saveuser(&u1)
	//fmt.Println(u1)

	//根据ID来查询数据
	//u2 := GetById(1)
	//fmt.Println(u2)

	//查询全部数据
	//users := GetAll()
	//fmt.Println(users)

	//更新数据
	//UpdateUser(1002)

	//删除数据
	DeleteUser(1001)
}
```





## GORM Model定义

在案例中，我们定义了`UserInfo`结构体用来和数据库表`users`做映射，`UserInfo`结构体，我们将其称为数据模型，在gorm框架中，操作数据库需要预先定义模型。

底层原理：**使用golang的database标准库，利用反射原理，在执行读写操作时将结构体翻译成sql语句，并将结果转化为对应的模型。**



### 绑定数据库默认约定规则


gorm自动迁移数据库，自动将数据库的表名、列名和结构体名、字段名绑定。gorm的绑定规则是约定的，不是配置的。gorm默认约定规则：

+ 结构体中的ID作为主键，对应表中id列。
+ 结构体名称转换成蛇形命名的复数形式，对应表名。 (UserTable-->user_tables)
+ 结构体字段名转换成蛇形命名的单数形式，对应表的列名。 (UserName-->user_name)
+ 按照默认约定定义结构体，使用自动迁移方法，可以创建对应的表结构。

### 修改默认约定规则

使用go语言的tag标签能修改以下内容：

- 修改默认ID主键
- 修改结构体字段对应的表列名

```go
type Animal struct {
  UUID   string `gorm:"primaryKey"` //设置主键，与表中主键字段进行绑定
  Age    int64  `gorm:"column:user_age"` //设置列名，同样将该字段与表中字段进行绑定
}
```





### 模型定义

假设有一个商品表

```go
CREATE TABLE `goods` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID，商品Id',
  `name` varchar(30) NOT NULL COMMENT '商品名',
  `price` decimal(10,2) unsigned  NOT NULL COMMENT '商品价格',
  `type_id` int(10) unsigned NOT NULL COMMENT '商品类型Id',
  `createtime` int(10) NOT NULL DEFAULT 0 COMMENT '创建时间',
   PRIMARY KEY (`id`)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4
```

将上述表翻译成模型后，如下：

```go
type Good struct {
	Id         int  //表字段名为：id
	Name       string //表字段名为：name
	Price      float64 //表字段名为：price
	TypeId     int  //表字段名为：type_id
    //gorm有默认的映射，但是如果表中列名与默认映射有出入时，可以通过标记tag来指定映射关系，如CreateTime默认映射create_time,但是在表中是createtime，因此，此时默认映射行不通，故采用tag来指定映射
	CreateTime int64 `gorm:"column:createtime"`  //表字段名为：createtime
}
```

默认gorm对struct字段名使用**Snake Case**命名风格转换成mysql表字段名(需要转换成小写字母)。

> Snake Case命名风格，就是各个单词之间用下划线（_）分隔，例如： CreateTime的Snake Case风格命名为create_time

==同时默认情况下，使用`ID`做为其主键（注意是ID，而Id也会默认当成主键），使用结构体名称的`Snake Case`风格的**复数**形式做为表名，使用 `CreatedAt`、`UpdatedAt` 字段追踪创建、更新时间。==



### 模型标签

标签定义：

```go
`gorm:"标签内容"`
```

**标签定义部分，==多个标签定义可以使用分号（;）分隔==**

gorm常用标签如下：

| 标签       | 说明     | 例子                                                         |
| :--------- | :------- | :----------------------------------------------------------- |
| column     | 指定列名 | `gorm:"column:createtime"`                                   |
| primaryKey | 指定主键 | `gorm:"column:id; PRIMARY_KEY"`                              |
| -          | 忽略字段 | `gorm:"-"` 可以忽略struct字段，被忽略的字段不参与gorm的读写操作 |

其他的可以查看官方文档：`https://gorm.io/zh_CN/docs/models.html#embedded_struct`



### 表名映射

- **复数表名，比如结构体User，默认的表名为users**

- **实现Tabler接口 （`TableName` 不支持动态变化，它会被缓存下来以便后续使用。）**

  ```go
  type Tabler interface {
      TableName() string
  }
  
  // TableName 会将 User 的表名重写为 `profiles`
  func (User) TableName() string {
    return "profiles"
  }
  ```

- **动态表名，使用Scopes**

  ```go
  func UserTable(user User) func (tx *gorm.DB) *gorm.DB {
    return func (tx *gorm.DB) *gorm.DB {
      if user.Admin {
        return tx.Table("admin_users")
      }
  
      return tx.Table("users")
    }
  }
  
  db.Scopes(UserTable(user)).Create(&user)
  ```

- **临时表名**

  ==临时表名的优先级高于默认表名==
  
  ```go
  //或者可以使用临时表名，因为临时表名优先级高于默认表名和TableName，当需要临时插入其他表中，可以使用临时表名
  db.Table("deleted_users").Create(&user)
  ```



+ **通过使用`Model()`函数来确定表**

  + 对于查询来说，一般使用Find，First就够了。当查询链中没有用到Find，First等函数时，这时就无法指定要查询的表了，此时就要用Model来指定表
  + ==使用Model**不仅可以指定表，还可以指定查询条件**，但是这个条件却只能是id，不能使用其他条件。==
  + ==Model方法传入参数必须是指针对象==

  ```go
  //比如：我们通过使用map创建来插入数据，此时无法确定数据库中哪张表
  //但是，如果是结构体，则不需要用model方法来链接确定，因为gorm中就是根据结构体来链接表的。
  db.Model(&User{}).Create(map[string]interface{}{"Name":"哈哈","Age":26})
  ```

  ==Model方法可以指定要操作的数据库表，因为在gorm中，model默认会将struct转换为表名，而Model可以指定要操作的表，从而确定要操作的表。==



### Model

GORM 定义一个 `gorm.Model` 结构体，其包括字段 `ID`、`CreatedAt`、`UpdatedAt`、`DeletedAt`

```go
// gorm.Model 的定义
type Model struct {
  ID        uint           `gorm:"primaryKey"`
  CreatedAt time.Time
  UpdatedAt time.Time
  DeletedAt gorm.DeletedAt `gorm:"index"`
}
```

**GORM 约定使用 `CreatedAt`、`UpdatedAt` 追踪创建/更新时间**。==如果定义了这种字段，GORM 在创建、更新时会自动填充当前时间==。

**要使用不同名称的字段，您可以配置 autoCreateTime、autoUpdateTime 标签**

==如果想要保存 UNIX（毫/纳）秒时间戳，而不是 time，只需简单地将 time.Time 修改为 int 即可==。

例子：

```go
type User struct {
  CreatedAt time.Time // 默认创建时间字段， 在创建时，如果该字段值为零值，则使用当前时间填充
    //注意；必须和表中字段类型对应，数据库表字段类型为datetime，则结构体字段类型为time.Time，如果数据库表字段类型为int，则结构体字段类型为int
  UpdatedAt int       // 默认更新时间字段， 在创建时该字段值为零值或者在更新时，使用当前时间戳秒数填充
//  Updated   int64 `gorm:"autoUpdateTime:nano"` // 自定义字段， 使用时间戳纳秒数填充更新时间
  Updated   int64 `gorm:"autoUpdateTime:milli"` //自定义字段， 使用时间戳毫秒数填充更新时间
  Created   int64 `gorm:"autoCreateTime"`      //自定义字段， 使用时间戳秒数填充创建时间
}
```



可以将它嵌入到您的结构体中，以包含这几个字段，比如

```go
type User struct {
  gorm.Model
  Name string
}
// 等效于
type User struct {
  ID        uint           `gorm:"primaryKey"`
  CreatedAt time.Time
  UpdatedAt time.Time
  DeletedAt gorm.DeletedAt `gorm:"index"`
  Name string
}
```



对于正常的结构体字段，你也可以通过标签 `embedded` 将其嵌入，例如：

```go
type Author struct {
    Name  string
    Email string 
}

type Blog struct {
  ID      int
  Author  Author `gorm:"embedded"` //注意：这里可以不使用标签，系统也会自动将Author转换成下面结构体形式
  Upvotes int32
}
// 等效于
type Blog struct {
  ID    int64
  Name  string
  Email string
  Upvotes  int32
}
```



可以使用标签 `embeddedPrefix` 来为 db 中的字段名添加前缀，例如：

```go
type Blog struct {
  ID      int
  Author  Author `gorm:"embedded;embeddedPrefix:author_"` //添加前缀必须使用embeddedPrefix标签
  Upvotes int32
}
// 等效于
type Blog struct {
  ID          int64
  AuthorName  string
  AuthorEmail string
  Upvotes     int32
}
```



### 数据库连接

GORM 官方支持的数据库类型有： MySQL, PostgreSQL, SQlite, SQL Server

连接数据库主要是两个步骤：

- 配置DSN (Data Source Name)
- 使用gorm.Open连接数据库



#### DSN

gorm库使用dsn作为连接数据库的参数，dsn（data source name）翻译过来就叫数据源名称，用来描述数据库连接信息。一般都包含数据库连接地址，账号，密码之类的信息。

格式：

```go
[username[:password]@][protocol[(address)]]/dbname[?param1=value1&...&paramN=valueN]
//username:password@tcp(host:port)/Dbname?charset=utf8mb4&parseTime=True&loc=Local
```

mysql的dsn的一些例子：

```go
//mysql dsn格式
//涉及参数:
//username   数据库账号
//password   数据库密码
//host       数据库连接地址，可以是Ip或者域名
//port       数据库端口
//Dbname     数据库名
username:password@tcp(host:port)/Dbname?charset=utf8mb4&parseTime=True&loc=Local

//填上参数后的例子
//username = root
//password = 123456
//host     = localhost
//port     = 3306
//Dbname   = gorm
//后面K/V键值对参数含义为：
//  charset=utf8mb4 客户端字符集为utf8
//  parseTime=true 支持把数据库datetime和date类型转换为golang的time.Time类型
//  loc=Local 使用系统本地时区
root:123456@tcp(localhost:3306)/gorm?charset=utf8mb4&parseTime=True&loc=Local

//gorm 设置mysql连接超时参数
//开发的时候经常需要设置数据库连接超时参数，gorm是通过dsn的timeout参数配置
//例如，设置10秒后连接超时，timeout=10s
//下面是完成的例子
root:123456@tcp(localhost:3306)/gorm?charset=utf8mb4&parseTime=True&loc=Local&timeout=10s

//设置读写超时时间
// readTimeout - 读超时时间，0代表不限制
// writeTimeout - 写超时时间，0代表不限制
root:123456@tcp(localhost:3306)/gorm?charset=utf8mb4&parseTime=True&loc=Local&timeout=10s&readTimeout=30s&writeTimeout=60s

```

==要支持完整的 UTF-8 编码，您需要将 `charset=utf8` 更改为 `charset=utf8mb4`==



#### 连接数据库

```go
import (
  "gorm.io/driver/mysql"
  "gorm.io/gorm"
)

func main() {
  // 参考 https://github.com/go-sql-driver/mysql#dsn-data-source-name 获取详情
  dsn := "user:pass@tcp(127.0.0.1:3306)/dbname?charset=utf8mb4&parseTime=True&loc=Local"
  db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{Logger: logger.Default.LogMode(logger.Info)})
}
```



MySQL 驱动程序提供了 [一些高级配置open in new window](https://github.com/go-gorm/mysql) 可以在初始化过程中使用，例如：

```go
db, err := gorm.Open(mysql.New(mysql.Config{
  DSN: "gorm:gorm@tcp(127.0.0.1:3306)/gorm?charset=utf8&parseTime=True&loc=Local", // DSN data source name
  DefaultStringSize: 256, // string 类型字段的默认长度
  DisableDatetimePrecision: true, // 禁用 datetime 精度，MySQL 5.6 之前的数据库不支持
  DontSupportRenameIndex: true, // 重命名索引时采用删除并新建的方式，MySQL 5.7 之前的数据库和 MariaDB 不支持重命名索引
  DontSupportRenameColumn: true, // 用 `change` 重命名列，MySQL 8 之前的数据库和 MariaDB 不支持重命名列
  SkipInitializeWithVersion: false, // 根据当前 MySQL 版本自动配置
}), &gorm.Config{Logger: logger.Default.LogMode(logger.Info)})
```



GORM 允许通过 `DriverName` 选项自定义 MySQL 驱动，例如：

```go
import (
  _ "example.com/my_mysql_driver"
  "gorm.io/driver/mysql"
  "gorm.io/gorm"
)

db, err := gorm.Open(mysql.New(mysql.Config{
  DriverName: "my_mysql_driver",
  DSN: "gorm:gorm@tcp(localhost:9910)/gorm?charset=utf8&parseTime=True&loc=Local", // data source name, 详情参考：https://github.com/go-sql-driver/mysql#dsn-data-source-name
}), &gorm.Config{Logger: logger.Default.LogMode(logger.Info)})
```



#### 调试模式

```go
db.Debug()
//可以在插入、更新、删除、查找时添加调试模式，相当于会打印日志信息
//例如：db.Debug().Create() ....
//即在进行操作前，打开调试模式，DB.Debug().操作
```



#### 连接池设置

```go
sqlDB, err := db.DB()

//设置数据库连接池参数
sqlDB.SetMaxOpenConns(100)   //设置数据库连接池最大连接数
sqlDB.SetMaxIdleConns(20)   //连接池最大允许的空闲连接数，如果没有sql任务需要执行的连接数大于20，超过的连接会被连接池关闭
```



## 增删改查



### 插入数据

```go
user := User{
	Username:"zhangsan",
	Password:"123456",
	CreateTime:time.Now().Unix(),
}
tx:=db.Create(&user)    //插入单条数据，如果数据库表设为auto_increment，那么ID可以不用写，如果写了ID则会将该值插入

user.ID             // 如果，结构体ID没有设置，会返回插入数据的主键，所以这里必须使用地址，注意：只会返回主键，其他不会
tx.Error        // 返回 error，如果插入数据出错，数据库会将报错信息返回到Error里面
tx.RowsAffected // 返回插入记录的条数，插入成功后，数据库会将受影响的行数返回到RowsAffected
```

==**在插入数据时，必须传递是地址**，否则报错，原因如上注释==

==create插入单条数据，如果结构体设置了ID值，那么ID值会插入进去，如果没有设置ID值，那么数据库表中ID的auto_increment会起作用，并且会将该主键ID值保存在结构体ID字段中，因此，如果不传递地址，那么则无法保存在结构体字段内。==



#### 用指定的字段创建

```go
//select其实跟sql语句是相关的，底层其实：在插入数据时只有这两个字段
// INSERT INTO `users` (`name`,`age`,`create_time`) VALUES ('张三',12,'2023-03-14 14:20:51.297')
//created 由于添加了标签`gorm:"autoCreateTime"`，所以即使没写，会自动插入

//DB.Select("name", "age").Create(&user)

func insertSome() {
	user := User{
		Id:   101,
		Name: "张三",
		Age:  12,
	}
	err := DB.Select("name", "age").Create(&user).Error
	if err != nil {
		panic(err)
	}
	fmt.Println("insert success!")
}

```

==**注意：这里选择字段和忽略字段是针对结构体的字段，即在插入时不将该字段插入进去。**==





#### 忽略字段

```go
//忽略字段，用法类似
//INSERT INTO `users` (`created`,`addr`,`id`) VALUES ('2023-02-12 19:55:18.448','辽宁省大连市
庄河市',1003)

db.Omit("name","age").Create(&user)
```



#### 批量插入

```go
//批量插入，其实就是将多个数据放在切片内，然后一起插入，从而实现批量插入
var users = []User{{Username: "jinzhu1"}, {Username: "jinzhu2"}, {Username: "jinzhu3"}}
db.Create(&users)  //通过create方法一起插入进去

for _, user := range users {
  user.ID // 1,2,3
}
```

==**使用结构体形式批量插入，create_time会自动插入**==



使用 `CreateInBatches` 分批创建时，你可以指定每批的数量，例如：

```go
var users = []User{{Username: "jinzhu_1"}, ...., {Username: "jinzhu_10000"}}

// 数量为 100
db.CreateInBatches(users, 100)  //指定一次插入的数量，分多次进行插入

//分批插入，即底层分几批执行插入的sql语句  **
```



#### 使用map创建

```go
db.Model(&User{}).Create(map[string]interface{}{
  "Name": "jinzhu", "Age": 18,
})

// batch insert from `[]map[string]interface{}{}`
db.Model(&User{}).Create([]map[string]interface{}{
  {"Name": "jinzhu_1", "Age": 18},
  {"Name": "jinzhu_2", "Age": 20},
})
```

**==注意：采用map创建，主键会被填充，但是 create_time 不会自动插入。==**



#### sql表达式

```go
//insert into users (username,password) values("jinzhu",md5("123456"))  这种sql语句可以对应下面
DB.Model(&User{}).Create(map[string]interface{}{
		"username": "jinzhu",
		"password": clause.Expr{SQL: "md5(?)", Vars: []interface{}{"123456"}},
})
```



#### 使用原生sql语句

**用exec执行原生sql语句可以==直接==用于==插入，更新、删除==。**

```go
//可以通过Exec来使用原生sql语句，而不是用框架中一些便捷的方法
//注意：使用原生sql，也不会像gorm一样自动导入时间，主键之所以会自动导入是由mysql机制决定，因为主键后面带了auto_increment
db.Exec("insert into users (username,password,createtime) values (?,?,?)", user.Username, user.Password, user.CreateTime)
```



#### 插入总示例

```go
package main

import (
	"fmt"
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
	"gorm.io/gorm/logger"
	"time"
)

var DB *gorm.DB

type Address struct {
	Addr string
}

type User struct {
	Id      int
	Name    string
	Age     int
	Created time.Time `gorm:"autoCreateTime"`
	Address `gorm:"embedded"`
}

func initDB() {
	//初始化，连接数据库
	username := "root"
	password := "kxh19970301"
	dbname := "gorm"
	dsn := fmt.Sprintf("%s:%s@tcp(127.0.0.1:3306)/%s?charset=utf8mb4&parseTime=True&loc=Local", username, password, dbname)
	//dsn:="root:kxh19970301@tcp(127.0.0.1)/sql_test"
	//官方示例 dsn := "user:pass@tcp(127.0.0.1:3306)/dbname?charset=utf8mb4&parseTime=True&loc=Local"
	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{
		//设置一个日志的级别，可以设置也可以不设置
		Logger: logger.Default.LogMode(logger.Info),
	})
	if err != nil {
		fmt.Printf("open mysql failed,err:%v\n", err)
		return
	}
	DB = db
	fmt.Println("mysql connect success!")
}

//插入
func insert(u *User) {
	//整体单条数据创建
	//res := DB.Create(u)
	//选择字段创建 Select
	//res := DB.Select("name", "age").Create(u)
	//忽略字段创建
	res := DB.Omit("name", "age").Create(u)
	err := res.Error
	if err != nil {
		fmt.Printf("insert user'data failed,err:%v\n", err)
		return
	}
	fmt.Println("affectedRows:", res.RowsAffected)
	fmt.Println("insert user'data success!")

	//使用原生的sql语句，即自己写sql语句。
	//DB.Exec("insert into users(name,age)values(?,?)", "张瑜", 27)

}

func main() {
	//初始化连接
	initDB()

	u := User{
		Id:      1003,
		Name:    "张瑜",
		Age:     26,
		Address: Address{Addr: "辽宁省大连市庄河市"},
	}
	insert(&u)
}

```







### 更新数据

在创建一个表

```sql
CREATE TABLE `goods` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '商品id',
  `title` varchar(100) NOT NULL COMMENT '商品名',
  `price` decimal(10, 2) NULL DEFAULT 0.00 COMMENT '商品价格',
  `stock` int(11) DEFAULT '0' COMMENT '商品库存',
  `type` int(11) DEFAULT '0' COMMENT '商品类型',
  `create_time` datetime NOT NULL COMMENT '商品创建时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```



```go
package dao

import "time"

type Goods struct {
	Id         int
	Title      string
	Price      float64
	Stock      int
	Type       int
	CreateTime time.Time
}

func (v Goods) TableName() string {
	return "goods"
}

func SaveGoods(goods Goods) {
	DB.Create(&goods)
}
```



```go
package dao

import (
	"testing"
	"time"
)

func TestSaveGoods(t *testing.T) {
	goods := Goods{
		Title:      "毛巾",
		Price:      25,
		Stock:      100,
		Type:       0,
		CreateTime: time.Now(),
	}
	SaveGoods(goods)
}
```



#### 插入或更新

```go
//在数据库中保存更新值。如果值不包含匹配的主键，则插入值

//即goods中的ID值在数据库表中存在，则是根据id来更新数据，否则，则是插入数据
DB.Save(&goods)   //如果id存在，会将根据id来更新数据，如果id不存在，则会插入数据 

```

==注意：Save方法与Create方法区别：如果Save保存的数据中ID已经在数据库表中存在了，此时会更新数据，其实可以理解为：Save更新其实是根据ID这个条件来进行判断的，但是如果不存在则会将该数据插入到表中。==



#### 更新单列

```go
//update用于更新单列数据
//通过Model来确定哪张数据库表，通过where条件来确定哪条数据需要更新
DB.Model(&User{}).Where("id = ?", id).Update("name", "kk")  //name表示数据库表中对应的字段
```



#### 更新多列

**在gorm中，如果在更新的数据中包含了id，会自动将id作为筛选条件**

```go
//更新多列：其实利用结构体字段和表字段对应，从而实现多列更新
//更新多列，分为两种情况，第一种使用结构体，第二种使用map

//使用结构体
DB.Where("id = ?", id).Updates(User{
		Name: "小章鱼",
		Age:  16,
})

//因为在gorm中，如果在更新的数据中包含了id，会自动将id作为筛选条件
DB.Updates(User{
		//注意：这里要更新的数据id，name，age，但是id值已经存在了，即使没有where条件，会自动将id作为where条件
		//默认会将id作为条件
		//UPDATE `users` SET `id`=1080,`name`='小章鱼2',`age`=16,`created`='2023-02-12 22:38:25.047'
		//WHERE `id` = 1080
		Id:   1080,
		Name: "小章鱼2",
		Age:  16,
})


//使用map
//利用map的键和表字段对应，从而实现更新多列
//因为这里使用是map，而不是结构体，所以无法自动将id作为判断条件
DB.Model(&User{}).Where("id = ?", id).Updates(map[string]interface{}{  
		"name": "张三",
		"age":  26,
}
```

#### 更新选定的字段

**选定字段**

```go
u := User{
		Id:      1008,
		Name:    "小人",
		Age:     266,
		Created: time.Now(),
		Address: Address{
			Addr: "大连",
		},
	}
	err := DB.Select("name", "addr").Updates(u).Error
	if err != nil {
		fmt.Println("updates data failed,err:", err)
		return
}

//UPDATE `users` SET `name`='小人',`created`='2023-02-12 22:48:36.496',`addr`='大连'
// WHERE `id` = 1008
```



```go
func selectUpdate() {
	u := UserInfo{
		UserName: "赵六",
		Age:      31,
		Address: Address{
			Country: "中国",
			City:    "澳门",
		},
	}
    //由于u结构体中没有写主键，不会自动以主键作为判断条件，此时需要通过where来指定判断条件
	err := DB.Select("age", "country", "city").Where("user_id=?", 5).Updates(&u).Error
	if err != nil {
		panic(err)
	}
	fmt.Println("select updates success!")
}
```





**排除字段**：

```go
u := User{
		Id:      1008,  //这里有主键，那么判断条件自动选择主键
		Name:    "小人",
		Age:     266,
		Created: time.Now(),
		Address: Address{
			Addr: "大连",
		},
	}
	err := DB.Omit("name", "addr").Updates(u).Error
	if err != nil {
		fmt.Println("updates data failed,err:", err)
		return
	}

//UPDATE `users` SET `id`=1008,`age`=266,`created`='2023-02-12 22:50:26.412' WHERE `id` = 1008
```

**select和omit组合使用**

```go
Select("*").Omit("title")
```

**gorm更新必须带条件进行更新，否则会返回错误**`gorm.ErrMissingWhereClause`，或者启用 `AllowGlobalUpdate` 模式

```go
db.Session(&gorm.Session{AllowGlobalUpdate: true}).Model(&User{}).Update("name", "jinzhu")
```



#### 表达式

```go
db.Model(&User{}).Where("id=?",1008).Update("age", gorm.Expr("age + 1"))
//会将age+1 的值更新到数据库表中
//UPDATE `users` SET `age`=age+1,`created`='2023-02-12 22:59:13.82' WHERE id = 1008

db.Model(&User{}).Where("id=?",1008).Updates(map[string]interface{}{"age": gorm.Expr("age + 1")})
//会将age+1 得到的值更新到数据库表中
```



#### 子查询更新

```go
//DB.Table("admin_users").Where("name=?", "哈哈") 将admin_users这张表的查询结果作为Update的更新数据，即子查询
DB.Model(&User{}).Where("id=?", 1008).Update("name", DB.Table("admin_users").Where("name=?", "哈哈"))

//UPDATE `users` SET `name`=(SELECT * FROM `admin_users` WHERE name='哈哈'),`created`='2023-02-
//12 23:16:01.592' WHERE id=1008
```



#### 更新总示例

```go
package main

import (
	"fmt"
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
	"gorm.io/gorm/logger"
	"time"
)

var DB *gorm.DB

type Address struct {
	Addr string
}

type User struct {
	Id      int
	Name    string
	Age     int
	Created time.Time `gorm:"autoUpdateTime"`
	Address `gorm:"embedded"`
}

func initDB() {
	//初始化，连接数据库
	username := "root"
	password := "kxh19970301"
	dbname := "gorm"
	dsn := fmt.Sprintf("%s:%s@tcp(127.0.0.1:3306)/%s?charset=utf8mb4&parseTime=True&loc=Local", username, password, dbname)
	//dsn:="root:kxh19970301@tcp(127.0.0.1)/sql_test"
	//官方示例 dsn := "user:pass@tcp(127.0.0.1:3306)/dbname?charset=utf8mb4&parseTime=True&loc=Local"
	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{
		//设置一个日志的级别，可以设置也可以不设置
		Logger: logger.Default.LogMode(logger.Info),
	})
	if err != nil {
		fmt.Printf("open mysql failed,err:%v\n", err)
		return
	}
	DB = db
	fmt.Println("mysql connect success!")
}

//更新
func updateAll(u User) {
	//在数据库中保存更新值。如果值不包含匹配的主键，则插入值
	err := DB.Save(&u).Error
	if err != nil {
		fmt.Printf("update data failed,err:%v\n", err)
		return
	}
	fmt.Println("update data success!")
}

//更新单列
func update(id int) {
	err := DB.Model(&User{}).Where("id = ?", id).Update("name", "小瑜").Error
	if err != nil {
		fmt.Println("update data failed,err:", err)
		return
	}
	fmt.Println("update data success!")
}

//更新多列
func updates(id int) {
	err := DB.Updates(User{
		//注意：这里要更新的数据id，name，age，但是id值已经存在了，即使没有where条件，会自动将id作为where条件
		//默认会将id作为条件
		//UPDATE `users` SET `id`=1080,`name`='小章鱼2',`age`=16,`created`='2023-02-12 22:38:25.047'
		//WHERE `id` = 1080
		Id:   1080,
		Name: "小章鱼2",
		Age:  16,
	}).Error
	if err != nil {
		fmt.Println("updates data failed,err:", err)
		return
	}
	fmt.Println("updates data success!")
}

func updateSelect() {
	u := User{
		Id:      1008,
		Name:    "张瑜小女人",
		Age:     266,
		Created: time.Now(),
		Address: Address{
			Addr: "大连庄河",
		},
	}
	err := DB.Omit("name", "addr").Updates(u).Error
	if err != nil {
		fmt.Println("updates data failed,err:", err)
		return
	}
	fmt.Println("updates data success!")
}

func updateExpr(id int) {
	err := DB.Model(&User{}).Where("id = ?", id).Update("age", gorm.Expr("age+1")).Error
	if err != nil {
		fmt.Println("update data failed,err:", err)
		return
	}
	fmt.Println("update data success!")

}

func main() {
	//初始化连接
	initDB()
	//u := User{
	//	Id:      1008,
	//	Name:    "张瑜2",
	//	Age:     26,
	//	Address: Address{Addr: "辽宁大连市庄河市"},
	//}
	//updateAll(u)
	//update(1020)
	//updates(1008)
	//updateSelect()
	//updateExpr(1008)
	
	//子查询更新
	//DB.Model(&User{}).Where("id=?", 1008).Update("name", DB.Table("admin_users").Where("name=?", "哈哈"))
}

```







### 删除数据

```go
//根据主键删除，填入的1003会被自动作为主键的判断条件
DB.Delete(&User{}, 1003)  //直接写数字，则是将该数和主键进行比较

//DELETE FROM `users` WHERE `users`.`id` = 1003
```

```go
//可以在后面定义判断条件
DB.Delete(&User{}, "age=27")  //&User{}用来定位哪张数据库表
//DELETE FROM `users` WHERE age=27
```

```go
//也可以不在Delete后面添加判断条件，利用where来添加判断条件，效果是一样的
DB.Where("id=?", 1002).Delete(&User{})
//DELETE FROM `users` WHERE id=1002
```

同样的道理，不带条件不能进行删除，必须加一些条件，或者使用原生 SQL，或者启用 `AllowGlobalUpdate` 模式

```go
db.Session(&gorm.Session{AllowGlobalUpdate: true}).Delete(&User{})
// DELETE FROM users
```



#### 删除总示例：

```go
package main

import (
	"fmt"
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
	"gorm.io/gorm/logger"
	"time"
)

var DB *gorm.DB

type Address struct {
	Addr string
}

type User struct {
	Id      int
	Name    string
	Age     int
	Created time.Time `gorm:"autoUpdateTime"`
	Address `gorm:"embedded"`
}

func initDB() {
	//初始化，连接数据库
	username := "root"
	password := "kxh19970301"
	dbname := "gorm"
	dsn := fmt.Sprintf("%s:%s@tcp(127.0.0.1:3306)/%s?charset=utf8mb4&parseTime=True&loc=Local", username, password, dbname)
	//dsn:="root:kxh19970301@tcp(127.0.0.1)/sql_test"
	//官方示例 dsn := "user:pass@tcp(127.0.0.1:3306)/dbname?charset=utf8mb4&parseTime=True&loc=Local"
	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{
		//设置一个日志的级别，可以设置也可以不设置
		Logger: logger.Default.LogMode(logger.Info),
	})
	if err != nil {
		fmt.Printf("open mysql failed,err:%v\n", err)
		return
	}
	DB = db
	fmt.Println("mysql connect success!")
}

//删除
func delete() {
	//Delete删除与给定条件匹配的值。如果值包含主键，则该值包含在条件中。
	//如果值包含deleted_at字段，则Delete将执行软删除，而不是将deleted_at设置为当前时间（如果为空）。
	//err := DB.Delete(&User{}, "age=27").Error

	//err := DB.Delete(&User{}, 1003).Error

	err := DB.Where("id=?", 1002).Delete(&User{}, nil).Error
	//DELETE FROM `users` WHERE id=1002
	if err != nil {
		fmt.Println("delete data failed,err:", err)
		return
	}
	fmt.Println("delete data success!")
}

func main() {
	initDB()
	delete()
}

```





### 查询数据

#### 查询函数

- Take：查询一条记录（查不出来会 报  record not found  错误）

  ```go
  //查询一条数据，相当于sql语句后面加了limt1
  db.Take(&goods)  //包含了将数据查询出来赋值给goods操作
  
  //可以加判断条件
  db.where("id=?",100).Take(&goods)
  ```

- First: 根据主键正序排序后，查询第一条数据（查不出来会 报  record not found  错误）

  ```go
  db.First(&goods)  //查询后进行保存
  //可以加判断条件
  ```

- Last：根据主键倒序排序后，查询最后一条记录（查不出来会 报  record not found  错误）

  ```go
  db.Last(&goods)   //查询后进行保存
  //可以加判断条件
  ```

- Find：查询==单条记录==或==多条记录==（查不出来不报错）

  ```go
   //查询后进行保存
  //可以加判断条件
  
  var u1 = []User{}
  //查询多条数据
  DB.Find(&u1)  //查询出全部数据
  
  //根据条件查询多条数据
  DB.Where("name=?","kk").Find(&u1)  //或者  DB.Find(&u1,`name="kk"`)
  
  //还可以利用Limit()方法，从多条中获取一条
  DB.Limit(1).Find(&u1, `name="kk"`)
  //SELECT * FROM `users` WHERE name="kk" LIMIT 1
  ```

- Scan:可以用于查询==单条数据==或==多条数据==

  - **scan类似Find都是用于执行查询语句，然后把查询结果赋值给结构体变量，区别在于scan不会从传递进来的结构体变量提取表名**

    ```go
    DB.Table("users").where("id=?",3).Scan(&u) 
    
    DB.Raw("select name,age,gender from users where name=?","zhangsan").Scan(&u)
    ```
    
    

- Pluck：查询一列值

  ```go
  var names []string
  db.Model(&Users{}).Pluck("name", &names)  //这个方法也必须指定表名
  //SELECT `name` FROM `users`
  ```

当 First、Last、Take 方法找不到记录时，GORM 会返回 ErrRecordNotFound 错误，可以通过对比`gorm.ErrRecordNotFound`进行判断，或者使用Find和Limit的组合进行查询。

```go
db.Limit(1).Find(&user)
```



#### where

通过db.Where函数设置条件

函数说明： `db.Where(query interface{}, args ...interface{})`

参数说明:

| 参数名 | 说明                                                         |
| :----- | :----------------------------------------------------------- |
| query  | sql语句的where子句, where子句中使用问号(?)代替参数值，则表示通过args参数绑定参数 |
| args   | where子句绑定的参数，可以绑定多个参数                        |

比如：

```go
//注意：这里是用切片来包裹一组值，表示sql语句中的一个集合
db.Where("id in (?)", []int{1,2,5,6}).Take(&goods)

```



#### select

设置select子句, 指定返回的字段

```go
var u User
DB.Select("name", "age").Find(&u)
```

也可以写聚合函数

```go
var total int
DB.Model(&User{}).Select("count(*) as name").Pluck("name", &total)
fmt.Println(total)  

//SELECT count(*) as name FROM `users`
```



#### order

排序

```go
var goods []Goods
//desc 降序，asc 升序，如果不写默认为升序
DB.Order("id desc").Find(&goods)  //只能写在find的前面，写find后面无用，跟sql原生语句还是存在差别
```



#### 分页

通过limit和Offset实现

```go
var users []User
//Offset() 偏移量指定开始返回记录之前要跳过的记录数
//asc 表示升序，desc 表示降序，如果不添加，则默认为升序
DB.Order("id asc").Limit(4).Offset(1).Find(&users) //SELECT * FROM `users` ORDER BY id LIMIT 4 OFFSET 1
fmt.Println("users:", users)


//利用limit和offset实现分页
//pageSize 页面中记录条数，page 页码
//limit --> pageSize ，Offset--> page
//Limt(pageSize).Offset((page-1)*pageSize)
```



#### count

返回查询匹配的行数

```go
//var count int64
//DB.Model(&User{}).Select("*").Count(&count)  // SELECT count(*) FROM `users`
//fmt.Println("count:", count)

var count int64
DB.Model(&User{}).Count(&count)  //SELECT count(*) FROM `users`
fmt.Println("count:", count) 
```



#### 分组

```go
//定一个Result结构体类型，用来保存查询结果
type Result struct {
		Name  string
		Count int   //注意：要存储结果的结构体中的字段名必须与select查询结果表字段对应上，否则无法将结果解析到结构体中国
	}
var results []Result
DB.Model(&User{}).Select("name,count(name)as count").Group("name").Having(`name="kk"`).Scan(&results)
fmt.Println("result:", results)

//scan类似Find都是用于执行查询语句，然后把查询结果赋值给结构体变量，区别在于scan不会从传递进来的结构体变量提取表名.
//这里因为我们重新定义了一个结构体用于保存结果，但是这个结构体并没有绑定users表，所以这里只能使用scan查询函数。
```

==**Group函数必须搭配Select函数一起使用**==



#### 直接执行sql语句

```go
//定一个Result结构体类型，用来保存查询结果
type Result struct {
		Name  string
		Count int   //注意：要存储结果的结构体中的字段名必须与select查询结果表字段对应上，否则无法将结果解析到结构体中国
	}
var results []Result
sql := "select name,count(name) as count from users group by name having name = ?"

//因为sql语句使用了一个问号(?)作为绑定参数, 所以需要传递一个绑定参数(Raw第二个参数).
//Raw函数支持绑定多个参数

db.Raw(sql, "kk").Scan(&results)
fmt.Println(results)
```



==**在gorm中有两个方法用来执行SQL原生语句：**==

+ **`func (db *DB) Exec(sql string, values ...interface{}) (tx *DB)`  ，注意：Exec用于执行插入、更新、删除**
+ **`func (db *DB) Raw(sql string, values ...interface{}) (tx *DB)` ，注意：Raw用于执行查询**





#### 查询总示例

```go
package main

import (
	"fmt"
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
	"gorm.io/gorm/logger"
	"time"
)

var DB *gorm.DB

type Address struct {
	Addr string
}

type User struct {
	Id      int
	Name    string
	Age     int
	Created time.Time `gorm:"autoUpdateTime"`
	Address `gorm:"embedded"`
}

func initDB() {
	//初始化，连接数据库
	username := "root"
	password := "kxh19970301"
	dbname := "gorm"
	dsn := fmt.Sprintf("%s:%s@tcp(127.0.0.1:3306)/%s?charset=utf8mb4&parseTime=True&loc=Local", username, password, dbname)
	//dsn:="root:kxh19970301@tcp(127.0.0.1)/sql_test"
	//官方示例 dsn := "user:pass@tcp(127.0.0.1:3306)/dbname?charset=utf8mb4&parseTime=True&loc=Local"
	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{
		//设置一个日志的级别，可以设置也可以不设置
		Logger: logger.Default.LogMode(logger.Info),
	})
	if err != nil {
		fmt.Printf("open mysql failed,err:%v\n", err)
		return
	}
	DB = db
	fmt.Println("mysql connect success!")
}

//查询
func FindUser() {
	//var u = User{}

	//没有条件，从表中查询一条数据
	//SELECT * FROM `users` LIMIT 1
	//err := DB.Take(&u).Error

	//根据主键来判断，默认会将1008作为主键判断的条件
	//SELECT * FROM `users` WHERE `users`.`id` = 1008 LIMIT 1
	//err := DB.Take(&u, 1008).Error

	//SELECT * FROM `users` WHERE name="张瑜小女人" LIMIT 1
	//注意：`name="张瑜小女人"` ，这里用``来包裹起来，因为name字段为字符串
	//err := DB.Take(&u, `name="张瑜小女人"`).Error

	//SELECT * FROM `users` WHERE name='张瑜小女人' LIMIT 1
	//err := DB.Where("name=?", "张瑜小女人").Take(&u).Error

	//根据主键正序排序后，查询第一条数据
	//err := DB.First(&u).Error  //同样也可以加判断条件
	//根据主键倒序排序后，查询最后一条记录
	//err := DB.Last(&u).Error

	//查询多条数据
	//var u1 = []User{}
	//err := DB.Find(&u1).Error

	//查询多条数据中一条
	//SELECT * FROM `users` WHERE name="张瑜" LIMIT 1
	//var u1 = []User{}
	//err := DB.Limit(1).Find(&u1, `name="张瑜"`).Error
	//
	//if err != nil {
	//	fmt.Println("find data failed,err:", err)
	//	return
	//}
	//fmt.Printf("find data success!,user'data :\n", u1)

	//查询一列的值
	//var names []string
	//DB.Model(&User{}).Pluck("name", &names)
	//fmt.Println(names)

	//var total int
	//DB.Model(&User{}).Select("count(*) as name").Pluck("name", &total)
	//fmt.Println(total)

	//var users []User
	////Offset() 偏移量指定开始返回记录之前要跳过的记录数
	//DB.Order("id asc").Offset(1).Limit(4).Find(&users)
	//fmt.Println("users:", users)

	//var count int64
	//DB.Model(&User{}).Select("*").Count(&count)  // SELECT count(*) FROM `users`
	//fmt.Println("count:", count)

	//var count int64
	//DB.Model(&User{}).Count(&count) //SELECT count(*) FROM `users`
	//fmt.Println("count:", count)

	//type Result struct {
	//	Name  string
	//	Count int
	//}
	//var results []Result
	////SELECT name,count(name)as count FROM `users` GROUP BY `name` HAVING name="张瑜"
	//DB.Model(&User{}).Select("name,count(name)as count").Group("name").Having(`name="张瑜"`).Scan(&results)
	//fmt.Println("result:", results)

	//select name,count(name) as count from users group by name having name = '张瑜'
	//DB.Raw("select name,count(name) as count from users group by name having name = ?", "张瑜").Scan(&results)
	//fmt.Println("result:", results)

	//insert into users(id,name,age)values(1028,'张瑜1',23)
	//err := DB.Exec("insert into users(id,name,age)values(?,?,?)", 1028, "张瑜1", 23).Error
	//fmt.Println(err)

}

func main() {
	initDB()
	FindUser()
}

```







## 事务和Hook

### 会话Session

为了避免共用db导致的一些问题(比如：公用一个db来操作不同的表，可能在对某个进行操作时有部分数据缓存在db内，其他进程可能利用该db操作其他表，于是存在将缓存的数据写入其他表中，这样在高并发情况下是非常不安全的)，**gorm提供了会话模式，通过新建session的形式，将db的操作分离，互不影响。**

创建session的时候，有一些配置：

```go
// Session 配置
type Session struct {
  DryRun                   bool   //生成 SQL 但不执行
  PrepareStmt              bool   //预编译模式
  NewDB                    bool  //新db 不带之前的条件
  Initialized              bool  //初始化新的db
  SkipHooks                bool  //跳过钩子
  SkipDefaultTransaction   bool  //禁用默认事务
  DisableNestedTransaction bool  //禁用嵌套事务
  AllowGlobalUpdate        bool  //允许不带条件的更新
  FullSaveAssociations     bool  //允许更新关联数据
  QueryFields              bool  //select（字段）,即不用*，而是会显示出字段
  Context                  context.Context
  Logger                   logger.Interface
  NowFunc                  func() time.Time //允许改变 GORM 获取当前时间的实现
  CreateBatchSize          int  
}
```

```go
//在操作的时候，于是就可以利用会话模式将db的操作分离，互不影响
//GORM 提供了 Session 方法，这是一个 New Session Method，它允许创建带配置的新建会话模式：
DB.Session(&gorm.Session{})
```



比如说可以禁用默认的事务，从而提供性能，官方说大致能提升30%左右：

```go
// 持续会话模式
tx := db.Session(&Session{SkipDefaultTransaction: true})
tx.First(&user, 1)
tx.Find(&users)
tx.Model(&user).Update("Age", 18)
```



比如使用`PreparedStmt` 在执行任何 SQL 时都会创建一个 prepared statement 并将其缓存，以提高后续的效率

```go
// 会话模式
tx := db.Session(&Session{PrepareStmt: true})
tx.First(&user, 1)
tx.Find(&users)
tx.Model(&user).Update("Age", 18)

// returns prepared statements manager
stmtManger, ok := tx.ConnPool.(*PreparedStmtDB)

// 关闭 *当前会话* 的预编译模式
stmtManger.Close()

// 为 *当前会话* 预编译 SQL
stmtManger.PreparedSQL // => []string{}

// 为当前数据库连接池的（所有会话）开启预编译模式
stmtManger.Stmts // map[string]*sql.Stmt

for sql, stmt := range stmtManger.Stmts {
  sql  // 预编译 SQL
  stmt // 预编译模式
  stmt.Close() // 关闭预编译模式
}
```



还有，gorm的db默认是协程安全的，如果使用初始化参数，则db不在协程安全：

```go
tx := db.Session(&gorm.Session{Initialized: true})
```



比如context：

```go
timeoutCtx, _ := context.WithTimeout(context.Background(), time.Second)
tx := db.Session(&Session{Context: timeoutCtx})

tx.First(&user) // 带有 context timeoutCtx 的查询操作
tx.Model(&user).Update("role", "admin") // 带有 context timeoutCtx 的更新操作
```



### 事务

#### 自动事务

```go
//1,自动事务

DB.Transaction(func(tx *gorm.DB) error {
	//在事务中执行一些db操作（从这里开始，应该使用tx而不是db）
	if err := tx.Create(&User{Name: "kk", Age: 18}).Error; err != nil {
		//返回任何错误都会自动回滚事务
		return err
	}
	//返回nil ，如果执行过程未出错，会自动提交事务
	return nil
})
```



#### 嵌套事务

GORM 支持嵌套事务，您可以回滚较大事务内执行的一部分操作，例如：

```go
//嵌套事务，自动事务
DB.Transaction(func(tx *gorm.DB) error {
	tx.Create(&User{Id: 2000})

	tx.Transaction(func(tx2 *gorm.DB) error {
		tx2.Create(&User{Id: 2001})
		//出现错误，即回滚事务
		return errors.New("rollback u2") //回滚u2
	})

	tx.Transaction(func(tx2 *gorm.DB) error {
		tx2.Create(&User{Id: 2002})
		//未出现错误，提交事务
		return nil
	})
	return nil
})
```



#### 手动事务

```go
//3，手动事务
tx := DB.Begin() //手动开启事务

//在事务中执行一些db操作（从这里开始，应使用tx而不是db）
err := tx.Create(&User{Id: 2010}).Error
//....

if err != nil {
	//遇到错误，执行回滚事务
	tx.Rollback() //手动回滚事务
} else {
	//未遇到错误，则提交事务
	tx.Commit() //手动提交事务
}
```



#### 保存点

GORM 提供了 `SavePoint`、`Rollbackto` 方法，来提供保存点以及回滚至保存点功能，例如：

**注意：如果已经提交了，则无法回滚到提交前的保存点。**

```go
//4，保存点
//gorm 提供了 SavePoint Rollbackto 方法，来提供保存点以及回滚至保存点的功能

tx := DB.Begin() //手动开启事务

tx.Create(&User{Name: "kk"}) //执行一些操作

tx.SavePoint("sp1") //添加保存点sp1

tx.Create(&User{Name: "kk1"}) //执行一些操作

tx.RollbackTo("sp1") //回滚至保存点sp1

tx.Commit() //手动提交事务

```



#### 事务总示例

```go
package main

import (
	"errors"
	"fmt"
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
	"gorm.io/gorm/logger"
	"time"
)

var DB *gorm.DB

type Address struct {
	Addr string
}

type User struct {
	Id      int
	Name    string
	Age     int
	Created time.Time `gorm:"autoUpdateTime"`
	Address `gorm:"embedded"`
}

func initDB() {
	//初始化，连接数据库
	username := "root"
	password := "kxh19970301"
	dbname := "gorm"
	dsn := fmt.Sprintf("%s:%s@tcp(127.0.0.1:3306)/%s?charset=utf8mb4&parseTime=True&loc=Local", username, password, dbname)
	//dsn:="root:kxh19970301@tcp(127.0.0.1)/sql_test"
	//官方示例 dsn := "user:pass@tcp(127.0.0.1:3306)/dbname?charset=utf8mb4&parseTime=True&loc=Local"
	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{
		//设置一个日志的级别，可以设置也可以不设置
		Logger: logger.Default.LogMode(logger.Info),
	})
	if err != nil {
		fmt.Printf("open mysql failed,err:%v\n", err)
		return
	}
	DB = db
	fmt.Println("mysql connect success!")
}

//事务
//1,自动事务
func autoTransaction() {
	DB.Transaction(func(tx *gorm.DB) error {
		//在事务中执行一些db操作（从这里开始，应该使用tx而不是db）
		if err := tx.Create(&User{Name: "kk1", Age: 18}).Error; err != nil {
			//返回任何错误都会自动回滚事务
			return err
		}
		//返回nil 会自动提交事务
		return nil
	})
}

//2，嵌套事务，用于回滚较大事务内的一部分操作
func NestTransaction() {
	DB.Transaction(func(tx *gorm.DB) error {
		tx.Create(&User{Id: 2000})

		tx.Transaction(func(tx2 *gorm.DB) error {
			tx2.Create(&User{Id: 2001})
			//出现错误，即回滚事务
			return errors.New("rollback u2") //回滚u2
		})

		tx.Transaction(func(tx2 *gorm.DB) error {
			tx2.Create(&User{Id: 2002})
			//未出现错误，提交事务
			return nil
		})
		return nil
	})
}

//3，手动事务
func ManualTransaction() {

	tx := DB.Begin() //手动开启事务

	//在事务中执行一些db操作（从这里开始，应使用tx而不是db）
	err := tx.Create(&User{Id: 2010}).Error
	//....

	if err != nil {
		//遇到错误，执行回滚事务
		tx.Rollback() //手动回滚事务
	} else {
		//未遇到错误，则提交事务
		tx.Commit() //手动提交事务
	}
}

//4，保存点
func save_point() {
	//gorm 提供了 SavePoint Rollbackto 方法，来提供保存点以及回滚至保存点的功能

	tx := DB.Begin() //手动开启事务

	tx.Create(&User{Name: "kk"}) //执行一些操作

	tx.SavePoint("sp1") //添加保存点sp1

	tx.Create(&User{Name: "kk"}) //执行一些操作

	tx.RollbackTo("sp1") //回滚至保存点sp1

	tx.Commit() //手动提交事务
}

func main() {
	//初始化链接mysql
	initDB()
	//autoTransaction()  //自动事务

	//NestTransaction() //嵌套事务

	save_point() //保存点
}

```







### Hook

==Hook 是在**创建、查询、更新、删除**等操作**之前、之后**调用的函数。==

**如果您已经为模型定义了指定的方法，它会在创建、更新、查询、删除时自动被调用**。==如果钩子方法返回错误，GORM 将停止后续的操作并回滚事务==。

钩子方法的函数签名应该是 `func(*gorm.DB) error`

**钩子的作用：可以在钩子执行一些操作，利用钩子的自动调用，可以在 创建、更新、删除、查询 前后进行一些操作，使之更加灵活。**





#### 创建

**创建时可用的 hook，有四个钩子**，create可以触发钩子，创建了几条记录触发几次钩子，没有创建成功则不会触发钩子

```go
钩子和创建操作的执行顺序：
// 开始事务
BeforeSave  //钩子
BeforeCreate   //钩子
// 关联前的 save
插入记录至 db   //插入操作
// 关联后的 save
AfterCreate  //钩子
AfterSave   //钩子  只要是创建操作都可以使用钩子，不局限使用创建哪种方法
// 提交或回滚事务
```

```go
func (*User) BeforeSave(tx *gorm.DB) error {  //gorm中定义相关接口，只要实现实现接口中的方法，就可以使用钩子
	fmt.Println("before save ...")
	return nil
}

func (*User) AfterSave(tx *gorm.DB) error {
	fmt.Println("after save ...")
	return nil
}

func (*User) BeforeCreate(tx *gorm.DB) error {
	fmt.Println("before create ...")
	return nil
}

func (*User) AfterCreate(tx *gorm.DB) error {
	fmt.Println("after create ...")
	return nil
}
```

> ==**在 GORM 中保存、删除操作会默认运行在事务上**==， 因此在事务完成之前该事务中所作的更改是不可见的，如果您的钩子返回了任何错误，则修改将被回滚。



#### 更新

**更新时可用的 hook，有四个钩子**，update、updates、save可以触发钩子，更新了几条记录触发几次钩子，表中记录没有更新成功则不会触发

```go
钩子和更新操作的执行顺序：钩子执行顺序：
// 开始事务
BeforeSave //钩子
BeforeUpdate   //钩子
// 关联前的 save
更新 db 操作  //更新操作
// 关联后的 save
AfterUpdate  钩子
AfterSave  //钩子  只要是更新操作都可以使用钩子，不局限使用更新哪种方法
// 提交或回滚事务
```

代码示例：

```go
func (*User) BeforeSave(tx *gorm.DB) error {  //gorm中定义相关接口，只要实现实现接口中的方法，就可以使用钩子
	fmt.Println("before save ...")
	return nil
}

func (*User) AfterSave(tx *gorm.DB) error {
	fmt.Println("after save ...")
	return nil
}

func (*User) BeforeUpdate(tx *gorm.DB) error {
	fmt.Println("before update ...")
	return nil
}

func (*User) AfterUpdate(tx *gorm.DB) error {
	fmt.Println("after update ...")
	return nil
}


```

>==**在 GORM 中保存、删除操作会默认运行在事务上**==， 因此在事务完成之前该事务中所作的更改是不可见的，如果您的钩子返回了任何错误，则修改将被回滚。



#### 删除

**删除时可用的 hook，只有两个钩子**，delete可以触发钩子，只有删除了记录才会触发钩子，没有删除掉记录则不会触发

```go
钩子和删除操作的执行顺序：

// 开始事务
BeforeDelete  //钩子
删除 db 中的数据操作  //删除操作
AfterDelete   //钩子   
// 提交或回滚事务
```

代码示例：

```go
func (*User) BeforeDelete(tx *gorm.DB) error {
	fmt.Println("before delete ...")
	return nil
}

func (*User) AfterDelete(tx *gorm.DB) error {
	fmt.Println("after delete ...")
	return nil
}
```

>==**在 GORM 中保存、删除操作会默认运行在事务上**==， 因此在事务完成之前该事务中所作的更改是不可见的，如果您的钩子返回了任何错误，则修改将被回滚。



#### 查询

**查询时可用的 hook，只有一个钩子**，take、frist、last、find、pluck、分页 触发钩子，注意只有查询出结果才会触发，几条结果触发几次

```go
钩子和查询操作的执行顺序：

从 db 中加载数据  // Preloading (eager loading)  //查询操作

AfterFind  //钩子，只要是查询操作都可以使用钩子，不一定是使用find
```

代码示例：

```go
func (*User) AfterFind(tx *gorm.DB) error {
	fmt.Println("after find ...")
	return nil
}
```

>==**在 GORM 中保存、删除操作会默认运行在事务上**==， 因此在事务完成之前该事务中所作的更改是不可见的，如果您的钩子返回了任何错误，则修改将被回滚。



#### 钩子总示例

```go
package main

import (
	"fmt"
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
	"gorm.io/gorm/logger"
	"time"
)

var DB *gorm.DB

type Address struct {
	Addr string
}

type User struct {
	Id      int
	Name    string
	Age     int
	Created time.Time `gorm:"autoUpdateTime"`
	Address `gorm:"embedded"`
}

func initDB() {
	//初始化，连接数据库
	username := "root"
	password := "kxh19970301"
	dbname := "gorm"
	dsn := fmt.Sprintf("%s:%s@tcp(127.0.0.1:3306)/%s?charset=utf8mb4&parseTime=True&loc=Local", username, password, dbname)
	//dsn:="root:kxh19970301@tcp(127.0.0.1)/sql_test"
	//官方示例 dsn := "user:pass@tcp(127.0.0.1:3306)/dbname?charset=utf8mb4&parseTime=True&loc=Local"
	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{
		//设置一个日志的级别，可以设置也可以不设置
		Logger: logger.Default.LogMode(logger.Info),
	})
	if err != nil {
		fmt.Printf("open mysql failed,err:%v\n", err)
		return
	}
	DB = db
	fmt.Println("mysql connect success!")
}

//钩子 Hook 是在创建、查询、更新、删除等操作之前、之后调用的函数

func (*User) BeforeSave(tx *gorm.DB) error {
	fmt.Println("before save ...")
	return nil
}

func (*User) AfterSave(tx *gorm.DB) error {
	fmt.Println("after save ...")
	return nil
}

func (*User) BeforeCreate(tx *gorm.DB) error {
	fmt.Println("before create ...")
	return nil
}

func (*User) AfterCreate(tx *gorm.DB) error {
	fmt.Println("after create ...")
	return nil
}

func (*User) BeforeUpdate(tx *gorm.DB) error {
	fmt.Println("before update ...")
	return nil
}

func (*User) AfterUpdate(tx *gorm.DB) error {
	fmt.Println("after update ...")
	return nil
}

func (*User) BeforeDelete(tx *gorm.DB) error {
	fmt.Println("before delete ...")
	return nil
}

func (*User) AfterDelete(tx *gorm.DB) error {
	fmt.Println("after delete ...")
	return nil
}

func (*User) AfterFind(tx *gorm.DB) error {
	fmt.Println("after find ...")
	return nil
}

//更新
func create() {
	DB.Create(&User{Id: 3001})
}

//删除
func delete() {
	DB.Delete(&User{}, "id=1006")
}

func main() {
	//初始化连接数据库
	initDB()
	//创建
	//create()
	//
	//删除
	delete()
}

```







## 高级查询

### scope

**作用域允许你复用通用的逻辑**，这种共享逻辑需要定义为类型`func(*gorm.DB) *gorm.DB`。

例子：

```go
func Paginate(r *http.Request) func(db *gorm.DB) *gorm.DB {
  return func (db *gorm.DB) *gorm.DB {
    q := r.URL.Query()
    page, _ := strconv.Atoi(q.Get("page"))
    if page == 0 {
      page = 1
    }

    pageSize, _ := strconv.Atoi(q.Get("page_size"))
    switch {
    case pageSize > 100:
      pageSize = 100
    case pageSize <= 0:
      pageSize = 10
    }

    offset := (page - 1) * pageSize
    return db.Offset(offset).Limit(pageSize)
  }
}

db.Scopes(Paginate(r)).Find(&users)
db.Scopes(Paginate(r)).Find(&articles)
```





### 智能选择字段

```go
type User struct {
  ID     uint
  Name   string
  Age    int
  Gender string
  // 假设后面还有几百个字段...
}

//由于User中包含很多的字段，我们查询结果可能只需要几个字段的数据即可，可以创建一个结构体来存储查询的数据，该结构体首先要保证查询的字段必须在原先的结构体内，并且类型也要相同
type APIUser struct {
  ID   uint
  Name string 
}

// 查询时会自动选择 `id`, `name` 字段
//利用Model来确定是哪张表，将查询结果放在另一个结构体，如下所示，之所以没有用select选择字段，是因为find可以根据结构体中字段自动赋值
db.Model(&User{}).Limit(10).Find(&APIUser{})
// SELECT `id`, `name` FROM `users` LIMIT 10
```



### 子查询

**where子查询**

```go
//在mysql中，子查询主要在，where子句中出现的子查询，from子句中出现的子查询，以及 select子句中出现的子查询。

//这里就可以在where子句进行子查询，因为where第二个参数为interface{}
db.Where("amount > (?)", db.Table("orders").Select("AVG(amount)")).Find(&orders)
// SELECT * FROM "orders" WHERE amount > (SELECT AVG(amount) FROM "orders");
```

**from子查询**

```go
db.Table("(?) as u", db.Model(&User{}).Select("name", "age")).Where("age = ?", 18).Find(&User{})
// SELECT * FROM (SELECT `name`,`age` FROM `users`) as u WHERE `age` = 18

subQuery1 := db.Model(&User{}).Select("name")
subQuery2 := db.Model(&Pet{}).Select("name")
db.Table("(?) as u, (?) as p", subQuery1, subQuery2).Find(&User{})
```



==**使用gorm子查询如果太复杂，可以直接执行sql的原生语句。**==



### 关联操作

```sql
CREATE TABLE `gorm`.`user_profiles`  (
  `id` int(20) NOT NULL AUTO_INCREMENT,
  `sex` tinyint(4) NULL DEFAULT NULL,
  `age` int(10) NULL DEFAULT NULL,
  `user_id` int(20) NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4;
```



比如有一个用户属性表，查询用户的时候需要将其的性别和年龄查询出来：

```go
type UserProfile struct {
	ID     int64
	UserId int64
	Sex    int
	Age    int

}

func (u UserProfile) TableName() string {
	return "user_profiles"
}
```



```go
type User struct {
	ID          int64
	Username    string `gorm:"column:username"`
	Password    string `gorm:"column:password"`
	CreateTime  int64  `gorm:"column:createtime"`
	UserProfile UserProfile
}
```



保存User

```go
var user = User{
		Username:   "ms",
		Password:   "ms",
		CreateTime: time.Now().UnixMilli(),
		UserProfile: UserProfile{
			Sex: 0,
			Age: 20,
		},
	}
	DB.Save(&user)
```

会产生两条sql，users表和user_profiles表都有数据

> 这是因为默认的外键是结构体名字+下划线+id，即UserId或者表字段是user_id

如果将user_profiles表中的user_id改为other_id就会失败。

```go
type User struct {
	ID          int64
	Username    string      `gorm:"column:username"`
	Password    string      `gorm:"column:password"`
	CreateTime  int64       `gorm:"column:createtime"`
	UserProfile UserProfile `gorm:"foreignKey:OtherId"`
}
```

只要给UserProfile添加上相应的tag即可。



**关联标签**

| 标签             | 描述                                     |
| :--------------- | :--------------------------------------- |
| foreignKey       | 指定当前模型的列作为连接表的外键         |
| references       | 指定引用表的列名，其将被映射为连接表外键 |
| polymorphic      | 指定多态类型，比如模型名                 |
| polymorphicValue | 指定多态值、默认表名                     |
| many2many        | 指定连接表表名                           |
| joinForeignKey   | 指定连接表的外键列名，其将被映射到当前表 |
| joinReferences   | 指定连接表的外键列名，其将被映射到引用表 |
| constraint       | 关系约束，例如：`OnUpdate`、`OnDelete`   |



#### 查询

```go
var users []User
err := DB.Preload("UserProfile").Find(&users).Error
fmt.Println(err)
fmt.Println(users)
```

Preload预加载，直接加载关联关系

也可以使用joins进行加载关联数据：

```go
var users []User
err := DB.Joins("UserProfile").Find(&users).Error
fmt.Println(err)
fmt.Println(users)
```

从sql中能看的出来，使用了left join。

如果不想要User的数据，只想要关联表的数据，可以这么做：

```go
var user User
DB.Where("id=?", 25).Take(&user)
var userProfile UserProfile
err := DB.Model(&user).Association("UserProfile").Find(&userProfile)
fmt.Println(err)
fmt.Println(userProfile)
```







# Go-micro框架

## 微服务架构

### 微服务架构和微服务

+ 微服务架构：微服务架构是一种具体的设计实现或者设计方案，是将复杂的系统使用组件化的方式进行拆分，并使用轻量级通讯方式进行整合的一种设计方法。
+ 微服务：微服务是微服务架构具体的实现方案，是通过微服务架构设计方法拆分出来的一个独立的组件化的小应用。

微服务架构定义的精髓，就是==**“分而治之，合而用之”**==。将复杂的系统进行拆分的方法，即使“分而治之”。使用轻量级通讯等方式进行整合的设计，就是“合而用之”的方法，合而用之可以让微小的力量变强大。



### 什么是微服务架构



#### 单体架构的程序部署在单台服务器

将整个应用程序部署在一台服务器上，如 web服务（nginx）、网站程序、静态资源（图片）、数据库（mysql、redis）都在一台服务器上面。如果每天网站的访问IP在5万以下这种架构完全可以应付。

![image-20230221100232701](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230221100232701.png)



#### 单体架构的程序部署在多台服务器（负载均衡）

将程序部署在多台服务器上，然后通过nginx配置负载均衡，当客户访问我们的项目的时候，将请求随机分配到不同服务器处理响应，这样可以防止宕机，提升系统的稳定性。

![image-20230221100740417](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230221100740417.png)



#### 单体架构的程序部署在多台服务器（负载均衡+主从数据库）

这样的架构能轻松应对每天几百万、上千万的访问量。

![image-20230221100905991](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230221100905991.png)



当每天有上亿访问量，或者更高并发量的时候，上面的方法就有点力不从心，这时候可以使用微服务架构。



#### 微服务架构

**微服务架构：**就是把单体架构项目抽离成多个项目（服务），部署到多台服务器。



![image-20230221101537915](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230221101537915.png)





## RPC架构

### RPC概念

RPC（Remote Procedure Call Protocol），是远程过程调用的缩写，即调用其他服务器上的函数。



























# Go-Zero框架

官方文档：[go-zero](https://go-zero.dev/)

《go-zero [微服务框架](https://so.csdn.net/so/search?q=微服务框架&spm=1001.2101.3001.7020)的架构设计》：[#101 晓黑板 go-zero 微服务框架的架构设计_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1rD4y127PD/?vd_source=f0bcc8cf1682bd302fd8fa31924451d2)









# Nginx框架









# Docker教程

## docker简介

 docker是基于Go语言实现的云开源项目，主要目标是“build、ship and run any app、anywhere”，通过对应用组件的封装、分发、部署、运行等声明周期的管理，使用户的APP（或者一个web应用或数据库应用）及其运行环境能够做到**“一次镜像，处处运行”。**

Linux容器技术的出现就解决了这样一个问题，而docker就是在它的基础上发展过来的。==将应用打成镜像，通过镜像成为运行在docker容器上面的实例，而docker容器在任何操作系统上都是一致的，这就实现了跨平台、跨服务器。只需要一次配置好环境，换到别的机器上就可以一键部署好，大大简化了操作。==



==**docker优点：**==

+  系统平滑移植，容器虚拟化技术
+ 软件可以带环境安装，即在安装时，把原始环境一摸一样地复制过来，开发人员利用docker可以消除协作编码时“在我机器上能运行”的问题。

![image-20230221112952374](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230221112952374.png)



==**容器和虚拟机的区别**：==

+ 传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整的操作系统，在该系统上再运行所需应用进程。
+ 容器内的应用进程直接运行于宿主的内核，**容器没有自己的内核且没有进行硬件虚拟**。因此容器比传统虚拟机更为轻便。
+ 每个容器之间相互隔离，每个容器有自己的文件系统，**容器之间进程不会相互影响**，能区分计算资源。





## docker安装

docker并非是一个通用的容器工具，它依赖于已存在并运行的Linux内核环境。Docker实质上是已经运行的Linux下制造了一个隔离的文件环境，因此它执行的效率几乎等同于所部署的Linux主机。

即：==docker必须部署在linux内核的系统上，如果其他系统想要部署docker就必须安装一个虚拟linux环境。==

在windows上部署docker的方法是先安装一个虚拟机，并在安装linux系统的虚拟机上运行docker。



### docker基本组成

#### 镜像

docker镜像就是一个只读的模板。**通过镜像可以用来创建docker容器，一个镜像可以创建很多容器。** docker镜像文件类似于java的类模板，而docker容器实例类似于java中new出来的实例对象。

![image-20230222103608057](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230222103608057.png)



#### 容器

+ 从面向对象角度：**docker利用容器独立运行的一个或一组应用，应用程序或服务运行在容器里面，容器就类似于一个虚拟化的运行环境，**容器是用镜像创建的运行实例。镜像是静态的定义，容器是镜像运行时的实体。**容器为镜像提供了一个标准的隔离运行环境**，它可以被启动、开始、停止、删除。每个容器都是相互隔离的，保证了安全平台。
+ 从镜像容器角度：**可以把容器看作是一个简易版的linux环境**（包括root用户权限、进程空间、用户空间和网络空间等）和运行在其中的应用程序。



#### 仓库

仓库（repository）是集中存放镜像文件的场所。类似于github仓库存放各种git项目的地方。docker公司提供了官方registry被称为docker hub ，存放各种镜像模板的地方。

仓库分为公开仓库（public）和私有仓库（private）两种形式。

==最大的公开仓库是 docker hub（https://hub.docker.com/），存放了大量的镜像文件供用户下载。==国内的公开仓库包括阿里云、网易云等。



### docker平台架构图解

#### 架构图（简化版）



![image-20230222111659946](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230222111659946.png)



#### 架构图（详细版）

**Docker运行的基本流程：**

+ 用户使用docker client 与 docker daemon 建立通信，并发送请求给后者。
+ docker daemon 作为docker架构中主体部分，首先提供 docker server 的功能使其可以接受docker client的请求
+ docker engine 作为docker 内部的一系列工作，每一项工作都是以一个job的形式存在。
+ job的运行过程中，当需要容器镜像时，则从docker registry 中下载镜像，并通过镜像管理驱动Graph driver 将下载的镜像以Graph的形式存储。
+ 当需要为docker 创建网络环境时，通过网络管理驱动network driver 创建并配置docker 容器网络环境。
+ 当需要限制docker 容器运行资源或执行用户指令等操作时，则通过Exec driver来完成。
+ Libcontainer 是一项独立的容器管理包，network driver 以及 Exec driver 都是通过Libcontainer 来实现具体对容器进行操作。

<img src="C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230222152852145.png" alt="image-20230222152852145"  />







#### docker工作原理

docker是一个client-server结构的系统，docker守护进程运行在主机上，然后通过socket连接从客户端访问，守护进程从客户端接受命令并管理运行在主机上的容器。容器，是一个运行时环境，即前面所说的集装箱。

![image-20230222112136356](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230222112136356.png)



### 安装步骤

在ubuntu18.04上安装docker ，可以参考：[(30条消息) 在Ubuntu上安装docker（Ubuntu版本18.04）_WuJiaYFN的博客-CSDN博客_ubuntu18.04安装docker](https://blog.csdn.net/qq_44749630/article/details/128637716)



### 配置镜像加速器

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://yradmdz3.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



### docker run 工作流程

![image-20230223181031852](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230223181031852.png)



### dokcer 为什么比VM虚拟机快

+ docker 有着比虚拟机更少的抽象层：由于docker不需要Hypervisor（虚拟机）实现硬件资源虚拟化，运行在docker容器上的程序直接使用的都是实际物理机的硬件资源，因此在CPU、内存利用率上docker将会在效率上有明显优势。
+ docker 利用的是宿主机的内核，而不需要加载操作系统OS内核：当新建一个容器时，docke不需要和虚拟机一样重新加载一个操作系统。进而避免引寻、加载操作系统内核返回等比较费时费资源的过程，当新建一个虚拟机时，虚拟机软件需要加载OS，返回新建过程是分钟级别的。而docker由于直接利用宿主机的操作系统，则省略了返回过程，因此新建一个docker容器只需要几秒钟。

![image-20230223182307648](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230223182307648.png)







## docker常用命令

### 帮助启动类命令

| 命令                     | 说明                   |
| ------------------------ | ---------------------- |
| systemctl start docker   | 启动docker             |
| systemctl stop docker    | 停止docker             |
| systemctl restart docker | 重启docker             |
| systemctl status docker  | 查看docker状态         |
| systemctl enable docker  | 开机启动               |
| docker info              | 查看docker概要信息     |
| docker --help            | 查看docker总体帮助文档 |
| docker 具体命令 --help   | 查看docker命令帮助文档 |



### 镜像命令

| 命令                                  | 说明                                 |
| ------------------------------------- | ------------------------------------ |
| docker images                         | 列出本地主机上的镜像                 |
| docker images -a                      | 列出本地所有的镜像（含历史版本）     |
| docker images -q                      | 只显示镜像ID                         |
|                                       |                                      |
| docker search 镜像名                  | 在远程仓库查找某个镜像               |
| docker search --limit 数量  镜像名    | 只列出N个镜像，默认25个              |
| docker search --limit 5 redis         | 只列出5个redis镜像                   |
|                                       |                                      |
| docker pull 镜像名                    | 从远程仓库拉取某个镜像，默认拉取最新 |
| docker pull 镜像名:TAG                | 从远程仓库拉取某个版本的镜像         |
|                                       |                                      |
| docker system df                      | 查看镜像/容器/数据卷所占的空间       |
|                                       |                                      |
| docker rmi 镜像名或镜像ID             | 删除某个镜像                         |
| docker rmi -f 镜像名或镜像ID          | 强制删除某个镜像                     |
| docker rmi -f 镜像名1:TAG 镜像名2:TAG | 强制删除多个镜像                     |
| docker rmi -f $(docker images -qa)    | 强制删除全部镜像                     |





**docker images 显示的表头：**

![image-20230223183802279](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230223183802279.png)







### 容器命令

```go
//1,新建+启动容器
docker run [options] images [command] [arg...]

	options说明：选项有些是一个减号，有些是两个减号：
		--name="容器名字"     为容器指定一个名称；
		--d    后台运行容器返回ID，即启动守护式容器（后台运行）；

		-i     以交互模式运行容器，通常与 -t 同时使用；
		-t     为容器重新分配一个伪输入终端，通常与 -i 同时使用；
		-it    即将-i和-t同时使用，即启动交互式容器即启动交互式容器（前台有伪终端，等待交互）；

		-P     随机端口映射
		-p     指定端口映射（常用）
				-p 用法：
						-p hostPort:containerPort 端口映射 -p 8080:80
						-p ip:hostPort:containerPort 配置监听地址 -p 10.0.0.100:8080:80
						-p ip::containerPort 随机分配端口 -p 10.0.0.100::80
						-p hostPort:containerPort:udp 指定协议 -p 8080:80:tcp
						-p 81:80 -p 443:443 指定多个
	
		//表示使用镜像ubuntu:latest以交互模式启动一个容器，在容器内执行/bin/bash命令
		docker run -it ubuntu /bin/bash   
		//-i 表示交互式操作，-t 表示终端  ubuntu 是镜像  
		// /bin/bash 放在镜像名后的命令，因为希望有一个交互式shell，因此用的是 /bin/bash
		// 退出容器内的终端，直接输入exit



//2，列出当前所有正在运行的容器
	docker ps [options]
		
		options说明：
				-a 列出当前所有正在运行的容器+历史上运行过的容器
				-l 显示最近创建的容器
			   -n 数字  显示最近创建的n个容器
				-q 静默模式，只显示容器编号




//3，退出容器
	第一种方式，exit退出：即run进去容器，exit退出容器，此时容器会停止运行
	第二种方式，ctrl+p+q退出：run进去容器，ctrl+p+q退出，容器不会停止运行



//4，启动已停止运行的容器
	docker start 容器ID或者容器名



//5，重启容器
	docker restart 容器ID或者容器名


//6，停止容器
	docker stop 容器ID或者容器名


//7，强制停止容器
	docker kill 容器ID或者容器名


//8，删除已停止的容器
	docker rm 容器ID或容器名

//9，删除未停止的容器
	docker -rm -f 容器ID或容器名  //删除单个
	docker -rm -f $(docker ps -a -q)  //删除多个


//10，启动守护式容器（后台服务器）
	docker run -d 容器名或容器ID   	指定容器后台运行模式

	注意：某些容器，用上面命令启动容器后，我们用命令 docker ps 进行查看，发现容器已经退出了
	 docker容器在后台运行，就必须有一个前台进程，容器运行的命令如果不是那些一起挂起的命令就是会自动退出

	解决方案：将你要运行的程序以前台进程的形式运行，常见的就是命令行模式，表示我们还有交互操作，别中断
	


//11，查看容器日志
	docker logs 容器ID



//12，查看容器内运行的进程
	docker ps  			  查看docker运行的所有容器
	docker top 容器ID 	查看某个容器的内的进程


//13，查看容器内部细节
	dokcer inspect 容器ID


//14，进入正在运行的容器并以命令行交互

	//exec是在容器中打开新的终端，并且可以启动新的进程，用exit退出，不会导致容器的停止
	第一种方式： docker exec -it 容器ID  /bin/bash或者bash

	//attach直接进入容器启动命令的终端，不会启动新的进程，用exit退出，会导致容器的停止。
	第二种方式： docker attach 容器ID

	推荐：使用第一种方式 docker exec 命令进入容器，因为退出容器终端不会导致容器停止
		  因此，一般用 -d 后台启动程序，再用exec进入对应的容器实例

//15，从容器内拷贝文件到主机上
	docker cp 容器ID：容器内路径  目的主机路径
	
	//将主机上的文件拷贝到容器内
	docker cp 目的主机路径 容器ID:容器内路径


//16，导入和导出容器

	//export 导出容器的内容留作为一个tar归档文件（对应import命令）
	docker export 容器ID > 文件名.tar   //将整个容器备份
	//如：docker export a3c06329e82f > abcd.tar

	//import 从tar包中的内容创建一个新的文件系统再导入为镜像（对应export）
	cat 文件名.tar | docker import -镜像用户/镜像名：镜像版本号  //根据tar文件生成一个镜像，再根据镜像恢复上个容器
	//如：cat abcd.tar | docker import - atguigu/ubuntu:3.7


//17，docker镜像commit操作
	//docker commit 提交容器副本使之成为一个新的镜像
	docker commit -m=“提交的描述信息” -a=“作者” 容器ID 新目标镜像名：[版本名]
	//如：docker commit -m="vim cmd add ok" -a="zzyy" fcbfa3e8efc5 atguigu/myubuntu:1.3
```







## docker镜像

 ### 镜像

镜像是一种轻量级、可执行的独立软件包，它包含运行某个软件所需的所有内容，我们把应用程序和配置依赖打包好形成一个可交付的运行环境（包括代码、运行时需要的库、环境变量和配置文件等），这个打包好的运行环境就是image镜像文件。



只有通过这个镜像文件才能生成docker容器实例。



### 镜像的分层

在下载过程中，我们可以看到docker的镜像是一层一层的在下载。

![image-20230224135026404](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230224135026404.png)





### 联合文件系统（unionFS）

Union文件系统是一种分层、轻量级并且高性能的文件系统，它支持==对文件系统的修改作为一次提交来一层一层的叠加==，同时可以将不同目录挂载到同一个虚拟文件系统下。Union文件系统是docker镜像的基础。==镜像可以通过分层来进行继承，基于基础镜像（没有父镜像）==，可以制作各种具体的应用镜像。

特点：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。





### docker镜像加载原理

 docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，==在Docker镜像的最底层是引导文件系统bootfs==。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。



rootfs (root file system) ，在bootfs之上。包含的就是典型 Linux 系统中的 /dev, /proc, /bin, /etc 等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。





对于一个精简的OS，rootfs可以很小，只需要包括最基本的命令、工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供 rootfs 就行了。由此可见对于不同的linux发行版, bootfs基本是一致的, rootfs会有差别, 因此不同的发行版可以公用bootfs。





### 为什么docker镜像采用分层结构

镜像分层最大的好处就是共享资源，方便复制迁移，就是为了复用。

如：有多个镜像都是从相同的base镜像构建而来，那么docker host只需要在磁盘上保存一份base镜像；同时内存中也只需加载一份base镜像，就可以为所有的容器服务了。而且镜像的每一层都可以被共享。





==**docker 镜像层都是只读的，容器层是可写的，当容器启动时**==，一个新的可写层被加载到镜像的顶部。这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。











## 本地镜像发布到阿里云

官方Docker hub 地址：https://hub.docker.com/ ，中国大陆访问太慢了，且有准备被阿里云取代的趋势，不太主流，因此演示以阿里云为例。







![image-20230224154922769](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230224154922769.png)











## 本地镜像发布到私有库



![image-20230224154922769](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230224154922769.png)



### Docker Registry

Dockerhub、阿里云这样的公共镜像仓库可能不太方便，涉及机密的公司不太可能提供镜像给公网，所以需要创建一个本地私人仓库供团队使用，基于公司内部项目构建镜像。

Docker Registry 是官方提供的工具，可以用于构建私有镜像仓库。 



 ### 将本地镜像推送到私有库

+ 下载镜像Docker Registry

  + ```go
    docker pull registry
    ```

+ 运行私有库Registry，相当于本地有个私有Docker hub

  + ```go
    //默认情况，仓库被创建在容器的/var/lib/registry目录下，建议自行用容器卷映射，方便于宿主机联调
    //-p 5000:5000 端口映射
    //-v /smallkk/myregistry/:/tmp/registry --privileged=true   容器卷映射
    docker run -d -p 5000:5000 -v /smallkk/myregistry/:/tmp/registry --privileged=true registry
    ```

+ 案例演示创建一个新镜像，Ubuntu安装ifconfig命令

  + ```go
    apt-get install net-tools
    ```

+ curl验证私服库上有什么镜像

  + ```go
    curl -XGET http://192.168.0.149:5000/v2/_catalog
    ```

+ 将新镜像myubuntu1.0 修改符合私服规范的tag

  + ```go
    docker tag 镜像:Tag Host:Port/Repository:Tag
    //例如：docker tag myubuntu:2.0 192.168.0.149:5000/myubuntu:2.0
    ```

+ 修改配置文件使之支持http

  + ```go
    //修改配置，vim /etc/docker/daemon.json
    
    {
        "registry-mirros":["https://yradmdz3.mirror.aliyuncs.com"],
        "insecure-registries":["192.168.0.149:5000"]
    }
    
    //原因：docker 默认不允许http方式推送镜像，通过配置选项来取消该限制。（修改完后，如果不生效，则应重启docker）
    ```

+ push推送到私服库

  + ```go
    docker push 192.168.0.149:5000/myubuntu:2.0
    ```

+ curl验证私服库上有什么镜像

  + ```go
    curl -XGET http://192.168.0.149:5000/v2/_catalog
    ```

+ pull到本地并运行

  + ```go
    docker pull 192.168.0.149:5000/myubuntu:2.0
    ```







## dokcer容器数据卷

注意：docker挂载主机目录访问如果出现 `cannot open directory Permission denied` ，解决方案：在挂载目录后多加一个`--privileged=true`参数即可。



### 宿主和容器之间映射添加容器卷

==**卷：**==就是目录或文件，存在于一个或多个容器中，由docker挂载到容器，但不属于联合文件系统，因此能绕过Union File System 提供一些用于持续存储或共享数据的特性。

==**卷的设计目的**==：就==数据持久化==，==完全独立于容器的生存周期，因此docker不会在容器删除时删除其挂载的数据卷==。





![image-20230224195812697](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230224195812697.png)





### 读写规则映射添加说明



```go
//读写（默认）
docker run -it或-d --priviledge=true -v /宿主机绝对路径目录:/容器内目录  镜像名           //默认
等价于 docker run -it或-d --priviledge=true -v /宿主机绝对路径目录:/容器内目录:rw  镜像名

//只读，限制容器内部，只能读取不能写
docker run -it或-d --priviledge=true -v /宿主机绝对路径目录:/容器内目录:ro 镜像名

//例如：docker run -it --privileged=true -v /mydocker/u:/tmp/u:ro --name=uu ubuntu

//例如：docker run -d -p 5000:5000 -v /smallkk/myregistry/:/tmp/registry --privileged=true registry
```





由于docker可以将运用和运行的环境打包镜像，run后形成容器实例运行，但是我们对数据的要求希望是持久化的，docker容器产生的数据如果不备份，那么当容器实例删除后，容器内的数据自然也就没有了。

**为了将容器内的数据保存在主机中，我们使用卷。**



**特点**：

+ 数据卷可以在容器之间共享或重用数据。
+ 卷中的更改可以直接实时生效。
+ 数据卷中的更改不会包含在镜像的更新中。
+ 数据卷的生命周期一直持续到没有容器使用它为止。





### 容器卷的继承和共享

```go
//容器1完成和宿主机的映射
docker run -it --privileged=true -v /mydocker/u:/tmp/u --name=u1 ubuntu /bin/bash

//容器2继承容器1的卷规则 
docker run -it --privileged=true --volumes-from 父类 --name=u2 ubuntu /bin/bash
//docker run -it --privileged=true --volumes-from u1 --name=u2 ubuntu /bin/bash
//表示继承u1容器卷的规则，即容器卷映射路径跟u1是一样的
//既然宿主机路径没变，那么u1时对应的宿主机路径下的文件，在u2中容器卷路径中也是存在的
```







## docker 常规安装简介

### 用SQLyog连接mysql存在问题

**用SQLyog连接mysql时，会报如下的错误：**

![image-20230226145503839](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230226145503839.png)



**原因：**在mysql8.0之后更改了加密规则为`caching_sha2_passwoed`，而在mysql8.0之前则为`mysql_native_password`，用语句`alter user 'root'@'localhost' identified by 'xxx';`来修改密码会使用8.0默认的`caching_sha2_password`规则来加密，而SQLyog中找不到新的身份验证插件，加载身份验证插件错误，因此产生上述错误。



**解决防办法：**使用mysq_native_password规则重新设置密码，指令如下：

```go
//进入mysql数据库
use mysql;
//更改密码
alter user 'root'@'%' identified with mysql_native_password by 'xxxx';
//刷新权限
flush privileges;
```

最后，打开SQLyog重新连接即可。





### mysql安装（简单版）

```shell
#启动docker容器
docker run -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:latest

#进入容器
docker exec -it 容器ID /bin/bash

#进入mysql 
mysql -u root -p

```



### mysql安装（工作版）

`docker 中的mysql`存在几个问题：

+ 无法插入中文数据：
+ 删除容器，mysql中数据如何保存：



```shell
# 新建mysql容器实例
# 在启动docker容器时配置容器数据卷，来将容器mysql中的数据同步到宿主机，实现数据备份，防止容器删除造成数据丢失。

docker run -d -p 3306:3306 --privileged=true -v /huihui97/mysql/log:/var/log/mysql -v /huihui97/mysql/data:/var/lib/mysql -v /huihui97/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 --name mysql mysql:latest


# 新建my.cnf 通过容器卷同步给mysql容器实例,目的是解决容器中mysql无法插入中文字符问题
在 /huhui97/mysql/conf/下面新建一个my.cnf，内容如下：
# ======================
[client]
default_character_set=utf8mb4
[mysqld]
collation-server=utf8mb4_unicode_ci
character_set_server=utf8mb4
# ======================


# 查看mysql字符编码是否更改了
show variables like 'character%';


# 重新启动mysql容器实例，再重新进入容器并查看字符编码
docker restart 容器ID

# 再新建库，新建表，再插入中文数据
经历上述步骤后，插入中文数据是有效的
```







### redis安装

```shell
# 新建redis容器
docker run -d -p 6379:6379 --name myredis --privileged=true -v /huihui97/redis/redis.conf:/etc/redis/redis.conf -v /huihui97/redis/data:/data -d redis:latest redis-server /etc/redis/redis.conf

#进入容器
docker exec -it myredis /bin/bash

#连接redis
redis-cli
```







## docker复杂安装详说

### MySQL主从复制

```go
//1,新建主服务器容器实例3307
docker run -p 3307:3306 --name mysql-master -v /mydata/mysql-master/log:/var/log/mysql -v /mydata/mysql-master/data:/var/lib/mysql -v /mydata/mysql-master/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 -d mysql:latest


//2，进入/mydata/mysql-master/conf目录下新建my.cnf
my.cnf文件内容如下：
//====================================
[mysqld]
## 设置server_id，同一局域网中需要唯一
server_id=101 
## 指定不需要同步的数据库名称
binlog-ignore-db=mysql  
## 开启二进制日志功能
log-bin=mall-mysql-bin  
## 设置二进制日志使用内存大小（事务）
binlog_cache_size=1M  
## 设置使用的二进制日志格式（mixed,statement,row）
binlog_format=mixed  
## 二进制日志过期清理时间。默认值为0，表示不自动清理。
expire_logs_days=7  
## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断。
## 如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致
slave_skip_errors=1062

//====================================


//3,修改完配置后重启master实例
docker restart 容器名/容器ID


//4，进入mysql-master容器
docker exec -it 容器名/容器ID /bin/bash

注意：这里存在一个问题：
进入容器后，进入mysql中，输入命令 mysql -u root -p 后，直接回车不用输入密码即可进入。如果输入密码则会报错：Access denied for user 'root'@'localhost' (using password: YES)，可以参考博客上这种方法进行解决：https://blog.csdn.net/xiaoxiaoxiaopb/article/details/119351708


//5，master容器实例内创建数据同步给用户
create user 'slave'@'%' identified by '123456';   //在mysql中创建用户
grant replication slave,replication client on *.* TO 'slave'@'%';   //在mysql中给用户授予权限


//6，新建从服务器容器实例3308
docker run -p 3308:3306 --name mysql-slave -v /mydata/mysql-slave/log:/var/log/mysql -v /mydata/mysql-slave/data:/var/lib/mysql -v /mydata/mysql-slave/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 -d mysql:latest


//7,进入/mydata/mysql-slave/conf目录下新建my.cnf
my.cnf文件内容如下：
//====================================
[mysqld]
## 设置server_id，同一局域网中需要唯一
server_id=102
## 指定不需要同步的数据库名称
binlog-ignore-db=mysql  
## 开启二进制日志功能，以备Slave作为其它数据库实例的Master时使用
log-bin=mall-mysql-slave1-bin  
## 设置二进制日志使用内存大小（事务）
binlog_cache_size=1M  
## 设置使用的二进制日志格式（mixed,statement,row）
binlog_format=mixed  
## 二进制日志过期清理时间。默认值为0，表示不自动清理。
expire_logs_days=7  
## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断。
## 如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致
slave_skip_errors=1062  
## relay_log配置中继日志
relay_log=mall-mysql-relay-bin  
## log_slave_updates表示slave将复制事件写进自己的二进制日志
log_slave_updates=1  
## slave设置为只读（具有super权限的用户除外）
read_only=1

//====================================


//8，修改完配置后重启slave实例
docker restart mysql-slave



//9，在主数据库中查看主从同步状态
show master status;

//10,进入mysql-slave容器
docker exec -it mysql-slave /bin/bash

注意：这里使用命令： mysql -u root -p 进入mysql中需要输入密码：123456。

//11，在从数据库中配置主从复制
change master to master_host='192.168.0.149',master_user='slave',master_password='123456',master_port=3307,master_log_file='mall-mysql-bin.000001',master_log_pos=711,master_connect_retry=30;

注意：master_log_file和master_log_pos的值可以根据第9步中查看主数据库状态获取

//12，在从数据库中查看主从同步状态
show slave status \G;

//13，在从数据库中开启主从同步
start slave;

//14，查看从数据库状态发现已经同步
show slave status \G;

//15，主从复制测试

在主数据库中创建数据库，创建表，插入数据
create database mysql_master;
use mysql_master;
create table t1(id int,name varchar(32));
insert into t1(id,name)values(101,'zhangsan');

在从数据库就可以直接使用主数据库中的数据了，这就是主从同步
```





### redis集群









## dockerFile解析



### dockerFile简介



**dockerFile是用来构建docker镜像的文本文件，是由一条条构建镜像所需的指令和参数构成的脚本。**



利用dockerFile构建镜像三步：

+ 编写dockerFile文件
+ docker build命令构建镜像
+ docker run 依镜像运行容器实例



### dockerFile构建过程解析

#### dockerFile内容基础知识

+ 每条==保留字指令都必须为**大写字母**且后面要跟随至少一个参数==
+ 指令按照从上到下，顺序 执行
+ \# 表示注释
+ 每条指令都会创建一个新的镜像层并对镜像进行提交





#### docker执行dockerFile的大致流程

+ docker从基础镜像运行一个容器
+ 执行一条指令并对容器作出修改
+ 执行类似docker commit的操作提交一个新的镜像层
+ docker再基于刚提交的镜像运行一个新的容器
+ 执行dockerfile中的下一条执行直到所有指令都执行完成





### dockerFile常用保留字指令

```go
//1,FROM 基础镜像，当前新镜像是基于哪个镜像的，指定一个已经存在的镜像作为模板，第一条一般都是FROM
FROM 基础镜像
例如：FROM amazoncorrectto:8

//2，MAINTAINER 镜像的维护者的姓名和邮箱地址
MAINTAINER

//3 RUN 容器构建时需要运行的命令，有两种格式，一是shell格式：，二是exec格式：，RUN实在docker build时运行的
//shell格式：
RUN <命令行命令> //等同于在终端操作shell命令
例如：RUN yum -y install vim       等同于在linux上运行 yum -y install vim 命令

//exec格式：
RUN ["可执行文件","参数1","参数2"]
例如：RUN["./test.php","dev","offline"]    等价于 RUN ./test.php dev offline


//4，EXPOSE  当前容器对外暴露出的端口
EXPOSE

//5，WORKDIR 指定在创建容器后，终端默认登陆进来的工作目录，一个落脚点
WORKDIR

//6，USER  指定该镜像是以什么样的用户去执行，如果都不指定，默认是root，一般都不指定
USER

//7，ENV 用来在构建镜像过程中设置环境变量
ENV

//8，ADD  将宿主机目录下的文件拷贝进镜像且会自动处理URL和解压tar压缩包
ADD

//9, COPY  类似ADD，拷贝文件和目录到镜像中。
//将从构建上下文目录中<源路径>的文件/目录复制到新的一层的镜像内的<目标路径>位置。                                   
COPY

//10,VOLUME 容器数据卷，用于数据保存和持久化工作
VOLUME

//11, CMD  指定容器启动后的要干的事情
//CMD指令的格式和RUN相似，也是两种格式：
//shell格式：CMD <命令>
//exec格式：CMD ["可执行文件","参数1","参数2",...]
//参数列表格式：CMD ["可执行文件","参数1","参数2",...]。在指定ENTRYPOINT指令后，用CMD指定具体的参数。
//注意：dockerfile中可以有多个CMD指令，但只有最后一个CMD指令生效，CMD指令会被docker run 之后的参数替换

//CMD和RUN命令的区别：    CMD是在docker run 时运行的；    RUN是在docker build时运行的。
CMD

//12，ENTRYPOINT 也是用来指定一个容器启动时要运行的命令，类似于CMD命令，但是 ENTRYPOINT 不会被docker run 后面的命令覆盖，而且这些命令行参数会被送给 ENTRYPOINT 指令指定的程序。
ENTRYPOINT
```





**ENTRYPOINT命令详细说明：**

![image-20230227205002968](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230227205002968.png)





### 案例

注意：==编写Dockerfile文件时，文件名D一定要大写。==

```go
需求：centos7镜像具备vim+ifconfig+go

//1，编写Dockerfile文件
如编写示例中的Dockerfile文件

//2，构建镜像，利用下面命令构建镜像必须在Dockerfile文件路径下执行，否则 . 有问题
docker build -t 新镜像名:TAG .             //注意，TAG后面有一个空格，有个点

//3，运行新镜像
docker run -it 新镜像名:TAG
```



```shell
# Dockerfile文件 示例：
FROM centos
MAINTAINER zzyy<zzyybs@126.com>
 
ENV MYPATH /usr/local
WORKDIR $MYPATH
 
#安装vim编辑器
RUN yum -y install vim
#安装ifconfig命令查看网络IP
RUN yum -y install net-tools
#安装java8及lib库
RUN yum -y install glibc.i686
RUN mkdir /usr/local/java
#ADD 是相对路径jar,把jdk-8u171-linux-x64.tar.gz添加到容器中,安装包必须要和Dockerfile文件在同一位置
ADD jdk-8u171-linux-x64.tar.gz /usr/local/java/
#配置java环境变量
ENV JAVA_HOME /usr/local/java/jdk1.8.0_171
ENV JRE_HOME $JAVA_HOME/jre
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
ENV PATH $JAVA_HOME/bin:$PATH
 
EXPOSE 80

CMD echo $MYPATH
CMD echo "success--------------ok"
CMD /bin/bash

```







```shell
# 演示示例

FROM ubuntu
MAINTAINER huihui<huihui@163.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN apt-get update
RUN apt-get install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "install ifocnfig cmd into ubuntu success------ok"
CMD /bin/bash

```

```shell
# 演示示例的运行结果：
root@huihui-virtual-machine:~/myfile# docker build -t newubuntu:1.0.0 .
[+] Building 42.5s (8/8) FINISHED                                                                          
 => [internal] load build definition from Dockerfile                                                  0.0s
 => => transferring dockerfile: 278B                                                                  0.0s
 => [internal] load .dockerignore                                                                     0.0s
 => => transferring context: 2B                                                                       0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                      0.0s
 => [1/4] FROM docker.io/library/ubuntu                                                               0.0s
 => [2/4] WORKDIR /usr/local                                                                          0.0s
 => [3/4] RUN apt-get update                                                                         39.5s
 => [4/4] RUN apt-get install net-tools                                                               2.9s
 => exporting to image                                                                                0.1s 
 => => exporting layers                                                                               0.1s 
 => => writing image sha256:2f670a88aa6b3a87cbcf2d6373e67923dc0d86419bddb7630bd9a1c9864cd61c          0.0s 
 => => naming to docker.io/library/newubuntu:1.0.0                                                    0.0s 
root@huihui-virtual-machine:~/myfile#          
```







### 虚悬镜像

虚悬镜像：该镜像是 一种仓库名、标签都是<none>的镜像，俗称 dangling image。

```go
//查看所有的虚悬镜像
docker image Is -f dangling=true

//一般虚悬镜像是错乱的，失去了存在的价值，应该删除
docker image prune   //删除虚悬镜像
```







## docker微服务实战

通过docker将微服务部署到docker容器上即可。











## docker网络

### 常用基本命令

```go
//1，查看docker 网络模式 
docker network ls

//2,创建网络
docker network create 网络名

//3，删除一个或多个网络
docker network rm 网络名

//4,查看网络源数据
docker network inspect 网络名

//5,将容器连接到网络
docker network connect 网络名或ip地址 容器名或容器ID
//6，断开容器网络
docker network disconnet 网络名或ip地址 容器名或容器ID

//7，删除所有未使用的网络
docker network prune

```



### docker网络作用

+ 容器间的互联和通信以及端口映射
+ 容器IP变动时候可以通过服务名直接网络通信而不受到影响







### 网络模式



| 网络模式   | 简介                                                         | 指定命令                                                     |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **bridge** | 为每一个容器分配、设置IP等，<br/>并将容器连接到一个docker0 虚拟网桥，默认为该模式 | `--network bridge`指定，**默认使用docker0**                  |
| **host**   | 容器将不会虚拟自己的网卡，<br/>配置自己的IP等，而是使用宿主机的IP和容器的端口号 | `--network host`指定                                         |
| none       | 容器有独立的network namespace，<br/>但并没有对其进行任何网络设置，如分配veth pair和网桥连接，IP等 | `--network none`指定                                         |
| container  | 新创建的容器不会创建自己的网卡和配置自己的IP，<br/>而是和一个指定的容器共享IP、端口范围等。 | `--network container:容器名或容器ID`指定                     |
| 自定义网络 | 1，自定义桥接网络，自定义默认使用bridge<br>2, 新建自定义网络<br>3，新建容器加入上一步新建的自定义网络<br>4，互相ping测试 | docker network create h1<br>docker run -d -p 8081:8080 --network h1<br> --name tomcat81 tomcat<br>docker run -d -p 8082:8080 --network h1 <br>--name tomcat82 tomcat |



```go
//例如：指定网络模式 host
docker run -d -p 8083:8080 --network host --name tomcat83 billygoo/tomcat8-jdk8

//例如，默认网络模式采用 bridge
docker run -d -p 8083:8080 --name tomcat83 billygoo/tomcat8-jdk8
docker run -d -p 8083:8080 --network bridge --name tomcat83 billygoo/tomcat8-jdk8

//例如，指定网络模式 none
docker run -d -p 8083:8080 --network none --name tomcat83 billygoo/tomcat8-jdk8

//例如，指定网络模式 container
docker run -d -p 8083:8080 --network container:tomcat85 --name tomcat83 billygoo/tomcat8-jdk8
```











## docker-compose容器编排

docker-compose是docker官方的开源项目，负责实现对docker容器集群的快速编排。

Compose是Docker公司推出的一个工具软件，可以管理多个docker容器组成一个应用。我们需要定义**一个YAML格式的配置文件**`docker-compose.yml`，写好多个容器之间的调用关系，然后，只要一个命令，就能同时启动或关闭这些容器。



Docker-Compose 具体安装方法参考：[Install the Compose plugin (docker.com)](https://docs.docker.com/compose/install/linux/)







### Compose核心概念

+ 一文件：`docker-compose.yml`
+ 两要素：
  + 服务（service）：一个个应用容器实例，比如订单微服务、库存微服务、mysql容器、nginx容器、redis容器等。
  + 工程（project）：由一组关联的应用容器组成**一个完整业务单元**，在`docker-compose.yml`文件中定义。







### Compose使用步骤

+ 第一步：编写Dockerfile，定义各个微服务应用并构建出对应的镜像文件
+ 第二步：使用`docker-compose.yml`定义一个完整业务单元，安排好整体应用中的各个容器服务。
+ 第三步：执行`docker-compose up`命令来启动并运行整个应用程序，完成一键部署上线。







### Compose常用命令

```go
//1,查看帮助
docker-compose -h 

//2,启动所有的docker-compose服务
docker-compose up

//3,启动所有docker-compose服务并后台运行
docker-compose up -d

//4,停止并删除容器、网络、卷、镜像
docker-compose down

//5,进入容器实例内部 docker-compose exec docker-compose.yml文件中写的服务id /bin/bash
docker-compose exec yml里面的服务id

//6，展示当前docker-compose编排的运行的所有容器
docker-compose ps

//7,展示当前docker-compose编排的容器进程
docker-compose top

//8，查看容器输出日志
docker-compose logs yml里面的服务id

//9,检查docker-compose.yml文件
docker-compose config

//10,检查docker-compose.yml文件是否有问题，有问题才输出
docker-compose config -q

//11，重启服务
docker-compose restart

//12,启动服务
docker-compose start

//13，停止服务
docker-compose stop
```







### Compose编排微服务

```go
//使用Compose

1,编写docker-compose.yml文件
2，执行docker-compose up 或者 执行 docker-compose up -d

```



```yaml
# docker-compose文件示例：

# 版本号
version: "3"    
 
# 服务，即需要一键启动的服务 
services:
  microService:  # 服务名，自己定义，只要不冲突就行
    image: zzyy_docker:1.6  # 镜像名：版本号
    container_name: ms01    #定义容器名
    ports:                   # 端口映射
      - "6001:6001"
    volumes:               # 容器数据卷
      - /app/microService:/data
    networks:             # 网络模式 自定义网络
      - atguigu_net 
    depends_on:          # 表名该微服务依赖redis和mysql才启动 
      - redis
      - mysql
 
  redis:
    image: redis:6.0.8
    ports:
      - "6379:6379"
    volumes:
      - /app/redis/redis.conf:/etc/redis/redis.conf
      - /app/redis/data:/data
    networks: 
      - atguigu_net
    command: redis-server /etc/redis/redis.conf    #命令  redis-server运行/etc/redis/redis.conf 
 
  mysql:
    image: mysql:5.7
    environment:  # 环境
      MYSQL_ROOT_PASSWORD: '123456'
      MYSQL_ALLOW_EMPTY_PASSWORD: 'no'
      MYSQL_DATABASE: 'db2021'
      MYSQL_USER: 'zzyy'
      MYSQL_PASSWORD: 'zzyy123'
    ports:
       - "3306:3306"
    volumes:
       - /app/mysql/db:/var/lib/mysql
       - /app/mysql/conf/my.cnf:/etc/my.cnf
       - /app/mysql/init:/docker-entrypoint-initdb.d
    networks:
      - atguigu_net
    command: --default-authentication-plugin=mysql_native_password   #解决外部无法访问问题
 
networks:  #自定义创建网络
   atguigu_net:   

```











## docker轻量级可视化工具Portainer

Portainer是一款轻量级的应用，它提供了图形可视化界面，用于方便地管理docker环境，包括单机环境和集群环境。

Portainer安装详细说明请参考:[Install Portainer CE with Docker on Linux - Portainer Documentation](https://docs.portainer.io/start/install-ce/server/docker/linux)









## docker容器监控之CAdvisor+InfluxDB+Granfana









































# Git教程

## 常用Git命令

| 命令                                     | 说明                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| **安装与配置**                           |                                                              |
| sudo apt-get install git                 | Ubuntu上安装Git命令                                          |
| git config --global user.name 用户名     | 设置用户签名（安装Git后务必设置）                            |
| git config --global user.email email地址 | 设置用户email地址（安装Git后务必设置）                       |
|                                          |                                                              |
| **获取与创建项目**                       |                                                              |
| git init                                 | 初始化本地库                                                 |
| git clone                                | 远程库地址	从远程库克隆到本地                             |
|                                          |                                                              |
| **基本快照**                             |                                                              |
| git status                               | 查看本地库状态                                               |
| git add 文件名                           | 添加变动文件到暂存区                                         |
| git add .                                | 添加当前目录下所有变动文件到暂存区                           |
| git restore --staged 文件名              | 复位在暂存区的文件（add反悔药）                              |
| git commit -m “备注文本” 文件名          | 提交暂存区文件到本地库（文件名缺省时，将暂存区所有文件提交） |
| git commit --amend                       | 修改上次提交的备注文本                                       |
| git revert 版本号(7位)                   | 撤销指定的提交（commit反悔药）(慎用)                         |
| git reset --hard 版本号(7位)             | 版本间穿梭（配合git reflog使用）                             |
| git reset --hard HEAD^                   | 穿梭到上一个版本                                             |
|                                          |                                                              |
| **分支与合并**                           |                                                              |
| **git branch**                           | 列出所有分支                                                 |
| **git branch 分支名**                    | 创建分支                                                     |
| **git checkout 分支名**                  | 切换分支                                                     |
| **git merge 分支名B**                    | 分支B合并到A（A为当前工作目录所处分支）                      |
| **git branch -d 分支名**                 | 删除分支                                                     |
| git tag                                  | 列出所有本地标签                                             |
| git tag -l 通配模式文本(*)               | 根据符合通配模式文本，列出所有本地标签                       |
| git tag 标签名                           | 为最新提交创建轻量标签                                       |
| git tag 标签名 版本号(7位)               | 为对应版本号提交创建轻量标签（在后期打标签）                 |
| git tag -a 标签名 -m 备注文本            | 为最新提交创建附注标签                                       |
| git tag -d 标签名                        | 删除指定标签                                                 |
|                                          |                                                              |
| **共享与更新项目**                       |                                                              |
| git remote add 别名 远程仓库地址         | 添加远程库                                                   |
| git remote -v                            | 查看添加过的远程库                                           |
| git push 远程库地址或其别名 分支名       | 推送到远程库                                                 |
| git push 远程库地址或其别名 --tags       | 推送所有标签到远程库                                         |
| git fetch                                | 将远程库的最新内容拉到本地                                   |
| git pull 远程库地址或其别名 分支名       | 将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并，<br/>相当于git fetch + git merge，这样可能会产生冲突，需要手动解决 |
|                                          |                                                              |
| **检查与比较**                           |                                                              |
| git show 标签名                          | 显示标签信息和与之对应的提交信息                             |
| git show 版本号(7位)                     | 显示对应版本对应的提交信息                                   |
| git log                                  | 显示当前分支所有提交过的版本信息                             |
| git log --follow 文件名                  | 显示当前分支所有提交过的关于指定文件版本信息                 |
| git log --pretty=oneline                 | 显示当前分支所有提交过的版本信息（精简）                     |
| git log --graph                          | 显示当前分支所有提交过的版本信息（附有分支合并图）           |
| git diff 分支一 分支二                   | 显示两分支差异                                               |
| git diff 版本号一(7位) 版本号二(7位)     | 显示同一分支两版本差异                                       |
|                                          |                                                              |
| **管理**                                 |                                                              |
| git reflog                               | 可以查看所有分支的所有操作记录<br/>（包括已被删除的commit记录和reset的操作，git log所不能） |





## Git

### Git 介绍 

Git是一个==免费的、开源的分布式版本控制系统==，可以快速高效处理从小型到大型的各种项目。



#### 版本控制

版本控制**是一种记录文件内容变化，以便将来查阅**特定版本修订情况的系统。

版本控制其实最重要的是**可以记录文件修改历史记录**，从而让用户能够查看历史版本，方便版本切换。



#### 为什么需要版本控制

通过版本控制，从个人开发过渡到团队协作。

![image-20230217004033875](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217004033875.png)

#### 版本控制工具



##### 集中式版本控制工具

常用：CVS、SVN、VSS ...

![image-20230217004926508](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217004926508.png)





##### 分布式版本控制工具

常用：Git、Mercurial、...

![image-20230217005326095](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217005326095.png)

优点：

+ 在本地进行版本控制，服务器宕机也可以进行开发
+ 每个客户端保存的也都是整个完整的项目（包含历史记录、更加安全）



#### Git工作机制

![image-20230217011701302](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217011701302.png)

#### 代码托管中心

代码托管中心是基于网络服务器的远程代码仓库，一般都简单称为==远程库==。

+ 局域网：GitLab
+ 互联网：Github（外网），Gitee码云（国内）





### Git 安装

官网地址：https://git-scm.com/

在[Git下载页面](https://git-scm.com/downloads)，选择下载Windows 64位版的Git安装软件。

安装步骤按照安装软件的安装向导安装即可，无需过多配置。

安装成功后，通常在文件浏览器空白处单击击鼠标右键，弹出菜单栏有Git的选项。



### Git命令

| 命令名称                             | 作用              |
| ------------------------------------ | ----------------- |
| git config --global user.name 用户名 | 设置用户签名      |
| git config --global user.email 邮箱  | 设置用户email地址 |
| git init                             | 初始化本地库      |
| git status                           | 查看本地库状态    |
| git add 文件名                       | 添加到暂存区      |
| git commit -m “日志信息” 文件名      | 提交到本地库      |
| git reflog                           | 查看历史记录      |
| git reset --hard 版本号              | 版本穿梭          |



#### 设置用户签名

`git config --global user.name 用户名 `    设置用户签名作用是区分不同操作者身份。用户的签名信息在每一个版本的提交信息中能查看到，以此确认本次提交是谁做的。然后，同样需要设置一下邮箱`git config --global user.email 邮箱`,==Git首次安装必须设置一下用户签名，否则无法提交代码==

**注意**：这里设置的用户签名和将来登录Github（或其他代码托管中心）的账号没有任何关系



![image-20230220111746148](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220111746148.png)





#### 初始化本地库

```go
//1,初始化本地库，初始化一个空的仓库，
//找一个地方用于管理本地项目，找到地方后，右键打开git bash here，最后 执行 git init
//Initialized empty Git repository in D:/code/gitWorkSpace/gitDemo/.git/
git init  
```



#### 查看本地库状态

```go
//查看本地库状态
git status
```

![image-20230217021854170](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217021854170.png)



当我在`gitDemo`下新建一个文件`hello.txt`后，再次执行`git status`命令

![image-20230217023138805](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217023138805.png)



#### 添加暂存区

将文件添加到暂存区，就是git追踪文件过程。

```go
//添加文件到暂存区
git add hello.txt   //即 git add 需要添加到暂存区的文件
```



![image-20230217024403140](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217024403140.png)





#### 提交本地库

将暂存区的文件 提交到本地库，形成历史版本。

```go
//提交到本地库
git commit -m "日志信息" 文件名
```



![image-20230217030017515](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217030017515.png)





#### 修改文件

修改文件后，需要再次提交文件。

![image-20230217134126429](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217134126429.png)







#### 历史版本

##### 查看历史版本

```go
git reflog //查看版本信息
git log  //查看详细的版本信息
```

![image-20230217134728925](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217134728925.png)





##### 版本穿梭

```go
//1,通过 git reflog 查看历史版本号
git reflog

//2，可以根据版本号，进行版本穿梭
git reset --hard 版本号  
```



![image-20230217141254364](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217141254364.png)





**==git切换版本，底层其实是移动HEAD指针：head指针指向master分支，master分支根据版本变换指向不同的版本，从head指针也指向了不同版本==**

![image-20230217142021994](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217142021994.png)





### Git分支

![image-20230217143148048](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217143148048.png)

#### 什么是分支

==在版本控制过程中，同时推进多个任务，可以为每个任务单独创建分支。使用分支意味着程序员可以把自己的工作从开发主线上分离出来，开发自己分支的时候，不会影响主线分支的运行。分支可以简单理解为副本，一个分支就是一个单独的副本。（其实，分支底层也是指针的引用）==

![image-20230217144420774](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217144420774.png)



#### 分支好处

+ 可以同时并行推进多个功能开发，提高开发效率
+ 各分支在开发过程中，如果某个分支失败，不会对其他分支有任何影响。失败分支删除后可以重新开始。





#### 分支操作

| 命令名称            | 作用                       |
| ------------------- | -------------------------- |
| git branch 分支名   | 创建分支                   |
| git branch -v       | 查看分支                   |
| git checkout 分支名 | 切换分支                   |
| git merge 分支名    | 把指定分支合并到当前分支上 |





##### 查看分支

```go
//查看当前有哪些分支
git branch -v
```

![image-20230217144722371](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217144722371.png)





##### 创建分支

```go
//创建分支
git branch 分支名
```

![image-20230217145222158](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217145222158.png)

##### 切换分支

```go
//切换分支
git checkout 分支名
```

![image-20230217145851947](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217145851947.png)



##### 修改分支

```go
//修改分支
修改分支上的文件内容 --> 添加到暂存区 --> 提交到本地库 
```

![image-20230217150612303](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217150612303.png)



##### 合并分支（正常合并）

**注意：不管是分支还是主干必须提交后，才能合并**

```go
//合并分支
git merge 分支名   //表示将指定的分支合并到当前分支上来
```

![image-20230217151900227](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217151900227.png)



**分支合并不会产生冲突情况：**

```go
我们从master分支上引出hot-fix分支，对hot-fix分支上的hello.txt文件进行修改并提交后，
切换到master分支上，master分支上，此时合并hot-fix分支会成功。

因为：在master分支上hello.txt 在引出分支之后一直没有被修改，所以合并时不会出现冲突
```





##### 冲突合并

```go
//冲突合并
在git中出现合并冲突时，需要手动进行合并，即对文件内容进行修改然后进行合并

当产生冲突合并后，需要人工合并
1，首先打开需要合并的文件 hello.txt
2，将需要合并的内容保留，不需要合并的内容删除，同时要将 <<<head === >>>hot-fix 这三行删除
3，将修改后的hello.txt 添加到暂存区
4，将hello.txt 提交到本地库，即合并完成

```

**注意：提交时，不要带hello.txt文件名，否则合并失败，原因就是如果指定文件名，无法确定是哪个分支上的**==



**分支合并产生冲突情况：**

```go
我们从master分支上引出hot-fix分支之后，我们对master分支上的文件hello.txt 文件进行了修改并提交，然后切换到hot-fix分支
我们对hot-fix分支上的文件hello.txt 文件进行了修改并提交。
然后，我们切换到master，将hot-fix分支合并到master分支，此时会产生冲突。


因为：master分支和hot-fix分支都修改了 {引出hot-fix时刻的hello.txt文件内容} ，此时合并就会产生冲突。
```



![image-20230217161940415](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217161940415.png)





#### 创建分支和切换分支图解

master、 hot-fix 其实都是指向具体版本记录的指针。当前所在的分支，其实是由 HEAD决定的。所以创建分支的本质就是多创建一个指针。

HEAD 如果指向 master，那么我们现在就在 master 分支上。
HEAD 如果执行 hot-fix，那么我们现在就在 hot-fix 分支上。
所以切换分支的本质就是移动 HEAD 指针。



### Git团队协作机制

#### 团队内协作

![image-20230217185055231](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217185055231.png)





#### 跨团队协作

![image-20230217185707679](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217185707679.png)







### Goland集成Git

#### 配置Git忽略文件

![image-20230220104040392](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220104040392.png)



其中，有些文件如`.idea文件夹里面的文件`以及如图`.iml`文件与实际项目无关，不参与服务器上的部署运行，把它们忽略掉可以屏蔽IDE工具版本之间的差异。



**通过创建忽略规则文件`xxx.ignore`（前缀名随便起，建议是`git.ingore`），这个文件的存放位置原则上随便，为了便于让~/.gitconfig文件引用，建议放在用户家目录下**



**git.ignore文件模板：**

```shell
# Compiled class file
*.class  # 表示所有的.class 文件都忽略

# Log file
*.log  # 表示所有的.log文件都忽略

# BlueJ files
*.ctx

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip.gz
*.rar

# virtual machine crash logs,see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
.classpath
.project
.settings
target
.idea
*.iml

```



**在`.gitconfig`文件中引用忽略配置文件（此文件在windows的家目录下）**

```go
[user]
	name = kk
	email = 18390241528@163.com
[core]
	excludesfile= C:/Users/18390/git.ignore
```

==**注意：这里使用“正斜线(/)”，而不要使用“反斜线(\\)”**==





#### 定位Git程序

![image-20230220111119510](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220111119510.png)







#### 初始化、添加、提交

**初始化**:

![image-20230220134917528](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220134917528.png)

**添加**:

如果添加单个文件，则可以点击点个文件，右键git，然后 add，就添加了，此时文件会变为绿色。

![image-20230220135106648](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220135106648.png)



如果想要添加项目下的所有文件，可以直接点击项目根目录右键，然后git，最后add



**提交:**

![image-20230220135331079](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220135331079.png)

![image-20230220135428143](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220135428143.png)





**修改后提交：**

![image-20230220140955584](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220140955584.png)





#### 切换版本



![image-20230220141811897](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220141811897.png)

```go
在goland中，点击左下角的git -->  可以看到如上四个方框，其中第二个方框中有几次提交的记录，第四个方框中有提交的日志信息，版本信息等。
在第二个方框中，点击提交的记录，右键可以切换版本 checkout revision  实现版本穿梭
```







#### 创建分支、切换分支

```go
//创建分支
选择git，点击new branch按钮，然后填写分支名称，创建hot-fix分支

//切换分支
在下图中，第一个方框的local中，点击分支右键，然后checkout，就是实现了分支的切换，右下角可以看到当前处于的分支，或者通过第一个方框中的Head指针来查看当前处于的分支
```

![image-20230220143007876](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220143007876.png)









#### 合并分支

##### 正常合并

```go
//正常合并分支
在goland窗口右下角，将hot-fix分支合并到当前master分支，或者通过如下图的第一个方框中的hot-fix分支右键合并到主分支
如果代码没有冲突，分支直接合并成功，分支合并成功后，代码自动提交，无需手动提交到本地
```

![image-20230220151620240](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220151620240.png)



##### 冲突合并

```go
//合并分支
如果说，两个分支上的同一个文件内容都出现了修改并提交，可以点击下图的第一个方框中的hot-fix右键选择将hot-fix分支合并到master分支
或者点击右下角master右键选择将hot-fix分支合并到master分支。
由于两个分支都出现了修改，此时合并会出现分支合并冲突，需要手动进行合并，合并完之后会自动进行提交，最后点击apply即可。
```

![image-20230220152850275](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220152850275.png)



![image-20230220153517200](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220153517200.png)









## Github

Github官网：https://github.com



### 创建远程库

![image-20230217195029971](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217195029971.png)



#### 创建远程仓库别名

```go
//查看当前所有远程地址别名
git remote -v 

//为远程仓库创建别名  为什么要创建别名：那是因为远程仓库地址太长，难以记住，用别名方便记忆，也可以不用别名，直接用仓库地址
//例如，仓库https地址：https://github.com/huihui97/git-demo.git
git remote add 别名 远程地址  //别名最好与库名一致，便于记忆
```



![image-20230217195916341](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217195916341.png)



### 代码推送Push

```go
//将本地库代码推送到远程库
//注意：推送最小单位是分支，因此推送的是本地库的哪个分支
git push 别名(远程库地址) 分支  
```

![image-20230217200759935](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217200759935.png)





### 代码拉取Pull

```go
//从远程库中拉取代码到本地库，拉取后自动合并提交到本地库
//注意：最小单位也还是分支，拉取远程库哪个分支
git pull 别名(远程库地址) 分支
```

![image-20230217201725154](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217201725154.png)



**如果出现SSL报错：**

```go
//使git忽略ssl证书验证
git config --global http.sslVerify "false"  
```

**如果出现`Failed to connect to github.com port 443 after 21098 ms:Timed out`：**

```go
//出现该问题往往是由于网络慢、访问超时，这时候我们可以在终端选择使用设置代理和取消代理的命令

//1,先设置代理
git config --global https.proxy
//2，再取消代理,即可解决问题
git config --global --unset https.proxy


解决步骤：我们直接在终端先输入设置代理的命令，再输入取消代理的命令即可解决
```







### 代码克隆Clone

```go
//从远程库地址克隆，由于该克隆仓库是公共仓库，是不需要权限的，因此不需要登录账号直接克隆
git clone 远程库地址

```

==注意：clone会执行以下三个操作：1，拉取代码，2，初始化本地库，3，创建别名==



![image-20230217202958838](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217202958838.png)





### 团队内协作

```go
//1,由于远程库是公开的，那么任何人都是可以直接克隆项目到本地库

//2，如果，远程库是私秘库，那么需要团队内的成员才能克隆该项目

//3，如何成为团队成员，首先需要项目方邀请你入队，会生成一个邀请函，他将邀请函发送给你，你通过网页打开这邀请函，并点击接受即可。

//4，同样，当你将克隆下文件内容修改，你想要推送到项目远程库中，此时只有是团队内成员才能推送入库。

//5，推送入库后，项目就又可以从远程库上拉取最新文件内容（即你刚才推送入库修改的内容）

//6，从而实现了团队内协作开发
```



![image-20230217211514529](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217211514529.png)

**团队内协作：**

+ 拉取：如果库是公开库，任何人都可以拉取该库，如果是私有库，只有团队内成员才能拉取该库
+ 推送：如果团队内成员，可以直接将修改的内容推送到该项目库中，如果是团队外，则需要被邀请入对才能推送





### 跨团队协作

![image-20230217221116529](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217221116529.png)



```go
1,团队外成员，发现你项目，fork一份到自己的远程库
2，他修改后，通知你进行更新，
3，首先 他要创建pull request 给你
4，你收到他的pull request 后
5，检查他修改的内容
6，发现没有问题，点击合并
7，合并完成后，你发现你的文件内容已经修改完成了
```





### SSH免密登录

我们看到远程库中还有一个SSH地址，因此我们也可以使用SSH地址进行访问

![image-20230217222250906](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217222250906.png)



```go
abc@DESKTOP-R85C9HV MINGW64 ~
$ cd ~

abc@DESKTOP-R85C9HV MINGW64 ~
$ pwd
/c/Users/abc

abc@DESKTOP-R85C9HV MINGW64 ~
$ ls -a .ssh
./  ../  id_rsa  id_rsa.pub

abc@DESKTOP-R85C9HV MINGW64 ~
$ rm -rf .ssh

abc@DESKTOP-R85C9HV MINGW64 ~
$ ls -a .ssh
ls: cannot access '.ssh': No such file or directory

```

运行命令ssh-keygen生成.ssh目录：

```go
abc@DESKTOP-R85C9HV MINGW64 ~/Desktop/HelloGit-clone/HelloGit (master)
$ ssh-keygen -t rsa -C abc@123.com
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/abc/.ssh/id_rsa):
Created directory '/c/Users/abc/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/abc/.ssh/id_rsa
Your public key has been saved in /c/Users/abc/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:aeNMB/hP2yiH/Dka2jK9BJciSgA8yKKLlKXX8oei7J0 jallenkwong@163.com
The key's randomart image is:
+---[RSA 3072]----+
|=                |
|++ .   .         |
|+ = . . .        |
|.= o . . +       |
|o.o + + S o      |
|o. o + @ * +     |
|. o . ..O = .    |
| o. . o+.=..     |
|.. E  .o+oo.     |
+----[SHA256]-----+

abc@DESKTOP-R85C9HV MINGW64 ~
$ ls -a .ssh
./  ../  id_rsa  id_rsa.pub

# 生成公钥
abc@DESKTOP-R85C9HV MINGW64 ~
$ cat .ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQChXy8I20br9nu4GCNeZSDkozfHvlRFpXiImYnVlHVvyvFgjct1/zMeJgot1J6+yArSJbA4TMlS9nG8owCE6C9yqhPceDlKtQbARKS2pW7IyP5OhIbcqVmWmvvd+IMmsWrWgK9S6jqp0xSqv3Z3mlcHWOAK18oOe6wF6b3SyGgCP/EcwwUGX4NG7jukhK+In9joSuAxchEg/Ba2/LVjqtfBn3hXZx/SEt+rJ0UVPIT/eEe32HflrzokNcO7l0IgyLntv5QEAsSC2hiGxrM6vF5tQpb12MVZnt1/01ytP0ruQn2TVTI96vsOAa3Cj98dAH2Z0JdqZUSVBw+o3AqXP5oeF1JWkDHZzHQjLgu741wnUZn+vVXFBu1xQyApbvH7y7cNbq8PaxU+SyZbVXbq3RwTywJsyFQvsIOM5l0tG7jUD0QAd6dP3rcNODjFTaafJaBsR9aMwvKQd/d7H+BdwFPYOFp8HB2JAzhRpvlS4Av9MCIe0474wZ0T2QOJmcs7mns= abc@123.com
```

然后，将生成的公钥添加至Github账号SSH设置

![image-20230217222439110](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217222439110.png)



![image-20230217222453623](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217222453623.png)



![image-20230217222503047](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217222503047.png)



![image-20230217222514420](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217222514420.png)

```go
abc@DESKTOP-R85C9HV MINGW64 ~/Desktop/HelloGit-clone/HelloGit (master)
$ cat hello.txt
hello, git!hot-fix
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!modify from Github editor
hello, git!hot-fix test
hello, git!master test

abc@DESKTOP-R85C9HV MINGW64 ~/Desktop/HelloGit-clone/HelloGit (master)
$ vim hello.txt

abc@DESKTOP-R85C9HV MINGW64 ~/Desktop/HelloGit-clone/HelloGit (master)
$ cat hello.txt
hello, git!hot-fix
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!ssh test
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!
hello, git!modify from Github editor
hello, git!hot-fix test
hello, git!master test

abc@DESKTOP-R85C9HV MINGW64 ~/Desktop/HelloGit-clone/HelloGit (master)
$ git add .

abc@DESKTOP-R85C9HV MINGW64 ~/Desktop/HelloGit-clone/HelloGit (master)
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   hello.txt

abc@DESKTOP-R85C9HV MINGW64 ~/Desktop/HelloGit-clone/HelloGit (master)
$ git commit -m "ssh test"
[master 9602a37] ssh test
 1 file changed, 1 insertion(+), 1 deletion(-)

# 通过SSH推送
abc@DESKTOP-R85C9HV MINGW64 ~/Desktop/HelloGit-clone/HelloGit (master)
$ git push git@github.com:abc/HelloGit.git master
The authenticity of host 'github.com (13.250.177.223)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,13.250.177.223' (RSA) to the list of known hosts.
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 283 bytes | 283.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:JallenKwong/HelloGit.git
   47e257f..9602a37  master -> master
```

![image-20230217222546895](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230217222546895.png)









### Goland集成Github

#### 设置Github账号

![image-20230220154930470](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220154930470.png)



#### 分享项目到Github

```go
//分享项目到Github

在goland中，可以直接点击vcs，点击github，点击share project on Github,就可以将项目分享到Github，在Github上的远程库名即为项目名
```



![image-20230220161716961](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220161716961.png)



#### 推送代码到远程库

```go
//推送代码到远程库
代码出现修改后，必须要先提交到本地库中，然后才能推送到github上
```

==**注意**==：push是将本地代码推送到远程库，如果本地库代码跟远程库代码版本不一致，push操作是会被拒绝的。即，想要push成功，一定要保证本地代码的版本要比远程库的版本高！==因此，一个成熟的程序员在动手修改本地库代码之前，一定会先检查远程库跟本地库代码的区别！**如果本地的代码版本已经落后了，要先pull拉取一下远程库的代码，将本地代码更新到最新版本，然后再修改、提交、推送！**==



![](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220163205478.png)









#### 拉取远程库代码

```go
//拉取远程库代码到本地

右键点击项目，可以将远程库的内容pull到本地库
```

==**注意：pull是拉取远程库代码到本地库，如果远程库代码和本地库代码不一致，会自动合共，如果自动合并失败，还会涉及手动解决冲突的问题。**==



![image-20230220165757372](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220165757372.png)



#### 克隆代码到本地

```go
//从远程库中克隆代码到本地

如果本地没有该项目代码，可以直接打开goland，点击Get from VCS
第一种方式：直接利用git，从远程库中克隆
第二种：针对已经登录Github账号了，从该账号中克隆代码
```







![image-20230220182618291](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220182618291.png)







## Gitee码云

码云是开源中国推出的基于Git的代码托管中心，网址为：https://gitee.com/ ，使用方式跟Github一样，而且是一个中文网站。



### Goland集成Gitee码云

```go
1，首先去插件中心安装gitee
2，第一种方式：可以跟github一样，不用创建远程库，直接将项目分享到gitee上
3，第二种方式：自己去码云上创建一个远程库，将本地库代码推送到远程库
```

**第一种方式**

![image-20230220192850229](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220192850229.png)

**第二种方式push**

![image-20230220195631275](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220195631275.png)



==**注意：创建库，分享、推送、拉取、克隆等操作，都和github一样，没有什么区别。**==







### 码云连接Github进行代码的复制和迁移

**码云提供了直接复制Github项目的功能，方便我们做项目的迁移和下载。**

![image-20230220200742406](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220200742406.png)



==**注意：码云从github上复制项目后，如果github上项目出现了更新，此时可以在码云进行强制更新来保证内容的一致性。**==



![image-20230220201524353](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220201524353.png)







## Gitlab

### 简介和安装环境准备

#### 简介

Gitlab是由GitLabInc.开发的，使用MIT许可证的基于网络的Git仓库管理工具，并且具有wiki和issue跟踪功能。使用Git作为代码管理工具，并在此基础上搭建起来的web服务。

GitLab是由乌克兰程序员DmitriyZaporazhets和ValerySizov开发，它使用Ruby语言编写成。后来，一些部分用Go语言重写。截止2018年5月，该公司约290名团队成员，以及2000名开源贡献者。GitLab被IBM，Sony，NASA，Alibaba，CERN，SpaceX等组织使用。



#### 官网地址

官网地址：https://about.gitlab.com

安装说明: https://about.gitlab.com/installation/



### 安装、初始化、启动服务

#### Gitlab 安装准备

+ 准备一个系统为 CentOS7 以上版本的服务器， 要求内存 4G，磁盘 50G。
+ 关闭防火墙， 并且配置好主机名和 IP，保证服务器可以上网。
+ 此教程使用虚拟机：主机名： gitlab-server IP 地址： 192.168.6.200
+ Yum 在线安装 gitlab- ce 时，需要下载几百 M 的安装文件，非常耗时，所以最好提前把所需 RPM 包下载到本地，然后使用离线 rpm 的方式安装。下载地址。注：资料里提供了此 rpm 包，直接将此包上传到服务器/opt/module 目录下即可。



**gitlab可以在哪些系统上安装：**

![image-20230220204023462](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220204023462.png)







#### 编写安装脚本

安装 gitlab 步骤比较繁琐，因此我们可以参考[官网编写 gitlab 的安装脚本](https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh)。

```shell
[root@gitlab-server module]# vim gitlab-install.sh
sudo rpm -ivh /opt/module/gitlab-ce-13.10.2-ce.0.el7.x86_64.rpm

sudo yum install -y curl policycoreutils-python openssh-server cronie

sudo lokkit -s http -s ssh

sudo yum install -y postfix

sudo service postfix start

sudo chkconfig postfix on

curl https://packages.gitlab.com/install/repositories/gitlab/gitlabce/script.rpm.sh | sudo bash

sudo EXTERNAL_URL="http://gitlab.example.com" yum -y install gitlabce

```

给脚本增加执行权限

```shell
[root@gitlab-server module]# chmod +x gitlab-install.sh
[root@gitlab-server module]# ll
总用量 403104
-rw-r--r--. 1 root root 412774002 4 月 7 15:47 gitlab-ce-13.10.2-
ce.0.el7.x86_64.rpm
-rwxr-xr-x. 1 root root 416 4 月 7 15:49 gitlab-install.sh

```

然后执行该脚本，开始安装 gitlab-ce。注意一定要保证服务器可以上网。

```shell
[root@gitlab-server module]# ./gitlab-install.sh
警告： /opt/module/gitlab-ce-13.10.2-ce.0.el7.x86_64.rpm: 头 V4
RSA/SHA1 Signature, 密钥 ID f27eab47: NOKEY
准备中... #################################
[100%]
正在升级/安装...
1:gitlab-ce-13.10.2-ce.0.el7
################################# [100%]
。 。 。 。 。 。

```





#### 初始化Gitlab服务

执行以下命令初始化 GitLab 服务，过程大概需要几分钟，耐心等待…

```shell
[root@gitlab-server module]# gitlab-ctl reconfigure
。 。 。 。 。 。
Running handlers:
Running handlers complete
Chef Client finished, 425/608 resources updated in 03 minutes 08
seconds
gitlab Reconfigured!

```





#### 启动Gitlab服务

执行以下命令启动 GitLab 服务，如需停止，执行 gitlab-ctl stop

```shell
[root@gitlab-server module]# gitlab-ctl start
ok: run: alertmanager: (pid 6812) 134s
ok: run: gitaly: (pid 6740) 135s
ok: run: gitlab-monitor: (pid 6765) 135s
ok: run: gitlab-workhorse: (pid 6722) 136s
ok: run: logrotate: (pid 5994) 197s
ok: run: nginx: (pid 5930) 203s
ok: run: node-exporter: (pid 6234) 185s
ok: run: postgres-exporter: (pid 6834) 133s
ok: run: postgresql: (pid 5456) 257s
ok: run: prometheus: (pid 6777) 134s
ok: run: redis: (pid 5327) 263s
ok: run: redis-exporter: (pid 6391) 173s
ok: run: sidekiq: (pid 5797) 215s
ok: run: unicorn: (pid 5728) 221s

```





### 登录Gitlab并创建远程库

#### 登录Gitlab

使用主机名或者 IP 地址即可访问 GitLab 服务。可配一下 windows 的 hosts 文件（C:\Windows\System32\drivers\etc）。

![image-20230220203032093](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220203032093.png)

![image-20230220203022930](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220203022930.png)

首次登陆之前，需要修改下 GitLab 提供的 root 账户的密码，要求 8 位以上，包含大小写子母和特殊符号。

然后使用修改后的密码登录 GitLab。

![image-20230220203056293](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220203056293.png)

GitLab 登录成功。


![image-20230220203111068](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220203111068.png)







#### 创建远程库



![image-20230220203134871](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220203134871.png)

![image-20230220203155777](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220203155777.png)

![image-20230220203208112](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230220203208112.png)









### Goland集成Gitlab

![img](https://img-blog.csdnimg.cn/img_convert/1f34175126922c56c158f466dd4d665c.png)

接下来插件配置，Git操作等与Github、Gitee的IDEA插件大同小异，不再赘述，自己触类旁通吧！























# HTML+CSS+JS

推荐使用工具：vscode+google+open in browser



vscode常用快捷键：

```go
代码格式化：shift+alt+f

向上/向下移动一行： alt+up/alt+down

快速复制一行代码： shift+alt+up/shift+alt+down

快速保存：ctrl+s

快速查找：ctrl+f

快速替换：ctrl+h
```









## HTML 

vscode中快速生成`.html`文件框架: `!+回车`

### HTML5简介

HTML5是用来描述网页的一种语言，称为超文本标记语言。标记语言是一套标记标签。标签是由尖括号包围的关键字。

**标签两种形式**：

+ 双标签，例如：`<html></html>`
+ 单标签，例如：`<img>`



**DOCTYPE声明**：

​	**DOCTYPE是`document type` (文档类型)的缩写**。`<!DOCTYPE html>`是H5的声明，位于文档的最前面，处于标签之前。是网页必备的组成部分，避免浏览器的怪异模式。

```html
<!DOCTYPE html>
```



**HTML5基本骨架：**

```html
<html lang="en">
    <head>
        
    </head>
    
    <body>
        
    </body>
</html>
```



**html标签:**

​	定义html文档，**标签限定了文档的开始点和结束点。**

```html
<!DOCTYPE html>
<html> <!--文档开始-->
    
</html> <!--文档结束-->
```



**head标签：**

head标签用于定义文档的头部。**文档的头部描述了文档的各种属性信息**，包括文档的标题、在web中的位置以及和其他文档的关系等。绝大多数文档头部包含的数据都不会真正作为内容显示给读者。

```html
<!DOCTYPE html>
<html>
    <head>
        
    </head>
</html>
```



**body标签：**

body元素定义文档的主体。**body元素包含文档的所有内容（**比如：文本、超链接、图像、表格和列表等）。它会直接在页面显示出来，用户可以直观看到的内容。

```html
<!DOCTYPE html>
<html>
    <head>
        
    </head>
    
    <body>
        body中的内容会显示在网页上
    </body>
</html>
```



**title标签：**

+ **定义文档的标题**
+ 显示在浏览器窗口的标题栏或状态栏上
+ **`title`标签是`<head>`标签中唯一必须要求包含的东西**，即写head必须写title。
+ `title0`的增加有利于SEO优化

> SEO是搜索引擎优化的英文缩写。通过对网站内容调整，满足搜索引擎的排名要求

```html
<!DOCTYPE html>
<html>
    <head>
        <title>第一个页面</title>
    </head>
    <body>
        body内容会显示在网页上
    </body>
</html>
```



**meta 标签：**

​	**meta标签用来描述一个html网页文档的属性，关键词等**。例如：`charset="utf-8"`是说当前使用的是`utf-8`编码格式，在开发中经常会看到`utf-8`，或是`gbk`，这些都是编码格式。通常使用`utf-8`编码格式。

```html
<!DOCTYPE html>
<html>
    <head>
        <!--meta标签是单标签-->
        <meta charset="utf-8">
        <title>标题</title>
    </head>
    <body>
        
    </body>
</html>
```



### 标签之标题

标题（Heading）是通过`<h1>...<h6>`标签来定义的。`<h1>最大的标题</h1>` 、`<h6>最小的标题</h6>`

```html
<h1>一级标题</h1>

<h2>二级标题</h2>

<h3>三级标题</h3>

<h4>四级标题</h4>

<h5>五级标题</h5>

<h6>六级标题</h6>
```



**应该正确使用标题：**

+ 确保HTML标题标签只用于标题。
+ 不要为了生成粗体或者大号的文本而使用标题
+ 正确使用标题有益于SEO
+ 应该将`<h1>`用作主标题，其次是`<h2>`，再其次`<h3>`，以此类推。



**标题标签位置摆放：**

​	在标签中添加属性：`align="left|center|right"` ，默认居左。

```html
<!DOCTYPE html>
<html>
    <head>
        <title>标题</title>
    </head>
    
    <body>
        <!--在标签的第一个<>里面添加东西就是属性，其中"center"是属性值-->
        <h1 align="center">一级标题</h1>
    </body>
</html>
```



### 标签之段落、换行、水平线

#### 标签之段落

段落是通过`<p>`标签定义的

```html
<p>这是一个段落</p>

<p>这是另一个段落</p>
```



#### 标签之换行

在不产生一个新的段落的情况下进行换行（新行），可以使用`<br>`，单标签。

`<br>`元素是一个空的HTML元素。

```html
<p>
    这个<br>段落<br>演示了分行的效果
</p>
```



#### 标签之水平线

`<hr>` 标签在HTML页面中创建水平线。

```html
<hr color= "" width= "" size="" align=""/>
```

**属性：**

+ color ：设置水平线的颜色
+ width ：设置水平线的长度
+ size ：设置水平线的高度
+ align ： 设置水平线的对齐方式（默认居中），可以取值 left、center、right





### 标签之图片

`<img>`标签定义HTML页面中的图像，是单标签。

```html
<img src="" alt="" title="" width="" height="">
```

**属性：**

+ src : 路径
+ alt : 规定图像的替代文本
+ width ：规定图像的宽度
+ height ：规定图像的高度
+ title：鼠标悬停在图片上给予的提示





### 图片路径详解

**绝对路径**：电脑的盘符存储与访问的具体地址

```html
<img src="E:\study\1.webp">
```



**相对路径**：两者相对关系，两者在同一路径下可以直接访问

+ 子级关系：`/`
+ 父级关系：`../`
+ 同级关系：`./`



**网络路径**：具体的网络地址：http://iwenwiki.com/api/newworld/images/n1.png





### 标签之超文本链接

HTML使用标签`<a>`来设置超文本链接

**链接文本可以是一个字、一个词、或者一组词，也可以是一幅图像，**可以点击这些内容来挑战到新的文档或者文档中的某个部分。

```html
<a href="url">链接文本</a>
```

**属性：**

​	在标签`<a>`中使用`href`属性来描述链接的地址。默认情况下，链接将以下面两种方式显示在浏览器上：

+ 一个未访问过的链接显示为蓝色字体并带有下划线
+ 访问过的链接显示为紫色并带有下划线。
+ 点击链接时，链接显示为红色并带有下划线

> 后期可以通过CSS样式修改掉这些效果



### 标签之文本

常用的文本标签：

+ `<em>`：定义着重文字
+ `<b>`：定义粗体文字
+ `<i>`：定义斜体字
+ `<strong>`：定义加重语气
+ `<del>`：定义删除字
+ `<span>`：元素没有特定的含义

> 常用文本标签和段落都是不同的，段落代表一段文字，而文本标签一般表示文本词汇

```html
<em>白菜</em>
<b>白菜</b>
<i>白菜</i>
<strong>白菜</strong>
<del>白菜</del>
<span>白菜</span>
```

> **文本标签可以嵌套到`<p> </p>`标签中**



### 列表标签之有序列表

**有序列表是一列项目**，列表项目使用数字进行标记。**有序列表始于`ol`标签**。**每个列表项始于`li`标签**。

```html
<ol>
    <li>你好</li>
    <li>世界</li>
</ol>
```

**`<ol>`的属性type拥有的选项：**

+ 1 ，表示列表项目用数字标号（1，2，3...)
+ a ，表示列表项目用小写字母标号（a，b，c..）
+ A ，表示列表项目用大写字母标号（A，B，C...）
+ i ，表示列表项目用小写罗马数字标号（i，ii，iii...）
+ I ，表示列表项目用大写罗马数字标号（I，II，III...）

> **有序列表是支持有序列表嵌套的**





### 列表标签之无序列表

**无序列表是一个项目列表**，此列项目使用粗体圆点（典型的小黑圆圈）进行标记

无序列表始于`<ul>`标签。每个列表项始于`<li>`标签。

```html
<ul>
    <li>香蕉</li>
    <li>苹果</li>
    <li>草莓</li>
</ul>
```

**`<ul>`的属性type拥有的选项**：

+ disc ：默认实心圆
+ circle ：空心圆
+ square ：小方块
+ none ：不显示

> 无序列表也是支持嵌套的，**有序列表和无序列表支持互相嵌套。**



无序列表常见的应用场景：

+ 无序的列表效果
+ 导航效果

> 快速生成ul+li的布局：ul>li*3（数字根据自己的需要的li数量进行修改）





### 标签之表格

**表格组成与特点**：行、列、单元格

​	单元格特点：同行等高、同列等宽



**表格标签：**

+ 表格：`<table>`
+ 行：`<tr>`
+ 单元格（列）：`<td>`

```html
<!-- border，width，height 后面都是跟像素-->
<table border="1px" width="400px" height="200px">
    <tr>
        <td>猪肉</td>
        <td>牛肉</td>
    </tr>
    <tr>
        <td>香蕉</td>
        <td>苹果</td>
    </tr>
</table>
```

> 快捷键：快速生成表格结构：table>tr*2>td\*3 {文本信息}

**`<table>`的表格属性：**

+ border：设置表格的边框
+ width：设置表格的宽度
+ height：设置表格的高度





### 表格单元格合并

**单元格合并，`<td>`属性：**

+ 水平合并：colspan
+ 垂直合并：rowspan

```html
    <table border="1px" width="300px" height="100px">
        <tr>
            <!--水平合并单元格1和单元格2-->
            <!--colspan="2",表示水平合并两个单元格-->
            <td colspan="2">单元格1</td>
           <!-- <td>单元格2</td> 既然合并，还需要将合并的单元格删除-->
            
            <!--垂直合并3个单元格，rowspan表示垂直合并，3表示合并三个单元格-->
            <td rowspan="3">单元格3</td>
        </tr>
        <tr>
            <td>单元格4</td>
            <td>单元格5</td>
            <!--  <td>单元格6</td>  垂直合并删除下边单元格 -->
        </tr>
        <tr>
            
            <td>单元格7</td>
            <td>单元格8</td>
            <!--   <td>单元格9</td>  垂直合并删除下边单元格 -->
        </tr>
    </table>
```

==**注意：水平合并：保留左边单元格，删除右边单元格；垂直合并：保留上边单元格，删除下边单元格；既水平合并又垂直合并，则以左上为基准，先左右合并再上下合并**==





### Form表单

**表单在web网页中用来给用户填写信息，从而能采集用户信息，使网页具有交互的功能**。所有的用户输入内容的地方都是用表单来写的，如登录注册、搜索框等。

表单是由容器和控件组成的，一个表单一般应该包含用户填写信息的输入框，提交按钮等，这些输入框按钮叫做控件，表单就是容器，它能够容纳各种各样的控件。

```html
<form action="url" method="get/post" name="myform">
    
</form>
```



**form表单属性：**

+ action：服务器地址
+ name：表单名称
+ method：数据提交方式
  + get提交方式：get把提交的数据url可以看到，一般用于提交少量数据。
  + post提交方式：post提交数据无法看到，一般用来提交大量数据。



**表单元素：**

一个完整的表单包含三个基本组成部分：表单标签、表单域、表单按钮

+ 表单标签：
+ 表单域：
+ 表单按钮：

```html
<form>
    <input type="text">
    <input type="submit">
</form>
```





### 表单元素

**文本框：**

 	文本域通过`<input type="text">`标签俩设定，当用户要在表单中键入字母、数字等内容时，就会用到文本域。

```html
<form>
    First name: <input type="text" name="firstname">
    <br>
    Last name: <input type="text" name="lastname">
</form>
```



**密码框：**

​	密码字段通过标签`<input type="password">`来**定义**

```html
<form>
    Password: <input type="password" name="pwd">
</form>
```

> **密码字段字符不会明文显示，而是以星号或圆点表示**



**提交按钮：**

​	当用户点击提交按钮时，表单内容就会被传送到另一个文件。表单的动作属性定义了目的文件的文件名。由动作属性定义的这个文件通常会对接收到的输入数据进行相关的处理。

```html
<form name="input" action="url" method="get/post">
    Username: <input type="text" name="user">
    <!-- 通过value来定义提交按钮的名字，下面就是将默认的提交改成登录 -->
    <input type="submit" value="登录">
</form>
```





### 块元素与行内元素（内联元素）

**内联元素和块元素的区别：**

| 块级元素                                     | 内联元素                                     |
| -------------------------------------------- | -------------------------------------------- |
| 块元素会在页面中独占一行（自上向下垂直排列） | 行内元素不会独占页面中的一行，只占自身的大小 |
| 可以设置width，height属性                    | 行内元素设置width，height属性无效            |
| 一般块级元素可以包含行内元素和其他块级元素   | 一般内联元素包含内联元素不包含块级元素       |

**常见块级元素：**

```html
div  form  h1~h6  hr  p  table  ul  ol 等
```

**常见内联元素：**

```html
a  b  em  i  span  strong等
```

**行内块级元素：（特点：不换行、能识别宽高）**

```html
button  img  input
```





### HTML5新增标签

HTML5是HTML最新的修订版本，在HTML5出现前，我们采用`DIV+CSS`布局我们的页面。但是这样的布局方式不仅使我们的文档结构不够清晰，而且不利于搜索引擎爬虫对我们页面的爬取。为了解决上述缺点，HTML5新增了很多新的语义化标签。



 **`<div>`标签：**

`<div>`容器元素，将整个页面划分一块一块，我们可以在块内进行设置其他标签，便于维护。

```html
<div>
    <p>
        段落
    </p>
    <ul>
        <li>列表1</li>
        <li>列表2</li>
    </ul>
</div>
```



**HTML5新标签：**

+ `<header></header>`：头部
+ `<nav></nav>`：导航
+ `<section></section>`：定义文档中的节，比如章节、页眉、页脚
+ `<aside></aside>`：侧边栏
+ `<footer></footer>`：脚部
+ `<article></article>`：代表一个独立的、完整的相关内容块，例如一篇完整的论坛帖子，一篇博客文章。







## CSS

### CSS简介

CSS（Cascading Style Sheets）层叠样式表，又叫级联样式表，简称样式表。

CSS文件后缀名：`.css`

CSS用于HTML文档中元素样式的定义



**CSS作用：** 使用CSS的目的就是让网页具有美观一致的页面



**CSS语法：**

​	css规则由两个主要的部分构成：**选择器，以及一条或多条声明（样式）。**

​	![image-20230318201925048](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230318201925048.png)









### CSS的引入方式

#### 内联样式（行内样式）

使用内联样式，需要在标签内使用样式（style）属性。style属性可以包含任何css属性。

```html
<p style="background:orange;font-size:20px;">
    CSS
</p>
```

> 注意：内联样式缺乏整体性和规划性，不利于维护，维护成本高



#### 内部样式

当单个文档需要特殊的样式时，应该使用内部样式表。可以使用`<style>`标签在文档头部定义内部样式表。

```html
<!DOCTYPE html>
<html>
    <head>
        <!--内部样式，可以对多个标签定义样式-->
        <style>
            p{
                color:red;
                font-size:20px;
            }
        </style>
    </head>
    <body>
        
    </body>
</html>
```

> 注意：单个页面内的CSS代码具有统一性和规划性，便于维护，但是在多个页面之间容易混乱



#### 外部样式（推荐）

当样式需要应用很多页面时，外部样式表是理想的选择。在使用外部样式表的情况下，可以通过改变一个文件来改变整个站点的外观。每个页面使用`<link>`标签链接到样式表。`<link>`标签在（文档的）头部。

```html
<!DOCTYPE html>
<html>
    <head>
       <linK ref="stylesheet" type="text/css" href="xxx.css">
    </head>
    <body>
        
    </body>
</html>
```

```css
/* 文件名：xxx.css */
p{
    color:red;
    font-size:20px;
}
```





### 选择器

#### 全局选择器

可以与任何元素（html标签）匹配，**优先级最低**，一般做样式初始化

```css
*{
    color:red;
    font-size:30px;
}
```



#### 元素选择器

HTML文档中的元素，`p、b、div、a、img、body`等。

标签选择器，选择的是页面上所有这种类型的标签，所以经常描述“共性”，无法描述某一个元素的“个性”。

```css
p{
    font-size:20px;
}
```

> 1，所有的标签，都可以是选择器。比如：ul、li、label、dt、dl、input、div等
>
> 2，无论这个标签藏的多深，一定能够被选择上



#### 类选择器

**规定用圆点`.`来定义**，针对你想要的所有的标签使用

```html
<!DOCTYPE html>
<html>
    <head>
        <!--类选择器-->
        .oneclass{
        width:800px;
        }
    </head>
    <body>
        <!--class后面的名字可以由数字、下划线、字母组成，数字不可开头，尽量少用数字-->
        <h2 class="oneclass">
    	你好
		</h2>
    </body>
</html>

```

**class属性的特点：**

+ 类选择器可以被多种标签使用
+ 类名可以由字母、数字、下划线组成，不能以数字开头。
+ 同一个标签可以使用多个类选择器。用空格隔开

```html
<!--这种写法是对的，一个标签只能出现一次，多个类选择器可以在一个标签中，用空格隔开-->
<h3 class="classone classtwo">
    标题
</h3>


<!--这种写法是错误的，一个标签只能出现一次-->
<h3 class="teshu" class="zhongyao">
    标题
</h3>
```



#### ID选择器

针对某一个特定的标签来使用，**只能用一次**。css中的ID选择器以`#`来定义。

```html
<!DOCTYPE html>
<html>
    <head>
        #mytitle{
        border:3px dashed green;
        }
    </head>
    <body>
        <h2 id="mytitle">
            标题
        </h2>
    </body>
</html>
```

> 1，ID选择器是唯一的，只能用于其中某个标签，不能用于多个标签
>
> 2，ID选择器后面的名字，由字母、下划线、数字组成，但是不能以数字开头





#### 合并选择器

语法：`选择器1，选择器2,...{}`，通过这种方式可以**提取共同的样式，减少代码的重复**

```css
.header,.footer{
    color:red;
}
```





#### 选择器的优先级

CSS中，权重用数字衡量：

​		元素选择器的权重为：1

​		class选择器的权重为：10

​		id选择器的权重为：100

​		内联样式的权重：1000

**优先级顺序从高到低：内联样式>ID选择器>类选择器>元素选择器> 全局选择器**





### 字体属性

css字体属性定义字体，颜色、大小、加粗、文字样式。



**规定文本颜色 ：`color`**:

```css
/*颜色可以由下面四种方式表示，最常用的是第二种十六进制方式来表示颜色*/
p{
	color:red;
	color:#ff0000;
	color:rgb(255,0,0);
	color:rgba(255,0,0,0.5)
}
```



**设置文本的大小：`font-size`**

```css
h1{
    font-size:40px;
}
```

> chorme浏览器接受最小字体是12px



**设置文本的粗细：`font-weight`**

| 值      | 描述                                   |
| ------- | -------------------------------------- |
| bold    | 定义粗体字符                           |
| bolder  | 定义更粗的字符                         |
| lighter | 定义更细的字符                         |
| 100~900 | 定义由细到粗，400等同默认，700等同bold |

```css
h1{
    font-weight:normal;
}

div{
    font-weight:bold;
}

p{
    font-weight:900;
}
```





**定义文本的字体样式：`font-style`**

| 值     | 描述     |
| ------ | -------- |
| normal | 默认值   |
| italic | 定义斜体 |

```css
p{
    font-style:italic;
}
```





**指定一个元素的字体：`font-family`**

> 1，每个值用逗号分开
>
> 2，如果字体名称包含空格，它必须加上引号

```css
p{
    font-family:"Microsoft YaHei","Simsun";
}
```







### 背景属性

CSS背景属性主要有以下几个：

| 属性                | 描述                     |
| ------------------- | ------------------------ |
| background-color    | 设置背景颜色             |
| background-image    | 设置背景图片             |
| background-position | 设置背景图片显示位置     |
| background-repeat   | 设置背景图片如何填充方式 |
| background -size    | 设置背景图片大小属性     |



**`background-color`属性：**设置背景颜色

```html
<!DOCTYPE html>
<html>
    <head>
        <sytle>
            .box{
            	width:300px;
            	height:400px;
            	<!--背景颜色-->
            	background-color:red;
            }
        </sytle>
    </head>
    <body>
        <div class=".box">
            
        </div>
    </body>
</html>
```



**`background-image`属性**：设置元素的背景图片

```html
<style>
    .box1{
        width:300px;
        height:300px;
        <!--注意插入背景格式-->
        background-image:url("./xxx.jpg");
    }
</style>

<div class=".box1">
    
</div>
```



**`background-repeat`属性**：设置背景图片平铺方式

| 值        | 说明             |
| --------- | ---------------- |
| repeat    | 默认值           |
| repeat-x  | 只向水平方向平铺 |
| repeat-y  | 只向垂直方向平铺 |
| no-repeat | 不平铺           |

```html
<style>
    .box{
        width:1200px;
        height:1200px;
        background-color:red;
        background-image:url("./xxx.jpg");
        <!--设置平铺方式，此时不平铺-->
        background-repeat:no-repeat;
        }
</style>

<div class=".box">
    
</div>
```



**`background-size`属性**:设置图片的大小

| 值         | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| length     | 设置背景图片的宽度和高度，第一个值宽度，第二个值高度，如果只设置一个，第二个值默认为auto |
| percentage | 计算相对位置区域百分比，第一个值宽度，第二个值高度，如果只是设置一个，第二个值auto |
| cover      | 保持图片纵横比并将图片缩放成完全覆盖背景区域的最小大小       |
| contain    | 保持图片纵横比并将图片缩放成适合背景定位区域的最大大小       |



```html
<style>
    .box{
        width:1200px;
        height:1200px;
        background-image:url("./1.png");
        background-repeat:no-repeat;
        <!--设置图片的宽度和高度，一般不用第一种和第二种方式，常常采用第三种方式cover-->
        background-size:100% 100%;
        <!--background-size:cover;-->
    }
</style>
<div class=".box">
    
</div>
```



**`background-position`属性**：设置图片的起始位置，其默认值为：0% 0%

| 值            | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| left top      | 左上角                                                       |
| left center   | 左中                                                         |
| left bottom   | 左下                                                         |
| right top     | 右上角                                                       |
| right center  | 右中                                                         |
| right bottom  | 右下                                                         |
| center top    | 中上                                                         |
| center center | 中中                                                         |
| center bottom | 中下                                                         |
| x% y%         | 第一个值是水平位置，第二个值是垂直位置，左上角是0% 0%，右下角是100% 100% <br>如果只指定一个值，其他默认是50%。未指定默认为0% 0% |
| xpos ypos     | 单位是像素                                                   |



```html
<style>
    .box{
        width:300px;
        height:300px;
        background-image:url("./2.webp");
        background-position:left center;
    }
</style>
<div class=".box">
    
</div>
```







### 文本属性

`text-align`属性：指定元素文本的水平对齐方式

| 值     | 描述                 |
| ------ | -------------------- |
| left   | 文本居左排列，默认值 |
| right  | 把文本排列到右边     |
| center | 把文本排列到中间     |

```html
<style>
    h1{
        text-align:cneter;
    }
</style>


<h1>
    标题
</h1>
```





`text-decoration`属性：规定添加到文本的修饰，下划线、上划线、删除线等

| 值           | 描述       |
| ------------ | ---------- |
| underline    | 定义下划线 |
| overline     | 定义上划线 |
| line-through | 定义删除线 |

```html
<style>
    p1{
        text-decoration:line-through;
    }
</style>

<p>段落</p>
```



**`text-transform`属性**：控制文本大小写

| 值         | 描述                 |
| ---------- | -------------------- |
| captialize | 定义每个单词开头大写 |
| uppercase  | 定义全部大写字母     |
| lowercase  | 定义全部小写字母     |

```html
<style>
    p{
       text-transform:uppercase; 
    }
</style>

<p>
    段落
</p>
```



**`text-indent`属性**：规定文本块中首行文本缩进

```html
<style>
    p{
        text-indent:30px;
    }
</style>

<p>
    段落
</p>
```

> **注意：允许负值，但是值如果是负数，将第一行左缩进**





### 表格属性

**`border`表格边框属性**：指定css表格边框

```html
<style>
	table,td{
        <!--这里有三个值，第一个值为线粗细，第二值为线体，solid表示实体，dashed表示虚线，第三个值为线颜色-->
		border:1px solid red;
	}
</style>

```





**`border-collapse`折叠边框属性**：设置表格的边框是否被折叠成一个单一的边框或隔开

```html
<style>
    table{
        border-collapse:collapse;
    }
    table,td{
        border:1px solid black;
    }
</style>
```



**`width和height`表格宽度和高度属性**：定义表格的宽度和高度

```html
<style>
    table{
        width:500px;
        height:300px;
    }
    td{
        height:50px;
    }
</style>
```



`text-align`表格文字对齐属性：设置表格中的文字对齐方式

```html
<style>
    td{
        <!--设置水平对齐方式，left center right-->
        text-align:right;
    }
    
    td{
        <!--垂直对齐方式，top cneter bottom-->
       	vertical-align:bottom;
    }
</style>
```



**`padding`表格填充属性**：在表的内容中控制表格之间的边框，应使用td和th元素的填充属性

```html
<style>
    td{
        <!--将文字和边框之间用设定值来撑开，上下左右都会撑开-->
        padding:20px;
        <!--可以设置双值，第一个值代表上下，第二个值代表左右-->
        <!--下面的padding表示，上下内边距是20px，左右内边距是10px-->
        <!--padding:20px 10px;-->
    }
</style>
```



表格颜色属性：可以指定表格边框颜色，文本颜色和背景颜色。

```html
<style>
    table,td{
        <!--表格边框颜色-->
        border:1px solid green;
    }
    td{
        <!--表格背景颜色-->
        background-color:red;
        <!--文本颜色-->
        color:white;
    }
</style>
```











### 关系选择器

#### 后代选择器

**选择所有被E元素包含的F元素，只要是E的后代中的F元素都生效，中间用空格隔开**

```html
E F{}


<style>
    ul li{
        color:green;
    }
</style>


<ul>
    <li>宝马</li>
    <li>奔驰</li>
</ul>

<ol>
    <li>奥迪</li>
</ol>


```





#### 子代选择器

选择所有作为E元素**的直接子元素F，对更深一层的元素不起作用**，用 > 表示

```html
E > F{}

<style>
    div > a{
        color:red;
    }
</style>


<div>
    <a href="#">子元素1</a>
    <p>
        <a href="#">孙元素</a>
    </p>
    <a href="#">子元素2</a>
</div>
```





#### 相邻兄弟选择器

**选择紧跟E元素后的F元素，用 + 表示**，选择相邻的第一个兄弟元素，**注意只能向下选择**

```html
E + F{}

<style>
    h1+p{
        color:red;
    }
</style>


<h1>
    标题
</h1>
<p>
    段落1
</p>
<p>
    段落2
</p>
```





#### 通用兄弟选择器

选择E元素之后的所有兄弟元素F，作用于多个元素，用~隔开。**注意；向下选择**

```html
E~F{}

h1~p{
	color:red;
}


<h1>
    标题
</h1>
<p>
    段落1
</p>
<p>
    段落2
</p>
```





### CSS盒子模型（Box Model）

所有的HTML元素都可以看作盒子，在CSS中，“box model”这一术语是用来涉及和布局时使用的。

CSS 盒子模型本质是一个盒子，封装周围的HTML元素，它包括：外边距（margin），边框（border），内边距（padding），和实际内容（content）。

![image-20230320152249423](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230320152249423.png)

+ Margin（外边距）：清除边框外的区域，外边距是透明
+ Border（边框）：围绕在内边距和内容外的边框
+ Padding（内边距）：清除内容周围的区域
+ Content（内容）：盒子的内容，显示文本和图像

```html
<style>
	div{
	width:100px;
	height:100px;
	background-color:green;
	padding:50px 10px;
	border:5px;
    <!--第一个值是上下，第二个值是左右，如果只设置一个值则上下左右均为该值-->
	margin:50px 10px;
	}
</style>

<div>
    块内容
</div>
```







### 弹性盒子模型

弹性盒子模型是CSS3的一种新的布局模式，CSS3弹性盒子是一种当页面需要适应不同的屏幕大小以及设备类型时确保元素拥有恰当行为的布局方式。

引入弹性盒子布局模型的目的是提供一种更加有效的方式来**对一个容器中的子元素（子容器）进行排列、对齐和分配空白空间。**



CSS3弹性盒子内容：

​	弹性盒子由弹性容器（Flex container）和弹性子元素（Flex item）组成。

​	弹性容器通过设置`display`属性的值为`flex`将其定义为弹性容器。

​	弹性容器内包含了一个或多个弹性子元素。

> 弹性容器外及弹性子元素内是正常渲染的。弹性盒子只定义了弹性子元素如何在弹性容器内布局。



```html
<style>
    .container{
        width:500px;
        height:500px;
        background-color:#555;
        display:flex;
    }
    .box1{
        width:100px;
        height:100px;
        background-color:red;
    }
    .box2{
        width:100px;
        height:100px;
        background-color:green;
    }
    .box3{
        width:100px;
        height:100px;
        background-color:blue;
    }
</style>
```

> **注意：弹性盒子里内容默认是横向摆放**



==**父元素上的属性**==：

​	**display属性**：

​		`display:flex;`开启弹性盒子

​		`display:flex;`属性设置后子元素默认水平排列



​	**flex-direction属性**：

​		`flext-direction`属性指定了弹性子元素在父容器中的位置

​		`flext-direction:row | row-reverse | column | column-reverse`

​			其中：

​				`row`：横向从左到右排列（左对齐），默认的排列方式

​				`row-reverse`：反转横向排列（右对齐，从后往前排，最后一项排在最前面）

​				`column`：纵横排列

​				`column-reverse`：反转纵向排列，从后往前排，最后一项排在最上面

​	

​	**justify-content属性：**

​	`justify-content`属性应用在弹性容器上，把弹性项沿着弹性容器的主轴线（纵向）（main axis）对齐

​	`justify-content:flex-start | flex-end | center`

​		其中：

​			`flext-start`：弹性项目向行头填充。这是默认值。一个弹性项的main-start外边距边线被放置在该行的main-start边线，而后续弹性项依次平齐									   摆放。

​			`flex-end`：弹性项目向行尾紧挨填充。第一个弹性项的main-end外边距边线被放置在该行的main-end边线，而后续弹性项依次平齐摆放

​			`center`：弹性项目居中紧挨着填充。（如果剩余的自由空间是负的，则弹性项目将在两个方向上同时溢出）

​	



​	**align-items属性：**

​	`align-items`属性设置或检索弹性盒子元素在侧轴（水平）上的对齐方式

​	`align-items:flex-start| flext-end |center`

​		其中：

​			`flex-start`：弹性盒子元素的侧轴起始位置的边界紧靠住该行的侧轴起始边界

​			`flex-end`：弹性盒子元素的侧轴起始位置的边界紧靠住该行的侧轴结束边界

​			`center`：弹性盒子元素在该行的侧轴上居中放置。





==**子元素上的属性：**==

​	**flex属性：**

​	`flex`：**根据弹性盒子元素所设置的扩展因子作为比率来分配剩余空间**

​	默认为0，即如果存在剩余空间，也不放大。

​	如果只有一个子元素设置，那么按扩展因子转化的百分比对其分配剩余空间。0.1即10%，1即100%，超出按100%

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>

    <style>
        .container{
            width:500px;
            height:500px;
            background-color:#555;
            display:flex;
            flex-direction: row;
            justify-content:center;
            align-items: center;
         
        }
        .box1{
            width:100px;
            height:100px;
            background-color:red;
            flex: 3;
        }
        .box2{
            width:100px;
            height:100px;
            background-color:green;
            flex: 1;
        }
        .box3{
            width:100px;
            height:100px;
            background-color:blue;
            flex:1;
        }
    </style>

</head>
<body>
    <div class="container">

        <div class="box1"></div>
        <div class="box2"></div>
        <div class="box3"></div>

    </div>

</body>
</html>
```







### 文档流

文档流是文档中可显示对象在排列时所占用的位置/空间。

例如：块元素自上而下摆放，内联元素从左到右摆放 





**脱离文档流**：

​	使用一个元素脱离标准文档流有三种：

​	1，浮动

​	2，绝对定位

​	3，固定定位	





### 浮动

`float`属性定义元素在哪个方向浮动，任何元素都可以浮动。

| 值    | 描述         |
| ----- | ------------ |
| left  | 元素向左浮动 |
| right | 元素向右浮动 |

**注意：浮动以后使元素脱离了文档流；浮动只有左右浮动，没有上下浮动。**





**元素浮动**：

脱离文档流后，元素相当于在页面上增加一个浮层来放置内容。此时可以理解为有两层，一层是底层的原页面，一层是脱离文档流的上层页面，所以会出现折叠现象。

```html
<style>
    .box{
        float:left;
        <!--float:right;-->
        width:200px;
        height:200px;
        background-color:blue;
    }
    .container{
        width:400px;
        height:400px;
        background-color:red;
    }
</style>

<div class="box"></div>
<div class="container"></div>
```

**注意：当容器不足以横向摆放内容时，会在下一行摆放**











### 清除浮动



**浮动的副作用：**

+ 浮动元素会造成父元素高度塌陷
+ 后续元素会受到影响



**清除浮动：**当父元素出现塌陷的时候，对布局是不利的，所以我们必须清除副作用。

+ 父元素设置高度

+ 受影响的元素增加clear属性

  + clear属性有三个值，left，right，both，推荐使用both

+ overflow清除浮动

  + 如果父级塌陷，并且同级元素也受到影响，可以使用overflow属性清除浮动
  + 这种情况下，父布局不能设置高度
  + 父级标签的样式里面添加：`overflow:hidden;`和` clear:both;`

+ 伪对象方式

  + ```html
    <!--给父级标签添加伪对象-->
    
    .container::after{
    	content:"";
    	display:block;
    	clear:both;
    }
    ```

    







### 定位

**`position`属性指定了元素的定位类型**

| 值       | 描述      |
| -------- | --------- |
| relative | 相对定位  |
| absolute | 绝对定位  |
| fixed    | 固定定位i |

**其中，绝对定位和固定定位会脱离文档流**

**设置定位后，可以使用四个方向属性进行调整位置，属性分别为：left、top、right、bottom**





**相对定位：**

```html
<style>
    .box{
        width:200px;
        height:200px;
        background-color:red;
        position:relative;
        left:40px;
        top:50px;
    }
</style>

<div class="box"></div>
```



**绝对定位：**脱离文档流，==注意：多个绝对定位会产生多层==

```html
<style>
    .box1{
        width:200px;
        height:200px;
        background-color:red;
        position:absolute;
        left:40px;
        top:50px;
    }
    
    .box2{
        width:300px;
        height:300px;
        background-color:aqua;
    }
</style>

<div class="box1"></div>
<div class="box2"></div>
```



**固定定位：**脱离文档流，会固定在某个位置，不会随着页面滚动而滚动。

```html
<style>
    .box1{
        width:200px;
        height:200px;
        background-color:red;
        position:fixed;
        left:40px;
        top:50px;
    }
    .box2{
        width:400px;
        height:400px;
        background-color:blueviolet;
    }
    
</style>

<div class="box"></div>
```



> 设置定位后，相对定位和绝对定位都是相较于具有定位的父级元素进行位置调整，如果父级元素不存在定位，则继续向上级寻找，直到顶层文档。

```html
<style>
    .container{
        width:200px;
        height:200px;
        background-color:#888;
        
        <!--如果想要子盒子相对父盒子进行移动，则需要给父级元素添加定位，position:relative;-->
        margin-left:100px;
    }
    .box{
        width:100px;
        height:100px;
        background-color:blueviolet;
        position:absolute;
        left:50px;
        top:50px;
    }
</style>

<div class="container"></div>
<div class="box"></div>
```





`z-index`属性：设置元素的堆叠顺序。拥有更高的堆叠顺序的元素总是会处于堆叠顺序较低的元素的前面。

```html
<style>
    .box1{
        width:200px;
        height:200px;
        background-color:red;
        position:absolute;
        z-index:2;
    }
	
    .box2{
        width:300px;
        height:300px;
        background-color:green;
        position:absolute;
        z-index:1
    }
</style>

<div class="box1"></div>
<div class="box2"></div>
```







### CSS3新特性



#### 圆角

使用CSS3的`border-radius`属性，可以给任何元素制作“圆角”。

`border-radius`属性，可以使用下列规则：

+ 四个值：第一个值为左上角，第二值为右上角，第三个值为右下角，第四个值为左下角
+ 三个值：第一个值为左上角，第二个值为右上角和左下角，第三个值为右下角
+ 两个值：第一个值为左上角和右下角，第二值为右上角和左下角
+ 一个值：四个圆角相同



```html
<style>
    .box{
        width:56px;
        height:56px;
        background-color:blueviolet;
        border-radius:15px,
    }
</style>
```









#### 阴影

`box-shadow`属性：向框添加一个或多个阴影。

```html
box-shadow:h-shadow v-shadow blur color;
```

| 值       | 描述                 |
| -------- | -------------------- |
| h-shadow | 必选，水平阴影的位置 |
| v-shadow | 必选，垂直阴影的位置 |
| blur     | 可选，模糊距离       |
| color    | 可选，阴影的颜色     |



```html
<style>
    .box{
        width:400px;
        height:400px;
        background-color:blueviolet;
        box-shadow:10px 10px 20px rgba(0,0,0,0.5);
    }
</style>

<div class="box"></div>
```







### 动画

动画是使元素从一种样式逐渐变化成另一个样式的效果。

可以改变任意多的样式任意多的次数。

可以使用百分比来规定变化发生的事件，或用关键字“from”和“to”，等同于0%和100%。

其中0%是动画的开始，100%是动画的完成。



#### 使用`@keyframes`创建动画

使用`@keyframes`规则，可以创建动画：

```html
<style>
	@keyframes name{
		from或0%{
			css样式
		}
		percent{
			css样式
		}
        to或100%{
            css样式
        }
	}
</style>
```

其中：

+ name：动画名称，开发人员自己命名；
+ percent：为百分比值，可以添加多个百分比值；



```html
<!--示例-->

<style>
    @keyframes myAnim{
        0%{
            background-color:red;
        }
        50%{
            background-color:green;
        }
        100%{
            background-color:red;
        }
    }
</style>
```





#### `animation`执行动画

`animation:name duration timing-function delay iteration-count direction;`

| 值                   | 描述                                                      |
| -------------------- | --------------------------------------------------------- |
| name                 | 设置动画的名称                                            |
| duration             | 设置动画的持续时间                                        |
| timing-function      | 设置动画效果的速率                                        |
| delay                | 设置动画的开始时间                                        |
| iteration-count      | 设置动画的循环次数，infinite为无限次的循环                |
| direction            | 设置动画的播放方向                                        |
| animation-play-state | 控制动画的播放状态：running代表播放，而paused代表停止播放 |



| `timing-function`值 | 描述             |
| ------------------- | ---------------- |
| ease                | 逐渐变慢（默认） |
| linear              | 匀速             |
| ease-in             | 加速             |
| ease-out            | 减速             |
| ease-in-out         | 先加速后减速     |



| `direction`值 | 描述                                             |
| ------------- | ------------------------------------------------ |
| normal        | 默认值为normal表示向前播放                       |
| alternate     | 动画播放在第偶数次向前播放，第奇数次向反方向播放 |





```html
<!--示例-->

<style>
    .animation{
        width:300px;
        height:300px;
        background-color:red;
        animation:myAnim 5s linear 0s infinite;
    }
    .animation:hover{
        animation-play-state:paused;
    }
    
    @keyframes myAnim{
        0%{
            background-color:red;
        }
        50%{
            background-color:green;
        }
        100%{
            background-color:red;
        }
    }
</style>

<div class="animation"></div>
```



#### 呼吸示例

```html
<style>
    .box{
        width:500px;
        height:500px;
        margin:40px auto;
        background-color:#2b92d4;
        border-radius:50%;
        box-shadow:0 1px 2px rgba(0,0,0,0.3);
        animation:breathe 1s ease-in-out infinite alternate;
    }
    
    @keyframes breathe{
        0%{
            opacity:0.2;
            box-shadow:0 1px 2px rgba(255,255,255,0.1);
        }
        50%{
            opacity:0.5;
            box-shadow:0 1px 2px rgba(18,190,84,0.76);
        }
        100%{
            opacity:1;
            box-shadow:0 1px 30px rgba(59,255,255,1);
        }
    }
	
</style>
<div class="box"></div>
```







### 媒体查询

媒体查询能使页面在不同终端设备下达到不同的效果。媒体查询会根据设备的大小自动识别加载不同的样式。



**设置`meta`标签：**

​	使用设备的宽度作为视图宽度并禁止初始的缩放。在`<head>`标签里加入这个`meta`标签。

```html
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
```

其中：

+ `width=device-width`宽度等于当前设备的宽度
+ `initial-scale`初始的缩放比例（默认设置为1.0）
+ `maximum-scale`允许用户缩放到的最大比例（默认设置为1.0）
+ `user-scalable`用户是否可以手动缩放（默认设置为no）



**媒体查询语法：**

```html
<style>
    
    .box{
        width:300px;
        height:300px;
    }
    
    @media screen and (max-width:768px){
        /* 设备小于768px加载样式*/
        .box{
            background-color:red;
        }
    }
    
    @media screen and (max-width:992px)and (min-width:768px){
        /*设备大于768px但小于992px加载样式*/
        .box{
            background-color:pink;
        }
    }
    
    @media screen and (min-width:992px){
        /*设备大于992px加载模式*/
        .box{
            background-color:green;
        }
    }
</style>

<div class="box"></div>
```







### 雪碧图

CSS Sprite也叫CSS精灵图、CSS雪碧图，是一种网页图片应用处理方式。它允许你将一个页面涉及到的所有零星图片都包含到一张大图中去。



**优点：**

+ 减少图片的字节
+ 减少网页的http请求，从而大大的提高页面的性能



**原理：**

+ 通过`background-image`引入背景图片
+ 通过`background-position`把背景图片移动到自己需要的位置



**实例：**

```html
<style>
    .icon1{
        /*display可以将一个元素变为block块级元素，inline行内元素*/
        display:block;
        background-image:url("1.png");
        background-position:-20px 0;
        width:45px;
        height:70px;
    }
    
    .icon2{
        display:block;
        background-image:url("1.png");
        background-position:-93px -84px;
        width:45px;
        height:70px;
    }
</style>

<span class="icon1"></span>
<span class="icon2"></span>
```





### 字体图标

我们经常会用到一些图标，但是在使用这些图标时，往往会遇到失真的情况，而且图片数量很多的话，页面加载就越慢。所以，可以使用字体图标的方式来显示图标，既解决了失真的问题，也解决了图片占用资源的问题。

常用字体图标库：https://iconfont.cn/



**优点：**

+ 轻量级：加载速度快，减少http请求
+ 灵活性：可以利用CSS设置大小颜色等
+ 兼容性：网页字体支持所有现代浏览器，包括IE低版本



**使用字体图标：**

+ 注册账号并登录
+ 选取图标或搜索图标
+ 添加购物车
+ 下载代码
+ 选择`font-class`引用

```html
<head>
    <link rel="stylesheet" href="./font/iconfont.css">
    <style>
        .home{
         	font-size:100px;
            color:#e1251b;
        }
    </style>
</head>

<body>
    <span class="iconfont icon-home home"></span>
</body>
```











## JS 

JavaScript 是一种轻量级的脚本语言。所谓”脚本语言“，指的是它不具备开发操作系统的能力，而是用来编写控制其他大型应用程序的”脚本“。



**JS作用：**

+ 操控浏览器的能力
+ 广泛的使用领域
+ 易学性





### 语句和标识符

#### 语句

```js
var num = 10;  //在js中，语句以分号结束
```



#### 标识符

标识符（identifier）指的是用来识别各种值得合法名称。最常见的标识符就是变量名。

标识符是由：字母、美元符号（$）、下划线（_）、数字组成，其中数字不能用于开头。

> 中文是合法的标识符，可以用作变量名（不推荐）



#### 保留字

```js
arguments break case catch class const continue debugger default delete do else enum eval export extends false finally for function if implements import in instanceof interface let new null package private protected public return static super switch this throw true try typeof var void while with yield
```





### 变量



#### 变量提升

javascript引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成地结果，就是所有的变量的声明语句，都会提升到代码的头部，这叫做变量提升。

```js
console.log(num);  //打印结果为：undefined
var num =10;

上面的语句等价于下面的执行过程：

var num;
console.log(num);
num = 10;
```





#### 打印

```js
var num = 10;
console.log(num);
```









### javascript引入到文件



#### 嵌入HTML文件中

```html
<!DOCTYPE html>
<html>
    <head>
        
    </head>
    
    <body>
        <script>
        	var num = 10;
            console.log(num);
        </script>
    </body>
</html>
```





#### 引入本地独立js文件

**注意：`<script></script>`标签可以在head中也可以在body中，推荐在body中**

```html
<!DOCTYPE html>
<html>
    <head>
        <!-- 注意：<script></script>标签可以在head中也可以在body中，推荐在body中 -->
        <!-- <script src="./hello.js"></script>  -->
    </head>
    
    <body>
        <script src="./hello.js"></script>
    </body>
</html>
```

```js
//hello.js文件

var age = 10;
console.log(num);
```









#### 引入网络来源文件

```html
<!DOCTYPE html>
<html>
    <head>
        
    </head>
    
    <body>
        <script scr="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
    </body>
</html>
```







### javascript注释与常见输出方式

  #### 注释

```js
//   单行注释


/* */  多行注释
```

> 嵌入在HTML文件中的注释： `<!-- 注释 -->`
>
> CSS 文件注释 ： `/* 注释 */`
>
> 在HTML、CSS、JS 注释快捷方式：`ctrl + /`





#### 输出方式

```js
//第一种方式，弹出框
alert("要输出的内容")

//第二种方式，输出到页面
document.write("输出内容到页面上")

//第三种方式，输出到控制台
console.log("输出内容到控制台")

```





### 数据类型

javascript语言有6种数据类型+ES6新增加的2种数据类型。



#### 原始类型（基本类型）

+ 数值类型

  + ```js
    var age = 20;  //20就是数值
    ```

+ 字符串类型

  + ```js
    var name = "张三";  //被字符串包裹的 称为字符串类型， 单引号和双引号都可以
    ```

+ 布尔类型

  + ```js
    var flag= true;
    var flag = false;
    ```

  

#### 复合数据类型（引用类型）

对象：因为一个对象往往是由多个原始类型的值合成，可以看作是一个存放各种值的容器。

```js
var user = {
    name:"张三",
    age :20,
    learn :true
}
```

> **undefined、null ，一般将其看作两个特殊的值**





### typeof运算符

**`typeof`判断数据类型，用于==判断基本数据类型和对象==**



```js
//判断是数值，返回number
typeof 20;   // 返回number

//判断是字符串，返回string
typeof "hello world";  //返回string

//判断是布尔值，返回boolean
typeof false;  //返回boolean

//判断是对象，返回object
typeof {};  //返回object

//判断null，返回object
typeof null;  //返回object

//判断undefined，返回undefined
typeof undefined;  //返回undefined

```





### 运算符

```js
//算术运算符

+ - * / % ++ --

注意：
自增和自减 ++ -- ，用法和c语言中一样

a++  表示：先用后自增 
a--  表示：先用后自减
++a  表示：先自增后用
--a  表示：先自减后用



//赋值运算符
= ，+= ，-= ， *= ， /= ， %= 


//比较运算符
>  <  <=  >=  ==  ===  !=  !==
    
注意：
=== 和 ！== 是严格比较的，要求值和类型都必须相等
var a= 10;
var b= "10";
var c= 10;
console.log(a===b)  //false
console.log(a===c)  //true


== 和 ！= 是不严格比较，只比较值是否相等，而不比较类型

console.log(a==b)  //true
console.log(a==c)  //true




//布尔运算符
! 取反运算符
&& 且运算符
|| 或运算符

```





### 条件语句

#### if条件判断

```js
if (布尔值){
    语句;
}

-----------------------

if (布尔值){
    语句；
}else{
    语句;
}

------------------------

if (布尔值){
    语句;
}else if(布尔值){
    语句;
}else if (布尔值){
    
}else (布尔值){
    
}


```



#### switch

```js
switch(变量){
    case 值1:
        语句1;
        break;
    case 值2:
        语句2;
        break;
    case 值3:
        语句3;
        break;
    ...
    default:
    	语句;
        
}
```

**注意：每个case代码块内部的break语句不能少，否则会接下去执行下一个case代码块，而不是跳出switch结构。**





#### 三元运算符

```js
(条件) ？ 表达式1 : 表达式2


var a;
a = (10>12)?10:12;
console.log(a);  // 12
```







### 循环语句

#### for循环

```js
for (初始化表达式; 条件; 迭代因子){
    语句;
}


示例:
for (var i=0;i<10;i++){
    console.log(i);
}
```

+ 初始化表达式：确定循环变量的初始值，只在循环开始时执行一次
+ 布尔表达式：每轮循环开始时，都要执行这个条件表达式，只有值为真，才继续进行循环
+ 迭代因子：每轮循环的最后一个操作，通常用来递增循环变量

> for循环语句中的三个表达式 , 可以省略其中任何一个, 也可以全部省略 .
>
> 但是全部省略 , 就会变成死循环





#### while循环

```js
while (条件){
    //迭代因子;
    语句;
    迭代因子;
}
```



#### break语句

```js
break 语句用于跳出代码块或循环
```





#### continue语句

```js
continue 语句用于立刻终止本轮循环,返回循环结构的头部,开始下一轮循环
```





### 字符串

字符串就是由零个或多个排在一起的字符，放在单引号或双引号中。

```js
'hello world!'
"hello world!"
```

注意：单引号中可以嵌套使用双引号，反之亦然。但是单引号中嵌套单引号或双引号中嵌套双引号，则需要使用转义，需要在引号前面添加反斜杠。



**如果长字符串必须分成多行，可以在每一行的尾部使用反斜杠。**

```js
var longstring = "long\
long\
string";
```



#### length属性

`length`属性返回字符串的长度，该属性是无法改变的。

```js
var s = "hello";
s.length // 5
```



#### 字符串常用方法

+ `charAt`方法：返回指定位置的字符，位置是从0开始的

  + ```js
    var s = 'hello';
    s.charAt(1);  //e
    s.charAt(s.length-1);  //o
    ```

  + 如果参数为负数，或大于等于字符串的长度，`charAt`返回空字符串



+ `concat`方法：用于连接两个字符串，返回一个新字符串，不改变原字符串

  + ```js
    var s1 = "hello";
    var s2 = " world!";
    s1.concat(s2);  //hello world!
    s1  // hello
    ```

  + `concat`方法可以接受多个参数

    ```js
    "sxt".concat("itbaizhan","hello world!")  //"sxtitbaizhanhello world!"
    ```

  + 如果参数不是字符串，`concat`方法会将其先转换为字符串，然后再连接

    ```js
    var one = 1;
    var two = 2;
    var three = '3';
    
    ''. concat(one,two,three)  //"123"
    ```



+ `substring`方法：用于从原字符串中取出字符串并返回，不改变原字符串。它的第一个参数表示字符串的开始位置（从0开始），第二个参数表示结束位置（左包含右不包含），如果省略第二个参数，则表示字符串一直到原字符串的结尾。

  + 注意：如果第一个参数大于第二个参数，`substring`方法会自动更换两个参数的位置；如果参数是负数，`substring`方法自动将负数转为0

  + ```js
    var str = "itbaizhan";
    document.write(str.substring(0,2)); //it
    document.write(str.substring(0)); //itbaizhan
    ```



+ `substr`方法：用于从原字符串取出字符串并返回，不改变原字符串，跟`substring`方法作用相同。

  + 注意：`substr`方法的第一个参数是字符串的开始位置（从0开始），第二个参数是子字符串的长度。

  + 注意：如果省略第二个参数，表示子字符串一直到原字符串的结尾。如果第一个参数是负数，表示倒数计算的字符位置。如果第二个参数是负数，将自动转为0，因此会返回空字符串。

  + ```js
    var str = "itbaizhan";
    document.write(str.substr(-7,3)); //bai
    document.write(str.substr(-4));  //zhan
    ```



+ `indexOf`方法：用于确定一个字符串在另一个字符串中第一次出现的位置，返回结果是匹配开始的位置。如果返回-1，则表示不匹配。

  + ```js
    var str = "hello world!";
    document.write(indexOf("llo")) //2
    document.write(indexOf("ha"))  //-1
    ```



+ `trim`方法：用于去掉字符串两端的空格，返回一个新字符串，不改变原字符串。

  + 注意：该方法不仅去掉空格，还包括制表符(`\t`,`\v`)、换行符(`\n`)和回车符(`\r`)

  + ```js
    var str = "  hello world!  ";
    document.write(str.trim(str)); //hello world!
    ```

  + ES6扩展方法：

    + `trimEnd()`方法：去掉字符串尾部空格

      ```js
      var str = "hello   ";
      document.write(trimEnd(str)); //hello
      ```

    + `trimStart()`方法：去掉字符串首部空格

      ```js
      var str = "   hello";
      document.write(trimStart(str));  //hello
      ```



+ `split`方法：按照给定的规则分割字符串，返回一个由分割出来的字符串组成的数组。

  + 注意：如果分割规则为空字符串，则返回数组的成员是原字符串的每一个字符。

  + 如果省略参数，则返回数组的唯一成员就是原字符串。

  + `split`方法可以接受第二个参数，限定返回数组的最大成员数

  + ```js
    var str = "it|bai|zhan";
    document.write(str.split("|")); //["it","bai","zhan"]
    document.write(str.split("")); //['i','t','|','b','a','i','z','h','a','n']
    document.write(str.split()); //["it|bai|zhan"]
    document.write(str.split("|",1)); //["it"]
    ```







### 数组

数组（Array）是按次序排列的一组值。每个值的位置都有编号（从0开始），整个数组用方括号表示。

```js
var array = ["it","bai","zhan"];
```



数组可以在定义时赋值，也可以先定义后赋值。

```js
var arr = [];
arr[0]="it";
arr[1]="bai";
arr[2]="zhan";
```



**==任何类型的数据==，都可以放入数组。**

```js
var arr = [100,[1,2,3],false];
```



**如果数组的元素还是数组，就形成了多维数组。**

```js
var a = [[1,2],[3,4]]
a[0][1] //2
a[1][1]  //4
```



**数组`length`属性，返回数组的成员数量**

```js
var str = ["it","bai","zhan"];
document.write(str.length)  //3
```



#### 数组遍历

**使用for循环或while循环：**

```js
var a = ["it","bai","zhan"];

//for循环
for (var i=0;i<a.length;i++){
    console.log(a[i]);
}

//while循环
var i = 0;
while(i<a.length){
    console.log(a[i]);
    i++;
}
```



**使用for...in遍历数组：**

```js
var a = ["it","bai","zhan"];

for (var i in a){
    console.log(a[i]);
}
```







#### 数组方法

##### `Array.isArray()`方法

`Array.isArray()`方法==返回一个布尔值==，表示参数是否为数组。它可以弥补`typeof`运算符的不足。

```js
var arr = ["张三",100,false];
console.log(typeof arr); //输出：object


通过Array.isArray()方法可以快速判断是一个参数是否为数组
console.log(Array.isArray(arr)); //true
```



##### `push()/pop()`方法



`push`方法用于==在数组的末端添加一个或多个元素==，并返回添加新元素后的数组长度。注意：该方法会改变原数组。

```js
var arr = [];
arr.push("it");
arr.push("bai");
arr.push("zhan",true,false,{})

```



`pop`方法 ==用于删除数组的最后一个元素==，并返回该元素。注意：该方法会改变数组

```js
var arr = ["it","bai","zhan",true,false,{}];
arr.pop();  //{}
```



##### `shift()/unshift()`方法



`shift()`方法用于==删除数组的第一个元素==，并返回该元素。注意：该方法会改变数组。

```js
var arr = ["it","bai","zhan",true,false,{}];
arr.shift(); //it
```



`shift()`方法可以==遍历并清空一个数组==。

```js
var arr = ["it","bai","zhan",true,false,{}];
var item;
while(item = arr.shift()){
    console.log(item);
}
最后，arr为空数组。
```





`unshift()`方法用于==在数组的第一个位置添加元素==，并返回添加新元素后的数组长度。注意：该方法改变原数组。

```js
var arr = ["it","bai","zhan",true,false,{}];

arr.unshift("hello");  //7

其中，arr=["hello","it","bai","zhan",true,false,{}]
```



`unshift()`方法可以接受多个参数，这些参数都会添加到目标数组头部。

```js
var arr = ["it","bai","zhan"];
arr.unshift({},false);
此时，arr=[{},false,"it","bai","zhan"];
```



##### `join()`方法

`join()`方法以指定参数作为分隔符，==将所有数组成员连接作为一个字符串返回==。如果不提供参数，默认用逗号分隔。

```js
var a=["it","bai","zhan",true,1];
a.join(" "); //"it bai zhan true 1"
a.join("|"); //"it|bai|zhan|true|1"
a.join(); //"it,bai,zhan,true,1"
```



==如果数组成员是`undefined`或`null`或空位，会被转成空字符串。==

```js
var arr = [undefined,null];
arr.join("#") //"#"

var arr=["a",,"b"];
arr.join("-");  //"a--b"
```



数组的`join`配合字符串的`split`可以实现数组与字符串的互换。

```js
var arr = ["a","b","c"];
var myArr = arr.join("");
console.log(myArr); //"abc"
console.log(myArr.split("")); //["a","b","c"]
```



##### `concat()`方法

`concat()`方法用于将多个数组合并。它将新数组的成员添加到原数组成员的后部，然后返回一个新数组，原数组不变。

```js
var arr1 =["hello","world"];
var arr2 = ["!"];
arr1.concat(arr2); //["hello","world","!"]
```

除了数组作为参数，`concat`也接受其他类型的值作为参数，添加到目标数组尾部。

```js
[1,2,3].concat("it",true,{})   //[1,2,3,"it",true,{}]
```





##### `reverse()`方法

`reverse()`方法用于颠倒数组元素，返回改变后的数组。注意：该方法将改变数组。

```js
var a = ["a","b","c"];
a.reverse(); //["c","b","a"]

//实现一个字符串反转排序
var str = "hello";
str.split("").reverse().join("") //"olleh"
```





##### `indexOf()`方法

`indexOf()`方法返回==给定元素在数组中第一次出现的位置==，如果没有出现则返回`-1`。

```js
var arr = ["a","b","c"];
arr.indexOf("b")  //1
arr.indexOf("y"); //-1
```



`indexOf()`方法还可以==接受第二个参数，表示搜索的开始位置==

```js
["it","bai","zhan"].indexOf("it",1)   //-1
```





### 函数

#### 函数声明

`function`：该命令声明的代码区块，就是一个函数。`function`命令后面是函数名，函数名后面是一对圆括号，里面是传入函数的参数。函数体放在大括号里面。

```js
function print(s){
    console.log(s);
}
```



#### 函数名的提升

`javaScript`引擎将函数名视为变量名，采用function命令声明函数时，整个函数会像变量声明一样，被提升到代码头部。

```js
print("hello world!")

function print(s){
    console.log(s);
}
```





#### 函数返回值

`javaScript`函数提供了两个接口实现与外界的交互，其中参数作为入口，接受外界信息；返回值作为出口，把运算结果反馈给外界。

```js
function getName(name){
    return name;
}

var myName = getName("itbaizhan");
console.log(myName); //itbaizhan
```







### 对象

对象（object）是javascript语言的核心概念，也是最重要的数据类型。简单说，对象就是**一组“键值对（key-value）”的集合**，**是一种无序的复合数据集合**。

```js
var user = {
    name:"itbaizhan",
    age:13,
    job:["itbaizhan","hello world!"]
    flag:true
}
```





对象的每一个键名又称为“属性”，它的“键值”可以是任何数据类型。**如果一个属性的值为函数，通常把这个属性称为“方法”，它们可以像函数那样调用。**

```js
var user={
    getName:function(name){
        return name;
    }
};
user.getName("itbaizhan") //itbaizhan
```



**如果属性的值还是一个对象，就形式链式引用。**

```js
var user={
    name:"itbaizhan",
    age:12,
    container:{
        frontEnd:["web","android","ios"],
        backEnd:["java","python","golang"]
    }
}
user.container.frontEnd  // ["web","android","ios"]
```





#### Math对象

`Math`是JavaScript的原生对象，提供了各种数学功能。



**`Math.abs()`方法返回参数值的绝对值**

```js
Math.abs(1)  //1
Math.abs(-1) //1
```



**`Math.max()和Math.min()`:**

`Math.max()`方法返回参数之中最大的那个值，`Math.min`返回最小的那个值。如果参数为空，`Math.min`返回`infinity` , `Math.max`返回`-Infinity`。

```js
Math.max(2,-1,5) //5
Math.min(2,-1,5) //-1
Math.min()  //Infinity
Math.max()  //-Infinity
```



**`Math.floor()和 Math.ceil()`**

+ `Math.floor`方法返回==小于参数值的最大整数==

  + ```js
    Math.floor(3.2) //3
    Math.floor(-3.2) //-4
    ```



+ `Math.ceil`方法返回==大于参数值的最小整数==

  + ```js
    Math.ceil(3.2)  //4
    Math.ceil(-3.2)  //-3
    ```

    

**`Math.random()`返回0到1之间的伪随机数**，可能等于0，但是一定小于1.

```js
Math.random()  // 产生大于0小于1的随机数
```







#### Date对象

`Date`对象是JavaScript原生时间库。它以1970年1月1日00：00：00作为时间的零点。



##### `Date.now()`

`Date.now()`方法**返回当前时间距离时间零点（1970年1月1日00：00：00 UTC）的毫秒数**，相当于Unix时间戳乘以1000

```js
Date.now();  //返回距离时间零点的毫秒数
```



##### 时间戳

时间戳是指从时间零点到现在的总秒数。

`Date`对象提供了一系列`get*`方法，用来获取实例对象某个方面的值。

```js
getTime()  //返回实例距离时间零点的总毫秒数
getDate()  //返回实例对象对应每个月的几号（从1开始）
getDay()  //返回星期几，星期日为0，星期一为1
getYear()  //返回距离1900的年数
getFullyear()  //返回四位的年份
getMonth() //返回月份（0表示1月，11表示12月）
getHours()  //返回小时（0-23）
getMilliseconds()   //返回毫秒（0-999）
getMinutes()   //返回分钟（0-59）
getSeconds()  //返回秒（0-59）
```







#### DOM概述

DOM是javascript操作网页的接口，全称为“文档对象模型”（Document Object Model）。它的作用是将网页转为一个JavaScript对象，从而可以使用脚本进行各种操作（比如对元素增删内容）。

浏览器会根据DOM模型，将结构化文档HTML解析成一系列节点，再由这些节点组成一个树状结构（DOM Tree）。所有的节点和最终的树状结构，都有规范的对外接口。

DOM只是一个接口规范，可以用各种语言实现。所以说，DOM不是javascript语法的一部分，但是DOM操作是javascript最常见的任务，都离开了DOM，Javascript就无法控制网页。另一方面，javascript也是最常见DOM操作的语言。





##### 节点

DOM的最小组成单位叫做节点（node）。文档的树形结构（DOM树），就是由各种不同类型的节点组成。每个节点都可以看作是文档树的一片叶子。

![image-20230325184636905](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230325184636905.png)

节点的类型有七种：

+ Document：整个文档树的顶层节点
+ DocumentType：doctype标签
+ Element：网页的各种HTML标签
+ Attribute：网页元素的属性
+ Text：标签之间或标签包含的文本
+ Comment：注释
+ DocumentFragment：文档的片段



##### 节点树

一个文档的所有节点，按照所在的层级，可以抽象成一种树状结构。这种树状结构就是DOM树。它有一个顶层节点，下一层都是顶层节点的子节点，然后子节点又有自己的子节点，就这样层层衍生出一个金子塔结构，倒过来就像一棵树。

浏览器原生提供document节点，代表整个文档

```js
document  //整个文档树
```

除了根节点，其他节点都有三种层级关系：

+ 父节点关系：直接的那个上级节点
+ 子节点关系：直接的下级节点
+ 同级节点关系：拥有一个父节点的节点



##### Node.nodeType属性

不同节点的nodeType属性值和对应的常量如下：

+  文档节点（document）：属性值为9，对应常量为Node.DOCUMENT_NODE
+ 元素节点（element）：属性值为1，对应常量为Node.ELEMENT_NODE
+ 属性节点（attr）：属性值为2，对应常量为Node.ATTRIBUTE_NODE
+ 文本节点（text）：属性值为3，对应常量为Node.TEXT_NODE
+ 文档片段节点（Document Fragment）：属性值为11，对应常量为Node.DOCUMENT_FRAGMENT_NODE

```js
document.nodeType  //9
document.nodeType === Node.DOCUMENT_NODE  //true
```









#### document对象



##### `document.getElementByTagName()`：

`document.getElementsByTagName`方法搜索HTML标签名，返回符合条件的元素。它的返回值是一个类似数组对象（`HTMLCollection`实例），可以实时反映HTML文档的变化。如果没有任何匹配的元素，就返回一个空集。

```js
var paras = document.getElementsByTagName('p');
```

如果传入`*`，就可以返回文档中所有HTML元素

```js
var allElements = document.getElementByTagName("*");
```





##### `document.getElementsByClassName()`

`document.getElementsByClassName`方法返回一个类似数组的对象（`HTMLCollection`实例），包括了所有`class`名字符合指定条件的元素，元素的变化实时反映在返回结果中。

```js
var elements = document.getElementsByClassName(names);
```

由于`class`是保留字，所以javascript一律使用`className`表示CSS的`class`。

参数可以是多个`class`，它们之间使用空格分隔

```js
var elements = document.getElementsByClassName("foo")
```



##### `document.getElementsByName()`

`document.getElementsByName`方法用于选择拥有`name`属性的HTML元素（比如：`<form>`、`<radio>`、`<img>`等），返回一个类似数组的对象（`NodeList`实例），因为`name`属性相同的元素可能不止一个。

```js
//表单 <form name="itbaizhan"></form>
var forms = document.getElementsByName("itbaizhan");
```



##### `document.getElementById()`

`document.getElementById`方法返回匹配指定`id`属性的节点。如果没有发现匹配的节点，则返回`null`

```js
var elem = document.getElementById("para");
```

注意：该方法的参数是大小写敏感的。比如，如果某个节点的`id`属性是`main`，那么`document.getElementById("Main")`会返回`null`.





##### `document.querySelector()`

`document.querySelector()`方法接受一个CSS选择器作为参数，返回匹配该选择器的元素节点。**如果有多个节点满足匹配条件，则返回第一个匹配的节点。**如果没有发现匹配的节点，则会返回`null`

```js
var el1 = document.querySelector(".myclass");
```





##### `document.querySelectorAll()`

`document.querySelectorAll`方法与`querySelector`用法类似，区别是返回一个`NodeList`对象，**包含所有匹配给定选择器的节点**

```js
var elementList = document.querySelectorAll(".myclass");
```





##### `document.creatElement()`

`document.createElement`方法用来生成元素节点，并返回该节点

```js
var newDiv = document.createElement("div");
```





##### `document.createTextNode()`

`document.createTextNode`方法用来生成文本节点（`Text`实例），并返回该节点。它的参数是文本节点的内容

```js
var newDiv = document.createElement("div");
var newContent = document.createTextNode("hello");
newDiv.appendChild(newContent);
```





##### `document.createAttribute()`

`document.createAttribute()`方法生成一个新的属性节点（`Attr`实例），并返回它。

```js
var attribute = document.createAttribute(name);

var root = document.getElementById("root");
var it = document.createAttribute("itbaizhan");
it.value = 'it';
root.setAttributeNode(it);
```







#### Element对象

Element对象对应网页的HTML元素。每一个HTML元素，在DOM树上都会转换成一个Element节点对象。



##### `Element.id`

`Element.id`属性返回指定元素的`id`属性，该属性可读写

```js
//HTML 代码为 <p id="foo"></p>
var p = document.querySelector("p");
p.id //"foo"
```





##### `Element.className`

`className`属性用来读写当前元素节点的`class`属性。它的值是一个字符粗，每个`class`之间用空格分割

```js
//HTML 代码 <div class="one two three" id="myDiv"></div>
var div = document.getElementById("myDiv");
div.className
```





##### `Element.classList`

`classList`对象有下列方法：

+ `add()`：增加一个class
+ `remove()`：移除一个class
+ `contains()`：检查当前元素是否包含某个class
+ `toggle()`：将某个class移入或移出当前元素

```js
var div = document.getElementById("myDiv");
div.classList.add("myCssClass");
div.classList.add("foo","bar");
div.classList.remove("myCssClass");
div.classList.toggle("myCssClass"); //如果myCssClass不存在就加入，否则移除
div.classList.contains("myCssClass"); //返回true或者false
```





##### `Element.innerHTML`

`Element.innerHTML`属性返回一个字符串，等同于该元素包含的所有HTML代码。该属性可读写 ，常用来设置某个节点的内容。它能改写所有元素节点的内容，包括`<HTML>`和`<body>`元素。

```js
el.innerHTML="";
```





##### `Element.innerText`

`innerText`和`innerHTML`类似，不同的是`innerText`无法识别元素，会直接渲染成字符串。





##### Element获取元素位置

| 属性         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| clientHeight | 获取元素高度包括padding部分，但是不包括border、margin        |
| clientWidth  | 获取元素宽度包括padding部分，但是不包括border、margin        |
| scrollHeight | 元素总高度，它包括padding，但是不包括border、margin，包括溢出的不可见内容 |
| scrollWidth  | 元素总宽度，它包括padding，但是不包括border、margin，包括溢出的不可见内容 |
| scrollLeft   | 元素的水平滚动条向右滚动的像素数量                           |
| scrollTop    | 元素的垂直滚动条向下滚动的像素数量                           |
| offsetHeight | 元素CSS垂直高度（单位像素），包括元素本身的高度，padding和border |
| offsetWidth  | 元素CSS水平宽度（单位像素），包括元素本身的高度，padding和border |
| offsetLeft   | 到定位父级左边界的间距                                       |
| offsetTop    | 到定位父级上边界的间距                                       |





##### 操作css属性



**`HTML`元素的`style`属性：**

 操作CSS样式最简单的方法，使用网页元素节点的`setAttribute`方法直接操作网页元素的`style`属性

```js
div.setAttribute(
	'style',
    'background-color:red;border:1px solid black;'
);
```



**元素节点的`style`属性：**

```js
var divStyle = document.querySelector('div');

divStyle.style.backgroundColor = 'red';	
divStyle.style.border = '1px solid black';
divStyle.style.width = '100px';
divStyle.style.height = '100px';
divStyle.style.fontSize = '10em';
```



**`cssText`属性：**

```js
var divStyle = document.querySelector('div');
divStyle.style.cssText = "background-color:red;border:1px solid black;height:100px;width:100px;"
```





###  事件

事件处理程序分为：

+ HTML事件处理

  + ```html
    <body>
        <button onClick="clickHandle()">按钮</button>
        <script>
        	function clickHandle(){
                console.log("点击按钮");
            }
        </script>
    </body>
    ```



+ DOM0级事件处理

  + ```html
    <body>
        <button id="btn">按钮</button>
        <script>
            //优点：js内容和html内容分离，缺点：无法触发多个事件，写多个事件会将之前事件覆盖，只触发最后一个事件
           	var btn = document.getElementById("btn");
            btn.onclick=function clickHandle(){
                console.log("点击按钮");
            }
        </script>
    </body>
    ```



+ DOM2级事件处理

  + ```html
    <body>
        <button id="btn">按钮</button>
        <script>
            //优点：分离，并且可以触发多个事件，缺点：书写麻烦
           	var btn = document.getElementById("btn");
            btn.addEventListener("click",function(){
                console.log("点击按钮");
            })
            btn.addEventListener("click",function(){
                console.log("点击按钮");
            })
        </script>
    </body>
    ```



































# Linux教程

## linux概述

## linux安装

## linux文件系统

**Linux系统中，一切皆文件。**



### Linux目录结构

![image-20230318152820519](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230318152820519.png)

+ **bin目录：存放一些可以直接执行的常用命令，存放二进制码。目录图标右下角有个箭头表示是个链接**
+ sbin目录：存放一些系统管理员可以直接执行的常用命令，也是存放二进制码。
+ **lib目录：库目录，存放系统和应用程序动态链接库文件**
+ lib64目录：64位特殊的库文件
+ **usr目录：包含了用户级别所有东西。**
+ boot目录：引导启动所需的文件
+ dev目录：设备目录，管理所有设备
+ **etc目录：存放系统管理的配置文件**
+ home目录：**每个普通用户自己的主目录**
+ root目录：**系统管理员的用户主目录**
+ opt目录：==linux给第三方软件包留下的存储位置==
+ media目录：可以识别一些可移动媒体设备，如U盘、光驱 挂载到media。
+ mnt目录：挂载目录，跟media类似，将外部存储挂载mnt
+ proc目录：系统进程目录，存放一些硬件和当前进程的信息
+ run目录：运行目录，存放系统运行的临时信息
+ srv目录：存放系统服务相关内容
+ sys目录：存放系统硬件信息的文件
+ tmp目录：**临时目录，临时存放的东西可以放在这里**
+ var目录：可变目录，存放一些可以扩充内容目录，如日志。













## vim编辑器

### vim/vi

#### vi

vi是unix操作系统和类uinx操作系统中最通用的文本编辑器。

#### vim

vim编辑器是从vi发展而来的，可以主动的以字体颜色辨别语法的正确性，方便程序设计。vim和vi完全兼容。



### vim一般模式

**一般模式主要操作：删除、复制、粘贴。**

![image-20230318162829755](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230318162829755.png)



### vim编辑模式

![image-20230318164924755](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230318164924755.png)





### vim指令模式

![image-20230318165013986](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230318165013986.png)





### 模式之间转换

![image-20230318161850380](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230318161850380.png)







## 网络配置

**VMware提供了三种网络连接模式**：

+ 桥接模式：虚拟机直接连接外部物理网络的模式，主机起到了网桥的作用。这种模式下，虚拟机可以直接访问外部网络，并且对外部网络是可见的。
+ **NAT模式**：虚拟机和主机构建一个专用网络，并通过虚拟网络地址转换（NAT）设备对IP进行转换。虚拟机通过共享主机IP可以访问外部网络，但是外部网络无法访问虚拟机。
+ 仅主机模式：虚拟机只与主机共享一个专用网络，与外部网络无法通信。





**修改静态IP：**

```shell
vim /etc/sysconfig/network-scripts/ifcfg-ens33


# 修改
BOOTPROTO="static"

# 增加IP地址
IPADDR=192.168.0.130
# 增加网关
GATEWAY=192.168.0.2
# 增加域名解析器
DNS1=192.168.0.2
```





**配置主机名：**

```shell
# 在/etc/hostname 可以修改主机名
vim /etc/hostname

# 编辑完成后，需要重启才能生效

# 另一种不需要重启就可以修改主机名的方法：
hostnamectl set-hostname 主机名


# 修改hosts文件映射文件，可以将主机名和ip地址进行一一对应
vim /etc/hosts

# 如增加下面的ip和主机名的映射
ip地址  主机名
192.168.0.130 kk
```









## 远程登录

**使用window的自带远程连接：**

```shell
ssh 主机名@ip地址
ssh root@192.168.0.130

回车，然后yes，最后输入密码就能连接上了
```



**使用工具远程登录：**

​	常用命令行工具：Xshell，SecureCRT

​	常用图形远程连接工具：teamviewer ，VNC



 



## 系统管理



### linux中的进程和服务

+ 计算机中，一个**正在执行的程序或命令**，被叫做 “进程”（process）；
+ 启动之后，一个**常驻内存的进程**，一般称为“服务”（service）；



### systemctl

```shell
# 利用systemctl 管理服务
systemctl start|stop|restart|status  服务名
```







### 关机和重启

```shell
# 默认shutdown命令是一分钟后关机
shutdown

# 在没有关机前，可以通过命令取消关机
shutdown -c

# 立刻关机
shutdown now


# 将数据从内存同步到硬盘
sync

# 停机，关闭系统，但不断电
halt

# 关机断电
poweroff

# 重启 等同于 shutdown -r now    其中 -r相当于reboot意思
reboot

# shutdown [选项] 时间   常见选项： -H 相当于--halt，停机  -p 相当于--poweroff  -r 相当于-r=reboot重启
# 时间是分钟，如果是数字表示分钟，如果是15：32 表示某个时刻
shutdown [选项] 时间
```







### 关闭防火墙

#### 临时关闭防火墙

```shell
systemctl status firewalld   # 查看防火墙状态
systemctl stop firewalld  # 临时关闭防火墙
```



#### 开机启动时关闭防火墙

```shell
systemctl enable firewalld.service  # 查看防火墙开机启动状态
systemctl disable firewalld.service  # 设置开机时关闭防火墙
```







## shell 命令整体介绍及帮助命令

### 帮助命令



#### man获得帮助信息

语法：`man 命令` 获取对应命令的全部帮助信息

或者：`外部命令 --help` 获取命令的部分帮助信息



#### help获得shell内置命令的帮助信息

语法：`hlep 命令` 获取shell内置命令的帮助信息



#### 如何判断是否为内嵌命令

语法：`type 命令` 只要显示为shell内嵌命令则为内嵌命令，否则，则不是内嵌命令



#### 常用快捷键

| 快捷键 | 功能                                 |
| ------ | ------------------------------------ |
| ctrl+c | 停止进程                             |
| ctrl+l | 清屏，等同于clear；彻底清屏是：reset |
| tab    | 提示                                 |
| 上下键 | 查找执行过的命令                     |





### 文件项目类

#### pwd 显示当前工作目录的绝对路径

```shell
pwd   # 显示当前工作目录的绝对路径
```



#### ls 列出目录内容

```shell
ls [选项]     # 列出目录内容

# 常见选项
-a 全部文件，连同隐藏档（开头为.的文件）一起列出来（常用）
-l 长数据串列出，包含文件的属性与权限等等数据    ls -l 相当于 ll
```





#### cd 切换目录

```shell
cd   # 切换目录

# 相对路径
例如：cd home 表示使用相对路径

# 绝对路径
例如：cd /home 表示使用相对路径  其中 / 表示根目录
```





#### mkdir 创建一个新目录

```shell
mkdir 文件名  # 创建文件夹

示例：
mkdir c   # 表示在当前目录下创建了一个c目录

mkdir -p  e/f/g  # -p参数表示父级，如果父目录不存在就会自动创建，利用该参数可以多级创建
```





#### rmdir 删除一个空目录

```shell
rmdir 文件名 # 删除文件夹

示例：
rmdir c  # 表示删除当前目录下的一个c目录

rm -p e/f/g # 表示将多级目录都删除，先删除g，再删除f，最后删除e
```





#### touch 创建空文件

```shell
touch 文件名称   # 创建一个空文件

示例：
touch hello  # 表示创建一个hello的文本文件，在linux中，文件可以不带后缀名，不带后缀名的文件默认为文本文件
touch /home/huihui/hello  # 文件名也可以包含路径，表示在该路径下创建文件
```





#### cp 复制文件或目录

```shell
cp [选项] source dest  # 将source源文件复制到dest目标文件

# 注意：如果dest是路径，则会将source文件拷贝到该路径下；如果dest是文件，则会将dest文件内容替换成source文件中的内容

常用选项：
-r # 表示递归父复制整个文件夹，即将整个文件夹中的所有内容全部复制过去
```





#### rm删除文件或目录

```shell
rm [选项] deleteFile  # 递归删除目录中的所有内容

rm deleteFile # 不加选项，默认是删除文件，而不能删除文件夹，因此，删除文件夹需要加入选项

示例：
rm -rf a/*  # 表示将a目录下的所有文件都是删除，但是a目录还是存在的，* 表示通配符
rm -rf a/  # 表示将a目录下所有内容删除，a目录也会删除

常用的选项：
-r 表示递归删除目录中的所有内容，包括目录
-f 强制执行删除操作，而不提示用于确认的信息
-v 显示指令的详细执行过程
```







#### mv 移动文件与目录或重命名

```shell
mv 源文件 新文件  # 如果是新文件，即相当于将文件移动到某个目录并进行重命名

示例：
mv hello ./a/b/hello1  # 表示将文件移动到a/b目录下，并将该移动的文件重命名为hello1

mv 源文件 目录 #表示将源文件移动到该目录下

示例：
mv hello ./a/b/
```







#### cat 查看文件内容

```shell
# 查看文件内容，从第一行开始显示，一般用于查看比较小的文件，即在一个屏幕上能显示全的。
cat [选项] 要查看的文件

常用的选项：
-n  显示所有行的行号，包括空行
```







#### more 文件内容分屏查看器

`more`指令是一个基于VI编辑器的文本过滤器，他以**全屏幕的方式按页显示文本文件的内容**。`more`指令中内置了若干快捷键。

```shell
more 文件名
```

```shell
# more 指令的常用快捷键操作

空格space  代表向下翻一页
Enter  代表向下翻一行
q  代表立刻离开more，不再显示该文件内容
ctrl + f  向下滚动一屏
ctrl + b 返回上一屏
=  输出当前行的行号
:f  输出文件名和当前行的行号
```







#### less 分屏显示文件内容

`less`指令用来**分屏查看文件内容**，它的功能与more指令类似，但是比`more`指令更加强大，支持各种显示终端。`less`指令在显示文件内容时，并不是依次将整个文件加载之后才显示，而是根据显示需要加载的内容，对于显示大型文件具有较高的效率。

```shell
less 文件名
```

```shell
# less 指令的常用快捷键操作

空格space  向下翻动一页
pagedown  向下翻动一页
pageup  向上翻动一页
Enter  代表向下翻一行
shift + G   翻到结尾
g  翻到开头

/字串  向下搜寻[字串]的功能；n:向下查找；N：向上查找
？字串  向上搜寻[字串]的功能；n:向上查找；N：向下查找
```







#### echo

```shell
# echo 输出内容到控制台
echo [选项] [输出内容]

选项：
-e  支持反斜杠控制的字符转换
```

| 控制字符 | 作用                |
| -------- | ------------------- |
| `\\`     | 输出`\`本身         |
| `\n`     | 换行符              |
| `\t`     | 制表符，也就是Tab键 |







#### head显示文件头部内容

```shell
# head用于显示文件的开头部分内容，默认情况下head指令显示文件的前10行内容

head 文件名  # 查看文件头的10行内容

head -n 5 文件名  # 查看文件头5行内容，5可以是任意行数

选项：
-n 行数  表示指定显示头部内容的行数
```







#### tail 输出文件尾部内容

```shell
# tail用于输出文件中的尾部内容，默认情况下tail指令显示文件的后10行内容

tail 文件名  # 查看文件尾部10行内容

tail -n 5 文件名  # 查看文件尾部5行内容，5可以是任意行数

tail -f 文件名  # 实时追踪文档的所有更新

选项：
-n 行数  # 输出文件尾部行数内容
-f  # 显示文件最新追加的内容，监视文件变化
```







#### `>`输出重定向和`>>`追加

```shell
# > 覆盖
ls -l > 文件(a.txt)  #表示将列表的内容写入文件a.txt中
cat 文件1 > 文件2   # 将文件1的内容覆盖到文件2中

# >> 追加
ls -al >> 文件(aa.txt)  # 列表的内容追加到文件aa.txt的末尾
echo "内容" >> 文件  # 将 字符串的内容 追加到 文件中
```





#### ln软链接

**软链接也称为符号链接**，==类似于windows里的快捷键方式==，有自己的数据块，主要存放了链接其他文件的路径。

```shell
ln -s [原文件或目录] [软链接名]  # 给原文件创建一个软链接

注意：
删除软链接：rm -rf 软链接名，而不是 rm -rf 软链接名/
如果使用 rm -rf 软链接名/ ,则会将软链接对应的真实目录下的内容删掉

查询：通过ll可以查看，尾部会有位置指向
```







#### history查看已经执行过历史命令

```shell
history   # 查看已经执行过的历史命令

history -c # 表示将所有历史记录删除
```







### 时间日期类

#### date显示当前时间

```shell
date  # 显示当前时间
date +%Y  # 显示当前年份
date +%m  # 显示当前月份
date +%d  # 显示当前是哪一天
date +"%Y-%m-%d %H:%M:%S"   # 显示年月日时分秒
date +%s  # 当前时间戳（秒）
```



#### date显示非当前时间

```shell
date -d"1 days ago"  # 显示前一天时间
date -d"-1 days ago"  #显示明天时间
```





#### date设置系统时间

```shell
date -s "2023-03-12 23:23:23"   # 设置系统时间

ntpdate 203.107.6.88    # 依据阿里云NTP服务器来同步时间
```





#### cal查看日历

```shell
cal # 显示本月日历
cal -y #显示本年的日历
cal 2023  # 显示某年的日历
```





### 用户权限类

#### 用户管理命令

##### useradd添加用户

```shell
useradd 用户名   # 添加新用户
useradd -g 组名 用户名  # 添加新用户到某个组
```



##### passwd设置用户密码 

```shell
passwd 用户名  # 设置用户密码
```





##### 删除一个用户

```shell
userdel 用户名  # 删除用户，但 home目录下的用户目录不会删除

userdel -r 用户名  # 删除用户，并删除home目录下的用户目录
```



##### id查看用户

```shell
id 用户名  # 可以查看用户是否存在，用户id，用户组id
```

 



##### cat  /etc/passwd查看创建了哪些用户

```shell
cat /etc/passwd # 查看创建了哪些用户
```





##### su 切换用户

```shell
su 用户名 # 切换用户，只能获得用户的执行权限，不能获得环境变量
su -用户名  # 切换到用户并获得该用户的环境变量及执行权限
```



##### 查看当前用户

```shell 
whoami   # 查看当前用户名
```





##### sudo 设置普通用户具有root权限

```shell
如果给普通用户，临时赋予管理员权限，需要使用sudo

例如，当前是huihui普通用户，查看root目录，此时权限不够
ls /root  # 权限不够
sudo ls /root  # 赋予管理员权限

但是直接使用 sudo ls /root 命令还是存在问题，因为还需要在 /etc/sudoers文件中配置对应的用户
/etc/sudoers文件
## Allow root to run any commands anywhere
root    ALL=(ALL)  ALL
huihui  ALL=(ALL)  ALL


修改完/etc/sudoers文件后，此时在sudo ls /root 命令，就能查看root目录中的文件了
```





##### usermod修改用户

```shell
usermod -g 用户组 用户名  # 修改用户的初始登陆组，给定的组必须存在。默认组id为1
```





#### 用户组管理命令

每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。不同linux系统对用户组的规定是有所不同，如linux下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。



##### groupadd新增组

```shell
groupadd 组名  # 增加组
```





##### groupdel删除组

```shell 
groupdel 组名 # 删除组
```



##### groupmod修改组

```shell
groupmod -n 新组名 旧组名  # 对旧组名进行重命名
```



##### 查看创建了哪些用户组

```shell
cat /etc/group  # 用户组的信息在/etc/group文件中
```





#### 文件权限类

##### 文件属性

linux系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。为了保护系统的安全性，linux系统对不同的用户访问同一文件（包括目录文件）的权限做了不同的规定。在linux中我们可以使用`ll`或`ls -l`命令来显示一个文件的属性以及文件所属的用户和组。

![image-20230326142731631](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230326142731631.png)





![image-20230326142935068](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230326142935068.png)





![image-20230326151759419](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230326151759419.png)







#### chmod改变权限

![image-20230326152306738](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230326152306738.png)

```shell
chmod u=rw 文件名 # 表示给文件的属主 赋予 r（读）w（写）权限
chmod u+r 文件名  # 表示给文件的属主 增加 r（读）权限
chmod g-w 文件名  # 表示给文件的属组 减去 w（执行）权限
chmod o-rw 文件名  # 表示给文件的其他用户 减去 r（读）w（写）权限
chmod a+rwx 文件名  # 表示给文件的属主、属组、其他用户 都增加 r（读）w（写）x（执行）权限
```





```shell
# 用数字表示的形式
1 --x
2 -w-
3 -wx
4 r--
5 r-x
6 rw-
7 rwx

chmod 777 文件名 # 表示给ugo分别对应是7，即u=rwx，g=rwx，a=rwx

chmod -R 777 目录 # 表示给该目录和该目录下的所有文件和目录都赋予 rwx 权限
```









#### chown改变所有者

```shell
chown 新属主（用户名） 文件名  # 表示指定新属主

chown -R 新属主 目录  # 表示给该目录和该目录下的所有文件都更改为新属主
```







#### chgrp改变所属组

```shell
chown 新属组 文件名  # 表示给该文件指定新属组

chown -R 新属组 目录  # 表示给该目录和该目录下的所有文件都更改为新属组
```







### 搜索查找类

#### find查找文件或者目录

find指令将从指定目录向下递归地遍历其各个子目录，将满足条件的文件显示在终端。

```shell
find [搜索范围] [选项]

搜索范围：
如果不给搜索范围，则会从当前目录开始查询
如果指定目录，则会从指定目录下开始查询


常用选项：
-name  按照指定地文件名查找文件
-user  查找属于指定用户名的所有文件
-size  按照指定的文件大小查找文件，单位：b（块-512字节），c（字节），w（字-2字节），k（千字节），M（兆字节），G（吉字节）


示例：
find /root -name info
find -name info
find /root -name "*.cfg"  # * 表示通配符

find /root -size +10M  # 表示查找大于10M的文件 
```





#### locate快速定位路径文件

locate指令利用事先建立的系统中所有文件名称及路径的locate数据库实现快速定位给定的文件。locate指令无需遍历整个文件系统，查询快速。为了保证查询结果的准确度，管理员必须定期更新locate时刻。

```shell
locate 文件名

注意：由于locate指令基于数据库查找，所以在第一次运行前，必须使用updatedb指令创建一个locate数据库

还需注意：数据库会每天更新一次，所以在查找前，需要先updatedb，对数据库进行更新，然后再用locate 命令查找

# 下面两个指令最好一起使用
updatedb
locate tmp
```





#### grep过滤查找及“|”管道符

管道符，“|”，表示将前一个命令的处理结果（输出）作为后一个命令的输入

```shell
grep 选项 查找内容 源文件   # 从文件内容中查找某个内容

选项：
-n  # 显示匹配行及行号

示例：

grep -n boot initial-setup-ks.cfg # 表示从initial-setup-ks.cfg文件内容查找boot关键字


ls | grep -n test # ls的输出结果作为grep 命令的输入，再从其中过滤查询
```





### 压缩解压类

#### gzip/gunzip压缩（不推荐使用）

```shell
gzip 文件
gunzip 文件.gz
```

**注意**：gzip/gunzip压缩

+ 只能压缩文件而不能压缩目录
+ 不保留原来的文件
+ 同时多个文件会产生多个压缩包





#### zip/unzip压缩

```shell
zip [选项] 压缩后的压缩包名(.zip) 要压缩的文件或目录 # 压缩文件和目录
unzip [选项] 压缩包（xxx.zip） # 解压文件或目录

zip 选项：
-r   # 压缩目录

unzip 选项：
-d 指定目录  # 指定解压后文件的存放目录
```

注意：zip压缩命令再window中和linux中都是通用的，**可以压缩目录且保留源文件**。





#### tar打包

```shell
tar [选项] 压缩后的压缩包名（xxx.tar.gz） 要压缩的文件或目录  # 打包目录，压缩后的文件格式.tar.gz

常用选项：
-c  产生.tar打包文件
-v  显示详细信息
-f  指定压缩后的文件名
-z  打包同时压缩
-x  解压.tar文件
-C  解压到指定目录


示例：

# 压缩多个文件
tar -zcvf temp.tar.gz  initial-setup-ks.cfg  xzhdx.txt info

# 压缩目录
tar -zcvf temp.tar.gz  目录1 目录2 ...

# 解压
tar -zxvf 压缩文件（.tar.gz）  [-C 指定目录]   # [-C 指定目录] 为可选，否则会解压到当前目录下
```







### 磁盘管理类



#### du查看文件和目录占用的磁盘空间

```shell
du 目录/文件  # 显示目录下每个子目录的磁盘使用情况

常用选项：
-h   # 以较易阅读的GBytes，MBytes，KBytes等格式显示
-a   # 不仅查看子目录大小，还包括文件
-c   # 显示所有的文件和子目录大小后，显示总和
-s   # 只显示总和
--max-depth=n   # 指定统计目录的深度为第n层


示例：
du -sh  # 查看当前目录占用的磁盘空间大小
```





#### df查看磁盘使用情况

```shell
df [选项]  # 列出文件系统的整体磁盘使用量，检查文件系统的磁盘空间占用情况

选项：
-h   # 以较易阅读的GBytes，MBytes，KBytes等格式显示

示例：
df -h # 一般加上-h，查看磁盘使用情况
```





#### free查看内存使用情况

```shell
free -h  # 查看内存使用情况
```









#### lsblk查看设备挂载情况

```shell
lsblk  # 查看设备挂载情况

lsblk -f  #显示文件系统信息
```







#### mount/umout 挂载/卸载

linux中每个分区都是用来组成整个文件系统的一部分，它在用一种叫做 "挂载" 的处理方法，它整个文件系统中包含了一整套的文件和目录，并将一个分区和一个目录联系起来，要载入的那个分区将使它的存储空间在这个目录下获得。 



详细内容查看`尚硅谷高级技术之Linux(CentOS7.9).pdf`





#### fdisk分区

```shell
fdisk -l  # 查看磁盘分区详情

fdisk 硬盘设备名  # 对新增硬盘进行分区操作
```



详细内容查看`尚硅谷高级技术之Linux(CentOS7.9).pdf`





### 进程管理类

#### ps查看当前系统进程状态

```shell
ps 常用选项：
a  # 列出带有终端的所有用户的进程
x  # 列出当前用户的所有进程，包括没有终端的进程
u  # 面向用户友好的显示风格
-e  # 列出所有进程
-u  # 列出某个用户关联的所有进程
-f  # 显示完整格式的进程列表


ps aux | grep xxx  # 查看系统中所有的进程

ps -ef | grep xxx  # 可以查看子父进程之间的关系
```



详细内容查看`尚硅谷高级技术之Linux(CentOS7.9).pdf`



#### kill终止进程

```shell
kill [选项] 进程号  # 通过进程号杀死进程

killall 进程名称   # 通过进程名称杀死进程，也支持通配符

选项：
-9  #表示强迫进程立即停止


示例：
kill -9 PID   # 通过PID杀死进程
killall firefox  # 通过进程名杀死进程
```





#### pstree查看进程树

```shell
pstree [选项]

常用选项：
-p   # 显示进程的PID
-u   # 显示进程的所属用户
```





#### top查看系统健康状态

```shell
top [选项]

选项：
-d 秒数   # 指定top命令每隔几秒更新。默认是3秒在top命令的交互模式当中可以执行的命令
-i  # 使top不显示任何闲置或僵死进程
-p  # 通过指定监控进程ID来仅仅监控某个进程的状态
```







#### netstat显示网络统计信息和端口占用情况

```shell
netstat -anp | grep 进程号  # 查看该进程网络信息

netstat -nlp | grep 端口号  # 查看网络端口号占用情况

常用选项： 
-a  # 显示所有在监听和未监听的套接字
-n  # 拒绝显示别名，能显示数字的全部转换成数字
-l  # 仅列出在监听的服务状态
-p  # 表示显示哪个进程在调用
```













### 系统定时任务



#### crontab服务管理

```shell
# 重新启动crond服务
systemctl restart crond
```







#### crontab定时任务设置

```shell
crontab [选项]

选项：
-e  # 编辑crontab定时任务
-l  # 查询crontab任务
-r  # 删除当前用户所有的crontab任务
```



详细内容查看`尚硅谷高级技术之Linux(CentOS7.9).pdf`







### 软件包管理

#### RPM

RPM（RedHat Package Manager），RedHat软件包管理工具，类似window里面的setup.exe 是linux这系列操作系统里面的打包安装工具，它虽然是RedHat的标志，但理念相同。



#### RPM查询命令

```shell
rpm -qa  # 查询所安装的所有rpm软件包

由于软件包比较多，一般都会过滤。 rpm -qa | grep rpm软件包

示例：
rpm -qa | grep firefox

rpm -qi rpm软件名  # 查看软件安装详细信息
```







#### RPM卸载命令

```shell
rpm -e RPM软件包   # 卸载软件包

选项：
-e       # 卸载
--nodeps # 安装前不检查依赖，不推荐用

示例：
rpm -e firefox
```





#### RPM安装命令

```shell
rpm -ivh RPM包全名   # 安装软件包

选项：
-i  # install,安装
-v  # --verbose，显示详细信息
-h  # --hash，进度条
--nodeps # 安装前不检查依赖，不推荐用

示例：
```







#### YUM

YUM（Yellow dog Updater，Modified）是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理。**基于RPM包管理**，能够从指定的服务器自动下载RPM包并安装，**可以自动处理依赖关系**，并且**一次安装所有依赖的软件包**，无需繁琐地一次下载再安装。



#### YUM的常用命令

```shell
yum [选项] 参数 软件名

选项：
-y   # 对所有提问都回答“yes”

参数：
install  # 安装rpm软件包
update  # 更新rpm软件包
check-update  # 检查是否有可用的更新rpm软件包
list  # 显示软件包信息
clean  # 清理yum过期的缓存
deplist  # 显示yum软件包的所有依赖关系
```





#### 修改网络YUM源

默认的系统YUM源，需要连接国外apache网站，网速比较慢，可以修改关联的网络YUM源为国内镜像的网站，比如网易163，aliyun等。



详细内容查看`尚硅谷高级技术之Linux(CentOS7.9).pdf`







### 克隆虚拟机



详细内容查看`尚硅谷高级技术之Linux(CentOS7.9).pdf`















### shell编程

shell是一个命令行解释器，它接收应用程序/用户命令，然后调用操作系统内核。

shell还是一个功能相当强大的编程语言，易编写、易调试、灵活性强。





#### shell脚本

shell脚本文件一般为`xxx.sh`，其实不以`.sh`为结尾也可以，但是必须是shell语言。

```shell
#！/bin/bash   # 指定解析器，这一行必须有
```



脚本执行：

```shell
# 第一种方式：推荐
bash 脚本文件的相对路径或脚本的绝对路劲

示例：
bash hello.sh


# 第二种方式：
脚本文件的相对路径或脚本的绝对路劲   # 可以省略bash
hello.sh  # 在当前目录下，执行hello.sh会报错
./hello.sh  # 这样没有问题
/root/shell/hello.sh # 绝对路径也没有问题
```







#### 变量

##### 系统预定义变量

常用系统变量：`$HOME、$PWD、$SHELL、$USER`等



可以通过`set`命令显示当前shell中的所有变量。

可以通过`env`命令可以显示系统全局环境变量





##### 自定义变量

**定义变量**：变量名=变量值，==**注意：=号前后不能有空格**==

```shell
a="hello world"
echo $a   # 输出结果为：hello world
```



**撤销变量**：unset 变量名

```shell
a="hello world"
unset a
echo $a #由于撤销变量了，所以什么都不输出
```



**声明全局变量**：export 变量名

```shell
my_var="hello world"
export my_var  # 此时，my_var就是全局变量了
```



**声明静态变量（只读变量）**：readonly 变量，==**注意：不能unset**==

```shell
readonly b=5
echo $b  # 输出结果为：5
b=10  # 这里会报错，会提示：b是只读变量，不能更改
```



**变量命名规则**：

+ 变量名称可以由字母、数字和下划线组成，但是不能以数字开头，**环境变量名建议大写。**
+ **等号两侧不能有空格**
+ 在bash中，**变量默认类型都是字符串类型**，无法直接进行数值运算
+ 变量的值如果有空格，需要使用双引号或单引号括起来。





##### 特殊变量



```shell
$n   # n为数字，$0代表该脚本名称，$1-$9代表第一到第九个参数，十以上的参数需要用大括号包裹，如：${10}

$#  # 获取所有输入参数个数，常用于循环，判断参数的个数是否正确以及加强脚本的健壮性

$*  # 这个变量代表命令行中所有参数，$*把所有的参数看成一个整体

$@  # 这个变量也代表命令行中所有的参数，不过$@把每个参数区分对待

$?  # 最后一次执行命令的返回状态。如果值为0，则表示上一个命令正确；如果值非0，则表示上一个命令错误
```







#### 运算符

```shell
在linux中不能直接将两个数进行加减乘除运算，会将整体看成一个字符串

因此，进行运算，必须使用运算符：$(()) 或 $[]

示例：
s=$[1+2]
echo s  # 输出结果为；3

s1=$((2*3))
echo s1  # 输出结果为：6

s2=$[(2+1)*3]
echo s2  # 输出结果为：9
```







#### 条件判断

[ condition ]   ==注意：condition前后必须有空格==

```shell
# 第一种方式：test condition ，注意：判断条件中的=或其他符号 左右两侧也要空格
a="hello"
test a = "hello"
echo $?  #输出0，表示上一条命令是正确的

# 第二种方式：[ condition ]  注意：判断条件中的=或其他判断符号 左右两侧也要空格
[ a = "hello" ]  
echo $?  #输出0，这种方式也行
```





**常用的判断条件**：

+ -eq  等于
+ -ne  不等于
+ -lt  小于
+ -le  小于等于
+ -gt  大于
+ -ge  大于等于
+ 如果是字符串之间进行比较，= 判断两个字符串相等，!= 判断两个字符串不相等

```shell
[ 2 -ne 3]
echo $?  # 输出：0
```





**按照文件权限进行判断**：

+ -r    有读的权限
+ -w   有写的权限
+ -x   有执行的权限

```shell
[ -w hello.sh ]
echo $?  #输出：0
```





**按照文件类型进行判断**：

+ -e  文件存在
+ -f   文件存在并且是一个常规的文件
+ -d   文件存在并且是一个目录

```shell
[ -f /root/shell/hello.sh ]  # 绝对路径
echo $?  #输出：0
```





**多条件判断**：

+ && : 逻辑与
+ || ： 逻辑或

逻辑与和逻辑或，可以用来进行多条件判断

```shell
[ 1 -lt 2 ] && [ 2 -gt 3 ]
echo $?  # 输出结果：1，上一条命令是错误的
```





#### 流程控制

##### if判断

```shell
# 单分支
if [ 条件判断式 ];then
	程序
fi

或

if [ 条件判断式 ]
then
	程序
fi



# 多分支
if [ 条件判断式 ]
then
	程序
elif [ 条件判断式 ]
then
	程序
else
	程序
fi	
```





##### case语句

```shell
case $变量名 in
"值1")
	如果变量的值等于值1，则执行程序1
;;
"值2")
	如果变量的值等于值2，则执行程序2
;;
	.......
*)
	如果变量的值都不是以上的值，则执行此程序
;;
esac
```

注意事项：

+ case行尾必须为关键字“in”，每个模式匹配必须以右括号“）”结束。
+ 双分号“;;”表示命令序列结束，相当于java中的break
+ 最后的“*)”表示默认模式，相当于java中的default。





##### for循环

```shell
for ((初始值;循环控制条件;变量变化))
do
	程序
done
```



shell脚本文件：

```shell
#!/bin/bash

sum=0
for((i=0;i<=100;i++))
do
	sum=$[$sum+$i]  # 这里是对变量值相加，$sum 表示变量sum的值
done
echo $sum
```

注意：在shell脚本文件中，在for循环中, 可以使用`i++、i<=100`





##### while循环

```shell
while [ 条件表达式 ]
do
	程序
done
```



shell脚本示例：

```shell
#!/bin/bash
sum=0
i=1
while [  $i<=100 ]
do
	sum=$[ $sum+$i ]
	i++
done
echo $sum
```







#### read读取控制台输入

详细内容查看`尚硅谷高级技术之Linux(CentOS7.9).pdf`



```shell
read [选项] 参数

选项：
-p   # 指定读取值的提示符
-t   # 设置超时时间，如果在指定时间内没有输入，则会自动退出

参数:
	变量：指定读取值的变量名
```



shell示例：

```shell
#!/bin/bash
read -t 10 -p "请输入姓名：" name #-t设置输入时间，-p设置提示
echo "welcome,$name"
```









#### 函数

详细内容查看`尚硅谷高级技术之Linux(CentOS7.9).pdf`

##### 系统函数



**basename**：

```shell
basename [string/pathname] [suffix]  # basename命令会删除所有的前缀包括最后一个‘/’字符，然后将字符串显示出来

basename可以理解为取路径里的文件名称

选项：
suffix为后缀，如果suffix被制定了，basename会将pathname或string中的suffix去掉
```



示例:

```shell
basename /home/atguigu/banzhang.txt    # 命令执行后的结果为：banzhang.txt
```





**dirname**：

```shell
dirname 文件绝对路径  # 从给定的包含绝对路径的文件名中去除文件名（非目录的部分），然后返回剩下的路径（目录的部分）

dirname 可以理解为获取文件路径的绝对路径名称
```



示例：

```shell
dirname /home/atguigu/banzhang.txt     # 命令执行后的结果为：/home/atguigu
```





##### 自定义函数



```shell
function funname()
{
	Action;
	[return int;]
}
```

注意：

+ 必须在调用函数地方之前，先声明函数，shell 脚本是逐行运行。不会像其它语言一 样先编译。
+ 函数返回值，只能通过$?系统变量获得，可以显示加：return 返回，如果不加，将 以最后一条命令运行结果，作为返回值。return 后跟数值 n(0-255)



示例：计算两个输入参数的和

```shell
[atguigu@hadoop101 shells]$ touch fun.sh
[atguigu@hadoop101 shells]$ vim fun.sh
#!/bin/bash
function sum()
{
s=0
s=$[$1+$2]
echo "$s"
}
read -p "Please input the number1: " n1;
read -p "Please input the number2: " n2;
sum $n1 $n2;
[atguigu@hadoop101 shells]$ chmod 777 fun.sh
[atguigu@hadoop101 shells]$ ./fun.sh
Please input the number1: 2
Please input the number2: 5
7
```





#### 正则表达式入门

正则表达式使用单个字符串来描述、匹配一系列符合某个语法规则的字符串。在很多文 本编辑器里，正则表达式通常被用来检索、替换那些符合某个模式的文本。在 Linux 中，grep， sed，awk 等文本处理工具都支持通过正则表达式进行模式匹配。



详细内容查看`尚硅谷高级技术之Linux(CentOS7.9).pdf`



##### 常规匹配

```shell
# 一串不包含特殊字符的正则表达式匹配它自己
cat /etc/passwd | grep atguigu
```





#####  常用特殊字符

###### `^`

```shell
# ^ 匹配一行的开头
cat /etc/passwd | grep ^a    # 会显示出所有以a开头的行
```





###### `$`

```shell
# $ 匹配一行的结束
cat /etc/passwd | grep t$    # 会匹配出所有以t结尾的行
```





###### `.`

```shell
# . 匹配一个任意的字符
cat /etc/passwd | grep r..t  # 会匹配出包含rabt,rbbt,rxbt,root等所有行
```





###### `*`

```shell
# * 不单独使用，它和上一个字符连用，表示匹配上一个字符0次或多次
cat /etc/passwd | grep ro*t  # 会匹配出rt，rot，root，rooot，roooooot等所有行
```





###### `[]`

```shell
# [] 表示匹配某个范围内的一个字符
# [6,8] 匹配 6 或者 8
# [0-9]  匹配一个 0-9 的数字
# [0-9]* 匹配任意长度的数字字符串
# [a-z]  匹配一个 a-z 之间的字符
# [a-z]*  匹配任意长度的字母字符串
# [a-c,e-f]  匹配 a-c 或者 e-f 之间的任意字符

cat /etc/passwd | grep r[a,b,c]*t  # 会匹配出rt,rat,rbt,rabt,rabct,rabbbaaacct等所有行
```





###### `\`

```shell
# \ 表示转义，并不会单独使用

# 注意：要使用单引号将表达式引起来
cat etc/passwd | grep 'a\$b'  # 会匹配出所有包含 a$b的行。
```







#### 文本处理工具

详细内容查看`尚硅谷高级技术之Linux(CentOS7.9).pdf`



##### cut

cut 的工作就是“剪”，具体的说就是在文件中负责剪切数据用的。cut 命令从文件的每 一行剪切字节、字符和字段并将这些字节、字符和字段输出。

```shell
cut [选项]  filename

选项：
-f  # 列号，提取第几列
-d  # 分隔符，按照指定分隔符分割列，默认是制表符“\t”
-c  # 按字符进行切割，后加n，表示取第几列
```













##### awk

一个强大的文本分析工具，把文件逐行的读入，以空格为默认分隔符将每行切片，切开 的部分再进行分析处理。

```shell
awk [选项] '/pattern1/{action1} /pattern2/{action2}...' filename

pattern  # 表示 awk 在数据中查找的内容，就是匹配
action  # 在找到匹配内容时所执行的一系列命令

选项：
-F  # 指定输入文件分隔符
-v  # 赋值一个用户定义变量
```



















































# 数据结构

## 线性表

线性表是具有==相同数据类型==的n个数据元素的==有序列表==





### 顺序表

**线性表顺序存储又称顺序表**，它是用==一组地址连续==的存储单元依次存储线性表中的数据元素，从而使逻辑上相邻的两个元素在物理位置上也相邻。





### 链表

顺序表可以随时存取表中的任意一个元素，但是插入和删除操作会移动大量元素。

链式存储线性表，==不需要使用地址连续的存储单元==，即不要求逻辑上相邻的元素在物理位置上也相邻，插入和删除操作不需要移动元素，而只需要修改指针，但是也失去了顺序表可以随机存取的优点



#### 单链表

```go
//单链表节点结构
type LNode struct{
    data string
    next *LNode
}
```





##### 头插法

```go
package main

import "fmt"

type LNode struct {
	data string
	next *LNode
}

func main() {

	//生成头节点
	head := LNode{
		data: "头节点",
	}
	//循环生成节点，并采用头插法插入节点
	for i := 0; i < 5; i++ {
		var str string
		fmt.Scan(&str)
		node := LNode{
			data: str,
		}
		node.next = head.next
		head.next = &node
	}
	for p := &head; p != nil; p = p.next {
		fmt.Println(p.data)
	}
}
```





##### 尾插法

```go
package main

import "fmt"

type LNode struct {
	data string
	next *LNode
}

func main() {

	//生成头节点
	head := LNode{
		data: "头节点",
	}
	r := &head
	//循环生成节点，并采用尾插法插入节点
	for i := 0; i < 5; i++ {
		var str string
		fmt.Scan(&str)
		node := LNode{
			data: str,
		}
		r.next = &node
		r = r.next
	}
	for p := &head; p != nil; p = p.next {
		fmt.Println(p.data)
	}
}
```





##### 按序号查找节点

```go
package main

import "fmt"

type LNode struct {
	data string
	next *LNode
}

func main() {

	//生成头节点
	head := LNode{
		data: "头节点",
	}
	h := &head
	//循环生成节点，并采用尾插法插入节点
	for i := 0; i < 5; i++ {
		var str string
		fmt.Scan(&str)
		node := LNode{
			data: str,
		}
		h.next = &node
		h = h.next
	}

	//按序号查找节点
	target := 3
	k := 1
	p := &head
	for p != nil && k < target {
		p = p.next
		k++
	}
	fmt.Println("查找的节点", p.data)
}
```





##### 按值查找节点

```go
按值查找节点，循环条件：指针不为空且值不一样，则指针继续向后移动，当值相等或指针为空时会退出循环
```





##### 插入节点

```go
插入节点操作，将值为x的节点插入到链表中第i个位置，先检查插入位置的合法性，然后再找到待插入位置的前驱节点，即第i-1个节点，再在其后插入新节点
```

==注意：在链表中，插入包含前插和后插，可以通过**交换数据域**来灵活插入节点==



##### 删除节点

```go
删除节点就是将单链表的第i个节点删除。先检查删除位置的合法性，后查找表中第i-1个节点，即被删除节点的前驱节点，再将其删除
```

==注意：删除前驱节点和后驱节点，同样可以使用**交换数据域方式**来灵活删除节点==



##### 求表长

```go
求表长就是计算单链表中的数据节点（不包含头节点）的个数，需要从第一个节点开始顺序依次访问表中的每个节点，为此需要设置一个计数器变量，每访问一个节点，计数器加1，直到访问到空节点为止。
```

==注意：单链表的长度不包含头节点，因此不带头节点和带头结点的单链表在求表长操作上也会略有不同。==





#### 双链表

单链表节点中只有一个指向其后继节点的指针，使得单链表只能从头节点依次顺序向后遍历。要访问某个节点的前驱节点（插入、删除操作），只能从头开始遍历。



双链表有两个指针，分别指向前驱节点和后继节点

```go
//双链表节点结构：
type DNode struct{
    data string
    prior *DNode
    next *DNode
}
```



##### 插入操作

```go
//在双链表中的节点p后面插入节点s
s.next = p.next
p.next.prior = s
p.next = s
s.prior = p
```





##### 删除操作

```go
//删除双链表中的节点p的后继节点q
p.next = q.next
q.next.prior = p
```





#### 循环单链表

循环单链表和单链表的区别在于：表中的最后一个节点的指针不是nil，而改为指向头节点，从而整个链表形成一个环。





#### 循环双链表

循环双链表和双链表区别在于：表中的最后一个节点的后继指针指向头节点，而头结点的前驱指针指向表中的最后一个节点。











## 栈和队列

### 栈

栈（stack）是一种只允许在一端进行插入或删除操作的线性表。

栈顶：线性表允许进行插入删除的一端

栈底：固定的，不允许进行插入和删除的另一端

空栈：不含任何元素的空表

==**栈的特点：先进后出**==



#### 栈结构

```go
var MaxSize = 50
type SqStack struct{  //顺序栈
    data [MaxSize]int  //MaxSize 为栈长度
    top int  //top为栈顶指针
}
```



#### 栈初始化

```go
//初始时，将栈顶设置为-1

ss := SqStack{
    top:-1,  //设置栈顶指针
}
```



#### 进栈和出栈

```go
//注意：我们设置的栈顶初始值为-1，而数组从下标0开始，所以插入时先top++，再插入元素

//进栈:栈不满时，栈顶指针先加1，再将值送到栈顶元素
ss.top++
ss.data[ss.top]=x  //x：表示要插入的元素

//出栈：栈非空时，先取栈顶元素值，再将栈顶指针减1
x:=ss.data[ss.top]
ss.top--
```



#### 判断栈是否为空

```go
if ss.top == -1 {
    栈空
}else{
    栈非空
}
```



### 共享栈

利用栈底位置相对不变的特性，可以让两个顺序栈共享一个一维数组空间，将两个栈的栈底分别设置在共享空间的两端，两个栈顶向共享空间的中间延申。

![image-20230411132934589](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230411132934589.png)

```go
//栈结构
var MaxSize = 50
type SqStack struct{
    data [MaxSize]int
    top0 int
    top1 int
}

ss := SqStack{
    top0:-1, //初始时，将0号栈的栈顶设置为-1
    top1:MaxSize, //初始时，将1号栈的栈顶设置为MaxSize
}

//0号栈进栈和出栈
top0++
ss.data[top0]=x

x:= ss.data[top0]
top0--

//1号栈进栈和出栈
top1--
ss.data[top1]=x

x:=ss.data[top1]
top1++


//判断栈空
ss.top0==-1
ss.top1==MaxSize

//判断栈满
ss.top0+1==ss.top1
```





#### 栈的链式存储结构

栈的链式存储称为链栈，链栈可以便于多个栈共享存储空间和提高效率，且不存在栈满上溢的情况。

链栈通常由单链表实现，并规定所有操作都是在单链表的表头进行



```go
//链栈的存储结构
type LinkNode struct{
    data string
    next *LinkNode
}

LinkStack := LinkNode{}

//入栈和出栈
入栈和出栈的操作都是在链表的表头进行
入栈：采用头插法，先插入的在链尾部，后插入的在链表头
出栈：从表头出栈
```







### 队列

队列（Queue）是一种操作受限的线性表，只允许在表的一端进行插入，而在表的另一端进行删除。

==**队列特点：先进先出**==

![image-20230411134136189](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230411134136189.png)





#### 队列顺序存储

队列的顺序实现是指分配一块连续的存储单元存放队列中的元素，并附带两个指针：==队头指针`front`指向队头元素，队尾指针`rear`指向队尾元素的下一个位置。==



```go
var MaxSize = 50
type SqQueue struct{  //队列存储结构
    data [MaxSize]int
    front int  //队头指针
    rear int  //队尾指针
}

sq := SqQueue{
    front:0,
    rear:0,
}

//初始状态时
sq.front == sq.rear


//进队，队不满时，先将值送入队尾，再将队尾指针加1
sq.data[sq.rear]=x
sq.rear++

//出队，队不满时，先取队头元素值，再将队头指针加1
x:= sq.data[sq.front]
sq.front++

```





#### 循环队列

循环队列：是将顺序队列臆造为一个环状的空间，即把存储队列元素的表从逻辑上视为一个环。

```go
//初始时
sq.front=sq.rear=0

//队首指针进1
sq.front=(sq.front+1)%MaxSize
//出队
x := sq.data[sq.front]
sq.front=(sq.front+1)%MaxSize


//队尾指针进1
sq.rear = (sq.rear+1)%MaxSize
//入队
sq.data[sq.rear]=x
sq.rear = (sq.rear+1)%MaxSize


//队列长度
(sq.rear+MaxSize-sq.front)%MaxSize
```





**如何区分循环队列队满还是队空？**

```go
//第一种方法,牺牲一个单元来区分队空还是队满，入队时少用一个队列单元，即约定：队头指针在队尾指针的下一个作为队满标志

队满条件：(sq.rear+1)%MaxSize == sq.front
队空条件：sq.front == sq.rear

//第二种方法：类型增设表示元素个数的数据成员
比如在队列结构中增加一个size数据成员

var MaxSize = 50
type SqQueue struct{  //队列存储结构
    data [MaxSize]int
    front int  //队头指针
    rear int  //队尾指针
    size int //队列中元素个数
}
队空条件：sq.size == 0
队满条件: sq.size == MaxSize

//第三种方法：类型中增设tag数据成员，以区分是队满还是队空

tag==0时，若因删除导致sq.front == sq.rear，则为队空
tag==1时，若因插入导致sq.front == sq.rear，则为队满

```







#### 队列链式存储

队列的链式存储，即为链队列，它实际上是一个同时带有队头指针和队尾指针的单链表。头指针指向队头节点，尾指针指向队尾节点，即单链表的最后一个节点

![image-20230411191018955](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230411191018955.png)







#### 双端队列

双端队列是允许两端都可以进行入队和出队操作的队列。其元素的逻辑结构是线性结构。**将队列的两端分别称为前端和后端，两端都可以入队和出队**。![image-20230411191554411](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230411191554411.png)



##### 输出受限的双端队列

![image-20230411191923676](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230411191923676.png)



##### 插入受限的双端队列

![image-20230411191932465](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230411191932465.png)







## 串

### 串定义

串（string）是由零个或多个字符组成的有限序列。

串中的任意个连续的字符组成的子序列称为串的子串，包含子串的串相应的称为主串。

某个字符在串中的序号称为该字符在串中的位置。

子串在主串中的位置以子串的第一个字符在主串中的位置来表示。

当两个串的长度相等且每个对应位置的字符都相等，则称这两个串是相等的。

由一个或多个空格（空格是特殊字符）组成的串称为空格串。







### 串的模式匹配

#### 简单模式匹配算法                                                                                      

```go
package main

import "fmt"

func index(str1, str2 string) int {
	i, j := 0, 0
	for i < len(str1) && j < len(str2) {
		if str1[i] == str2[j] {
            //继续比较后继字符
			i++
			j++
		} else {
            //指针后退，重新开始匹配
			i = i - j + 1
			j = 0
		}
	}
	if j >= len(str2) {
		return i - len(str2) + 1
	} else {
		return -1
	}
}
func main() {
	var str1 string = "ababcabcacbab"
	var str2 string = "abcac"
	rst := index(str1, str2)
	fmt.Println("子串位置：", rst)
}
```

暴力模式匹配算法的最坏时间复杂度为`O(nm)`，其中，`n`和`m`分别为主串和模式串的长度。







#### 改进的模式匹配算法—KMP算法







#### KMP算法进一步优化









## 树和二叉树



### 树

![image-20230425110112705](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230425110112705.png)

树是一种递归的数据结构。树作为一种逻辑结构，同时也是一种分层结构，具有以下特点：

+ 树的根节点没有前驱，除根节点外的所有节点有且仅有一个前驱
+ 树中所有节点可以有零个或多个后继

树适合表示具有层次结构的数据。树中的某个节点（除根节点外）最多和上一层的一个节点（父节点）有直接关系，根节点没有直接上层节点，因此在n个节点的树中有$n-1$条边。

+ 节点的度：树中节点的最大度数称为树的度。树中一个节点的孩子个数称为该节点的度。
+ 路径和路径长度：树中两个节点之间的路径是由这两个节点之间所经过的节点序列构成的，而路径长度是路径上所经过的边的个数。





### 二叉树

二叉树是一种树形结构，其特点是每个节点至多只有两棵子树（即二叉树中不存在度大于2的节点），并且二叉树的子树有左右之分，其次序不能任意颠倒。

注意：二叉树是一种有序树，若将其左、右子树颠倒，则成为另一棵不同的二叉树。即使树中的节点只有一棵子树，也要区分它是左子树还是右子树。



#### 特殊的二叉树

+ 满二叉树：树中的每层都含有最多的节点，满二叉树的叶节点都是集中在二叉树的最下一层，并且除叶子节点之外的每个节点度数均为2。一棵高度为$h$，且含有$2^{h}-1$个节点的二叉树。
+ 完全二叉树：高度为h，有n个节点的二叉树，当且仅当其每个节点都与高度为h的满二叉树中的编号1~n的节点一一对应时，成为完全二叉树。
+ 二叉排序树：左子树上所有节点的关键字均小于根节点的关键字；右子树上的所有关键字均大于根节点的关键字；左子树和右子树又各是一棵二叉排序树。
+ 平衡二叉树：树上任一节点的左子树和右子树的深度之差不超过1。





#### 二叉树的性质

+ 非空二叉树上的叶子节点数等于度为2的节点数加1，即 $n_{0}=n_{2}+1$

+ 非空二叉树上第 k 层上至多有 $2^{k-1}$ 个节点。
+ 高度为 h 的二叉树至多有 $2^{h}-1$。
+ 对完全二叉树按从上到下、从左到右的顺序依次编号$1,2,...,n$，当双亲编号为 $i$ ，则左孩子编号为 $2i$，右孩子的编号为 $2i+1$。
+ 具有n个（n>0）节点的完全二叉树的高度为 $\lceil log_{2}(n+1) \rceil$或 $\lfloor log_{2}n \rfloor + 1$



#### 二叉树的顺序存储结构

二叉树的顺序存储是指用一组地址连续的存储单元依次自上而下、自左至右存储完全二叉树上的节点元素，即将完全二叉树上的编号为 $i$ 的节点元素存储在一维数组下标为 $i-1$ 的分量中。



注意：完全二叉树和满二叉树采用顺序存储比较合适，树中节点序号可以唯一地反映节点之间的逻辑关系。

特别注意：采用顺序存储结构建议从数组下标1开始存储树中的节点，若从数组下标0开始存储，则不满足节点序号之间的逻辑关系



![image-20230425114623830](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230425114623830.png)





#### 二叉树的链式存储结构

顺序存储结构的空间利用率较低，一般二叉树都采用链式存储结构，用链表节点来存储二叉树的每个节点。二叉链表至少包含三个域：数据域 $data$，左指针域 $lchild$ 和右指针域 $rchild$。



```go
//二叉树的链式存储结构

type BiTNode struct{
    data string
    lchild *BiTNode
    rchild *BiTNode
}
```





### 二叉树遍历和线索二叉树

#### 二叉树

二叉树的遍历是指按照某条搜索路径访问树中每个节点，使得每个节点均被访问一次，而且仅被访问一次。



#####  先序遍历

+ 访问根节点
+ 先序遍历左子树
+ 先序遍历右子树

```go
func PreOrder(T  *BiTNode){
    if T!=nil{
        Visit(T)             //访问根节点
        PreOrder(T.lchild)   //递归遍历左子树
        PreOrder(T.rchild)   //递归遍历右子树
    }
}
```



##### 中序遍历

+ 中序遍历左子树
+ 访问根节点
+ 中序遍历右子树

```go
func InOrder(T *BiTNode){
    if T!=nil{
        InOrder(T.lchild)  //中序遍历左子树
        Visit(T)           //访问根节点
        InOrder(T.rchild)  //中序遍历右子树
    }
}
```



##### 后序遍历

+ 后序遍历左子树
+ 后序遍历右子树
+ 访问根节点

```go
func PostOrder(T *BiTNode){
    if T != nil{
        PostOrder(T.lchild) //后序遍历左子树
        PostOrder(T.rchild) //后序遍历右子树
        Visit(T)            //访问根节点
    }
}
```





##### 逆后序遍历

+ 访问根节点
+ 遍历右子树
+ 遍历左子树

==注意：逆后序遍历的方式和先序遍历方式很类似，将逆后序遍历的结果 逆置 ， 便可以得到后序遍历的结果==

```go
func RevPostOrder(T *BiTNode){
    if T!=nil{
        Visit(T)               //访问根节点
        RevPostOrder(T.rchild) //遍历右子树
        RevPostOrder(T.lchild) //遍历左子树
    }
}
```





#####  先序遍历非递归

```go
func PreOrder(T *BiTNode){
    InitStack(S) //初始化栈
    p := T      //p是遍历指针
    
    for p || !IsEmpty(S) {
        if p {
            Visit(p)   //访问当前节点
            Push(S,p)  //入栈
            p=p.lchild  //左孩子不为空，一直向左走
        }else{          //出栈，并转向出栈节点的右子树
            Pop(S,p)    //栈顶元素出栈
            p=p.rchild  //向右子树走，p赋值为当前节点的右孩子
        }
    }
}
```





##### 中序遍历非递归

```go
func InOrder(T *BiTNode){
    InitStack(S)             //初始化栈S
    p := T                  //p是遍历指针
    
    for p || !IsEmpty(S){   //栈不为空或p不为空时，循环
        if p {
            Push(S,p)      //向左，将节点入栈
            p=p.lchild    //左孩子不为空，一直向左走
        }else{            //出栈，并转向出栈节点的右子树
            Pop(S,p)     //栈顶元素出栈
            Visit(p)    //访问出栈节点
            p=p.rchild  //向右子树走，p赋值为当前节点的右孩子
        }
    }
}
```





##### 后序遍历非递归

```go
func PostOrder(T *BiTNode){
    InitStack(S)
    p := T
    r = nil
    
    for p || !IsEmpty(S) {
        if p {             //走到最左边
            Push(S,p)
            p = p.lchild  
        }else{            //向右
            GetTop(S,p)  //读取栈顶节点（非出栈）
            if p.rchild && p.rchild != r{  //若右子树存在，且未被访问
                p=p.rchild  //转向右
                Push(S,p)   //压入栈
                p=p.lchild  //再走到左子树
            }else{         //否则，弹出节点并访问
                Pop(S,p)   //将节点弹出
                Visit(p)  //访问该节点
                r=p      //标记最近访问过的节点
                p=nil    //节点访问完，重置p指针
            }
        }
    }
}
```





##### 层次遍历

```go
func LevelOrder(T *BiTNode){
    InitQueue(Q)     //初始化辅助队列
    p = nil
    EnQueue(Q,T)    //将根节点入队
    for !IsEmpty(Q){  //队列不空则循环
        DeQueue(Q,p)   //队头节点出栈
        Visit(p)   //访问出队节点
        if p.lchild != nil{
            EnQueue(Q,p.lchild)  //左子树不空，则左子树根节点入队           
        }
        if p.rchild != nil{
            EnQueue(Q,p.rchild)  //右子树不空，则右子树根节点入队
        }
    }
}
```





##### 由遍历序列构造二叉树

+ 二叉树先序序列和中序序列可以唯一确定一棵二叉树
+ 二叉树的后序序列和中序序列也可以唯一确定一棵二叉树
+ 二叉树的层次序列和中序序列也可以唯一确定一棵二叉树
+ 只知道二叉树的先序序列和后序序列无法唯一确定一棵二叉树



#### 线索二叉树

传统的二叉链表存储仅能体现一种父子关系，不能直接得到节点在遍历中的前驱或后继。

引入线索二叉树正是为了加快查找节点的前驱和后继的速度。

规定：若无左子树，令 lchild 指向其前驱节点；若无右子树，令 rchild 指向其后继节点

![image-20230426114844800](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230426114844800.png)

```go
//线索二叉树的存储结构
type ThreadNode struct{
    data string 
    lchild *ThreadNode
    rchild *ThreadNode
    ltag int
    rtag int
}
```



```go
//中序线索二叉树构造

func InThread(p *ThreadNode,pre *ThreadNode){
    if p != nil {
        InThread(p.lchild,pre)
        if p.lchild == nil{
            p.lchild = pre
            p.ltag = 1
        }
        if  pre!=nil && pre.rchild ==nil{
            pre.rchild = p
            pre.rtag = 1
        }
        pre = p
        InThread(p.rchild,pre)
    }
}

func CreateInThread(T *ThreadNode){
    pre := nil
    if T != nil{         //非空二叉树，线索化
        InThread(T,pre)  //线索化二叉树
        pre.rchild = nil //处理遍历的最后一个节点
        pre.rtag=1
    }
}
```







### 树、森林

#### 树的存储结构

树的存储既可以采用顺序存储又可以采用链式存储结构。常见存储方式：**双亲表示法、孩子表示法、孩子兄弟表示法。**





#### 树、森林与二叉树转换



**树转换二叉树：**每个节点左指针指向它的第一个孩子，右指针指向它在树中相邻的右兄弟。简称“左孩子右兄弟”

![image-20230427151710966](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230427151710966.png)





**森林转换二叉树：**

+ 将森林中的每棵树转换成相应的二叉树
+ 每棵树的根也可以视为兄弟关系，在每棵树的根之间加上一根横线
+ 以第一棵树的根为轴心顺时针旋转45°

![image-20230427151730331](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230427151730331.png)





#### 树和森林的遍历

**树的遍历**：是指用某种方式访问树中的每个节点，且仅访问一次。

+ 先根遍历：若树非空，先访问根节点，再一次遍历根节点的每棵子树，遍历子树时仍遵循先根后子树的规则。遍历序列与这棵树相应的二叉树的先序序列相同。
+ 后根遍历：若树非空，先依次遍历根节点的每棵子树，再访问根节点，遍历子树时遵循先子树后根的规则。遍历序列与这棵的对应二叉树的中序遍历序列相同。





森林遍历：

+ 先序遍历森林：遍历序列和森林对应的二叉树的先序遍历序列相同
  + 访问森林中每一棵树的根节点
  + 先序遍历第一棵树中根节点的子树森林
  + 先序遍历除去第一棵树之后剩余的树构造的森林

+ 中序遍历森林：遍历序列与森林对应的二叉树的中序遍历序列相同
  + 中序遍历森林中的第一棵树的根节点的子树森林
  + 访问第一棵树的根节点
  + 中序遍历除去第一棵树之后剩余的树构成的森林







### 树与二叉树的应用

#### 二叉排序树（BST）

二叉排序树的特点：**左子树节点值 < 根节点值 < 右子树节点值**

+ 若左子树非空，则左子树上所有的节点均小于根节点的值
+ 若右子树非空，则右子树上所有的节点大于根节点的值
+ 左、右子树分别是一棵二叉排序树



![image-20230427152924639](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230427152924639.png)



**二叉排序树的非递归查找算法：**

```go
func BST_Search(T *BiTNode,key string)*BiTNode{
    for T!=nil && key != T.data {
        if key < T.data {
            T = T.lchild
        }else{
            T = T.rchild
        }
    }
    return T
}
```





**二叉排序树的插入：**

​	插入节点的过程：若原二叉排序树为空，则直接插入节点；否则，若关键字 k 小于根节点值，则插入到左子树中；若关键字 k 大于根节点的值，在插入右子树。

​	注意：插入的节点一定是一个新添加的叶子节点，且是查找失败时的查找路径上访问的最后一个节点的左孩子或右孩子。



**二叉排序树插入操作算法：**

```go
func BST_Insert(T *BiTNode,key string)int{
    if T == nil{                //原树为空，新插入的记录为根节点
        T = BiTNode{data:key}
        return 1                //返回1，插入成功
    }else if k == T.data{       //树中存在相同关键字的节点，插入失败
        return 0
    }else if k < T.data{        //插入到T的左子树
        return BST_Insert(T.lchild,key)
    }else{                     // 插入到T的右子树
        return BST_Insert(T.rchild,key)
    }
}
```



**二叉排序树构造算法：**

```go
func Create_BST(T *BiTNode, str []string, n int){
    T = nil      //初始时，T为空树
    i := 0
    for i<n{     //依次将每个关键字插入到二叉排序树中
        BST_Insert(T,str[i])
        i++
    }
}
```



二叉排序树的删除：

​	在二叉排序树中删除一个节点时，不能把以该节点为根的子树上的节点都删除，必须先把被删除节点从存储二叉排序树的链表上摘下，将因删除节点而断开的二叉链表重新链接起来，同时确保二叉排序树的性质不会丢失。

删除操作的实现过程按以下三种情况处理：

+ 若被删除的节点 z 是叶子节点，则直接删除，不会破坏二叉排序树的性质
+ 若节点 z 只有一棵左子树或右子树，则令 z 的子树成为 z 父节点的子树，替代 z 的位置。
+ 若节点 z 有左、右子树，则令 z 的直接后继（或直接前驱）替代 z ，然后从二叉排序树中删去这个直接后继（或直接前驱）。此时就转换成了第一种情况或第二种情况。

![image-20230427155639648](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230427155639648.png)





#### 平衡二叉树（ASL）

为了避免树的高度增长过快，降低二叉排序树的性能，规定在插入和删除二叉树的节点时，要保证任意节点的左、右子树高度差的绝对值不超过1，将这样的二叉树称为平衡二叉树。

平衡因子：左子树和右子树的高度之差，差值只能为 -1，0，1













#### 哈夫曼树和哈夫曼编码













## 图

### 图

图 G 由顶点集 V 和边集 E ，其中 V(G) 表示图 G 中顶点的有限非空集， E(G) 表示图 G 中的顶点之间的关系（边）集合。其中，顶点的个数称为图的阶。

注意：线性表可以是空表，树可以是空树，但是图不可以是空图。即图中至少有一个顶点，可以没有边。



+ 有向图：若 E 是有向边（也称弧）的有限集合时，则图G为有向图。弧是顶点的有序对，记为 <v,w> ，其中 v ，w 是顶点，v 称为弧尾，w 称为弧头，<v , w> 称为从顶点 v 到顶点 w 的弧。

+ 无向图：若 E 是无向边（简称为边）的有序集合时，则图 G 为无向图。边是顶点的无序对，记为 (v ，w) 或 (w , v)。

+ 简单图：一个图 G 满足：

  + 不存在重复边
  + 不存在顶点到自身的边

+ 多重图：若图 G 中某两个节点之间的边数多于一条，又允许顶点通过同一条边与自己关联，称为多重图。**即：存在重复边或存在节点到自身的边。**

+ 完全图：

  + 有向完全图：任意两个顶点之间存在有向弧，即存在 $n(n-1)$ 条弧的有向图，称为有向完全图
  + 无向完全图：任意两个顶点之间存在边，即存在 $n(n-1)/2$ 条边，称为无向完全图。

  









### 图的存储

+ 邻接矩阵法：指用一个一维数组存储图中顶点的信息，用一个二维数组存储图中边的信息，其中存储顶点之间关系的二维数组称为邻接矩阵。

  ```go
  type MGraph struct{
      Vex [10]char     //顶点表，10表示顶点数
      Edge [10][10]int //邻接矩阵
  }
  ```

  

+ 邻接表法：指对图G中的每个顶点 $v_i$ 单独建立一个单链表，第 $i$ 个单链表中的节点表示依附于顶点 $v_i$ 的边（对于有向图则是以顶点 $v_i$ 为尾的弧），这个单链表就称为顶点 $v_i$ 的边表。边表中头指针和顶点的数据信息采用顺序存储（称为定点表），所以在邻接表中存在两种节点：顶点表节点和边表节点。

  ```go
  type ArcNode struct{  //边表节点
      adjvex int      //该弧所指向的顶点的位置
      next *ArcNode  //指向下一条弧的指针
  }
  
  type VNode struct{  //顶点表节点
  	data string    //顶点信息
      first *ArcNode //指向第一条依附该顶点的弧的指针
  }
  
  type ALGraph struct{
      AdjList VNode  //邻接表
      vexnum int    //图的顶点数
      arenum int   //图的弧数
  }
  ```

![image-20230428144941671](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230428144941671.png)	

+ 十字链表：是**有向图**的一种链式存储结构
+ 邻接多重表：是**无向图**的一种链式存储结构







### 图的遍历

图的遍历是指从图中的某一顶点出发，按照某个搜索方法沿着图中的边对图中的所有顶点访问一次且仅访问一次。

为了避免同一顶点被访问多次，在遍历的过程中，必须记下每个已访问过的顶点，为此可以设一个辅助数组`visited[]`来标记顶点是否被访问过。



#### 广度优先搜素

广度优先搜索（BFS）类似二叉树的层序遍历算法。



**基本思想：**

​	首先访问起始顶点 $v$ ，接着由 $v$  出发，依次访问 $v$ 的各个未访问的邻接顶点 $w_1,w_2,...,w_i$ ，然后依次访问 $w_1,w_2,...,w_i$ 的所有未被访问过的邻接顶点；再从这些访问过的顶点出发，访问它们所有未被访问过的邻接顶点，直至图中所有顶点都被访问过为止。若此时图中尚有顶点未被访问，则另选图中一个未曾被访问的顶点作为起始点，重复上述过程，直至图中所有顶点都被访问到为止。



```go
var visited [100]bool  //100表示顶点数

func BFSTraverse(G Graph){          //对图G进行广度优先遍历
    for i:=0;i<G.vexnum;i++{        
        visited[i] = false          //访问标记数组初始化
    }
    InitQueue(Q);                   //初始化辅助队列Q
    for i:=0;i<G.vexnum;i++{       //从0号顶点开始遍历
        if !visited[i] {           //对每个连通分量调用一次BFS
            BFS(G,i)               //vi 未访问过，从 vi 开始BFS
        }
    }
}

func BFS(G Graph,v int){         //从顶点v出发，广度优先遍历图G
    visit(v)                     //访问初始顶点v
    visited[v] = true            //对v做已访问标记
    Enqueue(Q,v)                 //顶点v入队列Q
    for !isEmpty(Q) {
        DeQueue(Q,v)            //顶点v出队列
        for w:=FirstNeighbor(G,v); w>=0; w=NextNeighbor(G,v,w){   //检测v所有邻接顶点
            if !visited[w] {            //w为v的尚未访问的邻接顶点
                visit(w)                //访问顶点w
                visited[w] = true       //对w做已访问标记
                EnQueue(Q,w)            //顶点w入队列
            }
        }
    }
}
```







#### 深度优先搜索

深度优先搜索（DFS）类似于树的先序遍历。

基本思想：

​	首先访问图中的某个起始顶点 $v$ ，然后由 $v$ 出发，访问与 $v$ 紧接且未被访问的任一顶点 $w_1$，再访问与 $w_1$ 紧接且未被访问的任一顶点 $w_2,...$ 重复上述过程。当不能再继续向下访问时，依次退回到最近被访问的顶点，若它还有邻接顶点未被访问过，则从该点开始继续上述搜索过程，直至图中所有顶点均被访问过为止。



```go
var visited [10]bool  //访问标记数组

func DFSTraverse(G Graph){
    for v:=0; v<G.vexnum;v++{
        visited[v] = false   //初始化已访问标记数组
    }
    for v:=0;v<G.vexnum;v++{    //从v=0开始遍历
        if !visited[v]{
            DFS(G,v)
        }
    }
}

func DFS(G Graph,v int){  //从顶点v出发，深度优先遍历图G
    visit(v)              //访问顶点v
    visited[v]=true      //设已访问标记
    for w:=FirstNeighbor(G,v);w>=0;w=NextNeighbor(G,v,w){
        if !visited[w] {   //w为u的尚未访问的邻接顶点
            DFS(G,w)
        }
    }
}
```







### 图的应用



#### 最小生成树



#### 最短路径



##### Dijkstra算法求单源最短路径问题



##### Floyd算法求各顶点之间的最短路径问题



#### 有向无环图描述表达式



#### 拓扑序列



#### 关键路劲









##  查找

### 顺序查找

顺序查找适用于顺序表和链表



**顺序表查找算法：**

```go
func search(arr []int,n int,key int) int{
    for i:=0;i<n;i++{
        if arr[i] == key{
            return i      //查找成功返回1
        }
    }
    return -1     //查找失败返回-1
}
```

平均查找成功的长度：$\frac{n+1}{2}\times n \times \frac{1}{n} = \frac{n+1}{2}$，**时间复杂度$O(n)$** 

查找失败的长度：$n+1$



**链表查找算法：**

```go
func search(head *LNode,key int)*LNode{
    p := head.next
    for p!=nil{
        if p.data == key {
            return p
        }
        p=p.next
    }
    return nil
}
```





### 折半查找

折半查找针对是**有序（递增或递减）的顺序表**

```go
func B_Search(arr []int,low int,high int,key int) int {
    var mid int
    for low<=high{
        mid = (low+high)/2     //取顺序表的中间位置
        if arr[mid] ==  key{  
            return mid
        }else if arr[mid]>key{  //说明需要在 arr[low,..,mid-1]中查找
            high = mid-1
        }else{
            low = mid+1        //说明需要在 arr[mid+1,...,high]中查找
        }
    }
    return -1
}
```

**折半查找的时间复杂度：$O(log_{2}{n})$**  





**折半查找递归算法：**

```go
func BinSearch(arr []int ,low int,high int,key int)int{
    if low > high{
        return -1    //查找失败，返回-1
    }
    mid := (low+high)/2
    
    if arr[mid] == key{
        return mid
    }else if arr[mid]>key{
        return BinSearch(arr,low,mid-1,key)
    }else{
        return BinSearch(arr,mid+1,high,key)
    }
}
```





### 分块查找 

分块查找又称为索引顺序查找。

分块查找的基本思想：将查找表分为若干个块。块内的元素可以是无序的，但块之间是有序的。即第一个块中的最大关键字小于第二个块中的所有记录的关键字，第二个块中的最大关键字小于第三个块中的所有记录的关键字，以此类推。再建立一个索引表，**索引表中的每个元素含有各块的最大关键字和各块的第一个元素的地址**，**索引表按关键字有序排序**。



查找过程：第一步：先在索引表中确定待查记录的所在的块，可以顺序查找或折半查找索引表；第二步是在块内顺序查找

**分块查找的长度 = 索引查找和块内查找的平均长度之和**



![image-20230429125158281](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230429125158281.png)









### B树



### B+树



### 散列表

散列表：根据关键字的值来计算出关键字在表中的地址，即将关键字映射到对应的存储位置上。

散列表的构造方法：

+ 直接定址法
+ 除留余数法：一般使用该方法，$H(key)=key\%p，其中p取不大于表长m，但最接近m的质数$。
+ 数字分析法
+ 平方取中法
+ 折叠法

散列表处理冲突的方法：

+ 开放定址法（闭散列法）：$H_i=(H(key)+d_i)\%m，其中m为表长，d_i为增量序列$
  + 线性探测法：当$d_i=0,1,2,...,m-1$时，当出现冲突时，顺序查找出一个空闲单元（当表未填满时一定能找到一个空闲单元）或查遍全表。
  + ==注意：在开放定址法的情形下，不能随便物理删除表中的已有元素，因为若是删除元素，则会截断其他具有相同散列地址的元素的查找地址。==
+ 拉链法（开散列法）：将所有的具有相同散列地址的元素存储到一个线性链表中。





## 排序



### 插入排序

#### 直接插入排序

```go
func insertSort(arr []int,n int){
    var i,j,temp int
    for i=0;i<n;i++{
        temp = arr[i]
        for j=i-1;j>=0&& arr[j]>temp;j--{   //形成递增，向前插入
            arr[j+1]=a[j]
        }
        a[j+1]=temp    //若找到插入位置，将temp中的暂存值插入
    }
}
```

**空间复杂度：$O(1)$，时间复杂度：$O(n^2)$**

稳定性：稳定的排序算法

适用性：适用于任何顺序存储和链式存储的线性表

==注意：直接插入排序的元素插入位置不一定是最终位置，元素位置处于运动中，元素比较次数与初始序列有关，初始序列有序时，插入排序的比较次数最小。==





#### 折半插入排序

**折半插入排序的基本思想**：利用折半查找，在前一部分顺序表中，找出合适的位置进行插入

```go
func insertSort(arr []int,n int){
    var i,j,low,high,mid temp int
    for i=1;i<n;i++{
        temp = arr[i]
        low = 0
        high = i-1
        for low<=high{
            mid := (low+high)/2
            if arr[mid] > temp{
                high = mid-1
            }else{
                low = mid+1
            }
        }
        for j=i-1;j>=high+1;j--{   //向后移动元素，空出插入位置
            arr[j+1] = arr[j]
        }
        arr[high+1] = temp       //插入元素
    }
}
```

空间复杂度：$O(1)$

时间复杂度：时间复杂度仍为$O(n^2)$，折半查找减少了比价次数，约为$O(nlog_2{n})$，比较次数与初始序列无关

稳定性：稳定的排序算法

适用性：顺序表（顺序存储）





#### 希尔排序

**希尔排序的基本思想**：将一个序列分割成几个子表，然后采用直接插入排序思想。

```go
func shellSort(arr []int,n int){  //注意：这里关键字的存储设定从数组下标1开始
    var i,j,dk int
    for dk=n/2;dk>=1;dk=dk/2{   //dk表示步长变化
        for i=dk+1;i<=n;i++{    //将arr[i]插入有序递增子表中
            arr[0]=arr[i]      //arr[0]用来暂存arr[i]，相当于temp的作用
            for j=i-dk;j>0&&arr[0]<arr[j];j=j-dk{
                arr[j+dk] = arr[j]   //元素后移，查找插入位置
            }
            arr[j+dk]=arr[0]    //插入元素
        }
    }
}
```

空间复杂度：$O(1)$

时间复杂度：最坏情况下：$O(n^2)$；某种特殊范围内：$O(n^{1.3})$

稳定性：不稳定的排序算法

适用性：仅适用线性表（顺序存储）

==注意：希尔排序是基于直接插入排序算法改进过来的，故每趟插入位置不一定是最终位置，比较次数与初始序列有关。==







### 交换排序

#### 冒泡排序

**冒泡排序的基本思想**：将 $i$ 之前元素中的最大元素移至 $i$ 的位置，$i$ 不断向左边移动

```go
func bubleSort(arr []int,n int){
    var i,j,temp,flag int
    for i=n-1;i>=1;i--{
        flag = 0        //用来标记本趟排序是否发生了交换
        for j=1;j<=i;j++{
            if arr[j-1] > arr[j] {
                temp = arr[j-1]
                arr[j-1] = arr[j]
                arr[j] = temp
                flag = 1          //发生了交换，将flag值置为1
            }
        }
        if flag == 0{       //若一趟排序中没有发生关键字交换，则证明序列有序
            return         //排序结束
        }
    }
}
```

空间复杂度：$O(1)$ 

时间复杂度：

+ 最好情况下：初始序列有序，只需要比较 $n-1$ 次，移动 $0$ 次，复杂度：$O(n)$
+ 最坏情况下：初始序列逆序，比较 $\frac{n(n+1)}{2}$，移动次数 $3\times \frac{n(n-1)}{2}$，复杂度：$O(n^2)$
+ 平均情况下：时间复杂度： $O(n^2)$

稳定性：稳定的排序算法，冒泡排序产生的有序序列全局有序

适用性：顺序存储和链式存储

==注意：一次冒泡排序，将一个元素放到它最终的位置，元素比较次数与初始序列有关==







#### 快速排序

**快速排序的基本思想：**基于分治法思想，在待排序表`arr[1,...,n]`中任取一个元素`pivot`作为枢轴（通常取首元素），通过一趟排序，将待排序表划分两个独立部分，左部分小于`pivot`，右部分小于等于`pivot`，最后将`pivot`放在 $i$（最终位置）。

```go
func quickSort(arr []int,low int,high int){
    var pivot int
    var i = low
    var j = high
    if low<high{
        pivot = arr[i]    //将当前表中第一个关键字设为枢轴，对表进行划分
        for i<j{
            for i<j && arr[j] >= pivot{  //从右向左扫描，找到一个小于pivot关键字
                j--
            }
            if i<j{
                arr[i] = arr[j]  //放在pivot左边
                i++
            }
            for i<j && arr[i] <= pivot{  //从左向右扫描，找到一个大于pivot关键字
                i++
            }
            if i<j{
                arr[j] = arr[i]
                j--
            }
        }
        arr[i] = pivot   //将pivot放在最终位置上
    }
    quickSort(arr,low,i-1)  //递归遍历pivot左边关键字，进行排序
    quickSort(arr,i+1,high) //递归遍历pivot右边关键字，进行排序
}
```

空间复杂度：平均情况下：栈深度$O(log_2{n})$，最坏情况下：$O(n)$，最好情况下：$O(log_2{n})$

时间复杂度：最坏情况下（初始序列表有序）：$O(n^2)$，平均情况下：$O(nlog_2{n})$，最好情况下（每次枢轴恰好等分长度相近两部分）：$O(nlog_2{n})$

稳定性：不稳定排序算法

适用性：顺序存储

==注意：快排一次划分会将一个元素（pivot）放在最终位置上，但不会产生有序子列。快排是内部排序中性能最优算法，排序趟数与初始序列有关。==











### 选择排序



#### 简单选择排序

**简单选择排序的基本思想**：从 $i$ 的后半部分中找到一个最小的元素，与 `arr[i]` 进行交换，$i$ 向右移动，重复此操作。

```go
func selectSort(arr []int,n int){
    var i,j,k,temp;
    for i=0;i<n;i++{
        k=i          //记录最小元素的位置
        for j=i+1;j<n;j++{
            if arr[j]<arr[k]{
                k=j           //更新最小元素的位置
            }
        }
        temp = arr[i]  
        arr[i]=arr[k]
        arr[k]=temp
    }
}
```

空间复杂度：$O(1)$

时间复杂度：元素的比较次数与初始序列无关，复杂度：$O(\frac{n(n-1)}{2})$，移动次数不超过 $3(n-1)$

稳定性：不稳定的排序算法

适用性：顺序存储和链式存储

==注意：每一趟排序可以确定一个元素的最终位置，比较次数与初始序列无关。==







#### 堆排序















### 归并排序和基数排序

#### 归并排序

**归并排序的基本思想**：先将一个有序表进行划分，分成每个元素长度为1，然后进行两两归并













####  基数排序





### 各种内部排序算法的比较及应用





### 外部排序























# 计算机网络



三次握手：

































# 计算机系统



































# 计算机组成原理



































# 常用组件和技巧

## http常用状态码

###  1xx

需要请求者继续执行操作的状态码

- 100 continue 请求者需要继续发送请求，服务器收到部分请求，等待其余部分
- 101 切换协议 请求要求服务器切换协议，服务器已经确认，准备切换

###  2xx

请求成功

- 200 ok 服务器收到请求，并处理成功，返回结果
- ❗️201 已创建 请求成功并且服务器已经创建了资源
- ❗️202 已接受 服务器接受了请求但是未处理
- 204 no content 服务器成功处理了请求，但是没有返回内容
- 206 部分请求

###  3xx

重定向，完成请求需要进一步操作

- 301 （Moved Permanently） 永久重定向 所请求的资源被永久移动了，响应的 location 字段中指明了新的 URL，浏览器自动使用 get 方法重定向到新的 URL，并且后续的请求也使用新的 URL
- 302 （Found） 临时重定向 所请求的资源被临时移动了，响应的 location 字段中指明了新的 URL，浏览器自动使用 get 方法重定向到新的 URL，但是后续的请求依旧使用旧的 URL



**301 和 302 应该是支持任意方法的重定向，但是浏览器都使用 get 方法重定向**

- 303 see other 临时重定向 **会将任意请求方法都重定向为 get** 类似于 302
- 304 not modified 协商缓存
- 307 （Temporary Redirect） 临时重定向 类似于 302 , **不会修改重定向请求的方法**
- 308 （Permanent Redirect） 永久重定向 类似于 301



**301、302、303、307、308 的区别**

301、308 都是永久重定向
302、303、307 都是临时重定向



**302 Found**

302 状态码表示目标资源临时移动到了另一个 URI 上。由于重定向是临时发生的，所以客户端在之后的请求中还应该使用原本的 URI。

服务器会在响应 Header 的 Location 字段中放上这个不同的 URI。浏览器可以使用 Location 中的 URI 进行自动重定向。

> 由于历史原因，用户代理可能会在重定向后的请求中把 POST 方法改为 GET 方法。如果不想这样，应该使用 307（Temporary Redirect） 状态码。



**303 See Other**

303 状态码表示服务器要将浏览器重定向到另一个资源，这个资源的 URI 会被写在响应 Header 的 Location 字段。从语义上讲，重定向到的资源并不是你所请求的资源，而是对你所请求资源的一些描述。

303 常用于将 POST 请求重定向到 GET 请求，比如你上传了一份个人信息，服务器发回一个 303 响应，将你导向一个“上传成功”页面。

不管原请求是什么方法，重定向请求的方法都是 GET（或 HEAD，不常用）

到这里你可能发现，303 和 302 的作用很类似，除去语义差别，似乎是 302 包含了 303 的情况。



**307 Temporary Redirect**

307 的定义实际上和 302 是一致的，唯一的区别在于，307 状态码不允许浏览器将原本为 POST 的请求重定向到 GET 请求上。



**总结**

302 允许各种情况的重定向，一般情况下会实现为到 GET 的重定向，但是不能确保 POST 会重定向为 POST；303 会允许任意请求到 GET 的重定向；307 和 302 一样，但是不允许 GET 到 POST 的重定向



**301 和 308**

**301 Moved Permanently**

301 状态码表明目标资源被永久的移动到了一个新的 URI，任何未来对这个资源的引用都应该使用新的 URI。

**308 Permanent Redirect**

308 的定义实际上和 301 是一致的，唯一的区别在于，308 状态码不允许浏览器将原本为 POST 的请求重定向到 GET 请求上。



###  4xx

客户端错误

- 400 bad request 客户端的请求服务其无法理解
- 401 未授权 需要客户端登陆认证
- 403 forbidden 禁止访问
- 404 not found 请求的资源不存在
- 413 请求实体过大
- 414 请求的 url 过长
- 415 不支持的媒体类型

###  5xx

服务器错误

- 500 服务器内部错误
- 502 错误网关 服务器作为网关或代理，从上游服务器收到无效响应
- 503 服务不可用 由于超载或者是维护无法提供服务，是暂时的状态 （响应中有一个 Retry-After 字段，提示客户端在多久之后重试）
- 504 Gateway Timeout 服务器作为网关或者代理，但是没有即使从上游服务器收到响应
- 505 HTTP 版本不支持 服务器不支持请求中使用的 http 协议的版本









## json

### json数据格式

json数据解析网站：`json.cn`



#### 对象形式

- JSON中对象的属性名必须是双引号，属性值如果是字符串也必须是双引号
- JSON只要涉及到字符串就必须使用双引号，不支持undefined

```go
{
    "name":"zhangsan",
    "age":18,
    "hobby":["篮球","足球"],
}
```



#### 数组/集合形式

- 数组中的字符串必须使用双引号
- JSON中只要涉及到字符串 就必须使用双引号
- 不支持undefined

```go
[1,"hangzhou",null]


[
    {
        "name":"zhangsan",
        "age":18
    },
    {
        "name":"lisi",
        "age":23
    }
]
```











## go操作NSQ

### NSQ介绍

NSQ是go语言编写的一个开源的实时分布式内存消息队列，其性能十分优异。

好处：

+ NSQ提倡分布式和分散的拓扑，没有单点故障，支持容错和高可用性，并提供可靠的消息交付保证。
+ NSQ支持横向扩展，没有任何集中式代理
+ NSQ易于配置和部署，并且内置了界面管理





### NSQ应用场景

通常来说消息队列都适用一下场景。

#### 异步处理

![image-20230107175832138](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230107175832138.png)

#### 应用解耦

![image-20230107175947698](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230107175947698.png)

#### 流量削峰

![image-20230107180031035](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230107180031035.png)



### 安装

[官方下载页面](https://nsq.io/deployment/installing.html)根据自己的平台下载并解压即可。



### NSQ组件



#### nsqd

nsqd是一个守护进程，它接收、排队并向客户端发送消息。

启动`nsqd`，指定`-broadcast-address=127.0.0.1`来配置广播地址

```go
./nsqd -broadcast-address=127.0.0.1
```

如果是在搭配`nsqlookupd`使用的模式下需要还指定`nsqlookupd`地址:

```bash
./nsqd -broadcast-address=127.0.0.1 -lookupd-tcp-address=127.0.0.1:4160
```

如果是部署了多个`nsqlookupd`节点的集群，那还可以指定多个`-lookupd-tcp-address`。

`nsqdq`相关配置项如下：

```go
-auth-http-address value
    <addr>:<port> to query auth server (may be given multiple times)
-broadcast-address string
    address that will be registered with lookupd (defaults to the OS hostname) (default "PROSNAKES.local")
-config string
    path to config file
-data-path string
    path to store disk-backed messages
-deflate
    enable deflate feature negotiation (client compression) (default true)
-e2e-processing-latency-percentile value
    message processing time percentiles (as float (0, 1.0]) to track (can be specified multiple times or comma separated '1.0,0.99,0.95', default none)
-e2e-processing-latency-window-time duration
    calculate end to end latency quantiles for this duration of time (ie: 60s would only show quantile calculations from the past 60 seconds) (default 10m0s)
-http-address string
    <addr>:<port> to listen on for HTTP clients (default "0.0.0.0:4151")
-http-client-connect-timeout duration
    timeout for HTTP connect (default 2s)
-http-client-request-timeout duration
    timeout for HTTP request (default 5s)
-https-address string
    <addr>:<port> to listen on for HTTPS clients (default "0.0.0.0:4152")
-log-prefix string
    log message prefix (default "[nsqd] ")
-lookupd-tcp-address value
    lookupd TCP address (may be given multiple times)
-max-body-size int
    maximum size of a single command body (default 5242880)
-max-bytes-per-file int
    number of bytes per diskqueue file before rolling (default 104857600)
-max-deflate-level int
    max deflate compression level a client can negotiate (> values == > nsqd CPU usage) (default 6)
-max-heartbeat-interval duration
    maximum client configurable duration of time between client heartbeats (default 1m0s)
-max-msg-size int
    maximum size of a single message in bytes (default 1048576)
-max-msg-timeout duration
    maximum duration before a message will timeout (default 15m0s)
-max-output-buffer-size int
    maximum client configurable size (in bytes) for a client output buffer (default 65536)
-max-output-buffer-timeout duration
    maximum client configurable duration of time between flushing to a client (default 1s)
-max-rdy-count int
    maximum RDY count for a client (default 2500)
-max-req-timeout duration
    maximum requeuing timeout for a message (default 1h0m0s)
-mem-queue-size int
    number of messages to keep in memory (per topic/channel) (default 10000)
-msg-timeout string
    duration to wait before auto-requeing a message (default "1m0s")
-node-id int
    unique part for message IDs, (int) in range [0,1024) (default is hash of hostname) (default 616)
-snappy
    enable snappy feature negotiation (client compression) (default true)
-statsd-address string
    UDP <addr>:<port> of a statsd daemon for pushing stats
-statsd-interval string
    duration between pushing to statsd (default "1m0s")
-statsd-mem-stats
    toggle sending memory and GC stats to statsd (default true)
-statsd-prefix string
    prefix used for keys sent to statsd (%s for host replacement) (default "nsq.%s")
-sync-every int
    number of messages per diskqueue fsync (default 2500)
-sync-timeout duration
    duration of time per diskqueue fsync (default 2s)
-tcp-address string
    <addr>:<port> to listen on for TCP clients (default "0.0.0.0:4150")
-tls-cert string
    path to certificate file
-tls-client-auth-policy string
    client certificate auth policy ('require' or 'require-verify')
-tls-key string
    path to key file
-tls-min-version value
    minimum SSL/TLS version acceptable ('ssl3.0', 'tls1.0', 'tls1.1', or 'tls1.2') (default 769)
-tls-required
    require TLS for client connections (true, false, tcp-https)
-tls-root-ca-file string
    path to certificate authority file
-verbose
    enable verbose logging
-version
    print version string
-worker-id
    do NOT use this, use --node-id
```



#### nsqlookupd

nsqlookupd是维护所有nsqd状态、提供服务发现的守护进程。它能为消费者查找特定`topic`下的nsqd提供了运行时的自动发现服务。 它不维持持久状态，也不需要与任何其他nsqlookupd实例协调以满足查询。因此根据你系统的冗余要求尽可能多地部署`nsqlookupd`节点。它们小豪的资源很少，可以与其他服务共存。我们的建议是为每个数据中心运行至少3个集群。

`nsqlookupd`相关配置项如下：

```go
-broadcast-address string
    address of this lookupd node, (default to the OS hostname) (default "PROSNAKES.local")
-config string
    path to config file
-http-address string
    <addr>:<port> to listen on for HTTP clients (default "0.0.0.0:4161")
-inactive-producer-timeout duration
    duration of time a producer will remain in the active list since its last ping (default 5m0s)
-log-prefix string
    log message prefix (default "[nsqlookupd] ")
-tcp-address string
    <addr>:<port> to listen on for TCP clients (default "0.0.0.0:4160")
-tombstone-lifetime duration
    duration of time a producer will remain tombstoned if registration remains (default 45s)
-verbose
    enable verbose logging
-version
    print version string
```



#### nsqadmin



一个实时监控集群状态、执行各种管理任务的Web管理平台。 启动`nsqadmin`，指定`nsqlookupd`地址:

```bash
./nsqadmin -lookupd-http-address=127.0.0.1:4161
```

我们可以使用浏览器打开`http://127.0.0.1:4171/`访问如下管理界面。![nsqadmin管理界面](https://www.liwenzhou.com/images/Go/nsq/nsqadmin0.png)

`nsqadmin`相关的配置项如下：

```go
-allow-config-from-cidr string
    A CIDR from which to allow HTTP requests to the /config endpoint (default "127.0.0.1/8")
-config string
    path to config file
-graphite-url string
    graphite HTTP address
-http-address string
    <addr>:<port> to listen on for HTTP clients (default "0.0.0.0:4171")
-http-client-connect-timeout duration
    timeout for HTTP connect (default 2s)
-http-client-request-timeout duration
    timeout for HTTP request (default 5s)
-http-client-tls-cert string
    path to certificate file for the HTTP client
-http-client-tls-insecure-skip-verify
    configure the HTTP client to skip verification of TLS certificates
-http-client-tls-key string
    path to key file for the HTTP client
-http-client-tls-root-ca-file string
    path to CA file for the HTTP client
-log-prefix string
    log message prefix (default "[nsqadmin] ")
-lookupd-http-address value
    lookupd HTTP address (may be given multiple times)
-notification-http-endpoint string
    HTTP endpoint (fully qualified) to which POST notifications of admin actions will be sent
-nsqd-http-address value
    nsqd HTTP address (may be given multiple times)
-proxy-graphite
    proxy HTTP requests to graphite
-statsd-counter-format string
    The counter stats key formatting applied by the implementation of statsd. If no formatting is desired, set this to an empty string. (default "stats.counters.%s.count")
-statsd-gauge-format string
    The gauge stats key formatting applied by the implementation of statsd. If no formatting is desired, set this to an empty string. (default "stats.gauges.%s")
-statsd-interval duration
    time interval nsqd is configured to push to statsd (must match nsqd) (default 1m0s)
-statsd-prefix string
    prefix used for keys sent to statsd (%s for host replacement, must match nsqd) (default "nsq.%s")
-version
    print version string
```





### NSQ架构



#### NSQ工作模式

![nsq架构设计](https://www.liwenzhou.com/images/Go/nsq/nsq4.png)

#### Topic和Channel

每个nsqd实例旨在一次处理多个数据流。这些数据流称为`“topics”`，一个`topic`具有1个或多个`“channels”`。每个`channel`都会收到`topic`所有消息的副本，实际上下游的服务是通过对应的`channel`来消费`topic`消息。

`topic`和`channel`不是预先配置的。`topic`在首次使用时创建，方法是将其发布到指定`topic`，或者订阅指定`topic`上的`channel`。`channel`是通过订阅指定的`channel`在第一次使用时创建的。

`topic`和`channel`都相互独立地缓冲数据，防止缓慢的消费者导致其他`chennel`的积压（同样适用于`topic`级别）。

`channel`可以并且通常会连接多个客户端。假设所有连接的客户端都处于准备接收消息的状态，则每条消息将被传递到随机客户端。例如：

![nsq架构设计](https://www.liwenzhou.com/images/Go/nsq/nsq5.gif)总而言之，消息是从`topic -> channel`（每个channel接收该topic的所有消息的副本）多播的，但是从`channel -> consumers`均匀分布（每个消费者接收该channel的一部分消息）。



#### NSQ接收和发送消息流程

![nsq架构设计](https://www.liwenzhou.com/images/Go/nsq/nsq6.png)









### NSQ特性

- 消息默认不持久化，可以配置成持久化模式。nsq采用的方式时内存+硬盘的模式，当内存到达一定程度时就会将数据持久化到硬盘。
  - 如果将`--mem-queue-size`设置为0，所有的消息将会存储到磁盘。
  - 服务器重启时也会将当时在内存中的消息持久化。
- 每条消息至少传递一次。
- 消息不保证有序。





### go操作NSQ

官方提供了Go语言版的客户端：[go-nsq](https://github.com/nsqio/go-nsq)，更多客户端支持请查看[CLIENT LIBRARIES](https://nsq.io/clients/client_libraries.html)。

#### 安装

```go
go get -u github.com/nsqio/go-nsq
```

#### 生产者

一个简单的生产者示例代码如下：

```go
// nsq_producer/main.go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"

	"github.com/nsqio/go-nsq"
)

// NSQ Producer Demo

var producer *nsq.Producer

// 初始化生产者
func initProducer(str string) (err error) {
	config := nsq.NewConfig()
	producer, err = nsq.NewProducer(str, config)
	if err != nil {
		fmt.Printf("create producer failed, err:%v\n", err)
		return err
	}
	return nil
}

func main() {
	nsqAddress := "127.0.0.1:4150"
	err := initProducer(nsqAddress)
	if err != nil {
		fmt.Printf("init producer failed, err:%v\n", err)
		return
	}

	reader := bufio.NewReader(os.Stdin) // 从标准输入读取
	for {
		data, err := reader.ReadString('\n')
		if err != nil {
			fmt.Printf("read string from stdin failed, err:%v\n", err)
			continue
		}
		data = strings.TrimSpace(data)
		if strings.ToUpper(data) == "Q" { // 输入Q退出
			break
		}
		// 向 'topic_demo' publish 数据
		err = producer.Publish("topic_demo", []byte(data))
		if err != nil {
			fmt.Printf("publish msg to nsq failed, err:%v\n", err)
			continue
		}
	}
}
```

将上面的代码编译执行，然后在终端输入两条数据`123`和`456`：

```bash
$ ./nsq_producer 
123
2018/10/22 18:41:20 INF    1 (127.0.0.1:4150) connecting to nsqd
456
```

使用浏览器打开`http://127.0.0.1:4171/`可以查看到类似下面的页面： 在下面这个页面能看到当前的`topic`信息：![nsqadmin界面1](https://www.liwenzhou.com/images/Go/nsq/nsqadmin1.png)

点击页面上的`topic_demo`就能进入一个展示更多详细信息的页面，在这个页面上我们可以查看和管理`topic`，同时能够看到目前在`LWZMBP:4151 (127.0.01:4151)`这个`nsqd`上有2条message。又因为没有消费者接入所以暂时没有创建`channel`。![nsqadmin界面2](https://www.liwenzhou.com/images/Go/nsq/nsqadmin2.png)

在`/nodes`这个页面我们能够很方便的查看当前接入`lookupd`的`nsqd`节点。![nsqadmin界面3](https://www.liwenzhou.com/images/Go/nsq/nsqadmin3.png)

这个`/counter`页面显示了处理的消息数量，因为我们没有接入消费者，所以处理的消息数量为0。![nsqadmin界面4](https://www.liwenzhou.com/images/Go/nsq/nsqadmin4.png)

在`/lookup`界面支持创建`topic`和`channel`。![nsqadmin界面5](https://www.liwenzhou.com/images/Go/nsq/nsqadmin5.png)





#### 消费者

一个简单的消费者示例代码如下：

```go
// nsq_consumer/main.go
package main

import (
	"fmt"
	"os"
	"os/signal"
	"syscall"
	"time"

	"github.com/nsqio/go-nsq"
)

// NSQ Consumer Demo

// MyHandler 是一个消费者类型
type MyHandler struct {
	Title string
}

// HandleMessage 是需要实现的处理消息的方法
func (m *MyHandler) HandleMessage(msg *nsq.Message) (err error) {
	fmt.Printf("%s recv from %v, msg:%v\n", m.Title, msg.NSQDAddress, string(msg.Body))
	return
}

// 初始化消费者
func initConsumer(topic string, channel string, address string) (err error) {
	config := nsq.NewConfig()
	config.LookupdPollInterval = 15 * time.Second
	c, err := nsq.NewConsumer(topic, channel, config)
	if err != nil {
		fmt.Printf("create consumer failed, err:%v\n", err)
		return
	}
	consumer := &MyHandler{
		Title: "沙河1号",
	}
	c.AddHandler(consumer)

	// if err := c.ConnectToNSQD(address); err != nil { // 直接连NSQD
	if err := c.ConnectToNSQLookupd(address); err != nil { // 通过lookupd查询
		return err
	}
	return nil

}

func main() {
	err := initConsumer("topic_demo", "first", "127.0.0.1:4161")
	if err != nil {
		fmt.Printf("init consumer failed, err:%v\n", err)
		return
	}
	c := make(chan os.Signal)        // 定义一个信号的通道
	signal.Notify(c, syscall.SIGINT) // 转发键盘中断信号到c
	<-c                              // 阻塞
}
```

将上面的代码保存之后编译执行，就能够获取之前我们publish的两条消息了：

```bash
$ ./nsq_consumer 
2018/10/22 18:49:06 INF    1 [topic_demo/first] querying nsqlookupd http://127.0.0.1:4161/lookup?topic=topic_demo
2018/10/22 18:49:06 INF    1 [topic_demo/first] (127.0.0.1:4150) connecting to nsqd
沙河1号 recv from 127.0.0.1:4150, msg:123
沙河1号 recv from 127.0.0.1:4150, msg:456
```

同时在nsqadmin的`/counter`页面查看到处理的数据数量为2。![nsqadmin界面5](https://www.liwenzhou.com/images/Go/nsq/nsqadmin6.png)

关于`go-nsq`的更多内容请阅读[go-nsq的官方文档](https://godoc.org/github.com/nsqio/go-nsq)。



## tailf 日志收集

`tailf`是一个第三方库`github.com/hpcloud/tail`，常用于日志收集，查看log日志。



### 示例

```go
package main

import (
	"fmt"
	"time"

	"github.com/hpcloud/tail"
)

func main(){
	fileName:="./my.log"
	config:=tail.Config{ 
		ReOpen: true, //重新打开
		Follow: true, //是否跟随
		Location: &tail.SeekInfo{Offset: 0,Whence: 2},  //从文件哪个地方开始读
		MustExist: false, //文件不存在不报错
		Poll: true, //
	}
    // tail.TailFile()函数开启goroutine去读取文件，通过channel格式的tails.lines传递内容。
	tails,err:=tail.TailFile(fileName,config)
	if err!=nil{
		fmt.Printf("tail file failed,err:%v\n",err)
		return
	}

	var(
		line *tail.Line
		ok bool
	)

	for {
		line,ok = <-tails.Lines
		if !ok{
			fmt.Printf("tail file close reopen,filename:%s\n",tails.Filename)
			time.Sleep(time.Second)
			continue
		}
		fmt.Println("line:",line.Text)
	}
}
```



## sarama 写日志

往kafka中写日志可以使用第三方库 `sarama`

```go
go get github.com/Shopify/sarama

//注意：在sarama v1.20版本之后加入了zstd压缩算法，需要用到cgo，在windows平台编译时会提示类似如下错误：
# github.com/DataDog/zstd
exec:"gcc":executable file not found in %PATH%
所以，在windows平台请使用v1.19 版本的sarama
```



**示例：**

```go
package main

import (
	"fmt"
	"github.com/Shopify/sarama"
)

//基于sarama第三方库开发的 kafka client
func main() {
	config := sarama.NewConfig()
	//tailf使用
	config.Producer.RequiredAcks = sarama.WaitForAll //发送完数据需要leader和follower都确认
	config.Producer.Partitioner = sarama.NewRandomPartitioner //新选出一个partition
	config.Producer.Return.Successes = true  //成功交付的消息将在success channel返回

	//构造一个消息
	msg := &sarama.ProducerMessage{}
	msg.Topic = "web_log"
	msg.Value = sarama.StringEncoder("this is a test log")

	//连接kafka
	client, err := sarama.NewSyncProducer([]string{"127.0.0.1:9092"}, config)
	if err != nil {
		fmt.Printf("producer closed,err:%v\n", err)
		return
	}
	defer client.Close()

	//发送消息
	pid,offset,err:=client.SendMessage(msg)
	if err!=nil{
		fmt.Println("send msg failed,err:",err)
		return
	}
	fmt.Printf("pid:%v offset:%v\n",pid,offset)
}

```



## ini 配置文件解析

解析`.ini`配置文件第三方库`go get gopkg.in/ini.v1`

查看官方使用文档[开始使用 - go-ini/ini (unknwon.io)](https://ini.unknwon.io/docs/intro/getting_started)









## go语言获取系统性能数据gopsutil库

`psutil`是一个跨平台进程和系统监控的Python库，而`gopsutil`是go语言版本的实现。

Go语言部署简单、性能好的特点非常适合做一些诸如采集系统信息和监控的服务，本文介绍的[gopsutil](https://github.com/shirou/gopsutil)库是知名Python库：[psutil](https://github.com/giampaolo/psutil)的一个Go语言版本的实现。



### 安装

```go
go get github.com/shirou/gosutil
```





### 使用

#### CPU

采集CPU相关信息

```go
import "github.com/shirou/gopsutil/cpu"

//cpu information

func getCpuInfo(){
    cpuInfos,err:=cpu.Info()
    if err!=nil{
        fmt.Printf("get cpu info failed,err:%v\n",err)
        return 
    }
    for _,ci:=range cpuInfos{
        fmt.Println(ci)
    }
    
    //cpu使用率
    for {
        percent,_:=cpu.Percent(time.Second,false)
        fmt.Printf("cpu percent:%v\n",percent)
    }
}
```

获取CPU负载信息：

```go
import "github.com/shirou/gopsutil/load"

func getCpuLoad(){
    info,_:=load.Avg()
    fmt.Printf("%v\n",info)
}
```





#### Memory

```go
import "github.com/shirou/gopsutil/mem"

//mem info 

func getMemInfo(){
    memInfo ,_ :=mem.VirtualMemory()
    fmt.Printf("mem info:%v\n",memInfo)
}
```



#### Host

```go
import "github.com/shirou/gopsutil/host"

//host info 

func getHostInfo(){
    hInfo,_:=host.Info()
    fmt.Printf("host info:%v uptime:%v boottime:%v\n",hInfo,hInfo.Uptime,hInfo.BootTime)
}
```



#### Disk

```go
import "github.com/shirou/gopsutil/disk"

//disk info

func getDiskInfo(){
    parts,err:= disk.Partitions(true)
    if err!=nil{
        fmt.Printf("get Partitions failed,err:%v\n",err)
        return 
    }
    for _,part:=range parts{
        fmt.Printf("part:%v\n",part.String())
        diskInfo,_:=disk.Usage(part.Mountpoint)
        fmt.Printf("disk info: used:%v,free:%V\n",diskInfo.UsedPercent,diskInfo.Free)
    }
    ioStat,_:=disk.IOCounters()
    for k,v:=range ioStat{
        fmt.Printf("%v:%v\n",k,v)
    }
}
```



#### net IO 

```go
import "github.com/shirou/gopsutil/net"

func getNetInfo(){
    info,_:=net.IOcounters(true)
    for index,v:=range info{
        fmt.Printf("%v:%v send:%v recv:%v\n",index,v,v.BytesSent,v.BytesRecv)
    }
}
```





### net

#### 获取本机IP的两种方式

```go
func GetLocalIP()(ip string,err error){
    addrs,err:=net.InterfaceAddrs()
    if err!=nil{
        return 
    }
    for _,addr:=range addrs{
        ipAddr,ok:=addr.(*net.IPNet)
        if !ok{
            continue
        }
        if ipAddr.IP.IsLoopback(){
            continue
        }
        if !ipAddr.IP.IsGlobalUnicast(){
            continue
        }
        return ipAddr.IP.String(),nil
    }
    return 
}
```

或：

```go
//Get perferred outbound ip of this machine

func GetOutboundIP()string{
    conn,err:=net.Dial("udp","8.8.8.8:80")
    if err!=nil{
        log.Fatal(err)
    }
    defer conn.Close()
    localAddr:=conn.LocalAddr().(*net.UDPAddr)
    fmt.Println(localAddr.String())
    return localAddr.IP.String()
}
```



## influxDB

本文介绍了`influxDB`时序数据库及Go语言操作`influxDB`

[InfluxDB](https://www.influxdata.com/)是一个开源分布式时序、事件和指标数据库。使用Go语言编写，无需外部依赖。其设计目标是实现分布式和水平伸缩扩展。



### 安装

#### 下载

https://portal.influxdata.com/downloads/

这里需要注意因为这个网站引用了google的api所以国内点页面的按钮是没反应的，怎么办呢？

按照下图所示，按`F12`打开浏览器的控制台，然后点击`Elements`，按下`Ctrl/Command+F`搜索`releases/influxdb`，按回车查找自己所需版本的下载地址。![download influxDB](https://www.liwenzhou.com/images/Go/influxDB/influxdb_01.png)

Mac和Linux用户可以点击https://v2.docs.influxdata.com/v2.0/get-started/下载。



#### 安装

将上一步的压缩包，解压到本地。



### influxDB介绍

#### 名词介绍

| influxDB名词 | 传统数据库概念 |
| :----------: | :------------: |
|   database   |     数据库     |
| measurement  |     数据表     |
|    point     |     数据行     |



#### point

influxDB中的point相当于传统数据库里的一行数据，由时间戳（time）、数据（field）、标签（tag）组成。

| Point属性 |                传统数据库概念                |
| :-------: | :------------------------------------------: |
|   time    |     每个数据记录时间，是数据库中的主索引     |
|   field   | 各种记录值（没有索引的属性），例如温度、湿度 |
|   tags    |       各种有索引的属性，例如地区、海拔       |



#### Series

`Series`相当于是 InfluxDB 中一些数据的集合，在同一个 database 中，retention policy、measurement、tag sets 完全相同的数据同属于一个 series，同一个 series 的数据在物理上会按照时间顺序排列存储在一起。

[想要了解更多](http://blog.fatedier.com/2016/08/05/detailed-in-influxdb-tsm-storage-engine-one/)



### Go操作influxDB

#### 安装

##### influxDB 1.x版本

```bash
go get github.com/influxdata/influxdb1-client/v2
```



##### influxDB 2.x版本

```bash
go get github.com/influxdata/influxdb-client-go
```



#### 基本使用

```go
package main

import (
	"fmt"
	"log"
	"time"

	client "github.com/influxdata/influxdb1-client/v2"
)

// influxdb demo

func connInflux() client.Client {
	cli, err := client.NewHTTPClient(client.HTTPConfig{
		Addr:     "http://127.0.0.1:8086",
		Username: "admin",
		Password: "",
	})
	if err != nil {
		log.Fatal(err)
	}
	return cli
}

// query
func queryDB(cli client.Client, cmd string) (res []client.Result, err error) {
	q := client.Query{
		Command:  cmd,
		Database: "test",
	}
	if response, err := cli.Query(q); err == nil {
		if response.Error() != nil {
			return res, response.Error()
		}
		res = response.Results
	} else {
		return res, err
	}
	return res, nil
}

// insert
func writesPoints(cli client.Client) {
	bp, err := client.NewBatchPoints(client.BatchPointsConfig{
		Database:  "test",
		Precision: "s", //精度，默认ns
	})
	if err != nil {
		log.Fatal(err)
	}
	tags := map[string]string{"cpu": "ih-cpu"}
	fields := map[string]interface{}{
		"idle":   201.1,
		"system": 43.3,
		"user":   86.6,
	}

	pt, err := client.NewPoint("cpu_usage", tags, fields, time.Now())
	if err != nil {
		log.Fatal(err)
	}
	bp.AddPoint(pt)
	err = cli.Write(bp)
	if err != nil {
		log.Fatal(err)
	}
	log.Println("insert success")
}

func main() {
	conn := connInflux()
	fmt.Println(conn)

	// insert
	writesPoints(conn)

	// 获取10条数据并展示
	qs := fmt.Sprintf("SELECT * FROM %s LIMIT %d", "cpu_usage", 10)
	res, err := queryDB(conn, qs)
	if err != nil {
		log.Fatal(err)
	}

	for _, row := range res[0].Series[0].Values {
		for j, value := range row {
			log.Printf("j:%d value:%v\n", j, value)
		}
	}
}
```





## Prometheus 系统监控和报警系统

`Prometheus`是一个开源的系统监控和报警系统，现在已经加入到[CNCF](https://so.csdn.net/so/search?q=CNCF&spm=1001.2101.3001.7020)基金会，成为继k8s之后第二个在CNCF托管的项目，在kubernetes容器管理系统中，通常会搭配prometheus进行监控，同时也支持多种`exporter`采集数据，还支持`pushgateway`进行数据上报，Prometheus性能足够支撑上万台规模的集群。



**prometheus的架构图**：

![image-20230203213048368](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230203213048368.png)





prometheus介绍可以参考：[Prometheus介绍_假装是小胖子的博客-CSDN博客](https://blog.csdn.net/cp3_zyh/article/details/124019043)

prometheus详细内容可以参考官网：[Prometheus - Monitoring system & time series database](https://prometheus.io/)









## IPFS

### 基本使用

```go
package utils

import (
	"bytes"
	"context"
	"encoding/base64"
	"fmt"
	"io/ioutil"
	shell "github.com/ipfs/go-ipfs-api"
)

type Ipfs struct {
	Url string
	Sh  *shell.Shell
}

func NewIpfs(url string) *Ipfs {
	sh := shell.NewShell(url)
	return &Ipfs{
		Url: url,
		Sh:  sh,
	}
}

// UploadIPFS 上传数据到ipfs
func (i *Ipfs) UploadIPFS(str string) (hash string, err error) {
	hash, err = i.Sh.Add(bytes.NewBufferString(str), shell.Pin(true))
	if err != nil {
		return
	}
	return
}

// UnPinIPFS 从ipfs上删除数据
func (i *Ipfs) UnPinIPFS(hash string) (err error) {
	err = i.Sh.Unpin(hash)
	if err != nil {
		return
	}

	err = i.Sh.Request("repo/gc", hash).
		Option("recursive", true).
		Exec(context.Background(), nil)
	if err != nil {
		return
	}

	return nil
}

// CatIPFS 从ipfs下载数据
func (i *Ipfs) CatIPFS(hash string) (string, error) {
	read, err := i.Sh.Cat(hash)
	if err != nil {
		return "", err
	}
	body, err := ioutil.ReadAll(read)

	return string(body), nil
}
```









## Golang连接以太坊智能合约

1，首先，获取智能合约的`abi`文件

2，然后，使用`abigen`生成合约交互工具`xxx.go`

3，最后，程序调用`xxx.go`实现合约的部署、方法调用等交互操作。

**官网详细用法，可以参考：[Go Contract Bindings | go-ethereum](https://geth.ethereum.org/docs/developers/dapp-developer/native-bindings)**



**github上别人用`abigen`和go与智能合约交互示例：[go-connect-eth-contract/connecter.go at master · finejian/go-connect-eth-contract · GitHub](https://github.com/finejian/go-connect-eth-contract/blob/master/connecter.go)**



**CSDN上别人用`abigen`和go与智能合约交互示例：[(35条消息) 在本地以太坊私链上，使用go调用智能合约，获取事件日志_go 调用合约_luopiao19岁青少年软件从业人员的博客-CSDN博客](https://blog.csdn.net/qq_37575994/article/details/127409552)**





### 基本用法

+ 智能合约示例

  ```solidity
  //SPDX-License-Identifier:MIT
  pragma solidity ^0.8.0;
  
  contract Hash{
      mapping(uint256 =>string ) public hashData;
  
      function setHash(uint256 _id,string memory _hash) external {
          hashData[_id] = _hash;
      }
  
      function getHash(uint256 _id) external view returns(string memory hash){
          return hashData[_id];
      }
  }
  ```

  

+ 利用智能合约的abi，生成能智能合约交互的go代码

  ```go
  abigen.exe --abi Hash.abi --bin Hash.bin --pkg contract --type Hash --out Hash.go
  ```

+ 生成的go代码

  ```go
  //Hash.go
  
  package contract
  
  import (
  	"errors"
  	"math/big"
  	"strings"
  
  	ethereum "github.com/ethereum/go-ethereum"
  	"github.com/ethereum/go-ethereum/accounts/abi"
  	"github.com/ethereum/go-ethereum/accounts/abi/bind"
  	"github.com/ethereum/go-ethereum/common"
  	"github.com/ethereum/go-ethereum/core/types"
  	"github.com/ethereum/go-ethereum/event"
  )
  
  // Reference imports to suppress errors if they are not otherwise used.
  var (
  	_ = errors.New
  	_ = big.NewInt
  	_ = strings.NewReader
  	_ = ethereum.NotFound
  	_ = bind.Bind
  	_ = common.Big1
  	_ = types.BloomLookup
  	_ = event.NewSubscription
  	_ = abi.ConvertType
  )
  
  // HashMetaData contains all meta data concerning the Hash contract.
  var HashMetaData = &bind.MetaData{
  	ABI: "[{\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"_id\",\"type\":\"uint256\"}],\"name\":\"getHash\",\"outputs\":[{\"internalType\":\"string\",\"name\":\"hash\",\"type\":\"string\"}],\"stateMutability\":\"view\",\"type\":\"function\"},{\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"name\":\"hashData\",\"outputs\":[{\"internalType\":\"string\",\"name\":\"\",\"type\":\"string\"}],\"stateMutability\":\"view\",\"type\":\"function\"},{\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"_id\",\"type\":\"uint256\"},{\"internalType\":\"string\",\"name\":\"_hash\",\"type\":\"string\"}],\"name\":\"setHash\",\"outputs\":[],\"stateMutability\":\"nonpayable\",\"type\":\"function\"}]",
  	Bin: `0x{"functionDebugData": {},"generatedSources": [],"linkReferences": {},"object": "608060405234801561001057600080fd5b506107f8806100206000396000f3fe608060405234801561001057600080fd5b50600436106100415760003560e01c80632fbaba26146100465780636b2fafa9146100625780639512182e14610092575b600080fd5b610060600480360381019061005b91906103ba565b6100c2565b005b61007c60048036038101906100779190610416565b6100e6565b60405161008991906104c2565b60405180910390f35b6100ac60048036038101906100a79190610416565b61018a565b6040516100b991906104c2565b60405180910390f35b8060008084815260200190815260200160002090816100e191906106f0565b505050565b6060600080838152602001908152602001600020805461010590610513565b80601f016020809104026020016040519081016040528092919081815260200182805461013190610513565b801561017e5780601f106101535761010080835404028352916020019161017e565b820191906000526020600020905b81548152906001019060200180831161016157829003601f168201915b50505050509050919050565b600060205280600052604060002060009150905080546101a990610513565b80601f01602080910402602001604051908101604052809291908181526020018280546101d590610513565b80156102225780601f106101f757610100808354040283529160200191610222565b820191906000526020600020905b81548152906001019060200180831161020557829003601f168201915b505050505081565b6000604051905090565b600080fd5b600080fd5b6000819050919050565b6102518161023e565b811461025c57600080fd5b50565b60008135905061026e81610248565b92915050565b600080fd5b600080fd5b6000601f19601f8301169050919050565b7f4e487b7100000000000000000000000000000000000000000000000000000000600052604160045260246000fd5b6102c78261027e565b810181811067ffffffffffffffff821117156102e6576102e561028f565b5b80604052505050565b60006102f961022a565b905061030582826102be565b919050565b600067ffffffffffffffff8211156103255761032461028f565b5b61032e8261027e565b9050602081019050919050565b82818337600083830152505050565b600061035d6103588461030a565b6102ef565b90508281526020810184848401111561037957610378610279565b5b61038484828561033b565b509392505050565b600082601f8301126103a1576103a0610274565b5b81356103b184826020860161034a565b91505092915050565b600080604083850312156103d1576103d0610234565b5b60006103df8582860161025f565b925050602083013567ffffffffffffffff811115610400576103ff610239565b5b61040c8582860161038c565b9150509250929050565b60006020828403121561042c5761042b610234565b5b600061043a8482850161025f565b91505092915050565b600081519050919050565b600082825260208201905092915050565b60005b8381101561047d578082015181840152602081019050610462565b60008484015250505050565b600061049482610443565b61049e818561044e565b93506104ae81856020860161045f565b6104b78161027e565b840191505092915050565b600060208201905081810360008301526104dc8184610489565b905092915050565b7f4e487b7100000000000000000000000000000000000000000000000000000000600052602260045260246000fd5b6000600282049050600182168061052b57607f821691505b60208210810361053e5761053d6104e4565b5b50919050565b60008190508160005260206000209050919050565b60006020601f8301049050919050565b600082821b905092915050565b6000600883026105a67fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff82610569565b6105b08683610569565b95508019841693508086168417925050509392505050565b6000819050919050565b60006105ed6105e86105e38461023e565b6105c8565b61023e565b9050919050565b6000819050919050565b610607836105d2565b61061b610613826105f4565b848454610576565b825550505050565b600090565b610630610623565b61063b8184846105fe565b505050565b5b8181101561065f57610654600082610628565b600181019050610641565b5050565b601f8211156106a45761067581610544565b61067e84610559565b8101602085101561068d578190505b6106a161069985610559565b830182610640565b50505b505050565b600082821c905092915050565b60006106c7600019846008026106a9565b1980831691505092915050565b60006106e083836106b6565b9150826002028217905092915050565b6106f982610443565b67ffffffffffffffff8111156107125761071161028f565b5b61071c8254610513565b610727828285610663565b600060209050601f83116001811461075a5760008415610748578287015190505b61075285826106d4565b8655506107ba565b601f19841661076886610544565b60005b828110156107905784890151825560018201915060208501945060208101905061076b565b868310156107ad57848901516107a9601f8916826106b6565b8355505b6001600288020188555050505b50505050505056fea264697066735822122089239483b4da47f09cfd9ed82812704312665243ace75ea2fe0fe1d3aa5297e864736f6c63430008120033","opcodes": "PUSH1 0x80 PUSH1 0x40 MSTORE CALLVALUE DUP1 ISZERO PUSH2 0x10 JUMPI PUSH1 0x0 DUP1 REVERT JUMPDEST POP PUSH2 0x7F8 DUP1 PUSH2 0x20 PUSH1 0x0 CODECOPY PUSH1 0x0 RETURN INVALID PUSH1 0x80 PUSH1 0x40 MSTORE CALLVALUE DUP1 ISZERO PUSH2 0x10 JUMPI PUSH1 0x0 DUP1 REVERT JUMPDEST POP PUSH1 0x4 CALLDATASIZE LT PUSH2 0x41 JUMPI PUSH1 0x0 CALLDATALOAD PUSH1 0xE0 SHR DUP1 PUSH4 0x2FBABA26 EQ PUSH2 0x46 JUMPI DUP1 PUSH4 0x6B2FAFA9 EQ PUSH2 0x62 JUMPI DUP1 PUSH4 0x9512182E EQ PUSH2 0x92 JUMPI JUMPDEST PUSH1 0x0 DUP1REVERT JUMPDEST PUSH2 0x60 PUSH1 0x4 DUP1 CALLDATASIZE SUB DUP2 ADD SWAP1 PUSH2 0x5B SWAP2 SWAP1 PUSH2 0x3BA JUMP JUMPDEST PUSH2 0xC2 JUMP JUMPDEST STOP JUMPDEST PUSH2 0x7C PUSH1 0x4 DUP1 CALLDATASIZE SUB DUP2 ADD SWAP1 PUSH2 0x77 SWAP2 SWAP1 PUSH2 0x416 JUMP JUMPDEST PUSH2 0xE6 JUMP JUMPDEST PUSH1 0x40 MLOAD PUSH2 0x89 SWAP2 SWAP1 PUSH2 0x4C2 JUMP JUMPDEST PUSH1 0x40 MLOAD DUP1 SWAP2 SUB SWAP1 RETURN JUMPDEST PUSH2 0xAC PUSH1 0x4 DUP1 CALLDATASIZE SUB DUP2 ADD SWAP1 PUSH2 0xA7 SWAP2 SWAP1 PUSH2 0x416 JUMP JUMPDEST PUSH20x18A JUMP JUMPDEST PUSH1 0x40 MLOAD PUSH2 0xB9 SWAP2 SWAP1 PUSH2 0x4C2 JUMP JUMPDEST PUSH1 0x40 MLOAD DUP1 SWAP2 SUB SWAP1 RETURN JUMPDEST DUP1 PUSH1 0x0 DUP1 DUP5 DUP2 MSTORE PUSH1 0x20 ADD SWAP1 DUP2 MSTORE PUSH1 0x20 ADD PUSH1 0x0 KECCAK256 SWAP1 DUP2 PUSH2 0xE1 SWAP2 SWAP1 PUSH2 0x6F0 JUMP JUMPDEST POP POP POP JUMP JUMPDEST PUSH1 0x60 PUSH1 0x0 DUP1 DUP4 DUP2 MSTORE PUSH1 0x20 ADD SWAP1 DUP2 MSTORE PUSH1 0x20 ADD PUSH1 0x0 KECCAK256 DUP1 SLOAD PUSH2 0x105 SWAP1 PUSH2 0x513 JUMP JUMPDEST DUP1 PUSH1 0x1F ADD PUSH1 0x20DUP1 SWAP2 DIV MUL PUSH1 0x20 ADD PUSH1 0x40 MLOAD SWAP1 DUP2 ADD PUSH1 0x40 MSTORE DUP1 SWAP3 SWAP2 SWAP1 DUP2 DUP2 MSTORE PUSH1 0x20 ADD DUP3 DUP1 SLOAD PUSH2 0x131 SWAP1 PUSH2 0x513 JUMP JUMPDEST DUP1 ISZERO PUSH2 0x17E JUMPI DUP1 PUSH1 0x1F LT PUSH2 0x153 JUMPI PUSH2 0x100 DUP1 DUP4 SLOAD DIV MUL DUP4 MSTORE SWAP2 PUSH1 0x20 ADD SWAP2 PUSH2 0x17E JUMP JUMPDEST DUP3 ADD SWAP2 SWAP1 PUSH1 0x0 MSTORE PUSH1 0x20 PUSH1 0x0 KECCAK256 SWAP1 JUMPDEST DUP2 SLOAD DUP2 MSTORE SWAP1 PUSH1 0x1 ADD SWAP1 PUSH1 0x20 ADD DUP1 DUP4 GTPUSH2 0x161 JUMPI DUP3 SWAP1 SUB PUSH1 0x1F AND DUP3 ADD SWAP2 JUMPDEST POP POP POP POP POP SWAP1 POP SWAP2 SWAP1 POP JUMP JUMPDESTPUSH1 0x0 PUSH1 0x20 MSTORE DUP1 PUSH1 0x0 MSTORE PUSH1 0x40 PUSH1 0x0 KECCAK256 PUSH1 0x0 SWAP2 POP SWAP1 POP DUP1 SLOAD PUSH2 0x1A9 SWAP1 PUSH2 0x513 JUMP JUMPDEST DUP1 PUSH1 0x1F ADD PUSH1 0x20 DUP1 SWAP2 DIV MUL PUSH1 0x20 ADD PUSH1 0x40 MLOAD SWAP1 DUP2 ADD PUSH1 0x40 MSTORE DUP1 SWAP3 SWAP2 SWAP1 DUP2 DUP2 MSTORE PUSH1 0x20 ADD DUP3 DUP1 SLOAD PUSH2 0x1D5 SWAP1 PUSH2 0x513 JUMP JUMPDESTDUP1 ISZERO PUSH2 0x222 JUMPI DUP1 PUSH1 0x1F LT PUSH2 0x1F7 JUMPI PUSH2 0x100 DUP1 DUP4 SLOAD DIV MUL DUP4 MSTORE SWAP2 PUSH1 0x20ADD SWAP2 PUSH2 0x222 JUMP JUMPDEST DUP3 ADD SWAP2 SWAP1 PUSH1 0x0 MSTORE PUSH1 0x20 PUSH1 0x0 KECCAK256 SWAP1 JUMPDEST DUP2 SLOAD DUP2 MSTORE SWAP1 PUSH1 0x1 ADD SWAP1 PUSH1 0x20 ADD DUP1 DUP4 GT PUSH2 0x205 JUMPI DUP3 SWAP1 SUB PUSH1 0x1F AND DUP3 ADD SWAP2 JUMPDEST POP POP POP POP POP DUP2 JUMP JUMPDEST PUSH1 0x0 PUSH1 0x40 MLOAD SWAP1 POP SWAP1 JUMP JUMPDEST PUSH1 0x0 DUP1 REVERT JUMPDESTPUSH1 0x0 DUP1 REVERT JUMPDEST PUSH1 0x0 DUP2 SWAP1 POP SWAP2 SWAP1 POP JUMP JUMPDEST PUSH2 0x251 DUP2 PUSH2 0x23E JUMP JUMPDEST DUP2 EQ PUSH2 0x25C JUMPI PUSH1 0x0 DUP1 REVERT JUMPDEST POP JUMP JUMPDEST PUSH1 0x0 DUP2 CALLDATALOAD SWAP1 POP PUSH2 0x26E DUP2 PUSH20x248 JUMP JUMPDEST SWAP3 SWAP2 POP POP JUMP JUMPDEST PUSH1 0x0 DUP1 REVERT JUMPDEST PUSH1 0x0 DUP1 REVERT JUMPDEST PUSH1 0x0 PUSH10x1F NOT PUSH1 0x1F DUP4 ADD AND SWAP1 POP SWAP2 SWAP1 POP JUMP JUMPDEST PUSH32 0x4E487B7100000000000000000000000000000000000000000000000000000000 PUSH1 0x0 MSTORE PUSH1 0x41 PUSH1 0x4 MSTORE PUSH1 0x24 PUSH1 0x0 REVERT JUMPDEST PUSH2 0x2C7 DUP3 PUSH2 0x27E JUMPJUMPDEST DUP2 ADD DUP2 DUP2 LT PUSH8 0xFFFFFFFFFFFFFFFF DUP3 GT OR ISZERO PUSH2 0x2E6 JUMPI PUSH2 0x2E5 PUSH2 0x28F JUMP JUMPDEST JUMPDEST DUP1 PUSH1 0x40 MSTORE POP POP POP JUMP JUMPDEST PUSH1 0x0 PUSH2 0x2F9 PUSH2 0x22A JUMP JUMPDEST SWAP1 POP PUSH2 0x305 DUP3 DUP3 PUSH2 0x2BE JUMP JUMPDEST SWAP2 SWAP1 POP JUMP JUMPDEST PUSH1 0x0 PUSH8 0xFFFFFFFFFFFFFFFF DUP3 GT ISZERO PUSH2 0x325 JUMPI PUSH2 0x324 PUSH2 0x28F JUMP JUMPDEST JUMPDEST PUSH2 0x32E DUP3 PUSH2 0x27E JUMP JUMPDEST SWAP1 POP PUSH1 0x20 DUP2 ADD SWAP1 POP SWAP2SWAP1 POP JUMP JUMPDEST DUP3 DUP2 DUP4 CALLDATACOPY PUSH1 0x0 DUP4 DUP4 ADD MSTORE POP POP POP JUMP JUMPDEST PUSH1 0x0 PUSH2 0x35D PUSH2 0x358 DUP5 PUSH2 0x30A JUMP JUMPDEST PUSH2 0x2EF JUMP JUMPDEST SWAP1 POP DUP3 DUP2 MSTORE PUSH1 0x20 DUP2 ADD DUP5 DUP5 DUP5 ADD GT ISZERO PUSH2 0x379 JUMPI PUSH2 0x378 PUSH2 0x279 JUMP JUMPDEST JUMPDEST PUSH2 0x384 DUP5 DUP3 DUP6 PUSH2 0x33B JUMP JUMPDEST POP SWAP4 SWAP3 POP POP POP JUMP JUMPDEST PUSH1 0x0 DUP3 PUSH1 0x1F DUP4 ADD SLT PUSH2 0x3A1 JUMPI PUSH2 0x3A0 PUSH2 0x274 JUMP JUMPDEST JUMPDEST DUP2 CALLDATALOAD PUSH2 0x3B1 DUP5 DUP3 PUSH1 0x20 DUP7 ADD PUSH2 0x34A JUMP JUMPDEST SWAP2 POP POP SWAP3 SWAP2 POP POPJUMP JUMPDEST PUSH1 0x0 DUP1 PUSH1 0x40 DUP4 DUP6 SUB SLT ISZERO PUSH2 0x3D1 JUMPI PUSH2 0x3D0 PUSH2 0x234 JUMP JUMPDEST JUMPDEST PUSH1 0x0 PUSH2 0x3DF DUP6 DUP3 DUP7 ADD PUSH2 0x25F JUMP JUMPDEST SWAP3 POP POP PUSH1 0x20 DUP4 ADD CALLDATALOAD PUSH8 0xFFFFFFFFFFFFFFFF DUP2 GT ISZERO PUSH2 0x400 JUMPI PUSH2 0x3FF PUSH2 0x239 JUMP JUMPDEST JUMPDEST PUSH2 0x40C DUP6 DUP3 DUP7 ADD PUSH2 0x38C JUMPJUMPDEST SWAP2 POP POP SWAP3 POP SWAP3 SWAP1 POP JUMP JUMPDEST PUSH1 0x0 PUSH1 0x20 DUP3 DUP5 SUB SLT ISZERO PUSH2 0x42C JUMPI PUSH2 0x42B PUSH2 0x234 JUMP JUMPDEST JUMPDEST PUSH1 0x0 PUSH2 0x43A DUP5 DUP3 DUP6 ADD PUSH2 0x25F JUMP JUMPDEST SWAP2 POP POP SWAP3 SWAP2 POP POP JUMP JUMPDEST PUSH1 0x0 DUP2 MLOAD SWAP1 POP SWAP2 SWAP1 POP JUMP JUMPDEST PUSH1 0x0 DUP3 DUP3 MSTORE PUSH1 0x20 DUP3 ADD SWAP1 POP SWAP3 SWAP2 POP POP JUMP JUMPDEST PUSH1 0x0 JUMPDEST DUP4 DUP2 LT ISZERO PUSH2 0x47D JUMPI DUP1 DUP3 ADD MLOAD DUP2 DUP5ADD MSTORE PUSH1 0x20 DUP2 ADD SWAP1 POP PUSH2 0x462 JUMP JUMPDEST PUSH1 0x0 DUP5 DUP5 ADD MSTORE POP POP POP POP JUMP JUMPDEST PUSH1 0x0 PUSH2 0x494 DUP3 PUSH2 0x443 JUMP JUMPDEST PUSH2 0x49E DUP2 DUP6 PUSH2 0x44E JUMP JUMPDEST SWAP4 POP PUSH2 0x4AE DUP2 DUP6 PUSH1 0x20 DUP7 ADD PUSH2 0x45F JUMP JUMPDEST PUSH2 0x4B7 DUP2 PUSH2 0x27E JUMP JUMPDEST DUP5 ADD SWAP2 POP POP SWAP3 SWAP2 POP POP JUMP JUMPDEST PUSH1 0x0 PUSH1 0x20 DUP3 ADD SWAP1 POP DUP2 DUP2 SUB PUSH1 0x0 DUP4 ADD MSTORE PUSH2 0x4DC DUP2 DUP5 PUSH2 0x489 JUMP JUMPDEST SWAP1 POP SWAP3 SWAP2 POP POP JUMP JUMPDEST PUSH32 0x4E487B7100000000000000000000000000000000000000000000000000000000 PUSH10x0 MSTORE PUSH1 0x22 PUSH1 0x4 MSTORE PUSH1 0x24 PUSH1 0x0 REVERT JUMPDEST PUSH1 0x0 PUSH1 0x2 DUP3 DIV SWAP1 POP PUSH1 0x1 DUP3 AND DUP1 PUSH2 0x52B JUMPI PUSH1 0x7F DUP3 AND SWAP2 POP JUMPDEST PUSH1 0x20 DUP3 LT DUP2 SUB PUSH2 0x53E JUMPI PUSH2 0x53D PUSH2 0x4E4 JUMP JUMPDEST JUMPDEST POP SWAP2 SWAP1 POP JUMP JUMPDEST PUSH1 0x0 DUP2 SWAP1 POP DUP2 PUSH1 0x0 MSTORE PUSH1 0x20 PUSH1 0x0 KECCAK256 SWAP1 POP SWAP2 SWAP1 POP JUMP JUMPDEST PUSH1 0x0 PUSH1 0x20 PUSH1 0x1F DUP4 ADD DIV SWAP1 POP SWAP2 SWAP1 POP JUMP JUMPDEST PUSH1 0x0 DUP3 DUP3 SHL SWAP1 POP SWAP3 SWAP2 POP POP JUMP JUMPDEST PUSH1 0x0 PUSH1 0x8 DUP4 MUL PUSH2 0x5A6 PUSH32 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF DUP3 PUSH2 0x569 JUMP JUMPDEST PUSH2 0x5B0 DUP7 DUP4 PUSH2 0x569 JUMP JUMPDEST SWAP6 POP DUP1 NOT DUP5 AND SWAP4 POP DUP1 DUP7 AND DUP5 OR SWAP3 POP POP POP SWAP4 SWAP3 POP POP POP JUMP JUMPDEST PUSH1 0x0 DUP2 SWAP1POP SWAP2 SWAP1 POP JUMP JUMPDEST PUSH1 0x0 PUSH2 0x5ED PUSH2 0x5E8 PUSH2 0x5E3 DUP5 PUSH2 0x23E JUMP JUMPDEST PUSH2 0x5C8 JUMP JUMPDEST PUSH2 0x23E JUMP JUMPDEST SWAP1 POP SWAP2 SWAP1 POP JUMP JUMPDEST PUSH1 0x0 DUP2 SWAP1 POP SWAP2 SWAP1 POP JUMP JUMPDEST PUSH20x607 DUP4 PUSH2 0x5D2 JUMP JUMPDEST PUSH2 0x61B PUSH2 0x613 DUP3 PUSH2 0x5F4 JUMP JUMPDEST DUP5 DUP5 SLOAD PUSH2 0x576 JUMP JUMPDEST DUP3 SSTORE POP POP POP POP JUMP JUMPDEST PUSH1 0x0 SWAP1 JUMP JUMPDEST PUSH2 0x630 PUSH2 0x623 JUMP JUMPDEST PUSH2 0x63B DUP2 DUP5 DUP5 PUSH2 0x5FE JUMP JUMPDEST POP POP POP JUMP JUMPDEST JUMPDEST DUP2 DUP2 LT ISZERO PUSH2 0x65F JUMPI PUSH2 0x654 PUSH1 0x0 DUP3 PUSH2 0x628 JUMP JUMPDEST PUSH1 0x1 DUP2 ADD SWAP1 POP PUSH2 0x641 JUMP JUMPDEST POP POP JUMP JUMPDEST PUSH1 0x1F DUP3 GT ISZERO PUSH2 0x6A4 JUMPI PUSH2 0x675 DUP2 PUSH2 0x544 JUMP JUMPDEST PUSH2 0x67E DUP5 PUSH2 0x559 JUMP JUMPDEST DUP2 ADD PUSH1 0x20 DUP6 LT ISZERO PUSH2 0x68D JUMPI DUP2 SWAP1 POP JUMPDEST PUSH2 0x6A1 PUSH2 0x699 DUP6 PUSH2 0x559 JUMP JUMPDEST DUP4 ADD DUP3 PUSH2 0x640 JUMP JUMPDEST POP POP JUMPDEST POP POP POP JUMP JUMPDEST PUSH1 0x0 DUP3 DUP3 SHR SWAP1 POP SWAP3 SWAP2 POP POP JUMP JUMPDEST PUSH1 0x0PUSH2 0x6C7 PUSH1 0x0 NOT DUP5 PUSH1 0x8 MUL PUSH2 0x6A9 JUMP JUMPDEST NOT DUP1 DUP4 AND SWAP2 POP POP SWAP3 SWAP2 POP POP JUMP JUMPDEST PUSH1 0x0 PUSH2 0x6E0 DUP4 DUP4 PUSH2 0x6B6 JUMP JUMPDEST SWAP2 POP DUP3 PUSH1 0x2 MUL DUP3 OR SWAP1 POP SWAP3 SWAP2 POP POP JUMP JUMPDEST PUSH2 0x6F9 DUP3 PUSH2 0x443 JUMP JUMPDEST PUSH8 0xFFFFFFFFFFFFFFFF DUP2 GT ISZERO PUSH2 0x712 JUMPI PUSH2 0x711 PUSH2 0x28F JUMP JUMPDEST JUMPDEST PUSH2 0x71C DUP3 SLOAD PUSH2 0x513 JUMP JUMPDEST PUSH2 0x727 DUP3 DUP3 DUP6 PUSH2 0x663 JUMP JUMPDEST PUSH1 0x0 PUSH1 0x20 SWAP1 POP PUSH1 0x1F DUP4 GT PUSH1 0x1 DUP2 EQ PUSH2 0x75A JUMPI PUSH1 0x0 DUP5 ISZERO PUSH2 0x748 JUMPI DUP3 DUP8 ADD MLOAD SWAP1 POP JUMPDEST PUSH2 0x752 DUP6 DUP3 PUSH2 0x6D4 JUMP JUMPDEST DUP7 SSTORE POP PUSH2 0x7BA JUMP JUMPDEST PUSH1 0x1FNOT DUP5 AND PUSH2 0x768 DUP7 PUSH2 0x544 JUMP JUMPDEST PUSH1 0x0 JUMPDEST DUP3 DUP2 LT ISZERO PUSH2 0x790 JUMPI DUP5 DUP10 ADD MLOAD DUP3 SSTORE PUSH1 0x1 DUP3 ADD SWAP2 POP PUSH1 0x20 DUP6 ADD SWAP5 POP PUSH1 0x20 DUP2 ADD SWAP1 POP PUSH2 0x76B JUMP JUMPDEST DUP7 DUP4 LT ISZERO PUSH2 0x7AD JUMPI DUP5 DUP10 ADD MLOAD PUSH2 0x7A9 PUSH1 0x1F DUP10 AND DUP3 PUSH2 0x6B6 JUMP JUMPDEST DUP4 SSTOREPOP JUMPDEST PUSH1 0x1 PUSH1 0x2 DUP9 MUL ADD DUP9 SSTORE POP POP POP JUMPDEST POP POP POP POP POP POP JUMP INVALID LOG2 PUSH5 0x6970667358 0x22 SLT KECCAK256 DUP10 0x23 SWAP5 DUP4 0xB4 0xDA SELFBALANCE CREATE SWAP13 REVERT SWAP15 0xD8 0x28 SLT PUSH17 0x4312665243ACE75EA2FE0FE1D3AA5297E8 PUSH5 0x736F6C6343 STOP ADDMOD SLT STOP CALLER ","sourceMap": "58:291:0:-:0;;;;;;;;;;;;;;;;;;;"}`,
  }
  
  // HashABI is the input ABI used to generate the binding from.
  // Deprecated: Use HashMetaData.ABI instead.
  var HashABI = HashMetaData.ABI
  
  // HashBin is the compiled bytecode used for deploying new contracts.
  // Deprecated: Use HashMetaData.Bin instead.
  var HashBin = HashMetaData.Bin
  
  // DeployHash deploys a new Ethereum contract, binding an instance of Hash to it.
  func DeployHash(auth *bind.TransactOpts, backend bind.ContractBackend) (common.Address, *types.Transaction, *Hash, error) {
  	parsed, err := HashMetaData.GetAbi()
  	if err != nil {
  		return common.Address{}, nil, nil, err
  	}
  	if parsed == nil {
  		return common.Address{}, nil, nil, errors.New("GetABI returned nil")
  	}
  
  	address, tx, contract, err := bind.DeployContract(auth, *parsed, common.FromHex(HashBin), backend)
  	if err != nil {
  		return common.Address{}, nil, nil, err
  	}
  	return address, tx, &Hash{HashCaller: HashCaller{contract: contract}, HashTransactor: HashTransactor{contract: contract}, HashFilterer: HashFilterer{contract: contract}}, nil
  }
  
  // Hash is an auto generated Go binding around an Ethereum contract.
  type Hash struct {
  	HashCaller     // Read-only binding to the contract
  	HashTransactor // Write-only binding to the contract
  	HashFilterer   // Log filterer for contract events
  }
  
  // HashCaller is an auto generated read-only Go binding around an Ethereum contract.
  type HashCaller struct {
  	contract *bind.BoundContract // Generic contract wrapper for the low level calls
  }
  
  // HashTransactor is an auto generated write-only Go binding around an Ethereum contract.
  type HashTransactor struct {
  	contract *bind.BoundContract // Generic contract wrapper for the low level calls
  }
  
  // HashFilterer is an auto generated log filtering Go binding around an Ethereum contract events.
  type HashFilterer struct {
  	contract *bind.BoundContract // Generic contract wrapper for the low level calls
  }
  
  // HashSession is an auto generated Go binding around an Ethereum contract,
  // with pre-set call and transact options.
  type HashSession struct {
  	Contract     *Hash             // Generic contract binding to set the session for
  	CallOpts     bind.CallOpts     // Call options to use throughout this session
  	TransactOpts bind.TransactOpts // Transaction auth options to use throughout this session
  }
  
  // HashCallerSession is an auto generated read-only Go binding around an Ethereum contract,
  // with pre-set call options.
  type HashCallerSession struct {
  	Contract *HashCaller   // Generic contract caller binding to set the session for
  	CallOpts bind.CallOpts // Call options to use throughout this session
  }
  
  // HashTransactorSession is an auto generated write-only Go binding around an Ethereum contract,
  // with pre-set transact options.
  type HashTransactorSession struct {
  	Contract     *HashTransactor   // Generic contract transactor binding to set the session for
  	TransactOpts bind.TransactOpts // Transaction auth options to use throughout this session
  }
  
  // HashRaw is an auto generated low-level Go binding around an Ethereum contract.
  type HashRaw struct {
  	Contract *Hash // Generic contract binding to access the raw methods on
  }
  
  // HashCallerRaw is an auto generated low-level read-only Go binding around an Ethereum contract.
  type HashCallerRaw struct {
  	Contract *HashCaller // Generic read-only contract binding to access the raw methods on
  }
  
  // HashTransactorRaw is an auto generated low-level write-only Go binding around an Ethereum contract.
  type HashTransactorRaw struct {
  	Contract *HashTransactor // Generic write-only contract binding to access the raw methods on
  }
  
  // NewHash creates a new instance of Hash, bound to a specific deployed contract.
  func NewHash(address common.Address, backend bind.ContractBackend) (*Hash, error) {
  	contract, err := bindHash(address, backend, backend, backend)
  	if err != nil {
  		return nil, err
  	}
  	return &Hash{HashCaller: HashCaller{contract: contract}, HashTransactor: HashTransactor{contract: contract}, HashFilterer: HashFilterer{contract: contract}}, nil
  }
  
  // NewHashCaller creates a new read-only instance of Hash, bound to a specific deployed contract.
  func NewHashCaller(address common.Address, caller bind.ContractCaller) (*HashCaller, error) {
  	contract, err := bindHash(address, caller, nil, nil)
  	if err != nil {
  		return nil, err
  	}
  	return &HashCaller{contract: contract}, nil
  }
  
  // NewHashTransactor creates a new write-only instance of Hash, bound to a specific deployed contract.
  func NewHashTransactor(address common.Address, transactor bind.ContractTransactor) (*HashTransactor, error) {
  	contract, err := bindHash(address, nil, transactor, nil)
  	if err != nil {
  		return nil, err
  	}
  	return &HashTransactor{contract: contract}, nil
  }
  
  // NewHashFilterer creates a new log filterer instance of Hash, bound to a specific deployed contract.
  func NewHashFilterer(address common.Address, filterer bind.ContractFilterer) (*HashFilterer, error) {
  	contract, err := bindHash(address, nil, nil, filterer)
  	if err != nil {
  		return nil, err
  	}
  	return &HashFilterer{contract: contract}, nil
  }
  
  // bindHash binds a generic wrapper to an already deployed contract.
  func bindHash(address common.Address, caller bind.ContractCaller, transactor bind.ContractTransactor, filterer bind.ContractFilterer) (*bind.BoundContract, error) {
  	parsed, err := HashMetaData.GetAbi()
  	if err != nil {
  		return nil, err
  	}
  	return bind.NewBoundContract(address, *parsed, caller, transactor, filterer), nil
  }
  
  // Call invokes the (constant) contract method with params as input values and
  // sets the output to result. The result type might be a single field for simple
  // returns, a slice of interfaces for anonymous returns and a struct for named
  // returns.
  func (_Hash *HashRaw) Call(opts *bind.CallOpts, result *[]interface{}, method string, params ...interface{}) error {
  	return _Hash.Contract.HashCaller.contract.Call(opts, result, method, params...)
  }
  
  // Transfer initiates a plain transaction to move funds to the contract, calling
  // its default method if one is available.
  func (_Hash *HashRaw) Transfer(opts *bind.TransactOpts) (*types.Transaction, error) {
  	return _Hash.Contract.HashTransactor.contract.Transfer(opts)
  }
  
  // Transact invokes the (paid) contract method with params as input values.
  func (_Hash *HashRaw) Transact(opts *bind.TransactOpts, method string, params ...interface{}) (*types.Transaction, error) {
  	return _Hash.Contract.HashTransactor.contract.Transact(opts, method, params...)
  }
  
  // Call invokes the (constant) contract method with params as input values and
  // sets the output to result. The result type might be a single field for simple
  // returns, a slice of interfaces for anonymous returns and a struct for named
  // returns.
  func (_Hash *HashCallerRaw) Call(opts *bind.CallOpts, result *[]interface{}, method string, params ...interface{}) error {
  	return _Hash.Contract.contract.Call(opts, result, method, params...)
  }
  
  // Transfer initiates a plain transaction to move funds to the contract, calling
  // its default method if one is available.
  func (_Hash *HashTransactorRaw) Transfer(opts *bind.TransactOpts) (*types.Transaction, error) {
  	return _Hash.Contract.contract.Transfer(opts)
  }
  
  // Transact invokes the (paid) contract method with params as input values.
  func (_Hash *HashTransactorRaw) Transact(opts *bind.TransactOpts, method string, params ...interface{}) (*types.Transaction, error) {
  	return _Hash.Contract.contract.Transact(opts, method, params...)
  }
  
  // GetHash is a free data retrieval call binding the contract method 0x6b2fafa9.
  //
  // Solidity: function getHash(uint256 _id) view returns(string hash)
  func (_Hash *HashCaller) GetHash(opts *bind.CallOpts, _id *big.Int) (string, error) {
  	var out []interface{}
  	err := _Hash.contract.Call(opts, &out, "getHash", _id)
  
  	if err != nil {
  		return *new(string), err
  	}
  
  	out0 := *abi.ConvertType(out[0], new(string)).(*string)
  
  	return out0, err
  
  }
  
  // GetHash is a free data retrieval call binding the contract method 0x6b2fafa9.
  //
  // Solidity: function getHash(uint256 _id) view returns(string hash)
  func (_Hash *HashSession) GetHash(_id *big.Int) (string, error) {
  	return _Hash.Contract.GetHash(&_Hash.CallOpts, _id)
  }
  
  // GetHash is a free data retrieval call binding the contract method 0x6b2fafa9.
  //
  // Solidity: function getHash(uint256 _id) view returns(string hash)
  func (_Hash *HashCallerSession) GetHash(_id *big.Int) (string, error) {
  	return _Hash.Contract.GetHash(&_Hash.CallOpts, _id)
  }
  
  // HashData is a free data retrieval call binding the contract method 0x9512182e.
  //
  // Solidity: function hashData(uint256 ) view returns(string)
  func (_Hash *HashCaller) HashData(opts *bind.CallOpts, arg0 *big.Int) (string, error) {
  	var out []interface{}
  	err := _Hash.contract.Call(opts, &out, "hashData", arg0)
  
  	if err != nil {
  		return *new(string), err
  	}
  
  	out0 := *abi.ConvertType(out[0], new(string)).(*string)
  
  	return out0, err
  
  }
  
  // HashData is a free data retrieval call binding the contract method 0x9512182e.
  //
  // Solidity: function hashData(uint256 ) view returns(string)
  func (_Hash *HashSession) HashData(arg0 *big.Int) (string, error) {
  	return _Hash.Contract.HashData(&_Hash.CallOpts, arg0)
  }
  
  // HashData is a free data retrieval call binding the contract method 0x9512182e.
  //
  // Solidity: function hashData(uint256 ) view returns(string)
  func (_Hash *HashCallerSession) HashData(arg0 *big.Int) (string, error) {
  	return _Hash.Contract.HashData(&_Hash.CallOpts, arg0)
  }
  
  // SetHash is a paid mutator transaction binding the contract method 0x2fbaba26.
  //
  // Solidity: function setHash(uint256 _id, string _hash) returns()
  func (_Hash *HashTransactor) SetHash(opts *bind.TransactOpts, _id *big.Int, _hash string) (*types.Transaction, error) {
  	return _Hash.contract.Transact(opts, "setHash", _id, _hash)
  }
  
  // SetHash is a paid mutator transaction binding the contract method 0x2fbaba26.
  //
  // Solidity: function setHash(uint256 _id, string _hash) returns()
  func (_Hash *HashSession) SetHash(_id *big.Int, _hash string) (*types.Transaction, error) {
  	return _Hash.Contract.SetHash(&_Hash.TransactOpts, _id, _hash)
  }
  
  // SetHash is a paid mutator transaction binding the contract method 0x2fbaba26.
  //
  // Solidity: function setHash(uint256 _id, string _hash) returns()
  func (_Hash *HashTransactorSession) SetHash(_id *big.Int, _hash string) (*types.Transaction, error) {
  	return _Hash.Contract.SetHash(&_Hash.TransactOpts, _id, _hash)
  }
  
  ```

  

+ 利用生成的代码，与智能合约建立连接

  ```go
  //connecter.go 
  package contract
  
  import (
  	"context"
  	"crypto/ecdsa"
  	"fmt"
  	"github.com/ethereum/go-ethereum/accounts/abi/bind"
  	"github.com/ethereum/go-ethereum/common"
  	"github.com/ethereum/go-ethereum/crypto"
  	"github.com/ethereum/go-ethereum/ethclient"
  	"log"
  	"math/big"
  	"strings"
  )
  
  var (
  	//HashAddr Hash合约的地址
  	HashAddr = common.HexToAddress("0xFdBAC859a23D3B61fdC21C4eb0C3005a41b8D247")
  	//Hash Hash合约的地址
  	HashHash = HashAddr.Hash()
  )
  
  type Connecter struct {
  	ctx  context.Context
  	conn *ethclient.Client
  	hash *Hash
  }
  
  // NewConnecter 生成一个Hash连接者
  func NewConnecter() *Connecter {
  	// Dial这里支持传入 ws、http、ipc的多种链接
  	// 如果是经常需要调用最好还是使用 ws 方式保持通讯状态
  	conn, err := ethclient.Dial("ws://127.0.0.1:8545")
  	if err != nil {
  		panic(err)
  	}
  	hash, err := NewHash(HashAddr, conn)
  	if err != nil {
  		panic(err)
  	}
  	return &Connecter{
  		ctx:  context.Background(),
  		conn: conn,
  		hash: hash,
  	}
  }
  
  // NewConncterWithDeploy 部署合约，并创建一个connecter
  func NewConnecterWithDeploy(ownerAuth *bind.TransactOpts) *Connecter {
  	conn, err := ethclient.Dial("ws://127.0.0.1:8545")
  	if err != nil {
  		panic(err)
  	}
  	contractAddr, tx, hash, err := DeployHash(ownerAuth, conn)
  	fmt.Println("部署在区块链上的合约地址：", contractAddr)
  	if err != nil {
  		panic(err)
  	}
  	ctx := context.Background()
  	HashAddr, err = bind.WaitDeployed(ctx, conn, tx)
  	if err != nil {
  		panic(err)
  	}
  	HashHash = HashAddr.Hash()
  	return &Connecter{
  		ctx:  ctx,
  		conn: conn,
  		hash: hash,
  	}
  }
  
  // BlockNumber 当前区块高度
  func (c *Connecter) BlockNumber() *big.Int {
  	blockNumber, err := c.conn.BlockByNumber(c.ctx, nil)
  	if err != nil {
  		log.Fatal("Get block number error:", err)
  		return big.NewInt(0)
  	}
  	return blockNumber.Number()
  }
  
  // ChainID 获取区块链的id
  func (c *Connecter) ChainID() (id *big.Int, err error) {
  	id, err = c.conn.ChainID(c.ctx)
  	if err != nil {
  		log.Fatal("get chainID failed,err:", err)
  		return
  	}
  	return
  }
  
  // Privatekey 获取私钥和公钥
  func (c *Connecter) PrivateKey(privatekey string) (privateKey *ecdsa.PrivateKey, publicKeyECDSA *ecdsa.PublicKey, err error) {
  	//私钥注意去掉ox
  	//privateKey, err := crypto.HexToECDSA("ac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80")
  	privateKey, err = crypto.HexToECDSA(privatekey)
  	if err != nil {
  		log.Fatal("generate privateKey failed,err:", err)
  		return
  	}
  	publicKey := privateKey.Public()
  	publicKeyECDSA, ok := publicKey.(*ecdsa.PublicKey)
  	if !ok {
  		fmt.Errorf("cannot assert type: publicKey is not of type *ecdsa.PublicKey")
  		return
  	}
  	//通过公钥计算账户地址
  	//fromAddress := crypto.PubkeyToAddress(*publicKeyECDSA)
  	return
  }
  
  // AuthAccount 解锁账户
  // 正式使用时，此处应该限制GasPrice和GasLimit
  func AuthAccount(key, passphrse string) *bind.TransactOpts {
  	auth, err := bind.NewTransactor(strings.NewReader(key), passphrse)
  	if err != nil {
  		log.Fatal("Auth account error:", err)
  		return nil
  	}
  	log.Printf("Gas price is:%v,Gas limit is:%v", auth.GasPrice, auth.GasLimit)
  	return auth
  }
  
  // GetHash 获取存储在区块链上的哈希值
  func (c *Connecter) GetHash(fromAuth *bind.CallOpts, id *big.Int) string {
  	result, err := c.hash.GetHash(fromAuth, id)
  	if err != nil {
  		log.Fatal("Get Hash failed,err:", err)
  		return ""
  	}
  	return result
  }
  
  func (c *Connecter) SetHash(fromAuth *bind.TransactOpts, id *big.Int, hashData string) bool {
  	tx, err := c.hash.SetHash(fromAuth, id, hashData)
  	if err != nil {
  		log.Fatal("set hash failed,err:", err)
  		return false
  	}
  	//等待执行
  	receipt, err := bind.WaitMined(c.ctx, c.conn, tx)
  	if err != nil {
  		log.Fatal("wait setHash Mined failed,err:", err)
  		return false
  	}
  	return receipt.Status == 1
  }
  ```

  

+ main函数，运行

  ```go
  package main
  
  import (
  	"encrypt/contract"
  	"fmt"
  	"github.com/ethereum/go-ethereum/accounts/abi/bind"
  	"github.com/ethereum/go-ethereum/common"
  	"github.com/ethereum/go-ethereum/crypto"
  	"math/big"
  )
  
  func main() {
  	h := contract.NewConnecter()
  
  	//从区块链上获取值
  	//fromAuth := bind.CallOpts{
  	//	From: common.HexToAddress("0xDDdC15064127C6B2A9a0ca5a4EA757dfee6CbD4d"),
  	//}
  	//id := big.NewInt(1001)
  	//res := h.GetHash(&fromAuth, id)
  	//fmt.Println(res)
  
  	//将值保存在区块链上
  	chainID, _ := h.ChainID()
  	privateKey, _ := crypto.HexToECDSA("bfbc22e0a52a085ca8916eea99527e551f56a3278027e7a34e1efa1ff57a52d5")
      
  	auth, _ := bind.NewKeyedTransactorWithChainID(privateKey, chainID)
      
  	fromAuth := bind.TransactOpts{
  		From:   common.HexToAddress("0xDDdC15064127C6B2A9a0ca5a4EA757dfee6CbD4d"),
  		Signer: auth.Signer,
  	}
  	id := big.NewInt(1002)
  	hashData := "77df263f49123356d28a4a8715d25bf5b980beeeb503cab46ea61ac9f3320edA"
  	t := h.SetHash(&fromAuth, id, hashData)
  	fmt.Println(t)
  }
  
  ```

  



















# 日志收集项目架构设计之kafka介绍

## 日志收集项目架构

### 项目背景

每个业务系统都有日志，当系统出现问题时， 需要通过日志信息来定位和解决问题。当系统机器比较少时，登陆到服务器上查看即可满足。当系统的机器规模较大，登陆到机器上查看几乎不现实（分布式的系统，一个系统部署在十几台机器上）



### 解决方案

把机器上的日志实时收集，统一存储到中心系统。再对这些日志建立索引，通过搜索即可快速找到对应的日志记录。通过提供一个界面友好的web页面实现日志展示和检索。



### 面临问题

实时日志量非常大，每天处理几十亿条。日志准实时收集，延迟控制在分钟级别。系统架构设计能支持水平扩展。



### 业界方案

#### ELK

![image-20230113213703472](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230113213703472.png)





#### ELK方案的问题

+ 运维成本高，每增加一个日志收集项，都需要手动修改配置。
+ 监控缺失，无法准确获取logstash的状态。
+ 无法做到定制化开发和维护。





### 日志收集系统架构设计

#### 架构设计

![image-20230113214204271](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230113214204271.png)

**logAgent的工作流程：**

​	1，读日志--`tailf`第三方库`github.com/hpcloud/tail`

​	2，往kafka中写日志



#### 组件介绍

LogAgent：日志收集客户端，用来收集服务器上的日志。Kafka：高吞吐量的分布式队列（Linkin开发，apache顶级开源项目）。ElasticSearch：开源的搜索引擎，提供基于HTTP RESTful 的web接口。Kibana：开源的ES数据分析和可视化工具。Hadoop：分布式计算框架，能够对大量的数据进行分布式处理的平台。Storm：一个免费的开源的分布式实时计算系统。



#### 将学到的技能

+ 服务端agent开发
+ 后端服务组件开发
+ kafka和zookeeper的使用
+ ES和kibana的使用
+ etcd的使用



### 消息队列的通信模型



#### 点对点模式（queue）

消息生成者生成消息，发送到queue中，然后，消息消费者从queue中取出并且消费消息。一条消息被消费以后，queue中就没有了，不存在 重复消费。



#### 发布/订阅（topic）

消息生产者（发布）将消息发布到topic中，同时有多个消息消费者（订阅）消费该消息。和点对点方式不同，发布到topic的消息会被所有的订阅者消费（类似于关注微信公众号的人都能收到推送的文章）。

注意：发布订阅模式下，当发布者消息量很大时，显然单个订阅者的处理能力是不足的。实际上现实场景中是多个订阅者节点组成一个订阅组负载均衡消费topic消息即分组订阅，这样订阅者很容易实现消费能力线性扩展。可以看成一个tipic下有多个queue，每个queue是点对点的方式，queue之间是发布订阅方式。



## go操作kafka

### Kafka

Apache Kafka由著名职业社交公司LinkedIn开发，最初是被设计用来解决LinkedIn公司内部海量日志传输等问题。Kafka使用Scala语言编写，于2011年开源并进入Apache孵化器，2012年10月正式毕业，现在为Apache顶级项目。

#### 介绍

Kafka是一个分布式数据流平台，可以运行在单台服务器上，也可以在多台服务器上部署形成集群。它提供了发布和订阅功能，使用者可以发送数据到Kafka中，也可以从kafka中读取数据（以便进行后续的处理）。Kafka具有高吞吐、低延迟、高容错等特点。

![image-20230114132705181](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230114132705181.png)



#### 架构图

![image-20230114132729863](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230114132729863.png)

+ Producer：producer为生产者，消息的生产者，是消息的入口
+ Kafka cluster：kafka集群，一台或多台服务器组成
  + Broker：Broker是指部署了kafka实例的服务器节点。每个服务器上有一个或多个kafka的实例，我们姑且认为每个broker对应一台服务器。每个kafka集群内的broker都有一个不重复的编号，如图中的broker-0，broker-1等...
  + Topic：消息的主题，可以理解为消息分类，kafka的数据就保存在topic。在每个broker上都可以创建多个topic。实际应用中通常是一个业务建一个topic。
  + Partition：Topic的分区，每个topic可以有多个分区，分区的作用是做负载，提高kafka的吞吐量。同一个topic在不同的分区的数据是不重复的，partition的表现形式就是一个一个的文件夹。
  + Replication：每个分区都有多个副本，副本的作用就是作为备胎。当主分区（Leader）故障后，会选择一个备胎（Follower）上位，成为新的Leader。在kafka中默认副本的最大数量为10个，且副本的数量不能大于Broker数量，follower和leader绝对是在不同的机器，同一机器对同一个分区也能存放一个副本（包括自己）
+ Consumer：消费者，即消息的消费方，是消息的出口。
+ Consumer Group：我们可以将多个消费者组成一个消费者组，在kafka的设计中，==同一个分区的数据只能被消费者组中的某一个消费者消费==。==同一个消费者组的消费者可以消费同一个topic的不同分区的数据==，这也是提高kafka的吞吐量。



#### 工作流程

在上面架构图中，producer会将生产的数据写入leader中，而不会直接将数据写入follower中。



![image-20230114142900534](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230114142900534.png)

#### 选择partition的原则

在kafka中，如果某个topic有多个partition，producer如何知道将数据发送到哪个partition呢？

**kafka原则：**

+ partition：在写入的时候可以指定需要写入的partition，如果有指定，则写入对应的partition
+ 如果没有指定partition，但是设置了数据的key，则会根据key的值hash出一个partition
+ 如果既没有指定partition，也没有设置key，则会采用轮询方式，即每次取一小段时间的数据写入到某个partition，下一小段时间的数据写入到下一个partition中。



#### ACK应答机制

producer在向kafka写入消息的时候，可以设置参数来确定是否确认kafka接收到数据，这个参数可以设置的值为0，1，all。

+ 0代表producer往集群发送数据不需要等到集群的返回，不确保消息发送成功。安全性最低但是效率最高。
+ 1代表producer往集群发送数据只要leader应答就可以发送下一条数据，只确保leader发送成功。
+ all代表producer往集群中发送数据需要所有的follower都完成从leader的同步才会发送下一条，确保leader发送成功和所有的副本都完成备份。安全性最高，但是效率最低。

==注意：==如果往不存在的topic中写入数据，kafka会自动创建一个topic，partition和replication的数量默认配置都是1



#### Topic和数据日志

`topic`是同一类别的消息记录（record）的集合。在kafka中，一个主题通常有多个订阅者。对于每个主题，kafka集群维护了一个分区数据日志文件结构如下：

![image-20230114155143241](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230114155143241.png)



每个partition都是一个有序并且不可改变的消息记录集合。==当新的数据写入时，就被追加到partition的末尾。在每个partition中，每条消息都会被分配到一个顺序的唯一标识，这个标识被称为offset，即偏移量==。注意：kafka只保证在同一个partition内部消息是有序的，在不同的partition之间，并不能保证消息有序。



kafka可以配置一个保留期限，用来标识日志会在kafka集群内保留多长时间。kafka集群会保留在保留期限内所有被发布的消息，不管这些消息是否被取消过。比如保留期限是两天，那么数据被发布到kafka集群的两天内，所有的这些数据都是可以被消费的。当超过两天，这些数据就会被清空，以便为后续的数据腾出空间。由于kafka会将数据进行持久化存储（即写入到硬盘上），所以保留的数据大小可以设置为一个比较大的值。





#### partition结构

partition在服务器上的表现形式就是一个一个文件夹，每个partition的文件下面会有多组segment文件，每组segment文件又包含`.index`文件、`.log`文件、`.timeindex`文件三个文件，其中`.log`文件就是实际存储message的地方，而`.index`和`.timeindex`文件为索引文件，用于检索消息。



#### 消费数据

多个消费者实例可以组成一个消费者组，并用一个标签来标识这个消费者组。一个消费者组中的不同消费者实例可以运行在不同的进程上，甚至在不同的服务器上。

如果所有的消费者实例都在同一个消费者组中，那么消息记录会被很好的均衡的发送到每个消费者实例。

如果所有的消费者实例都在不同的消费者组，那么每一条消息记录会被广播到每一个消费者实例。



![image-20230114160928072](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230114160928072.png)

```go
在上图结构中，可以看出：
一个两个节点的kafka集群上拥有一个四个分区（P0-P3）的topic。有两个消费者组都在消费这个topic中的数据，消费者组A有两个消费者实例，消费者组B中有四个消费者实例。从图中我们可以看出，在同一个消费者组中，每个消费者实例可以消费多个分区，但是每个分区最多只能被消费者组中的一个实例消费。也就是说：如果有一个4分区的主题，那么消费者组中最多只能有4个消费者实例去消费。
```





#### 使用场景

+ 消息队列（MQ）
+ 追踪网站活动
+ Metrics
+ 日志聚合





### kafka启动

```go
//首先获取kafka压缩包，解压

//进入到kafka_2.12-3.3.1目录下
//进入config目录下，设置zookeeper.properties 和 server.properties 配置文件中的log存储路径

//先启动zookeeper
D:\software\kafka\kafka_2.12-3.3.1>bin\windows\zookeeper-server-start.bat config\zookeeper.properties

//再启动kafka
D:\software\kafka\kafka_2.12-3.3.1>bin\windows\kafka-server-start.bat config\server.properties

//可以启动kafka消费者终端，来从kafka中读取数据
D:\software\kafka\kafka_2.12-3.3.1>bin\windows\kafka-console-consumer.bat --bootstrap-server=127.0.0.1:9092 --topic=web_log --from-beginning
```



### go操作kafka

go语言中连接kafka使用第三方库：[github.com/Shopify/sarama](https://github.com/Shopify/sarama)。

#### 下载安装

```go
go get github.com/shopify/sarama
```



#### 注意事项

`sarama`在v1.20之后的版本中加入了`zstd`压缩算法，需要用到cgo，在windows平台编译时会提示类似如下错误：

```bash
# github.com/DataDog/zstd
exec: "gcc":executable file not found in %PATH%
```

==所以在Windows平台请使用v1.19版本的sarama。==



#### 连接kafka发送消息

```go
package main

import(
	"fmt"
    "github.com/shopify/sarama"
)
//基于sarama第三方库开发的kafka client

func main(){
    config := sarama.NewConfig()
    config.Producer.RequiredAcks = sarama.WaitForAll  //发送完数据需要leader和follower都确认
    config.Producer.Partitioner = sarama.NewRandomPartitioner  //新选出一个partition
    config.Producer.Return.Success = true  //成功交付的消息在success channel 返回
    
    //构造一个消息
    msg := &sarama.ProducerMessage{}
    msg.Topic = "web_log"
    msg.Value = sarama.StringEncoder("this is a test log")
    
    //连接kafka
    client,err := sarama.NewSyncProducer([]string{"192.168.1.7.9092"},config)
    if err!=nil{
        fmt.Printf("producer closed,err:%v\n",err)
        return
    }
    defer client.Close()
    
    //发送消息
    pid, offset, err:= client.SendMessage(msg)
    if err!=nil{
        fmt.Println("send msg failed,err:",err)
        return
    }
    fmt.Printf("pid:%v,offset:%v\n",pid,offset)
}
```



#### 连接kafka消费消息

```go
package main

import(
	"fmt"
    "github.com/shopify/sarama"
)

//kafka consumer

func main(){
    consumer,err := sarama.NewConsumer([]string{"127.0.0.1:9092"},nil)
    if err!=nil{
        fmt.Printf("fail to start consumer,err:%v\n",err)
        return
    }
    patitionList,err := consumer.Partitions("web_log")  //根据topic取到所有的分区
    if err!=nil{
        fmt.Printf("fail to get list of partition:err:%v\n",err)
        return
    }
    fmt.Println(partitionList)
    
    for partition := range partitionList{ //遍历所有的分区
        //针对每个分区创建一个对应的分区消费者
        pc,err := consumer.ConsumePartition("web_log",int32(partition),sarama.OffsetNewest)
        if err!=nil{
            fmt.Printf("failed to start consumer for partition %d,err:%v\n",partition,err)
			return
        }
        defer pc.AsyncClose()
        
        //异步从每个分区中消费消息
        go func (sarama.PartionConsumer){
            for msg := range pc.Message(){
                fmt.Printf("Partition:%d Offset:%d Key:%v 							       			   								Value:%v\n",msg.Partition,msg.Offset,msg.Key,msg.Value)
            }
        } (pc)
    }
}
```











### zookeeper

**zookeeper是一个分布式的，开源的分布式应用程序协调服务，**是Google的Chubby一个开源的实现，==**它是集群的管理者，监视着集群中的各个节点的状态，根据节点提交的反馈进行下一步合理操作**==。最终，将简单易用的接口和性能高效、功能稳定的系统提供给用户。











### logAgent开发

```go
package main

import (
	"fmt"
	"gopkg.in/ini.v1"
	"logAgent/conf"
	"logAgent/kafka"
	"logAgent/tailfLog"
	"time"
)

var (
	cfg = new(conf.AppConf)
)

func run() {
	//1,读取日志
	for {
		select {
		case line := <-tailfLog.ReadChan():
			//2，发送到kafka中
			kafka.SendToKafka(cfg.Topic, line.Text)
		default:
			//未读取到日志，cpu休息一秒
			time.Sleep(time.Second)
		}
	}

}

//logAgent 入口程序
func main() {
	//0，加载配置文件
	//cfg, err := ini.Load("./conf/config.ini")
	//if err != nil {
	//	fmt.Printf("ini load failed,err:%v\n", err)
	//	return
	//}
	////kafka连接地址
	//addr := cfg.Section("kafka").Key("address").String()
	////kafka topic
	//topic := cfg.Section("kafka").Key("topic").String()
	////日志文件路径
	//fileName := cfg.Section("taillog").Key("fileName").String()

	err := ini.MapTo(cfg, "./conf/config.ini")
	if err != nil {
		fmt.Println("load ini failed,err:", err)
		return
	}

	//1,初始化kafka连接
	err = kafka.Init([]string{cfg.Address})
	if err != nil {
		fmt.Printf("init kafka failed,err:%v\n", err)
		return
	}
	fmt.Println("init kafka success！")

	//2，打开日志文件准备收集日志
	err = tailfLog.Init(cfg.FileName)
	if err != nil {
		fmt.Printf("Init taillog failed,err:%v\n", err)
		return
	}
	fmt.Println("init taillog success!")

	//3,读取日志发送到kafka
	run()
}
```



```go
package conf

type AppConf struct {
	KafkaConf   `ini:"kafka"`
	TaillogConf `ini:"taillog"`
}

type KafkaConf struct {
	Address string `ini:"address"`
	Topic   string `ini:"topic"`
}

type TaillogConf struct {
	FileName string `ini:"fileName"`
}
```

```go
//config ini 配置文件
[kafka]
# 配置文件中 # 表示注释
address=127.0.0.1:9092
topic=web_log

[taillog]
fileName=./my.log
```



```go
package kafka

import (
	"fmt"
	"github.com/shopify/sarama"
)

//专门往kafka中写日志

//定义全局连接
var (
	client sarama.SyncProducer //声明一个全局的连接kafka的生产者client
)

func Init(addrs []string) (err error) {
	config := sarama.NewConfig()
	config.Producer.RequiredAcks = sarama.WaitForAll          //发送完数据需要leader和follower都确认
	config.Producer.Partitioner = sarama.NewRandomPartitioner //新选出一个partition
	config.Producer.Return.Successes = true                   //成功交付的消息将在success channel返回

	//连接kafka
	client, err = sarama.NewSyncProducer(addrs, config)
	if err != nil {
		fmt.Printf("producer closed,err:%v\n", err)
		return
	}
	return
}

func SendToKafka(topic, data string) {
	//构造一个信息
	msg := &sarama.ProducerMessage{}
	msg.Topic = topic
	msg.Value = sarama.StringEncoder(data)
	//发送到kafka
	pid, offset, err := client.SendMessage(msg)
	if err != nil {
		fmt.Println("send msg failed,err:", err)
		return
	}
	fmt.Printf("pid:%v,offset:%v\n", pid, offset)
}
```



```go
package tailfLog

import (
	"fmt"
	"github.com/hpcloud/tail"
)

//专门读取日志

var (
	tailObj *tail.Tail
)

func Init(fileName string) (err error) {
	config := tail.Config{
		ReOpen:    true,
		Follow:    true,
		Location:  &tail.SeekInfo{Offset: 0, Whence: 2},
		MustExist: false,
		Poll:      true,
	}
	// tail.TailFile()函数开启goroutine去读取文件，通过channel格式的tailObj.lines传递内容。
	tailObj, err = tail.TailFile(fileName, config)
	if err != nil {
		fmt.Println("tail file failed,err:", err)
		return
	}
	return
}

func ReadChan() <-chan *tail.Line {
	return tailObj.Lines
}

```





## go操作etcd

### etcd介绍

etcd是使用GO语言开发的一种开源、高可用的分布式key-value存储系统，可以用于配置共享和服务的注册和发现。

类似项目有zookeeper和consul。



**etcd的特点：**

+ 完全复制：集群中的每个节点都可以使用完整的存档
+ 高可用性：etcd可用于避免硬件的单点故障或网络问题
+ 一致性：每次读取都会返回跨多主机的最新写入
+ 简单：包括一个定义良好、面向用户的API（gRPC）
+ 快速：每秒10000次写入的基准速度
+ 可靠：使用Raft算法实现了强一致性，高可用的服务存储目录



### etcd应用场景

#### 服务注册发现

服务发现要解决的也是分布式系统中最常见的问题之一，即在同一个分布式集群中的进程或服务，要如何才能找到对方并建立连接。本质上来说，服务发现就是想要了解集群中是否有进程在监听udp或tcp端口，并且通过名字就可以查找和连接。

![image-20230127142721378](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230127142721378.png)

#### 配置中心

将一些配置信息放到etcd上进行集中存储。

这类场景使用方式通常为：应用在启动的时候主动从etcd获取一次配置信息，同时，在etcd节点上注册一个watcher并等待，以后每次配置有更新的时候，etcd都会实时通知订阅者，以此达到获取最新的配置信息的目的。



#### 分布式锁

因为etcd使用raft算法保证了数据的强一致性，某次操作存储到集群中的值必然是全局一致的，所以很容易是实现分布式锁。锁服务有两种使用方式：一是保持独占、二是控制时序。

+ **保持独占即所有获取锁的用户最终只有一个可以得到**。etcd为此提供了一套实现分布式锁原子操作CAS（compareAndSwap）的API。通过设置`preExist`值，可以保证在多个节点同时去创建某个目录时，只有一个成功。而创建成功的用户就可以认为是获得了锁。
+ **控制时序**。即所有想要获取锁的用户都会被安排执行，但是获得锁的顺序也是全局唯一的，同时决定了执行顺序。etcd为此也提供了一套API（自动创建有序键），对一个目录建值时指定为`Post`动作，这样etcd会自动在目录下生成一个当前最大的值为键，存储这个新的值（客户端号）。同时还可以使用API按顺序列出所有当前目录下的键值。此时这些键的值就是客户端的时序，而这些键中的存储的值可以是代表客户端的编号。



![image-20230127144627199](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230127144627199.png)



### 为什么使用etcd而不用zookeeper？

etcd实现的这些功能，zookeeper都能实现。那为什么要用etcd而不是使用zookeeper呢？

相比较之下，zookeeper有如下缺点：

+ 复杂：zookeeper的部署维护复杂，管理员需要掌握一系列知识和技能，而paxos强一致性算法也是很复杂。另外zookeeper的使用也比较复杂，需要安装客户端，官方只提供了java和 c两种语言接口。
+ java编写：java本身就偏向于重型应用，会引入大量的依赖。而运维人员则普遍希望保持强一致、高可用的机器集群尽可能简单，维护起来也不容易出错。
+ 发展缓慢：Apache基金会项目特有的“Apache Way”在开源界饱受争议，其中一大原因就是由于基金会庞大的结构以及松散的管理导致项目发展缓慢。

etcd作为后起之秀，优点也很明显：

+ 简单：使用go语言编写部署简单，使用http作为接口使用简单，使用raft算法保证了强一致性让用户易于理解。
+ 数据持久化：etcd默认数据一更新就进行持久化处理
+  安全：etcd支持ssl客户端安全认证。

最后，etcd作为一个年轻的项目，处于迭代和开发中，这既是缺点也是优点。优点是它的未来具有无限的可能性，缺点就是无法得到大项目长时间使用的检验。然而目前，Core OS、Kubernetes和CloudFoundry等知名项目均在生产环境中使用了etcd，所以总的来说，etcd值得尝试。



### etcd架构

![image-20230127151101823](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230127151101823.png)

从etcd的架构图中可以看出，etcd主要分为四个部分。

+ HTTP Server：用于处理用户发送的API请求以及其他的etcd节点的同步与心跳信息请求。
+ Store：用于处理etcd支持的各类功能的事务，包括数据索引、节点状态变更、监控与反馈、事件处理与执行等等，是etcd对用户提供的大多数API功能的具体实现。
+ Raft：Raft强一致性算法的具体实现，是etcd的核心
+ WAL：write ahead Log（预写式日志），是etcd的数据存储方式。除了在内存中存有所有数据的状态以及节点的索引以外，etcd就通过WAL进行持久化存储。WAL中，所有的数据提交前都会事先记录日志。Snapshot是为了防止数据过多而进行的状态快照，Entry表示存储的具体日志内容。



### etcd集群

==etcd作为一个高可用的键值存储系统，天生就是为了集群化而设计的==。由于raft算法在做决策时需要多数节点的投票，所以etcd一般部署集群推荐奇数个节点，推荐的数量为3、5或者7个节点构成一个集群。



#### 搭建3个节点集群示例

在每个etcd节点指定的集群成员，为了区分不同的集群最好同时配置一个独一无二的token。

下面是提前定义好的集群信息，其中`n1,n2,n3`表示3个不同的etcd节点。

```go
TOKEN=token-01
CLUSTER_STATE=new
CLUSTER=n1=http://10.240.0.17:2380,n2=http://10.240.0.18:2380,n3=http://10.240.0.19:2380
```

在`n1`这台机器上执行以下命令来启动etcd：

```bash
etcd --data-dir=data.etcd --name n1 \
	--initial-advertise-peer-urls http://10.240.0.17:2380 --listen-peer-urls http://10.240.0.17:2380 \
	--advertise-client-urls http://10.240.0.17:2379 --listen-client-urls http://10.240.0.17:2379 \
	--initial-cluster ${CLUSTER} \
	--initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}
```

在`n2`这台机器上执行以下命令启动etcd：

```bash
etcd --data-dir=data.etcd --name n2 \
	--initial-advertise-peer-urls http://10.240.0.18:2380 --listen-peer-urls http://10.240.0.18:2380 \
	--advertise-client-urls http://10.240.0.18:2379 --listen-client-urls http://10.240.0.18:2379 \
	--initial-cluster ${CLUSTER} \
	--initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}
```

在`n3`这台机器上执行以下命令启动etcd：

```bash
etcd --data-dir=data.etcd --name n3 \
	--initial-advertise-peer-urls http://10.240.0.19:2380 --listen-peer-urls http://10.240.0.19:2380 \
	--advertise-client-urls http://10.240.0.19:2379 --listen-client-urls http://10.240.0.19:2379 \
	--initial-cluster ${CLUSTER} \
	--initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}
```

etcd 官网提供了一个可以公网访问的 etcd 存储地址。你可以通过如下命令得到 etcd 服务的目录，并把它作为`-discovery`参数使用。

```bash
curl https://discovery.etcd.io/new?size=3
https://discovery.etcd.io/a81b5818e67a6ea83e9d4daea5ecbc92

# grab this token
TOKEN=token-01
CLUSTER_STATE=new
DISCOVERY=https://discovery.etcd.io/a81b5818e67a6ea83e9d4daea5ecbc92


etcd --data-dir=data.etcd --name n1 \
	--initial-advertise-peer-urls http://10.240.0.17:2380 --listen-peer-urls http://10.240.0.17:2380 \
	--advertise-client-urls http://10.240.0.17:2379 --listen-client-urls http://10.240.0.17:2379 \
	--discovery ${DISCOVERY} \
	--initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}


etcd --data-dir=data.etcd --name n2 \
	--initial-advertise-peer-urls http://10.240.0.18:2380 --listen-peer-urls http://10.240.0.18:2380 \
	--advertise-client-urls http://10.240.0.18:2379 --listen-client-urls http://10.240.0.18:2379 \
	--discovery ${DISCOVERY} \
	--initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}


etcd --data-dir=data.etcd --name n3 \
	--initial-advertise-peer-urls http://10.240.0.19:2380 --listen-peer-urls http://10.240.0.19:2380 \
	--advertise-client-urls http://10.240.0.19:2379 --listen-client-urls http:/10.240.0.19:2379 \
	--discovery ${DISCOVERY} \
	--initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}
```

到此etcd集群就搭建起来了，可以使用`etcdctl`来连接etcd。

```bash
export ETCDCTL_API=3
HOST_1=10.240.0.17
HOST_2=10.240.0.18
HOST_3=10.240.0.19
ENDPOINTS=$HOST_1:2379,$HOST_2:2379,$HOST_3:2379

etcdctl --endpoints=$ENDPOINTS member list
```



### etcd安装

从`https://github.com/etcd-io/etcd/releases`下载etcd的windows压缩包，解压之后，双击etcd.exe 就是启动etcd。其他平台解压之后在bin目录下找etcd可执行文件，默认会在2379端口监听客户端通信，在2380端口监听节点间通信。



```go
//存值
D:\software\etcd\etcd-v3.5.7-windows-amd64>etcdctl.exe --endpoints=127.0.0.1:2379 put zhangsan "sb"
OK

//取值
D:\software\etcd\etcd-v3.5.7-windows-amd64>etcdctl.exe --endpoints=127.0.0.1:2379 get zhangsan
zhangsan
sb

//删除值
D:\software\etcd\etcd-v3.5.7-windows-amd64>etcdctl.exe --endpoints=127.0.0.1:2379 del zhangsan
1
```



### go语言操作etcd

#### put和get操作

```go
package main

import (
	"context"
	"fmt"
	"go.etcd.io/etcd/clientv3"
	"time"
)

func main() {
	cli, err := clientv3.New(clientv3.Config{
		Endpoints:   []string{"127.0.0.1:2379"},
		DialTimeout: 5 * time.Second, //超时时间
	})
	//处理错误
	if err != nil {
		fmt.Printf("connect to etcd failed,err:%v\n", err)
		return
	}
	fmt.Println("connect to etcd success!") //连接成功
	defer cli.Close()
	//put
	ctx, cancel := context.WithTimeout(context.Background(), time.Second)
	_, err = cli.Put(ctx, "ZhangYu", "beautiful")
	cancel()
	if err != nil {
		fmt.Printf("put to etcd failed,err:%v\n", err)
		return
	}
	fmt.Println("put key success!")
	//get
	ctx, cancel = context.WithTimeout(context.Background(), time.Second)
	resp, err := cli.Get(ctx, "ZhangYu")
	cancel()
	if err != nil {
		fmt.Printf("get from etcd failed,err:%v\n", err)
		return
	}
	for _, ev := range resp.Kvs {
		fmt.Printf("%s:%s\n", ev.Key, ev.Value)
	}

	//del
	ctx, cancel = context.WithTimeout(context.Background(), time.Second)
	_, err = cli.Delete(ctx, "ZhangYu")
	cancel()
	if err != nil {
		fmt.Printf("delete from etcd failed,err:%v\n", err)
		return
	}
	fmt.Println("delete key success!")
}

```



#### watch操作

```go
package main

import (
	"context"
	"fmt"
	"go.etcd.io/etcd/clientv3"
	"time"
)

func main() {
	//连接etcd
	cli, err := clientv3.New(clientv3.Config{
		Endpoints:   []string{"127.0.0.1:2379"},
		DialTimeout: 5 * time.Second, //超时时间
	})
	//处理错误
	if err != nil {
		fmt.Printf("connect to etcd failed,err:%v\n", err)
		return
	}
	fmt.Println("connect to etcd success!") //连接成功
	defer cli.Close()

	//watch
	//派一个哨兵一直监视 ZhangYu 这个key的变化（包括：新增、修改、删除）
	ch := cli.Watch(context.Background(), "ZhangYu")
	//从通道中尝试取值（监视的信息）
	for wresp := range ch {
		for _, evt := range wresp.Events {
			fmt.Printf("Type:%v,Key:%v,Value:%v\n", evt.Type, string(evt.Kv.Key), string(evt.Kv.Value))
		}
	}
}
```



#### lease租约

```go
package main

import (
	"context"
	"fmt"
	"go.etcd.io/etcd/clientv3"
	"log"
	"time"
)

func main() {
	cli, err := clientv3.New(clientv3.Config{
		Endpoints:   []string{"127.0.0.1:2379"},
		DialTimeout: time.Second * 5,
	})
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("connect to etcd success!")
	defer cli.Close()

	//创建一个5秒的租约 
	resp, err := cli.Grant(context.TODO(), 10)
	if err != nil {
		log.Fatal(err)
	}
	//5秒之后，XiaoYu 这个key就会被移除
	_, err = cli.Put(context.TODO(), "XiaoYu", "love", clientv3.WithLease(resp.ID))
	if err != nil {
		log.Fatal(err)
	}
}
```



#### keepAlive

```go
package main

import (
	"context"
	"fmt"
	"go.etcd.io/etcd/clientv3"
	"log"
	"time"
)

func main() {
	cli, err := clientv3.New(clientv3.Config{
		Endpoints:   []string{"127.0.0.1:2379"},
		DialTimeout: time.Second * 5,
	})
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("connect to etcd success!")
	defer cli.Close()
	resp, err := cli.Grant(context.TODO(), 5)
	if err != nil {
		log.Fatal(err)
	}
	_, err = cli.Put(context.TODO(), "XiaoYu", "beautiful", clientv3.WithLease(resp.ID))
	if err != nil {
		log.Fatal(err)
	}
	//the key 'XiaoYu' will be kept forever
	ch, kaerr := cli.KeepAlive(context.TODO(), resp.ID)
	if kaerr != nil {
		log.Fatal(kaerr)
	}
	for {
		ka := <-ch
		fmt.Println("ttl:", ka.TTL)
	}
}
```



#### 基于etcd实现分布式锁

`go.etcd.io/etcd/clientv3/concurrency`在etcd之上实现并发操作，如分布式锁、屏障和选举。

导入包：`import "go.etcd.io/etcd/clientv3/concurrency"`

基于etcd实现分布式锁的示例：

```go
package main

import (
	"context"
	"fmt"
	"github.com/coreos/etcd/clientv3"
	"go.etcd.io/etcd/clientv3/concurrency"
	"log"
)

//基于etcd实现分布式锁的示例

func main() {
	//连接etcd
	cli, err := clientv3.New(clientv3.Config{
		Endpoints: []string{"127.0.0.1:2379"},
	})
	if err != nil {
		log.Fatal(err)
	}
	//关闭客户端连接
	defer cli.Close()
	//创建两个单独的会话来演示竞争
	s1, err := concurrency.NewSession(cli) //会话s1
	if err != nil {
		log.Fatal(err)
	}
	defer s1.Close() //关闭会话s1

	//使用Session对象通过concurrency.NewMutex 创建了一个Mutex对象，这个对象是分布式锁的核心逻辑
	m1 := concurrency.NewMutex(s1, "/my-lock/")

	s2, err := concurrency.NewSession(cli) //会话s2
	if err != nil {
		log.Fatal(err)
	}
	defer s2.Close() //关闭会话s2
	//使用Session对象通过concurrency.NewMutex 创建了一个Mutex对象，这个对象是分布式锁的核心逻辑
	m2 := concurrency.NewMutex(s2, "/my-lock/")

	//会话s1获取锁
	if err := m1.Lock(context.TODO()); err != nil {
		log.Fatal(err)
	}
	fmt.Println("acquired lock for s1")
	m2Locked := make(chan struct{})
	go func() {
		defer close(m2Locked)
		//等待直到会话s1释放了/my-lock/的锁，会话s2再去获取锁
		if err := m2.Lock(context.TODO()); err != nil {
			log.Fatal(err)
		}
	}()
	if err := m1.Unlock(context.TODO()); err != nil {
		log.Fatal(err)
	}
	fmt.Println("released lock for s1")

	<-m2Locked
	fmt.Println("acquired lock for s2")
}

//运行结果为：
D:\学习笔记\后端学习资料\日志收集架构之kafka项目\etcdDemo\etcd_lock>go run main.go
acquired lock for s1
released lock for s1
acquired lock for s2
```

[查看文档了解更多](https://godoc.org/go.etcd.io/etcd/clientv3/concurrency)



#### 其他操作

其他操作请查看[etcd/clientv3官方文档](https://github.com/etcd-io/etcd/tree/master/client/v3)。

参考链接：

- https://etcd.io/docs/v3.3.12/demo/
- https://www.infoq.cn/article/etcd-interpretation-application-scenario-implement-principle/







## go操作ES

### Elastic search-全文检索引擎

#### 介绍

Elasticsearch（ES）是一个基于Lucene构建的开源、分布式、RESTful接口的全文搜索引擎。

Elasticsearch还是一个分布式文档数据库，其中每个字段均可被索引，而且每个字段的数据均可被搜索，ES能够横向扩展至数以百计的服务器存储以及处理PB级的数据。可以在极短时间内存储、搜索和分析大量的数据。通常作为具有复杂搜索场景下的核心发动机。



#### Elastic search 能做什么

+ 当你经营一家网上商店，你可以让你的客户搜索你卖的商品。在这种情况下，你可以使用Elasticsearch来存储你的的整个产品目录和库存信息，为客户提供精准搜索，可以为客户推荐相关商品。
+ 当你想收集日志或交易数据的时候，需要分析和挖掘这些数据，寻找趋势，进行统计，总结或发现异常。在这种情况下，你可以使用Logstash或者其他工具来进行收集数据，当数据存储到Elasticsearch中，你可以搜索和汇总这些数据，找到任何你感兴趣的信息。
+ 对于程序员来说，比较有名的案例是github，github的搜索是基于Elasticsearch构建的，在github.com/search页面，可以搜索项目、用户、issue、pull request，还有代码。共有40~50个索引库，分别用于索引网站需要跟踪的各种数据。虽然只索引项目的主分支（master），但这个数据量依然巨大，包括20个亿索引文档，30TB索引文件。



#### Elastic search 基本概念

##### Near Realtime（NRT）几乎实时

Elasticsearch是一个几乎实时的搜索平台，即从索引一个文档到这个文档可被搜索只需要一点点的延迟，这个时间一般为毫秒级。



##### Cluster集群

集群是一个或多个节点（服务器）的集合，这些节点共同保存整个数据，并在所有节点上提供联合索引和搜索功能。一个集群由一个唯一集群ID确定，并指定一个集群名（默认为“elasticsearch”）。该集群名非常重要，因为节点可以通过这个集群名加入集群，一个节点只能是集群的一部分。



确保在不同的环境中不要使用相同的集群名称，否则可能会导致连接错误的集群节点。例如，你可以使用logging-dev、logging-stage、logging-prod分别为开发、阶段产品、生产集群做记录。



##### Node节点

节点是单个服务器实例，它是集群的一部分，可以存储数据，并参与集群的索引和搜索功能。就像一个集群，节点的名称默认为一个随机的通用唯一标识符（UUID），确定在启动时分配给节点。如果不希望采用默认节点名称，可以定义任何节点名称。这个名称对管理很重要，目的是要确定你的网络服务器对应于你的Elasticsearch集群节点。



我们可以通过集群名配置节点以连接特定的集群。默认情况下，每个节点设置加入名为“elasticsearch”的集群。这意味着如果启动多个节点在网络上，假设它们能发现彼此都会自动形成和加入一个名“elasticsearch”的集群。



在单个集群中，你可以拥有尽可能多的节点。此外，如果“elasticsearch”在同一个网络中，没有其他节点正在运行，从单个节点的默认情况下会形成一个新的单节点名为“elasticsearch”的集群。



##### Index索引

索引是具有相似性的文档集合。例如，可以为客户数据提供索引，为产品目录建立另一个索引，以及为订单数据建立另一个索引。索引由名称（必须全部为小写）标识，该名称用于在对其中的文档执行索引、搜索、更新和删除操作时引用索引。在单个集群中，你可以定义尽可能多的索引。



##### Type类型

在索引中，可以定义一个或多个类型。类型时索引的逻辑类别/分区，其语义完全取决于你的。一般来说，类型定义为具有公共字段集的文档。例如，假设你运行一个博客平台，并所有数据存储在一个索引中。在这个索引中，你可以为用户数据定义一种类型，为博客数据定义另一种类型，以及为注释数据定义另一种类型。



##### Document文档

文档是可以被索引的信息的基本单位。例如，你可以为单个客户提供一个文档，单个产品提供另一个文档，以及单个订单提供另一个文档。本文件的表示形式为JSON（javaScript Object Notation）格式，这是一个非常普遍的互联网数据交换格式。



在索引/类型中，你可以存储尽可能多的文档。注意：尽管文档物理驻留在索引中，文档实际上必须索引或分配到索引中的类型。



##### Shard & Replicas 分片与副本

索引可以存储大量的数据，这些数据可能超过单个节点的硬件限制。例如，十亿个文件占用磁盘空间1TB的单指标可能不适合对个单个节点的磁盘或可能太慢服务仅从单个节点的搜索请求。



为了解决这一问题，Elasticsearch提供细分你的指标分成多个块称为分片能力。当你创建一个索引，你可以简单地定义你想要的分片数据。每个分片本身是一个全功能的、独立的“指数”，可以托管在集群中任何节点。



**Shards分片的重要性主要体现在以下两个特征：**

1. 分片允许你水平拆分或缩放内容的大小
2. 分片允许你分配和并行操作的碎片（可能在多个节点上）从而提高性能/吞吐量 这个机制中的碎片是分布式的以及其文件汇总到搜索请求是完全由ElasticSearch管理，对用户来说是透明的。

在同一个集群网络或云环境上，故障是任何时候都会出现的，拥有一个故障转移机制以防分片和节点因为某些原因离线或消失是非常有用的，并且被强烈推荐。为此，Elasticsearch允许你创建一个或多个拷贝，你的索引分片进入所谓的副本或称作复制品的分片，简称Replicas。

**Replicas的重要性主要体现在以下两个特征：**

1. 副本为分片或节点失败提供了高可用性。为此，需要注意的是，一个副本的分片不会分配在同一个节点作为原始的或主分片，副本是从主分片那里复制过来的。
2. 副本允许用户扩展你的搜索量或吞吐量，因为搜索可以在所有副本上并行执行。



##### ES基本概念与关系型数据库的比较

|                     ES概念                     |    关系型数据库    |
| :--------------------------------------------: | :----------------: |
|           Index（索引）支持全文检索            | Database（数据库） |
|                  Type（类型）                  |    Table（表）     |
| Document（文档），不同文档可以有不同的字段集合 |   Row（数据行）    |
|                 Field（字段）                  |  Column（数据列）  |
|                Mapping（映射）                 |   Schema（模式）   |



### ES 安装

打开elasticsearch[Elasticsearch：官方分布式搜索和分析引擎 | Elastic](https://www.elastic.co/cn/elasticsearch/)，下载elsaticsearch和对应的kibana（注意elasticsearch版本号和kibana版本号必须一致，否则容易出现错误）。



解压elasticsearch压缩包，打开bin文件夹，启动`elasticsearch.bat`，如果启动后，无法打开`127.0.0.1:9200`页面，可以查看[ElasticSearch 启动失败无法访问9200_普通网友的博客-CSDN博客_elasticsearch无法访问](https://blog.csdn.net/nana1253431195/article/details/126359042)，进行修改。



elasticsearch启动成功后，`127.0.0.1:9200`页面将显示以下内容。

![image-20230131213250251](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230131213250251.png)





### ES API

以下示例使用`curl`演示。

#### 查看健康状态

```go
curl -X GET 127.0.0.1:9200/_cat/health?v
```

输出：

```go
epoch      timestamp cluster       status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1564726309 06:11:49  elasticsearch yellow          1         1      3   3    0    0        1             0                  -                 75.0%
```



网页输出结果为：

![image-20230201150225020](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230201150225020.png)



#### 查询当前ES集群中所有的indices

```bash
curl -X GET 127.0.0.1:9200/_cat/indices?v
```

输出：

```go
health status index                uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .kibana_task_manager LUo-IxjDQdWeAbR-SYuYvQ   1   0          2            0     45.5kb         45.5kb
green  open   .kibana_1            PLvyZV1bRDWex05xkOrNNg   1   0          4            1     23.9kb         23.9kb
yellow open   user                 o42mIpDeSgSWZ6eARWUfKw   1   1          0            0       283b           283b
```

#### 创建索引

```bash
curl -X PUT 127.0.0.1:9200/www
```

输出：

```go
{"acknowledged":true,"shards_acknowledged":true,"index":"www"}
```

#### 删除索引

```go
curl -X DELETE 127.0.0.1:9200/www
```

输出：

```go
{"acknowledged":true}
```

#### 插入记录

```bash
curl -H "ContentType:application/json" -X POST 127.0.0.1:9200/user/person -d '
{
	"name": "dsb",
	"age": 9000,
	"married": true
}'
```

输出：

```bash
{
    "_index": "user",
    "_type": "person",
    "_id": "MLcwUWwBvEa8j5UrLZj4",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 3,
    "_primary_term": 1
}
```

也可以使用PUT方法，但是需要传入id

```bash
curl -H "ContentType:application/json" -X PUT 127.0.0.1:9200/user/person/4 -d '
{
	"name": "sb",
	"age": 9,
	"married": false
}'
```

#### 检索

Elasticsearch的检索语法比较特别，使用GET方法携带JSON格式的查询条件。

全检索：

```bash
curl -X GET 127.0.0.1:9200/user/person/_search
```

按条件检索：

```bash
curl -H "ContentType:application/json" -X PUT 127.0.0.1:9200/user/person/4 -d '
{
	"query":{
		"match": {"name": "sb"}
	}	
}'
```

ElasticSearch默认一次最多返回10条结果，可以像下面的示例通过size字段来设置返回结果的数目。

```bash
curl -H "ContentType:application/json" -X PUT 127.0.0.1:9200/user/person/4 -d '
{
	"query":{
		"match": {"name": "sb"},
		"size": 2
	}	
}'
```



### go操作 Elsatic search

#### Elastic client

我们使用第三方库https://github.com/olivere/elastic来连接ES并进行操作。

注意下载与你的ES相同版本的client，例如我们这里使用的ES是7.2.1的版本，那么我们下载的client也要与之对应为`github.com/olivere/elastic/v7`。

使用`go.mod`来管理依赖：

```go
require (
    github.com/olivere/elastic/v7 v7.0.4
)
```

示例：

```go
package main

import(
	"context"
    "fmt"
    "github.com/olivere/elastic/v7"
)
//Elasticsearch demo

type person struct{
    Name string `json:"name"`
    Age int `json:"age"`
    Married bool `json:"married"`
}

func main(){
    client,err:=elastic.NewClient(elastic.SetURL("http://192.168.1.7.9200"))
    if err!=nil{
        panic(err)
    }
    fmt.Println("connect to es success")
    p1:=Person{
        Name:"rion",
        Age:22,
        Married:false,
    }
    put1,err:=client.Index().Index("user").BodyJson(p1).Do(context.Background())
    if err!=nil{
        panic(err)
    }
    fmt.Printf("Indexed user:%s to index %s,type %s\n",put1.Id,put1.Index,put1.Type)
}
```

更多使用详见文档：https://godoc.org/github.com/olivere/elastic





## Kibana-数据分析和可视化平台

kibana是一个开源的分析和可视化平台，设计用于和Elasticsearch一起工作。可以使用Kibana来搜索、查看、并和存储在Elasticsearch索引中的数据进行交互。可以轻松地执行高级数据分析，并且以各种图标、表格和地图的形式可视化数据。

kibana使得理解大量数据变得很容易。它简单的、基于浏览器的界面使你能够快速创建和共享动态仪表板，实时显示elasticsearch查询的变化。



### Kibana安装

官方下载连接：https://www.elastic.co/cn/downloads/kibana

==注意：kibana与elasticsearch的版本需要对应，否则可能出现不兼容！！！==

































