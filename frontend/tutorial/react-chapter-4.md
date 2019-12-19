---
title: 《最适合后端的 React Admin》第 3 章：与后端开发的规范约定
date: 2018-04-07 19:32:55
description: "本系列更多的作用是如何使用和引导现代前端开发的思路"
categories: [React]
tags: [React]
---

## 字典数据：全球国家 JSON

- 数据来源：<http://qiaolevip.iteye.com/blog/2149765>

## 字典数据：中华人民共和国行政区划 JSON

- 数据来源：<https://github.com/modood/Administrative-divisions-of-China>

--------------------------------------------------------------------------------------------------------

## 与后端数据交互的请求约定

- 不使用 RESTful 风格，只用 GET 和 POST 两种请求。

#### 简单查询接口 -- GET 请求

- Get 请求适合只能传输几个简单的 key value 类型值在 URL 地址上

#### 复杂查询、编辑、新增、删除、批量删除 -- POST 请求

- 复杂查询：包含请求参数中有：数组、对象，以及太多请求参数
- 请求参数全部为 JSON
- 请求参数命名全部采用下划线，比如：`role_name`


--------------------------------------------------------------------------------------------------------

## 与后端数据交互的响应约定

#### 注意

- 所有请求场景的 HTTP 响应码都是：200，所有不要拿 HTTP 的响应码做任何判断。
- 对应的状态值代表什么含义，可以从后端返回的 code 里面拿，这个可以前后端约定后自己瞎搞。
- 时间用的是整型。
- 登录后，所有请求头都要带上：`platform_token`


#### 常量数据（local storge）

```
{
  "constant_is_delete": [
    {
      "label": "未删除",
      "value": "0"
    },
    {
      "label": "已删除",
      "value": "0"
    }
  ],
  "constant_sex": [
    {
      "label": "保密",
      "value": "0"
    },
    {
      "label": "男",
      "value": "1"
    },
    {
      "label": "女",
      "value": "2"
    }
  ]
}
```



#### 常量数据（session storge）

```
{
  "constant_user_role": [
    {
      "label": "系统高级管理员",
      "value": "top_admin_role"
    },
    {
      "label": "系统管理员",
      "value": "admin_role"
    },
    {
      "label": "操作员",
      "value": "operator_role"
    },
    {
      "label": "默认用户",
      "value": "default_role"
    }
  ]
}
```


#### 未登录

```
{
  "error_info": {
    "code": 401,
    "msg": "请还未登录，请先登录",
    "date": 1536739520111
  },
  "result": null,
  "is_success": false
}
```

#### 登录失败

```
{
  "error_info": {
    "code": 401,
    "msg": "用户名或密码不正确",
    "date": 1536739520111
  },
  "result": null,
  "is_success": false
}
```

#### 登录成功

```
{
  "error_info": null,
  "result": {
    "platform_token": "cd534924c12561de4eb948531a7fdeb9"
  },
  "is_success": true
}
```

#### 查询单个对象

```
{
  "result": {
    "id": "417454619141211111",
    "role_name": "管理员1",
    "role_introduce": "管理员介绍1",
    "create_date": 1536739520111,
    "update_date": null
  },
  "error_info": null,
  "is_success": true
}
```

#### 查询列表

```
{
  "result": [
    {
      "id": "417454619141211111",
      "role_name": "管理员1",
      "role_introduce": "管理员介绍1",
      "create_date": 1536739520111,
      "update_date": null
    },
    {
      "id": "417454619141211112",
      "role_name": "管理员2",
      "role_introduce": "管理员介绍2",
      "create_date": 1536739110111,
      "update_date": null
    }
  ],
  "error_info": null,
  "is_success": true
}
```

#### 返回分页数据

```
{
  "result": {
    "items": [
      {
        "id": "417454619141211111",
        "role_name": "管理员1",
        "role_introduce": "管理员介绍1",
        "create_date": 1536739520111,
        "update_date": null
      },
      {
        "id": "417454619141211112",
        "role_name": "管理员2",
        "role_introduce": "管理员介绍2",
        "create_date": 1536739110111,
        "update_date": null
      }
    ],
    "total_count": 1,
    "page_index": 2,
    "page_size": 5
  },
  "error_info": null,
  "is_success": true
}
```


#### 添加数据成功

```
{
  "result": {
    "msg": "操作成功",
    "date": 1536768054052,
    "code": 200
  },
  "error_info": null,
  "is_success": true
}
```

#### 修改数据成功

```
{
  "result": {
    "msg": "操作成功",
    "date": 1536768054052,
    "code": 200
  },
  "error_info": null,
  "is_success": true
}
```

#### 删除数据成功

```
{
  "result": {
    "msg": "操作成功",
    "date": 1536768054052,
    "code": 200
  },
  "error_info": null,
  "is_success": true
}
```

#### 请求格式不正确

```
{
  "result": null,
  "error_info": {
    "code": 400,
    "msg": "请求格式不正确",
    "date": 1536768082587,
    "items": [
      {
        "message": "角色名不能为空",
        "member": "addRoleDTO.roleName"
      }
    ]
  },
  "is_success": false
}
```


#### 系统内部异常


```
{
  "result": null,
  "error_info": {
    "code": 500,
    "msg": "因为服务器异常，添加失败",
    "date": 1536768096796
  },
  "is_success": false
}
```


## 联系我

- 博客：<http://sayshy.com>
- 邮件：`satan31415#qq.com`
- Github：`https://github.com/satan31415`
