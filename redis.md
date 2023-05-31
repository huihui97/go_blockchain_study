# NoSQL数据库简介

## NoSQL数据库

### 概述

NoSQL（not only sql）：即“不仅仅是SQL”，泛指：非关系型数据库。

NoSQL不依赖业务逻辑方式存储，而以简单的key-value模式存储。因此，大大增加了数据库的扩展能力。



NoSQL特点：

+ 不遵循SQL标准
+ 不支持ACID（原子性、一致性、隔离性、持久性）
+ 远超SQL的性能



### 适用场景

+ 对数据高并发读写

+ 海量数据的读写

+ 对数据高扩展性的

  



### 不适用场景

+ 需要事务支持
+ 基于sql的结构化查询存储，处理复杂的关系，需要即席查询



### 常见NoSQL数据库

+ Memcache
+ redis   
+ mongoDB





## 行式存储数据库

### 行式数据库



### 列式数据库







# redis概述安装

## redis概述

+ redis是一个开源的**key-value存储系统**

+ 和Memcache类似，它支持存储的value类型相对较多，包括：字符串string，链表list，集合set，有序集合zset（sorted set）和 哈希类型hash

+ 这些数据类型都支持push/pop、add/remove及取交集和差集及更丰富的操作

+ 在该基础上，redis支持各种不同方式的排序

+ 与memcache一样，为了保证效率，数据都是缓存在内存中

+ 区别：redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件中

+ 并在此基础上实现了master-slave（主从）同步。

+ redis数据在内存中，周期性保存在到磁盘上。

  

## 适用场景

### 配合关系型数据库做高速缓存

+ 高频次，热门访问的数据，降低数据库IO
+ 分布式架构，做session共享
+ **通常作为缓存服务器，客户端访问数据时，先检查redis中是否有数据，有则直接返回客户端，没有从后端数据库（mysql）中加载数据到redis中，然后再返回给客户端**



### 多样的数据结构存储持久化数据

+ 最新N个数据 <—— 通过List实现按照自然时间排序的数据
+ 排行榜，TopN <—— 利用zset（有序集合）
+ 时效性的数据，如手机验证码 <—— Expire过期
+ 计数器，秒杀 <—— 原子性，自增方法INCR、DECR
+ 去除大量数据中的重复数据 <—— 利用set集合
+ 构建队列 <—— 利用list集合
+ 发布订阅消息系统 <—— pub/sub模式



## redis相关知识

+ redis默认端口号：6379
+ redis默认16个数据库，类似数组从下标1开始，==初始默认使用0号库==
+ 登录redis：`redis-cli`，退出redis：`exit/quit`
+ 使用 `select 库号` 来切换数据库，如`select 8`表示切换到8号数据库
+ 统一密码管理，所有库具有相同的密码
+ `dbsize`**查看当前数据库的key数量**
+ `flushdb`清空当前库
+ `flushall` 清空全部数据库

## redis优点

redis采用==单线程+多路IO复用技术==

多路复用：指使用一个线程来检查多个文件描述符（socket）的就绪状态，比如调用select和poll函数，传入多个文件描述符，如果有一个文件描述符就绪，则返回，否则阻塞直到超时。得到就绪状态后，进行真正的操作可以在同一个线程里执行，也可以启动线程执行（比如使用线程池）



Memchache数据库使用 *多线程+锁* ，与memcache相比，redis：**支持多数据类型，支持持久化，单线程+多路IO复用**

多路：多个tcp连接

复用：用一个线程或一组线程来处理这些连接





# redis常用数据类型

可以从`http://www.redis.cn/commands.html` 获得redis常见的数据类型操作命令

## 键 key 

==**redis中的key是string类型**==

```go
keys * 查看当前库中所有的key

exists key 判断某个key是否存在

type key 查看key是什么类型

del key 删除指定的key数据

unlink key 根据value选择非阻塞删除，仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作。

expire key 10  10表示10秒针，为给定的key设置过期时间，过期后则键值对已经删除了

ttl key 查看还有多少秒过期，-1表示永不过期，-2表示已经过期


select 库号  切换数据库

dbsize 查看当前数据库的key数量

flushdb 清空当前库

flushall 清空所有库
```





## 字符串 String

String是redis最基本的数据类型，String类型是==二进制安全的（即只要内容能字符串表示）==，意味着redis的string可以包含任何数据。比如 jpg图片，或者序列化的对象。

==**redis中的value若为字符串类型，则该value最大能存储512M**==

**redis中每条指令都是原子操作**

```go
//string操作 常用命令

set key value   // 添加键值对 set key value [expiration EX seconds|PX milliseconds] [NX|XX]
				//NX：当前数据库中的key不存在时，可以将key-value添加到数据库
				//XX；当前数据库中key存在，可以将key-value添加到数据库，与NX参数互斥
				//EX：key的超时秒数
				//PX：key的超时毫秒数，与EX互斥


get key   //查询键对应的值

append key value  //将给定的value追加到原值的末尾，返回修改后的值的长度

strlen key    //获取键对应的值的长度

setnx key value  //只有当key不存在时才设置key的值

incr key   //将key中存储的数字值增加1，只能对数字值操作，如果为空，新增值为1，返回增加后的结果
//注意：incr key 是原子操作，原子操作表示不会线程的调度机制打断的操作，这种操作一旦开始，就会一直运行到结束，中间不会有任何 context switch （切换到另一个线程）。
//在单线程中，能够在单条指令中完成的操作都可以认为是“原子操作”，因为中断只能发生在指令之间
//在多线程中，不能被其他进程（线程）打断的操作就叫原子操作。
//redis中的单命令的原子性主要得益于redis的单线程。


decr key //将key中存储的数字值减少1，只能对数字值操作，如果为空，新增值为-1，返回减去后的操作结果

incrby key 步长  // 将key中的存储数字值增加一个步长，自定义步长，返回增加后的结果

decrby key 步长  //将key中的存储数字值减少一个步长，自定义步长，返回减去后的结果

mset key1 value1 key2 value2 ... //同时设置一个或多个key-value对

mget key1 key2 key3 ...  //同时获取一个或多个value

msetnx key1 value1 key2 value2 ...  //同时设置一个或多个key-value对，当且仅当所给定key都不存在才能设置成功

getrange key 起始位置 结束位置  //获取值的范围，redis中字符串下标从0开始，相当于获取字串，注意两边都包括

setrange key 起始位置 value //用value值覆盖key所存储的字符串值，从起始位置开始（索引从0开始）

setex key 过期时间 value //设置键值的同时，设置过期时间，单位秒

getset key value //以新换旧，设置新值同时返回旧值
```





**string底层数据结构**：底层为简单的动态字符串。是可以修改的字符串，类似于go语言中的切片，**采用预分配冗余空间的方式来减少内存的频繁分配**。

![image-20230104233853599](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230104233853599.png)









## 列表 List

列表其实对应的单键多值（如`”name":{zhangsan,lisi,...}`）。

redis列表是简单的**字符串列表**，==**按照插入的顺序排序**==，**可以添加元素到列表头或列表尾**。==列表的底层实际上是一个**双向链表**==，对两端的操作性能很高，通过索引下标的操作中间节点的性能会较差。



```go
//list操作 常用命令

lpush/rpush key value1 value2 value3 ... //从左边/右边插入一个或多个值，返回列表的长度

lpop/rpop key //从左边/右边弹出一个值 ，值在键在，值光键亡，该操作取出一个元素，那么列表中就少了一个元素

rpoplpush key1 key2 //从key1列表右边弹出一个值（那么原列表中则没有该值了），将其插入到key2列表的左边

lrange key start stop //按照索引下标（下标从0开始）查看元素（从左至右），该操作可以获取列表中所有元素，但是键还在
						//lrange key 0 -1 表示获取列表中的所有值，0表示左边第一个，-1表示右边第一个

lindex key index //按照索引下标查看元素（从左到右）

llen key //获得列表长度，元素个数

linsert key before value newvalue //在value的前面插入newvalue值
linsert key after value newvalue //在value的后面插入newvalue值


lrem key n value //从左边删除n个value（从左到右）

lset key index value //将列表key下标为index的值替换成value

```



**list底层的数据结构**：首先在列表元素较少的情况下会使用一块连续的内存存储，结构为ziplist（压缩列表/顺序表），它将所有的元素紧挨着一起存储，分配的是一块连续的内存。 当数据量比较多的时候，结构会改成快速链表（quickList）。



![image-20230105004041760](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105004041760.png)



如果采用普通的链表结构由于有前后指针，会造成空间的浪费，为了节省空间，Redis将链表和ziplist结合起来，形成一个快速链表quicklist。即**将多个ziplist使用双向指针串联**起来使用，这样既能满足快速插入和删除，又不会出现太大的空间冗余。



## 集合 Set

redis中的set对外提供的功能与list类似，是一个列表的功能，特殊点：==set可以自动排重==，当需要存储一个列表数据，但又不希望列表中出现重复的数据时，set是一个很好的选择。

==set是string类型的**无序集合**，底层其实是一个value为null的hash表，所以添加，删除，查找的复杂度都是O(1)==。



```go
//set 常用命令

sadd key value1 value2 ...  //将一个或多个元素（member）添加到集合key中，已经存在的元素将被忽略

smembers key //查看集合中所有的值

sismember key value //判断集合key是否含有value值，有返回1，无返回0

scard key //返回集合中元素的个数

srem key value1 value2 ... //删除集合中某个元素

spop key n//随机从集合中弹出n个元素，会从集合删除

srandmember key n //随机从集合中取出n个值，但是不会从集合中删除

smove sourece distination value //把source集合中一个值移动到另一个distination集合

sinter key1 key2  //返回两个集合的交集元素

sunion key1 key2  //返回两个集合中的并集元素

sdiff key1 key2 //返回两个集合的差集元素（在key1中，但不在key2中的元素）
```







## 哈希 Hash

redis中的hash是**存储键值对的哈希表，**即是**一个string类型的field和value的哈希表**，**hash 特别适合存储对象**。value中其实包含了一个对象信息（如用户的身份Id、姓名、年龄等）。



![image-20230105154348968](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105154348968.png)





```go
//hash  常用命令

hset key field value //给key的hash表中的field键赋值value

hget key field //从key哈希表中取出field键对应的值

hmset key1 field1 value1 field2 value2 ...  //批量设置hash表的值

hexists key field //查看哈希表key中的给定的field是否存在

hkeys key //列出该哈希集合中的所有的field 

hvals key //列出该哈希集合中的所有的value

hincrby key field increment //为哈希表key中的field对应的值加上增量（increment），返回增加后的值

hsetnx key field value //将哈希表key中的field的值设置为value，当且仅当field不存在
```



```go
//示例：
127.0.0.1:6379> hmset user:1001 name zhangsan age 21 home huangmei 
OK
127.0.0.1:6379> hmset user:1002 name lisi age 32 home wuhan
OK
127.0.0.1:6379> hget user:1001 name
"zhangsan"

//注意：这里user:1001,user:1002  是一个整体，将这个整体作为key，没有其他含义。

```



hash类型对应的数据结构是两种：**ziplist（哈希列表）和hashtable（哈希表）**。当field-value的长度较短时且个数较少，使用ziplist，否者使用hashtable。





## 有序集合 Zset

redis有序集合zset与普通集合set非常类似，==**是一个没有重复元素的字符串集合**==。不同之处在于，==**有序集合的每个成员都关联了一个评分（score）**==，这个评分被用来按照从最低分到最高分的方式排序集合中的成员。==**集合的成员是唯一的，但是评分是可以重复的**==。

因为元素是有序的，所以可以根据评分或次序来获取一个范围的元素。访问有序集合的中间元素也是非常快的。因此，可以使用有序集合作为一个没有重复成员的智能列表。



```go
//zset  常用命令

//zadd key [NX|XX] [CH] [INCR] score member [score member ...]

//XX: 仅仅更新存在的成员，不添加新成员。

//NX: 不更新存在的成员。只添加新成员。

//CH: 修改返回值为发生变化的成员总数，原始是返回新添加成员的总数 (CH 是 changed 的意思)。更改的元素是新添加的成员，已经存在的成员更新分数。 所以在命令中指定的成员有相同的分数将不被计算在内。注：在通常情况下，ZADD返回值只计算新添加成员的数量。

//INCR: 当ZADD指定这个选项时，成员的操作就等同ZINCRBY命令，对成员的分数进行递增操作。







zadd key score1 value1 score2 value2 ... //将一个或多个member元素及其对应的score加入到有序集合key中


zrange key start stop [withscores] //返回有序集合key，下标在start和stop之间的元素，带withscores，可以将分数和值一起返回，返回的成员是默认按照分数从小到大排序的
zrevrange key start stop [withscores]  //同上，返回结果按照分数从大到小排序
  

zrangebyscore key min max [withscores] [limit offset count] //返回有序集合key，所有score介于min和max之间（包括min和max）的成员。有序集合成员按score值递增排序（从小到大排序）。
zrevrangebyscore key max min [withscores] [limit offset count]  //同上，返回结果按照分数从大到小排序


zscore key value    //输出指定的key下的value的对应的分数

zcard key    //返回指定key中的元素个数

zremrangebyscore key minScore maxScore  //删除key对应的zset中的分数处于minscore到maxscore之间的元素


zincrby key increment value //为元素value的score加上增量increment

zrem key value [value ...]//删除该集合下，指定的值

zcount key min max //统计该集合中分数在min到max之间元素的个数

zrank key value //返回该值在集合中的排名，从0开始，默认是从小到大进行排序的，即分数越大排名数字越大

zrevrank key value //返回该在该集合中的排名，先将元素按分数从大到小排列，然后给出排名，注意排名从0开始
```



zset是redis提供的一个非常特殊的数据结构，一方面每个元素value都有一个权重score，另一方面又类似TreeSet，内部的元素按照权重score进行排序，可以得到每个元素的名次，还可以通过score的范围来获取元素的列表。

zset底层使用了两种数据结构：hash和跳跃表

+ **hash**：**hash作用是关联数据value和权重score**，保障元素value的唯一性，可以通过元素value找到对应的score值
+ **跳跃表**：跳跃表作用在于给元素value排序，根据score的范围获取元素列表。



## 新数据类型Bitmaps

bitmaps 数据类型可以对位进行操作：

+ bitmaps本身不是一种数据类型，实际上它就是**字符串（key-value），因此最大长度也为512M**，但是它**可以对字符串的位进行操作**。
+ bitmaps单独提供了一套命令，在redis中使用bitmaps和使用字符串方法不同。**可以将bitmaps想象成一个以位为单位的数组**，==数组的每个单元智能存储0和1，数组下标在bitmaps中叫做偏移量，下标从0开始==。

![image-20230105193927957](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105193927957.png)



```go
//Bitmaps 常用命令
//setbit设置Bitmaps中的值

setbit key offset value //设置Bitmaps中某个偏移量的值（0或1），offset偏移量从0开始，返回原来该位上的值


//向key为user:20230105 中插入1 
127.0.0.1:6379> setbit users:20230105 1 1
(integer) 0
127.0.0.1:6379> setbit users:20230105 6 1
(integer) 0
127.0.0.1:6379> setbit users:20230105 11 1
(integer) 0
127.0.0.1:6379> setbit users:20230105 15 1
(integer) 0
127.0.0.1:6379> setbit users:20230105 19 1
(integer) 0
```

![image-20230105194338411](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105194338411.png)

==在初始化Bitmaps时，如果偏移量很大，那么整个初始化过程执行会比较慢，可能会造成redis的阻塞==





```go
//getbit获取Bitmaps中的值

getbit key offset //获取Bitmaps中某个偏移量的值，offset从0开始

127.0.0.1:6379> getbit users:20230105 11
(integer) 1
127.0.0.1:6379> getbit users:20230105 10
(integer) 0
```



```go
//bitcount统计字符串被设置为1的bit数。一般情况下，给定的整个字符串都会被进行计数，通过指定额外的start和end参数，就可以让计数只在特定的位上进行。start和end参数的设置，都可以使用负数值：比如-1表示最后一位，-2表示倒数第二位，start、end是指bit组的字节下标数，二者皆包含

//一定要注意：这里是字节下标，一个字节占8位
bitcount key start end //统计字符串从start字节到end字节，注意是字节（按照8个bit位划分为一个字节），字节下标也是从0开始


127.0.0.1:6379> bitcount users:20230105 0 3  //从下标0字节到下标3字节
(integer) 5
```



```go
//bitop是一个复合操作，它可以做多个Bitmaps的and（交集）、or（并集）、not（非）、xor（异或）操作，并将结果保存在destkey中

bitop and(or/not/xor) destkey key1 key2 ... //将key1和key2的交集/并集/非/异或操作 结果保存在destkey中
```





**Bitmaps和set对比**

![image-20230105202134920](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105202134920.png)

![image-20230105202209849](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105202209849.png)

![image-20230105202311343](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105202311343.png)

![image-20230105202332555](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105202332555.png)







## 新数据类型HyperLogLog

在实际工作中，经常遇到统计相关功能的需求，比如**统计网站PV（pageview页面访问量）**，可以使用redis的incr、incrby来实现。

但是像UV（uniquevistor，独立访客）、独立IP数、搜索记录数等需要去重和计数的问题如何解决？这种集合中不重复元素个数问题称为基数问题。

**解决基数问题的方案：**

+ 数据存储在mysql表中，使用distinct count 计算不重复个数
+ 使用redis提供的hash、set、bitmaps等数据结构来处理。

这些方案结果精确，但随着数据不断增加，导致空间越来越大，对于非常大的数据集不切实际。



能否降低一定的精度来平衡存储空间？redis推出了HyperLogLog。

==**HyperLogLog是用来做基数统计的算法**==，**HyperLogLog的优点是在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的、并且时很小的。**

在redis里面，**每个HyperLogLog键只需要12KB内存**，就可以计算接近2^64个不同元素的基数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明的对比。但是，因为**HyperLogLog只会根据输入元素来计算基数，而不会存储输入元素的本身**，所以HyperLogLog不能像集合那样，返回输入的各个元素。



基数：比如数据集{1，3，5，7，5，7，9}，那么这个数据集的基数集位{1，3，5，7，9}，基数（不重复元素）为5。基数估计就是在误差可接受范围内，快速计算基数。





```go
//HyperLogLog 命令

pfadd key element1 element2 ... //添加指定元素到HyperLogLog中

pfcount key1 key2 ...  //计算key中存储了多少个元素，而无法不会返回元素具体值

pfmerge destkey sourcekey1 sourcekey2 ... //将一个或多个sourcekey合并后的结果存储在destkey中，比如每月的活跃用户可以使用每天的活跃用户来合并计算可得
```





## 新数据类型Geospatial

redis3.2版本中增加了对GEO类型的支持。GEO，Geographic，地理信息的缩写。**该类型是元素的2维坐标**，**在地图上就是经纬度。**redis基于该类型，提供了经纬度设置，查询，范围查询，距离查询，经纬度Hash等常见的操作。



```go
//Geospatial 常用命令

geoadd key longitude(经度) latitude(维度) member(名称) [longitude latitude member...]  //添加地理位置（经度、纬度、名称），可以添加多条数据

注意：北极和南极无法添加，有效的经度从-180度到180度，有效纬度 -85.05112878度 到 85.05112878度。 当坐标位置超过指定范围时，该命令将会返回一个错误。已经添加的数据，是无法再次往里添加的。


//示例
127.0.0.1:6379> geoadd china:city 121.47 31.23 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 106.50 29.53 chongqing 114.05 22.52 shenzhen
(integer) 2



geopos key member1 member2 ...   //获取指定地区的坐标值

//示例
127.0.0.1:6379> geopos china:city shanghai chongqing
1) 1) "121.47000163793563843"
   2) "31.22999903975783553"
2) 1) "106.49999767541885376"
   2) "29.52999957900659211"



geodist key member1 member2 [m|km|ft|mi]  //获取两个位置之间的直线距离
//注意：[m|km|ft|mi]表示单位，m为米，km为千米，ft为英里，mi为英尺，如果没有显示指定单位参数，则默认使用m作为单位

//示例
127.0.0.1:6379> geodist china:city shanghai chongqing km
"1447.6737"


georadius key longitude latitude radius m|km|ft|mi  //以给定的经纬度为中心，找出某一个半径内的元素，radius表示为范围距离，m|km|ft|mi采用什么单位

//示例
127.0.0.1:6379> georadius china:city 110 30 1000 km
1) "chongqing"
2) "shenzhen"
```

















# redis配置文件详解



```shell
//redis 配置文件

# Redis configuration file example

//单位换算描述
# Note on units: when memory size is needed, it is possible to specify
# it in the usual form of 1k 5GB 4M and so forth:
#
# 1k => 1000 bytes         
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
#
# units are case insensitive so 1GB 1Gb 1gB are all the same.

################################## INCLUDES ###################################

# Include one or more other config files here.  This is useful if you
# have a standard template that goes to all Redis servers but also need
# to customize a few per-server settings.  Include files can include
# other files, so use this wisely.
#
# Notice option "include" won't be rewritten by command "CONFIG REWRITE"
# from admin or Redis Sentinel. Since Redis always uses the last processed
# line as value of a configuration directive, you'd better put includes
# at the beginning of this file to avoid overwriting config change at runtime.
#
# If instead you are interested in using includes to override configuration
# options, it is better to use include as the last line.
# 通常用于redis的主从复制，搭建集群时候使用
#  环境中使用的redis.conf可以包含其他的redis.conf
# include .\path\to\local.conf
# include c:\path\to\other.conf

################################## MODULES #####################################

# Load modules at startup. If the server is not able to load modules
# it will abort. It is possible to use multiple loadmodule directives.
#
# loadmodule .\path\to\my_module.dll
# loadmodule c:\path\to\other_module.dll

################################## NETWORK #####################################

# By default, if no "bind" configuration directive is specified, Redis listens
# for connections from all the network interfaces available on the server.
# It is possible to listen to just one or multiple selected interfaces using
# the "bind" configuration directive, followed by one or more IP addresses.
#
# Examples:
#
# bind 192.168.1.100 10.0.0.1
# bind 127.0.0.1 ::1
#
# ~~~ WARNING ~~~ If the computer running Redis is directly exposed to the
# internet, binding to all the interfaces is dangerous and will expose the
# instance to everybody on the internet. So by default we uncomment the
# following bind directive, that will force Redis to listen only into
# the IPv4 loopback interface address (this means Redis will be able to
# accept connections only from clients running into the same computer it
# is running).
#
# IF YOU ARE SURE YOU WANT YOUR INSTANCE TO LISTEN TO ALL THE INTERFACES
# JUST COMMENT THE FOLLOWING LINE.
# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bind 127.0.0.1   #表示只支持本地连接，而不支持远程连接，将 bind 127.0.0.1 给注释了，就支持远程连接了

# Protected mode is a layer of security protection, in order to avoid that
# Redis instances left open on the internet are accessed and exploited.
#
# When protected mode is on and if:
#
# 1) The server is not binding explicitly to a set of addresses using the
#    "bind" directive.
# 2) No password is configured.
#
# The server only accepts connections from clients connecting from the
# IPv4 and IPv6 loopback addresses 127.0.0.1 and ::1, and from Unix domain
# sockets.
#
# By default protected mode is enabled. You should disable it only if
# you are sure you want clients from other hosts to connect to Redis
# even if no authentication is configured, nor a specific set of interfaces
# are explicitly listed using the "bind" directive.
protected-mode yes    #表示开启保护模式，本机访问，远程不能访问，改成no则远程也可以访问

# Accept connections on the specified port, default is 6379 (IANA #815344).
# If port 0 is specified Redis will not listen on a TCP socket.
port 6379   #默认端口号

# TCP listen() backlog.
#
# In high requests-per-second environments you need an high backlog in order
# to avoid slow clients connections issues. Note that the Linux kernel
# will silently truncate it to the value of /proc/sys/net/core/somaxconn so
# make sure to raise both the value of somaxconn and tcp_max_syn_backlog
# in order to get the desired effect.
tcp-backlog 511   #backlog是一个连接队列，backlog队列总和=未完成三次握手+已完成三次握手队列，在高并发环境中，需要一个
				 #高的backlog值来避免慢客户端连接问题。

# Unix socket.
#
# Specify the path for the Unix socket that will be used to listen for
# incoming connections. There is no default, so Redis will not listen
# on a unix socket when not specified.
#
# unixsocket /tmp/redis.sock
# unixsocketperm 700

# Close the connection after a client is idle for N seconds (0 to disable)
timeout 0   #表示连接redis超时时间（单位秒），0 表示永不超时

# TCP keepalive.
#
# If non-zero, use SO_KEEPALIVE to send TCP ACKs to clients in absence
# of communication. This is useful for two reasons:
#
# 1) Detect dead peers.
# 2) Take the connection alive from the point of view of network
#    equipment in the middle.
#
# On Linux, the specified value (in seconds) is the period used to send ACKs.
# Note that to close the connection the double of the time is needed.
# On other kernels the period depends on the kernel configuration.
#
# A reasonable value for this option is 300 seconds, which is the new
# Redis default starting with Redis 3.2.1.
tcp-keepalive 300  #表示 心跳检测，每n秒检测一次

################################# GENERAL #####################################

# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
# NOT SUPPORTED ON WINDOWS daemonize no

# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
# NOT SUPPORTED ON WINDOWS supervised no

# If a pid file is specified, Redis writes it where specified at startup
# and removes it at exit.
#
# When the server runs non daemonized, no pid file is created if none is
# specified in the configuration. When the server is daemonized, the pid file
# is used even if not specified, defaulting to "/var/run/redis.pid".
#
# Creating a pid file is best effort: if Redis is not able to create it
# nothing bad happens, the server will start and run normally.
# NOT SUPPORTED ON WINDOWS pidfile /var/run/redis.pid

# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice     #表示日志级别

# Specify the log file name. Also 'stdout' can be used to force
# Redis to log on the standard output.
logfile ""  #设置日志文件输出路径，默认为空

# To enable logging to the Windows EventLog, just set 'syslog-enabled' to
# yes, and optionally update the other syslog parameters to suit your needs.
# If Redis is installed and launched as a Windows Service, this will
# automatically be enabled.
# syslog-enabled no  

# Specify the source name of the events in the Windows Application log.
# syslog-ident redis

# Specify the syslog facility. Must be USER or between LOCAL0-LOCAL7.
# NOT SUPPORTED ON WINDOWS syslog-facility local0

# Set the number of databases. The default database is DB 0, you can select
# a different one on a per-connection basis using SELECT <dbid> where
# dbid is a number between 0 and 'databases'-1
databases 16   #表示 redis默认数据库数量

# By default Redis shows an ASCII art logo only when started to log to the
# standard output and if the standard output is a TTY. Basically this means
# that normally a logo is displayed only in interactive sessions.
#
# However it is possible to force the pre-4.0 behavior and always show a
# ASCII art logo in startup logs by setting the following option to yes.
always-show-logo yes   # 是否总是显示logo

################################ SNAPSHOTTING  ################################
#
# Save the DB on disk:
#
#   save <seconds> <changes>
#
#   Will save the DB if both the given number of seconds and the given
#   number of write operations against the DB occurred.
#
#   In the example below the behaviour will be to save:
#   after 900 sec (15 min) if at least 1 key changed
#   after 300 sec (5 min) if at least 10 keys changed
#   after 60 sec if at least 10000 keys changed
#
#   Note: you can disable saving completely by commenting out all "save" lines.
#
#   It is also possible to remove all the previously configured save
#   points by adding a save directive with a single empty string argument
#   like in the following example:
#
#   save ""

save 900 1
save 300 10
save 60 10000

# By default Redis will stop accepting writes if RDB snapshots are enabled
# (at least one save point) and the latest background save failed.
# This will make the user aware (in a hard way) that data is not persisting
# on disk properly, otherwise chances are that no one will notice and some
# disaster will happen.
#
# If the background saving process will start working again Redis will
# automatically allow writes again.
#
# However if you have setup your proper monitoring of the Redis server
# and persistence, you may want to disable this feature so that Redis will
# continue to work as usual even if there are problems with disk,
# permissions, and so forth.
stop-writes-on-bgsave-error yes

# Compress string objects using LZF when dump .rdb databases?
# For default that's set to 'yes' as it's almost always a win.
# If you want to save some CPU in the saving child set it to 'no' but
# the dataset will likely be bigger if you have compressible values or keys.
rdbcompression yes

# Since version 5 of RDB a CRC64 checksum is placed at the end of the file.
# This makes the format more resistant to corruption but there is a performance
# hit to pay (around 10%) when saving and loading RDB files, so you can disable it
# for maximum performances.
#
# RDB files created with checksum disabled have a checksum of zero that will
# tell the loading code to skip the check.
rdbchecksum yes

# The filename where to dump the DB
dbfilename dump.rdb

# The working directory.
#
# The DB will be written inside this directory, with the filename specified
# above using the 'dbfilename' configuration directive.
#
# The Append Only File will also be created inside this directory.
#
# Note that you must specify a directory here, not a file name.
dir ./

################################# REPLICATION #################################

# Master-Replica replication. Use replicaof to make a Redis instance a copy of
# another Redis server. A few things to understand ASAP about Redis replication.
#
#   +------------------+      +---------------+
#   |      Master      | ---> |    Replica    |
#   | (receive writes) |      |  (exact copy) |
#   +------------------+      +---------------+
#
# 1) Redis replication is asynchronous, but you can configure a master to
#    stop accepting writes if it appears to be not connected with at least
#    a given number of replicas.
# 2) Redis replicas are able to perform a partial resynchronization with the
#    master if the replication link is lost for a relatively small amount of
#    time. You may want to configure the replication backlog size (see the next
#    sections of this file) with a sensible value depending on your needs.
# 3) Replication is automatic and does not need user intervention. After a
#    network partition replicas automatically try to reconnect to masters
#    and resynchronize with them.
#
# replicaof <masterip> <masterport>

# If the master is password protected (using the "requirepass" configuration
# directive below) it is possible to tell the replica to authenticate before
# starting the replication synchronization process, otherwise the master will
# refuse the replica request.
#
# masterauth <master-password>

# When a replica loses its connection with the master, or when the replication
# is still in progress, the replica can act in two different ways:
#
# 1) if replica-serve-stale-data is set to 'yes' (the default) the replica will
#    still reply to client requests, possibly with out of date data, or the
#    data set may just be empty if this is the first synchronization.
#
# 2) if replica-serve-stale-data is set to 'no' the replica will reply with
#    an error "SYNC with master in progress" to all the kind of commands
#    but to INFO, replicaOF, AUTH, PING, SHUTDOWN, REPLCONF, ROLE, CONFIG,
#    SUBSCRIBE, UNSUBSCRIBE, PSUBSCRIBE, PUNSUBSCRIBE, PUBLISH, PUBSUB,
#    COMMAND, POST, HOST: and LATENCY.
#
replica-serve-stale-data yes

# You can configure a replica instance to accept writes or not. Writing against
# a replica instance may be useful to store some ephemeral data (because data
# written on a replica will be easily deleted after resync with the master) but
# may also cause problems if clients are writing to it because of a
# misconfiguration.
#
# Since Redis 2.6 by default replicas are read-only.
#
# Note: read only replicas are not designed to be exposed to untrusted clients
# on the internet. It's just a protection layer against misuse of the instance.
# Still a read only replica exports by default all the administrative commands
# such as CONFIG, DEBUG, and so forth. To a limited extent you can improve
# security of read only replicas using 'rename-command' to shadow all the
# administrative / dangerous commands.
replica-read-only yes

# Replication SYNC strategy: disk or socket.
#
# -------------------------------------------------------
# WARNING: DISKLESS REPLICATION IS EXPERIMENTAL CURRENTLY
# -------------------------------------------------------
#
# New replicas and reconnecting replicas that are not able to continue the replication
# process just receiving differences, need to do what is called a "full
# synchronization". An RDB file is transmitted from the master to the replicas.
# The transmission can happen in two different ways:
#
# 1) Disk-backed: The Redis master creates a new process that writes the RDB
#                 file on disk. Later the file is transferred by the parent
#                 process to the replicas incrementally.
# 2) Diskless: The Redis master creates a new process that directly writes the
#              RDB file to replica sockets, without touching the disk at all.
#
# With disk-backed replication, while the RDB file is generated, more replicas
# can be queued and served with the RDB file as soon as the current child producing
# the RDB file finishes its work. With diskless replication instead once
# the transfer starts, new replicas arriving will be queued and a new transfer
# will start when the current one terminates.
#
# When diskless replication is used, the master waits a configurable amount of
# time (in seconds) before starting the transfer in the hope that multiple replicas
# will arrive and the transfer can be parallelized.
#
# With slow disks and fast (large bandwidth) networks, diskless replication
# works better.
repl-diskless-sync no

# When diskless replication is enabled, it is possible to configure the delay
# the server waits in order to spawn the child that transfers the RDB via socket
# to the replicas.
#
# This is important since once the transfer starts, it is not possible to serve
# new replicas arriving, that will be queued for the next RDB transfer, so the server
# waits a delay in order to let more replicas arrive.
#
# The delay is specified in seconds, and by default is 5 seconds. To disable
# it entirely just set it to 0 seconds and the transfer will start ASAP.
repl-diskless-sync-delay 5

# Replicas send PINGs to server in a predefined interval. It's possible to change
# this interval with the repl_ping_replica_period option. The default value is 10
# seconds.
#
# repl-ping-replica-period 10

# The following option sets the replication timeout for:
#
# 1) Bulk transfer I/O during SYNC, from the point of view of replica.
# 2) Master timeout from the point of view of replicas (data, pings).
# 3) Replica timeout from the point of view of masters (REPLCONF ACK pings).
#
# It is important to make sure that this value is greater than the value
# specified for repl-ping-replica-period otherwise a timeout will be detected
# every time there is low traffic between the master and the replica.
#
# repl-timeout 60

# Disable TCP_NODELAY on the replica socket after SYNC?
#
# If you select "yes" Redis will use a smaller number of TCP packets and
# less bandwidth to send data to replicas. But this can add a delay for
# the data to appear on the replica side, up to 40 milliseconds with
# Linux kernels using a default configuration.
#
# If you select "no" the delay for data to appear on the replica side will
# be reduced but more bandwidth will be used for replication.
#
# By default we optimize for low latency, but in very high traffic conditions
# or when the master and replicas are many hops away, turning this to "yes" may
# be a good idea.
repl-disable-tcp-nodelay no

# Set the replication backlog size. The backlog is a buffer that accumulates
# replica data when replicas are disconnected for some time, so that when a replica
# wants to reconnect again, often a full resync is not needed, but a partial
# resync is enough, just passing the portion of data the replica missed while
# disconnected.
#
# The bigger the replication backlog, the longer the time the replica can be
# disconnected and later be able to perform a partial resynchronization.
#
# The backlog is only allocated once there is at least a replica connected.
#
# repl-backlog-size 1mb

# After a master has no longer connected replicas for some time, the backlog
# will be freed. The following option configures the amount of seconds that
# need to elapse, starting from the time the last replica disconnected, for
# the backlog buffer to be freed.
#
# Note that replicas never free the backlog for timeout, since they may be
# promoted to masters later, and should be able to correctly "partially
# resynchronize" with the replicas: hence they should always accumulate backlog.
#
# A value of 0 means to never release the backlog.
#
# repl-backlog-ttl 3600

# The replica priority is an integer number published by Redis in the INFO output.
# It is used by Redis Sentinel in order to select a replica to promote into a
# master if the master is no longer working correctly.
#
# A replica with a low priority number is considered better for promotion, so
# for instance if there are three replicas with priority 10, 100, 25 Sentinel will
# pick the one with priority 10, that is the lowest.
#
# However a special priority of 0 marks the replica as not able to perform the
# role of master, so a replica with priority of 0 will never be selected by
# Redis Sentinel for promotion.
#
# By default the priority is 100.
replica-priority 100

# It is possible for a master to stop accepting writes if there are less than
# N replicas connected, having a lag less or equal than M seconds.
#
# The N replicas need to be in "online" state.
#
# The lag in seconds, that must be <= the specified value, is calculated from
# the last ping received from the replica, that is usually sent every second.
#
# This option does not GUARANTEE that N replicas will accept the write, but
# will limit the window of exposure for lost writes in case not enough replicas
# are available, to the specified number of seconds.
#
# For example to require at least 3 replicas with a lag <= 10 seconds use:
#
# min-replicas-to-write 3
# min-replicas-max-lag 10
#
# Setting one or the other to 0 disables the feature.
#
# By default min-replicas-to-write is set to 0 (feature disabled) and
# min-replicas-max-lag is set to 10.

# A Redis master is able to list the address and port of the attached
# replicas in different ways. For example the "INFO replication" section
# offers this information, which is used, among other tools, by
# Redis Sentinel in order to discover replica instances.
# Another place where this info is available is in the output of the
# "ROLE" command of a master.
#
# The listed IP and address normally reported by a replica is obtained
# in the following way:
#
#   IP: The address is auto detected by checking the peer address
#   of the socket used by the replica to connect with the master.
#
#   Port: The port is communicated by the replica during the replication
#   handshake, and is normally the port that the replica is using to
#   listen for connections.
#
# However when port forwarding or Network Address Translation (NAT) is
# used, the replica may be actually reachable via different IP and port
# pairs. The following two options can be used by a replica in order to
# report to its master a specific set of IP and port, so that both INFO
# and ROLE will report those values.
#
# There is no need to use both the options if you need to override just
# the port or the IP address.
#
# replica-announce-ip 5.5.5.5
# replica-announce-port 1234

################################## SECURITY ###################################

# Require clients to issue AUTH <PASSWORD> before processing any other
# commands.  This might be useful in environments in which you do not trust
# others with access to the host running redis-server.
#
# This should stay commented out for backward compatibility and because most
# people do not need auth (e.g. they run their own servers).
#
# Warning: since Redis is pretty fast an outside user can try up to
# 150k passwords per second against a good box. This means that you should
# use a very strong password otherwise it will be very easy to break.
#
# requirepass foobared  //表示无密码设置，将注释去掉，可以设置密码

# Command renaming.
#
# It is possible to change the name of dangerous commands in a shared
# environment. For instance the CONFIG command may be renamed into something
# hard to guess so that it will still be available for internal-use tools
# but not available for general clients.
#
# Example:
#
# rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52
#
# It is also possible to completely kill a command by renaming it into
# an empty string:
#
# rename-command CONFIG ""
#
# Please note that changing the name of commands that are logged into the
# AOF file or transmitted to replicas may cause problems.

################################### CLIENTS ####################################

# Set the max number of connected clients at the same time. By default
# this limit is set to 10000 clients, however if the Redis server is not
# able to configure the process file limit to allow for the specified limit
# the max number of allowed clients is set to the current file limit
# minus 32 (as Redis reserves a few file descriptors for internal uses).
#
# Once the limit is reached Redis will close all the new connections sending
# an error 'max number of clients reached'.
#
# maxclients 10000  //设置客户端的最大连接数，默认注释掉了，没有设置

############################## MEMORY MANAGEMENT ################################

# If Redis is to be used as an in-memory-only cache without any kind of
# persistence, then the fork() mechanism used by the background AOF/RDB
# persistence is unnecessary. As an optimization, all persistence can be
# turned off in the Windows version of Redis. This will redirect heap
# allocations to the system heap allocator, and disable commands that would
# otherwise cause fork() operations: BGSAVE and BGREWRITEAOF.
# This flag may not be combined with any of the other flags that configure
# AOF and RDB operations.
# persistence-available [(yes)|no]

# Set a memory usage limit to the specified amount of bytes.
# When the memory limit is reached Redis will try to remove keys
# according to the eviction policy selected (see maxmemory-policy).
#
# If Redis can't remove keys according to the policy, or if the policy is
# set to 'noeviction', Redis will start to reply with errors to commands
# that would use more memory, like SET, LPUSH, and so on, and will continue
# to reply to read-only commands like GET.
#
# This option is usually useful when using Redis as an LRU or LFU cache, or to
# set a hard memory limit for an instance (using the 'noeviction' policy).
#
# WARNING: If you have replicas attached to an instance with maxmemory on,
# the size of the output buffers needed to feed the replicas are subtracted
# from the used memory count, so that network problems / resyncs will
# not trigger a loop where keys are evicted, and in turn the output
# buffer of replicas is full with DELs of keys evicted triggering the deletion
# of more keys, and so forth until the database is completely emptied.
#
# In short... if you have replicas attached it is suggested that you set a lower
# limit for maxmemory so that there is some free RAM on the system for replica
# output buffers (but this is not needed if the policy is 'noeviction').
#
# WARNING: not setting maxmemory will cause Redis to terminate with an
# out-of-memory exception if the heap limit is reached.
#
# NOTE: since Redis uses the system paging file to allocate the heap memory,
# the Working Set memory usage showed by the Windows Task Manager or by other
# tools such as ProcessExplorer will not always be accurate. For example, right
# after a background save of the RDB or the AOF files, the working set value
# may drop significantly. In order to check the correct amount of memory used
# by the redis-server to store the data, use the INFO client command. The INFO
# command shows only the memory used to store the redis data, not the extra
# memory used by the Windows process for its own requirements. Th3 extra amount
# of memory not reported by the INFO command can be calculated subtracting the
# Peak Working Set reported by the Windows Task Manager and the used_memory_peak
# reported by the INFO command.
#
# maxmemory <bytes>  //设置redis可以使用的内存量，一旦达到内存使用上限，redis会试图移除内部数据

# MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached. You can select among five behaviors:
#
# volatile-lru -> Evict using approximated LRU among the keys with an expire set.
# allkeys-lru -> Evict any key using approximated LRU.
# volatile-lfu -> Evict using approximated LFU among the keys with an expire set.
# allkeys-lfu -> Evict any key using approximated LFU.
# volatile-random -> Remove a random key among the ones with an expire set.
# allkeys-random -> Remove a random key, any key.
# volatile-ttl -> Remove the key with the nearest expire time (minor TTL)
# noeviction -> Don't evict anything, just return an error on write operations.
#
# LRU means Least Recently Used
# LFU means Least Frequently Used
#
# Both LRU, LFU and volatile-ttl are implemented using approximated
# randomized algorithms.
#
# Note: with any of the above policies, Redis will return an error on write
#       operations, when there are no suitable keys for eviction.
#
#       At the date of writing these commands are: set setnx setex append
#       incr decr rpush lpush rpushx lpushx linsert lset rpoplpush sadd
#       sinter sinterstore sunion sunionstore sdiff sdiffstore zadd zincrby
#       zunionstore zinterstore hset hsetnx hmset hincrby incrby decrby
#       getset mset msetnx exec sort
#
# The default is:
#
# maxmemory-policy noeviction

# LRU, LFU and minimal TTL algorithms are not precise algorithms but approximated
# algorithms (in order to save memory), so you can tune it for speed or
# accuracy. For default Redis will check five keys and pick the one that was
# used less recently, you can change the sample size using the following
# configuration directive.
#
# The default of 5 produces good enough results. 10 Approximates very closely
# true LRU but costs more CPU. 3 is faster but not very accurate.
#
# maxmemory-samples 5

# Starting from Redis 5, by default a replica will ignore its maxmemory setting
# (unless it is promoted to master after a failover or manually). It means
# that the eviction of keys will be just handled by the master, sending the
# DEL commands to the replica as keys evict in the master side.
#
# This behavior ensures that masters and replicas stay consistent, and is usually
# what you want, however if your replica is writable, or you want the replica to have
# a different memory setting, and you are sure all the writes performed to the
# replica are idempotent, then you may change this default (but be sure to understand
# what you are doing).
#
# Note that since the replica by default does not evict, it may end using more
# memory than the one set via maxmemory (there are certain buffers that may
# be larger on the replica, or data structures may sometimes take more memory and so
# forth). So make sure you monitor your replicas and make sure they have enough
# memory to never hit a real out-of-memory condition before the master hits
# the configured maxmemory setting.
#
# replica-ignore-maxmemory yes

############################# LAZY FREEING ####################################

# Redis has two primitives to delete keys. One is called DEL and is a blocking
# deletion of the object. It means that the server stops processing new commands
# in order to reclaim all the memory associated with an object in a synchronous
# way. If the key deleted is associated with a small object, the time needed
# in order to execute the DEL command is very small and comparable to most other
# O(1) or O(log_N) commands in Redis. However if the key is associated with an
# aggregated value containing millions of elements, the server can block for
# a long time (even seconds) in order to complete the operation.
#
# For the above reasons Redis also offers non blocking deletion primitives
# such as UNLINK (non blocking DEL) and the ASYNC option of FLUSHALL and
# FLUSHDB commands, in order to reclaim memory in background. Those commands
# are executed in constant time. Another thread will incrementally free the
# object in the background as fast as possible.
#
# DEL, UNLINK and ASYNC option of FLUSHALL and FLUSHDB are user-controlled.
# It's up to the design of the application to understand when it is a good
# idea to use one or the other. However the Redis server sometimes has to
# delete keys or flush the whole database as a side effect of other operations.
# Specifically Redis deletes objects independently of a user call in the
# following scenarios:
#
# 1) On eviction, because of the maxmemory and maxmemory policy configurations,
#    in order to make room for new data, without going over the specified
#    memory limit.
# 2) Because of expire: when a key with an associated time to live (see the
#    EXPIRE command) must be deleted from memory.
# 3) Because of a side effect of a command that stores data on a key that may
#    already exist. For example the RENAME command may delete the old key
#    content when it is replaced with another one. Similarly SUNIONSTORE
#    or SORT with STORE option may delete existing keys. The SET command
#    itself removes any old content of the specified key in order to replace
#    it with the specified string.
# 4) During replication, when a replica performs a full resynchronization with
#    its master, the content of the whole database is removed in order to
#    load the RDB file just transferred.
#
# In all the above cases the default is to delete objects in a blocking way,
# like if DEL was called. However you can configure each case specifically
# in order to instead release memory in a non-blocking way like if UNLINK
# was called, using the following configuration directives:

lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
replica-lazy-flush no

############################## APPEND ONLY MODE ###############################

# By default Redis asynchronously dumps the dataset on disk. This mode is
# good enough in many applications, but an issue with the Redis process or
# a power outage may result into a few minutes of writes lost (depending on
# the configured save points).
#
# The Append Only File is an alternative persistence mode that provides
# much better durability. For instance using the default data fsync policy
# (see later in the config file) Redis can lose just one second of writes in a
# dramatic event like a server power outage, or a single write if something
# wrong with the Redis process itself happens, but the operating system is
# still running correctly.
#
# AOF and RDB persistence can be enabled at the same time without problems.
# If the AOF is enabled on startup Redis will load the AOF, that is the file
# with the better durability guarantees.
#
# Please check http://redis.io/topics/persistence for more information.

appendonly no

# The name of the append only file (default: "appendonly.aof")

appendfilename "appendonly.aof"

# The fsync() call tells the Operating System to actually write data on disk
# instead of waiting for more data in the output buffer. Some OS will really flush
# data on disk, some other OS will just try to do it ASAP.
#
# Redis supports three different modes:
#
# no: don't fsync, just let the OS flush the data when it wants. Faster.
# always: fsync after every write to the append only log. Slow, Safest.
# everysec: fsync only one time every second. Compromise.
#
# The default is "everysec", as that's usually the right compromise between
# speed and data safety. It's up to you to understand if you can relax this to
# "no" that will let the operating system flush the output buffer when
# it wants, for better performances (but if you can live with the idea of
# some data loss consider the default persistence mode that's snapshotting),
# or on the contrary, use "always" that's very slow but a bit safer than
# everysec.
#
# More details please check the following article:
# http://antirez.com/post/redis-persistence-demystified.html
#
# If unsure, use "everysec".

# appendfsync always
appendfsync everysec
# appendfsync no

# When the AOF fsync policy is set to always or everysec, and a background
# saving process (a background save or AOF log background rewriting) is
# performing a lot of I/O against the disk, in some Linux configurations
# Redis may block too long on the fsync() call. Note that there is no fix for
# this currently, as even performing fsync in a different thread will block
# our synchronous write(2) call.
#
# In order to mitigate this problem it's possible to use the following option
# that will prevent fsync() from being called in the main process while a
# BGSAVE or BGREWRITEAOF is in progress.
#
# This means that while another child is saving, the durability of Redis is
# the same as "appendfsync none". In practical terms, this means that it is
# possible to lose up to 30 seconds of log in the worst scenario (with the
# default Linux settings).
#
# If you have latency problems turn this to "yes". Otherwise leave it as
# "no" that is the safest pick from the point of view of durability.

no-appendfsync-on-rewrite no

# Automatic rewrite of the append only file.
# Redis is able to automatically rewrite the log file implicitly calling
# BGREWRITEAOF when the AOF log size grows by the specified percentage.
#
# This is how it works: Redis remembers the size of the AOF file after the
# latest rewrite (if no rewrite has happened since the restart, the size of
# the AOF at startup is used).
#
# This base size is compared to the current size. If the current size is
# bigger than the specified percentage, the rewrite is triggered. Also
# you need to specify a minimal size for the AOF file to be rewritten, this
# is useful to avoid rewriting the AOF file even if the percentage increase
# is reached but it is still pretty small.
#
# Specify a percentage of zero in order to disable the automatic AOF
# rewrite feature.

auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb

# An AOF file may be found to be truncated at the end during the Redis
# startup process, when the AOF data gets loaded back into memory.
# This may happen when the system where Redis is running
# crashes, especially when an ext4 filesystem is mounted without the
# data=ordered option (however this can't happen when Redis itself
# crashes or aborts but the operating system still works correctly).
#
# Redis can either exit with an error when this happens, or load as much
# data as possible (the default now) and start if the AOF file is found
# to be truncated at the end. The following option controls this behavior.
#
# If aof-load-truncated is set to yes, a truncated AOF file is loaded and
# the Redis server starts emitting a log to inform the user of the event.
# Otherwise if the option is set to no, the server aborts with an error
# and refuses to start. When the option is set to no, the user requires
# to fix the AOF file using the "redis-check-aof" utility before to restart
# the server.
#
# Note that if the AOF file will be found to be corrupted in the middle
# the server will still exit with an error. This option only applies when
# Redis will try to read more data from the AOF file but not enough bytes
# will be found.
aof-load-truncated yes

# When rewriting the AOF file, Redis is able to use an RDB preamble in the
# AOF file for faster rewrites and recoveries. When this option is turned
# on the rewritten AOF file is composed of two different stanzas:
#
#   [RDB file][AOF tail]
#
# When loading Redis recognizes that the AOF file starts with the "REDIS"
# string and loads the prefixed RDB file, and continues loading the AOF
# tail.
aof-use-rdb-preamble yes

################################ LUA SCRIPTING  ###############################

# Max execution time of a Lua script in milliseconds.
#
# If the maximum execution time is reached Redis will log that a script is
# still in execution after the maximum allowed time and will start to
# reply to queries with an error.
#
# When a long running script exceeds the maximum execution time only the
# SCRIPT KILL and SHUTDOWN NOSAVE commands are available. The first can be
# used to stop a script that did not yet called write commands. The second
# is the only way to shut down the server in the case a write command was
# already issued by the script but the user doesn't want to wait for the natural
# termination of the script.
#
# Set it to 0 or a negative value for unlimited execution without warnings.
lua-time-limit 5000

################################ REDIS CLUSTER  ###############################

# Normal Redis instances can't be part of a Redis Cluster; only nodes that are
# started as cluster nodes can. In order to start a Redis instance as a
# cluster node enable the cluster support uncommenting the following:
#
# cluster-enabled yes

# Every cluster node has a cluster configuration file. This file is not
# intended to be edited by hand. It is created and updated by Redis nodes.
# Every Redis Cluster node requires a different cluster configuration file.
# Make sure that instances running in the same system do not have
# overlapping cluster configuration file names.
#
# cluster-config-file nodes-6379.conf

# Cluster node timeout is the amount of milliseconds a node must be unreachable
# for it to be considered in failure state.
# Most other internal time limits are multiple of the node timeout.
#
# cluster-node-timeout 15000

# A replica of a failing master will avoid to start a failover if its data
# looks too old.
#
# There is no simple way for a replica to actually have an exact measure of
# its "data age", so the following two checks are performed:
#
# 1) If there are multiple replicas able to failover, they exchange messages
#    in order to try to give an advantage to the replica with the best
#    replication offset (more data from the master processed).
#    Replicas will try to get their rank by offset, and apply to the start
#    of the failover a delay proportional to their rank.
#
# 2) Every single replica computes the time of the last interaction with
#    its master. This can be the last ping or command received (if the master
#    is still in the "connected" state), or the time that elapsed since the
#    disconnection with the master (if the replication link is currently down).
#    If the last interaction is too old, the replica will not try to failover
#    at all.
#
# The point "2" can be tuned by user. Specifically a replica will not perform
# the failover if, since the last interaction with the master, the time
# elapsed is greater than:
#
#   (node-timeout * replica-validity-factor) + repl-ping-replica-period
#
# So for example if node-timeout is 30 seconds, and the replica-validity-factor
# is 10, and assuming a default repl-ping-replica-period of 10 seconds, the
# replica will not try to failover if it was not able to talk with the master
# for longer than 310 seconds.
#
# A large replica-validity-factor may allow replicas with too old data to failover
# a master, while a too small value may prevent the cluster from being able to
# elect a replica at all.
#
# For maximum availability, it is possible to set the replica-validity-factor
# to a value of 0, which means, that replicas will always try to failover the
# master regardless of the last time they interacted with the master.
# (However they'll always try to apply a delay proportional to their
# offset rank).
#
# Zero is the only value able to guarantee that when all the partitions heal
# the cluster will always be able to continue.
#
# cluster-replica-validity-factor 10

# Cluster replicas are able to migrate to orphaned masters, that are masters
# that are left without working replicas. This improves the cluster ability
# to resist to failures as otherwise an orphaned master can't be failed over
# in case of failure if it has no working replicas.
#
# Replicas migrate to orphaned masters only if there are still at least a
# given number of other working replicas for their old master. This number
# is the "migration barrier". A migration barrier of 1 means that a replica
# will migrate only if there is at least 1 other working replica for its master
# and so forth. It usually reflects the number of replicas you want for every
# master in your cluster.
#
# Default is 1 (replicas migrate only if their masters remain with at least
# one replica). To disable migration just set it to a very large value.
# A value of 0 can be set but is useful only for debugging and dangerous
# in production.
#
# cluster-migration-barrier 1

# By default Redis Cluster nodes stop accepting queries if they detect there
# is at least an hash slot uncovered (no available node is serving it).
# This way if the cluster is partially down (for example a range of hash slots
# are no longer covered) all the cluster becomes, eventually, unavailable.
# It automatically returns available as soon as all the slots are covered again.
#
# However sometimes you want the subset of the cluster which is working,
# to continue to accept queries for the part of the key space that is still
# covered. In order to do so, just set the cluster-require-full-coverage
# option to no.
#
# cluster-require-full-coverage yes

# This option, when set to yes, prevents replicas from trying to failover its
# master during master failures. However the master can still perform a
# manual failover, if forced to do so.
#
# This is useful in different scenarios, especially in the case of multiple
# data center operations, where we want one side to never be promoted if not
# in the case of a total DC failure.
#
# cluster-replica-no-failover no

# In order to setup your cluster make sure to read the documentation
# available at http://redis.io web site.

########################## CLUSTER DOCKER/NAT support  ########################

# In certain deployments, Redis Cluster nodes address discovery fails, because
# addresses are NAT-ted or because ports are forwarded (the typical case is
# Docker and other containers).
#
# In order to make Redis Cluster working in such environments, a static
# configuration where each node knows its public address is needed. The
# following two options are used for this scope, and are:
#
# * cluster-announce-ip
# * cluster-announce-port
# * cluster-announce-bus-port
#
# Each instruct the node about its address, client port, and cluster message
# bus port. The information is then published in the header of the bus packets
# so that other nodes will be able to correctly map the address of the node
# publishing the information.
#
# If the above options are not used, the normal Redis Cluster auto-detection
# will be used instead.
#
# Note that when remapped, the bus port may not be at the fixed offset of
# clients port + 10000, so you can specify any port and bus-port depending
# on how they get remapped. If the bus-port is not set, a fixed offset of
# 10000 will be used as usually.
#
# Example:
#
# cluster-announce-ip 10.1.1.5
# cluster-announce-port 6379
# cluster-announce-bus-port 6380

################################## SLOW LOG ###################################

# The Redis Slow Log is a system to log queries that exceeded a specified
# execution time. The execution time does not include the I/O operations
# like talking with the client, sending the reply and so forth,
# but just the time needed to actually execute the command (this is the only
# stage of command execution where the thread is blocked and can not serve
# other requests in the meantime).
#
# You can configure the slow log with two parameters: one tells Redis
# what is the execution time, in microseconds, to exceed in order for the
# command to get logged, and the other parameter is the length of the
# slow log. When a new command is logged the oldest one is removed from the
# queue of logged commands.

# The following time is expressed in microseconds, so 1000000 is equivalent
# to one second. Note that a negative number disables the slow log, while
# a value of zero forces the logging of every command.
slowlog-log-slower-than 10000

# There is no limit to this length. Just be aware that it will consume memory.
# You can reclaim memory used by the slow log with SLOWLOG RESET.
slowlog-max-len 128

################################ LATENCY MONITOR ##############################

# The Redis latency monitoring subsystem samples different operations
# at runtime in order to collect data related to possible sources of
# latency of a Redis instance.
#
# Via the LATENCY command this information is available to the user that can
# print graphs and obtain reports.
#
# The system only logs operations that were performed in a time equal or
# greater than the amount of milliseconds specified via the
# latency-monitor-threshold configuration directive. When its value is set
# to zero, the latency monitor is turned off.
#
# By default latency monitoring is disabled since it is mostly not needed
# if you don't have latency issues, and collecting data has a performance
# impact, that while very small, can be measured under big load. Latency
# monitoring can easily be enabled at runtime using the command
# "CONFIG SET latency-monitor-threshold <milliseconds>" if needed.
latency-monitor-threshold 0

############################# EVENT NOTIFICATION ##############################

# Redis can notify Pub/Sub clients about events happening in the key space.
# This feature is documented at http://redis.io/topics/notifications
#
# For instance if keyspace events notification is enabled, and a client
# performs a DEL operation on key "foo" stored in the Database 0, two
# messages will be published via Pub/Sub:
#
# PUBLISH __keyspace@0__:foo del
# PUBLISH __keyevent@0__:del foo
#
# It is possible to select the events that Redis will notify among a set
# of classes. Every class is identified by a single character:
#
#  K     Keyspace events, published with __keyspace@<db>__ prefix.
#  E     Keyevent events, published with __keyevent@<db>__ prefix.
#  g     Generic commands (non-type specific) like DEL, EXPIRE, RENAME, ...
#  $     String commands
#  l     List commands
#  s     Set commands
#  h     Hash commands
#  z     Sorted set commands
#  x     Expired events (events generated every time a key expires)
#  e     Evicted events (events generated when a key is evicted for maxmemory)
#  A     Alias for g$lshzxe, so that the "AKE" string means all the events.
#
#  The "notify-keyspace-events" takes as argument a string that is composed
#  of zero or multiple characters. The empty string means that notifications
#  are disabled.
#
#  Example: to enable list and generic events, from the point of view of the
#           event name, use:
#
#  notify-keyspace-events Elg
#
#  Example 2: to get the stream of the expired keys subscribing to channel
#             name __keyevent@0__:expired use:
#
#  notify-keyspace-events Ex
#
#  By default all notifications are disabled because most users don't need
#  this feature and the feature has some overhead. Note that if you don't
#  specify at least one of K or E, no events will be delivered.
notify-keyspace-events ""

############################### ADVANCED CONFIG ###############################

# Hashes are encoded using a memory efficient data structure when they have a
# small number of entries, and the biggest entry does not exceed a given
# threshold. These thresholds can be configured using the following directives.
hash-max-ziplist-entries 512
hash-max-ziplist-value 64

# Lists are also encoded in a special way to save a lot of space.
# The number of entries allowed per internal list node can be specified
# as a fixed maximum size or a maximum number of elements.
# For a fixed maximum size, use -5 through -1, meaning:
# -5: max size: 64 Kb  <-- not recommended for normal workloads
# -4: max size: 32 Kb  <-- not recommended
# -3: max size: 16 Kb  <-- probably not recommended
# -2: max size: 8 Kb   <-- good
# -1: max size: 4 Kb   <-- good
# Positive numbers mean store up to _exactly_ that number of elements
# per list node.
# The highest performing option is usually -2 (8 Kb size) or -1 (4 Kb size),
# but if your use case is unique, adjust the settings as necessary.
list-max-ziplist-size -2

# Lists may also be compressed.
# Compress depth is the number of quicklist ziplist nodes from *each* side of
# the list to *exclude* from compression.  The head and tail of the list
# are always uncompressed for fast push/pop operations.  Settings are:
# 0: disable all list compression
# 1: depth 1 means "don't start compressing until after 1 node into the list,
#    going from either the head or tail"
#    So: [head]->node->node->...->node->[tail]
#    [head], [tail] will always be uncompressed; inner nodes will compress.
# 2: [head]->[next]->node->node->...->node->[prev]->[tail]
#    2 here means: don't compress head or head->next or tail->prev or tail,
#    but compress all nodes between them.
# 3: [head]->[next]->[next]->node->node->...->node->[prev]->[prev]->[tail]
# etc.
list-compress-depth 0

# Sets have a special encoding in just one case: when a set is composed
# of just strings that happen to be integers in radix 10 in the range
# of 64 bit signed integers.
# The following configuration setting sets the limit in the size of the
# set in order to use this special memory saving encoding.
set-max-intset-entries 512

# Similarly to hashes and lists, sorted sets are also specially encoded in
# order to save a lot of space. This encoding is only used when the length and
# elements of a sorted set are below the following limits:
zset-max-ziplist-entries 128
zset-max-ziplist-value 64

# HyperLogLog sparse representation bytes limit. The limit includes the
# 16 bytes header. When an HyperLogLog using the sparse representation crosses
# this limit, it is converted into the dense representation.
#
# A value greater than 16000 is totally useless, since at that point the
# dense representation is more memory efficient.
#
# The suggested value is ~ 3000 in order to have the benefits of
# the space efficient encoding without slowing down too much PFADD,
# which is O(N) with the sparse encoding. The value can be raised to
# ~ 10000 when CPU is not a concern, but space is, and the data set is
# composed of many HyperLogLogs with cardinality in the 0 - 15000 range.
hll-sparse-max-bytes 3000

# Streams macro node max size / items. The stream data structure is a radix
# tree of big nodes that encode multiple items inside. Using this configuration
# it is possible to configure how big a single node can be in bytes, and the
# maximum number of items it may contain before switching to a new node when
# appending new stream entries. If any of the following settings are set to
# zero, the limit is ignored, so for instance it is possible to set just a
# max entires limit by setting max-bytes to 0 and max-entries to the desired
# value.
stream-node-max-bytes 4096
stream-node-max-entries 100

# Active rehashing uses 1 millisecond every 100 milliseconds of CPU time in
# order to help rehashing the main Redis hash table (the one mapping top-level
# keys to values). The hash table implementation Redis uses (see dict.c)
# performs a lazy rehashing: the more operation you run into a hash table
# that is rehashing, the more rehashing "steps" are performed, so if the
# server is idle the rehashing is never complete and some more memory is used
# by the hash table.
#
# The default is to use this millisecond 10 times every second in order to
# actively rehash the main dictionaries, freeing memory when possible.
#
# If unsure:
# use "activerehashing no" if you have hard latency requirements and it is
# not a good thing in your environment that Redis can reply from time to time
# to queries with 2 milliseconds delay.
#
# use "activerehashing yes" if you don't have such hard requirements but
# want to free memory asap when possible.
activerehashing yes

# The client output buffer limits can be used to force disconnection of clients
# that are not reading data from the server fast enough for some reason (a
# common reason is that a Pub/Sub client can't consume messages as fast as the
# publisher can produce them).
#
# The limit can be set differently for the three different classes of clients:
#
# normal -> normal clients including MONITOR clients
# replica  -> replica clients
# pubsub -> clients subscribed to at least one pubsub channel or pattern
#
# The syntax of every client-output-buffer-limit directive is the following:
#
# client-output-buffer-limit <class> <hard limit> <soft limit> <soft seconds>
#
# A client is immediately disconnected once the hard limit is reached, or if
# the soft limit is reached and remains reached for the specified number of
# seconds (continuously).
# So for instance if the hard limit is 32 megabytes and the soft limit is
# 16 megabytes / 10 seconds, the client will get disconnected immediately
# if the size of the output buffers reach 32 megabytes, but will also get
# disconnected if the client reaches 16 megabytes and continuously overcomes
# the limit for 10 seconds.
#
# By default normal clients are not limited because they don't receive data
# without asking (in a push way), but just after a request, so only
# asynchronous clients may create a scenario where data is requested faster
# than it can read.
#
# Instead there is a default limit for pubsub and replica clients, since
# subscribers and replicas receive data in a push fashion.
#
# Both the hard or the soft limit can be disabled by setting them to zero.
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit replica 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60

# Client query buffers accumulate new commands. They are limited to a fixed
# amount by default in order to avoid that a protocol desynchronization (for
# instance due to a bug in the client) will lead to unbound memory usage in
# the query buffer. However you can configure it here if you have very special
# needs, such us huge multi/exec requests or alike.
#
# client-query-buffer-limit 1gb

# In the Redis protocol, bulk requests, that are, elements representing single
# strings, are normally limited ot 512 mb. However you can change this limit
# here.
#
# proto-max-bulk-len 512mb

# Redis calls an internal function to perform many background tasks, like
# closing connections of clients in timeout, purging expired keys that are
# never requested, and so forth.
#
# Not all tasks are performed with the same frequency, but Redis checks for
# tasks to perform according to the specified "hz" value.
#
# By default "hz" is set to 10. Raising the value will use more CPU when
# Redis is idle, but at the same time will make Redis more responsive when
# there are many keys expiring at the same time, and timeouts may be
# handled with more precision.
#
# The range is between 1 and 500, however a value over 100 is usually not
# a good idea. Most users should use the default of 10 and raise this up to
# 100 only in environments where very low latency is required.
hz 10

# Normally it is useful to have an HZ value which is proportional to the
# number of clients connected. This is useful in order, for instance, to
# avoid too many clients are processed for each background task invocation
# in order to avoid latency spikes.
#
# Since the default HZ value by default is conservatively set to 10, Redis
# offers, and enables by default, the ability to use an adaptive HZ value
# which will temporary raise when there are many connected clients.
#
# When dynamic HZ is enabled, the actual configured HZ will be used as
# as a baseline, but multiples of the configured HZ value will be actually
# used as needed once more clients are connected. In this way an idle
# instance will use very little CPU time while a busy instance will be
# more responsive.
dynamic-hz yes

# When a child rewrites the AOF file, if the following option is enabled
# the file will be fsync-ed every 32 MB of data generated. This is useful
# in order to commit the file to the disk more incrementally and avoid
# big latency spikes.
aof-rewrite-incremental-fsync yes

# When redis saves RDB file, if the following option is enabled
# the file will be fsync-ed every 32 MB of data generated. This is useful
# in order to commit the file to the disk more incrementally and avoid
# big latency spikes.
rdb-save-incremental-fsync yes

# Redis LFU eviction (see maxmemory setting) can be tuned. However it is a good
# idea to start with the default settings and only change them after investigating
# how to improve the performances and how the keys LFU change over time, which
# is possible to inspect via the OBJECT FREQ command.
#
# There are two tunable parameters in the Redis LFU implementation: the
# counter logarithm factor and the counter decay time. It is important to
# understand what the two parameters mean before changing them.
#
# The LFU counter is just 8 bits per key, it's maximum value is 255, so Redis
# uses a probabilistic increment with logarithmic behavior. Given the value
# of the old counter, when a key is accessed, the counter is incremented in
# this way:
#
# 1. A random number R between 0 and 1 is extracted.
# 2. A probability P is calculated as 1/(old_value*lfu_log_factor+1).
# 3. The counter is incremented only if R < P.
#
# The default lfu-log-factor is 10. This is a table of how the frequency
# counter changes with a different number of accesses with different
# logarithmic factors:
#
# +--------+------------+------------+------------+------------+------------+
# | factor | 100 hits   | 1000 hits  | 100K hits  | 1M hits    | 10M hits   |
# +--------+------------+------------+------------+------------+------------+
# | 0      | 104        | 255        | 255        | 255        | 255        |
# +--------+------------+------------+------------+------------+------------+
# | 1      | 18         | 49         | 255        | 255        | 255        |
# +--------+------------+------------+------------+------------+------------+
# | 10     | 10         | 18         | 142        | 255        | 255        |
# +--------+------------+------------+------------+------------+------------+
# | 100    | 8          | 11         | 49         | 143        | 255        |
# +--------+------------+------------+------------+------------+------------+
#
# NOTE: The above table was obtained by running the following commands:
#
#   redis-benchmark -n 1000000 incr foo
#   redis-cli object freq foo
#
# NOTE 2: The counter initial value is 5 in order to give new objects a chance
# to accumulate hits.
#
# The counter decay time is the time, in minutes, that must elapse in order
# for the key counter to be divided by two (or decremented if it has a value
# less <= 10).
#
# The default value for the lfu-decay-time is 1. A Special value of 0 means to
# decay the counter every time it happens to be scanned.
#
# lfu-log-factor 10
# lfu-decay-time 1

################################## INCLUDES ###################################

# Include one or more other config files here.  This is useful if you
# have a standard template that goes to all Redis server but also need
# to customize a few per-server settings.  Include files can include
# other files, so use this wisely.
#
# include /path/to/local.conf
# include /path/to/other.conf

```







# redis的发布和订阅 

redis发布和订阅是一种消息的通信模式：发送者（pub）发送消息，订阅者（sub）接收消息。

redis客户端可以订阅任意数量的频道。



![image-20230105192158793](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105192158793.png)



**一个频道可以被多个客户端（订阅者）订阅**

![image-20230105191407420](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105191407420.png)



**订阅频道后，发布者通过该频道发送消息后，客户端可以接收到该消息**

![image-20230105191430279](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105191430279.png)







***发布订阅命令行实现***

![image-20230105192652527](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105192652527.png)

![image-20230105192740093](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105192740093.png)

![image-20230105192755718](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105192755718.png)



**![image-20230105192956178](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230105192956178.png)**







# redis事务操作

## 事务定义

redis事务是一个==单独的隔离操作==：**事务中的所有命令都会序列化、按顺序地执行**。**事务在执行过程中，不会被其他客户端发送来的命令请求打断**。

redis事务主要作用：==**串联多个命令防止别的命令插队**==



## Multi、Exec、discard

从输入Multi命令开始，输入的命令都会依次进入命令队列中，但是不会执行，直到输入Exec后，redis会将之前的命令队列中的命令依次执行。组队过程中，可以使用discard来放弃组队。

![image-20230107143634062](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230107143634062.png)



```go
redis 的 事务操作和mysql事务操是类似的

redis中的multi  ---- mysql中的start transaction
redis中的exec ---- mysql中的commit
redis中的discard----- mysql中的rollback
```



```go
127.0.0.1:6379> multi  //开启事务，组队阶段,相当于MySQL中的 start transaction
OK
127.0.0.1:6379> set name1 zhangsan
QUEUED
127.0.0.1:6379> set name2 lisi
QUEUED
127.0.0.1:6379> get name1
QUEUED
127.0.0.1:6379> exec  //执行阶段，exec结束后，即事务已经完成了，相当于mysql中的commit
1) OK
2) OK
3) "zhangsan"


127.0.0.1:6379> multi  //开启事务 ，相当于MySQL中的 start transaction
OK
127.0.0.1:6379> set k1 v1  //组队
QUEUED
127.0.0.1:6379> set k2 v2  //组队
QUEUED
127.0.0.1:6379> discard  //放弃组队，事务结束，相当于mysql中的rollback
OK
```





## 事务的错误处理

+ **==组队过程中==，==某个命令出现了报告错误==，执行时整个的所有队列都会被取消**。

  ![image-20230107144622544](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230107144622544.png)

  ```go
  127.0.0.1:6379> multi
  OK
  127.0.0.1:6379> set k1 v1
  QUEUED
  127.0.0.1:6379> set k2 v2
  QUEUED
  127.0.0.1:6379> set k3  //组队时，命令错误
  (error) ERR wrong number of arguments for 'set' command
  127.0.0.1:6379> exec   //执行后，发现：所有命令都不会被执行，事务终止了
  (error) EXECABORT Transaction discarded because of previous errors. 
  ```

  



+ 如果==**执行阶段**==某个命令报告出错，**则只有报错的命令不会被执行，而其他的命令都会执行**，不会回滚。

  ![image-20230107144800094](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230107144800094.png)

  ```go
  127.0.0.1:6379> multi  //组队
  OK
  127.0.0.1:6379> set k1 v1
  QUEUED
  127.0.0.1:6379> incr k1
  QUEUED
  127.0.0.1:6379> exec  //执行，组队后尽管中间存在命令报错，但是非报错命令都会执行
  1) OK  //第一条命令自行成功
  2) (error) ERR value is not an integer or out of range  //第二条命令报错
  ```

  





## 事务冲突

### 悲观锁

悲观锁：**指每次拿数据时都认为别人会修改，所以在每次拿到数据时会上**锁，此时，别人想拿到这个数据就会block直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在进行操作之前先上锁。



冲突比较多的时候, 使用悲观锁(没有乐观锁那么多次的尝试)对于每一次数据修改都要上锁，如果在DB读取需要比较大的情况下有线程在执行数据修改操作会导致读操作全部被挂载起来，等修改线程释放了锁才能读到数据，体验极差。所以比较**==适合用在DB写大于读==**的情况。

==**读取频繁使用乐观锁，写入频繁使用悲观锁。**==

### 乐观锁

乐观锁：每次拿数据时，都认为别人不会取修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有更新这个数据，可以使用版本号等机制。乐观锁适用于多读的应用类型，这样可以提高吞吐量。==**Redis就是利用乐观锁，这种check-and-set机制实现事务的。**==



冲突比较少的时候, 使用乐观锁(没有[悲观锁](https://so.csdn.net/so/search?q=悲观锁&spm=1001.2101.3001.7020)那样耗时的开销) 由于乐观锁的不上锁特性，所以在性能方面要比悲观锁好，**==比较适合用在DB的读大于写的业务==**场景。

**乐观锁的实现方式主要有两种，==一种是CAS（Compare and Swap，比较并交换）机制，一种是版本号机制。==**



### watch key [key ...]

**在执行multi之前，先执行`watch key1 [key2 ...] `，可以监视一个或多个key，如果在事务执行之前这些key被其他命令所改动，那么事务将被打断。**



![image-20230107153107479](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230107153107479.png)



**`unwatch`命令是用来取消所有key的监视**。如果在执行watch命令时发现，exec命令或discard命令先被执行，那么就不需要再执行unwatch命令了。



## redis事务三大特性

+ 单独的隔离操作
  + 事务中的所有命令都会序列化、按照顺序地执行。事务在执行过程中，不会被其他的客户端发送来的命令请求打断。
+ 没有隔离级别的概念
  + 队列中命令没有提交之前都不会被实际执行，因为事务提交前任何执行都不会被实际执行。
+ 不保证原子性
  + 事务中如果有一条命令执行失败，其后的命令仍然被执行，没有回滚。





# redis 持久化

redis提供了两种不同形式的持久化方式：

+ RDB（redis database）
+ AOF （Append Of File）



## RDB

### RDB是什么

RDB是在**指定的时间间隔内将内存中的数据集快照写入磁盘**，也就是snapshot快照，它恢复时是将快照文件直接读到内存里。



### RDB备份

redis会单独创建（fork）一个子进程来进行持久化，会将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能，**如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感**，那么RDB方式要比AOF方式更加的高效。**RDB的缺点：最后一次持久化后的数据可能丢失，因此持久化操作需要一定的时间间隔。**



### Fork

+ Fork的作用是复制一个与当前进程一样的进程。新进程的所有数据（变量、环境变量、程序计数器等）数值和原进程一致，但是是一个全新的进程，并作为原进程的子进程。
+ 在linux程序中，fork()会产生一个和父进程完全相同的子进程，但是子进程在此之前会exec系统调用，出于效率考虑，linux中引入“写时复制技术”
+ 在一般情况下，父进程和子进程会共用同一段物理内存，只有进程空间的各段的内容要发生变化时，才会将父进程的内容复制一份给子进程。



### RDB持久化流程

![image-20230107173925411](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230107173925411.png)



### dump.rdb文件

在linux系统中，在redis.conf中配置文件名称，默认为dump.rdb



### rdb优点和缺点

#### 优点

+ 适合大规模的数据恢复
+ 对数据完整性和一致性要求不高，更适合使用
+ 节省磁盘空间
+ 恢复速度块



#### 缺点

+ fork的时候，内存中的数据被克隆了一份，大致2倍的膨胀需要考虑
+ 虽然redis在fork时使用了写时拷贝复制技术，但是如果数据庞大时还是比较消耗性能
+ 在备份周期在一定时间间隔做一次备份，所以如果redis意外down掉的话，就会丢失最后一次快照后的所有修改。



## AOF

### AOF是什么

AOF：以日志的形式来记录每个写操作（增量保存），**将redis执行过的所有==写指令==记录下来（读操作不记录）**，只追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，**redis重启的话会根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作**。



### AOF持久化过程

（1）客户端的请求写命令会被appned追加到AOF缓冲区内。

（2）AOF缓冲区根据AOF持久化策略[always, everysec, no] 将操作sync同步到磁盘的AOF文件中

（3）AOF文件大小超过重写策略或手动重写时，会对AOF文件rewrite重写，压缩AOF文件容量。

（4）Redis服务启动时，会重新load（加载）AOF文件中的写操作达到数据恢复的目的。



### AOF默认不开启

可以在redis.conf（linux系统）中的配置文件名称，默认为appendonly.aof

AOF文件的保存路径，同rdb的路径一致



==**注意：AOF和RDB同时开启后，系统默认取AOF的数据（数据不会存在丢失）。**==





### AOF同步频率设置

+ `appendfsync always`：始终同步，每次redis的写入都会立刻记录日志；性能较差但是数据完整性较好
+ `appendfsync everysec`：每秒同步，每秒记录日志一次，如果出现宕机，最后一秒的数据可能存在丢失。
+ `appendfsync no`：redis不主动进行同步，把同步时机交给操作系统。



### rewrite压缩

AOF采用文件追加方式，文件只会越来越大，为了避免出现此种情况，新增了重写机制，当AOF文件的大小超过所设定的阈值时，redis会启动AOF文件的内容压缩，只保留可以恢复数据的最小指令集，可以使用命令`bgrewriteaof`



重写原理：AOF文件持续增长，会fork出一条新进程来将文件重写（也是先写临时文件最后再rename），redis4.0 版本的重写，是把rdb的快照，以二进制形式附加在新的aof头部，作为已有的历史数据，替换掉原来的流水操作。

redis会记录上次重写时AOF大小，默认配置是当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发。（每次重写文件大小必须为64M倍数）。



### AOF优点和缺点

#### 优点

+ 备份机制更稳健，丢失数据概率更低
+ 可读的日志文件，通过操作AOF文件，可以处理误操作



#### 缺点

+ 比起RDB占用更多的磁盘空间
+ 恢复备份速度要慢
+ 每次读写都同步的话，有一定的性能压力





# redis主从复制

主从复制（一主多从复制）：主机数据更新后根据配置和策略，自动同步到备机的master/slave机制**，Master以写为主，Slave以读为主。**



主从复制作用：

+ 读写分离，性能扩展
+ 容灾快速恢复
+ ![image-20230108170308482](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230108170308482.png)



## 主从复制

+ Slave启动成功连接到master后发送一个sync命令
+ master接到命令启动后台的磁盘进程，同时收集所有接收到的用于修改的数据集命令，在后台进程执行完毕之后，master将传送整个数据文件到slave，以完成一次完全同步
+ 全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中
+ 增量复制：master继续将新的所有收集到修改的命令一次传给slave，完成同步
+ 但是只要是重新连接master，一次完全同步（全量复制）将自动执行。



![image-20230108175903824](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230108175903824.png)

**将其中一个redis作为某个redis的从服务器命令： `slaveof  主服务器地址 端口号 `**





## 薪火相传

上一个slave可以是下一个slave的master，slave同样可以接收其他slaves的连接和同步请求，将该slave作为下一个slave的master，可以有效减轻master的写压力，去中心化，降低风险。



![image-20230108181110120](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230108181110120.png)



## 反客为主

当一个master宕机后，后面的slave可以立刻升为master，其后面的slave不用做任何修改。

**`slaveof no one` 命令可以将从机变为主机**



## 哨兵模式

**哨兵模式：即为反客为主的自动版，能够后台监视主机是否故障**，如果故障了根据投票数自动将从库转换为主库。



![image-20230108181742660](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230108181742660.png)





![image-20230108185253843](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230108185253843.png)



# redis集群

针对redis容量不足，redis如何扩容，并发写操作时，redis如何分摊？可以通过redis集群来解决。



对于主从模式，薪火相传模式，主机宕机，导致ip地址发生变化，应用程序中的配置需要修改对应的主机地址，端口等信息，这样是十分不方便的。

以前，通过采用**代理主机方式**来解决：即引入一个代理服务器，代理服务器根据客户端发送来的请求，来将该请求分配到对应的服务器中。



![image-20230111152701770](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230111152701770.png)



在redis3.0中，提供了一种**无中心化集群配置**：**即任何一台服务器均可以作为代理服务器来使用，**任何一台服务器都会查看客户端发送来的请求是否是自己的，如果不是，会将该请求发送给目标服务器。



![image-20230111152906341](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230111152906341.png)





## 集群定义

redis集群实现了对redis的水平扩容，即启动N个redis节点，将整个数据库分布存储在这N个节点上，每个节点存储总数居的1/N



redis集群通过分区（partition）来提供一定程度的可用性（availability）：即使集群中有一部分节点失效或者无法进行通讯，集群也可以继续处理命令请求。





## 删除持久化数据

将`rdb.aof`文件都删除。





## redis cluster 如何分配节点

**一个集群中至少有三个主节点**，选项`--cluster-replicas 1`表示：我们希望为集群中的每个主节点创建一个从节点

**分配原则：尽量保证每个主数据库运行在不同的IP地址，每个从库和主库不在一个IP地址上**。





# redis应用问题解决

## 缓存穿透

### 问题描述

key对应的数据在数据源中并不存在，每次针对此key的请求从缓存中获取不到，请求都会压到数据源，从而可能压垮数据源。比如用一个不存在的用户id获取用户信息，不论缓存还是数据库中都没有，若黑客利用此漏洞进行攻击可能压垮数据库。



![image-20230111170549065](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230111170549065.png)

### 解决方案

+ 对空值缓存：如何一个查询返回的数据为空，我们仍然把这个结果（null）进行缓存，设置空结果的过期时间，最长不超过5分钟。
+ 设置可访问名单（白名单）：使用bitmaps类型定义一个可以访问的名单，名单的id作为bitmaps的偏移量，每次访问和bitmaps里面的id进行比较，如果访问id不在bitmaps中，进行拦截和不允许访问。
+ 采用布隆过滤器：在1970年由布隆提出的，实际上是一个很长的二进制向量（位图）和一系列随机映射函数（哈希函数）。布隆过滤器可以用于检索一个元素是否在一个集合中。优点：空间效率和查询时间远远超过一般算法。缺点：有一定的识别错误率和删除困难。
+ 进行实时监控：当发现redis的命中率开始急速降低，需要排查访问对象和访问的数据，和运维人员配合，可以设置黑名单限制服务。





## 缓存击穿

### 问题描述

key对应的数据存在，但是在redis中过期了，此时若由大量并发请求过来，这些请求发现缓存过期一般都会从后端数据库中加载数据并回设到缓存中，这个时候大并发的请求可能瞬间把后端数据库压垮。



![image-20230111193200257](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230111193200257.png)



### 解决方案

key可能在某些时间点被超高并发地访问，是一种非常“热点”地数据。这个时候需要考虑一个问题：缓存击穿。

**解决方法**：

+ 预先设置热门数据：在redis高峰访问之前，把一些热门数据提前存入到redis中，加大这些热门数据key的时长。
+ 实时调正：现场监控哪些数据热门，实时调正key的过期时长。
+ 使用锁：
  + 就是在缓存失效的时候（判断拿出来的值是否为空），不是立即去load db。
  + 先使用缓存工具的某些带成功操作返回值的操作（如redis中的setnx）







## 缓存雪崩

### 问题描述

key对应的数据存在，但是在redis中过期，此时若有大量的并发请求过来，这些请求发现缓存过期一般都会在后端数据库中加载数据并回设到缓存中，这个时候大并发的请求可能回瞬间把后端数据压垮。

==缓存雪崩与缓存击穿的区别：缓存雪崩针对很多key的缓存，而缓存击穿则是某一个key的访问==

![image-20230112000718785](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230112000718785.png)



### 解决方案

缓存失效的雪崩效应对底层胸痛的冲击非常可怕。

**解决方法**：

+ 构建多级缓存架构：nginx缓存+redis缓存+其他缓存（ehcache等）
+ 使用锁队列：加锁或者队列的方式来保证不会有大量的线程对数据库一次性进行读写，从而避免失效时大量的并发请求落到底层存储系统上。不适用高并发情况。
+ 设置过期标志更新缓存：记录缓存数据是否过期（设置提前量），如果过期回触发通知另外的线程在后台去更新实际key的缓存。
+ 将缓存失效时间分散开：比如可以在原有的失效时间基础上增加一个随机值，比如1-5分钟随机，这样每一个缓存的过期时间的重复率就会降低，很难引发集体失效的事件。





## 分布式锁

### 问题描述

随着业务发展的需要，原单体单机部署的系统被演化成分布式集群系统后，由于分布式系统多线程、多进程并且分布在不同的机器上，这将使原单机部署情况下的并发控制锁策略失效，单纯的java API 并不能提供分布式锁的能力。为了解决这个问题就需要一种跨JVM的互斥机制来控制共享资源的访问，这就是分布式锁的重要问题。

**分布式锁主流的实现方案**

+ 基于数据库实现分布式锁
+ 基于缓存（redis等）
+ 基于Zookeeper



每一种分布式锁解决方案都有各自的优缺点：

+ 性能：redis最高
+ 可靠性：zookeeper 最高





### 解决方案

**使用redis实现分布式锁**

```go
//第一种方式，手动解锁
setnx user 10  //这表示上锁了 
del user //只有解锁后，才能继续赋值

//第二种方式，设置锁的过期时间，自动解锁
setnx user 10
expire user 12 //设置过期时间为12秒
...12秒之后，可以继续赋值

//第三种方式，在上锁的同时设置过期时间，实现原子操作
set user 10 nx ex 12 //nx 表示上锁，ex 12 表示过期时间12秒
```



### 误删锁

`uuid`来标识锁，在释放锁的时候，会对用户的`uuid`和锁的`uuid`进行比较，只有对应的用户才能释放锁。



### LUA保证删除原子性

#### 问题

![image-20230112170616420](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230112170616420.png)



#### 解决方案

使用`lua`脚本，`lua`脚本执行过程是一个原子操作。



### 分布式锁可用的条件

+ 互斥性：在任意时刻，只有一个客户端能持有锁
+ 不会发生死锁：即使有一个客户端在持有锁的期间崩溃而没有主动解锁，也能保证后续其他客户端能加锁
+ 解铃还须系铃人：加锁和解锁必须是同一个客户端，客户端自己不能把别人加的锁给解了
+ 加锁和解锁必须具有原子性













