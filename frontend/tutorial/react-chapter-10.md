---
title: 《最适合后端的 React Admin》第 10 章：新模块的 CURD 过程
date: 2018-09-11 20:21:23
description: "本系列更多的作用是如何使用和引导现代前端开发的思路"
categories: [React]
tags: [React]
---


## 开发注意点

### 什么时候需要重启前端服务

- 修改：`.umirc.js`
- 不然任何时候你都只要按 `Ctrl + S`，然后刷新浏览器即可（浏览器有时候会监控到会自动帮你刷新）


### 关于注释

- 参考：`src\pages\role\index.js`


### 懂得区分不同的开发阶段

#### Mock 开发阶段

- `src\request\api.js` 确保该值为这个：

```
target: '',
```

#### 本地前后端联调开发阶段

- `src\request\api.js` 确保该值为这个：

```
target: 'http://localhost:28081',
```

-------------------------------------------------------------------


## 如何添加 Mock 接口

#### 添加菜单节点

- `\mock\platform\menu.js` 是 Mock 页面右侧菜单

#### 新模块的 CURD Mock

- 复制：`\mock\platform\role` 整个目录
- `page.js` 是 Mock 分页接口 
- `object.js` 是 Mock 查看详情接口 
- `operate.js` 是 Mock 增删改接口 


-------------------------------------------------------------------

## 开始创建 CURD 页面


### 参考 src\pages\role 模块介绍

- **新手可以直接复制该模块进行开发**
- 主要文件介绍：

```
src\pages\role\index.js，页面结构
src\pages\role\services\index.js，管理 ajax 请求 URL 和参数
src\pages\role\models\index.js，管理 ajax 请求逻辑
```

- ajax 请求的 URL 统一放在 `src\request\api.js` 文件下，通过引入来使用, 这样方便管理整个项目的请求地址

### 一个新模块开发流程顺序

- 以角色模块为例：
- 1. `src\pages\role\model\index.js`, 该 JS 文件中 `namespace` 必须是全局唯一的值，不能有重复。
- 2. `src\request\api.js` 补充新模块的各种请求地址
- 3. `src\pages\role\services\index.js`，补充新模块所需的各个请求对应的方法（这里会用到 2. 的参数）
- 4. `src\pages\role\models\index.js`，补充新模块所需的各个请求对应的 ajax 具体方法（这里会用到 3. 的参数）
- 5. `src\pages\role\index.js`，补充新模块所需的页面元素和事件（这里会用到 1. 和 4. 参数）
- 这样的目录结构得到的页面路由地址：<http://localhost:8001/role>


### 列表页面的搜索区域 

- 组件位置：`src\pages\role\index.js`

```
{/* ===========================搜索区 start=========================== */}
<Card>
  <Search
    row={4}
    getSearchValue={this.eventSearchValue}
    options={[
      { key: '1', id: 'id', type: 'input', name: 'id' },
      { key: '2', id: 'role_name', type: 'input', name: '角色名称' },
      { key: '4', id: 'create_date', type: 'rangeDate', name: '创建时间', format: 'YYYY-MM-DD' },
    ]}
  />
</Card>
{/* ===========================搜索区 end=========================== */}
```

- 可修改参数:
	- row 每行显示几个, 只能填 3 和 4（组件封装好只能填这两个数字） 
	- options 表示搜索参数:
		- id:搜索字段名
		- type: 
			- 搜索框类型 输入框(input)
			- 时间范围选择框(rangeDate)
			- 下拉框(select)
		- key 和 id 都必须唯一，不能重复



### 表格列数据

- 组件位置：`src\pages\role\index.js`

```
{/*===========================表格列属性值 start===========================*/}
const columns = [
  { title: 'id', dataIndex: 'id' },
  { title: '角色名称', dataIndex: 'role_name' },
  { title: '角色描述', dataIndex: 'role_introduce' },
  {
    title: '创建时间', dataIndex: 'create_date', render: (text) => (
      moment(text).format('YYYY-MM-DD HH:mm:ss')
    ),
  },
  {
    title: '操作', width: 180, dataIndex: 'cz', render: (text, record) => (
      <span>
      <a onClick={() => this.eventOpenDetailByEdit(record.id)}>修改</a>
      <Divider type='vertical'/>
      <a onClick={() => this.eventOpenDetailByView(record.id)}>详情</a>
      <Divider type='vertical'/>
      <Popconfirm title={'确认删除'} okText='确认' cancelText='取消' onConfirm={() => this.serviceBatchDelete(record.id)}>
        <a>删除</a>
      </Popconfirm>
    </span>
    ),
  },
];
{/*===========================表格列属性值 end===========================*/}
```


### 新建 / 修改的模态框表单

- 组件位置：`src\pages\role\index.js`

```
{/* ===========================新增/编辑模态窗口 start=========================== */}
<NewForm
  visible={this.state.visible}
  onCancel={this.eventSubmitCancel}
  onOk={this.eventSubmitOk}
  loading={editStatus ? detailLoading : false}
  defaultValue={defaultValue}
  disabled={this.state.disabled}
  options={[
    { rule: { required: true }, name: '角色名称', id: 'role_name', type: 'input' },
    { rule: { required: false }, name: '角色描述', id: 'role_introduce', type: 'textarea' },
  ]}
/>
{/* ===========================新增/编辑模态窗口 end=========================== */}
```


