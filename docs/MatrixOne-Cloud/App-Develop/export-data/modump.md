# 使用 modump 导出数据

MatrixOne Cloud 支持使用 `mo-dump` 导出数据。

## 什么是 `mo-dump`

`mo-dump` 是 MatrixOne 的一个客户端实用工具，与 `mysqldump` 一样，它可以被用于通过导出 `.sql` 类型的文件来对 MatrixOne 数据库进行备份，该文件类型包含可执行以重新创建原始数据库的 SQL 语句。

使用 `mo-dump` 工具，你必须能够访问运行 MatrixOne 实例的服务器。你还必须拥有导出的数据库的用户权限。

### `mo-dump` 语法结构

```
./mo-dump -u ${user} -p${password} -h ${host} -P ${port} -db ${database} [--local-infile=true] [-csv] [-tbl ${table}...] -net-buffer-length ${net-buffer-length} > {dumpfilename.sql}
```

**参数释义**

- **-u [user]**：连接 MatrixOne 服务的用户名。只有具有数据库和表读取权限的用户才能使用 `mo-dump` 实用程序，默认值：dump。

- **-p [password]**：MatrixOne 用户的有效密码。

- **-h [host]**：MatrixOne 服务的主机 IP/HOST 地址。默认值：127.0.0.1。

- **-P [port]**：MatrixOne 服务的端口。默认值：6001。

- **-db [数据库名称]**：必需参数。要备份的数据库的名称。

- **-net-buffer-length [数据包大小]**: 数据包大小，即 SQL 语句字符的总大小。数据包是 SQL 导出数据的基本单位，如果不设置参数，则默认 1048576 Byte（1M），最大可设置 16777216 Byte（16M）。假如这里的参数设置为 16777216 Byte（16M），那么，当要导出大于 16M 的数据时，会把数据拆分成多个 16M 的数据包，除最后一个数据包之外，其它数据包大小都为 16M。

- **-no-data**: 当在命令中显式指定该项时仅导出数据库/包的创建语句，不导出数据。

- **-csv**：当在命令中显式指定该项时表示导出数据为 *CSV* 格式。

- **-csv-field-delimiter [","]**: 设置 csv 字段分隔符，仅支持一个 utf8 字符，默认值为“,”。该项仅当设置导出数据格式为“csv”时启用。

- **-tbl [表名]**：可选参数。如果参数为空，则导出整个数据库。如果要备份指定表，则可以在命令中指定多个 `-tbl` 和表名。

## 如何使用 `mo-dump`

### 1. 下载 mo-dump 二进制文件

- 下载 [Linux x86 mo-dump 文件](https://github.com/matrixorigin/artwork/blob/main/docs/download/modump/linux-x86/mo-dump)
- 下载 [Linux ARM mo-dump 文件](https://github.com/matrixorigin/artwork/blob/main/docs/download/modump/linuxarm/mo-dump)
- 下载 [macOS x86 mo-dump 文件](https://github.com/matrixorigin/artwork/blob/main/docs/download/modump/osx86/mo-dump)
- 下载 [macOS ARM mo-dump 文件](https://github.com/matrixorigin/artwork/blob/main/docs/download/modump/osarm/mo-dump)

!!! note
    `mo-dump` 文件可以在相同的硬件平台上工作。但是需要注意在 x86 平台中构建的 `mo-dump` 二进制文件在 Darwin ARM 平台中则无法正常工作。你可以在同一套操作系统和硬件平台内构建并使用 `mo-dump` 二进制文件。`mo-dump` 目前只支持 Linux 和 macOS。

### 2. 使用 `mo-dump` 导出 MatrixOne Cloud 的数据库

`mo-dump` 在命令行中非常易用。参见以下步骤示例，导出 *sql* 文件格式完整数据库：

1. 选择目标实例，点击**连接 > 通过第三方工具连接**，右侧滑窗内可查阅到 MatrixOne Cloud 上你的实例主机地址、端口号、用户名和密码。

    !!! note
        mo-dump 暂不支持连接串的用户名格式（即不支持原格式 `<accountname>:<username>:<rolename>`)，需要更改为 `<accountname>#<username>#<rolename>`。

2. 在你本地计算机上打开终端窗口，输入以下命令，连接到 MatrixOne Cloud，并且导出数据库：

    ```
    ./mo-dump -u <accountname>#<username>#<rolename> -p password -h moc_host_address -P 6001 -db database > exported_db.sql
    ```

例如，如果你想要生成单个数据库的备份，请运行以下命令。该命令将在 *t.sql* 文件中生成 **t** 数据库的结构和数据的备份。*t.sql* 文件将与您的 `mo-dump` 可执行文件位于同一目录中。

```
./mo-dump -u <accountname>#<username>#<rolename> -p password -h moc.cluster.matrixonecloud.cn -P 6001 -db t > t.sql
```

如果你想将数据库 *t* 内的表导出为 *CSV* 格式，参考使用下面的命令：

```
./mo-dump -u <accountname>#<username>#<rolename> -p password -h moc_host_address -db t -csv > ttt.csv
```

如果要在数据库中生成单个表的备份，可以运行以下命令。该命令将生成命名为 *t* 的数据库的 *t1* 表的备份，其中包含 *t.sql* 文件中的结构和数据。

```
./mo-dump -u <accountname>#<username>#<rolename> -p password -h moc_host_address -db t -tbl t1 > t1.sql
```

## 限制

- `mo-dump` 仅支持导出单个数据库的备份，如果你有多个数据库需要备份，需要手动运行 `mo-dump` 多次。
