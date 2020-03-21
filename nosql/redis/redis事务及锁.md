# redis事务及锁

## 事务
## 1.什么是事务

    可以一次执行多个命令，本质是一组命令的集合。一个事务中的，所有命令都会序列化，`按顺序地串行化执行而不会被其它命令插入，不许加塞`
## 2.能干吗
    一个队列中，一次性、顺序性、排他性的执行一系列命令
## 3.怎么玩
### （1）常用命令

| 命令名称             |               描述                                   |  
| -----------------   | ---------------------------------------------------|   
|  DISCARD            |取消事务，放弃执行事务块内的所有命令                    |
|EXEC                 |执行所有事务块内的命令                                |  
|UNWATCH              |取消WATCH命令对所有key的监视                         |  
|WATCH key[key...]    |监视一个(或多个)key，如果在事务执行之前这个（或这些)key被其他命令所改动，那么事务将被打断|  

### (2) case1：正常执行 

![string](https://github.com/MAZENAN/lear_note/blob/master/nosql/redis/img/redis_sw_case1.png) 
 
### (3) case2:放弃事务
![string](https://github.com/MAZENAN/lear_note/blob/master/nosql/redis/img/redis_sw_case2.png) 

### (4) case3:全体连坐

![string](https://github.com/MAZENAN/lear_note/blob/master/nosql/redis/img/redis_sw_case3.png) 
### (5) case4:冤头债主

![string](https://github.com/MAZENAN/lear_note/blob/master/nosql/redis/img/redis_sw_case4.png) 
### (6) case5:watch监控  


## 锁

- 悲观锁

    悲观锁(Pessimistic Lock), 顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁

- 乐观锁

    乐观锁(Optimistic Lock), 顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。乐观锁适用于多读的应用类型，这样可以提高吞吐量，  
    
    乐观锁策略:提交版本必须大于记录当前版本才能执行更新

>       Redis的事务中,启用的是乐观锁,只负责监测key没有被改动.