# 金枪鱼首页 Tuna Homepage

A homepage extension for Browsers.

金枪鱼首页正如 [应用创意：Chrome 共享首页](https://blog.meathill.com/apps/app-idea-share-bookmarks.html) 所说，是一个用来在群体内共享首页的插件。

# 用户角色

## 普通用户

1. 查看链接
2. 查看模块
3. 提交链接

## 管理员

1. 普通用户的全部功能
2. 审批链接

## 创始人

1. 创建本群的人（邀请制）
2. 管理员的全部功能
3. 用户管理：
    1. 任命/撤销管理员
    2. 用户审批，屏蔽
4. 群管理：
    1. 开放/封闭群

# 模块

以下交互数据格式均为 JSON。

返回的 HTTP Header 应该反映操作结果。如果操作失败，应该返回错误，格式为：

```json
{
  "coce": 1, // 错误码
  "message": "错误描述",
}
```

## 群管理 `/api/qun`

### 创建群

`POST`

#### 参数：

| 参数 | 类型 | 默认值 | 备注 |
| ---- | ---- | ---- | ---- |
| invite_code | string | | 邀请码，人工写表（临时） |

#### 响应

```json
{
  "code": 0,
  "qun_id": 1,
}
```

### 加载群内容

`GET /api/qun/:id`

后面的 `GET` 接口延续次设置

#### 参数

| 参数 | 类型 | 默认值 | 备注 |
| ---- | ---- | ---- | ---- |
| id | string | | 群 id |
| page | int | 0 | 页码 |
| `[field]` | string | | 按字段搜索 |
| asc/desc | `[field]` | | 按字段排序 |

#### 响应

```json
{
  "code": 0,
  "count": 100, // 符合要求的条目数
  "data": [....], // 列表
}
```

### 添加链接

`POST /api/qun/:id/link`

添加链接后，自动抓取网页内容，生成 title、excerpt

#### 参数

| 参数 | 类型 | 默认值 | 备注 |
| ---- | ---- | ---- | ---- |
| id | string | | 群 id |
| link | string | 0 | 网页 Url |
| description | string | | 描述，用户主观上的感觉 |

#### 响应

```json
{
  "code": 0,
  "link_id": 1,
}
```

## 用户管理 `/api/user/`

### 登录

1. 使用 GitHub 登录，新用户自动创建。
2. 登陆后，将 session 保存到 cookie，请求时需进行身份验证
3. 一个用户可以在不同设备上登录多次


