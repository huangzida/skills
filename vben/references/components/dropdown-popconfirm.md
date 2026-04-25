# Dropdown 和 Popconfirm 最佳实践

## 核心规则

### Dropdown 事件处理

只使用 `@menu-click`，不要在 menuItem 上定义 `onClick`：

```typescript
const menuItems: MenuItemType[] = [
  { key: 'export', label: '导出' },
  { key: 'delete', label: '删除', danger: true },
];

function handleMenuClick({ key }: { key: string }) {
  if (key === 'export') handleExport();
  else if (key === 'delete') handleDelete();
}
```

```vue
<Dropdown :menu="{ items: menuItems }" @menu-click="handleMenuClick">
  <Button>更多</Button>
</Dropdown>
```

### 危险操作处理

删除等危险操作**不要放在 Dropdown 中**，使用独立按钮 + Popconfirm：

```vue
<Popconfirm title="确定删除？" @confirm="handleDelete">
  <Button danger>删除</Button>
</Popconfirm>
```

## 常见问题

| 问题 | 原因 | 解决 |
|------|------|------|
| 点击菜单项触发两次 | menuItem 上同时定义了 onClick | 只使用 @menu-click |
| Popconfirm 不弹出 | Popconfirm 在 Dropdown 内无法正常工作 | 将按钮独立出来 |
