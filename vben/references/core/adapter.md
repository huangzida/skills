# 适配器模式

适配器模式用于适配不同的 UI 框架。

**官方文档**：https://doc.vben.pro/guide/advance/adapter.html

## 适配器目录

```
src/adapter/
├── component/        # 组件适配器（自定义组件注册）
│   ├── index.ts      # 组件注册入口
│   ├── IpInput/      # IP 地址输入组件
│   ├── MacInput/     # MAC 地址输入组件
│   └── TreeSelectWithCustomInput.vue  # 可自定义输入的树选择
├── form.ts           # 表单适配器
├── vxe-table.ts      # 表格适配器
└── index.ts          # 适配器汇总
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

在 `src/adapter/component/index.ts` 中注册自定义组件到 `ComponentType` 联合类型和 `components` map：

```typescript
// 1. 异步加载声明
const IpInput = defineAsyncComponent(() => import('./IpInput/index.vue'));

// 2. 添加到 ComponentType
export type ComponentType = | 'IpInput' | ... | BaseFormComponentType;

// 3. 注册到 components map
async function initComponentAdapter() {
  const components = { IpInput, ... };
  globalShareState.setComponents(components);
}
```

### 内联组件（defineComponent）

#### InputNumberRange — 数字范围输入

两个 InputNumber + 分隔符，同一行显示。适用于人数范围、价格区间等场景。

```typescript
// schema 使用
{
  component: 'InputNumberRange',
  componentProps: {
    min: 1,                                    // 最小值，透传给两个 InputNumber
    placeholder: ['最小人数', '最大人数'],       // [左placeholder, 右placeholder]
  },
  fieldName: 'capacity',
  label: '容纳人数',
}
```

| 属性 | 类型 | 说明 |
|------|------|------|
| modelValue | `[number?, number?]` | `[起始值, 结束值]` |
| min | `number` | 最小值，默认 0 |
| placeholder | `[string?, string?]` | 左右 placeholder |

### Vue SFC 组件（异步加载）

#### IpInput — IP 地址输入

4 段输入框，自动跳转、粘贴解析，每段 0-255。

```typescript
{
  component: 'IpInput',
  fieldName: 'ipAddress',
  label: 'IP 地址',
}
```

| 属性 | 类型 | 说明 |
|------|------|------|
| value | `string` | IP 地址，如 `192.168.1.1` |
| placeholder | `string` | placeholder |
| disabled | `boolean` | 禁用 |

特性：
- 4 段 Input，`.` 分隔
- 输入满 3 位自动跳转下一段
- 按 `.` / `ArrowRight` / `Backspace` / `ArrowLeft` 自动跳转
- 支持粘贴完整 IP 地址自动填充

#### MacInput — MAC 地址输入

6 段输入框，自动跳转、粘贴解析，每段十六进制。

```typescript
{
  component: 'MacInput',
  fieldName: 'macAddress',
  label: 'MAC 地址',
}
```

| 属性 | 类型 | 说明 |
|------|------|------|
| value | `string` | MAC 地址，如 `AA:BB:CC:DD:EE:FF` |
| placeholder | `string` | placeholder |
| disabled | `boolean` | 禁用 |

特性：
- 6 段 Input，`:` 分隔
- 自动转大写，只允许十六进制
- 输入满 2 位自动跳转下一段
- 支持粘贴 `XX:XX:XX:XX:XX:XX` / `XX-XX-XX-XX-XX-XX` / `XXXXXXXXXXXX` 格式

#### TreeSelectWithCustomInput — 可自定义输入的树选择

TreeSelect 支持选择树节点或手动输入自定义值，值格式为路径数组 `['总部', '1楼']`。

```typescript
{
  component: 'TreeSelectWithCustomInput',
  componentProps: {
    treeData: [
      { label: '总部大厦', value: 'hq', children: [
        { label: '1楼', value: 'hq-1f' },
        { label: '2楼', value: 'hq-2f' },
      ]},
    ],
    treeDefaultExpandAll: true,
  },
  fieldName: 'location',
  label: '位置',
}
```

| 属性 | 类型 | 说明 |
|------|------|------|
| value | `string \| string[]` | 路径数组 `['总部', '1楼']` 或路径字符串 `'总部 / 1楼'` |
| treeData | `TreeSelectProps['treeData']` | 树数据 |
| allowClear | `boolean` | 允许清除，默认 true |
| treeDefaultExpandAll | `boolean` | 默认展开全部，默认 true |

特性：
- 选择树节点 → 自动生成完整路径
- 关闭下拉时输入非选项值 → 保留为自定义输入
- 显示格式 `A / B / C`

#### Popconfirm — 确认气泡

antdv-next 原生 Popconfirm 组件包装，无特殊处理。

```typescript
{
  component: 'Popconfirm',
  componentProps: {
    title: '确认删除？',
    onConfirm: handleDelete,
  },
  fieldName: 'confirm',
}
```

## 适配层默认行为

以下默认行为已在适配层处理，使用时无需重复配置：

### Upload — 默认 bg-background

`withPreviewUpload` 自动注入 `class: 'bg-background'`，无需在 schema 中手动加 `class` 和 `classes.list`。

### Textarea — showCount overflow 全局修复

全局 CSS `.overflow-hidden:has(.ant-input-textarea-show-count) { overflow: visible !important }` 自动处理 showCount 字数统计被裁剪问题，无需加 `formItemClass: 'textarea-form-item'` 和 scoped style。

## 使用适配器

```typescript
import { useVbenForm } from '#/adapter/form';
import { useVbenVxeGrid } from '#/adapter/vxe-table';

// 使用适配后的组件
const [Form] = useVbenForm({ ... });
const [Grid] = useVbenVxeGrid({ ... });
```
