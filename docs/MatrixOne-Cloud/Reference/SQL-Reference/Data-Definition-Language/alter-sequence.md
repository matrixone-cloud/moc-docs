# **ALTER SEQUENCE**

## **语法说明**

`ALTER SEQUENCE` 用于修改现有序列。

## **语法结构**

```
> ALTER SEQUENCE [ IF EXISTS ] SEQUENCE_NAME
[ AS data_type ]
[ INCREMENT [ BY ] increment ]
[ MINVALUE minvalue] [ MAXVALUE maxvalue]
[ START [ WITH ] start ] [ [ NO ] CYCLE ]
```

### 语法释义

- `[ IF EXISTS ]`：可选的子句，表示如果指定的序列不存在，也不会引发错误。如果使用了此子句，系统将检查序列是否存在，如果不存在，将忽略修改请求。

- `SEQUENCE_NAME`：要修改的序列的名称。

- `[ AS data_type ]`：可选子句，它允许您为序列指定数据类型。通常，序列的数据类型是整数。

- `[ INCREMENT [ BY ] increment ]`：这是指定序列的增量值。序列的增量值是在每次递增或递减时要添加到当前值的数量。如果未指定增量值，通常默认为 1。

- `[ MINVALUE minvalue ]`：这是序列的最小值，它指定了序列允许的最小值。如果指定了最小值，序列的当前值不能低于此值。

- `[ MAXVALUE maxvalue ]`：这是序列的最大值，它指定了序列允许的最大值。如果指定了最大值，序列的当前值不能超过此值。

- `[ START [ WITH ] start ]`：这是序列的起始值，它指定序列的初始值。如果未指定起始值，通常默认为 1。

- `[ [ NO ] CYCLE ]`：可选子句，用于指定是否循环使用序列值。如果指定了 `NO CYCLE`，则在达到最大值或最小值后，序列将停止递增或递减。如果未指定此子句，通常默认为不循环。

## **示例**

```sql
-- 创建一个名为 alter_seq_01 的序列，将序列的增量设置为 2，设置序列的最小值为 30，最大值为 100，并启用循环
create sequence alter_seq_01 as smallint increment by 2 minvalue 30 maxvalue 100 cycle;

mysql> show sequences;
+--------------+-----------+
| Names        | Data Type |
+--------------+-----------+
| alter_seq_01 | SMALLINT  |
+--------------+-----------+
1 row in set (0.00 sec)

mysql> alter sequence alter_seq_01 as bigint;
Query OK, 0 rows affected (0.01 sec)

mysql> show sequences;
+--------------+-----------+
| Names        | Data Type |
+--------------+-----------+
| alter_seq_01 | BIGINT    |
+--------------+-----------+
1 row in set (0.00 sec)

-- 取消序列 alter_seq_01 的循环
mysql> alter sequence alter_seq_01 no cycle;
Query OK, 0 rows affected (0.01 sec)

mysql> select nextval('alter_seq_01'),currval('alter_seq_01');
+-----------------------+-----------------------+
| nextval(alter_seq_01) | currval(alter_seq_01) |
+-----------------------+-----------------------+
| 30                    | 30                    |
+-----------------------+-----------------------+
1 row in set (0.01 sec)

mysql> select nextval('alter_seq_01'),currval('alter_seq_01');
+-----------------------+-----------------------+
| nextval(alter_seq_01) | currval(alter_seq_01) |
+-----------------------+-----------------------+
| 32                    | 32                    |
+-----------------------+-----------------------+
1 row in set (0.00 sec)

-- 将序列 alter_seq_01 的起始值设置为 40
mysql> alter sequence alter_seq_01 start with 40;
Query OK, 0 rows affected (0.01 sec)

mysql> select nextval('alter_seq_01'),currval('alter_seq_01');
+-----------------------+-----------------------+
| nextval(alter_seq_01) | currval(alter_seq_01) |
+-----------------------+-----------------------+
| 40                    | 40                    |
+-----------------------+-----------------------+
1 row in set (0.01 sec)

mysql> select nextval('alter_seq_01'),currval('alter_seq_01');
+-----------------------+-----------------------+
| nextval(alter_seq_01) | currval(alter_seq_01) |
+-----------------------+-----------------------+
| 42                    | 42                    |
+-----------------------+-----------------------+
1 row in set (0.00 sec)

-- 将序列 alter_seq_01 的增量值设置为 3
mysql> alter sequence alter_seq_01 increment by 3;
Query OK, 0 rows affected (0.01 sec)

mysql> select nextval('alter_seq_01'),currval('alter_seq_01');
+-----------------------+-----------------------+
| nextval(alter_seq_01) | currval(alter_seq_01) |
+-----------------------+-----------------------+
| 40                    | 40                    |
+-----------------------+-----------------------+
1 row in set (0.00 sec)

mysql> select nextval('alter_seq_01'),currval('alter_seq_01');
+-----------------------+-----------------------+
| nextval(alter_seq_01) | currval(alter_seq_01) |
+-----------------------+-----------------------+
| 43                    | 43                    |
+-----------------------+-----------------------+
1 row in set (0.00 sec)
```
