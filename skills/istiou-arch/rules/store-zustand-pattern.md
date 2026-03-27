---
title: Zustand Store 规范
impact: MEDIUM
tags: [store, zustand, state]
---

## Zustand Store 规范

### Why it matters

规范的 Store 结构能够：
- 提高状态管理的可维护性
- 便于状态追踪和调试
- 避免不必要的重渲染
- 保持状态更新逻辑清晰

### Incorrect:

```typescript
// ❌ 直接修改状态
const useStore = create((set) => ({
  items: [],
  addItem: (item) => {
    const state = get();
    state.items.push(item); // 直接修改！
  },
}));

// ❌ 没有类型定义
const useStore = create((set) => ({
  data: null,
  setData: (data) => set({ data }),
}));

// ❌ 命名不规范
const useData = create((set) => ({
  // ...
}));

// ❌ 状态和操作混在一起，没有分组
const useStore = create((set) => ({
  // 50+ 个状态和操作
}));
```

### Correct:

```typescript
// ✅ 使用 interface 定义类型
import { create } from 'zustand';

interface StoreName {
  // 状态
  items: Item[];
  loading: boolean;
  error: string;

  // 操作
  setItems: (items: Item[]) => void;
  addItem: (item: Item) => void;
  setLoading: (loading: boolean) => void;
  setError: (error: string) => void;
  reset: () => void;
}

const useStoreName = create<StoreName>((set, get) => ({
  // 初始状态
  items: [],
  loading: false,
  error: '',

  // 状态更新方法
  setItems: (items) => set({ items }),

  addItem: (item) =>
    set((state) => ({ items: [...state.items, item] })),

  setLoading: (loading) => set({ loading }),
  setError: (error) => set({ error }),

  reset: () => set({ items: [], loading: false, error: '' }),
}));

export default useStoreName;
```

### Selector Pattern (避免不必要的重渲染)

```typescript
// ✅ 使用 selector 订阅特定状态
import { useStoreName } from './storeName';

// ❌ 订阅整个 store，任何变化都会重渲染
const Component = () => {
  const { items, loading } = useStoreName();
  // ...
};

// ✅ 只订阅 items，只有 items 变化才重渲染
const Component = () => {
  const items = useStoreName((state) => state.items);
  const loading = useStoreName((state) => state.loading);
  // ...
};

// ✅ 使用浅比较 selector
import { shallow } from 'zustand/shallow';

const Component = () => {
  const { items, loading } = useStoreName(
    (state) => ({ items: state.items, loading: state.loading }),
    shallow,
  );
  // ...
};
```

### Persist Middleware (持久化)

```typescript
// ✅ 使用 persist 中间件
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

type AuthStore = {
  token: string;
  user: UserInfo | null;
  setToken: (token: string) => void;
  setUser: (user: UserInfo) => void;
  clear: () => void;
};

const useAuthStore = create<AuthStore, [['zustand/persist', AuthStore]]>(
  persist(
    (set) => ({
      token: '',
      user: null,
      setToken: (token) => set({ token }),
      setUser: (user) => set({ user }),
      clear: () => set({ token: '', user: null }),
    }),
    {
      name: 'auth-storage', // localStorage key
      version: 1, // 版本号，用于迁移
      migrate: (persistedState: any, version: number) => {
        // 状态迁移逻辑
        if (version === 0) {
          return { ...persistedState, newField: 'default' };
        }
        return persistedState;
      },
    },
  ),
);

export default useAuthStore;
```

### Devtools Middleware (开发工具)

```typescript
// ✅ 使用 devtools 中间件
import { create } from 'zustand';
import { devtools } from 'zustand/middleware';

const useStore = create(
  devtools(
    (set) => ({
      // ...
    }),
    { name: 'MyStore' }, // 在 Redux DevTools 中显示的名称
  ),
);
```

### Store Organization

```
src/stores/
├── index.ts          # 统一导出
├── global.tsx        # 全局状态
├── user.ts           # 用户状态
└── module/           # 按模块分组
    └── store.ts
```

### Naming Conventions

- Store 文件: `useStoreName.ts` 或 `storeName.ts`
- Store hook: `useStoreName`
- Interface: `StoreName`
- Actions: 动词形式 `setXxx`, `addXxx`, `removeXxx`