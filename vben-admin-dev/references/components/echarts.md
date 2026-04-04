# 图表组件（ECharts）

vben-admin 内置的 ECharts 封装，提供组合式函数和容器组件，简化图表开发。

**官方文档**：https://echarts.apache.org/zh/index.html

**Playground**：`playground/src/views/dashboard/analytics/`

## 导入

```typescript
import { EchartsUI, useEcharts } from '@vben/plugins/echarts';
import type { EchartsUIType, ECOption } from '@vben/plugins/echarts';
```

## 基础用法

```vue
<script setup lang="ts">
import type { EchartsUIType } from '@vben/plugins/echarts';

import { onMounted, ref } from 'vue';

import { EchartsUI, useEcharts } from '@vben/plugins/echarts';

const chartRef = ref<EchartsUIType>();
const { renderEcharts } = useEcharts(chartRef);

onMounted(() => {
  renderEcharts({
    xAxis: {
      data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
      type: 'category',
    },
    yAxis: {
      type: 'value',
    },
    series: [
      {
        data: [120, 200, 150, 80, 70, 110, 130],
        type: 'bar',
      },
    ],
  });
});
</script>

<template>
  <EchartsUI ref="chartRef" />
</template>
```

## 图表类型

### 折线图（Line Chart）

```typescript
renderEcharts({
  xAxis: {
    data: Array.from({ length: 18 }).map((_, i) => `${i + 6}:00`),
    type: 'category',
    boundaryGap: false,
  },
  yAxis: {
    type: 'value',
    splitNumber: 4,
  },
  series: [
    {
      data: [111, 2000, 6000, 16000, 33333, 55555, 64000],
      smooth: true,
      type: 'line',
      areaStyle: {},
      itemStyle: {
        color: '#5ab1ef',
      },
    },
  ],
});
```

### 柱状图（Bar Chart）

```typescript
renderEcharts({
  xAxis: {
    data: ['1月', '2月', '3月', '4月', '5月', '6月'],
    type: 'category',
  },
  yAxis: {
    type: 'value',
    splitNumber: 4,
    max: 8000,
  },
  grid: {
    left: '1%',
    right: '1%',
    bottom: 0,
    containLabel: true,
    top: '2%',
  },
  series: [
    {
      data: [3000, 2000, 3333, 5000, 3200, 4200],
      barMaxWidth: 80,
      type: 'bar',
    },
  ],
  tooltip: {
    trigger: 'axis',
    axisPointer: {
      lineStyle: {
        width: 1,
      },
    },
  },
});
```

### 饼图（Pie Chart）

```typescript
renderEcharts({
  legend: {
    bottom: '2%',
    left: 'center',
  },
  series: [
    {
      data: [
        { name: '搜索引擎', value: 1048 },
        { name: '直接访问', value: 735 },
        { name: '邮件营销', value: 580 },
        { name: '联盟广告', value: 484 },
      ],
      type: 'pie',
      radius: ['40%', '65%'],
      itemStyle: {
        borderRadius: 10,
        borderWidth: 2,
      },
      color: ['#5ab1ef', '#b6a2de', '#67e0e3', '#2ec7c9'],
      emphasis: {
        label: {
          show: true,
          fontSize: 12,
          fontWeight: 'bold',
        },
      },
    },
  ],
  tooltip: {
    trigger: 'item',
  },
});
```

### 南丁格尔玫瑰图（Rose Chart）

```typescript
renderEcharts({
  series: [
    {
      data: [
        { name: '外包', value: 500 },
        { name: '定制', value: 310 },
        { name: '技术支持', value: 274 },
        { name: '远程', value: 400 },
      ],
      type: 'pie',
      radius: '80%',
      roseType: 'radius',
      center: ['50%', '50%'],
      animationDelay: () => Math.random() * 400,
      animationEasing: 'exponentialInOut',
      animationType: 'scale',
    },
  ],
});
```

### 雷达图（Radar Chart）

```typescript
renderEcharts({
  legend: {
    bottom: 0,
    data: ['访问', '趋势'],
  },
  radar: {
    indicator: [
      { name: '网页' },
      { name: '移动端' },
      { name: 'Ipad' },
      { name: '客户端' },
      { name: '第三方' },
      { name: '其它' },
    ],
    radius: '60%',
    splitNumber: 8,
  },
  series: [
    {
      type: 'radar',
      data: [
        {
          name: '访问',
          value: [90, 50, 86, 40, 50, 20],
          itemStyle: { color: '#b6a2de' },
        },
        {
          name: '趋势',
          value: [70, 75, 70, 76, 20, 85],
          itemStyle: { color: '#5ab1ef' },
        },
      ],
      symbolSize: 0,
      areaStyle: {
        opacity: 1,
        shadowBlur: 0,
        shadowColor: 'rgba(0,0,0,.2)',
        shadowOffsetX: 0,
        shadowOffsetY: 10,
      },
    },
  ],
});
```

## useEcharts API

| 方法 | 说明 |
|------|------|
| `renderEcharts(options)` | 渲染图表（首次/替换） |
| `updateData(options)` | 更新数据（合并模式） |
| `resize()` | 手动触发尺寸调整 |
| `getChartInstance()` | 获取 ECharts 实例 |

### renderEcharts vs updateData

```typescript
// renderEcharts - 清除旧图表，重新渲染
renderEcharts({
  series: [{ data: [1, 2, 3], type: 'bar' }],
});

// updateData - 合并配置，保留动画
updateData({
  series: [{ data: [4, 5, 6] }],
}, false, false);
// 参数2: notMerge - false=合并, true=替换
// 参数3: lazyUpdate - true时不立即重绘
```

## 容器配置

```vue
<!-- 高度默认 300px，宽度默认 100% -->
<EchartsUI />

<!-- 自定义尺寸 -->
<EchartsUI height="400px" width="600px" />
```

## 特性

### 自动响应式

`useEcharts` 自动监听窗口大小变化，图表会自动调整尺寸。

### 暗色模式支持

自动适配主题，无需手动处理颜色。

### 懒加载

当图表容器不可见时（`offsetHeight === 0`），会自动延迟渲染。

## Playground 示例

| 示例 | 路径 | 说明 |
|------|------|------|
| 流量趋势 | `dashboard/analytics/analytics-trends.vue` | 多系列折线图+面积图 |
| 月访问量 | `dashboard/analytics/analytics-visits.vue` | 柱状图 |
| 访问数量 | `dashboard/analytics/analytics-visits-data.vue` | 雷达图 |
| 访问来源 | `dashboard/analytics/analytics-visits-source.vue` | 饼图 |
| 商业占比 | `dashboard/analytics/analytics-visits-sales.vue` | 南丁格尔玫瑰图 |

## 完整示例：折线图

```vue
<script setup lang="ts">
import type { EchartsUIType } from '@vben/plugins/echarts';

import { onMounted, ref } from 'vue';

import { EchartsUI, useEcharts } from '@vben/plugins/echarts';

const chartRef = ref<EchartsUIType>();
const { renderEcharts } = useEcharts(chartRef);

onMounted(() => {
  renderEcharts({
    grid: {
      left: '1%',
      right: '1%',
      bottom: 0,
      containLabel: true,
      top: '2%',
    },
    series: [
      {
        areaStyle: {},
        data: [111, 2000, 6000, 16000, 33333, 55555],
        itemStyle: { color: '#5ab1ef' },
        smooth: true,
        type: 'line',
      },
      {
        areaStyle: {},
        data: [33, 66, 88, 333, 3333, 6200],
        itemStyle: { color: '#019680' },
        smooth: true,
        type: 'line',
      },
    ],
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        lineStyle: {
          color: '#019680',
          width: 1,
        },
      },
    },
    xAxis: {
      axisTick: { show: false },
      boundaryGap: false,
      data: Array.from({ length: 6 }).map((_, i) => `${i + 6}:00`),
      splitLine: {
        lineStyle: { type: 'solid', width: 1 },
        show: true,
      },
      type: 'category',
    },
    yAxis: [
      {
        axisTick: { show: false },
        max: 80000,
        splitArea: { show: true },
        splitNumber: 4,
        type: 'value',
      },
    ],
  });
});
</script>

<template>
  <EchartsUI ref="chartRef" />
</template>
```
