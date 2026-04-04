# Modal/Drawer 组件

弹窗和抽屉组件，用于创建模态对话框和侧边抽屉。

**官方文档**：
- Modal: https://doc.vben.pro/components/common-ui/vben-modal.html
- Drawer: https://doc.vben.pro/components/common-ui/vben-drawer.html

**Playground**：`playground/src/views/examples/modal/`

## ⚠️ 重要规则

### 组件拆分原则

> **组件超过 500 行必须拆分！**

| 场景 | 拆分方式 |
|------|----------|
| 表单 + 弹窗 | 使用 `useVbenForm` + `useVbenModal` |
| 复杂表单 | 按业务模块拆分为多个子表单 |
| 编辑详情 | 基本信息 + 物模型 等模块拆分 |

### 配置统一原则
✅ 最佳实践 ：所有配置参数统一在 useVbenModal / useVbenForm 的参数中设置，模板保持干净。

```typescript
<script setup lang="ts">
import { useVbenModal } from '@vben/common-ui';
import { useVbenForm } from '#/adapter/form';

const [Form, formApi] = useVbenForm({
  handleSubmit: onSubmit,
  layout: 'vertical',
  schema: [...],
  showDefaultActions: false,
});

const [Modal, modalApi] = useVbenModal({
  fullscreenButton: true,
  draggable: true,
  title: modalTitle.value,
  onConfirm() { onSubmit(); },
  onOpenChange(isOpen: boolean) { ... },
});
</script>

<template>
  <Modal>
    <Form />
  </Modal>
</template>
```
❌ 错误写法：配置分散在脚本和模板中，模板中的 props 会覆盖脚本中的配置。

```typescript
<script setup lang="ts">
const [Modal, modalApi] = useVbenModal({
  fullscreenButton: true, // 这里设置了 true
  ...
});
</script>

<template>
  <Modal
    :fullscreen-button="false" <!-- 这里又设置为 false，会覆盖上面的配置！ -->
  >
    ...
  </Modal>
</template>
```
关键点 ：模板中的 props 会覆盖 useVbenModal / useVbenForm 内部的默认值。如果想在脚本中统一管理配置，模板中就不要重复设置相同的 prop。

## ❌ 常见错误

### 错误1：在子组件中使用 useVbenDrawer/useVbenModal

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

### 错误2：直接在 template 中导入原生组件

**❌ 错误写法**：直接导入原生组件

```vue
<!-- ❌ 错误 -->
<script setup>
import { Button, Input } from 'antdv-next';
</script>
<template>
  <Button>提交</Button>
  <Input v-model="value" />
</template>
```

**✅ 正确做法**：
- 对于 Vben 封装的表单/表格等，使用 `useVbenForm`、`useVbenVxeGrid` 的 schema 定义
- 对于按钮等简单组件，可以直接导入 `import { Button } from 'antdv-next'`，但需确保组件已在适配器中注册

### 错误3：父子组件状态管理混乱

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

## useVbenModal

弹窗组件，用于信息展示、确认等场景。

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

## connectedComponent 模式

使用 `connectedComponent` 时，父子组件职责分离：

| 组件 | 职责 |
|------|------|
| **父组件** | 调用 `useVbenDrawer/Modal`，管理弹窗状态，通过 `drawerApi.setData()` 传递数据 |
| **子组件** | 只负责内容展示，通过 `onOpenChange` 获取数据，通过 `emit` 触发事件 |

## Playground 示例

| 示例 | 路径 |
|------|------|
| 基础抽屉 | `examples/modal/drawer.vue` |
| 表单抽屉 | `examples/modal/drawer-form.vue` |
| 基础弹窗 | `examples/modal/modal.vue` |
| 确认弹窗 | `examples/modal/confirm.vue` |
| 弹窗表单 | `examples/modal/form-modal-demo.vue` |
