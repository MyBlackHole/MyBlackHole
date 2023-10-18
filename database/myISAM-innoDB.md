# MyISAM与InnoDB 的区别
1. InnoDB支持事务，MyISAM不支持，对于InnoDB每一条SQL语言都默认封装成事务，自动提交，这样会影响速度，所以最好把多条SQL语言放在begin和commit之间，组成一个事务
2. InnoDB支持外键，而MyISAM不支持。对一个包含外键的InnoDB表转为MYISAM会失败；  
3. InnoDB是聚集索引，使用B+Tree作为索引结构，数据文件是和（主键）索引绑在一起的（表数据文件本身就是按B+Tree组织的一个索引结构），必须要有主键，通过主键索引效率很高。但是辅助索引需要两次查询，先查询到主键，然后再通过主键查询到数据。因此，主键不应该过大，因为主键太大，其他索引也都会很大。 
   MyISAM是非聚集索引，也是使用B+Tree作为索引结构，索引和数据文件是分离的，索引保存的是数据文件的指针。主键索引和辅助索引是独立的
   也就是说：InnoDB的B+树主键索引的叶子节点就是数据文件，辅助索引的叶子节点是主键的值；而MyISAM的B+树主键索引和辅助索引的叶子节点都是数据文件的地址指针
4. InnoDB不保存表的具体行数，执行select count(*) from table时需要全表扫描。而MyISAM用一个变量保存了整个表的行数，执行上述语句时只需要读出该变量即可，速度很快(注意不能加有任何WHERE条件)
   那么为什么InnoDB没有了这个变量呢？ 
   因为InnoDB的事务特性，在同一时刻表中的行数对于不同的事务而言是不一样的，因此count统计会计算对于当前事务而言可以统计到的行数，而不是将总行数储存起来方便快速查询。InnoDB会尝试遍历一个尽可能小的索引除非优化器提示使用别的索引。如果二级索引不存在，InnoDB还会尝试去遍历其他聚簇索引
   如果索引并没有完全处于InnoDB维护的缓冲区（Buffer Pool）中，count操作会比较费时。可以建立一个记录总行数的表并让你的程序在INSERT/DELETE时更新对应的数据。和上面提到的问题一样，如果此时存在多个事务的话这种方案也不太好用。如果得到大致的行数值已经足够满足需求可以尝试SHOW TABLE STATUS
5. Innodb不支持全文索引，而MyISAM支持全文索引，在涉及全文索引领域的查询效率上MyISAM速度更快高；PS：5.7以后的InnoDB支持全文索引了 
6. MyISAM表格可以被压缩后进行查询操作 
7. InnoDB支持表、行(默认)级锁，而MyISAM支持表级锁 
   InnoDB的行锁是实现在索引上的，而不是锁在物理行记录上。潜台词是，如果访问没有命中索引，也无法使用行锁，将要退化为表锁
例如： 
   t_user(uid, uname, age, sex) innodb; 
   uid PK 
   无其他索引 
   update t_user set age=10 where uid=1;             命中索引，行锁。 
   update t_user set age=10 where uid != 1;          未命中索引，表锁。 
   update t_user set age=10 where name='chackca';    无索引，表锁。 
8. InnoDB表必须有主键（用户没有指定的话会自己找或生产一个主键），而Myisam可以没有 
9. Innodb存储文件有frm、ibd，而Myisam是frm、MYD、MYI 
        Innodb：frm是表定义文件，ibd是数据文件 
        Myisam：frm是表定义文件，myd是数据文件，myi是索引文件 
如何选择： 
    1. 是否要支持事务，如果要请选择innodb，如果不需要可以考虑MyISAM； 
    2. 如果表中绝大多数都只是读查询，可以考虑MyISAM，如果既有读也有写，请使用InnoDB。 
    3. 系统奔溃后，MyISAM恢复起来更困难，能否接受； 
    4. MySQL5.5版本开始Innodb已经成为Mysql的默认引擎(之前是MyISAM)，说明其优势是有目共睹的，如果你不知道用什么，那就用InnoDB，至少不会差。 
InnoDB为什么推荐使用自增ID作为主键？ 
    答：自增ID可以保证每次插入时B+索引是从右边扩展的，可以避免B+树和频繁合并和分裂（对比使用UUID）。如果使用字符串主键和随机主键，会使得数据随机插入，效率比较差
innodb引擎的4大特性 
       插入缓冲（insert buffer),二次写(double write),自适应哈希索引(ahi),预读(read ahead) 
