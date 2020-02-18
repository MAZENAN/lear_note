- [1. PHP引用变量考察点](#yybl)
- [2. 常量及数据类型考察点](#cl)
- [3. 运算符知识点考察](#ysf)
- [4. 流程控制考察点](#lckz)
- [5. 自定义函数及内部函数考察点](#hs)
- [6. 正则表达式考察知识点](#zz)
- [7. 文件及目录处理相关考点](#file)
- [8. 会话控制技术](#hh)
- [9. 面向对象考点](#oop)
- [10. 网络协议考察点](#net)
- [11. 开发环境及配置相关考点](#env)
# <a id="yybl">PHP引用变量考察点</a>
## 1. 变量的COW(copy on write)机制  
    <?php
    $a = 10;
    var_dump(memory_get_usage());
    $b = $a;
    var_dump(memory_get_usage());
    $b = 20;
    var_dump(memory_get_usage());  

![cow](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/cow.png)  
## 2. 引用变量的工作原理  
    <?php
    $a = 10;
    var_dump(memory_get_usage());
    $b = &$a;
    var_dump(memory_get_usage());
    $b = 20;
    var_dump(memory_get_usage());  
![引用变量图解](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/yy.png)   
    

## 3. zval变量容器，分析变量工作原理  
![zval1](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/zval1.png)  

![zval2](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/zval2.png)  
  
![zval3](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/zval3.png)   

![zval4](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/zval4.png)   
## 4. 引用变量需要注意的点:unset()  
### (1) unset只会取消引用，不会销毁空间。  

    <?php
    $a = 10;
    var_dump(memory_get_usage());
    $b = &$a;
    var_dump(memory_get_usage());
    $b = 20;
    var_dump(memory_get_usage());
    unset($a);
    var_dump(memory_get_usage());   

![unset1](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/unset1.png)  



### (2) 对象本身就是引用传递，不用加&  
### (3) 真题
  
    <?php
    //写出如下程序的输出结果
    //程序运行时，每一次循环结束后变量$data的值是什么？请解释？
    //执行完的结果是什么？请解释？

    $data = ['a', 'b', 'c'];
    foreach ($data as $key => $value) {
        $value = &$data[$key];
    }

    var_dump($data);  

![zval4](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/zval4.png)  

# <a id="cl">常量及数据类型考察点</a> 
## （1） PHP字符串的定义方式以及各自的区别：  
__定义方式：__  
单引号  
双引号  
heredoc和newdoc  

__区别:__   
单引号不能解析变量；  
单引号不能解析转义字符，只能解析单引号和反斜线本身。  
变量和变量、变量和字符串、字符串和字符串之间可以用.连接。 

双引号可以解析变量，变量可以使用特殊字符和{}包含  
双引号可以解析所有转义字符  
也可以使用.来连接  

单引号效率更高。  
 
双引号内变量用{}包含会被解析，并且`不会返回{}`;双引号内变量用特殊字符包含会被解析，并且`会返回`特殊字符。  
例如： 
 
    <?php
    $name = 'zhangsan';
    $sql  = "select *  from user where name = '$name'";
    $sql  = 'select * from user where name = \'' . $name . '\''; //效率更高写法

heredoc类似于双引号；  
newdoc类似与单引号。  

## (2) 数据类型：8大数据类型（标量、复合、特殊）  
__浮点类型：__  
浮点类型不能用到比较运算中。  

__布尔类型：__  
FALSE的7种情况：0，0.0，‘’，‘0’，false,array(),NULL;  

__数组类型：__  
超全局数组：  


    $GLOBALS、$_GET、$_POST、$_REQUEST、$_SESSION、$_COOKIE、$_SERVER、$_FILES、$_ENV; 

 __NULL__：  
三种情况： 
直接赋值为null、未定义的变量、unset销毁的变量；  

__常量：__  
定义：const、define  
const更快，是语言结构。define是函数  
define不能用于定义类的常量，const可以  
常量一经定义，不能被修改，不能被删除。  

__预定义常量：__  
__FILE__、__LINE__、__DIR__、__FUNCTION__、__CLASS__、__TRAIT__、__METHOD__、__NAMESPACE__  

 

# <a id="ysf">运算符知识点考察</a>  
## (1) PHP的错误抑制符：  
PHP支持一个错误运算符：`@`。当将其放置在一个PHP表达式之前，该表达式可能产生的`任何错误`信息都被`忽略掉`。  
## (2) PHP运算符的优先级：  
![运算符优先级官方图](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/ysfyxj1.png)  

![运算符优先级常考](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/ysfyxj2.png)  

括号的使用可以增加代码的可读性，推荐使用。  
## (3) 比较运算符  
==和===:  
等值判断（FALSE的七种情况）  
## (4) 递增递减运算符  
递增递减运算符不影响布尔值。  
递减NULL没有效果  
递增NULL值为1  
递增和递减在前就先运算后返回，反之就先返回，后运算。  

 

## (5) 逻辑运算符  
短路作用  
||和&&与or和and的优先级不同  

![运算符优先级例题1](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/ysfyxjlt1.png)   

解题方法：  
重点记忆递增/递减运算符的运算问题，逻辑运算符的短路效果，在看到逻辑运算符要多考虑优先级问题。  

## (6) 真题： 
//输出打印的结果：  
 
    <?php
    $a = 0;
    $b = 0;

    if ($a = 3 > 0 || $b = 3 > 0) {
        $a++;
        $b++;
        echo $a . "\n";
        echo $b . "\n";
    }  
    //结果 1,1  


# <a id="lckz">流程控制考察点</a>  
## (1) PHP遍历数组的三种方式，以及各自的区别：  
使用for循环：  
使用foreach:  
使用while、list()、each()组合循环  

    for循环只能遍历索引数组、foreach可以遍历索引和关联数组，联合使用list()、each()和while()循环可以遍历索引和关联数组  

    while、list()、each()组合不会reset()  
    foreach遍历会对数组进行reset()操作  
  
## (2) PHP分支考点  
if....elseif  
    在elseif语句中只能有一个表达式为true，即在elseif语句中z只能有一个语句被执行，多个elseif从句是排斥关系。  
    使用elseif语句有一个基本原则，总把优先范围小的条件放在前面处理。  


switch...case  

    和if不同的是，switch后面的控制表达式额数据类型只能是整形、浮点型、或者字符串。  

	continue语句作用到switch的作用类似于break跳出switch外的循环，可以使用continue2  
    switch...case会生成跳转表，直接跳转到对应的case  

## (3) 效率：  
如果条件比一个简单的比较要复杂的多或者在一个很多次的循环中，那么用switch语句可能会快一些。  

## (4) 真题：  
PHP中如何优化多个if...else...的情况？  
    把可能性较大的条件往前移；如果判断的较复杂且数据类型为整形、浮点型、字符串，可以用switch...case...  

# <a id="hs">自定义函数及内部函数考察点</a>  
## (1) 例题1  

![函数例题1](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/hanshu1.png) 
 
## (2) 变量的作用域： 
 
变量的作用域也称为变量的范围，变量的范围即它定义的上下文背景（也是它生效的范围）。大部分的PHP变量只有一个单独的范围。这个单独的范围跨度同样包含了include和require引入的文件。
  
    global关键字  
    $GLOBALS以及其他超全局数组  

## (3) 静态变量  
静态变量仅在局部函数域中存在，但当程序执行离开此作用域时，其值并不会消失。  
__static关键字：__  

    仅初始化一次  
    初始化时需要赋值  
    每次执行函数值会保留  
    static修饰的变量是局部的，仅在函数内有效  
    可以记录函数的调用次数，从而可以在某些条件下终止递归  

## (4) 函数的参数  
    默认情况下，函数参数通过值传递  
    如果希望允许函数修改它的值，必须通过引用传递参数。  

![函数传参](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/hanshu2.png)   

## (5) 函数的返回值  
值通过使用可选的返回语句(return)返回  
可以返回包括数组和对象的任意类型  
返回语句会终止函数执行，将控制权交回函数调用处  
省略return,返回值为NULL，不可有多个返回值  
## (6) 函数的引用返回  
从函数返回一个引用，必须在函数声明和指派返回值给一个变量时都使用引用运算符&  


![函数的引用返回](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/yinyong.png)   

## (7) 外部文件的导入  
include/require语句包含并运行指定文件  
如果给出路径名按照路径来找，否则从include_path中查找  
如果include_path中也没有，则从脚本文件所在的目录和当前工作目录下寻找。  
当一个文件被包含时，其中所包含的代码继承了include所在行的变量范围  

    加载过程中未找到文件则include结构会发出一条警告；这一点和require不同，后者会发出一个致命错误。  

    require在出错时，产生E_COMPILE_ERROR级别的错误。换句话说将导致脚本终止而include只产生警告(E_WARNING),脚本会继续运行。  
	
    require(include)/require_once(include_once)唯一区别是PHP会检查该文件是否已经被包过，如果是则不会再次包含。  
## (8) 系统内置函数  
时间日期函数 
 
    date()、strtotime()、mktime()、time()、microtime()、date_default_timezone_set()  

IP处理函数  

    ip2long()、long2ip()  

打印处理  

    print()、printf()、print_r()、echo、sprintf()、var_dump()、var_export()  

序列化和反序列化函数  

    serialize()、unserialize()  

字符串处理函数  

    implode()、explode()、join()、strrev()、trim()、ltrim()、rtrim()、strstr()、number_format()...  
 
数组处理函数  

    array_keys()、array_values()、array_diff()、array_intersect()、array_merge()、array_shift()、array_unshift()、array_pop()、array_push()、sort() 等  

# <a id="file">文件及目录处理相关考点</a>  
## (1) 文件读取/写入操作  
  
__*fopen()函数*__  
    
    用来打开一个文件，打开时需要指定打开模式  
    打开模式：r/r+、w/w+、a/a+、x/x+、b、t  

__*读取函数*__  
 
    fread()  
    fgets()  
    fgetc()  

__*关闭文件函数*__  

    fclose()  

__*不需要fopen()打开的函数：*__  

    file_get_contents()  
    file_put_contents()  

__*其他读取函数：*__  
    
    file()  
    readfile()  

__*访问远程文件*__:  

    开启allow_url_fopen,HTTP协议连接只能使用只读、FTP协议可以使用只读或者只写。  

__*目录操作函数：*__  

名称相关  
    
    basename()、dirname()、pathinfo()  

目录读取  

    opendir()、readdir()、closedir()、rewinddir()  

目录删除  

    rmdir()  

目录创建  

    mkdir()  

文件大小  

    filesize()  

目录大小  
   
    disk_free_space()、disk_total_space()  

文件拷贝  
    
    copy()  

删除文件  

    unlink()  

文件类型  

    filetype  

重命名文件或者目录：  
    
    rename()  

文件截取:  

    ftruncate()  

文件属性：  

    file_exists()、is_readable()、is_writeable()、is_executable()、filectime()、fileatime()、	filemtime()  

文件锁：  

    flock()  

文件指针：  

    ftell()、fseek()、rewind()      

# <a id="zz">正则表达式考察点</a>  

## (1) 正则表达式的作用  

分割、查找、匹配、替换字符串。  

## (2) 分隔符  

正斜线(/)、hash符号(#)、以及取反符号(~)。  

## (3) 通用原子  

    \d、\D、\w、\W、\s、\S    
## (4) 元字符  

    . * ? ^ $ + {n} {n,} {n,m} [] () [^] | [-]  
## (5) 模式修正符  

    i m e s U x A D u  

## (6) 后向引用  
    
    <?php
    $subject = '<h1>一级标题</h1>';
    $pattern = '#<h1>(.*)</h1>#';
    echo preg_replace($pattern, '\\1', $subject);  

## (7) 贪婪模式  
在修饰匹配次数的特殊符号后再加上一个 "?" 号  
通过模式修饰符来实现，大写"U"  

![取消贪婪模式](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/zz1.png)  
 
## (8) 正则表达式PCRE函数：  

    preg_match()、preg_match_all()、preg_replace()、preg_split()  

## (9) 中文匹配：  
    UTF-8汉字编码范围是`0x4e00-0x9fa5`，在ANSI(gb2312)环境下，`0xb0-0xf7`,`0xa1-0xfe`。 

UTF-8要使用u模式修正符使模式字符串被当成UTF-8，在ANSI(gb2312)环境下，要使用chr将Ascii码换为字符  
 
![中文匹配](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/zwpp.png)  

## (10) 

# <a id="hh">会话控制技术</a>  
## (1) 为什么使用会话控制技术？  
Web是通过http协议来实现的，http是无状态协议，没有办法来维护两个事务之间的状态，会把用户连续的请求当作是独立的请求，没有状态的保持，会话控制技术就是为了解决这个问题。  

## (2) 会话控制实现的方式：  
➀  通过GET参数传	递：问题：信息不安全；参数丢失等。  
➁ Cookie技术：信息存储在客户端。
cookie的操作：  
 写操作：  `bool setcookie( string $name[, string $value = ""[, int $expire = 0[, string $path = ""[, string $domain = ""[, bool $secure = false[, bool $httponly = false]]]]]] )`
 读操作:$_COOKIE  
 优缺点：存储在客户端不会占用服务端资源；缺点：存储在客户端不宜保存敏感数据，而且用户有权禁用cookie。  

➂ Session技术：将使用者的信息存在服务端，无法被禁用。session是基于cookie的（cookie被禁用可以通过传递url的session_id来使用）。  
session的操作:  
session_start();  
$_SESSION;  
 
__*删除session:*__   
$_SESSSION = [];  
session_destroy();  
set_cookie();..

__*session的配置：*__  
session.auto_start  
session_cookie_domain  
session.cookie_lifetime  
session.cookie_path  
session.name  
session.save_path  
session.use_cookies  
session.use_trans_sid  

session.gc_probability  
session.gc_divisor  
session.gc_maxlifetime  

session.save_handler  

__session的优缺点：__  
存储在服务端，安全；占用服务器资源。  

__禁用cookie，使用session__:  
通过在url中传递session_id的值。  

__session的存储：__   
当使用负载均衡时:  
使用session_set_save_handler()  
MySQL、Memcache、Redis等。    

![session](https://github.com/MAZENAN/lear_note/blob/master/PHP%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95%E8%80%83%E7%82%B9/img/session.png)   
 
__session信息的存储方式，如何进行遍历？__  
文件、memcache....等等；遍历:$_SESSION  



# <a id="oop">面向对象考点</a>  
## (1) PHP类权限控制修饰符。  
## (2) 面向对象的封装  
## (3) 继承 
单一继承  
方法重写 
## (4) 多态  
## (5) 魔术方法  
    __construct()、__destruct()、__call()、__callStatic()、__get()、__set()、__isset()、__unset()、__sleep()、__wakeup、__toString()、__clone()
## (6) 设计模式  
 常见设计模式/

# <a id="net">网络协议考察点</a>  
## (1) http协议状态码  
## (2) OSI七层模型  
## (3) HTTP协议的工作特点和工作原理  
## (4) HTTP协议常见请求/响应头  
## (5) HTTP协议的请求方法  
## (6) HTTPS的工作原理  
HTTPS是一种基于SSL/TSL的HTTP协议，所有的HTTP数据都是在SSL/TLS协议封装之上传输的。  
HTTPS协议在HTTP协议的基础之上，添加了SSL/TLS握手以及加密传输，也属于应用层协议。  
## （7）常见网络协议以及端口。  

# <a id="env">开发环境及配置相关考点</a>  
## (1) 版本控制软件。  
集中式和分布式。   
集中式：CVS、SVN。  
分布式：GIT。  
## (2) PHP的运行原理  
### Nginx+PHP-FPM  
相关概念：  
__CGI__: 早期WebServer只能处理简单的HTML静态文件，随着技术的发展出现了动态语言比如PHP、Python，比如PHP处理完应该怎么和WebServer进行通信，所以就出现了CGI,只要按照CGI要求进行就能实现，是PHP和WebServer进行通信的桥梁。  
__FastCGI__:  CGI的改良版本，CGI程序处理完后不会kill掉进程，提升了处理的效率。  
__PHP-FPM__:  是FastCGI的实现，并且提供了进程管理的功能。  Master进程和Worker进程，Master进程监听端口来自WebServer的请求，Worker进程一般会有多个，每个进程嵌入一个PHP解析器处理。  
## (3) PHP常见配置项。  
