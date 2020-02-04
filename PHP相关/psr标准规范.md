- [简介](#jianjie)
- [psr0](#psr0)
- [psr1](#psr1)
- [psr2](#psr2)
- [psr3](#psr3)
- [psr4](#psr4)
# <a id="jianjie">关于psr标准规范</a> 
* 简介  
    >     PHP FIG
    >     PSR 是 PHP Standard Recommendations （PHP 推荐标准）的简写，由 PHP FIG 组织制定的 PHP 规范，是 PHP 开发的实践标准。
    >     PHP FIG，FIG 是 Framework Interoperability Group（框架可互用性小组）的缩写，由几位开源框架的开发者成立于 2009 年，从那开始也选取了很多其他成员进来（包括但不限于 Laravel, Joomla, Drupal, Composer, Phalcon, Slim, Symfony, Zend Framework 等），虽然不是「官方」组织，但也代表了大部分的 PHP 社区。
    >     项目的目的在于：通过框架作者或者框架的代表之间讨论，以最低程度的限制，制定一个协作标准，各个框架遵循统一的编码规范，避免各家自行发展的风格阻碍了 PHP 的发展，解决这个程序设计师由来已久的困扰。
    >     目前已表决通过了不少套标准，已经得到大部分 PHP 框架的支持和认可。
    >     本项目的主要面向对象是所有参与的各个成员（也就是各自框架的社区），这里是完整的 成员列表，当然，同时也欢迎其它 PHP 社区采用本规范。
***

# <a id="psr0">PSR-0 自动加载规范（已弃用）</a> 
# <a id="psr1">PSR-1 基础编码规范</a> 
1. __文件__  
    - __PHP 标签__
    >     PHP 代码 必须 使用 <?php ?> 长标签 或 <?= ?> 短输出标签；一定不可 使用其它自定义标签。
    - __字符集编码__  
    >     PHP 代码 必须 且只可使用 不带 BOM 的 UTF-8 编码。
    -  __副作用__  
    >     一份 PHP 文件中 应该 要不就只定义新的声明，如类、函数或常量等不产生 副作用 的操作，要不就只书写会产生 副作用 的逻辑操作，但 不该 同时具有两者。
    >     //反例
    >     <?php
    >     // 「副作用」：修改 ini 配置
    >     ini_set('error_reporting', E_ALL);
    >     // 「副作用」：引入文件
    >     include "file.php";
    >     // 「副作用」：生成输出
    >     echo "<html>\n";
    >     // 声明函数
    >     function foo()
    >     {
    >        // function body
    >     }
    >     //正例
    >     <?php
    >     // 声明函数
    >     function foo()
    >     {
    >        // 函数主体部分
    >     }
    >     // 条件声明 **不** 属于「副作用」
    >     if (! function_exists('bar')) {
    >        function bar()
    >        {
    >           // 函数主体部分
    >       }
    >     }
2. __命名空间和类名__
    >     命名空间和类名 必须 遵循『自动加载』规范： [PSR-0， PSR-4]。
    >     这意味着每个类都独立为一个文件，并且至少在一个层次的命名空间内，那就是：顶级组织名（vendor name）。
    >     类名 必须 以类似 StudlyCaps 形式的大写开头的驼峰命名方式声明。
    >     <?php
    >     // PHP 5.3 及更高版本：
    >     namespace Vendor\Model;
    >     class Foo
    >     {
    >     }
3. __类的常量、属性和方法__
    - 常量
    >     类的常量中所有字母都必须大写，词间以下划线分隔。例如：
    >     <?php
    >     namespace Vendor\Model;
    >     class Foo
    >     {
    >        const VERSION = '1.0';
    >        const DATE_APPROVED = '2012-06-01';
    >     }
    - 属性

    >     大写开头的驼峰式 ($StudlyCaps)
    >     小写开头的驼峰式 ($camelCase)
    >     下划线分隔式 ($under_score)
    >     本规范不做强制要求，但无论遵循哪种命名方式，都 应该 在一定的范围内保持一致。这个范围可以是整个团队、整个包、整个类或整个方法。
    - 方法
    >     方法名称 必须 符合 camelCase() 式的小写开头驼峰命名规范。
# <a id="psr2">PSR-2 编码风格规范</a>
1. __基本编码准则__:  
    - 代码 必须 符合 PSR-1 中的所有规范。
2. __文件__:
    - 所有 PHP 文件 必须 使用 Unix LF (linefeed) 作为行的结束符。
    - 所有 PHP 文件 必须 以一个空白行作为结束。
    - 纯 PHP 代码文件 必须 省略最后的 ?> 结束标签。
3.  __行__:  
   - 行的长度 一定不可 有硬性的约束。
   - 软性的长度约束 必须 要限制在 120 个字符以内，若超过此长度，带代码规范检查的编辑器 必须 要发出警告，不过 一定不可 发出错误提示。
   - 每行 不该 多于 80 个字符，大于 80 字符的行 应该 折成多行。
   - 非空行后 一定不可 有多余的空格符。
   - 空行 可以 使得阅读代码更加方便以及有助于代码的分块。
   - 每行 一定不可 存在多于一条语句。
4. __缩进__:  
   - 代码 必须 使用 4 个空格来进行缩进， 并且 一定不能 使用 tab 键来缩进。
5. __关键字与 True/False/Null__
   - PHP 的 关键字 必须 使用小写形式。
   - PHP 的常量 true， false， 还有 null 必须 使用小写形式。
6. __命名空间和使用声明__:
    - namespace 声明之后 必须 存在一个空行。
    - 所有的 use 声明 必须 位于 namespace 声明之后。
    - 每条 use 声明 必须 只有一个 use 关键字。
    - use 语句块之后 必须 存在一个空行
    -  例如:  
    >     <?php
    >     namespace Vendor\Package;
    >     
    >     use FooClass;
    >     use BarClass as Bar;
    >     use OtherVendor\OtherPackage\BazClass;
	>     
	>     // ... 其他 PHP 代码 ...
7.  __类、属性和方法__  (此处的「类」泛指所有的「class 类」、「接口」以及「traits 可复用代码块」)
    - __扩展与继承__:  
      关键词 extends 和 implements 必须 写在类名称的同一行。  
      类的开始花括号 必须 独占一行，结束花括号也 必须 在类主体后独占一行。 
  
    >     <?php  
    >     namespace Vendor\Package;  
    >
    >     use FooClass;  
    >     use BarClass as Bar;  
    >     use OtherVendor\OtherPackage\BazClass;  
    >
    >     class ClassName extends ParentClass implements \ArrayAccess, \Countable  
    >     {  
    >     // 这里面是常量、属性、类方法  
    >     }   
         
     `implements` 的继承列表也 可以 分成多行，这样的话，每个继承接口名称都 必须 分开独立成行，包括第一个。
    >     <?php
    >     namespace Vendor\Package;
    >
    >     use FooClass;
    >     use BarClass as Bar;
    >     use OtherVendor\OtherPackage\BazClass;
    >    
    >     class ClassName extends ParentClass implements
    >         \ArrayAccess,
    >         \Countable,
    >         \Serializable
    >     {
    >         // 这里面是常量、属性、类方法
    >     }  

    - __属性__:  
    
    每个属性都必须添加访问修饰符。
    一定不可使用关键字`var`声明一个属性。  
    每条语句一定不可定义超过一个属性。  
    不该使用下划线作为前缀，来区分属性是`protected`或`private`。  
    以下是属性声明的一个范例：  
    >     <?php
    >     namespace Vendor\Package;
    >     
    >     class ClassName
    >     {
    >         public $foo = null;
    >     }  

    - __方法__:  
    所有方法都 `必须` 添加访问修饰符。  
	`不该` 使用下划线作为前缀，来区分方法是 protected 或 private 访问修饰符。  
	方法名称后 `一定不可` 有空格符，其开始花括号 `必须` 独占一行，结束花括号也 `必须` 在方法主体后单独成一行。参数左括号后和右括号前 `一定不可` 有空格。  
	一个标准的方法声明可参照以下范例，留意其括号、逗号、空格以及花括号的位置。  
    >     <?php
    >     namespace Vendor\Package;
    >     
    >     class ClassName
    >     {
    >         public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    >         {
    >             // 方法主体
    >         }
    >     }  
    - __方法的参数__:  
    参数列表中，每个逗号后面 `必须` 要有一个空格，而逗号前面 `一定不可` 有空格。  
	有默认值的参数，必须 放到参数列表的末尾。  
    >     <?php
    >     namespace Vendor\Package;
    >     
    >     class ClassName
    >     {
    >        public function foo($arg1, &$arg2, $arg3 = [])
    >        {
    >             // 方法主体
    >        }
    >     }  

	 参数列表 可以 分列成多行，这样，包括第一个参数在内的每个参数都 必须 单独成行。
 
	 拆分成多行的参数列表后，结束括号以及方法开始花括号 必须 写在同一行，中间用一个空格分隔。  
    >     <?php
    >     namespace Vendor\Package;
    >     
    >     class ClassName
    >     {
    >         public function aVeryLongMethodName(
    >             ClassTypeHint $arg1,
    >             &$arg2,
    >             array $arg3 = []
    >         ) {
    >            // 方法主体
    >        }
    >     }

    - __`abstract`, `final`, 和 `static` 关键字__:  
    需要添加 abstract 或 final 声明时，必须 写在访问修饰符前，而 static 则 必须 写在其后。  
    >     <?php
    >     namespace Vendor\Package;
    >     
    >     abstract class ClassName
    >     {
    >         protected static $foo;
    >     
    >         abstract protected function zim();
    >     
    >         final public static function bar()
    >         {
    >             // 方法主体
    >        }
    >     }

    - __方法及函数调用__:  
    方法及函数调用时，方法名或函数名与参数左括号之间 一定不可 有空格，参数右括号前也 一定不可 有空格。每个逗号前 一定不可 有空格，但其后 必须 有一个空格。  
    >     <?php
    >     bar();
    >     $foo->bar($arg1);
    >     Foo::bar($arg2, $arg3);  


    参数 可以 分列成多行，此时包括第一个参数在内的每个参数都 必须 单独成行。  
    >     <?php
    >     $foo->bar(
    >         $longArgument,
    >         $longerArgument,
    >         $muchLongerArgument
    >     );     
    

8. __控制结构__  
    - 控制结构的基本规范如下：  
     控制结构关键词后 必须 有一个空格。
	 左括号 ( 后 一定不可 有空格。
     右括号 ) 前也 一定不可 有空格。
     右括号 ) 与开始花括号 { 间 必须 有一个空格。
     结构体主体 必须 要有一次缩进。
     结束花括号 } 必须 在结构体主体后单独成行。
     每个结构体的主体都 必须 被包含在成对的花括号之中，
     这能让结构体更加标准化，以及减少加入新行时，出错的可能性。   

   - __`if`, `elseif`, `else`__  
   	 标准的 if 结构如下代码所示，请留意「括号」、「空格」以及「花括号」的位置，
	 注意 else 和 elseif 都与前面的结束花括号在同一行。

    >     <?php
    >     if ($expr1) {
    >         // if body
    >     } elseif ($expr2) {
    >         // elseif body
    >     } else {
    >         // else body;
    >     }  

	应该 使用关键词 elseif 代替所有 else if ，以使得所有的控制关键字都像是单独的一个词。  
    - __`switch`, `case`__  
    标准的 switch 结构如下代码所示，留意「括号」、「空格」以及「花括号」的位置。case 语句 必须 相对 switch 进行一次缩进，而 break 语句以及 case 内的其它语句都 必须 相对 case 进行一次缩进。如果存在非空的 case 直穿语句，主体里 必须 有类似 // no break 的注释  
    >     <?php
    >     switch ($expr) {
    >         case 0:
    >             echo 'First case, with a break';
    >             break;
    >        case 1:
    >            echo 'Second case, which falls through';
    >            // no break
    >        case 2:
    >        case 3:
    >        case 4:
    >            echo 'Third case, return instead of break';
    >            return;
    >        default:
    >            echo 'Default case';
    >            break;
    >     }  

    - __`while`, `do while`__ :  
    一个规范的 while 语句应该如下所示，注意其「括号」、「空格」以及「花括号」的位置。  
    >     <?php
    >     while ($expr) {
    >         // 结构体
    >     }  

	标准的 do while 语句如下所示，同样的，注意其「括号」、「空格」以及「花括号」的位置。  
    >     <?php
    >      do {
    >        // 结构体
    >     } while ($expr);  

    - __`for`__ :  
    标准的 for 语句如下所示，注意其「括号」、「空格」以及「花括号」的位置。  
    >     <?php
    >     for ($i = 0; $i < 10; $i++) {
    >         // for 循环主体
    >     }  

    - __`foreach `__ :  
    标准的 foreach 语句如下所示，注意其「括号」、「空格」以及「花括号」的位置。  
    >     <?php
    >     foreach ($iterable as $key => $value) {
    >         // foreach 主体
    >     }  

    - __`try `，`catch `__ :  
    标准的 try catch 语句如下所示，注意其「括号」、「空格」以及「花括号」的位置。  
   >     <?php
   >     try {
   >         // try 主体
   >     } catch (FirstExceptionType $e) {
   >         // catch 主体
   >     } catch (OtherExceptionType $e) {
   >         // catch 主体
   >     }  

9. __闭包__ :  
    - 闭包声明时，关键词 function 后以及关键词 use 的前后都 必须 要有一个空格。
    - 开始花括号 必须 写在声明的同一行，结束花括号 必须 紧跟主体结束的下一行。
    - 参数列表和变量列表的左括号后以及右括号前，一定不可 有空格。
    - 参数和变量列表中，逗号前 一定不可 有空格，而逗号后 必须 要有空格。
    - 闭包中有默认值的参数 必须 放到列表的后面。
    - 标准的闭包声明语句如下所示，注意其「括号」、「空格」以及「花括号」的位置。 
    >     <?php
    >     $closureWithArgs = function ($arg1, $arg2) {
    >         // 主体
    >     };
    >     
    >     $closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    >         // 主体
    >     };  

    - 参数列表以及变量列表 可以 分成多行，这样，包括第一个在内的每个参数或变量都 必须 单独成行，而列表的右括号与闭包的开始花括号 必须 放在同一行。 
    - 以下几个例子，包含了参数和变量列表被分成多行的多情况。  
    >     <?php
    >     $longArgs_noVars = function (
    >         $longArgument,
    >         $longerArgument,
    >         $muchLongerArgument
    >     ) {
    >         // 主体
    >     };
    >     
    >     $noArgs_longVars = function () use (
    >         $longVar1,
    >         $longerVar2,
    >         $muchLongerVar3
    >     ) {
    >         // 主体
    >     };
    >     
    >     $longArgs_longVars = function (
    >         $longArgument,
    >         $longerArgument,
    >         $muchLongerArgument
    >     ) use (
    >         $longVar1,
    >         $longerVar2,
    >         $muchLongerVar3
    >     ) {
     >        // 主体
    >     };
    >     
    >     $longArgs_shortVars = function (
    >         $longArgument,
    >         $longerArgument,
    >         $muchLongerArgument
    >     ) use ($var1) {
    >         // 主体
    >     };
    >     
    >     $shortArgs_longVars = function ($arg) use (
    >         $longVar1,
    >         $longerVar2,
    >         $muchLongerVar3
    >     ) {
    >         // 主体
    >     };  

	注意，闭包被直接用作函数或方法调用的参数时，以上规则仍然适用。 
	>     <?php
	>     $foo->bar(
	>         $arg1,
	>         function ($arg2) use ($var1) {
	>             // 主体
	>         },
	>         $arg3
	>     );  
# <a id="psr3">PSR-3 日志接口规范</a>
# <a id="psr4">PSR-4 自动加载规范</a>
# PSR-5 PHPDoc 标准（未通过）
# PSR-6 缓存接口规范
# PSR-7 HTTP 消息接口规范
# PSR-8 Huggable 接口（未通过）
# PSR-9 项目安全问题公示（未通过）
# PSR-10 项目安全上报方法（未通过）
# PSR-12 编码规范扩充
# PSR-13 超媒体链接
# PSR-14 事件分发器
# PSR-15 HTTP 请求处理器
# PSR-16 缓存接口
# PSR-17 HTTP 工厂
# PSR-18 HTTP 客户端