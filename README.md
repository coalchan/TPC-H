# TPC-H

## 资源

1. TPC-H from http://www.tpc.org/tpch/ version: 2.18.0 been uploaded to [baidu pan(code: cnyx)](https://pan.baidu.com/s/1ydpAsj00zwt6qX31mtB_Cw)
2. TPC-H_on_Hive by Yuntao Jia: https://issues.apache.org/jira/browse/HIVE-600


## 操作

1. cd dbgen && cp makefile.suite makefile

2. 修改 makefile 108 行

`vi makefile`

```
################
## CHANGE NAME OF ANSI COMPILER HERE
################
CC      = gcc
# Current values for DATABASE are: INFORMIX, DB2, TDAT (Teradata)
#                                  SQLSERVER, SYBASE, ORACLE, VECTORWISE
# Current values for MACHINE are:  ATT, DOS, HP, IBM, ICL, MVS,
#                                  SGI, SUN, U2200, VMS, LINUX, WIN32
# Current values for WORKLOAD are:  TPCH
DATABASE= HIVE
MACHINE = LINUX
WORKLOAD = TPCH
```

3. 在 tpcd.h 增加 Hive 对应方式

`vi tpcd.h`

```
#ifdef HIVE
#define GEN_QUERY_PLAN "EXPLAIN"
#define START_TRAN "START TRANSACTION"
#define END_TRAN "COMMIT"
#define SET_OUTPUT ""
#define SET_ROWCOUNT "limit %d;\n"
#define SET_DBASE "use %s;\n"
#endif
```

3. 执行 make 命令

`make`

4. 生成数据

`./dbgen -s 1`

说明：-s 代表数据规模因子，1 代表产生 1G 的数据量，在当前文件夹下生成的8个 `.tbl` 文件就是我们所需要的数据。

## 问题

1. 在 macos 上出现 fatal error: 'malloc.h' file not found

`#include <malloc.h>` 改为 `#include <sys/malloc.h>`

