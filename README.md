1.下载完安装包，并解压 /usr/local/mongodb 
2.MongoDB 的可执行文件位于 bin 目录下，所以可以将其添加到 PATH 路径中：
  export PATH=<mongodb-install-directory>/bin:$PATH
3.MongoDB的数据存储在data目录的db目录下，但是这个目录在安装过程不会自动创建，所以你需要手动创建data目录，并在data目录中创建db目录。
  以下实例中我们将data目录创建于根目录下(/)。
  注意：/data/db 是 MongoDB 默认的启动的数据库路径(--dbpath)。
4.命令行中运行 MongoDB 服务 ./mongod
5.MongoDB后台管理 Shell ./mongo
6.MongoDB 提供了简单的 HTTP 用户界面。 如果你想启用该功能，需要在启动的时候指定参数 --rest 。
  ./mongod --dbpath=/data/db --rest
  MongoDB 的 Web 界面访问端口比服务的端口多1000。
  如果你的MongoDB运行端口使用默认的27017，你可以在端口号为28017访问web用户界面，即地址为：http://localhost:28017。

7.
SQL术语/概念	MongoDB术语/概念	解释/说明
database		database			数据库
table			collection			数据库表/集合
row				document			数据记录行/文档
column			field				数据字段/域
index			index				索引
table 			joins	 			表连接,MongoDB不支持
primary key		primary key			主键,MongoDB自动将_id字段设置为主键

8.
一个mongodb中可以建立多个数据库。
MongoDB的默认数据库为"db"，该数据库存储在data目录中。
MongoDB的单个实例可以容纳多个独立的数据库，每一个都有自己的集合和权限，不同的数据库也放置在不同的文件中。

9.
数据库命名规则：
数据库也通过名字来标识。数据库名可以是满足以下条件的任意UTF-8字符串。
不能是空字符串（"")。不得含有' '（空格)、.、$、/、\和\0 (空宇符)。应全部小写。最多64字节

10.
有一些数据库名是保留的，可以直接访问这些有特殊作用的数据库。
admin： 从权限的角度来看，这是"root"数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器。
local: 这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合
config: 当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息。

11.
文档中的键/值对是有序的。
文档中的值不仅可以是在双引号里面的字符串，还可以是其他几种数据类型（甚至可以是整个嵌入的文档)。
MongoDB区分类型和大小写。
MongoDB的文档不能有重复的键。
文档的键是字符串。除了少数例外情况，键可以使用任意UTF-8字符。

12.
集合就是 MongoDB 文档组，类似于 RDBMS （关系数据库管理系统：Relational Database Management System)中的表格。
集合存在于数据库中，集合没有固定的结构，这意味着你在对集合可以插入不同格式和类型的数据，但通常情况下我们插入集合的数据都会有一定的关联性。

合法的集合名
	集合名不能是空字符串""。
	集合名不能含有\0字符（空字符)，这个字符表示集合名的结尾。
	集合名不能以"system."开头，这是为系统集合保留的前缀。
	用户创建的集合名字不能含有保留字符。有些驱动程序的确支持在集合名里面包含，这是因为某些系统生成的集合中包含该字符。除非你要访问这种系统创建的集合，否则千万不要在名字里出现$。　

13.
capped collections
就是固定大小的collection。它有很高的性能以及队列过期的特性(过期按照插入的顺序). 有点和 "RRD" 概念类似。
Capped collections是高性能自动的维护对象的插入顺序。它非常适合类似记录日志的功能 和标准的collection不同，你必须要显式的创建一个capped collection， 指定一个collection的大小，单位是字节。collection的数据存储空间值提前分配的。
要注意的是指定的存储大小包含了数据库的头信息。
在capped collection中，你能添加新的对象。
能进行更新，然而，对象不会增加存储空间。如果增加，更新就会失败 。
数据库不允许进行删除。使用drop()方法删除collection所有的行。
注意: 删除之后，你必须显式的重新创建这个collection。
在32bit机器中，capped collection最大存储为1e9( 1X109)个字节。'

14.
数据库的信息是存储在集合中。它们使用了系统的命名空间：dbname.system.*
在MongoDB数据库中名字空间 <dbname>.system.* 是包含多种系统信息的特殊集合(Collection)，如下:
集合命名空间					描述
dbname.system.namespaces		列出所有名字空间。
dbname.system.indexes			列出所有索引。
dbname.system.profile			包含数据库概要(profile)信息。
dbname.system.users				列出所有可访问数据库的用户。
dbname.local.sources			包含复制对端（slave）的服务器信息和状态。

15.
数据类型			描述
String				字符串。存储数据常用的数据类型。在 MongoDB 中，UTF-8 编码的字符串才是合法的。
Integer				整型数值。用于存储数值。根据你所采用的服务器，可分为 32 位或 64 位。
Boolean				布尔值。用于存储布尔值（真/假）。
Double				双精度浮点值。用于存储浮点值。
Min/Max keys		将一个值与 BSON（二进制的 JSON）元素的最低值和最高值相对比。
Arrays				用于将数组或列表或多个值存储为一个键。
Timestamp			时间戳。记录文档修改或添加的具体时间。
Object				用于内嵌文档。
Null				用于创建空值。
Symbol				符号。该数据类型基本上等同于字符串类型，但不同的是，它一般用于采用特殊符号类型的语言。
Date				日期时间。用 UNIX 时间格式来存储当前日期或时间。你可以指定自己的日期时间：创建 Date 对象，传入年月日信息。
Object ID			对象 ID。用于创建文档的 ID。
Binary Data			二进制数据。用于存储二进制数据。
Code				代码类型。用于在文档中存储 JavaScript 代码。
Regular expression	正则表达式类型。用于存储正则表达式。





