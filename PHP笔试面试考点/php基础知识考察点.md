- [1. PHP引用变量考察点](#yybl)
- [2. 常量及数据类型考察点](#cl)
- [3. 运算符知识点考察](#ysf)
- [4. 自定义函数及内部函数考察点](#hs)
- [5. 正则表达式考察知识点](#zz)
- [6. 文件及目录处理相关考点](#file)
- [7. 会话控制技术](#hh)
- [8. 面向对象考点](#oop)
- [9. 网络协议考察点](#net)
- [10. 开发环境及配置相关考点](#env)
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
# <a id="hs">自定义函数及内部函数考察点</a>
# <a id="zz">正则表达式考察知识点</a>
# <a id="file">文件及目录处理相关考点</a>
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
