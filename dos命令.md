# dos窗口常用命令

## 修改dos窗口编码格式

```go
//utf-8格式：
chcp 65001

//gbk格式
chcp 936

//查看当前dos窗口的编码格式：
打开dos窗口，右键点击属性-->当前代码页
```

##  切换盘

```go
D:\>c: //切换盘

C:\Users\18390>cd /d D:\software   //切换到某盘下的某个目录

```

==**tab键可以自动补全目录或文件名**==





## 删除

```go
//删除文件 ----只能删除文件，而不能删除文件夹
到文件路径下，输入 del 文件名 

//删除空文件夹 ------ 只能删除空文件夹
到文件夹路径下，输入 rm 文件夹名

//删除文件夹，且文件夹中有内容 ----- 会将文件夹和里面的文件一起删除
// /s 表示：除目录本身外，还将删除指定目录下的所有子目录和文件。用于删除目录树
// /q 表示：安静模式，带 /s 删除目录树时不需要确认
到文件夹路径下，输入 rd /s /q 文件夹名 
```







# goland常用命令

## 代码收缩

```go
ctrl + "-"  //收缩代码块

ctrl+shift+“-”  //收缩所有代码块

ctrl+“+”   //展开代码块

ctrl+shift+“+”    //展开文件中所有代码块
```



## 替换

```go
ctrl + r  //打开替换
```



## 移动代码

```go
shift + alt +up/down  //将光标所在行的代码上下移动
```

