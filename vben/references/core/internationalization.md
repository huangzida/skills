# 国际化配置

项目国际化文件位于 `src/locales/` 目录。

**官方文档**：https://doc.vben.pro/guide/advance/i18n.html

## 语言包结构（按路由拆分 + 嵌套组件）

### 目录结构

```
src/locales/
└── langs/
    ├── zh-CN/
    │   ├── common.json      # 公共翻译（按钮、提示等通用字段）
    │   ├── auth.json        # 认证相关（登录、注册等）
    │   ├── dashboard.json   # 概览页
    │   ├── product.json     # 产品页（含组件嵌套）
    │   ├── device.json      # 设备页（含组件嵌套）
    │   ├── protocol.json    # 协议管理页
    │   └── product-category.json  # 产品分类页
    └── en-US/
        ├── common.json
        ├── auth.json
        ├── dashboard.json
        ├── product.json
        ├── device.json
        ├── protocol.json
        └── product-category.json
```

### 命名规则

| 层级 | 命名空间 | 示例 |
|------|---------|------|
| 一级路由 | `auth` / `dashboard` / `product` | `product.title` |
| 页面组件 | 嵌套在路由下 | `product.productEditModal.title` |
| 公共字段 | `common` | `common.confirm` |

## 定义翻译

### 按路由拆分文件

```json
// src/locales/langs/zh-CN/product.json
{
  "title": "产品",
  "addProduct": "新增",
  "searchPlaceholder": "请输入查询内容",

  "productCardItem": {
    "edit": "编辑",
    "delete": "删除",
    "viewDetail": "查看详情"
  },

  "productEditModal": {
    "title": "编辑产品",
    "productName": "产品名称",
    "productNamePlaceholder": "请输入产品名称",
    "confirm": "确定",
    "cancel": "取消"
  },

  "productTableView": {
    "id": "ID",
    "name": "名称",
    "status": "状态"
  }
}
```

### 嵌套组件的 key 组织

```json
{
  "页面级字段": "直接属于页面的字段",

  "组件名1": {
    "字段1": "组件内使用的字段",
    "字段2": "组件内使用的字段"
  },

  "组件名2": {
    "字段1": "组件内使用的字段"
  }
}
```

**推荐组件命名**：
- `productCardItem` - 产品卡片项
- `productEditModal` - 产品编辑弹窗
- `productTableView` - 产品表格视图
- `detailDrawer` - 详情抽屉

## 使用翻译

```vue
<script setup>
import { $t } from '#/locales';
</script>

<template>
  <!-- 页面级字段 -->
  <span>{{ $t('product.title') }}</span>

  <!-- 组件嵌套字段 -->
  <span>{{ $t('product.productCardItem.edit') }}</span>
  <span>{{ $t('product.productEditModal.productName') }}</span>

  <!-- 公共字段 -->
  <button>{{ $t('common.confirm') }}</button>
</template>
```

### key 格式规范

| 场景 | 格式 | 示例 |
|------|------|------|
| 页面级 | `路由.字段` | `product.title` |
| 组件级 | `路由.组件.字段` | `product.productEditModal.title` |
| 公共字段 | `common.字段` | `common.confirm` |
| 通用提示 | `ui.actionMessage.xxx` | `ui.actionMessage.deleteSuccess` |

### 嵌套层级控制

- **最多 2-3 层嵌套**：路由 → 组件 → 字段
- **避免过深嵌套**：过深的嵌套会导致 key 过长，如 `product.editModal.form.label.name`
- **组件名应简洁**：使用 `editModal` 而非 `productEditModal`，因为已经嵌套在 `product` 下

## 切换语言

```typescript
import { useLocale } from '@vben/locales';

const { locale } = useLocale();

// 切换到中文
locale.value = 'zh-CN';

// 切换到英文
locale.value = 'en-US';
```

## 动态语言包

```typescript
import { loadLocaleAsync } from '@vben/locales';

await loadLocaleAsync('zh-CN', () =>
  import('./langs/zh-CN/product.json'),
);
```

## 最佳实践

### 1. 公共字段抽离到 common.json

以下字段应使用 `common.xxx`：
- `common.confirm` - 确定
- `common.cancel` - 取消
- `common.reset` - 重置
- `common.search` - 搜索
- `common.export` - 导出
- `common.import` - 导入
- `common.delete` - 删除
- `common.edit` - 编辑
- `common.add` - 新增
- `common.view` - 查看
- `common.save` - 保存
- `common.close` - 关闭

### 2. 组件字段使用嵌套结构

```json
{
  "productEditModal": {
    "title": "编辑产品",
    "name": "产品名称",
    "confirm": "确定"
  }
}
```

使用时：`$t('product.productEditModal.title')`

### 3. 避免 key 过长

**不推荐**：
```json
{
  "editProductModalFormFieldProductNameLabel": "产品名称"
}
```

**推荐**：
```json
{
  "productEditModal": {
    "productName": "产品名称"
  }
}
```

使用时：`$t('product.productEditModal.productName')`

### 4. 枚举值处理

枚举值（如设备类型、状态）直接放在路由层级：

```json
{
  "directDevice": "直连设备",
  "gatewayDevice": "网关设备",
  "online": "在线",
  "offline": "离线"
}
```

使用时：`$t('product.directDevice')`
