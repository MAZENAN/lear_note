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
- [3.查询截取分析](#cxjq)
- [4.Mysql锁机制](#lock)
- [5.主从复制](#ms)
- [6.高可扩展性](#ms)

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

## 4、<a id="sy_xnfx">性能分析</a>
### (1) MySQL Query Optimizer  

![MySQL Query Optimizer](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/mqo.png)  

### (2) MySQL常见瓶颈

1. CPU:CPU在饱和的时候一般发生在数据装入在内存或从磁盘上读取数据时候  
2. IO:磁盘I/O瓶颈发生在装入数据远大于内存容量时  
3. 服务器硬件的性能瓶颈：top,free,iostat和vmstat来查看系统的性能状态  

### (3) Explain
#### 是什么（查看执行计划）:  
 使用EXPLAIN关键字可以模拟优化器执行SQL语句，从而知道MySQL是
 如何处理你的SQL语句的。分析你的查询语句或是结构的性能瓶颈  
#### 能干嘛:  
表的读取顺序;数据读取操作的操作类型;哪些索引可以使用;哪些索引被实际使用;表之间的引用;每张表有多少行被优化器查询;

#### 怎么玩:  
 Explain+SQL语句  
 执行计划包含的信息:    
![explain](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/explain.png)  

#### 各个字段解释:  
- `id`:  
select查询的序列号，包含一组数字，表示查询中执行select子句或操作表的顺序。    
三种情况：  
id相同，执行顺序由上至下；  

![id1](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/id1.png)

id不同，如果是子查询，id的序号会递增，id值越大优先级越高，越先被执行：  

![id2](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/id2.png)  

  
id相同不同，同时存在。  

![id3](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/id3.png)  

- `select_type`: 查询的类型，主要用于区别：普通查询、联合查询、子查询等的复杂查询   

1.SIMPLE： 简单的select查询，查询中不包含子查询或者UNION  
2.PRIMARY：查询中若包含任何复杂的子部分，最外层查询则被标记为  
3.SUBQUERY：在SELECT或者WHERE列表中包含了子查询  
4.DERIVED：在FROM列表中包含的子查询被标记为DERIVED（衍生）MySQL会递归执行这些子查询，把结果放在临时表里。  
5.UNION： 若第二个SELECT出现在UNION之后，则被标记为UNION;
若UNION包含在FROM子句的子查询中，外层SELECT将被标记为：DERIVED    
6.UNION RESULT： 从UNION表获取结果的SELECT  
  
- `table`:  显示这一行的数据是关于哪张表的   
- `type`:    

显示查询使用了何种类型  
从最好到最差依次是：  
`system>const>eq_ref>ref>range>index>ALL`  
一般来说得保证查询至少达到range级别，最好能达到ref  

1.`system`：表只有一行记录（等于系统表），这是const类型的特例，平时不会出现，这个也可以忽略不计  
2.`const`：表示通过索引一次就找到了，const用于比较primary key或者unique索引。因为只匹配一行数据，所以很快。如将主键至于where列表中，MySQL就能将该查询转换为一个常量  
3.`eq_ref`：唯一性索引，对于每个索引键，表中只有一条记录与之匹配，常见于主键或唯一索引扫描  
4.`ref`：非唯一索引扫描，返回匹配某个单独值的所有行。
本质上也是一种索引访问，它返回所有匹配某个单独值的行，然而，
它可能会找到多个符合条件的行，所以他应该属于查找和扫描的混合体  
5.`range`：只检索给定范围的行，使用一个索引来选择行。key列显示使用了哪个索引
一般就是在你的where语句中出现了between、<、>、in等的查询
这种范围扫描索引扫描比全表扫描要好，因为他只需要开始索引的某一点，而结束语另一点，不用扫描全部索引  
6.`index`：Full Index Scan,index与ALL区别为index类型只遍历索引树。这通常比ALL快，因为索引文件通常比数据文件小。
（也就是说虽然all和index都是读全表，但index是从索引中读取的，而all是从硬盘中读的）  
7.`all`:FullTable Scan,将遍历全表以找到匹配的行  

备注：一般来说，得保证查询只是达到range级别，最好达到ref

- `possible_keys`:   

显示可能应用在这张表中的索引,一个或多个。
查询涉及的字段上若存在索引，则该索引将被列出，但不一定被查询实际使用  

- `key`:   

实际使用的索引。如果为null则没有使用索引，查询中若使用了覆盖索引，则索引和查询的select字段重叠  

- `key_len`:  

表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度。在不损失精确性的情况下，长度越短越好。key_len显示的值为索引最大可能长度，并非实际使用长度，即key_len是根据表定义计算而得，不是通过表内检索出的  

![key_len](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/key_len.png) 

- `ref`:  

显示索引那一列被使用了，如果可能的话，是一个常数。那些列或常量被用于查找索引列上的值  

![ref](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/ref.png)

- `rows`  

根据表统计信息及索引选用情况，大致估算出找到所需的记录所需要读取的行数  
![rows](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/rows.png)  

越少越好

- `Extra` ： 包含不适合在其他列中显示但十分重要的额外信息  

1.`Using filesort`:  

说明mysql会对数据使用一个外部的索引排序，而不是按照表内的索引顺序进行读取。
MySQL中无法利用索引完成排序操作成为“文件排序”   

![filesort](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/filesort.png)   

2.`Using temporary`:  

使用了临时表保存中间结果，MySQL在对查询结果排序时使用临时表。常见于排序order by 和分组查询 group by  

![temporary](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/temporary.png)  

4.`Using where`: 表面使用了where过滤  

5.`using join buffer`   

使用了连接缓存
 
6.`impossible where`   

where子句的值总是false，不能用来获取任何元组

![impossible_where](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/impossible_where.png) 

7.`select tables optimized away`：  

在没有GROUPBY子句的情况下，基于索引优化MIN/MAX操作或者
对于MyISAM存储引擎优化COUNT(*)操作，不必等到执行阶段再进行计算，
查询执行计划生成的阶段即完成优化。    

8.`distinct`  

优化distinct，在找到第一匹配的元组后即停止找同样值的工作

case:  
![case](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/case.png)

## <a id="sy_yh">5、索引优化</a>  

### (1) 索引分析

### (2) 索引失效(应该避免)  

1.全值匹配我最爱  
![index_sx1](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index_sx1.png)

2.最佳左前缀法则  

如果索引了多例，要遵守最左前缀法则。指的是查询从索引的最左前列开始并且不跳过索引中的列。  

![index_sx2](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index_sx2.png)  

3.不在索引列上做任何操作（计算、函数、（自动or手动）类型转换），会导致索引失效而转向全表扫描  

![index_sx3](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index_sx3.png)

4.存储引擎不能使用索引中范围条件右边的列(范围之后全失效)  

![index_sx4](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index_sx4.png)

5.尽量使用覆盖索引（只访问索引的查询（索引列和查询列一致）），减少select*   

![index_sx5](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index_sx5.png)  

![index_sx6](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index_sx6.png)   
 
6.mysql在使用不等于（！=或者<>）的时候无法使用索引会导致全表扫描   

![index_sx7](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index_sx7.png)
 
7.is null,is not null 也无法使用索引  

![index_sx8](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index_sx8.png)  

8.like以通配符开头（'$abc...'）mysql索引失效会变成全表扫描操作  

![index_sx9](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index_sx9.png)    

问题：解决like'%字符串%'索引不被使用的方法？？  
 1、可以使用主键索引  
 2、使用覆盖索引，查询字段必须是建立覆盖索引字段  
 3、当覆盖索引指向的字段是varchar(380)及380以上的字段时，覆盖索引会失效！  

9.字符串不加单引号索引失效   

![index_sx10](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index_sx10.png) 

10.少用or,用它连接时会索引失效  

![index_sx11](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index_sx11.png)


11.小总结  
![index_sx12](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index_sx12.png)  

like KK%相当于=常量     %KK和%KK% 相当于范围   

![index_sx13](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/index_sx13.png)  

### (3) 一般性建议  

对于单键索引，尽量选择针对当前query过滤性更好的索引;  
在选择组合索引的时候，当前Query中过滤性最好的字段在索引字段顺序中，位置越靠前越好;  
在选择组合索引的时候，尽量选择可以能包含当前query中的where子句中更多字段的索引;  
尽可能通过分析统计信息和调整query的写法来达到选择合适索引的目的。  


# <a id="cxjq">三、查询截取分析</a>

# <a id="lock">四、Mysql锁机制</a>  
## 1.概述

### 定义

![lock](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/lock.png)   

### 锁的分类
- 从数据操作的类型（读、写）分:  
  - 读锁（共享锁）：针对同一份数据，多个读操作可以同时进行而不会互相影响   
  - 写锁（排它锁）：当前写操作没有完成前，它会阻断其他写锁和读锁。 
 
- 从对数据操作的颗粒度  
  - 表锁
  - 行锁

## 2.三锁
### 表锁（偏读）
(1) 特点： 偏向MyISAM存储引擎，开销小，加锁快，无死锁，锁定粒度大，发生锁冲突的概率最高，并发最低

(2) 案例分析   
   
- 建表SQL   

![lock_create](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/lock_create.png)   

![unlock](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/unlock.png)
  
- 加读锁  

![lock_read1](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/lock_read1.png)  

![lock_read2](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/lock_read2.png)  

![lock_read2](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/lock_read3.png)  

- 加写锁   
![lock_write1](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/lock_write1.png)  

![lock_write2](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/lock_write2.png)

(3) 案例结论  

![lock_result1](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/lock_result1.png)  

![lock_result2](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/lock_result2.png)  

(4) 表锁分析  

![table_lock1](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/table_lock1.png)  

![table_lock2](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/table_lock2.png)  

![table_lock3](https://github.com/MAZENAN/lear_note/blob/master/数据库/mysql/img/table_lock3.png)


### 行锁(偏写)
### 页锁



# <a id="ms">五、主从复制</a>