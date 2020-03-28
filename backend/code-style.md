

## 代码规范

- 请求规范
    - [后台请求规范](https://github.com/cdk8s/cdk8s-team-style/blob/master/dev/common/http-request.md)
    - [APP请求规范](https://github.com/cdk8s/cdk8s-team-style/blob/master/dev/common/api-request.md)
- [数据库约定](./db-style.md)
- 数据库创表 SQL 规则
    - foreignKey 单个参数、多个参数，返回 EntityObject、List。支持删除参数。
    - oneParam 单个请求参数，返回 List
    - listParam 多个请求参数，返回 List
- [log 输出原则](./log-style.md)
- 代码注释原则
    - 查询、操作分开
    - 不需要写参数注释，除了特别难懂的
    - 业务代码书写过程，先写注释梳理业务，后写代码


## YApi 原则

- 遵守 APP 请求规范
- 注释语法


