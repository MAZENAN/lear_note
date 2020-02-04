# composer安装步骤
# composer使用场景
## 场景一：依赖管理  
### 使用步骤：
1. 在根目录创建composer.json
2. 按照固定格式写上你要下载的第三方名称      
    >     {
    >     "require": {
    >     "smarty/smarty": "~3.1"
    >      }
    >     } 
3. 在根目录下运行cmd命令  
    >     composer validate(4. 验证composer.json的文件内容格式是否正确(非必备))
    >     composer install（安装）
### 其他命令：
- require 命令  

     >     composer require monolog/monolog 
     >     Composer 会先找到合适的版本，然后更新composer.json文件，在 require 那添加 monolog/monolog 包的相关信息，再把相关的依赖下载下来进行安装，最后更新 composer.lock 文件并生成 php 的自动加载文件。
- update 命令 update 命令用于更新项目里所有的包，或者指定的某些包：
     >     更新所有依赖
     >     composer update
     >     更新指定的包  
     >     composer update monolog/monolog  
     >     更新指定的多个包  
     >     composer update monolog/monolog symfony/dependency-injection  
     >     还可以通过通配符匹配包  
     >     composer update monolog/monolog symfony/*  
- remove 命令 remove 命令用于移除一个包及其依赖（在依赖没有被其他包使用的情况下），如果依赖被其他包使用，则无法移除：
     >     composer remove monolog/monolog
- search 命令  search 命令可以搜索包
     >     composer search monolog
- show 命令 show 命令可以列出当前项目使用到包的信息：
     >     composer show 列出所有已经安装的包
     >     composer show monolog/* 可以通过通配符进行筛选
     >     composer show monolog/monolog 显示具体某个包的信息
## 场景二：自动加载  
### 使用步骤 ：
1. 在根目录创建composer.json
2. 自动加载分类：
    * 加载目录下的所有文件(classmap中填写想要加载的目录路径)  
    >     {
    >      "require": {
    >      },
    >       "autoload": {
    >      "classmap": ["controller", "model"]
    >      }
    >     }
    * 单个文件自动加载 (files键用来指定单个文件的自动加载)  
    >     {
    >     "autoload": {
    >     "files": [
    >        		"libs/DB.php",
    >     		"libs/Model.php"
    >        	]
    >         }
    >       }
    * psr4 命名空间简化  (PSR4规范要求 空间名与路径一致, 但是实际项目中往往目录层级比较深, 会导致空间名太长, 此时就需要进行空间名的简化操作;通过映射的关系, 可以让composer记录, 某个空间名实际对应的是哪个路径)  
    >     {
	         "autoload": {
    >         	"psr-4": {
    >             	"app\\con\\": "application/controller/"
    >            }
    >         }
    >       }

3. 执行 composer dump-autoload 命令
