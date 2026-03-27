---
name: istiou-dev
description: Istiou Web 项目代码生成助手。基于 UmiJS + React + TypeScript 技术栈，用于快速生成符合项目规范的组件、API 服务、Zustand Store、路由配置等代码。触发关键词：生成组件、创建页面、新建服务、添加 Store、开发组件、开发页面。
license: MIT
metadata:
  author: istiou-team
  version: "1.0.0"
  tags: [istiou, umijs, react, typescript, code-generation, ant-design]
---

# Istiou 开发助手

用于 Istiou Web 项目（UmiJS 4.x + React 18 + TypeScript + Ant Design Pro）的代码生成。

## When to Apply

- 生成新的 React 组件
- 创建 API 服务层代码
- 添加 Zustand 状态管理
- 配置 UmiJS 路由
- 开发新页面

## Rule Categories

| Priority | Category | Prefix |
|----------|----------|--------|
| 1 | Component | `component-` |
| 2 | Service/API | `service-` |
| 3 | State Management | `store-` |
| 4 | Routing | `route-` |

## Quick Reference

### 1. Component (组件)
- `component-fc` - 函数式组件模板
- `component-props` - Props 定义规范
- `component-style` - 样式处理 (@ant-design/use-emotion-css)
- `component-table` - 表格组件
- `component-form` - 表单组件

### 2. Service/API (服务)
- `service-crud` - CRUD API 模板
- `service-request` - 请求工具配置

### 3. State Management (状态管理)
- `store-basic` - 基础 Zustand Store
- `store-persist` - 带持久化的 Store

### 4. Routing (路由)
- `route-module` - 模块路由配置
- `route-wrapper` - 路由包装器

---

## S - Scope

**Target Projects**: UmiJS 4.x + React 18 + TypeScript 项目

**Supported**:
- React 函数式组件 (FC)
- Ant Design / Ant Design Pro
- Zustand 状态管理
- @umijs/max request API

**Out of Scope**:
- 非 TypeScript 项目
- 旧版 UmiJS 3.x 或更低
- 类组件 (Class Components)

---

## P - Process

1. **Analyze Requirements** - 理解用户需要生成什么类型的代码
2. **Select Template** - 从 rules/ 目录选择合适的模板
3. **Adapt to Project** - 根据项目当前结构调整生成的代码
4. **Provide Explanations** - 解释代码结构和使用方式

---

## O - Output

生成完整的、可直接使用的代码，并提供：
- 代码解释
- 使用示例
- 扩展建议

---

## 技术栈参考

### 项目依赖
- **框架**: UmiJS 4.x + React 18
- **语言**: TypeScript 4.8+
- **UI**: Ant Design 5.x / Ant Design Pro 2.x
- **状态管理**: Zustand 5.x
- **工具库**: ahooks
- **样式**: @ant-design/use-emotion-css

### 目录结构
```
src/
├── components/       # 可复用组件
├── pages/           # 页面
├── services/        # API 服务
├── stores/          # Zustand Stores
├── layouts/         # 布局
├── hooks/           # 自定义 Hooks
├── utils/           # 工具函数
├── types/           # TypeScript 类型
└── app.tsx          # 应用入口
```

---

## 注意事项

1. **参考现有代码**: 生成代码前，先查看项目中已有的类似代码，保持风格一致
2. **类型安全**: 始终提供完整的 TypeScript 类型定义
3. **组件复用**: 优先使用项目已有的组件库
4. **错误处理**: API 调用需要包含适当的错误处理

详见 rules/ 目录下的各规则文件。