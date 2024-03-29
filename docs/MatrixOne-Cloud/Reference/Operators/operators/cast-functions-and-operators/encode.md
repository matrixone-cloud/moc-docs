# **ENCODE()**

## **函数说明**

`ENCODE()` 函数用于将二进制数据编码为文本表示。支持以下编码方式：

 - **Base64（Base64 编码）**：Base64 是一种用于将二进制数据转换为文本数据的编码方式。它将 3 个字节的二进制数据编码成 4 个字符的文本数据，通常用于在文本协议中传输二进制数据，如在电子邮件中嵌入图片或在网页中传输数据。

- **Hex（十六进制）**：十六进制是一种基数为 16 的数字系统，它使用 0-9 和 A-F 表示数值。在计算机中，常用于表示二进制数据的一种方式。每个十六进制数字对应四个二进制位，因此十六进制更紧凑地表示二进制数据。

## **函数语法**

```
> ENCODE (String, encoding);
```

## **参数释义**

|  参数             | 说明                           |
|  --------------- | ------------------------------ |
| String           | 必要参数。要进行编码的文本。        |
| encoding         | 必要参数。指定用于编码的字符集。    |

## **示例**

```SQL
mysql> select encode('hello', 'base64');
+-----------------------+
| encode(hello, base64) |
+-----------------------+
| aGVsbG8=              |
+-----------------------+
1 row in set (0.00 sec)

mysql> select encode('hello', 'hex');
+--------------------+
| encode(hello, hex) |
+--------------------+
| 68656c6c6f         |
+--------------------+
1 row in set (0.01 sec)

```

## **限制**

- 由于现在 MatrixOne 不处理字符串中的\（转义符），所以暂不支持 `escape` 编码方式。
!!! note
    **Escape（转义字符）**：转义字符是在字符串中用于表示一些特殊字符或执行特殊操作的字符。例如，\n 表示换行符，\t 表示制表符。Escape 字符通常与其他字符组合，以表示原本难以直接输入的字符。
