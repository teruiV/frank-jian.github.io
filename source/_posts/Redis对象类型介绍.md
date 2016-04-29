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
