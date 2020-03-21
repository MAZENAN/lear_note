# redis数据类型

| 数据类型  | 存储的值                                             |  读写能力                                  |    
| -------- | ---------------------------------- | ----------------------------------------- |  
| String   | 可以是字符串、整形或浮点，统称为元素                    |   对字符串操作，对整数类型加减               |  
| List     |  一个序列集合且每个节点都包好了一个元素                  | 序列两端推入、或弹出元素 修剪、查找或移除元素|      
| Set      | 各不相同的元素                                         |    从集合中插入或者删除元素  |    
| Hash     |    有key-value的散列组，其中key是字符串,value是元素      |  按照key进行增加删除  |     
| Sort Set |  带分数的score-value有序集合，其中score为浮点,value为元素|  集合插入，按照分数范围查找  |   

# `String`类型

    key | value（string/int/float）

- 设置值：`set k v`
- 获取值：`get k`
- 自增操作：`incr k`
- 自减操作：`decrby k`

# `list`类型

![list](https://github.com/MAZENAN/lear_note/blob/master/nosql/redis/img/redis_list.png)  

- 左边推入：`lpush l1 v1`
- 右边推入：`rpush l1 v2`
- 左边弹出：`lpop l1 v1`
- 右边弹出`rpop l1 v2`
- list长度：`llen l1`