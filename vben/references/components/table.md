# 表格组件（useVbenVxeGrid）

vben-admin 提供的表格组件，基于 vxe-table，集成了搜索表单。

**官方文档**：https://doc.vben.pro/components/common-ui/vben-vxe-table.html

**Playground**：`playground/src/views/examples/vxe-table/`

## 基础用法

> **⚠️ 最小配置原则**：`useVbenVxeGrid` 只需配置 `formOptions.schema`、`gridOptions.columns` 和 `gridOptions.proxyConfig.ajax.query`，其他配置已在全局 `adapter/vxe-table.ts` 中预设。详见 [rules.md](../rules.md)。

```vue
<script setup lang="ts">
import { useVbenVxeGrid } from '#/adapter/vxe-table';

const [Grid] = useVbenVxeGrid({
  gridOptions: {
    columns: [
      { type: 'seq', title: '序号', width: 50 },
      { field: 'name', title: '姓名', sortable: true },
    ],
    proxyConfig: {
      ajax: {
        query: async ({ page }, formValues) => ({ items: [], total: 0 }),
      },
    },
  },
});
</script>
```
```vue
<template>
  <Grid>
    <template #toolbar-actions />
  </Grid>
</template>
```

> **⚠️ 重要**：必须添加 `<template #toolbar-actions />` 或 `<template #toolbar-tools />` 才会渲染工具栏，即使为空也需要！

## 核心配置

### 列配置

> **⚠️ 最佳实践**：普通数据列不要设置 `width`，表格会自适应列宽。仅 `type: 'seq'`、`type: 'checkbox'`、操作列等固定宽度列才需要设置 `width`。

```typescript
columns: [
  { type: 'seq', title: '序号', width: 50 },
  { type: 'checkbox', title: '勾选', width: 50 },
  { field: 'name', title: '姓名', sortable: true },
  { field: 'status', title: '状态', cellRender: { name: 'CellTag' } },
]
```

### 代理配置（远程数据）

```typescript
proxyConfig: {
  autoLoad: true,                    // 初始化时自动加载
  showActiveMsg: true,               // 显示激活消息
  showResponseMsg: false,            // 显示响应消息
  response: {
    result: 'items',                 // 数据数组字段
    total: 'total',                 // 总数字段
    list: 'items',                   // 列表字段
  },
  ajax: {
    query: async ({ page, sort, form }) => {
      return await getListApi({
        page: page.currentPage,
        pageSize: page.pageSize,
        ...sort,
        ...form,
      });
    },
  },
}
```

### 代理配置说明

| 配置 | 说明 |
|------|------|
| `autoLoad` | 初始化时自动加载数据 |
| `showActiveMsg` | 显示加载激活消息 |
| `showResponseMsg` | 显示响应消息 |
| `response.result` | 数据数组字段路径 |
| `response.total` | 总数字段路径 |
| `response.list` | 列表字段路径 |

### 工具栏

```typescript
toolbarConfig: {
  search: true,    // 搜索
  export: false,    // 导出
  refresh: true,   // 刷新
  zoom: true,      // 全屏
  custom: true,    // 自定义列
}
```

> **⚠️ 重要**：当 `export: true` 时，必须同时设置 `exportConfig: {}`，否则 vxe-table 控制台会报 `"缺少必要的 export-config 参数"` 警告。

### 工具栏显示条件

使用工具栏时必须满足以下条件：

1. **设置 toolbarConfig**：在 gridOptions 中配置工具栏选项
2. **使用工具栏插槽**：必须在模板中使用 `#toolbar-actions` 或 `#toolbar-tools` 插槽

```vue
<template>
  <Grid>
    <template #toolbar-actions>
      <Button type="primary" @click="handleCreate">新增</Button>
    </template>
  </Grid>
</template>
```

如果不需要自定义内容，可以使用空插槽：

```vue
<Grid><template #toolbar-actions /></Grid>
```

### 工具栏插槽选择

| 插槽 | 用途 | 位置 |
|------|------|------|
| `#toolbar-actions` | 操作按钮（新增、导出等） | 工具栏左侧 |
| `#toolbar-tools` | 工具按钮（刷新、密度等） | 工具栏右侧 |

```vue
<Grid>
  <template #toolbar-actions>
    <Button type="primary" @click="handleCreate">新增</Button>
  </template>
</Grid>
```

> **⚠️ 最佳实践**：操作按钮（新增、导出等）固定使用 `#toolbar-actions`，不要使用 `#toolbar-tools`。`#toolbar-tools` 用于右侧工具区域。

> **⚠️ 最佳实践**：Grid 不要设置 `table-title` prop，表格标题通过路由 `meta.title` 自动显示。

### 分页

```typescript
pagerConfig: {
  enabled: true,
  currentPage: 1,
  pageSize: 20,
  pageSizes: [10, 20, 50, 100],
}
```

## 搜索表单

表格内置搜索表单：

```typescript
const [Grid] = useVbenVxeGrid({
  formOptions: {
    fieldMappingTime: [['createTime', ['startTime', 'endTime']]],
    schema: [
      { fieldName: 'name', label: '姓名', component: 'Input' },
      { fieldName: 'status', label: '状态', component: 'Select', options: [...] },
    ],
    submitOnEnter: true,
  },
  gridOptions: { ... },
});
```

## 行编辑

### 单元格编辑

```typescript
editConfig: {
  trigger: 'click',  // 'click' | 'dblclick' | 'manual'
  mode: 'cell',      // 'cell' | 'row'
}
```

### 保存编辑

```typescript
gridEvents: {
  editClosed: async ({ row }) => {
    await saveRowApi(row);
  },
}
```

## 展开行（子表格）

vxe-table 支持行展开显示子表格。

### 列定义

展开列需要使用 `type: 'expand'`，并定义 `slots.content`：

```typescript
columns: [
  {
    type: 'expand',
    width: 60,
    slots: { content: 'expand-content' },  // 只定义 content，不要定义 default
  },
  { field: 'name', title: '名称' },
  // ... 其他列
]
```

### 模板插槽

```vue
<template>
  <Grid>
    <!-- 其他插槽 -->
    <template #expand-content="{ row }">
      <div class="expand-wrapper">
        <!-- 子表格内容，如嵌套表格 -->
        <NestedTable :data="row.children" />
      </div>
    </template>
    <!-- 工具栏插槽必须添加 -->
    <template #toolbar-tools />
  </Grid>
</template>

<style scoped>
.expand-wrapper {
  padding: 16px;
  background: hsl(var(--muted));
}
</style>
```

### 完整示例

```vue
<script setup lang="ts">
import { useVbenVxeGrid } from '#/adapter/vxe-table';
import NestedTable from './NestedTable.vue';

interface ParentRow {
  id: number;
  name: string;
  children?: ChildRow[];
}

interface ChildRow {
  id: number;
  parentId: number;
  name: string;
}

const [Grid] = useVbenVxeGrid({
  gridOptions: {
    columns: [
      {
        type: 'expand',
        width: 60,
        slots: { content: 'expand-content' },
      },
      { field: 'name', title: '名称' },
    ],
    rowConfig: { keyField: 'id' },
  },
});
</script>

<template>
  <Grid>
    <template #expand-content="{ row }">
      <div class="expand-wrapper">
        <NestedTable
          v-if="row.children?.length"
          :data="row.children"
        />
        <div v-else class="py-4 text-center text-muted-foreground">
          暂无子数据
        </div>
      </div>
    </template>
    <template #toolbar-tools />
  </Grid>
</template>
```

### 注意事项

1. **不要定义 default 插槽**：expand 列只需要定义 `content` 插槽，不需要 `default` 插槽
2. **样式隔离**：使用 `.expand-wrapper` 类来设置展开内容的样式
3. **条件渲染**：确保子数据存在时再渲染子组件

## Grid API

| 方法 | 说明 |
|------|------|
| `gridApi.reload()` | 重新加载，回到第一页 |
| `gridApi.query()` | 刷新当前页 |
| `gridApi.setLoading(true)` | 设置加载状态 |
| `gridApi.toggleSearchForm()` | 切换搜索表单 |
| `gridApi.setGridOptions()` | 更新表格配置 |
| `gridApi.grid` | vxe-grid 实例 |
| `gridApi.formApi` | 搜索表单 API |

## 表格基础配置

```typescript
const [Grid] = useVbenVxeGrid({
  gridOptions: {
    showOverflow: true,               // 单元格内容溢出显示...
    border: true,                     // 显示边框
    round: true,                      // 圆角
    size: 'small',                   // 表格尺寸
    align: 'center',                 // 默认对齐方式
    rowConfig: {
      keyField: 'id',                // 行唯一标识字段
    },
    columnConfig: {
      resizable: true,               // 列可调整宽度
    },
  },
});
```

### 基础配置说明

| 配置 | 说明 |
|------|------|
| `height` | 表格高度，默认 `'auto'` |
| `showOverflow` | 溢出显示... |
| `border` | 显示边框 |
| `round` | 圆角 |
| `size` | 尺寸：`small`/`medium`/`large` |
| `align` | 对齐方式 |
| `rowConfig.keyField` | 行唯一标识字段 |
| `columnConfig.resizable` | 列宽可调整 |

## 表格事件（gridEvents）

```typescript
const [Grid, gridApi] = useVbenVxeGrid({
  gridEvents: {
    // 复选框变化
    checkboxChange: ({ records }) => {
      console.log('选中行:', records);
    },
    // 单选框变化
    radioChange: ({ row }) => {
      console.log('选中行:', row);
    },
    // 排序变化
    sortChange: ({ field, order }) => {
      console.log('排序:', field, order);
    },
    // 编辑关闭
    editClosed: async ({ row }) => {
      await saveRowApi(row);
    },
    // 滚动
    scroll: ({ scrollTop, scrollLeft }) => {
      console.log('滚动位置:', scrollTop, scrollLeft);
    },
    // 行点击
    cellClick: ({ row, column }) => {
      console.log('点击单元格:', row, column);
    },
  },
});
```

### 常用事件

| 事件 | 说明 |
|------|------|
| `checkboxChange` | 复选框变化 |
| `radioChange` | 单选框变化 |
| `sortChange` | 排序变化 |
| `filterChange` | 筛选变化 |
| `editClosed` | 编辑关闭 |
| `editActived` | 编辑激活 |
| `scroll` | 滚动 |
| `cellClick` | 单元格点击 |
| `cellDblClick` | 单元格双击 |
| `toolbarButtonClick` | 工具栏按钮点击 |

## 树形表格（treeConfig）

扁平数据转换为树形展示：

```typescript
const [Grid] = useVbenVxeGrid({
  gridOptions: {
    treeConfig: {
      transform: true,        // 开启转换
      parentField: 'parentId',  // 父字段名
      rowField: 'id',           // 主键字段名
    },
    columns: [
      { type: 'seq', title: '序号', width: 60 },
      { field: 'name', title: '名称', treeNode: true },  // 添加 treeNode 显示树线
    ],
  },
});
```

### 树形表格配置

| 属性 | 说明 |
|------|------|
| `transform` | 是否将扁平数据转换为树形 |
| `parentField` | 父级字段名 |
| `rowField` | 主键字段名 |
| `children` | 子数据字段名（默认 children） |
| `indent` | 缩进距离 |
| `expandAll` | 默认展开全部 |

## 单元格编辑（editConfig）

### 单元格编辑模式

```typescript
const [Grid, gridApi] = useVbenVxeGrid({
  gridOptions: {
    editConfig: {
      trigger: 'click',  // 点击编辑
      mode: 'cell',      // 单元格编辑
      showStatus: true,  // 显示编辑状态图标
    },
    editRules: {
      name: [{ required: true, message: '名称不能为空' }],
    },
  },
  gridEvents: {
    editClosed: async ({ row }) => {
      // 保存编辑后的行数据
      await updateRowApi(row);
    },
  },
});
```

### 行编辑模式

```typescript
editConfig: {
  trigger: 'click',
  mode: 'row',  // 整行编辑
}
```

### 编辑触发方式

| trigger | 说明 |
|---------|------|
| `click` | 点击单元格 |
| `dblclick` | 双击单元格 |
| `manual` | 手动模式 |

## 固定列（fixed）

列固定在左侧或右侧：

```typescript
columns: [
  { type: 'seq', width: 60, fixed: 'left' },
  { field: 'name', title: '姓名' },
  { field: 'actions', title: '操作', fixed: 'right' },
]
```

### 固定位置

| 值 | 说明 |
|----|------|
| `left` | 固定在左侧 |
| `right` | 固定在右侧 |
| `''` 或 `null` | 不固定 |

## 虚拟滚动（virtual）

大数据量时启用虚拟滚动：

```typescript
const [Grid] = useVbenVxeGrid({
  gridOptions: {
    scroll-y: {
      enabled: true,
      gt: 100,  // 超过 100 行启用虚拟滚动
    },
  },
});
```

## 自定义单元格渲染器

### 注册渲染器

在适配器中注册：

```typescript
import { h } from 'vue';
import { Button, Image } from 'ant-design-vue';

setupVbenVxeTable({
  configVxeTable: (vxeUI) => {
    // 图片单元格
    vxeUI.renderer.add('CellImage', {
      renderTableDefault(_renderOpts, params) {
        const { column, row } = params;
        return h(Image, { src: row[column.field] });
      },
    });

    // 链接单元格
    vxeUI.renderer.add('CellLink', {
      renderTableDefault(renderOpts) {
        const { props } = renderOpts;
        return h(
          Button,
          { size: 'small', type: 'link' },
          { default: () => props?.text },
        );
      },
    });
  },
});
```

### 使用渲染器

```typescript
columns: [
  { field: 'avatar', title: '头像', cellRender: { name: 'CellImage' } },
  { field: 'name', title: '名称', cellRender: { name: 'CellLink', props: { text: '点击查看' } } },
]
```

## 搜索表单（formOptions）

### 表单配置

```typescript
const [Grid] = useVbenVxeGrid({
  formOptions: {
    show: true,  // 默认显示搜索表单
    fieldMappingTime: [
      ['createTime', ['startTime', 'endTime'], 'YYYY-MM-DD'],
    ],
    commonConfig: {
      componentProps: {
        class: 'w-full',
      },
    },
    schema: [
      {
        fieldName: 'name',
        label: '名称',
        component: 'Input',
      },
      {
        fieldName: 'status',
        label: '状态',
        component: 'Select',
        componentProps: {
          options: [
            { label: '启用', value: 1 },
            { label: '禁用', value: 0 },
          ],
        },
      },
    ],
    submitOnEnter: true,
  },
});
```

### 分隔条配置（separator）

搜索表单和表格之间的分隔条：

```typescript
const [Grid] = useVbenVxeGrid({
  separator: false,  // 隐藏分隔条
  // 或自定义样式
  // separator: { show: false },
  // separator: { backgroundColor: 'rgba(100,100,0,0.5)' },
});
```

### 搜索表单插槽

所有 `form-` 开头的插槽会转发到搜索表单：

```vue
<template>
  <Grid>
    <template #form-before>搜索前内容</template>
    <template #form-after>搜索后内容</template>
  </Grid>
</template>
```

## 常见问题

### Q: 工具栏不显示？

是否在模板中使用了 `#toolbar-tools` 或 `#toolbar-actions` 插槽

### Q: 展开行不显示？

检查列定义是否正确：
```typescript
slots: { content: 'expand-content' }  // 只定义 content
```

## Playground 示例

| 示例 | 路径 |
|------|------|
| 基础表格 | `examples/vxe-table/basic.vue` |
| 远程加载 | `examples/vxe-table/remote.vue` |
| 行编辑 | `examples/vxe-table/edit-row.vue` |
| 固定列 | `examples/vxe-table/fixed.vue` |
| 树形表格 | `examples/vxe-table/tree.vue` |
