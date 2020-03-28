

## log 推荐使用

- 必须使用 `@Slf4j` 注解（Lombok）
- 要输出堆栈信息必须使用：`ExceptionUtil.getStackTraceAsString(e);`

## log 明确禁止

- 禁止使用字符串拼接，必须采用参数化方式
- 禁止：`e.printStackTrace()`
- 禁止：`System.out`
- 禁止：密码等重要信息打印
- 不要对可以预估非常大的参数值进行记录具体内容，而是记录其长度大小或是其他有意义的信息
- 不要在循环`（for, while）`体里面写 log，除非明确数据量小
- 不要记录重复的信息。有时候你会记录一些即将被传入某个方法的参数，然后你又忘记了，在函数内部把他又记录了一次。整个调用过程中要有侧重点。
- `application-test`，`application-prod` 配置里面不需要控制台输出
- `error 级别` 有单独文件存储