# CREATE SNAPSHOT

## 语法说明

`CREATE SNAPSHOT` 命令用于创建快照。实例只能为本实例创建快照，且创建的快照仅本实例可见。

## 语法结构

```sql
> CREATE SNAPSHOT snapshot_name FOR ACCOUNT account_name
```

## 示例

```sql
--在实例 585b49fc_852b_4bd1_b6d1_d64bc1d8xxxx 下执行
create snapshot sp1 for account `585b49fc_852b_4bd1_b6d1_d64bc1d8d485`;

mysql> show snapshots;
+---------------+-------------------------------+----------------+--------------------------------------+---------------+------------+
| SNAPSHOT_NAME | TIMESTAMP                     | SNAPSHOT_LEVEL | ACCOUNT_NAME                         | DATABASE_NAME | TABLE_NAME |
+---------------+-------------------------------+----------------+--------------------------------------+---------------+------------+
| sp1           | 2024-07-09 06:34:09.381035637 | account        | 585b49fc_852b_4bd1_b6d1_d64bc1d8xxxx |               |            |
+---------------+-------------------------------+----------------+--------------------------------------+---------------+------------+
1 row in set (0.16 sec)
```

## 限制

- 目前只支持创建实例级别的快照，不支持创建集群级别、数据库级别和表级别的快照。