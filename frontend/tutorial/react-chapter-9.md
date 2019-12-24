#《最适合后端的 React Admin》第 9 章：修改 Demo 侧边栏菜单


## 侧边栏数据结构

#### Ajax 请求事件

- 参考：`src\common\menu.js`

#### Mock 数据

- 参考：`\mock\platform\menu.js`
- 节点格式如下：

```
{
  'result': [
    {
      'name': '首页',
      'path': '/',
      'icon': 'home',
      'children': null,
    },
    {
      'name': '角色管理',
      'path': null,
      'icon': 'star',
      'children': [
        {
          'name': '角色列表',
          'path': '/role',
          'icon': null,
          'children': null,
        },
      ],
    },
  ],
  'error_info': null,
  'is_success': true,
}
```

#### 节点展示逻辑处理

- 参考：`src\layouts\side.js`


## 联系我

- 博客：<http://sayshy.com>
- 邮件：`satan31415#qq.com`
- Github：`https://github.com/satan31415`
