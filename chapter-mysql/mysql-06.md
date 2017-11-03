## MySQL导出和导入数据库

mysqldump提供的是逻辑备份，MySQL的企业版可以做到物理备份。

假设我们在星期日下午1点的时候，运行如下备份命令

```
mysqldump --all-databases --master-data --single-transaction > backup_sunday_1_PM.sql
```

这个备份操作在开始时，会获取一个在所有表上的全局读锁。一旦获取到了这个锁，`the binary log coordinates are read and the lock is released`。如果刚好有一个更新操作，备份操作将会等待知道这个操作完成。之后备份就会释放锁，并不会影响到表的读写。

一开始我们假设备份InnoDB，所以`--single-transaction`使用了一个一致读，保证了mysqldump看到的数据不会改变。（mysqldump过程中，其它客户端对InnoDB的修改不会被看到）。如果这个备份操作包含非事务型表格，一致性要求它们在备份过程中不会改变。例如，对于MyISAM表，在备份过程中，不能有修改。

完整的备份是必要的，但是不太方便。这会生成很大的备份文件，并需要花费一些时间来生成。如果能有一个初始备份，以后的备份在它的基础上做增量备份，会是一个不错的选择。增量备份比较小，并且只需要花费很少的时间。但代价是，在恢复的时候，你不能只是重新加载完整备份，也要处理一个个的增量备份。

为了制作增量备份，我们需要保存增量修改。在MySQL中，这些修改被放在了binlog中。所以MySQL服务器在启动的时候应该带有`--log-bin`选项，从而启动日志。启动了binlog以后，服务器的每一次修改都会记录到binlog中。

MySQL每次重启，都会创建一个新的binlog文件。当服务器在运行时，你也可以告诉MySQL关闭当前的binlog文件，并开始一个新的。参考命令`mysqladmin flush-logs`。

MySQL的binlog文件对于恢复非常重要，因为他们构成了增量备份。如果你确信在做全量备份的时候，flush了一下日志，之后创建的binlog文件包含了所有的修改。修改先前的`mysqldump`命令，在做全量备份的时候flush一下日志，这样dump文件会包含新的binlog文件的名称

```
mysqldump --single-transaction --flush-logs --master-data=2 --all-databases > backup_sunday_1_PM.sql
```

执行过上述命令以后，数据目录包含了一个新的binlog文件，gbichot2-bin.000007，因为`--flush-logs`选项使服务器flush了它的日志。`--master-data`选项使mysqldump将binlog信息输出到它的输出中，所以备份文件会包含如下内容，它告诉了你哪个binlog日志是在备份之后开始生成的：

```
-- Position to start replication or point-in-time recovery from
-- CHANGE MASTER TO MASTER_LOG_FILE='gbichot2-bin.000007',MASTER_LOG_POS=4;
```



### 参考

- [https://dev.mysql.com/doc/refman/5.7/en/backup-and-recovery.html](https://dev.mysql.com/doc/refman/5.7/en/backup-and-recovery.html)