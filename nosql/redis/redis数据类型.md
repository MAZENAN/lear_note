# redis数据类型

| 数据类型  | 存储的值                                             |  读写能力                 |其他|    
| -------- | ---------------------------------- | ----------------------------------------- |---------|
| String   | 可以是字符串、整形或浮点，统称为元素                    |   对字符串操作，对整数类型加减 |string类型是二进制安全的。意思是redis的string可以包含任何数据。比如jpg图片或者序列化的对象 。string类型是Redis最基本的数据类型，一个redis中字符串value最多可以是512M|  
| List     |  一个序列集合且每个节点都包好了一个元素                  | 序列两端推入、或弹出元素 修剪、查找或移除元素|      
| Set      | 各不相同的元素                                         |    从集合中插入或者删除元素  |    
| Hash     |    有key-value的散列组，其中key是字符串,value是元素      |  按照key进行增加删除  |     
| Sort Set |  带分数的score-value有序集合，其中score为浮点,value为元素|  集合插入，按照分数范围查找  |   

# 一、 `String`类型

![string](https://github.com/MAZENAN/lear_note/blob/master/nosql/redis/img/redis_string.png) 

- 设置值：`set k v`
- 获取值：`get k`
- 自增操作：`incr k`
- 自减操作：`decrby k`

# 二、 `list`类型

![list](https://github.com/MAZENAN/lear_note/blob/master/nosql/redis/img/redis_list.png)  

- 左边推入：`lpush l1 v1`
- 右边推入：`rpush l1 v2`
- 左边弹出：`lpop l1 v1`
- 右边弹出`rpop l1 v2`
- list长度：`llen l1`

# 三、 `set`类型

![set](https://github.com/MAZENAN/lear_note/blob/master/nosql/redis/img/redis_set.png)

- 插入值：`sadd sk1 v1`
- 删除值：`srem sk1 v1`
- 判读是否有值：`sismember sk1 v1`
- 查看集合中值的数量: `scard sk1`

# 四、`hash`类型

![hash](https://github.com/MAZENAN/lear_note/blob/master/nosql/redis/img/redis_hash.png)  

- 添加值：`hset hash1 k1 v1`
- 获取值：`hget hash1 k1`
- 批量获取值：`hmget hash1 k1 k2`
- hash长度：`hlen hash1`

# 五、`sort set`

![sort_set](https://github.com/MAZENAN/lear_note/blob/master/nosql/redis/img/redis_sort_set.png)

    如果score相同，则按照value的自然顺序排列

-  添加值:`zadd zset1 7 v1`
-  查看元素的数量：`zcard zset1`
-  按照range的值大小从小到大排列取出：`zrange zset1 0 2 withscores`
-  获取zset的某个值的排名：`zrange zset1 v1`
