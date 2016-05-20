---
title: Redis之集合并集、交集、差集
date: 2016-05-09 16:52:36
tags: [Redis]
categories: Redis
---
###### Redis交集、并集、差集应用场景
- 微博的共同和非共同好友显示;

###### 实战操作（备忘....）
```
# 创建集合
SADD priceA 1 2 3 4 5
SADD priceB 1 2 3 7 8

# 显示集合所有元素
SMEMBERS priceA
SMEMBERS priceB

# 交集
SINTER priceA priceB  //结果 1 2 3

# 差集
SDIFF priceA priceB   //结果 4 5

# 并集
SUNION priceA priceB  //结果 1 2 3 4 5 7 8
```
