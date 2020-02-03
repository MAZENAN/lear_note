# composer安装步骤
# composer使用场景
## 场景一：依赖管理  
### 使用步骤：
1. 在根目录创建composer.json
2. 按照固定格式写上你要下载的第三方名称  
         {
    	"require": {
    		"smarty/smarty": "~3.1"
    	    }
         } 
3. 在根目录下运行cmd命令
         composer validate(4. 验证composer.json的文件内容格式是否正确(非必备))
         composer install（安装）

***
## 场景二：自动加载  
### 使用步骤 ：
1. 在根目录创建composer.json
2. 自动加载分类：
    * 加载目录下的所有文件(classmap中填写想要加载的目录路径)
         {
	         "require": {
	         },
	         "autoload": {
		         "classmap": ["controller", "model"]
	         }
         }
    * 单个文件自动加载 (files键用来指定单个文件的自动加载)
         {
         	"autoload": {
         		"files": [
	         		"libs/DB.php",
        			"libs/Model.php"
	         	]
	        }
         }
    * psr4 命名空间简化  (PSR4规范要求 空间名与路径一致, 但是实际项目中往往目录层级比较深, 会导致空间名太长, 此时就需要进行空间名的简化操作;通过映射的关系, 可以让composer记录, 某个空间名实际对应的是哪个路径)
         {
	         "autoload": {
	         	"psr-4": {
		         	"app\\con\\": "application/controller/"
		         }
	         }
         }

3. 执行 composer dump-autoload 命令
