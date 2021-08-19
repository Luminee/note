## Usage

```shell
--aliyun-rds:是否在阿里云数据库上执行。true
--allow-master-master:是否允许gh-ost运行在双主复制架构中，一般与-assume-master-host参数一起使用
--allow-nullable-unique-key:允许gh-ost在数据迁移依赖的唯一键可以为NULL，默认为不允许为NULL的唯一键。如果数据迁移(migrate)依赖的唯一键允许NULL值，则可能造成数据不正确，请谨慎使用。
--allow-on-master:允许gh-ost直接运行在主库上。默认gh-ost连接的从库。
--alter string:DDL语句
--approve-renamed-columns ALTER:如果你修改一个列的名字，gh-ost将会识别到并且需要提供重命名列名的原因，默认情况下gh-ost是不继续执行的，除非提供-approve-renamed-columns ALTER。
--ask-pass:MySQL密码
--assume-master-host string:为gh-ost指定一个主库，格式为”ip:port”或者”hostname:port”。在这主主架构里比较有用，或则在gh-ost发现不到主的时候有用。
--assume-rbr:确认gh-ost连接的数据库实例的binlog_format=ROW的情况下，可以指定-assume-rbr，这样可以禁止从库上运行stop slave,start slave,执行gh-ost用户也不需要SUPER权限。
--check-flag
--chunk-size int:在每次迭代中处理的行数量(允许范围：100-100000)，默认值为1000。
--concurrent-rowcount:该参数如果为True(默认值)，则进行row-copy之后，估算统计行数(使用explain select count(*)方式)，并调整ETA时间，否则，gh-ost首先预估统计行数，然后开始row-copy。
--conf string:gh-ost的配置文件路径。
--critical-load string:一系列逗号分隔的status-name=values组成，当MySQL中status超过对应的values，gh-ost将会退出。-critical-load Threads_connected=20,Connections=1500，指的是当MySQL中的状态值Threads_connected>20,Connections>1500的时候，gh-ost将会由于该数据库严重负载而停止并退出。
--critical-load-hibernate-seconds int :负载达到critical-load时，gh-ost在指定的时间内进入休眠状态。 它不会读/写任何来自任何服务器的任何内容。
--critical-load-interval-millis int:当值为0时，当达到-critical-load，gh-ost立即退出。当值不为0时，当达到-critical-load，gh-ost会在-critical-load-interval-millis秒数后，再次进行检查，再次检查依旧达到-critical-load，gh-ost将会退出。
--cut-over string:选择cut-over类型:atomic/two-step，atomic(默认)类型的cut-over是github的算法，two-step采用的是facebook-OSC的算法。
--cut-over-exponential-backoff
--cut-over-lock-timeout-seconds int:gh-ost在cut-over阶段最大的锁等待时间，当锁超时时，gh-ost的cut-over将重试。(默认值：3)
--database string:数据库名称。
--debug:debug模式。
--default-retries int:各种操作在panic前重试次数。(默认为60)
--discard-foreign-keys:该参数针对一个有外键的表，在gh-ost创建ghost表时，并不会为ghost表创建外键。该参数很适合用于删除外键，除此之外，请谨慎使用。
--dml-batch-size int:在单个事务中应用DML事件的批量大小（范围1-100）（默认值为10）
--exact-rowcount:准确统计表行数(使用select count(*)的方式)，得到更准确的预估时间。
--execute:实际执行alter&migrate表，默认为noop，不执行，仅仅做测试并退出，如果想要ALTER TABLE语句真正落实到数据库中去，需要明确指定-execute
--exponential-backoff-max-interval int
--force-named-cut-over:如果为true，则'unpostpone | cut-over'交互式命令必须命名迁移的表
--force-table-names string:在临时表上使用的表名前缀
--heartbeat-interval-millis int:gh-ost心跳频率值，默认为500
--help
--hooks-hint string:任意消息通过GH_OST_HOOKS_HINT注入到钩子
--hooks-path string:hook文件存放目录(默认为empty，即禁用hook)。hook会在这个目录下寻找符合约定命名的hook文件来执行。
--host string :MySQL IP/hostname
--initially-drop-ghost-table:gh-ost操作之前，检查并删除已经存在的ghost表。该参数不建议使用，请手动处理原来存在的ghost表。默认不启用该参数，gh-ost直接退出操作。
--initially-drop-old-table:gh-ost操作之前，检查并删除已经存在的旧表。该参数不建议使用，请手动处理原来存在的ghost表。默认不启用该参数，gh-ost直接退出操作。
--initially-drop-socket-file:gh-ost强制删除已经存在的socket文件。该参数不建议使用，可能会删除一个正在运行的gh-ost程序，导致DDL失败。
--master-password string :MySQL 主密码
--master-user string:MysQL主账号
--max-lag-millis int:主从复制最大延迟时间，当主从复制延迟时间超过该值后，gh-ost将采取节流(throttle)措施，默认值：1500s。
--max-load string:逗号分隔状态名称=阈值，如：'Threads_running=100,Threads_connected=500'. When status exceeds threshold, app throttles writes
--migrate-on-replica:gh-ost的数据迁移(migrate)运行在从库上，而不是主库上。 
--nice-ratio float:每次chunk时间段的休眠时间，范围[0.0…100.0]。0：每个chunk时间段不休眠，即一个chunk接着一个chunk执行；1：每row-copy 1毫秒，则另外休眠1毫秒；0.7：每row-copy 10毫秒，则另外休眠7毫秒。
--ok-to-drop-table:gh-ost操作结束后，删除旧表，默认状态是不删除旧表，会存在_tablename_del表。
--panic-flag-file string:当这个文件被创建，gh-ost将会立即退出。
--password string :MySQL密码
--port int ：MySQL端口，最好用从库
--postpone-cut-over-flag-file string：当这个文件存在的时候，gh-ost的cut-over阶段将会被推迟，数据仍然在复制，直到该文件被删除。
--quiet：静默模式。
--replica-server-id uint : gh-ost的server_id
--replication-lag-query string:弃用
--serve-socket-file string：gh-ost的socket文件绝对路径。
--serve-tcp-port int:gh-ost使用端口，默认为关闭端口。
--skip-foreign-key-checks:确定你的表上没有外键时，设置为'true'，并且希望跳过gh-ost验证的时间-skip-renamed-columns ALTER
--skip-renamed-columns ALTER：如果你修改一个列的名字(如change column)，gh-ost将会识别到并且需要提供重命名列名的原因，默认情况下gh-ost是不继续执行的。该参数告诉gh-ost跳该列的数据迁移，让gh-ost把重命名列作为无关紧要的列。该操作很危险，你会损失该列的所有值。
--stack:添加错误堆栈追踪。
--switch-to-rbr:让gh-ost自动将从库的binlog_format转换为ROW格式。
--table string:表名
--test-on-replica：在从库上测试gh-ost，包括在从库上数据迁移(migration)，数据迁移完成后stop slave，原表和ghost表立刻交换而后立刻交换回来。继续保持stop slave，使你可以对比两张表。
--test-on-replica-skip-replica-stop:当-test-on-replica执行时，该参数表示该过程中不用stop slave。
--throttle-additional-flag-file string:当该文件被创建后，gh-ost操作立即停止。该参数可以用在多个gh-ost同时操作的时候，创建一个文件，让所有的gh-ost操作停止，或者删除这个文件，让所有的gh-ost操作恢复。
--throttle-control-replicas string:列出所有需要被检查主从复制延迟的从库。
--throttle-flag-file string:当该文件被创建后，gh-ost操作立即停止。该参数适合控制单个gh-ost操作。-throttle-additional-flag-file string适合控制多个gh-ost操作。
--throttle-http string
--throttle-query string:节流查询。每秒钟执行一次。当返回值=0时不需要节流，当返回值>0时，需要执行节流操作。该查询会在数据迁移(migrated)服务器上操作，所以请确保该查询是轻量级的。
--timestamp-old-table:在旧表名中使用时间戳。 这会使旧表名称具有唯一且无冲突的交叉迁移
--tungsten：告诉gh-ost你正在运行的是一个tungsten-replication拓扑结构。
--user string :MYSQL用户
--verbose
--version
```

```shell
$ gh-ost --user="root" --password="root" --host=192.168.163.131  --database="test" --table="t1"  --alter="ADD COLUMN cc2 varchar(10),add column cc3 int not null default 0 comment 'test' " --allow-on-master  --execute

$ gh-ost --user="root" --password="root" --host=192.168.163.131  --database="test" --table="t1"  --alter="ADD COLUMN o2 varchar(10),add column o1 int not null default 0 comment 'test' " --exact-rowcount --serve-socket-file=/tmp/gh-ost.t1.sock --panic-flag-file=/tmp/gh-ost.panic.t1.flag  --postpone-cut-over-flag-file=/tmp/ghost.postpone.t1.flag --allow-on-master  --execute
```

