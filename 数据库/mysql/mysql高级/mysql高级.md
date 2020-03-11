# 一、MySQL架构
## (1)、 MySQL简介  
### 概述
### 高级MySQl
- MySQL内核
- SQL优化工程师
- MySQL服务器的优化
- 查询语句优化
- 主从复制
- 软硬件升级
- 容灾备份
- sql编程

__说明__：完整的mysql优化需要很深的功底,大公司甚至有专门的DBA写上述  

## (2). MysqlLinux版本的安装--mysql5.5  

## (3). Mysql配置文件   
### 主要配置文件
- 二进制日志log-bin

主重复制  

- 错误日志`log-error`

默认是关闭的,记录严重的警告和错误信息,每次启动和关闭的详细信息等。  

- 查询日志`log`

默认关闭,记录查询的sql语句，如果开启会减低mysql的整体性能，因为记录日志也是需要消耗系统资源的

- 数据文件
   - windows:D:\ProgramFiles\MySQL\MySQLServer5.5\`data`目录下可以挑选很多库  
   linux: 看看当前系统中的全部库后再进去,默认路径：`/var/lib/mysql`  
   - `frm`文件:存放表结构
   - `myd`文件：存放表数据
   - `myi`文件：存放表索引
- 如何配置
   - windows：`my.ini`文件  
   - Linux：`/etc/my.cnf`文件  
    
## (4). MySQL逻辑架构介绍  

### 总体概述

和其他数据库相比，MySQL有点与众不同，它的架构可以在多种不同的场景中应用并发挥良好作用。主要体现在存储引擎的架构上。__插件式的存储引擎架构将查询处理和其他的系统任务以及数据的存储提取相分离。__这种架构可以根据业务的需求和实际需要选择合适的存储引擎。  

![mysql架构](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/mysqljiagou.png)   

1.__连接层：__  

  最上层是一些客户端和连接服务，包含本地socket通信和大多数基于客户端/服务器工具实现的类似于tcp/ip的通信，主要完成一些类似于连接处理、授权认证、及相关的安全方案。在该层上引入线程池的概念。为通过认证安全接入的客户端提供线程。同样在该层上可以实现基于SSL的安全链接。服务器也会为安全接入的每个客户端验证它所具有的操作权限。  

2.__服务层：__  

第二层架构主要完成大多数的核心服务功能，如SQL接口，并完成缓存的查询，SQL的分析和优化以及部分内置函数的执行，所有跨存储引擎的功能也在这一层实现，如过程、函数等。在该层，服务器会解析查询并创建相应的内部解析树，并对其完成相应的优化如确定查询表的顺序，是否利用索引等，最后生成相应执行操作。如果是select语句，服务器还会查询内部的缓存。如果缓存空间足够大，这样在解决大量读写操作的环境中能够很好的提升系统的性能。  

3.__引擎层：__  

存储引擎层，存储引擎真正的负责了MySQL中数据的存储和提取，服务器通过API与存储引擎进行通信。不同的存储引擎具体的功能不同，这样我们可以根据自己的实际需要进行选取。

4.__存储层：__
 
数据存储层。主要是将数据存储在运行于裸设备的文件系统上，并完成与存储引擎的交互。  

## (5). MySQL存储引擎  
- 查看命令
 
`show engines`;

-` MyISAM`和`InnoDB`  

![存储引擎对比](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/duibi.png)  
 
- 阿里巴巴，淘宝用哪个  

![阿里engine](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/aliengine.png)   

# 二、索引优化分析

## (1) SQL性能下降原因SQL慢(执行时间长，等待时间时长)  

- 查询语句写的烂
- 索引失效
- 关联查询太多join(设计缺陷或不得已的需求)
- 服务器调优以及各个参数设置(缓冲\线程数)

## (2) 常见通用的`join`查询
- SQL执行顺序
   - 手写：  
![手写](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/join1.png) 

   - 机读：  
![机读](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/join2.png)
   - 总结：

 ![总结](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/join3.png)

- Join图
 ![7种join图](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/join.png)

- 建表SQL
- 7种Join
## (3) 索引简介

### 索引是什么

    MySQL官方对索引的定义为：索引(Index)是帮助MySQL高校获取数据的数据结构。
    可以得到索引的本质：索引是数据结构；

你可以简单理解为"排好序的快速查找数据结构"。  

- 详解：

 ![索引详解](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index.png)

- 总结：  

    数据本身之外,数据库还维护着一个满足特定查找算法的数据结构，这些数据结构以某种方式指向数据，
    这样就可以在这些数据结构的基础上实现高级查找算法,这种数据结构就是索引。


一般来说索引本身也很大，不可能全部存储在内存中，因此索引往往以文件形式存储在硬盘上。  

__我们平时所说的索引，如果没有特别指明，都是指B树(多路搜索树，并不一定是二叉树)结构组织的索引。__其中聚集索引，次要索引，覆盖索引，
复合索引，前缀索引，唯一索引默认都是使用B+树索引，统称索引。当然,除了B+树这种类型的索引之外，还有哈希索引(hash index)等。  

### 优缺点

- 优点：  
  类似大学图书馆建书目索引，提高数据检索效率，降低数据库的IO成本。  
  通过索引列对数据进行排序，降低数据排序成本，降低了CPU的消耗。


- 缺点：  
  实际上索引也是一张表，该表保存了主键和索引字段，并指向实体表的记录,所以索引列也是要占用空间的。  
  虽然索引大大提高了查询速度，同时却会降低更新表的速度,如果对表INSERT,UPDATE和DELETE。
  因为更新表时，MySQL不仅要不存数据，还要保存一下索引文件每次更新添加了索引列的字段，
  都会调整因为更新所带来的键值变化后的索引信息。  
  索引只是提高效率的一个因素，如果你的MySQL有大数据量的表，就需要花时间研究建立优秀的索引，或优化查询语句。  

### mysql索引分类
- 单值索引  
  即一个索引只包含单个列，一个表可以有多个单列索引：  
  即一个索引只包含单个列，一个表可以有多个单列索引。  

- 唯一索引  
  索引列的值必须唯一，但允许有空值。  

- 复合索引  
  即一个索引包含多个列。  

- 基本语法  
  - 创建：  
  
    `CREATE [UNIQUE] INDEX  indexName ON mytable(columnname(length));`  
  如果是CHAR,VARCHAR类型，length可以小于字段实际长度；如果是BLOB和TEXT类型，必须指定length。  

    `ALTER mytable ADD [UNIQUE]  INDEX [indexName] ON(columnname(length));`  

  - 删除:  
    `DROP INDEX [indexName] ON mytable;`  

  - 查看:  
   `SHOW INDEX FROM table_name\G`    

  - 使用ALTER命令  
   
   ![alter_index](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/alter_index.png)  


