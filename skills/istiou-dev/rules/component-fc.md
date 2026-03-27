---
title: 函数式组件模板
impact: HIGH
tags: [component, react, typescript]
---

## 函数式组件模板

### 基础组件

```typescript
import { FC } from 'react';

interface ComponentNameProps {
  title?: string;
  onAction?: () => void;
}

const ComponentName: FC<ComponentNameProps> = ({ title, onAction }) => {
  return (
    <div>
      {title && <h3>{title}</h3>}
      <button onClick={onAction}>Click Me</button>
    </div>
  );
};

export default ComponentName;
```

### 带状态的组件

```typescript
import { FC, useState, useEffect } from 'react';
import { useBoolean } from 'ahooks';
import { Spin, Empty } from 'antd';

interface ComponentNameProps {
  id: string;
}

const ComponentName: FC<ComponentNameProps> = ({ id }) => {
  const [loading, { setTrue, setFalse }] = useBoolean(false);
  const [data, setData] = useState<DataType | null>(null);

  useEffect(() => {
    fetchData();
  }, [id]);

  const fetchData = async () => {
    setTrue();
    try {
      const res = await apiCall(id);
      setData(res.data);
    } catch (error) {
      console.error('Failed to fetch data:', error);
    } finally {
      setFalse();
    }
  };

  if (loading) {
    return <Spin />;
  }

  if (!data) {
    return <Empty description="No data available" />;
  }

  return <div>{/* Render data */}</div>;
};

export default ComponentName;
```

### 带样式的组件 (useEmotionCss)

```typescript
import { FC } from 'react';
import { useEmotionCss } from '@ant-design/use-emotion-css';

interface ComponentNameProps {
  children: React.ReactNode;
}

const ComponentName: FC<ComponentNameProps> = ({ children }) => {
  const containerCls = useEmotionCss(() => ({
    padding: '16px 24px',
    backgroundColor: '#fff',
    borderRadius: '8px',
    boxShadow: '0 2px 8px rgba(0, 0, 0, 0.1)',
  }));

  return <div className={containerCls}>{children}</div>;
};

export default ComponentName;
```