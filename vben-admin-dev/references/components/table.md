# 表格组件（useVbenVxeGrid）

vben-admin 提供的表格组件，基于 vxe-table，集成了搜索表单。

**官方文档**：https://doc.vben.pro/components/common-ui/vben-vxe-table.html

**Playground**：`playground/src/views/examples/vxe-table/`

## 基础用法

```vue
<script setup lang="ts">
import { useVbenVxeGrid } from '#/adapter/vxe-table';

const [Grid] = useVbenVxeGrid({
  gridOptions: {
    columns: [
      { type: 'seq', title: '序号', width: 50 },
      { field: 'name', title: '姓名', sortable: true },
    ],
    data: [],
    height: 'auto',
    rowConfig: { keyField: 'id' },
  },
});
</script>

<template>
  <Grid />
</template>
```

## 核心配置

### 列配置

```typescript
columns: [
  { type: 'seq', title: '序号', width: 50 },
  { type: 'checkbox', title: '勾选', width: 50 },
  { field: 'name', title: '姓名', width: 120, sortable: true },
  { field: 'status', title: '状态', cellRender: { name: 'CellTag' } },
]
```

### 代理配置（远程数据）

```typescript
proxyConfig: {
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

### 工具栏

```typescript
toolbarConfig: {
  custom: true,    // 自定义列
  export: true,    // 导出
  refresh: true,   // 刷新
  zoom: true,      // 全屏
}
```

### 分页

```typescript
pagerConfig: {
  enabled: true,
  currentPage: 1,
  pageSize: 10,
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
    submitOnChange: true,
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
  </Grid>
</template>

<style scoped>
.expand-wrapper {
  padding: 16px;
  background: #fafafa;
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

## Playground 示例

| 示例 | 路径 |
|------|------|
| 基础表格 | `examples/vxe-table/basic.vue` |
| 远程加载 | `examples/vxe-table/remote.vue` |
| 行编辑 | `examples/vxe-table/edit-row.vue` |
| 固定列 | `examples/vxe-table/fixed.vue` |
| 树形表格 | `examples/vxe-table/tree.vue` |
