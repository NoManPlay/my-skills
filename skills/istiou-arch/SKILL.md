---
name: istiou-arch
description: Istiou Web 项目架构规范检查。用于代码审查、规范验证、最佳实践检查。触发关键词：检查代码、代码审查、规范验证、代码问题、审查代码。
license: MIT
metadata:
  author: istiou-team
  version: "1.0.0"
  tags: [istiou, umijs, react, typescript, code-review, lint]
---

# Istiou 架构规范

用于 Istiou Web 项目的代码规范检查和最佳实践验证。

## When to Apply

- 代码审查时检查规范符合性
- 提交前验证代码质量
- 重构时识别问题代码
- 新人培训时进行规范指导

## Rule Categories by Priority

| Priority | Category | Impact | Prefix |
|----------|----------|--------|--------|
| 1 | TypeScript | CRITICAL | `ts-` |
| 2 | React Patterns | HIGH | `react-` |
| 3 | API/Service | HIGH | `api-` |
| 4 | State Management | MEDIUM | `store-` |
| 5 | File Organization | MEDIUM | `file-` |
| 6 | Import/Export | LOW | `import-` |

---

## S - Scope

**Target**: UmiJS 4.x + React 18 + TypeScript 项目

**Focus**: 代码规范和最佳实践

**Out of Scope**: 不包含业务逻辑审查

---

## P - Process

1. **Read Current Code** - 读取待检查的代码文件
2. **Compare with Rules** - 根据 rules/ 目录下的规则逐一检查
3. **Rate Confidence** - 对每个问题给出置信度评分 (0-100)
4. **Report Issues** - 只报告高置信度的问题 (>80 分)
5. **Provide Fixes** - 提供具体的修复建议

---

## O - Output

提供清晰的审查报告，格式如下：

```
### 代码规范审查

发现 N 个问题:

1. **[CRITICAL] 使用了 any 类型**
   - 位置: `src/components/Comp/index.tsx:12`
   - 说明: 禁止使用 any 类型，会丢失类型安全
   - 修复建议: 定义具体的接口或类型

2. **[HIGH] 组件未使用 FC 定义**
   - 位置: `src/pages/Page.tsx:5`
   - 说明: 组件应使用 `FC<Props>` 类型定义
   - 修复建议: 修改为 `const Component: FC<Props> = ({ ... })`

---

参考: rules/ 目录下的规范文档
```

---

## Quick Reference

### 1. TypeScript (CRITICAL)
- `ts-no-any` - 禁止使用 any 类型
- `ts-explicit-return` - 函数必须有显式返回类型
- `ts-interface-props` - Props 必须使用 interface 定义
- `ts-no-ts-ignore` - 禁止使用 @ts-ignore

### 2. React Patterns (HIGH)
- `react-fc-component` - 使用 FC 定义组件
- `react-hooks-rules` - Hooks 使用规范
- `react-no-index-key` - 禁止使用 index 作为 key
- `react-prop-types` - 正确的 Props 定义

### 3. API/Service (HIGH)
- `api-request-wrapper` - 使用 @umijs/max request
- `api-type-definition` - API 必须有类型定义
- `api-error-handling` - 错误处理规范
- `api-file-location` - API 服务位置规范

### 4. State Management (MEDIUM)
- `store-zustand-pattern` - Zustand Store 结构规范
- `store-no-mutation` - 禁止直接修改状态
- `store-selectors` - 使用 selector 避免不必要的重渲染

### 5. File Organization (MEDIUM)
- `file-component-index` - 组件文件使用 index.tsx
- `file-service-directory` - API 服务按模块分目录
- `file-naming-convention` - 文件命名规范

### 6. Import/Export (LOW)
- `import-order` - 导入语句排序规范
- `import-no-barrel` - 避免桶文件导入
- `export-style` - 导出风格统一

---

## 检查清单

在进行代码审查时，使用以下清单:

- [ ] 所有类型都已正确定义，未使用 any
- [ ] 组件使用 FC 类型定义
- [ ] Hooks 使用符合规范
- [ ] API 调用使用 @umijs/max request
- [ ] 有完整的错误处理
- [ ] 文件组织符合项目结构
- [ ] 导入语句正确排序

详见 rules/ 目录下的各规则文件。