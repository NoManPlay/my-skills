---
title: 函数式组件规范
impact: HIGH
tags: [react, component, fc]
---

## 函数式组件规范

### Why it matters

统一的组件定义风格能够：
- 提高代码可读性
- 便于类型推断
- 支持更好的 IDE 提示
- 保持团队代码风格一致

### Incorrect:

```typescript
// ❌ 使用普通函数声明
function ComponentName(props: Props) {
  return <div>{props.title}</div>;
}

// ❌ 箭头函数但没有 FC 类型
const ComponentName = (props: Props) => {
  return <div>{props.title}</div>;
};

// ❌ 使用 React.FunctionComponent（过时）
const ComponentName: React.FunctionComponent<Props> = (props) => {
  return <div>{props.title}</div>;
};

// ❌ Props 使用 type 而非 interface
type ComponentNameProps = {
  title: string;
};
```

### Correct:

```typescript
// ✅ 使用 FC 和 interface
import { FC } from 'react';

interface ComponentNameProps {
  title: string;
  onClick?: () => void;
}

const ComponentName: FC<ComponentNameProps> = ({ title, onClick }) => {
  return (
    <div onClick={onClick}>
      {title}
    </div>
  );
};

export default ComponentName;

// ✅ 带子组件的组件
interface ContainerProps {
  children: React.ReactNode;
}

const Container: FC<ContainerProps> = ({ children }) => {
  return <div className="container">{children}</div>;
};
```

### Additional Notes

1. **Props 命名规范**: 使用 `ComponentNameProps` 格式
2. **可选属性**: 使用 `?` 标记可选属性
3. **默认值**: 在解构时提供默认值
   ```typescript
   const Button: FC<ButtonProps> = ({ type = 'default', size = 'middle' }) => {
     // ...
   };
   ```