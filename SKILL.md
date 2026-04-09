# openclaw-skill-frontend-gen

## 简介

React / Vue 组件自动生成 Skill。根据描述或截图生成完整组件代码。

## 触发场景

- "写一个组件"、"generate component"
- "帮我生成前端"、"build this UI"
- "从设计稿生成代码"、"Figma to code"
- 描述 UI 需求（表单、列表、卡片、弹窗等）

## 核心流程

### 第一步：识别需求

1. **框架**：React（TS）还是 Vue（TS）？
2. **类型**：函数组件还是 Class 组件？
3. **功能**：展示型 / 表单型 / 交互型？
4. **样式**：Tailwind / CSS Modules / styled-components / plain CSS？
5. **状态**：是否有内部状态？是否需要 props？是否连接 Redux/Vuex/Pinia？

### 第二步：生成组件

**React 函数组件模板：**

```tsx
import React, { useState } from 'react';

interface Props {
  className?: string;
  onSubmit?: (data: unknown) => void;
}

export const ComponentName: React.FC<Props> = ({ className = '', onSubmit }) => {
  const [loading, setLoading] = useState(false);
  const [value, setValue] = useState('');

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setLoading(true);
    try {
      onSubmit?.({ value });
    } finally {
      setLoading(false);
    }
  };

  return (
    <form className={className} onSubmit={handleSubmit}>
      {/* 内容 */}
    </form>
  );
};
```

**Vue 3 Composition API 模板：**

```vue
<script setup lang="ts">
import { ref } from 'vue';

interface Props {
  className?: string;
}

const props = withDefaults(defineProps<Props>(), {
  className: '',
});

const emit = defineEmits<{
  submit: [data: unknown];
}>();

const loading = ref(false);
const value = ref('');

const handleSubmit = async () => {
  loading.value = true;
  try {
    emit('submit', { value: value.value });
  } finally {
    loading.value = false;
  }
};
</script>

<template>
  <form :class="props.className" @submit.prevent="handleSubmit">
    <!-- 内容 -->
  </form>
</template>

<style scoped>
/* 样式 */
</style>
```

### 第三步：最佳实践

**TypeScript 严格：**
- 所有 props 必须有类型
- 事件处理函数类型完整
- 避免 any

**可访问性（A11y）：**
```tsx
// 表单必须有 label
<label htmlFor="email">邮箱</label>
<input id="email" type="email" />

// 按钮加载状态
<button disabled={loading}>
  {loading ? '加载中...' : '提交'}
</button>

// 图片必须 alt
<img src="..." alt="描述" />
```

**响应式：**
```css
/* 移动优先 */
.container { padding: 16px; }
@media (min-width: 768px) {
  .container { padding: 32px; max-width: 1200px; }
}
```

**Tailwind 示例：**
```tsx
<div className="flex items-center justify-between p-4 bg-white rounded-lg shadow">
  <h2 className="text-lg font-semibold">标题</h2>
  <button className="px-4 py-2 text-sm bg-blue-500 text-white rounded hover:bg-blue-600">
    操作
  </button>
</div>
```

### 第四步：输出格式

```
## 组件：<名称>

### 技术栈
- 框架：React + TypeScript
- 样式：Tailwind CSS
- 状态：useState + props

### 代码
```tsx
<完整代码>
```

### 使用示例
```tsx
<ComponentName onSubmit={(data) => console.log(data)} />
```

### 注意事项
- ...
```

## 注意事项

- 如果框架不明确，先问用户
- 复杂页面建议拆分为多个小组件
- 生成后检查 TypeScript 类型错误
- Tailwind 类名拼写检查