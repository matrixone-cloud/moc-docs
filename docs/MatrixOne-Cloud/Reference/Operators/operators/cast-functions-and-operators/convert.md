# **CONVERT**

## **函数说明**

`CONVERT()` 函数将一个值转换为指定的数据类型或字符集。

## **语法结构**

```
> CONVERT(value, type)

```

或：

```
> CONVERT(value USING charset)
```

## **相关参数**

|  参数  | 说明 |
|  ----  | ----  |
| value  | 必要参数，待转化的值 |
| datatype  | 必要参数，目标数据类型 |
| charset | 必要参数，目标字符集 |

目前，`convert` 可以进行如下转换：

* 数值类型之间转换，主要包括 SIGNED，UNSIGNED，FLOAT，DOUBLE 类型
* 数值类型向字符 CHAR 类型转换
* 格式为数值的字符类型向数值类型转换 (负数需要转换为 SIGNED)

## **示例**

```sql
mysql> select convert(150,char);
+-------------------+
| cast(150 as char) |
+-------------------+
| 150               |
+-------------------+
1 row in set (0.01 sec)
```

```sql
CREATE TABLE t1(a tinyint);
INSERT INTO t1 VALUES (127);

mysql> SELECT 1 FROM
  -> (SELECT CONVERT(t2.a USING UTF8) FROM t1, t1 t2 LIMIT 1) AS s LIMIT 1;
+------+
| 1    |
+------+
|    1 |
+------+
1 row in set (0.00 sec)
```

## **限制**

* 非数值的字符类型无法转化为数值类型
* 日期格式的数值类型、字符类型无法转化为 Date 类型
* Date，Datetime 类型无法转化为字符类型
* Date 与 Datetime 暂不能互相转化
