---
title: Redis对象类型介绍
date: 2016-04-15 10:19:58
tags: [Redis]
categories: Redis
---
对象的类型
1. 字符串对象 -- REDIS_STRING
2. 列表对象 -- REDIS_LIST
3. 哈希对象 -- REDIS_HASH
4. 集合对象 -- REDIS_SET
5. 有序集合对象 -- REDIS_ZSET

实战操作
```
# 键为字符串对象，值为字符串对象
redis> SET msg "hello world"
OK

redis> TYPE msg
string

#键为字符串对象，值为列表对象
redis> RPUSH numbers 1 3 5
3

redis> TYPE numbers
list

#键为字符串对象，值为哈希对象
redis> HMSET profile name Tom age 25 career programmer
OK

redis> TYPE profile
hash

#键为集合对象，值为集合对象
redis> SADD fruits apples banana cherry
3

redis> TYPE fruits
set

#键为字符串对象，值为有序集合对象
redis> ZADD price 8.5 apple 5.0 banana 6.0 cherry
3

redis> TYPE price
zset
```
集合和列表的区别
- 集合中的元素是唯一的，但列表中的元素是不唯一
- 集合中的元素是没有顺序的，底层实现是散列表，列表中的元素是有顺序的，列表的底层实现是双向链表，所以获取两端的数据的速度快，但获取中间的数据速度慢，时间复杂度是O(N);
- 相同点是存储内容都是至多为2^32-1个字符串;

有序集合和列表的共同点和区别？
###### 共同点
- 两者均有序；
- 两者均可获得某一范围的元素

##### 不同点
- 列表是通过链表来实现的，获取靠近两端的数据速度极快，而当元素增多后，访问中间速度的速度会较慢，常用场景，适合实现如“新鲜事”、“日志”这样访问中间元素少的应用；
- 有序集合是通过散列表和跳跃表来实现的，所以读取位于中间部分的数据速度也很快(时间复杂度是O(logN))
- 列表不能简单地调整某个元素的位置，但有序集合可以，通过改变这个元素的分数
- 有序集合比列表更消耗内存

