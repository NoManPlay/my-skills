---
title: UmiJS 路由配置模板
impact: HIGH
tags: [route, umijs, config]
---

## UmiJS 路由配置模板

### 模块路由配置

```typescript
// config/routes/app.ts
export default [
  {
    path: '/app',
    name: '应用管理',
    icon: 'AppstoreOutlined',
    routes: [
      {
        path: '/app/list',
        name: '应用列表',
        component: './App/List',
      },
      {
        path: '/app/info/:id',
        name: '应用详情',
        component: './App/Info',
        hideInMenu: true,
      },
      {
        path: '/app/release',
        name: '发布管理',
        component: './App/Release',
      },
    ],
  },
];
```

### 路由入口配置

```typescript
// config/routes/index.ts
import appRoutes from './app';
import devopsRoutes from './devops';
import systemRoutes from './system';

export default [
  {
    path: '/login',
    layout: false,
    component: './Login',
  },
  {
    path: '/',
    redirect: '/app/list',
  },
  ...appRoutes,
  ...devopsRoutes,
  ...systemRoutes,
  {
    path: '*',
    layout: false,
    component: './404',
  },
];
```

### 带权限的路由

```typescript
export default [
  {
    path: '/admin',
    name: '系统管理',
    icon: 'SettingOutlined',
    access: 'canAdmin', // 权限标识
    routes: [
      {
        path: '/admin/users',
        name: '用户管理',
        component: './Admin/Users',
        access: 'canAdmin',
      },
    ],
  },
];
```

### 路由包装器 (Wrapper)

```typescript
// 用于路由级别的权限检查或其他处理
export default [
  {
    path: '/protected',
    name: '受保护页面',
    component: './Protected',
    wrappers: ['@/wrappers/auth'],
  },
];

// src/wrappers/auth.tsx
import { Navigate, useModel } from '@umijs/max';

export default (props: any) => {
  const { initialState } = useModel('@@initialState');
  const { currentUser } = initialState || {};

  if (!currentUser) {
    return <Navigate to="/login" replace />;
  }

  return props.children;
};
```