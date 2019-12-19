---
title: 《最适合后端的 React Admin》第 7 章：修改登录逻辑
date: 2018-07-12 21:31:22
description: "本系列更多的作用是如何使用和引导现代前端开发的思路"
categories: [React]
tags: [React]
---

## 登录效果图

![react-chapter-2-3.png](https://upload-images.jianshu.io/upload_images/1853136-f5e862736137d852.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 登录页面样式来源：[点击查看](https://www.js-css.cn/a/css/template/reglogin/2018/0129/1504.html?tdsourcetag=s_pctim_aiomsg)

## 修改请求参数

- 修改文件：`src\pages\login\index.js`

```
<form>
  <input ref='username' type="text" className={styles['text']} placeholder={'username'} defaultValue='admin'/>
  <input ref='password' type="password" placeholder={'password'} defaultValue='123456'/>
</form>
```


## 修改请求路径

- 修改文件: `src\request\api.js`

```
platformLogin: '/platform/login',
````

## 登录成功、失败处理逻辑

- 修改文件: `src\pages\login\models\login.js`

```
export default {
  namespace: 'loginToNamespace',
  state: {},
  subscriptions: {},
  effects: {
    * platformLogin({ payload }, { call }) {
      const response = yield call(login, payload);
      if (response && response[isSuccess] === true) {
        const token = response.result.token;
        sessionStorage.setItem(platformToken, token);
        router.push('/');
      } else if (response && response[isSuccess] === false) {
        message.error(response.error_info.msg);
      }
    },
  },
  reducers: {},
};
````


## 联系我

- 博客：<http://sayshy.com>
- 邮件：`satan31415#qq.com`
- Github：`https://github.com/satan31415`
