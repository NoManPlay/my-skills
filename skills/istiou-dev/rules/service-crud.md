---
title: CRUD API 服务模板
impact: HIGH
tags: [service, api, umijs]
---

## CRUD API 服务模板

### 基础 CRUD 服务

```typescript
// @ts-ignore
/* eslint-disable */
import { request } from '@umijs/max';

/** 查询列表 GET /api/v1/resources */
export async function getResources(
  params: API.GetResourcesParams,
  options?: { [key: string]: any },
) {
  return request<API.Response & { data?: API.ResourceList }>(
    `${window.API_URL}/resources`,
    {
      method: 'GET',
      params: { ...params },
      ...(options || {}),
    },
  );
}

/** 创建资源 POST /api/v1/resources */
export async function postResources(
  body: API.Resource,
  options?: { [key: string]: any },
) {
  return request<API.Response>(`${window.API_URL}/resources`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    data: body,
    ...(options || {}),
  });
}

/** 查询详情 GET /api/v1/resources/${id} */
export async function getResourceId(
  params: API.GetResourceIdParams,
  options?: { [key: string]: any },
) {
  const { id, ...queryParams } = params;
  return request<API.Response & { data?: API.Resource }>(
    `${window.API_URL}/resources/${id}`,
    {
      method: 'GET',
      params: { ...queryParams },
      ...(options || {}),
    },
  );
}

/** 更新资源 PUT /api/v1/resources/${id} */
export async function putResourceId(
  params: API.PutResourceIdParams,
  body: API.Resource,
  options?: { [key: string]: any },
) {
  const { id, ...queryParams } = params;
  return request<API.Response>(`${window.API_URL}/resources/${id}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    params: { ...queryParams },
    data: body,
    ...(options || {}),
  });
}

/** 删除资源 DELETE /api/v1/resources/${id} */
export async function deleteResourceId(
  params: API.DeleteResourceIdParams,
  options?: { [key: string]: any },
) {
  const { id, ...queryParams } = params;
  return request<API.Response>(`${window.API_URL}/resources/${id}`, {
    method: 'DELETE',
    params: { ...queryParams },
    ...(options || {}),
  });
}
```

### 服务聚合导出

```typescript
// services/Resource/index.ts
import * as Resource from './Resource';

export default {
  Resource,
};
```