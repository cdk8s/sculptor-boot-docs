---
title: 《最适合后端的 React Admin》第 6 章：修改 Ajax 请求框架（Axios 拦截器配置）
date: 2018-05-12 22:32:13
description: "本系列更多的作用是如何使用和引导现代前端开发的思路"
categories: [React]
tags: [React]
---


## 请求地址全局变量

- 在文件：`src\request\api.js` 里面配置

```
export default {

  // 本地开发 mock 的时候配置
  target: '',

  // 本地开发，调用本地后端的时候配置
  // target: 'http://localhost:28081',


  platformMenuList: '/platform/menu/list',


  platformRolePage: '/platform/role/page',
  platformRoleObject: '/platform/role/object',
  platformRoleAdd: '/platform/role/add',
  platformRoleUpdate: '/platform/role/update',
  platformRoleBatchDelete: '/platform/role/batch-delete',


  platformLogin: '/platform/login',
  platformLogout: '/platform/logout',

};
```


## Axios 封装 GET、POST 请求方法

- 参考：`src\request\request.js`

```
const get = (url, parmas) => {
  return new Promise((resolve, reject) => {
    axios.get(url, {
      params: parmas,
    })
      .then(res => {
        resolve(res);
      })
      .catch(err => {
        reject(err);
      });
  });
};
const post = (url, params) => {
  return new Promise((resolve, reject) => {
    axios.post(url, params)
      .then(res => {
        if (res) {
          resolve(res);
        }
      })
      .catch(err => {
        reject(err);
      });
  });
};

export default { get, post, api };
```


## Axios 设置全局配置

- 参考：`src\request\request.js`
- 设置请求域名根路径
- 设置超时时间
- 设置请求头参数

```
axios.defaults.baseURL = api.target;
axios.defaults.timeout = 10000;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded;charset=UTF-8;Accept-Language:zh-CN,zh;q=0.8';
```


## Axios 设置全局请求拦截器

- 参考：`src\request\request.js`

```
axios.interceptors.request.use(config => {
  let newConfig = config;
  newConfig = {
    ...config,
    headers: {
      post: {
        platform_token: sessionStorage.getItem(platformToken),
      },
      get: {
        platform_token: sessionStorage.getItem(platformToken),
      },
    },
  };
  return newConfig;
});
```


## Axios 设置全局响应拦截器

- 参考：`src\request\request.js`
- 只返回状态码 200 的内容
- 如果返回状态码是 4xx 则跳转到 404 页面
- 如果返回状态码是 5xx 则跳转到 500 页面

```
axios.interceptors.response.use(config => {
  if (config.data && config.data[isSuccess] === false && config.data.error_info.code === 401) {
    router.push('/login');
  }
  return config.data;
}, (error) => {
  if (error && error.response) {
    switch (error.response.status) {
      case 500:
        router.push('/500');
        message.error(codeMessage[error.response.status]);
        break;
      case 403:
        router.push('/403');
        message.error(codeMessage[error.response.status]);
        break;
      case 404:
        router.push('/404');
        message.error(codeMessage[error.response.status]);
        break;
      default:
        message.error('发生未知错误！！！');
    }
  } else if (JSON.stringify(error).indexOf('timeout') !== -1) {
    message.error('连接超时,请刷新试试');
  }
});
```


## 联系我

- 博客：<http://sayshy.com>
- 邮件：`satan31415#qq.com`
- Github：`https://github.com/satan31415`
