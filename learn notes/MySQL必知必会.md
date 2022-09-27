# MySQL必知必会

[TOC]

## MySQL安装

下载`deb`包

```
wget https://repo.mysql.com//mysql-apt-config_0.8.23-1_all.deb
```

安装 `gnupg` 包

```
sudo apt install gnupg
```

安装release包

```
sudo apt install ./mysql-apt-config_0.8.23-1_all.deb
```

安装MySQL

```
sudo apt update
sudo apt install mysql-server
```

查看服务状态

```
sudo service mysql status
```

## 卸载mysql

```
sudo systemctl stop mysql
sudo apt-get purge mysql-server mysql-client mysql-common mysql-server-core-* mysql-client-core-*
sudo rm -rf /etc/mysql /var/lib/mysql
sudo apt autoremove
sudo apt autoclean
```

## NOTE

***查看mysql表结构***

```
describe table_name;
```

***查看sqlite3表结构***

```
PRAGMA table_info(table_name);
```

**sqlite3用`regexp`**

需要安装：

```
sudo apt-get install sqlite3-pcre
```

进入sqlite3 运行：

```
.load /usr/lib/sqlite3/pcre.so
```

sqlite运行sql文件

```
.read db.sql
```

mysql运行sql文件

```
source db.sql
```

sqlite 仅支持用户重命名表或向现有表添加新列。无法重命名列、删除列或在表中添加或删除约束。

SQLite supports a limited subset of ALTER TABLE. The ALTER TABLE command in SQLite allows the user to rename a table or to add a new column to an existing table. It is not possible to rename a column, remove a column, or add or remove constraints from a table.

## Context

主键（primary key）①一一列（或一组列），其值能够唯一区分表
中每个行。

外键（foreign key） 外键为某个表中的一列，它包含另一个表
的主键值，定义了两个表之间的关系。

常用的文本处理函数

| 函 数       | 说 明             |
| ----------- | ----------------- |
| Left()      | 返回串左边的字符  |
| Length()    | 返回串的长度      |
| Locate()    | 找出串的一个子串  |
| Lower()     | 将串转换为小写    |
| LTrim()     | 去掉串左边的空格  |
| Right()     | 返回串右边的字符  |
| RTrim()     | 去掉串右边的空格  |
| Soundex()   | 返回串的SOUNDEX值 |
| SubString() | 返回子串的字符    |
| Upper()     | 将串转换为大写    |

SOUNDEX是一个将任何文
本串转换为描述其语音表示的字母数字模式的算法。SOUNDEX考虑了类似
的发音字符和音节，使得能对串进行发音比较而不是字母比较。

常用日期和时间处理函数

| 函 数         | 说 明                          |
| ------------- | ------------------------------ |
| AddDate()     | 增加一个日期（天、周等）       |
| AddTime()     | 增加一个时间（时、分等）       |
| CurDate()     | 返回当前日期                   |
| CurTime()     | 返回当前时间                   |
| Date()        | 返回日期时间的日期部分         |
| DateDiff()    | 计算两个日期之差               |
| Date_Add()    | 高度灵活的日期运算函数         |
| Date_Format() | 返回一个格式化的日期或时间串   |
| Day()         | 返回一个日期的天数部分         |
| DayOfWeek()   | 对于一个日期，返回对应的星期几 |
| Hour()        | 返回一个时间的小时部分         |
| Minute()      | 返回一个时间的分钟部分         |
| Month()       | 返回一个日期的月份部分         |
| Now()         | 返回当前日期和时间             |
| Second()      | 返回一个时间的秒部分           |
| Time()        | 返回一个日期时间的时间部分     |
| Year()        | 返回一个日期的年份部分         |

常用数值处理函数

| 函 数  | 说 明              |
| ------ | ------------------ |
| Abs()  | 返回一个数的绝对值 |
| Cos()  | 返回一个角度的余弦 |
| Exp()  | 返回一个数的指数值 |
| Mod()  | 返回除操作的余数   |
| Pi()   | 返回圆周率         |
| Rand() | 返回一个随机数     |
| Sin()  | 返回一个角度的正弦 |
| Sqrt() | 返回一个数的平方根 |
| Tan()  | 返回一个角度的正切 |

SQL聚集函数

| 函 数   | 说 明            |
| ------- | ---------------- |
| AVG()   | 返回某列的平均值 |
| COUNT() | 返回某列的行数   |
| MAX()   | 返回某列的最大值 |
| MIN()   | 返回某列的最小值 |
| SUM()   | 返回某列值之和   |

## sqlite3 数据类型

- NULL 
- INTEGER
- REAL 浮点数，存储一个8个字节的浮点数
- TEXT 字符
- BLOB 
