
## 开发环境搭建

#### Windows 10 x64

- **开发人员硬件要求**
    - 开发机内存不要低于 8G，最好是 16G 内存。不然开发效率会很低。
- **注意：** 软件包都已集合，无需到官网下载。关注 `cd-k8s` 微信公众号，回复 `dev` 即可
- **全部安装到 C 盘默认路径，不要乱修改安装路径**
- **前端相关**
    - 前端软件不允许随便升级、降级，必须保持一致
    - 如果已有安装版本，请按此篇文章进行卸载：[卸载旧版本方法](https://github.com/cdk8s/cdk8s-team-style/blob/master/files/frontend/windows-node-uninstall.md)
    - Node 12.13.1
        - [安装教程（后面的配置不需要）](https://www.bilibili.com/video/BV1RE411Q7bj)
    - Yarn 1.19.2
        - 下一步下一步安装即可
- **后端相关**
    - JDK 8
        - [安装教程（只安装 JDK8 即可）](https://www.bilibili.com/video/BV1GE411J7Um)
    - MySQL 5.7+
        - [安装教程](https://www.bilibili.com/video/BV184411j78d)
    - Maven 3.6.3
        - [安装教程](https://www.bilibili.com/video/BV1x7411n7PS)
        - [settings.xml 配置模板](https://github.com/cdk8s/cdk8s-team-style/blob/master/files/maven/settings.xml)
    - Redis Server 3.2+
        - [安装教程](https://www.runoob.com/redis/redis-install.html)
    - Redis Desktop Manager
        - [安装教程](https://www.cnblogs.com/chengxs/p/9090819.html)
    - SwitchHosts
        - [安装教程](https://www.bilibili.com/video/BV1G4411x7kz)
    - Navicat Premium
        - [安装教程](https://www.bilibili.com/video/BV1yE411u73v)
    - 花生壳（注册账号，方便后续联调）
        - [安装教程](https://github.com/cdk8s/cdk8s-team-style/blob/master/test/test-style.md)
- IntelliJ IDEA 及其插件（必装）
    - Lombok
    - MyBatis
    - MapStruct Support
    - Maven Helper
    - GenerateAllSetter
    - GsonFormat
    - String Manipulation

## 公众号

![公众号](http://img.gitnavi.com/markdown/cdk8s_qr_300px.png)