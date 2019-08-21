## mysql的数据库引擎



> 使用了数据库这么久了，对于数据库引擎不同类型还不是真的很了解，就做了一个总结。
>
> 本文只是个人观点和借鉴部分资料与网络资源

### 一、数据库引擎

数据库引擎是用于存储、处理和保护数据的核心服务。利用数据库引擎可控制访问权限并快速处理事务，从而满足企业内大多数需要处理大量数据的应用程序的要求。 使用数据库引擎创建用于[联机事务处理](https://baike.baidu.com/item/联机事务处理/218843)或联机分析处理数据的关系数据库。这包括创建用于存储数据的表和用于查看、管理和保护数据安全的[数据库对象](https://baike.baidu.com/item/数据库对象/927632)（如索引、视图和[存储过程](https://baike.baidu.com/item/存储过程/1240317)）。可以使用 SQL Server Management Studio 管理数据库对象，使用 SQL Server Profiler 捕获服务器事件。



### 二、常见的数据库引擎

1.**InnoDB存储引擎**（默认）

2.**MyISAM存储引擎**

3.**MEMORY存储引擎**

4.**ARCHIVE存储引擎**



### 三、数据库引擎的特点

#### InnoDB主要特性

InnoDB是事务型数据库的首选引擎，支持事务安全表（ACID），支持行锁定和外键，InnoDB是默认的MySQL引擎。

1、InnoDB是具有提交、回滚和崩溃恢复能力的事物安全（ACID）存储引擎。InnoDB锁定在行级并且也在SELECT语句中提供一个类似Oracle的非锁定读。这些功能增加了多用户部署和性能。在SQL查询中，可以自由地将InnoDB类型的表和其他MySQL的表类型连接，甚至在同一个查询中也可以连接。

2、InnoDB是为处理海量数据而设计。它的CPU效率高于其他基于磁盘的关系型数据库引擎锁

3、InnoDB存储引擎完全与MySQL服务器整合，InnoDB存储引擎为在主内存中缓存数据和索引而维持它自己的缓冲池。InnoDB将它的表和索引在一个逻辑表空间中，表空间可以包含数个文件（或原始磁盘文件）。这与MyISAM表不同，比如在MyISAM表中每个表被存放在分离的文件中。InnoDB表可以是任何尺寸，即使在文件尺寸被限制为2GB的操作系统上

4、InnoDB支持外键完整性约束，存储表中的数据时，每张表的存储都按主键顺序存放，如果没有显示在表定义时指定主键，InnoDB会为每一行生成一个6字节的ROWID，并以此作为主键

5、InnoDB被用在众多需要高性能的大型数据库站点上

InnoDB不创建目录，使用InnoDB时，MySQL将在MySQL数据目录下创建一个名为ibdata1的10MB大小的自动扩展数据文件，以及两个名为ib_logfile0和ib_logfile1的5MB大小的日志文件

#### **MyISAM存储引擎**

MyISAM基于ISAM存储引擎，并对其进行扩展。它是在Web、数据仓储和其他应用环境下最常使用的存储引擎之一。MyISAM拥有较高的插入、查询速度，但**不支持事物**。MyISAM主要特性有：

1、大文件（达到63位文件长度）在支持大文件的文件系统和操作系统上被支持

2、当把删除和更新及插入操作混合使用的时候，动态尺寸的行产生更少碎片。这要通过合并相邻被删除的块，以及若下一个块被删除，就扩展到下一块自动完成

3、每个MyISAM表最大索引数是64，这可以通过重新编译来改变。每个索引最大的列数是16

4、最大的键长度是1000字节，这也可以通过编译来改变，对于键长度超过250字节的情况，一个超过1024字节的键将被用上

5、BLOB和TEXT列可以被索引

6、NULL被允许在索引的列中，这个值占每个键的0~1个字节

7、所有数字键值以高字节优先被存储以允许一个更高的索引压缩

8、每个MyISAM类型的表都有一个AUTO_INCREMENT的内部列，当INSERT和UPDATE操作的时候该列被更新，同时AUTO_INCREMENT列将被刷新。所以说，MyISAM类型表的AUTO_INCREMENT列更新比InnoDB类型的AUTO_INCREMENT更快

9、可以把数据文件和索引文件放在不同目录

10、每个字符列可以有不同的字符集

11、有VARCHAR的表可以固定或动态记录长度

12、VARCHAR和CHAR列可以多达64KB

使用MyISAM引擎创建数据库，将产生3个文件。文件的名字以表名字开始，扩展名之处文件类型：frm文件存储表定义、数据文件的扩展名为.MYD（MYData）、索引文件的扩展名时.MYI（MYIndex）

#### **MEMORY存储引擎**

MEMORY存储引擎将表中的数据存储到内存中，未查询和引用其他表数据提供快速访问。MEMORY主要特性有：

1、MEMORY表的每个表可以有多达32个索引，每个索引16列，以及500字节的最大键长度

2、MEMORY存储引擎执行HASH和BTREE缩影

3、可以在一个MEMORY表中有非唯一键值

4、MEMORY表使用一个固定的记录长度格式

5、MEMORY不支持BLOB或TEXT列

6、MEMORY支持AUTO_INCREMENT列和对可包含NULL值的列的索引

7、MEMORY表在所由客户端之间共享（就像其他任何非TEMPORARY表）

8、MEMORY表内存被存储在内存中，内存是MEMORY表和服务器在查询处理时的空闲中，创建的内部表共享

9、当不再需要MEMORY表的内容时，要释放被MEMORY表使用的内存，应该执行DELETE FROM或TRUNCATE TABLE，或者删除整个表（使用DROP TABLE）



#### ARCHIVE存储引擎

1.archive存储引擎支持insert、replace和select操作，但是不支持update和delete。

2.archive存储引擎支持blob、text等大字段类型。支持auto_increment自增列同时自增列可以不是唯一索引。

3.archive支持auto_increment列，但是不支持往auto_increment列插入一个小于当前最大的值的值。

4.archive不支持索引所以无法在archive表上创建主键、唯一索引、和一般的索引。

5.archive表插入的数据会经过压缩，archive使用zlib进行数据压缩，archive支持optimize table、 check table操作。insert语句仅仅往压缩缓存中插入数据，插入的数据在压缩缓存中被锁定，当select操作时会触发压缩缓存中的数据进行刷新。insert delay除外。对于一个bulk insert操作只有当它完全执行完才能看到记录，除非在同一时刻还有其它的inserts操作，在这种情况下可以看到部分记录，select从不刷新bulk insert除非在它加载时存在一般的Insert操作。

6.检索请求返回的行不会压缩，且不会进行数据缓存；一个select查询会执行完整的表扫描；当一个select查询发生时它查找当前表所有有效的行，select执行一致性读操作，注意，过多的select查询语句会导致压缩插入性能变的恶化，除非使用bulk insert或delay insert，可以使用OPTIMIZE TABLE 或REPAIR TABLE来获取更好的压缩，可以使用SHOW TABLES STATUS查看ARCHIVE表的记录行。



四、总结

> 百度找的一张图片

![](https://upload-images.jianshu.io/upload_images/11464886-82267cb5926d26fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)