# 关于psr标准规范  
* 简介  
    >     PHP FIG
    >     PSR 是 PHP Standard Recommendations （PHP 推荐标准）的简写，由 PHP FIG 组织制定的 PHP 规范，是 PHP 开发的实践标准。
    >     PHP FIG，FIG 是 Framework Interoperability Group（框架可互用性小组）的缩写，由几位开源框架的开发者成立于 2009 年，从那开始也选取了很多其他成员进来（包括但不限于 Laravel, Joomla, Drupal, Composer, Phalcon, Slim, Symfony, Zend Framework 等），虽然不是「官方」组织，但也代表了大部分的 PHP 社区。
    >     项目的目的在于：通过框架作者或者框架的代表之间讨论，以最低程度的限制，制定一个协作标准，各个框架遵循统一的编码规范，避免各家自行发展的风格阻碍了 PHP 的发展，解决这个程序设计师由来已久的困扰。
    >     目前已表决通过了不少套标准，已经得到大部分 PHP 框架的支持和认可。
    >     本项目的主要面向对象是所有参与的各个成员（也就是各自框架的社区），这里是完整的 成员列表，当然，同时也欢迎其它 PHP 社区采用本规范。
***

# PSR-0 自动加载规范（已弃用）
# PSR-1 基础编码规范
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
# PSR-2 编码风格规范
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
6. 命名空间和使用声明:
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
7.  类、属性和方法  (此处的「类」泛指所有的「class 类」、「接口」以及「traits 可复用代码块」)
    - __扩展与继承__:  
      关键词 extends 和 implements 必须 写在类名称的同一行。  
      类的开始花括号 必须 独占一行，结束花括号也 必须 在类主体后独占一行。 
  
    <?php  
    namespace Vendor\Package;  

    use FooClass;  
    use BarClass as Bar;  
    use OtherVendor\OtherPackage\BazClass;  

    class ClassName extends ParentClass implements \ArrayAccess, \Countable  
    {  
    // 这里面是常量、属性、类方法  
    }  
# PSR-3 日志接口规范
# PSR-4 自动加载规范
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