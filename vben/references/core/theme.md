# 主题配置

支持完整的主题定制，包括颜色、布局、特殊模式等。

**官方文档**：https://doc.vben.pro/theme.html

## 基础配置

```typescript
// preferences.ts
export const overridesPreferences = defineOverridesPreferences({
  theme: {
    mode: 'light',           // 'light' | 'dark'
    builtinType: 'default',    // 内置主题
    colorPrimary: 'hsl(212 100% 45%)',
  },
});
```

## 内置主题

| 主题 | 说明 |
|------|------|
| `default` | 默认蓝色 |
| `violet` | 紫色 |
| `pink` | 粉色 |
| `rose` | 玫瑰色 |
| `sky-blue` | 天蓝色 |
| `deep-blue` | 深蓝色 |
| `green` | 绿色 |
| `orange` | 橙色 |
| `yellow` | 黄色 |
| `zinc` | 锌灰色 |
| `gray` | 灰色 |

## 特殊模式

```typescript
export const overridesPreferences = defineOverridesPreferences({
  app: {
    colorGrayMode: true,     // 灰色模式
    colorWeakMode: true,     // 色弱模式
  },
});
```

## 自定义主题

```css
/* light 模式 */
[data-theme='my-theme'] {
  --primary: 262.1 83.3% 57.8%;
}

/* dark 模式 */
.dark[data-theme='my-theme'] {
  --primary: 262.1 83.3% 57.8%;
}
```

```typescript
export const overridesPreferences = defineOverridesPreferences({
  theme: {
    builtinType: 'my-theme',
  },
});
```

## 水印

```typescript
export const overridesPreferences = defineOverridesPreferences({
  app: {
    watermark: true,
  },
});
```
