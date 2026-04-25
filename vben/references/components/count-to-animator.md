# VbenCountToAnimator 数字滚动动画

数字滚动动画组件，用于展示数字从起始值到目标值的滚动效果。

**官方文档**：https://doc.vben.pro/components/common-ui/count-to-animator.html

**组件位置**：`@vben/common-ui`

## 基础用法

通过 `start-val` 和 `end-val` 设置数字动画的起始值和结束值，配合 `duration` 控制动画时长。

```vue
<script setup lang="ts">
import { VbenCountToAnimator } from '@vben/common-ui';
</script>

<template>
  <VbenCountToAnimator
    :start-val="0"
    :end-val="30000"
    :duration="1500"
  />
</template>
```

## Props

| 属性 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| startVal | 起始值 | `number` | `0` |
| endVal | 结束值 | `number` | `2021` |
| duration | 动画持续时间（毫秒） | `number` | `1500` |
| autoplay | 是否自动播放 | `boolean` | `true` |
| prefix | 前缀 | `string` | `''` |
| suffix | 后缀 | `string` | `''` |
| separator | 千分位分隔符 | `string` | `','` |
| decimal | 小数点分隔符 | `string` | `'.'` |
| color | 文本颜色 | `string` | `''` |
| decimals | 保留小数位数 | `number` | `0` |
| useEasing | 是否启用过渡预设 | `boolean` | `true` |
| transition | 过渡预设名称 | `keyof typeof TransitionPresets` | `'linear'` |

## Events

| 事件名 | 说明 | 类型 |
|--------|------|------|
| started | 动画开始时触发 | `() => void` |
| finished | 动画结束时触发 | `() => void` |

## Methods

| 方法名 | 说明 | 类型 |
|--------|------|------|
| reset | 重置为 `startVal` 并重新执行动画 | `() => void` |

## 常见用法

### 带千分位分隔符

```vue
<VbenCountToAnimator
  :start-val="0"
  :end-val="2000000"
  separator=","
  prefix="$"
/>
<!-- 显示: $2,000,000 -->
```

### 带小数位

```vue
<VbenCountToAnimator
  :start-val="0"
  :end-val="99.8"
  :decimals="1"
  suffix="%"
/>
<!-- 显示: 99.8% -->
```

### 带后缀

```vue
<VbenCountToAnimator
  :start-val="0"
  :end-val="23"
  suffix="ms"
/>
<!-- 显示: 23ms -->
```

## 实际应用示例

### 仪表盘统计卡片

```vue
<script setup lang="ts">
import { VbenCountToAnimator } from '@vben/common-ui';

const overviewItems = [
  {
    title: '总产品数',
    value: 1248,
    decimals: 0,
    color: '#3b82f6',
  },
  {
    title: '设备在线率',
    value: 96,
    decimals: 0,
    suffix: '%',
    color: '#10b981',
  },
  {
    title: '协议合规率',
    value: 98.2,
    decimals: 1,
    suffix: '%',
    color: '#8b5cf6',
  },
];
</script>

<template>
  <div
    v-for="stat in overviewItems"
    :key="stat.title"
    class="stat-card bg-card rounded-xl border border-border p-5"
  >
    <div class="flex flex-col">
      <span class="text-muted-foreground">{{ stat.title }}</span>
      <VbenCountToAnimator
        :end-val="stat.value"
        :start-val="0"
        :suffix="stat.suffix || ''"
        :decimals="stat.decimals ?? 0"
        :duration="1500"
        class="text-4xl font-bold"
        :style="{ color: stat.color }"
      />
    </div>
  </div>
</template>
```

### 数据类型定义

在 mock.ts 中定义数据时，记得添加 `decimals` 属性：

```typescript
export const overviewItems = [
  {
    title: 'dashboard.totalProducts',
    value: 1248,
    decimals: 0,  // 整数，无小数位
    color: '#3b82f6',
  },
  {
    title: 'dashboard.deviceOnlineRate',
    value: 96,
    decimals: 0,
    suffix: '%',
    color: '#10b981',
  },
  {
    title: 'dashboard.protocolComplianceRate',
    value: 98.2,
    decimals: 1,  // 保留1位小数
    suffix: '%',
    color: '#8b5cf6',
  },
];
```

## 注意事项

1. **数据类型**：确保 `value` 是 `number` 类型，不是字符串
2. **decimals 属性**：对于有小数位的数值（如 98.2），需要显式设置 `decimals: 1`
3. **默认值为 0**：如果未设置，默认不显示小数位
4. **颜色设置**：可以通过 `color` 属性或内联样式 `:style="{ color: stat.color }"` 设置颜色
