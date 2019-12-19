
## Sculptor Boot 后端开发环境

- 系统环境
    - macOS High Sierra 10.13.6
- 后端
    - **IntelliJ IDEA 2019.3**
        - 必备插件：Lombok [教程](https://github.com/cdk8s/cdk8s-team-style/blob/master/dev/backend/java/java-lombok.md)
    - Oracle JDK 1.8.0_231
    - Maven 3.6.3
    - Redis 4（Docker Image）
    - Prometheus 2.11（Docker Image）
    - Grafana 6.2.5（Docker Image）
    - MySQL 5.7（Docker Image）
    - Postman 7.12.0
    - JMeter 5.2.1
    - Jenkins 2.176.2
    - JProfiler 11.0.1
    - Docker version 18.09.2
- 前+后端软件打包下载：
    - [Windows 环境（密码:38yk）](https://pan.baidu.com/s/1dFKwxAbxb0IQpIyrnONniw)
    - [macOS 环境（密码:z0eb）](https://pan.baidu.com/s/1EBocUjhg3e183j1oJ2eqzw)
    - Docker 镜像相关可以看该篇详细文章：[Github](https://github.com/cdk8s/tkey-docs/blob/master/deployment/deployment-core.md) [Gitee](https://gitee.com/cdk8s/tkey-docs/blob/master/deployment/deployment-core.md)


## 开发必备 hosts

```
127.0.0.1 sso.cdk8s.com
127.0.0.1 redis.cdk8s.com
127.0.0.1 mysql.cdk8s.com
127.0.0.1 sculptor.cdk8s.com
```


## 完整依赖包集合

- [后端 pom.xml](https://github.com/cdk8s/sculptor-boot-backend/blob/master/pom.xml)
- 已利用 Maven Helper 插件处理了所有包冲突问题，目前一个都没有。逼死强迫症 Orz...

## 后端核心依赖

- 以 pom.xml 为准
- spring-boot-starter-parent：**2.1.11.RELEASE**

## H2 连接配置

- 默认开发环境、单元测试环境启用的数据库都是 H2 数据，方便管理测试数据
- 开发环境配置文件路径：`application-dev.yml` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/resources/application-dev.yml)
- 单元测试环境配置文件路径：`application-junit.yml` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/test/resources/application-junit.yml)

## MySQL / PostgreSQL 连接配置

- PostgreSQL 的测试会在 2020 年之前完成
- 除了 H2 之外的配置都是 MySQL
- 但是，目前对 MySQL 的连接池做了区别对待，一种是：Druid，一种是：Hikari
- Spring Boot 默认是采用 Hikari，性能确实更好。但是开发初期主要还是寻找问题、瓶颈，所以有必要测试阶段引入 Druid 进行监控 SQL

## Redis 连接配置

- Sculptor Boot 从设计初期就没有考虑过线程缓存，所以任何配置都是有 Redis 连接信息
- 特别的地方在于引入了 Redisson 作为分布式重复提交的加锁工具，后续也可以扩展为分布式限流工具等
- Redisson 的连接信息在该目录下：[Github](https://github.com/cdk8s/sculptor-boot-backend/tree/master/src/main/resources/redisson)
- Redisson 密码配置跟 Redis 连接密码是采用同一个，但是这个功能不是框架给的，而是我在这个配置类上定义，框架没带这个特性：`RedissonConfig.java` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/java/com/cdk8s/sculptor/config/RedissonConfig.java)
- 如果，如果你要改 Redis 相关信息，要同时修改两个地方，或者说你要扩展下我的 `RedissonConfig.java` 也是可以的

## 后端核心依赖

#### 强依赖（都是主流框架）

- TKey 单点登录（CDK8S 产品）
- Spring Boot 基础
- Lombok 必须使用
- MapStruct 必须使用
- Redis 必须使用
- Hibernate Validator 必须使用
- MyBatis 必须使用
- PageHelper 必须使用
- Spring Cache 必须使用（Redis 类型）
    - 对参数生成策略有了统一的封装 `RedisCacheConfig.java` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/java/com/cdk8s/sculptor/config/RedisCacheConfig.java)
    - 对缓存对象序列化数据格式设置为 JSON

#### 弱依赖

- ip2region 是一个 IP 库，为了避免项目过大，IP DB 文件没有加入到 git 里面
    - 需要做 IP 解析的需要自行下载该 DB 文件
    - 项目地址：<https://gitee.com/lionsoul/ip2region>
    - DB 文件下载：<https://gitee.com/lionsoul/ip2region/tree/master/data>
    - 需要下载 `ip2region.db` 文件到 Sculptor Boot 的 `/sculptor-boot-backend/src/main/resources/ip2region` 目录下
- [easy-captcha](https://github.com/whvcse/EasyCaptcha) 一个好玩的验证码
    - 在密码错误输入 3 次之后会出现验证码就是利用此依赖完成
    - 因为支持横向扩展，所以验证码必然是要存储到 Redis 中，具体实现可以看：`ValidateCodeService.java` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/java/com/cdk8s/sculptor/service/ValidateCodeService.java)
- Swagger 和 knife4j 文档
    - [knife4j](https://gitee.com/xiaoym/knife4j) 前身叫做 swagger-bootstrap-ui
    - 相对于 Swagger 它有更好的阅读 UI，但是请求交互目前还不够好，Swagger 会更加好操作


## 事务

- 禁止在 Service 捕获异常，自己消化，除非自己确实知道要做什么，不然可能会造成无法回滚。
- `@Transactional` 对 public 修饰的方法有效，别放在 private 上

## 分页

- 分页采用 PageHelper 组件，并且在 Service 就返回 PageInfo 对象
- 最终响应到前端的 DTO 与 Entity 之间的转换是放在 Controller 层级
- 我是希望 Service 尽可能单纯，但是不排除有复杂业务场景必须在 Service 转换处理，那就写上去也不会有什么影响，只是我觉得不好看而已

```
@Transactional(readOnly = true)
@Cacheable(keyGenerator = "keyGeneratorToServiceParam")
public PageInfo findPageByServiceBO(SysRolePageQueryServiceBO serviceBO) {
    return PageHelper.startPage(serviceBO.getPageNum(), serviceBO.getPageSize()).doSelectPageInfo(() -> {
        sysRoleMapper.selectByPageQueryMapperBo(sysRoleMapStruct.pageQueryServiceBOToMapperBO(serviceBO));
    });
}
```

## 操作权限

- 在 Controller 层级采用如下注解

```
@RequestPermission("sys_employee_update")
```

- 其中：`sys_employee_update` 值为 sys_permission 表中的 permission_code 字段值
- 利用 AOP 在 `RequestPermissionAspect.java` 中进行判断 [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/java/com/cdk8s/sculptor/aop/permission/RequestPermissionAspect.java)


#### 数据权限

- 数据权限我个人觉得跟业务强耦合，基础系统我无法抽象出来。只能等到具体业务系统之后，根据这部分业务表做数据权限。
- 一般思路都是根据特定字段进行 SQL 控制，比如部门、公司这类字段。切入的地方可以在 Controller 传参方式，也可以通过扩展 Mybatis 插件

## 统一异常设计

- 核心逻辑在：`ExceptionControllerAdvice.java` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/java/com/cdk8s/sculptor/exception/ExceptionControllerAdvice.java)
- 除了大家常规的捕获指定异常外，额外加了请求来源的判断，如果是 ajax 请求则一定返回的时候 JSON，其他就是 error 页面
- 其中还可以主动控制是 JSON 输出，还是 Error 页面
- 其中为了尽可能不输出英文的错误进行，还增加了一个类：`ExceptionDescriptionEnum.java` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/java/com/cdk8s/sculptor/exception/ExceptionDescriptionEnum.java) 里面收集了常见的英文错误信息，然后转换成中文错误信息输出

## 审计日志

- 在 Controller 层级采用 @EventLog 注解

```
@EventLog(message = "创建 sysEmployee 对象", operateType = EventLogEnum.OPERATE_TYPE_CREATE)
@EventLog(message = "更新 sysEmployee 对象", operateType = EventLogEnum.OPERATE_TYPE_UPDATE_INFO)
@EventLog(message = "批量删除 sysEmployee 对象", operateType = EventLogEnum.OPERATE_TYPE_DELETE)
```

- 在 `EventLogAspect.java` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/java/com/cdk8s/sculptor/aop/eventlog/EventLogAspect.java) 中异步存储数据

## 程序日志

- 采用 logback
- 配置文件所在目录：`/sculptor-boot-backend/src/main/resources/logback` [Github](https://github.com/cdk8s/sculptor-boot-backend/tree/master/src/main/resources/logback)
- 本地开发默认只有控制台输出 `logback-dev.xml` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/resources/logback/logback-dev.xml)
- 测试环境下控制台和文件同时输出
- 生产环境只有文件输出 `logback-prod.xml` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/resources/logback/logback-prod.xml)
    - 你会发现有文件输出的配置文件都特别长
    - 这是因为：我把项目所有常用包都单独进行了配置，方便在 Actuator 接口中可以动态修改输出级别，方便在线上动态该输出级别，做问题排查

## 单元测试

- 这是本项目的核心中的核心
- 目前发现基本市面上所有系统都是缺少单元测试的，而我很看中这个，只因为：重构后的回归
- 如果有人跟我一样写完代码开始动不动就做重构，应该就能明白做回归测试的重要性
- 单元测试框架：JUnit 4
- 分别对 Mapper、Service、Controller 都进行了单元测试
    - Mapper：[Github](https://github.com/cdk8s/sculptor-boot-backend/tree/master/src/test/java/com/cdk8s/sculptor/mapper)
    - Service：[Github](https://github.com/cdk8s/sculptor-boot-backend/tree/master/src/test/java/com/cdk8s/sculptor/service)
    - Controller：[Github](https://github.com/cdk8s/sculptor-boot-backend/tree/master/src/test/java/com/cdk8s/sculptor/controller)

## 启动测试数据启动写入

- `ApplicationTestDataInitRunner.java` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/java/com/cdk8s/sculptor/init/ApplicationTestDataInitRunner.java)
- 在启动后会把 TKey 的测试 Client 写入 Redis，这样才能登陆。如果要生产使用，建议把 Client 数据从 DB 和 Redis 做同步，可以参考 [TKey Management](https://github.com/cdk8s/tkey-management) 项目

#### 启动的健康检查

- `ApplicationHealthInitRunner.java` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/java/com/cdk8s/sculptor/init/ApplicationHealthInitRunner.java)
- 在启动后会对 DB、Redis、HTTP 进行检查，后续增加其他组件可以再这里继续添加

## 启动业务缓存删除

- `ApplicationCacheInitRunner.java` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/java/com/cdk8s/sculptor/init/ApplicationCacheInitRunner.java)
- 在启动后会对指定的一些业务缓存进行删除，这个需要根据自身业务进行决定


#### 关于系统参数表、字典表、properties 的定位

- 系统参数：希望能在上线阶段进行动态控制的参数
- 字典：希望能在上线阶段动态显示的规律类型数据
- properties：
    - 不希望在上线阶段被修改
    - 希望只在启动阶段使用
    - 属性值被写死在代码里面，不希望被修改


## 预设数据无法删除设置

- `DefaultDataUtil.java` [Github](https://github.com/cdk8s/sculptor-boot-backend/blob/master/src/main/java/com/cdk8s/sculptor/util/DefaultDataUtil.java)
- 在该类上预设了一些常量数据，这些数据不允许被删除。如果不喜欢这种表达方式也可以放在字典表中，或是其他表中

#### Maven 使用规定

- Maven 足够用，不会引入 Gradle，不是不好，而是目前成本还过高
- **重要：每次用工具生成代码之后最好都先 mvn clean 下，保证 MapStruct 生成的代码都是最新的**

#### 压力测试

- 暂时只有 Gatling
- 后期再生成 JMeter 脚本

#### 监控

- 和 TKey 一样，还是 Prometheus 进行数据收集
- Grafana Dashboard 文件：[Github](https://github.com/cdk8s/sculptor-boot-backend/tree/master/doc/grafana)


## 异步写入

- 有用到 Event Listener，后期分布式场景下可以扩展到 MQ，也不需要大改
- [Github](https://github.com/cdk8s/sculptor-boot-backend/tree/master/src/main/java/com/cdk8s/sculptor/eventlistener/listener)


-------------------------------------------------------------------

## 编码规范

- 还是老规矩，无状态，没有本地缓存，全部靠 Redis，我喜欢无状态的设计，虽然性能较低，但是爽在易于横向扩展。
- 这套编码规范很细节，有各种限制，并且不能随意更改，不然整套代码生成就会无法使用，因为我都是写死在生成器的 Java 代码中的。
- 有更多细节可以查看我们这套规范：<https://github.com/cdk8s/cdk8s-team-style>
- 关于如何做压测、部署、监控，大家可以看这一套：<https://github.com/cdk8s/tkey>
- 我们在前期已经做了很多工作，这里就不再累赘，只讲观点理念。


#### Redis 规范

- Redis Key 复词用下划线，层级用冒号隔开，全部大写，比如：`OAUTH:ACCESS_TOKEN:AT-102-uUCkO2NgITHWJSD16g89C9loMwCVSQqh`
- Redis Value 必须是 JSON 序列化


#### 数据库（下面字段名称不允许修改，不然整套体系无法使用）

- 数据库脚本，一部分在 H2 要求的目录下，一部分在 doc 目录
    - 为了兼容 H2 和 MySQL，创建索引、唯一主键的语句要单独抽出去，不然 H2 会报错。
    - doc 目录：[Github](https://github.com/cdk8s/sculptor-boot-backend/tree/master/doc/db)
    - H2 目录：[Github](https://github.com/cdk8s/sculptor-boot-backend/tree/master/src/main/resources/h2db)
- 数据库各个地方的命名都是采用全小写、下划线分割的方式
- 逻辑删除的字段名称：`state_enum` 不允许修改，前端会自动生成 radio 等组件
- 表与字段命名中各种 `_id` 结尾，`bool_` 开头，`_enum` 结尾，`rel_` 开头等细节的必须遵守
- `_date` 结尾的字段必须是存储日期，在前端会生成区间查询组件
- 中间表不需要逻辑删除以及 `update_date` 和 `update_user_id`
- `ranking` 代表排序，默认100，最小1，最大100，值越小排越前。加 ing 主要是避免关键字问题
- 所有的主键字段都必须叫做：`id`
- 下面字段必须成套出现，并且不允许修改字段名称
    - 创建逻辑
        - create_date
        - create_user_id
    - 更新逻辑
        - update_date
        - update_user_id
    - 逻辑删除逻辑
        - delete_enum
        - delete_date
        - delete_user_id
- 树结构的必须含有：
    - parent_Id
    - parent_Ids
- 在 comment 加上：`oneParam,listParam` 的字段会自动生成该字段的查询方法
- 在 comment 加上：`foreignKey` 的字段会自动生成该字段的多个查询方法
- Enum 类必须在 comment 加上类似格式：`[1=启用=ENABLE, 2=禁用=DISABLE]max=2`
    - 可以自动生成对应的 Enum 类，以及再 RequestParam 参数上限制最大值为 2

-------------------------------------------------------------------

## Spring Boot 代码规定

#### 强规范

- 禁止使用标准的 RESTful 风格，只能是 GET 和 POST
- 业务接口 POST 传参必然是 JSON
- 禁止使用 PathVariable 方式传参，不易于拦截器
- 禁止使用简写的 Mapping 注解，比如：`@GetMapping`
- 禁止 requestMapping 没有以 / 开头
- UserInfoContext.java 是 TheadLocal 装载的，只能在 Controller 层级才是安全有效的，service 和 mapper 一定不可使用。不然你的异步线程肯定完蛋。暂时不推荐市场上衍生出来的 TheadLocal。
- 请求和响应的 JSON 标准可以看：[Github](https://github.com/cdk8s/cdk8s-team-style/blob/master/dev/common/http-request.md)
- 不允许使用三元表达式
- 因为默认都用了 Spring Cache 相关注解，看上去查询性能是提高。但是，缓存是不能随便用的，有些接口可能是强一致性的。极端的双写不一致，也需要评估收益比。还有读写比例等等。
- 请求参数
    - 严格划分 MVC 三层的参数，不允许跨界调用参数（之间的赋值是交给 MapStruct 处理的，不用自己手工处理）
    - 给 Controller 操作的参数叫做：requestParam
    - 给 service 操作的参数叫做：serviceBO
    - 给 mapper 操作的参数叫做：mapperBO
- 必须使用 Hibernate Validator 进行请求参数校验
