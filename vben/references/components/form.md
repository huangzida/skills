# 表单组件（useVbenForm）

vben-admin 提供的表单组件，基于 vee-validate，支持多种 UI 框架。

**官方文档**：https://doc.vben.pro/components/common-ui/vben-form.html

**Playground**：`playground/src/views/examples/form/`

## 基础用法

### ❌ 错误做法：直接在 template 中写 Input 组件

```vue
<!-- ❌ 错误：在 template 中使用原生组件 -->
<template>
  <Input v-model="value" />
</template>

<script setup>
import { Input } from 'antdv-next';  // 这会导致运行时错误
</script>
```

### ✅ 正确做法：使用 schema 注册的组件

```vue
<!-- ✅ 正确：在 schema 中定义组件 -->
<script setup lang="ts">
import { useVbenForm } from '#/adapter/form';

const [Form, formApi] = useVbenForm({
  layout: 'horizontal',
  handleSubmit: (values) => {
    console.log('提交的值:', values);
  },
  schema: [
    {
      fieldName: 'username',
      label: '用户名',
      component: 'Input',
      rules: 'required',
    },
  ],
});
</script>

<template>
  <Form />
</template>
```

## useVbenForm + useVbenModal 组合

### ✅ 正确示例：弹窗表单

```vue
<script setup lang="ts">
import { useVbenModal } from '@vben/common-ui';
import { useVbenForm } from '#/adapter/form';

const [Form, formApi] = useVbenForm({
  handleSubmit: onSubmit,
  schema: [
    { component: 'Input', fieldName: 'name', label: '名称', rules: 'required' },
    { component: 'Select', fieldName: 'category', label: '分类' },
  ],
  showDefaultActions: false,
});

const [Modal, modalApi] = useVbenModal({
  fullscreenButton: false,
  onCancel() {
    modalApi.close();
  },
  onConfirm: async () => {
    await formApi.validateAndSubmitForm();
  },
  onOpenChange(isOpen) {
    if (isOpen) {
      const data = modalApi.getData();
      if (data) {
        formApi.setValues(data);
      }
    }
  },
  title: '弹窗标题',
});

function onSubmit(values) {
  // 处理提交
  modalApi.close();
}

function open(data) {
  modalApi.setData(data);
  modalApi.open();
}

defineExpose({ open });
</script>

<template>
  <Modal class="sm:w-[600px]">
    <Form />
  </Modal>
</template>
```

## ❌ 常见错误

### 错误1：在 template 中使用未注册的组件（表单字段）

```vue
<!-- ❌ 错误：在 template 中直接使用表单组件 -->
<template>
  <Input v-model="value" />
</template>

<script setup>
import { Input } from 'antdv-next';
</script>
```

**正确做法**：在 schema 中定义组件。

### 错误2：使用原生 HTML 按钮或 antdv-next 的 Button

```vue
<!-- ❌ 错误：使用原生 button 元素 -->
<button type="button" class="ant-btn" @click="handleReset">重置</button>

<!-- ❌ 错误：使用 antdv-next 的 Button -->
import { Button } from 'antdv-next';
<Button @click="handleSubmit">提交</Button>
```

**✅ 正确做法：使用 @vben/common-ui 的 VbenButton 组件**

```vue
<!-- ✅ 正确：使用 @vben/common-ui 的 VbenButton 组件 -->
<script setup lang="ts">
import { VbenButton } from '@vben/common-ui';
</script>

<template>
  <VbenButton @click="handleReset">重置</VbenButton>
  <VbenButton variant="primary" @click="handleSubmit">提交</VbenButton>
</template>
```

### 错误3：组件命名不规范

```vue
<!-- ❌ 错误：通用名称 -->
<script setup>
import Form from './Form.vue';  // Form 容易与其他组件冲突
</script>

<!-- ✅ 正确：语义化命名 -->
<script setup>
import AddProtocolModal from './AddProtocolModal.vue';
import UpdateProtocolModal from './UpdateProtocolModal.vue';
</script>
```

### 错误4：组件职责不清

```vue
<!-- ❌ 错误：一个大组件包含所有功能 -->
<!-- ProductEditModal.vue (1000+ 行) -->
<!-- - 基本信息编辑 -->
<!-- - 物模型配置 -->
<!-- - 文件上传 -->
<!-- - 审核流程 -->

<!-- ✅ 正确：按职责拆分 -->
<!-- ProductEditModal.vue - 弹窗容器 -->
<!-- ProductBasicInfo.vue - 基本信息 -->
<!-- ProductThingModel.vue - 物模型 -->
<!-- ProductAudit.vue - 审核流程 -->
```

## 布局配置

| 布局 | 说明 |
|------|------|
| `horizontal` | 水平布局（默认） |
| `vertical` | 垂直布局 |
| `inline` | 行内布局 |

## 字段类型

### 常用字段

| 组件 | 说明 |
|------|------|
| `Input` | 文本输入 |
| `InputPassword` | 密码输入 |
| `InputNumber` | 数字输入 |
| `Textarea` | 文本域 |
| `Select` | 下拉选择 |
| `ApiSelect` | 远程数据选择 |
| `TreeSelect` | 树形选择 |
| `DatePicker` | 日期选择 |
| `RangePicker` | 日期范围 |
| `TimePicker` | 时间选择 |
| `RadioGroup` | 单选组 |
| `CheckboxGroup` | 多选组 |
| `Switch` | 开关 |
| `Rate` | 评分 |
| `Cascader` | 级联选择 |
| `Upload` | 文件上传 |
| `IconPicker` | 图标选择 |

### 字段定义示例

```typescript
{
  fieldName: 'username',
  label: '用户名',
  component: 'Input',
  componentProps: {
    placeholder: '请输入用户名',
  },
  rules: 'required',
}
```

## 表单校验

### 预定义规则

- `required` - 输入项必填
- `selectRequired` - 选择项必填

### Zod 校验

```typescript
import { z } from '#/adapter/form';

{
  fieldName: 'username',
  rules: z.string()
    .min(2, '用户名至少2个字符')
    .max(20, '用户名最多20个字符'),
}
```

## 字段插槽

### suffix / prefix 插槽

在 schema 字段中可以使用 `suffix` 和 `prefix` 属性添加自定义插槽内容，适用于添加按钮、图标等附加元素。

```typescript
import { h } from 'vue';
import { VbenButton } from '@vben/common-ui';

{
  fieldName: 'manualTime',
  label: '手动设置时间',
  component: 'DatePicker',
  suffix: () => h(VbenButton, { size: 'sm', onClick: handleApplyTime }, () => '应用时间'),
  componentProps: {
    showTime: true,
    format: 'YYYY-MM-DD HH:mm',
    placeholder: '请选择日期时间',
  },
}
```

### 自定义组件插槽

对于 Input 等支持插槽的组件，可以通过 `suffix` 和 `prefix` 属性添加前后缀内容：

```typescript
{
  component: 'Input',
  fieldName: 'price',
  label: '价格',
  suffix: () => h('span', { class: 'text-gray-500' }, '元'),
}
```

## DatePicker 最佳实践

### 日期时间选择（精确到分钟）

```typescript
{
  fieldName: 'scheduledTime',
  label: '计划时间',
  component: 'DatePicker',
  help: '留空则使用NTP同步',
  componentProps: {
    showTime: true,
    format: 'YYYY-MM-DD HH:mm',
    placeholder: '请选择日期时间',
    minuteStep: 1,  // 精确到每分钟
  },
}
```

### 带操作按钮的日期选择器

```typescript
import { h } from 'vue';
import { VbenButton } from '@vben/common-ui';

{
  fieldName: 'manualTime',
  label: '手动设置时间',
  component: 'DatePicker',
  suffix: () => h(VbenButton, { size: 'sm', onClick: handleApplyTime }, () => '应用时间'),
  componentProps: {
    showTime: true,
    format: 'YYYY-MM-DD HH:mm',
    placeholder: '请选择日期时间',
  },
}
```

### RangePicker 日期范围选择

```typescript
{
  fieldName: 'dateRange',
  label: '日期范围',
  component: 'RangePicker',
  componentProps: {
    format: 'YYYY-MM-DD',
    placeholder: ['开始日期', '结束日期'],
  },
}
```

## 表单联动

使用 `dependencies` 实现字段联动：

```typescript
{
  fieldName: 'type',
  component: 'Select',
  // ...
},
{
  fieldName: 'conditionalField',
  component: 'Input',
  dependencies: {
    triggerFields: ['type'],
    show: (values) => values.type === 'specific',
    required: (values) => values.type === 'specific',
  },
}
```

### dependencies 完整配置

```typescript
dependencies: {
  triggerFields: ['name'],           // 触发字段
  if(values, formApi) {},             // 动态显示/销毁
  show(values, formApi) {},            // CSS 隐藏
  disabled(values, formApi) {},       // 禁用状态
  trigger(values, formApi) {},        // 每次变更触发
  rules(values, formApi) {},          // 动态规则
  required(values, formApi) {},        // 动态必填
  componentProps(values, formApi) {}, // 动态组件参数
}
```

## 值映射（fieldMappingTime）

官方推荐的日期范围映射方式，比 `valueFormat` 更简洁：

```typescript
const [Form, formApi] = useVbenForm({
  // 将 timeRange 数组映射为 startTime / endTime
  fieldMappingTime: [
    ['timeRange', ['startTime', 'endTime'], 'YYYY-MM-DD'],
  ],
  schema: [
    {
      component: 'RangePicker',
      fieldName: 'timeRange',
      label: '时间范围',
    },
  ],
});
```

### 映射规则说明

| 参数 | 说明 |
|------|------|
| 第一个参数 | 源字段名（数组类型，如 RangePicker） |
| 第二个参数 | 目标字段名数组 `[开始时间, 结束时间]` |
| 第三个参数 | 可选，日期格式掩码（如 `'YYYY-MM-DD'`） |

### 自定义格式化函数

```typescript
fieldMappingTime: [
  [
    'reportRange',
    ['startTime', 'endTime'],
    (value, fieldName) => {
      // 自定义格式化逻辑
      return value?.valueOf();
    },
  ],
],
```

## 表单值变化（handleValuesChange）

监听表单值变化，用于联动逻辑或实时保存：

```typescript
const [Form, formApi] = useVbenForm({
  handleValuesChange(values, fieldsChanged) {
    console.log('变化的值:', values);
    console.log('变化的字段:', fieldsChanged);
    
    // 根据字段变化执行联动
    if (fieldsChanged.includes('category')) {
      // 分类变化时，重置子分类
      formApi.setFieldValue('subCategory', undefined);
    }
  },
  // ...
});
```

## 值格式化（valueFormat）

当组件的展示值与后端真正需要的 payload 不一致时，可以使用 `valueFormat` 进行转换。它会在 `getValues()`、提交、以及依赖这些输出的方法中生效。

### 三种返回值模式

| 返回值 | 行为 | 适用场景 |
|--------|------|----------|
| `return xxx` | 回写当前字段 | 格式转换（如 Date → 时间戳） |
| `setValue('field', xxx)` | 写入其他字段 | 字段拆分（如 RangePicker → startTime/endTime） |
| `return undefined` | 删除当前字段 | 字段移除（如隐藏字段） |

### 字段拆分示例（RangePicker → startTime/endTime）

```typescript
{
  component: 'RangePicker',
  fieldName: 'reportRange',
  help: '通过 setValue 拆分为 startTime / endTime，并移除原字段',
  label: '统计时间范围',
  valueFormat(value, setValue) {
    setValue('startTime', value?.[0]?.valueOf());
    setValue('endTime', value?.[1]?.valueOf());
  },
},
```

### 格式转换示例（Date → timestamp）

```typescript
{
  component: 'DatePicker',
  fieldName: 'deadline',
  help: '直接 return 时间戳，保留原字段名',
  label: '截止时间',
  valueFormat(value) {
    return value?.valueOf();
  },
},
```

### 完整示例

```vue
<script lang="ts" setup>
import { computed, nextTick, onMounted, ref, watch } from 'vue';
import { Button, Card, message, Space, Tag } from 'ant-design-vue';
import { useVbenForm } from '#/adapter/form';

const transformedValues = ref<Record<string, any>>({});
const liveValues = ref<Record<string, any>>({});

const [Form, formApi] = useVbenForm({
  commonConfig: {
    componentProps: {
      class: 'w-full',
    },
  },
  handleSubmit,
  schema: [
    {
      component: 'RangePicker',
      fieldName: 'reportRange',
      help: '通过 setValue 拆分为 startTime / endTime，并移除原字段',
      label: '统计时间范围',
      valueFormat(value, setValue) {
        setValue('startTime', value?.[0]?.valueOf());
        setValue('endTime', value?.[1]?.valueOf());
      },
    },
    {
      component: 'DatePicker',
      fieldName: 'deadline',
      help: '直接 return 时间戳，保留原字段名',
      label: '截止时间',
      valueFormat(value) {
        return value?.valueOf();
      },
    },
    {
      component: 'Input',
      componentProps: {
        placeholder: '请输入关键字',
      },
      fieldName: 'keyword',
      label: '关键字',
    },
  ],
  wrapperClass: 'grid-cols-1 md:grid-cols-2',
});

async function handleInspectValues() {
  const values = await formApi.getValues();
  console.log('valueFormat 后的值:', values);
}

function handleSubmit(values: Record<string, any>) {
  message.success({ content: `提交值: ${JSON.stringify(values)}` });
}
</script>

<template>
  <Space wrap>
    <Tag color="processing">return 值：回写当前字段</Tag>
    <Tag color="success">setValue：拆分写入其他字段</Tag>
    <Tag color="warning">return undefined：保持原字段删除</Tag>
  </Space>
  <Form />
</template>
```

## 通用配置（commonConfig）

为所有字段设置统一的默认配置：

```typescript
const [Form, formApi] = useVbenForm({
  commonConfig: {
    componentProps: {
      class: 'w-full',  // 所有组件宽度 100%
    },
    labelWidth: 120,     // 统一标签宽度
    disabled: false,     // 统一禁用状态
  },
});
```

### commonConfig 属性

| 属性 | 说明 |
|------|------|
| `componentProps` | 所有字段的组件默认参数 |
| `labelWidth` | 统一标签宽度 |
| `disabled` | 统一禁用状态 |
| `hideLabel` | 隐藏所有标签 |
| `hideRequiredMark` | 隐藏必填标记 |
| `colon` | 标签后显示冒号 |
| `controlClass` | 控件样式类 |
| `wrapperClass` | 包裹层样式类 |

## 查询表单（submitOnChange）

用于表格搜索表单，字段变化时自动触发查询：

```typescript
const [Form, formApi] = useVbenForm({
  submitOnChange: true,  // 字段变化时自动查询
  handleSubmit: (values) => {
    gridApi.query(values);
  },
});
```

## 操作按钮配置

### 自定义按钮文本和样式

```typescript
const [Form, formApi] = useVbenForm({
  submitButtonOptions: {
    content: '保存',
    variant: 'primary',
  },
  resetButtonOptions: {
    content: '重置',
    variant: 'default',
  },
});
```

### 隐藏操作按钮

```typescript
const [Form, formApi] = useVbenForm({
  showDefaultActions: false,  // 隐藏默认按钮
});
```

## Form API

| 方法 | 说明 |
|------|------|
| `formApi.setFieldValue(name, value)` | 设置单个字段值 |
| `formApi.setValues(values, filterFields?, shouldValidate?)` | 设置多个字段值，可选过滤字段 |
| `formApi.getValues()` | 获取表单值 |
| `formApi.validate()` | 校验表单 |
| `formApi.resetForm()` | 重置表单 |
| `formApi.validateAndSubmitForm()` | 校验并提交 |
| `formApi.updateSchema(schema)` | 更新字段配置 |
| `formApi.validateField(name)` | 校验单个字段 |
| `formApi.isFieldValid(name)` | 检查字段是否通过校验 |
| `formApi.getFieldComponentRef(name)` | 获取字段组件实例 |
| `formApi.getFocusedField()` | 获取当前焦点的字段 |
| `formApi.setState(state)` | 设置组件状态 |
| `formApi.getState()` | 获取组件状态 |

### setValues 过滤字段

```typescript
// 默认过滤不在 schema 中的字段
await formApi.setValues({ extraField: 'xxx' });  // extraField 会被过滤掉

// 关闭过滤，保留额外字段
await formApi.setValues({ extraField: 'xxx' }, false);
```

## 折叠配置

支持表单项折叠/展开功能：

```typescript
const [Form, formApi] = useVbenForm({
  showCollapseButton: true,  // 显示折叠按钮
  collapsed: false,          // 默认展开
  collapsedRows: 1,          // 折叠时显示 1 行
  handleCollapsedChange: (collapsed) => {
    console.log('折叠状态:', collapsed);
  },
  schema: [
    { fieldName: 'field1', component: 'Input', label: '字段1' },
    { fieldName: 'field2', component: 'Input', label: '字段2' },
    // ... 更多字段
  ],
});
```

### 折叠相关配置

| 配置 | 说明 |
|------|------|
| `showCollapseButton` | 显示折叠按钮 |
| `collapsed` | 默认折叠状态 |
| `collapsedRows` | 折叠时保留的行数 |
| `collapseTriggerResize` | 折叠时触发 resize 事件 |
| `handleCollapsedChange` | 折叠状态变化回调 |

## 操作区域布局

```typescript
const [Form, formApi] = useVbenForm({
  actionLayout: 'rowEnd',    // 'newLine'|'rowEnd'|'inline'
  actionPosition: 'right',   // 'left'|'center'|'right'
  actionButtonsReverse: false, // 调换按钮位置
});
```

### 布局配置

| 配置 | 说明 |
|------|------|
| `actionLayout` | 按钮布局：newLine(新行)、rowEnd(行尾)、inline(行内) |
| `actionPosition` | 按钮对齐：left、center、right |
| `actionButtonsReverse` | 调换重置/提交按钮位置 |

## 字段高级属性

```typescript
{
  fieldName: 'description',
  label: '描述',
  component: 'Textarea',
  description: '这是一段帮助说明文字',  // 字段描述
  hide: false,  // 是否隐藏字段
  defaultValue: '默认值',  // 默认值
  renderComponentContent: {},  // 自定义组件内容
}
```

### 字段属性说明

| 属性 | 说明 |
|------|------|
| `description` | 字段描述信息 |
| `hide` | 是否隐藏表单项（CSS 隐藏） |
| `defaultValue` | 字段默认值 |
| `renderComponentContent` | 自定义组件内部渲染内容 |

## 其他 Props 配置

| 属性 | 说明 |
|------|------|
| `submitOnEnter` | 按下回车键时提交表单 |
| `scrollToFirstError` | 验证失败时滚动到第一个错误字段 |
| `compact` | 紧凑模式（忽略校验信息预留空间） |
| `handleReset` | 表单重置回调 |
| `handleSubmit` | 表单提交回调 |

## Playground 示例

| 示例 | 路径 |
|------|------|
| 基础表单 | `examples/form/basic.vue` |
| 查询表单 | `examples/form/query.vue` |
| 表单联动 | `examples/form/linkage.vue` |
| 表单校验 | `examples/form/rules.vue` |
| 弹窗表单 | `examples/modal/form-modal-demo.vue` |
