# **CAST**

## **函数说明**

`CAST()` 函数可以将任何类型的一个值转化为另一个特定类型。

## **语法结构**

```
> CAST(value AS datatype)

```

## **相关参数**

|  参数  | 说明 |
|  ----  | ----  |
| value  | 必要参数，待转化的值 |
| datatype  | 必要参数，目标数据类型 |

目前，`cast` 可以进行如下转换：

* 数值类型之间转换，主要包括 SIGNED，UNSIGNED，FLOAT，DOUBLE 类型
* 数值类型向字符 CHAR 类型转换
* 格式为数值的字符类型向数值类型转换（负数需要转换为 SIGNED）
* Time 类型（包括 Date, Datetime, Timestamp 和 Time）转换为 INT 类型，存在小数点位四舍五入
* Time 类型（包括 Date, Datetime, Timestamp 和 Time）转换为定点类型，保留小数位

详细的数据类型转换规则，请参见[数据类型转换](../../../Data-Types/data-type-conversion.md)。

## **示例**

```sql
drop table if exists t1;
CREATE TABLE t1 (a int,b float,c char(1),d varchar(15));
INSERT INTO t1 VALUES (1,1.5,'1','-2');

mysql> SELECT CAST(a AS FLOAT) a_cast,CAST(b AS UNSIGNED) b_cast,CAST(c AS SIGNED) c_cast, CAST(d AS SIGNED) d_cast from t1;
+--------+--------+--------+--------+
| a_cast | b_cast | c_cast | d_cast |
+--------+--------+--------+--------+
|      1 |      2 |      1 |     -2 |
+--------+--------+--------+--------+
1 row in set (0.01 sec)

mysql> SELECT CAST(a AS CHAR) a_cast, CAST(b AS CHAR) b_cast,CAST(c AS DOUBLE) c_cast, CAST(d AS FLOAT) d_cast from t1;
+--------+--------+--------+--------+
| a_cast | b_cast | c_cast | d_cast |
+--------+--------+--------+--------+
| 1      | 1.5    |      1 |     -2 |
+--------+--------+--------+--------+
1 row in set (0.00 sec)
```

## **限制**

* 非数值的字符类型无法转化为数值类型。
* 字符类型无法转化为 Date 类型。
* Date，Datetime 类型无法转化为字符类型。
* Date 与 Datetime 暂不能互相转化。
