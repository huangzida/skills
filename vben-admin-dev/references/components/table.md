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
