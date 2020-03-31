

## 数据库约定

- 字段名要长也不要短，长了可以借助 IntelliJ IDEA 做全文检索，短了要批量替换、修改都很不方便
- ranking 和 state_enum 字段不要随便用，要根据实际业务场景用
- 不写 ` 号包裹字段名称和表名称，有助于更好地支持其他数据库，其他库不支持这个符号
- 不写 unsigned，有助于更好地支持其他数据库，其他库没有这个属性
- 不写 auto_increment，有助于更好地支持其他数据库，以及后续移库和分区分表的需求
- 不写 int，在 PGSQL 中它叫做 integer，不能简写。推荐 smallint 和 bigint 来代替一般数字类型上的需求
- 不写 datetime，主要是可以减少因为时区问题带来的一系列问题，对消息队列，大数据软件有更好的支持
- 不在表结构中写 unique key 和 index，推荐单独 SQL 语法写，H2 和 PGSQL 都不支持在表结构中定义。
- 逻辑删除命名为 delete_enum，不用 is_ 开头是因为 Java 对这类字段生成 set / get 会有特殊处理，会忽略到 is 这样不易于 Java 代码和数据库之间的维护
- 枚举字段都是以 `_enum` 结尾，如果是 boolean 类型的枚举规则是：以 `bool_` 开头，`_enum` 结尾。
- decimal 类型的完整格式都是：`decimal(18, 2)`，不允许有其他写法
- 链接字段一般都是 varchar(500)
- 创建时间字段可能会跟某些业务字段数据相同，比如激活时间，分享时间等等，即使他们相同也不能利用创建时间来做业务说明。创建时间是给数据库、程序用的
- 中间表
    - 表名都以 `rel_` 开头
    - 中间表也是有 id 字段
    - 中间表也是有 create_date，create_user_id 字段
- 注释语句中用英文冒号分开描述说明
- 数字 0,1 理论上代表 0 false ，1 true，mybatis 框架默认了这套逻辑，所以反而在判断数字类型的时候会自动帮你转换，造成了不必要的麻烦，所以不推荐 0 作为下标起点
- 一些 Java 特有的坑
    - [使用java Bean时，is 打头的boolean属性的小坑](https://blog.csdn.net/sdfgedcx/article/details/91953744)
    - [mybatis 中 if-test 判断大坑](https://www.cnblogs.com/grasp/p/11268049.html)