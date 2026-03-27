---
title: Zustand Store 模板
impact: HIGH
tags: [store, zustand, state]
---

## Zustand Store 模板

### 基础 Store

```typescript
import { create } from 'zustand';

interface StoreName {
  data: DataType[];
  loading: boolean;
  error: string;

  setData: (data: DataType[]) => void;
  setLoading: (loading: boolean) => void;
  setError: (error: string) => void;
  fetchData: () => Promise<void>;
  reset: () => void;
}

const useStoreName = create<StoreName>((set, get) => ({
  data: [],
  loading: false,
  error: '',

  setData: (data) => set({ data }),
  setLoading: (loading) => set({ loading }),
  setError: (error) => set({ error }),

  fetchData: async () => {
    set({ loading: true, error: '' });
    try {
      const res = await fetchDataApi();
      set({ data: res.data || [] });
    } catch (error) {
      set({ error: error instanceof Error ? error.message : 'Unknown error' });
    } finally {
      set({ loading: false });
    }
  },

  reset: () => set({ data: [], loading: false, error: '' }),
}));

export default useStoreName;
```

### 带持久化的 Store

```typescript
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

type StoreName = {
  token: string;
  user: UserInfo | null;
  setToken: (token: string) => void;
  setUser: (user: UserInfo) => void;
  clear: () => void;
};

const useStoreName = create<StoreName, [['zustand/persist', StoreName]]>(
  persist(
    (set) => ({
      token: '',
      user: null,
      setToken: (token) => set({ token }),
      setUser: (user) => set({ user }),
      clear: () => set({ token: '', user: null }),
    }),
    {
      name: 'STORE_NAME', // localStorage key
    },
  ),
);

export default useStoreName;
```

### 计算属性的 Store

```typescript
import { create } from 'zustand';

interface StoreName {
  items: Item[];
  addItem: (item: Item) => void;
  getItemById: (id: string) => Item | undefined;
  getTotalCount: () => number;
}

const useStoreName = create<StoreName>((set, get) => ({
  items: [],

  addItem: (item) =>
    set((state) => ({ items: [...state.items, item] })),

  getItemById: (id) =>
    get().items.find((item) => item.id === id),

  getTotalCount: () =>
    get().items.length,
}));

export default useStoreName;
```