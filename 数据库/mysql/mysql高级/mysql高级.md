- [1.MySQL架构](#mysql_jg)
  - [1.MySQL简介](#mysql_jj)
  - [2.MysqlLinux版本的安装](#mysql_az)
  - [3.Mysql配置文件](#mysql_pz)
  - [4.MySQL逻辑架构介绍](#mysql_ljjg)
  - [5.MySQL存储引擎](#mysql_ccyq)
- [2.索引优化分析](#sy)
  - [1.SQL性能下降原因SQL慢](#sy_xn)
  - [2.常见通用的`join`查询](#sy_join)
  - [3.索引简介](#sy_jj)
  - [4.性能分析](#sy_xnfx)
  - [5.索引优化](#sy_yh)
- [3.查询截取分析](#bx)
- [4.Mysql锁机制](#cxyh)
- [5.主重复制](#gky)

# 一、<a id="mysql_jg">MySQL架构</a>
## 1、<a id="mysql_jj">MySQL简介</a>  
### (1) 概述
### (2) 高级MySQl
- MySQL内核
- SQL优化工程师
- MySQL服务器的优化
- 查询语句优化
- 主从复制
- 软硬件升级
- 容灾备份
- sql编程

__说明__：完整的mysql优化需要很深的功底,大公司甚至有专门的DBA写上述  

## 2、 <a id="mysql_az">MysqlLinux版本的安装--mysql5.5</a>  

## 3、 <a id="mysql_pz">Mysql配置文件</a>   
### (1) 主要配置文件
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
    
## 4、<a id="mysql_ljjg">MySQL逻辑架构介绍</a> 

### (1) 总体概述

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

## 5、<a id="mysql_ccyq"> MySQL存储引擎 </a> 
- 查看命令
 
`show engines`;

- ` MyISAM`和`InnoDB`  

![存储引擎对比](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/duibi.png)  
 
- 阿里巴巴，淘宝用哪个  

![阿里engine](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/aliengine.png)   

# <a id="sy">二、索引优化分析</a>

## <a id="sy_xn">1、SQL性能下降原因SQL慢(执行时间长，等待时间时长)</a> 

- 查询语句写的烂
- 索引失效
- 关联查询太多join(设计缺陷或不得已的需求)
- 服务器调优以及各个参数设置(缓冲\线程数)

## <a id="sy_join">2、常见通用的`join`查询</a>
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
## <a id="sy_jj">3、索引简介</a>

### (1)、索引是什么

    MySQL官方对索引的定义为：索引(Index)是帮助MySQL高效获取数据的数据结构。
    可以得到索引的本质：索引是数据结构；

你可以简单理解为"排好序的快速查找数据结构"。  

- 详解：

 ![索引详解](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index.png)

- 总结：  

    数据本身之外,数据库还维护着一个满足特定查找算法的数据结构，这些数据结构以某种方式指向数据，
    这样就可以在这些数据结构的基础上实现高级查找算法,这种数据结构就是索引。


一般来说索引本身也很大，不可能全部存储在内存中，因此索引往往以文件形式存储在硬盘上。  

__我们平时所说的索引，如果没有特别指明，都是指B树(多路搜索树，并不一定是二叉树)结构组织的索引。__ 其中聚集索引，次要索引，覆盖索引，
复合索引，前缀索引，唯一索引默认都是使用B+树索引，统称索引。当然,除了B+树这种类型的索引之外，还有哈希索引(hash index)等。  

### (2)、优缺点

- 优点：  
  类似大学图书馆建书目索引，提高数据检索效率，降低数据库的IO成本。  
  通过索引列对数据进行排序，降低数据排序成本，降低了CPU的消耗。


- 缺点：  
  实际上索引也是一张表，该表保存了主键和索引字段，并指向实体表的记录,所以索引列也是要占用空间的。  
  虽然索引大大提高了查询速度，同时却会降低更新表的速度,如果对表INSERT,UPDATE和DELETE。
  因为更新表时，MySQL不仅要不存数据，还要保存一下索引文件每次更新添加了索引列的字段，
  都会调整因为更新所带来的键值变化后的索引信息。  
  索引只是提高效率的一个因素，如果你的MySQL有大数据量的表，就需要花时间研究建立优秀的索引，或优化查询语句。  

### (3)、mysql索引分类
- 单值索引  
  即一个索引只包含单个列，一个表可以有多个单列索引：  
  建议一张表索引不要超过5个,优先考虑复合索引    

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

### (4)、mysql索引结构  
- BTree索引  

![btree1](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/btree1.png)  

![btree2](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/btree2.png)  

Btree索引(或Balanced Tree)，是一种很普遍的数据库索引结构，oracle默认的索引类型（本文也主要依据oracle来讲）。其特点是定位高效、利用率高、自我平衡，特别适用于高基数字段，定位单条或小范围数据非常高效。理论上，使用Btree在亿条数据与100条数据中定位记录的花销相同。  

数据结构利用率高、定位高效  
Btree索引的数据结构如下：    

![btree3](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/btree3.png)  

结构看起来Btree索引与Binary Tree相似，但在细节上有所不同，上图中用不同颜色的标示出了Btree索引的几个主要特点：
 
树形结构：由根节(root)、分支(branches)、叶(leaves)三级节点组成，其中分支节点可以有多层。  
多分支结构：与binary tree不相同的是，btree索引中单root/branch可以有多个子节点(超过2个)。  
双向链表：整个叶子节点部分是一个双向链表(后面会描述这个设计的作用)
单个数据块中包括多条索引记录  
这里先把几个特点罗列出来，后面会说到各自的作用。  
 
结构上Btree与Binary Tree的区别，在于binary中每节点代表一个数值，而balanced中root和Btree节点中记录了多条”值范围”条目(如:[60-70][70-80])，这些”值范围”条目分别指向在其范围内的叶子节点。既root与branch可以有多个分支，而不一定是两个，对数据块的利用率更高。  
 
在Leaf节点中，同样也是存放了多条索引记录，这些记录就是具体的索引列值，和与其对应的rowid。另外，在叶节点层上，所有的节点在组成了一个双向链表。  
了解基本结构后，下图展示定位数值82的过程：  

![btree4](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/btree4.png)    

演算如下：    
读取root节点，判断82大于在0-120之间，走左边分支。  
读取左边branch节点，判断82大于80且小于等于120，走右边分支。  
读取右边leaf节点，在该节点中找到数据82及对应的rowid  
使用rowid去物理表中读取记录数据块(如果是count或者只select rowid，则最后一次读取不需要)  
 
在整个索引定位过程中，数据块的读取只有3次。既三次I/O后定位到rowid。  
 
而由于Btree索引对结构的利用率很高，定位高效。当1千万条数据时，Btree索引也是三层结构(依稀记得亿级数据才是3层与4层的分水岭)。定位记录仍只需要三次I/O，这便是开头所说的，100条数据和1千万条数据的定位，在btree索引中的花销是一样的。  
 
平衡扩张  
除了利用率高、定位高效外，Btree的另一个特点是能够永远保持平衡，这与它的扩张方式有关。(unbalanced和hotspot是两类问题，之前我一直混在一起)，先描述下Btree索引的扩张方式：  
 
新建一个索引，索引上只会有一个leaf节点，取名为Node A，不断的向这个leaf节点中插入数据后，直到这个节点满，这个过程如下图（绿色表示新建/空闲状态，红色表示节点没有空余空间）：       

![btree5](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/btree5.png)    

当Node A满之后，我们再向表中插入一条记录，此时索引就需要做拆分处理：会新分配两个数据块NodeB & C，如果新插入的值，大于当前最大值，则将Node A中的值全部插入Node B中，将新插入的值放到Node C中；否则按照5-5比例，将已有数据分别插入到NodeB与C中。  
 
无论采用哪种分割方式，之前的leaf节点A，将变成一个root节点，保存两个范围条目，指向B与C，结构如下图（按第一种拆分形式）：  


![btree6](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/btree6.png)  

当Node C满之后，此时 Node A仍有空余空间存放条目，所以不需要再拆分，而只是新分配一个数据块Node D，将在Node A中创建指定到Node D的条目：    

![btree7](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/btree7.png)   
 

如果当根节点Node A也满了，则需要进一步拆分：新建Node E&F&G，将Node A中范围条目拆分到E&F两个节点中，并建立E&F到BCD节点的关联，向Node G插入索引值。此时E&F为branch节点，G为leaf节点，A为Root节点：     

![btree8](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/btree8.png) 
   

在整个扩张过程中，Btree自身总能保持平衡，Leaf节点的深度能一直保持一致。  

实际应用中的一些问题  
前面说完了Btree索引的结构与扩张逻辑，接下来讲一些Btree索引在应用中的一些问题：  
 
单一方向扩展引起的索引竞争(Index Contention)  

若索引列使用sequence或者timestamp这类只增不减的数据类型。这种情况下Btree索引的增长方向总是不变的，不断的向右边扩展，因为新插入的值永远是最大的。  

当一个最大值插入到leaf block中后，leaf block要向上传播，通知上层节点更新所对应的“值范围”条目中的最大值，因此所有靠右边的block(从leaf 到branch甚至root)都需要做更新操作，并且可能因为块写满后执行块拆分。  

如果并发插入多个最大值，则最右边索引数据块的的更新与拆分都会存在争抢，影响效率。在AWR报告中可以通过检测enq: TX – index contention事件的时间来评估争抢的影响。解决此类问题可以使用Reverse Index解决，不过会带来新的问题。  

Index Browning 索引枯萎(不知道该怎么翻译这个名词，就是指leaves节点”死”了，树枯萎了)  
 
其实oracle针对这个问题有优化机制，但优化的不彻底，所以还是要拿出来的说。  
 
我们知道当表中的数据删除后，索引上对应的索引值是不会删除的，特别是在一性次删除大批量数据后，会造成大量的dead leaf挂到索引树上。考虑以下示例，如果表100以上的数据会部被删除了，但这些记录仍在索引中存在，此时若对该列取max()：  

![btree9](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/btree9.png)  



通过与之前相同演算，找到了索引树上最大的数据块，按照记录最大的值应该在这里，但发现这数据块里的数据已经被清空了，与是利用Btree索引的另一个特点：leaves节点是一个双向列表，若数据没有找到就去临近的一个数据块中看看，在这个数据块中发现了最大值99。  
 
在计算最大值的过程中，这次的定位多加载了一个数据块，再极端的情况下，大批量的数据被删除，就会造成大量访问这些dead leaves。  
 
针对这个问题的一般解决办法是重建索引，但记住! 重建索引并不是最优方案，详细原因可以看看这。使用coalesce语句来整理这些dead leaves到freelist中，就可以避免这些问题。理论上oracle中这步操作是可以自动完成的，但在实际中一次性大量删除数据后，oracle在短时间内是反应不过来的。  


- Hash索引
- full-text全文索引
- R-Tree索引  

### (5)、哪些情况下需要创建索引  

- 1.主键自动建立唯一索引  
- 2.频繁作为查询的条件的字段应该创建索引  
- 3.查询中与其他表关联的字段，外键关系建立索引
- 4.频繁更新的字段不适合创建索引：因为每次更新不单单是更新了记录还会更新索引，加重IO负担  
- 5.Where条件里用不到的字段不创建索引  
- 6.单间/组合索引的选择问题，who？（在高并发下倾向创建组合索引）
- 7.查询中排序的字段，排序字段若通过索引去访问将大大提高排序的速度
- 8.查询中统计或者分组字段  

### (6)、哪些情况不需要创建索引  

- 1.表记录太少
- 2.经常增删改的表
- 3.数据重复且分布平均的表字段，因此应该只为经常查询和经常排序的数据列建立索引。
注意，如果某个数据列包含许多重复的内容，为它建立索引就没有太大的实际效果。  

![举例](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index1.png)  

## 4、性能分析
## 5、索引优化

