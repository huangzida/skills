# Page 页面组件

页面内容区最常用的顶层布局容器，内置标题区、内容区和底部区。

**官方文档**：https://doc.vben.pro/components/common-ui/page.html

**Playground**：`playground/src/views/examples/page/`

## 基础用法

```vue
<template>
  <Page title="页面标题" description="页面描述">
    <!-- 页面内容 -->
  </Page>
</template>
```

## 自动高度

```vue
<template>
  <Page title="页面标题" auto-content-height>
    <div class="h-full overflow-auto">
      <!-- 内容 -->
    </div>
  </Page>
</template>
```

## 完整示例

```vue
<template>
  <Page
    title="用户管理"
    description="管理系统用户信息"
    auto-content-height
  >
    <template #extra>
      <Button type="primary">新增用户</Button>
    </template>

    <!-- 页面内容 -->
    <Grid />

    <template #footer>
      <Pagination />
    </template>
  </Page>
</template>
```

## Props

| 属性 | 说明 | 类型 |
|------|------|------|
| title | 页面标题 | `string` |
| description | 页面描述 | `string` |
| auto-content-height | 自动计算内容区高度 | `boolean` |

## 插槽

| 插槽 | 说明 |
|------|------|
| default | 页面内容 |
| extra | 页面头部右侧内容 |
| footer | 页面底部内容 |

## 配合 Modal/Drawer 使用

当使用 `appendToMain` 时，需要开启 `auto-content-height`：

```vue
<template>
  <Page auto-content-height>
    <Modal />
  </Page>
</template>
```
