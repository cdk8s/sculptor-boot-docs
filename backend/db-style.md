

## 数据库约定

- 1. 不写 ` 号包裹字段名称和表名称，有助于更好地支持其他数据库，其他库不支持这个符号
- 2. 不写 unsigned，有助于更好地支持其他数据库，其他库没有这个属性
- 3. 不写 auto_increment，有助于更好地支持其他数据库，以及后续移库和分区分表的需求
- 4. 不写 int，在 PGSQL 中它叫做 integer，不能简写。推荐 smallint 和 bigint 来代替一般数字类型上的需求
- 5. 不写 datetime，主要是可以减少因为时区问题带来的一系列问题，对消息队列，大数据软件有更好的支持
- 6. 不在表结构中写 unique key 和 index，推荐单独 SQL 语法写，H2 和 PGSQL 都不支持在表结构中定义。
- 7. 枚举字段命名带有 _enum 后缀
- 8. 逻辑删除命名为 delete_enum，不用 is_ 开头是因为 Java 对这类字段生成 set / get 会有特殊处理，会忽略到 is 这样不易于 Java 代码和数据库之间的维护
- 9. 数字 0,1 理论上代表 0 false ，1 true，mybatis 框架默认了这套逻辑，所以反而在判断数字类型的时候会自动帮你转换，造成了不必要的麻烦，所以不推荐 0 作为下标起点
- 一些 Java 特有的坑
    - [使用java Bean时，is 打头的boolean属性的小坑](https://blog.csdn.net/sdfgedcx/article/details/91953744)
    - [mybatis 中 if-test 判断大坑](https://www.cnblogs.com/grasp/p/11268049.html)