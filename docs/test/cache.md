#  <!-- {docsify-ignore} -->
有时候我们在做业务测试，除了验证服务器返回数据的正确性。还需要考虑业务中使用到的缓存地方，其数据的正确性。

## 缓存的位置

浏览器端有本地缓存、网络端有CDN 缓存、数据中心前端有反向代理的缓存、应用服务器端有本地缓存、数据库缓存

### 浏览器端的本地缓存

静态资源，当重复访问一些页面时，浏览器就可以从缓存中直接使用已有的资源副本，而不需要每次向 web 服务器请求资源。

### 网络端的 CDN 缓存

CDN (content delivery network 内容分发网络)采用各种缓存服务器，将这些缓存器分布到用户访问相对集中的地区的网络供应商机房内。

### 反向代理服务器的缓存

一个范围内可以共享该链接的缓存，类似于一个更高级的客户端。对重复的请求可以直接将其数据进行返回，不用直接请求服务器端。

### 服务器端的本地缓存

服务器更新时，当前本地的缓存信息滞后。需设置缓存过期时间，当缓存失效，客户端需要到服务器端获取文件；客户端请求时会带上该参数，服务器会根据该参数来确认是否让客户端直接获取本地缓存。如果数据未更新直接返回原有的缓存数据，对于返回状态码为 304。

```java
304 Not Modified 表示客户端发送附带条件的请求，服务器允许请求访问资源，返回的资源未满足请求的条件，用于页面未修改
```

### 数据库缓存

部分网站使用 Redis 缓存技术。

## 缓存带来的好处

页面性能方面：提高重复访问页面的性能，并减少 web 服务器的负载。

## 缓存可能带来的问题

- 两个并发操作，一个是更新操作，另一个是查询操作，更新操作删除缓存后，查询操作没有命中缓存，先把老数据读出来后放到缓存中，然后更新操作更新了数据库。导致缓存中的数据还是老的数据。
- 缓存穿透、缓存雪崩、缓存击穿引起的服务器压力。

## 缓存测试

### 浏览器缓存

分为缓存协商和强缓存。

缓存协商：从缓存取，不发送请求到服务器，通过服务器来告知缓存是否可用。用于频繁变动的内容。

强缓存：从缓存取，不发送请求到服务器，直接从缓存取。用于不常变化的内容。缓存位置在内存资源和磁盘中。按刷新(F5)如果匹配的话优先使用内存中的缓存，其次才是磁盘最后才是发送请求到服务器。

代码更新到线上后用户浏览器不能自行更新，在请求的 URL 中增加一个版本参数，更新后参数变化的时候，强缓存都会失效并重新加载。

### Redis 缓存测试

- 功能测试

    1.redis 数据生效时，读取是否正确

    2.redis 数据不存在，能否正常从 db 中读取到正确的值，并正确写入Redis和返回给上层

    3.数据在 redis 和 db 中都不存在时的表现是否正常

    4.删除数据时，redis 和 db 的数据是否一致

- 接口测试      
    接口自动化中，可以使用 Jedis。它是 Redis 的 Java 客户端。用来对数据库数据进行验证。

    1.先定义 redis 连接类，定义增删改查的方法

    2.对于修改删除的方法对 redis 添加主从实例判断，防止命中从实例而无法修改。

- 性能测试

    1.同一时间大量请求“缓存中没有”且“数据库中也没有”的数据（缓存穿透）

    2.同一时间大量请求”缓存中没有“且”数据库中有”的数据（缓存雪崩）

    3.同一时间大量请求“缓存中有”的数据（缓存击穿）

- Redis 集群缓存

    1.缓存数据的读取和写入有没有进入正确的分片

    2.缓存与数据库的数据一致性检测

    3.DB 事务性导致回滚，缓存是否回滚，有没有产生脏数据

    4.扩容后分片策略的变化、热点数据失效率和命中率对后端数据库带来的压力

