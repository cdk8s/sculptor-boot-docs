---
title: 《最适合后端的 React Admin》第 5 章：公共变量修改、后端字典数据存储
date: 2018-04-11 21:12:23
description: "本系列更多的作用是如何使用和引导现代前端开发的思路"
categories: [React]
tags: [React]
---

## 公共常量添加、引用

- 大部分公共常量都放在 `src\common\globalConstant.js` 里面，比如：

``` 
export const platformToken = 'platform_token';
export const isSuccess = 'is_success';
```
 
- 需要引用的时候导入并使用，可以参考 `src\models\logout.js`：

```
import { isSuccess, platformToken } from '../common/globalConstant';

sessionStorage.removeItem(platformToken);
```


## 初始化界面的时候获取到后端的常用字典数据，并存储、使用

#### 常用数据字典在登录的时候获取，分别存入 LocalStorage 和 SessionStorage

- SessionStorage 和 LocalStorage 都是 key-value 结构，value 只能为字符串，所以数据存储过程必须用 `JSON.stringify()` 函数进行转换。取出值必须用 `JSON.parse()` 函数进行转换
- SessionStorage 数据有效期只在当前会话，所以要注意：在浏览器关闭时，数据将会失效
- LocalStorage 数据只有在清除浏览器缓存的时候才会被移除

#### SessionStorage 添加数据和删除数据

- 添加数据参考：`src\pages\login\models\login.js`

```
sessionStorage.setItem(platformToken, token);
```

- 删除数据参考：`src\models\logout.js`

```
sessionStorage.removeItem(platformToken);
```

#### LocalStorage 添加数据和删除数据

- 添加数据：

```
localStorage.setItem(platformToken, token);
```

- 删除数据：

```
localStorage.removeItem(platformToken);
```



## 联系我

- 博客：<http://sayshy.com>
- 邮件：`satan31415#qq.com`
- Github：`https://github.com/satan31415`
