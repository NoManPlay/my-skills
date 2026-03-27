---
title: 禁止使用 any 类型
impact: CRITICAL
tags: [typescript, types]
---

## 禁止使用 any 类型

### Why it matters

使用 `any` 会完全关闭 TypeScript 的类型检查，导致：
- 丢失类型安全
- IDE 无法提供准确的自动补全
- 运行时错误风险增加
- 代码可维护性下降

### Incorrect:

```typescript
// ❌ 使用 any
const handleData = (data: any) => {
  return data.name; // 没有类型检查
};

// ❌ any 数组
const items: any[] = [];

// ❌ any 作为泛型
const result: Record<string, any> = {};
```

### Correct:

```typescript
// ✅ 使用具体类型
interface DataType {
  name: string;
  id: number;
}

const handleData = (data: DataType) => {
  return data.name;
};

// ✅ 泛型类型
interface Item {
  id: string;
  value: number;
}
const items: Item[] = [];

// ✅ 使用 unknown 配合类型守卫
const parseJson = (str: string): unknown => {
  return JSON.parse(str);
};

// ✅ 对于不确定的结构，使用 Record
const result: Record<string, string | number> = {};
```

### Exceptions

以下情况可以临时使用 `any`（但应添加注释说明原因）：
- 第三方库缺少类型定义
- 迁移遗留代码时的过渡方案
- 测试文件中的 mock 数据