# 组件拆分最佳实践

## 原则

**组件应保持单一职责，颗粒度细、易于维护和扩展。**

当组件超过一定行数或包含多个功能区块时，应主动拆分。

| 指标 | 阈值 | 动作 |
|------|------|------|
| 代码行数 | > 200 行 | 考虑拆分 |
| 功能区块 | ≥ 2 个独立区块 | 必须拆分 |
| 组件嵌套 | > 3 层 | 重构结构 |

## 拆分决策树

```
组件是否包含多个独立功能区块？
├── 否 → 继续观察，保持单组件
└── 是 → 进入拆分流程
    │
    ├── 每个区块是否可复用？
    │   ├── 是 → 拆分为独立可复用组件
    │   └── 否 → 进入下一步
    │
    └── 数据是否在区块间共享？
        ├── 否 → 各区块自包含，通过 props 接收数据
        └── 是 → 考虑是否有公共状态，提炼到父组件
```

## 拆分模式

### 1. 仪表板组件拆分

**典型结构：**

```
dashboard/
├── components/
│   ├── cards/
│   │   └── OverviewCards.vue       # 顶部概览统计卡片组
│   ├── monitor/
│   │   └── ProtocolMonitor.vue     # 协议执行监控模块
│   ├── charts/
│   │   ├── DeviceDonutChart.vue    # 设备状态环形图
│   │   └── ProductBarChart.vue     # 产品分类柱状图
│   └── alerts/
│       └── AlertList.vue          # 实时告警列表
├── index.vue                       # 主组件（布局协调）
└── mock.ts                         # Mock 数据
```

**拆分要点：**
- Header、卡片组、图表、列表等独立区块各自成组件
- 图表组件内部自包含（数据和渲染逻辑）
- Mock 数据集中管理，按需导入

### 2. 卡片组件拆分

**两层结构：**

```
components/
├── ProductCard.vue      # 单个卡片（可复用）
└── ProductCardList.vue  # 卡片列表容器
```

**示例：**

```vue
<!-- ProductCard.vue -->
<script lang="ts" setup>
defineProps<{
  title: string;
  description: string;
  image: string;
  status: 'active' | 'inactive';
}>();
</script>

<template>
  <div class="product-card">
    <img :src="image" :alt="title" />
    <h3>{{ title }}</h3>
    <p>{{ description }}</p>
    <span :class="status">{{ status }}</span>
  </div>
</template>
```

```vue
<!-- ProductCardList.vue -->
<script lang="ts" setup>
import ProductCard from './ProductCard.vue';

defineProps<{
  products: Array<{
    id: string;
    title: string;
    description: string;
    image: string;
    status: 'active' | 'inactive';
  }>;
}>();
</script>

<template>
  <div class="product-list">
    <ProductCard
      v-for="product in products"
      :key="product.id"
      :title="product.title"
      :description="product.description"
      :image="product.image"
      :status="product.status"
    />
  </div>
</template>
```

### 3. 表单组件拆分

**按业务模块拆分：**

```
components/
├── forms/
│   ├── BasicInfoForm.vue      # 基本信息
│   ├── DeviceConfigForm.vue   # 设备配置
│   └── NetworkForm.vue        # 网络配置
└── ProductForm.vue            # 产品表单（容器）
```

## 数据管理策略

### 混合模式（推荐）

| 数据类型 | 管理方式 | 说明 |
|----------|----------|------|
| 静态配置 | 组件内部定义 | 如卡片样式、图标配置 |
| 业务数据 | 通过 props 传递 | 父组件统一管理 |
| 事件回调 | 通过 emits 传递 | 子组件通知父组件 |
| 共享状态 | 使用 Pinia Store | 跨组件共享 |

### 示例

```typescript
// 主组件（数据管理）
const systemInfo = ref({
  deviceModel: 'EG8200',
  firmwareVersion: 'v2.1.0',
});

// 子组件（展示逻辑）
const props = defineProps<{
  deviceModel: string;
  firmwareVersion: string;
}>();
```

## 目录结构规范

### 按功能组织

```
views/
├── product/
│   ├── components/           # 产品模块专用组件
│   │   ├── ProductTable.vue
│   │   ├── ProductForm.vue
│   │   └── ProductDetail.vue
│   └── index.vue            # 产品页面入口
├── device/
│   ├── components/
│   │   ├── DeviceList.vue
│   │   └── DeviceStatus.vue
│   └── index.vue
└── dashboard/
    ├── components/          # 仪表板组件
    │   ├── charts/
    │   ├── cards/
    │   └── monitor/
    └── index.vue
```

### 命名规范

| 类型 | 命名方式 | 示例 |
|------|----------|------|
| 容器组件 | `XxxContainer` / `XxxList` | `AlertContainer`, `CardList` |
| 单个组件 | `XxxCard` / `XxxItem` | `StatCard`, `AlertItem` |
| 区块组件 | `XxxSection` / `XxxBlock` | `HeaderSection`, `InfoBlock` |
| 图表组件 | `XxxChart` | `DeviceDonutChart` |

## 验证清单

拆分完成后验证：

- [ ] TypeScript 类型检查通过：`pnpm run check:type`
- [ ] ESLint 检查通过：`pnpm run lint`
- [ ] 组件可独立运行
- [ ] 数据流清晰（props → component → emits）
- [ ] 无重复代码

## 何时重构

遇到以下情况时，考虑重构：

1. **组件行数 > 200 行** - 拆分功能区块
2. **模板中 v-for 超过 3 层** - 抽象中间层
3. **props 超过 8 个** - 使用配置对象或 context
4. **多个组件共享相似逻辑** - 提取公共组件或 composable
5. **组件职责不清晰** - 明确边界，重新拆分

## 最佳实践总结

1. **先设计，后拆分** - 明确组件边界和接口
2. **保持粒度细** - 单一职责，可测试
3. **数据管理清晰** - 混合模式，平衡独立性和灵活性
4. **命名规范** - 见名知意，一致性
5. **持续重构** - 定期审视，及时拆分
