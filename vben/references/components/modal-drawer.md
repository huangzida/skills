# Modal/Drawer 组件

弹窗和抽屉组件，用于创建模态对话框和侧边抽屉。

**官方文档**：
- Modal: https://doc.vben.pro/components/common-ui/vben-modal.html
- Drawer: https://doc.vben.pro/components/common-ui/vben-drawer.html

**Playground**：`playground/src/views/examples/modal/`

## ⚠️ 重要规则

### 弹窗表单必须使用 useVbenForm

> **禁止在弹窗/抽屉中手写原生表单元素（`<input>`、`<select>`、`<textarea>`）！**
>
> 必须使用 `useVbenForm` 的 schema 定义表单字段。

✅ 正确写法：
```typescript
const [Form, formApi] = useVbenForm({
  schema: [
    { component: 'Input', fieldName: 'name', label: '名称', rules: 'required' },
    { component: 'Select', fieldName: 'type', label: '类型',
      componentProps: { options: [...] } },
  ],
});

const [Modal, modalApi] = useVbenModal({
  onConfirm() { formApi.submitForm(); },
});
```

❌ 错误写法：
```html
<!-- 禁止在弹窗模板中手写表单 -->
<input v-model="formData.name" class="..." />
<select v-model="formData.type" class="...">...</select>
<textarea v-model="formData.reason" class="..."></textarea>
```

### 表单分隔使用 Divider

> **禁止用 `<h4>` 标题文字分隔表单模块！** 使用 `component: 'Divider'`。

```typescript
function useDivider(title: string) {
  return {
    component: 'Divider',
    fieldName: `divider_${title}`,
    formItemClass: 'col-span-2',
    labelWidth: 0,
    renderComponentContent: () => ({ default: () => h('div', title) }),
  };
}

// 在 schema 中使用
schema: [
  useDivider('基本信息'),
  { component: 'Input', fieldName: 'name', label: '名称' },
  useDivider('采购信息'),
  { component: 'DatePicker', fieldName: 'date', label: '日期' },
]
```

### 上传组件使用适配器注册的 Upload

> **禁止在弹窗中自定义 Upload 模板！** 使用 schema 中的 `component: 'Upload'`。
>
> 适配器已注册 `Upload` 组件，自带图片预览、裁剪、大小限制等功能。

```typescript
// ✅ 正确：在 schema 中使用
{
  component: 'Upload',
  fieldName: 'attachments',
  label: '上传附件',
  formItemClass: 'col-span-2',
  componentProps: {
    maxCount: 5,
    accept: '.jpg,.png,.pdf,.doc',
  },
}

// ❌ 错误：在模板中自定义 Upload
// <Upload :max-count="5" accept=".jpg,.png">
//   <button>上传</button>
// </Upload>
```

### 开始/结束时间合并为 RangePicker

> **禁止用两个独立 DatePicker 表示开始和结束时间！**
>
> 同时存在开始时间 + 结束时间时，必须合并为 `RangePicker` + `fieldMappingTime`。

```typescript
// ✅ 正确：RangePicker + fieldMappingTime
const [Form, formApi] = useVbenForm({
  schema: [
    {
      component: 'RangePicker',
      fieldName: 'dateRange',
      label: '日期范围',
    },
  ],
  fieldMappingTime: [
    ['dateRange', 'startTime', 'endTime'],
  ],
});
// formApi.getValues() => { startTime: '2025-01-01', endTime: '2025-12-31', ... }

// ❌ 错误：两个独立 DatePicker
// { component: 'DatePicker', fieldName: 'startDate', label: '开始日期' },
// { component: 'DatePicker', fieldName: 'endDate', label: '结束日期' },
```

### 配置统一原则

> **禁止在 `<Modal>` / `<Drawer>` 模板上设置属性！** 所有配置统一在 `useVbenModal` / `useVbenForm` 参数中设置，保持模板的整洁和纯粹。

✅ 正确写法：所有配置在脚本中统一管理，模板只负责结构。

```typescript
<script setup lang="ts">
import { useVbenModal } from '@vben/common-ui';
import { useVbenForm } from '#/adapter/form';

const [Form, formApi] = useVbenForm({
  schema: [...],
  handleSubmit: onSubmit,
});

const [Modal, modalApi] = useVbenModal({
  fullscreenButton: true,
  draggable: true,
  onConfirm() { onSubmit(); },
  onOpenChange(isOpen) {
    if (isOpen) {
      const data = modalApi.getData<RecordType>();
      modalApi.setState({
        title: data?.id ? $t('common.edit') : $t('common.create'),
      });
      if (data?.id) {
        formApi.setValues(data);
      }
    }
  },
});
</script>

<template>
  <Modal>
    <Form />
  </Modal>
</template>
```

❌ 错误写法：在模板 `<Modal>` 上设置属性，配置分散且会覆盖脚本中的配置。

```html
<template>
  <!-- ❌ 禁止在模板上设置属性！ -->
  <Modal :title="getTitle" :fullscreen-button="false">
    <Form />
  </Modal>
</template>
```

关键点：
- 模板中的 props 优先级高于 `useVbenModal` 参数，会覆盖脚本中的配置
- 动态标题使用 `modalApi.setState({ title })` 在 `onOpenChange` 中设置
- 模板保持 `<Modal><Form /></Modal>` 最简结构，不添加任何属性

## ❌ 常见错误

### 错误：在子组件中使用 useVbenDrawer/useVbenModal

**❌ 错误写法**：子组件直接使用 `useVbenDrawer`

```typescript
// ChildDrawer.vue - 错误！
// 会导致运行时错误：Failed to resolve component: Drawer
<script lang="ts" setup>
const [Drawer, drawerApi] = useVbenDrawer({ ... });
</script>
<template>
  <Drawer>内容</Drawer>
</template>
```

**✅ 正确写法**：子组件通过 `defineExpose` 暴露 API

```typescript
// ChildDrawer.vue - 正确！
<script lang="ts" setup>
// 不要在这里调用 useVbenDrawer

const emit = defineEmits<{
  edit: [data: any];
}>();

// 通过 onOpenChange 获取数据
function handleEdit() {
  emit('edit', currentData);
}

defineExpose({
  // 如果需要暴露给父组件
});
</script>
<template>
  <div>内容</div>
</template>
```

### 错误：父子组件状态管理混乱

**❌ 错误写法**：通过 reactive 状态传递

```typescript
// 父组件 - 混乱的状态管理，导致组件嵌套问题
const drawerState = reactive({ isOpen: false, data: null });
```

**✅ 正确写法**：使用 drawerApi.setData()

```typescript
// 父组件
const [Drawer, drawerApi] = useVbenDrawer({
  connectedComponent: ChildDrawer,
});

// 打开时设置数据
function openDrawer(data) {
  drawerApi.setData(data).open();
}
```

## useVbenDrawer

抽屉组件，用于表单编辑等场景。

### 基础用法

```typescript
const [Drawer, drawerApi] = useVbenDrawer({
  title: '抽屉标题',
  onConfirm: () => {
    // 确认逻辑
  },
});
```

### 配合表单使用

```typescript
import { useVbenForm } from '#/adapter/form';

const [Form, formApi] = useVbenForm({ schema: [...] });

const [Drawer, drawerApi] = useVbenDrawer({
  connectedComponent: Form,
  onConfirm: async () => {
    const { valid } = await formApi.validate();
    if (!valid) return;
    const values = await formApi.getValues();
    // 保存数据
    drawerApi.close();
  },
});
```

### 常用配置

| 配置 | 说明 |
|------|------|
| `title` | 抽屉标题 |
| `connectedComponent` | 关联的组件 |
| `destroyOnClose` | 关闭时销毁内容 |
| `onConfirm` | 确认回调 |
| `onOpenChange` | 打开/关闭变化回调 |

### 抽屉 API

| 方法 | 说明 |
|------|------|
| `drawerApi.open()` | 打开抽屉 |
| `drawerApi.close()` | 关闭抽屉 |
| `drawerApi.lock()` | 锁定（禁用关闭） |
| `drawerApi.unlock()` | 解锁 |
| `drawerApi.setData(data)` | 设置数据 |
| `drawerApi.getData()` | 获取数据 |
| `drawerApi.setState()` | 动态设置状态 |
| `drawerApi.useStore()` | 获取响应式状态 |

### 抽屉事件

| 事件 | 说明 |
|------|------|
| `onBeforeClose` | 关闭前触发，返回 false/Promise reject 阻止关闭 |
| `onConfirm` | 确认按钮点击 |
| `onCancel` | 取消按钮点击 |
| `onOpenChange(isOpen)` | 打开/关闭状态变化 |
| `onOpened` | 打开动画播放完毕 |
| `onClosed` | 关闭动画播放完毕 |

### 抽屉 Props

| 配置 | 说明 |
|------|------|
| `placement` | 弹出位置：`'left'\|'right'\|'top'\|'bottom'` |
| `closeIconPlacement` | 关闭按钮位置：`'left'\|'right'` |
| `loading` | 加载状态 |
| `titleTooltip` | 标题提示信息 |
| `description` | 描述信息 |
| `modal` | 显示遮罩 |
| `header` | 显示 header |
| `footer` | 显示 footer |
| `closable` | 显示关闭按钮 |
| `confirmDisabled` | 禁用确认按钮 |

### 抽屉插槽

| 插槽名 | 说明 |
|--------|------|
| `default` | 抽屉内容 |
| `prepend-footer` | 取消按钮左侧 |
| `center-footer` | 取消和确认按钮之间 |
| `append-footer` | 确认按钮右侧 |
| `close-icon` | 关闭按钮图标 |
| `extra` | 标题右侧额外内容 |

## useVbenModal

弹窗组件，用于信息展示、确认等场景。

### 确认弹窗（confirm / deleteConfirm）

> **⚠️ 禁止使用 `antdv-next` 的 `Modal.confirm`！**

**删除确认**：使用全局封装的 `deleteConfirm`（自动处理确认弹窗 + 成功提示）：

```typescript
import { deleteConfirm } from '#/components/delete-confirm';

deleteConfirm(row.name, async () => {
  await deleteApi(row.id);
  gridApi.reload();
});
```

**其他二次确认**：使用 vben 的 `vbenConfirm`：

```typescript
import { vbenConfirm } from '@vben/common-ui';

vbenConfirm({
  content: '确定要执行此操作吗？',
  icon: 'question',
  title: '确认操作',
}).then(() => {
  // 确认后执行
}).catch(() => {});
```

`vbenConfirm` 也支持简写：

```typescript
vbenConfirm('确定要执行此操作吗？').then(() => { ... }).catch(() => {});
```

### 基础用法

```typescript
const [Modal, modalApi] = useVbenModal({
  title: '弹窗标题',
  content: '弹窗内容',
});
```

### 确认弹窗

```typescript
const [Modal, modalApi] = useVbenModal({
  title: '确认操作',
  content: '确定要执行此操作吗？',
  onConfirm: () => {
    // 确认逻辑
  },
});
```

### 配合表单使用

```typescript
const [Modal, modalApi] = useVbenModal({
  connectedComponent: Form,
  onConfirm: async () => {
    const { valid } = await formApi.validate();
    if (!valid) return;
    // 保存数据
    modalApi.close();
  },
});
```

### 常用配置

| 配置 | 说明 |
|------|------|
| `title` | 弹窗标题 |
| `connectedComponent` | 关联的组件 |
| `destroyOnClose` | 关闭时销毁内容 |
| `fullscreenButton` | 是否显示全屏按钮 |
| `onConfirm` | 确认回调 |
| `onCancel` | 取消回调 |
| `onOpenChange` | 打开/关闭变化回调 |

### 弹窗 API

| 方法 | 说明 |
|------|------|
| `modalApi.open()` | 打开弹窗 |
| `modalApi.close()` | 关闭弹窗 |
| `modalApi.lock()` | 锁定（禁用关闭） |
| `modalApi.unlock()` | 解锁 |
| `modalApi.setData(data)` | 设置数据 |
| `modalApi.getData()` | 获取数据 |
| `modalApi.setState()` | 动态设置状态 |
| `modalApi.useStore()` | 获取响应式状态 |

### 弹窗事件

| 事件 | 说明 |
|------|------|
| `onBeforeClose` | 关闭前触发，返回 false/Promise reject 阻止关闭 |
| `onConfirm` | 确认按钮点击 |
| `onCancel` | 取消按钮点击 |
| `onOpenChange(isOpen)` | 打开/关闭状态变化 |
| `onOpened` | 打开动画播放完毕 |
| `onClosed` | 关闭动画播放完毕 |

### 弹窗 Props

| 配置 | 说明 |
|------|------|
| `loading` | 加载状态 |
| `fullscreen` | 全屏显示 |
| `fullscreenButton` | 显示全屏按钮 |
| `draggable` | 可拖拽 |
| `centered` | 居中显示 |
| `modal` | 显示遮罩 |
| `header` | 显示 header |
| `footer` | 显示 footer |
| `closable` | 显示关闭按钮 |
| `confirmDisabled` | 禁用确认按钮 |
| `confirmLoading` | 确认按钮 loading |
| `closeOnClickModal` | 点击遮罩关闭 |
| `closeOnPressEscape` | ESC 关闭 |
| `titleTooltip` | 标题提示信息 |
| `description` | 描述信息 |
| `zIndex` | ZIndex 层级 |
| `overlayBlur` | 遮罩模糊度 |
| `bordered` | 显示 border |
| `contentClass` | 内容区域 class |
| `footerClass` | 底部区域 class |
| `headerClass` | 顶部区域 class |
| `submitting` | 标记提交中，锁定状态 |

## connectedComponent 模式

使用 `connectedComponent` 时，父子组件职责分离：

| 组件 | 职责 |
|------|------|
| **父组件** | 调用 `useVbenDrawer/Modal`，管理弹窗状态，通过 `drawerApi.setData()` 传递数据 |
| **子组件** | 只负责内容展示，通过 `onOpenChange` 获取数据，通过 `emit` 触发事件 |

### connectedComponent 注意事项

- 存在 2 个 `useVbenModal/Drawer` 时，**内部（未设置 connectedComponent）为准**
- 内外同时设置 `onConfirm` 时，以内部为准
- `onOpenChange` 内外都会触发
- 设置 `destroyOnClose` 时，关闭后会完全销毁内部组件

## 锁定状态（lock/unlock）

用于防止重复提交和意外关闭：

```typescript
const [Modal, modalApi] = useVbenModal({
  onConfirm: async () => {
    // 锁定弹窗状态
    modalApi.lock();
    
    try {
      const values = await formApi.getValues();
      await saveData(values);
      modalApi.close();
    } finally {
      // 无论成功失败都要解锁
      modalApi.unlock();
    }
  },
});
```

### 锁定时的行为

| 状态 | 行为 |
|------|------|
| 确认按钮 | 显示 loading |
| 取消/关闭按钮 | 禁用 |
| ESC 键 | 禁用 |
| 点击遮罩 | 禁用 |
| 内容区域 | 显示 spinner 遮罩 |

### 关闭前回调（onBeforeClose）

```typescript
const [Modal, modalApi] = useVbenModal({
  onBeforeClose: async () => {
    // 询问是否确认关闭
    const confirmed = await confirm('确定要关闭吗？');
    return confirmed;
  },
  // 支持 Promise
  onBeforeClose: () => {
    return new Promise((resolve) => {
      // 异步确认
      resolve(true);
    });
  },
});
```

## 动画类型（animationType）

控制弹窗的动画效果：

```typescript
const [Modal, modalApi] = useVbenModal({
  animationType: 'slide',  // 从上往下（默认）
  // animationType: 'scale', // 缩放淡入淡出
});
```

## 挂载位置（appendToMain）

默认弹窗挂载到 body，可设置为内容区域：

```typescript
const [Modal, modalApi] = useVbenModal({
  appendToMain: true,  // 挂载到内容区域，标签栏/菜单不会被遮挡
});
```

::: tip 配合 Page 组件使用

挂载到内容区域时，Page 组件需要设置 `auto-content-height` 属性：

```vue
<Page auto-content-height>
  <Modal appendToMain />
</Page>
```

:::

## 动态标题（新增/编辑切换）

根据弹窗是否携带数据，动态显示「新增」或「编辑」标题。推荐在 `onOpenChange` 中使用 `modalApi.setState` 设置标题，保持模板纯净。

### ✅ 推荐写法：modalApi.setState（子组件）

```typescript
// modules/form.vue
const [Modal, modalApi] = useVbenModal({
  onOpenChange(isOpen) {
    if (isOpen) {
      const data = modalApi.getData<SomeType>();
      modalApi.setState({
        title: data?.id ? $t('common.edit') : $t('common.create'),
      });
      if (data?.id) {
        formApi.setValues(data);
      }
    }
  },
});
```

```vue
<!-- modules/form.vue -->
<template>
  <Modal>
    <Form />
  </Modal>
</template>
```

### ✅ 父组件调用：connectedComponent 模式

```typescript
// list.vue
import Form from './modules/form.vue';

const [FormModal, formModalApi] = useVbenModal({
  connectedComponent: Form,
  destroyOnClose: true,
});

// 新增
function handleCreate() {
  formModalApi.setData(null).open();
}

// 编辑
function handleEdit(row: RecordType) {
  formModalApi.setData(row).open();
}
```

```vue
<!-- list.vue -->
<template>
  <FormModal @success="handleSuccess" />
</template>
```

### ❌ 不推荐：computed + 模板 `:title` 绑定

```typescript
// 需要额外维护 formData ref，增加状态管理复杂度
const formData = ref<SomeType>();
const getTitle = computed(() =>
  formData.value?.id
    ? $t('ui.actionTitle.edit', [$t('module.name')])
    : $t('ui.actionTitle.create', [$t('module.name')]),
);
```

```html
<!-- ❌ 在模板上设置属性，违反配置统一原则 -->
<Modal :title="getTitle">
  <Form />
</Modal>
```

### ❌ 不推荐：defineExpose + ref 调用

```typescript
// 子组件暴露 open/close 方法 —— 不必要的复杂度
defineExpose({ open, close });

// 父组件通过 ref 调用 —— 耦合子组件内部实现
const modalRef = ref<InstanceType<typeof SomeModal>>();
modalRef.value?.open(row);
```

**推荐 `modalApi.setState` 的原因**：
- 无需额外维护 `formData` ref 和 `computed`，减少状态管理复杂度
- 配置集中在 `useVbenModal` 中，模板保持 `<Modal><Form /></Modal>` 最简结构
- `connectedComponent` 模式通过 `setData` 传数据，无需 `defineExpose`，更解耦
- 判断 `data?.id` 而非 `!!data`，避免空对象导致误判为编辑模式

**参考示例**：`playground/src/views/system/dept/`

## 默认属性设置

在 `apps/<app>/src/bootstrap.ts` 中设置默认属性：

```typescript
// 设置默认 Modal 属性
setDefaultModalProps({
  fullscreenButton: false,  // 默认隐藏全屏按钮
  zIndex: 1000,
});

// 设置默认 Drawer 属性
setDefaultDrawerProps({
  zIndex: 1000,
});
```

## 插槽（Slots）

| 插槽名 | 说明 |
|--------|------|
| `default` | 弹窗内容 |
| `prepend-footer` | 取消按钮左侧 |
| `center-footer` | 取消和确认按钮之间 |
| `append-footer` | 确认按钮右侧 |

## 参数优先级

`slot` > `props` > `state`（useVbenModal 参数）

如果在模板中传入了 slot 或 props，`setState` 不会生效。

## Playground 示例

| 示例 | 路径 |
|------|------|
| 基础抽屉 | `examples/modal/drawer.vue` |
| 表单抽屉 | `examples/modal/drawer-form.vue` |
| 基础弹窗 | `examples/modal/modal.vue` |
| 确认弹窗 | `examples/modal/confirm.vue` |
| 弹窗表单 | `examples/modal/form-modal-demo.vue` |
