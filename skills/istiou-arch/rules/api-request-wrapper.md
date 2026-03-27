---
title: API 请求封装规范
impact: HIGH
tags: [api, service, request, umijs]
---

## API 请求封装规范

### Why it matters

统一的 API 请求封装能够：
- 便于错误处理
- 支持请求/响应拦截
- 统一 API 路径管理
- 提供类型安全
- 便于后续维护和修改

### Incorrect:

```typescript
// ❌ 直接使用 fetch
const data = await fetch('/api/users').then(res => res.json());

// ❌ 直接使用 axios
import axios from 'axios';
const data = await axios.get('/api/users');

// ❌ 硬编码 URL
const data = await request('https://api.example.com/users');

// ❌ 没有类型定义
export async function getUsers() {
  return request('/api/users');
}

// ❌ 不使用 window.API_URL
export async function getUsers() {
  return request('/api/v1/users');
}
```

### Correct:

```typescript
// ✅ 使用 @umijs/max 的 request
import { request } from '@umijs/max';

/** 获取用户列表 GET /api/v1/users */
export async function getUsers(
  params: API.GetUsersParams,
  options?: { [key: string]: any },
) {
  return request<API.Response & { data?: API.User[] }>(
    `${window.API_URL}/users`,
    {
      method: 'GET',
      params: { ...params },
      ...(options || {}),
    },
  );
}

// ✅ 带路径参数的 API
export async function getUserId(
  params: API.GetUserIdParams,
  options?: { [key: string]: any },
) {
  const { id, ...queryParams } = params;
  return request<API.Response & { data?: API.User }>(
    `${window.API_URL}/users/${id}`,
    {
      method: 'GET',
      params: { ...queryParams },
      ...(options || {}),
    },
  );
}

// ✅ POST 请求
export async function postUsers(
  body: API.CreateUserParams,
  options?: { [key: string]: any },
) {
  return request<API.Response>(`${window.API_URL}/users`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    data: body,
    ...(options || {}),
  });
}

// ✅ PUT 请求
export async function putUserId(
  params: API.PutUserIdParams,
  body: API.UpdateUserParams,
  options?: { [key: string]: any },
) {
  const { id, ...queryParams } = params;
  return request<API.Response>(`${window.API_URL}/users/${id}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    params: { ...queryParams },
    data: body,
    ...(options || {}),
  });
}

// ✅ DELETE 请求
export async function deleteUserId(
  params: API.DeleteUserIdParams,
  options?: { [key: string]: any },
) {
  const { id, ...queryParams } = params;
  return request<API.Response>(`${window.API_URL}/users/${id}`, {
    method: 'DELETE',
    params: { ...queryParams },
    ...(options || {}),
  });
}
```

### File Organization

```
src/services/
├── index.ts          # 统一导出
├── typing.d.ts       # 全局类型定义
└── User/             # 按模块组织
    └── index.ts      # User 相关 API
```

### Type Definition Pattern

```typescript
// services/typing.d.ts
declare namespace API {
  // 通用响应结构
  interface Response {
    code: number;
    message?: string;
    success: boolean;
  }

  // 分页参数
  interface PaginationParams {
    current?: number;
    pageSize?: number;
  }

  // 分页结果
  interface PaginationResult<T> {
    list: T[];
    total: number;
  }
}
```

### Error Handling

```typescript
// ✅ 在组件中处理错误
import { message } from 'antd';

const handleFetch = async () => {
  try {
    const res = await getUsers({ current: 1, pageSize: 10 });
    if (res.success) {
      setData(res.data?.list || []);
    } else {
      message.error(res.message || '获取数据失败');
    }
  } catch (error) {
    message.error('网络请求失败');
  }
};
```