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

# <a id="cl">常量及数据类型考察点</a>
# <a id="ysf">运算符知识点考察</a>
# <a id="hs">自定义函数及内部函数考察点</a>
# <a id="zz">正则表达式考察知识点</a>
# <a id="file">文件及目录处理相关考点</a>
# <a id="hh">会话控制技术</a>
# <a id="oop">面向对象考点</a>
# <a id="net">网络协议考察点</a>
# <a id="env">开发环境及配置相关考点</a>
