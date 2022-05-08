---
title: 关于Mybatis二级缓存
catalog: true
date: 2022-04-12 02:34:17
subtitle: 
lang: cn
header-img: /img/header_img/lml_bg.jpg
tags:
- Mybatis
categories:
- Mybatis
---

###### Mybatis中有两级缓存，缓存的目的就是不让每一次数据库查询都到数据库进行，减少数据库的访问与交互，提高了数据的检索效率

#### 1.一级缓存

一级缓存是SqlSession级别的一个缓存，也就是用户会话级别的，一个用户都需要创建一个SqlSession来执行，一级缓存也叫本地缓存，mybatis会将查询成功获得的数据缓存到一级缓存中，后续的Sql在命中缓存的情况下，直接从本地缓存读

##### 2.二级缓存（跨SqlSession的缓存）

![开启二级缓存](Snipaste_2022-04-22_18-42-22.png)

只要有一个用户的SqlSession拿到了数据，就会将其存入到二级缓存中

#### 一二级缓存的实现原理

先看一级缓存：在SqlSession中会支持有一个Executor，每一个Executor又有一个LocalCache的对象，当用户发起请求的时候，Mybatis根据语句到localCache中进行查询，当然命中了就会返回数据，没有命中再到数据库中进行查询，查询完之后将其存入到SqlSessiond的LocalCache中，所以一级缓存的生命周期是SqlSession的生命周期，注意在多线程情况下一级缓存可能产生脏读。

![一二级缓存示意图](Snipaste_2022-04-22_18-49-58.png)

再看二级缓存（全局缓存）：在原来的Executor上加了一个装饰，引入了一个装饰器叫CachingExecutor，在进入一级缓存之前先进行二级缓存的查询（通过上面的装饰器），最后进行数据库的查询，缓存粒度控制在了nameSpace命名空间的级别，并且可以通过Cache这个接口实现不同缓存实现类的一个组合，提高对Cache的一个可控性。

![二级缓存实现图](Snipaste_2022-04-22_18-32-39.png)