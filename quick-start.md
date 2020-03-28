

## 快速开始

- [软件下载、安装方式介绍](./development-environment.md)
- [本地 Hosts 配置，保证每个人的配置文件内容不变动](./hosts.md)
- 前后端 DEMO 项目运行演示
- 后端配置说明
    - H2
    - Redis
    - jasypt 加密配置
    - Druid 与 Hikari
    - TKey SSO
    - 单元测试配置
    - log 输出配置
    - Init 操作
- 前端配置说明
    - globalConstant.ts
    - .umirc.ts
    - .umirc.test.ts
    - .umirc.prod.ts

## 生成代码演示

- 生成器配置说明
- 单表生成演示
- 一对多生成演示
- 生成步骤
    - Main 方法运行
    - Maven Clean，避免 MapStruct
    - 库 SQL 复制到 H2
    - 权限 SQL 复制到 H2
    - 删除前端不要的组件
    - 调整部分字段显示，比如外键内容

## 小技巧

- 必须先确保本地 H2 库无问题，再配置到测试库服务器
- 通过 IntelliJ IDEA 设置 Program arguments 保证每个人的配置文件内容不变动
    - `--SPRING_MYSQL_PASSWORD=123456`
    - `--SPRING_REDIS_PASSWORD=123456`
- 重复生成过程 IntelliJ IDEA 的 Git 重要性
- 基于花生壳域名，本地 IntelliJ IDEA 进行 Debug 问题排查
