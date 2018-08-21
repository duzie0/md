# Redis

Redis非关系型数据库

Redis功能齐全，但在突然终止或电源故障时更容易丢失数据

Redis是单线程的，内存数据库

NoSql：不需要经过SQL层处理，性能高

可扩展性：键值对的形式 ，水平扩展非常的容易

## Redis数据类型

字符串 string 

哈希 hash

列表 list

集合 set

有序集合 zset

redis数据库 有16个库

## Redis key 操作

**del key:**

删除给定的一个或多个Key(多个key用空格隔开),删除成功返回1，当key不存在时，返回0；

**dump key:**

序列化给定的key，并返回被序列化的值，使用restore可以反序列化；

**exists key:**

检查key是否存在，若key存在返回1，否则返回0；

**expire key seconds:**

为key设置超时时间（单位：秒），当key过期时，会被系统自动删除；

**expireat key timeamp:**

为key设置超时时间（单位：秒），和expire不同的是其接受的时间参数是Unix时间戳；例：expireat foo 1355292000。

**key pattern:**

查找所有符合给定模式的key，通常用于查找key；例：keys* 匹配所有的key、keys; h?llo匹配hello、hallo；h*llo匹配hallo和heeeello；h[ea]llo匹配hello和hallo，但不匹配hillo。

**migrate host port key destination-db timeout [copy]\[replace]:**

将key原子性的传送到指定数据库上，传送成功后，本地的key会被删除；例：migrate 127.0.0.1 6340 foo 0 100。

**move key db**

　　将key移动到指定数据库中;例：move foo 1，将foo从db0移动到db1，移动陈工返回1，移动失败返回0。

**object subcommand[agruments[arguments]]**

　　object允许从内部查看给定Key的Redis对象，通常用于拍错或节省空间而对key使用特殊编码：

- OBJECT REFCOUNT <key> 返回给定 key 引用所储存的值的次数。此命令主要用于除错；例：object refcount foo。
- OBJECT ENCODING <key> 返回给定 key 锁储存的值所使用的内部表示(representation)；例：object encoding foo。
- OBJECT IDLETIME <key> 返回给定 key 自储存以来的空转时间(idle， 没有被读取也没有被写入)，以秒为单位；例：object idletime foo。

**persist key**

　　移除指定key的生存时间，成功返回1，若key不存在或不存在生存时间时返回0；例：persist foo。

**pexpire key millseconds**

　　这个命令和expire类似，但是它是以毫秒为单位设置key的生存时间，而不像expire命令那样，以秒为单位；例：pexpire foo 30000。

**pexpireat key millseconds-timestamp**

　　这个命令和expireat类似，但是它是以毫秒为单位设置key的生存时间，而不像expireat命令那样，以秒为单位；例:pexpireat foo 1478007195543

**pttl key**

　　这个命令和ttl类似，它以毫秒为单位返回key的剩余生存时间，而不像ttl命令那样，以秒为单位；当key不存在时返回-2，当key存在但没有设置剩余时间时返回-1；例：pptl foo。

**randomkey**

　　从数据库中随机返回一个key，若数据库为空则返回nil；例:randomkey。

**rename key newkey**

　　为key改名，当key和newkey相同或者key不存在时返回一个错误，当newkey已存在时则会覆盖newkey中的值；例：rename foo foo1。

**renamenx key newkey**

　　当且仅当newkey不存在时将key改为newkey，若key不存在返回错误，若newkey存在返回0；例：renamex foo foo1。

**restore key ttl serialized-value**

　　反序列化给定的序列化值，并将它和给定的key关联；例： RESTORE greeting-again 0 "\x00\x15hello, dumping world!\x06\x00E\xa0Z\x82\xd8r\xc1\xde"（看别人的例子吧，说实话到现在除了做迁移验证兼容性之外我还没想到它的其他用处）。

**sort key [by pattern]\[limit offset count] [get pattern [get pattern ...]]\[asc（default） | desc]\[alpha]\[store destination]**

　　返回或保存给定列表、集合、有序集合中经过排序的元素：
　　最常用用法：sort key返回从小到大的排列；例：lpush day 1 2 3 5 79 18 | sort day。
　　sort默认排序对象为数字，当需要对字符串进行排序时，需要显示的在sort命令之后添加alpha修饰符；例：lpush name tom lilei hanmeimei unclewang| sort name alpha。
　　排序后返回元素的数量可以通过limit修饰符进行限制，修饰符接受offset和count两个参数；例：lpush day 1 2 3 5 79 18 | sort day limit 1 3返回2、3、5三个元素。
　　还有一些更为详细的用法请参见：[http://doc.redisfans.com/key/sort.html]\(http://doc.redisfans.com/key/sort.html)。

**ttl key**

　　以秒为单位返回key的剩余生存时间（time to live），当key不存在时返回-2，当key存在未设置生存时间时返回-1；例：ttl foo。

**type key**

　　返回key存储的值的类型，返回none（不存在）、string（字符串）、list（列表）、set（集合）、zset（有序集合）、hash（哈希表）；例：type foo。

## Redis list 操作