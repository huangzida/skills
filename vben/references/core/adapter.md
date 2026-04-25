# 适配器模式

适配器模式用于适配不同的 UI 框架。

**官方文档**：https://doc.vben.pro/guide/advance/adapter.html

## 适配器目录

```
src/adapter/
├── form.ts           # 表单适配器
├── vxe-table.ts     # 表格适配器
└── index.ts         # 适配器汇总
```

## 表单适配器

```typescript
// src/adapter/form.ts
import { merge } from 'lodash-es';
import { useForm } from 'vben-form';
import type { VbenFormSchema } from '@vben/form-ui';

export function useVbenForm(options: any) {
  return useForm({
    // 公共配置
    ...options,
  });
}
```

## 表格适配器

```typescript
// src/adapter/vxe-table.ts
import { merge } from 'lodash-es';
import { useVxeGrid } from 'vben-vxe-table';
import type { VxeTableGridOptions } from '@vben/vxe-table';

export function useVbenVxeGrid(options: any) {
  return useVxeGrid({
    // 公共配置
    ...options,
  });
}
```

## 自定义组件注册

在适配器中注册自定义组件：

```typescript
// src/adapter/form.ts
import { useForm } from 'vben-form';

export function useVbenForm(options: any) {
  return useForm({
    components: {
      // 注册自定义组件
      CustomInput,
      CustomSelect,
    },
    ...options,
  });
}
```

## 使用适配器

```typescript
import { useVbenForm } from '#/adapter/form';
import { useVbenVxeGrid } from '#/adapter/vxe-table';

// 使用适配后的组件
const [Form] = useVbenForm({ ... });
const [Grid] = useVbenVxeGrid({ ... });
```
