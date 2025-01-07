---



title: vue hooks使用有感

tags: [vue,hooks]

author: codefish

date: 2023-07-10 19:20:34

categories: vue hooks

top_img: /img/13.jpg

cover: /img/13.jpg



---

# Vue3 表单逻辑抽象与复用方案设计

## 1. 背景与问题

在开发中经常遇到复杂的表单页面,这些表单通常具有以下特点:

- 字段较多且类型复杂
- 存在多个依赖关系(如审核人依赖组织)
- 包含特殊功能(如地图选择)
- 需要处理编辑和新增两种场景
- 数据提交和回显需要转换

这些特点导致以下问题:

1. 代码组织混乱
2. 逻辑复用困难
3. 维护成本高
4. 代码重复率高

## 2. 解决方案

### 2.1 整体思路

将表单逻辑分为三层:

1. 基础功能层 - 处理常规表单字段
2. 定制功能层 - 处理特定业务逻辑
3. 特殊功能层 - 处理复杂交互功能

同时抽象出通用的工具层:

1. 通用表单数据处理 hook
2. 通用表单配置 hook
3. 工具函数库

### 2.2 核心抽象

#### 2.2.1 通用表单数据处理 Hook



```typescript
// useFormData.ts
export function useFormData<T extends object, S = T>(
initialData: T,
options: {
transform?: (data: T) => S;
reverseTransform?: (data: S) => T;
validate?: (data: T) => Promise<boolean>;
}
) {
const formData = reactive<T>(initialData);
// 转换提交数据
const transformSubmitData = () => {
return options.transform ? options.transform(formData) : formData;
};
// 处理编辑数据
const handleEditData = (data: S) => {
const transformed = options.reverseTransform ? options.reverseTransform(data) : data;
Object.assign(formData, transformed);
};
return {
formData,
transformSubmitData,
handleEditData
};
}
```



#### 2.2.2 通用表单配置 Hook

```typescript
typescript
// useFormConfig.ts
export function useFormConfig(
getConfig: () => FormItemConfig[],
dependencies?: {
watch: string[];
handler: (...args: any[]) => void;
}[]
) {
const formConfig = computed(() => ({
formItemsConfig: getConfig()
}));
if (dependencies) {
dependencies.forEach(dep => {
watch(dep.watch, dep.handler);
});
}
return { formConfig };
}
```

### 2.3 实现层级

#### 2.3.1 基础功能层

处理基本表单字段,如:

- 活动名称
- 活动描述
- 活动时间
- 联系方式

```typescript
typescript
// useBaseForm.ts
export function useBaseForm() {
const { formData, transformSubmitData, handleEditData } = useFormData<BaseFormData>(
{
name: '',
description: '',
activityTime: []
},
{
transform: (data) => ({
name: data.name,
startTime: formatTimestamp(data.activityTime[0])
})
}
);
return {
baseForm: formData,
transformBaseData: transformSubmitData,
handleBaseEdit: handleEditData
};
}
```

#### 2.3.2 定制功能层

处理特定业务逻辑,如:

- 审核人选择
- 组织选择
- 权限控制



```typescript
typescript
// useCustomForm.ts
export function useCustomForm() {
const { formData, transformSubmitData } = useFormData<CustomFormData>(
{
cid: '',
auditor: []
},
{
transform: (data) => ({
cid: data.cid,
auditor: arrayToString(data.auditor)
})
}
);
return {
customForm: formData,
transformCustomData: transformSubmitData
};
}
```

#### 2.3.3 特殊功能层

处理复杂交互功能,如:

- 地图选择
- 文件上传
- 自定义组件



```typescript
typescript
// useSpecialForm.ts
export function useSpecialForm() {
const { formData, transformSubmitData } = useFormData<SpecialFormData>(
{
signType: 1,
signPlace: '',
attachments: []
}
);
// 地图相关方法
const openMapSelect = () => {
// 实现地图选择逻辑
};
return {
specialForm: formData,
transformSpecialData: transformSubmitData,
openMapSelect
};
}
```

### 2.4 组合使用

```typescript
typescript
// useActivityForm.ts
export function useActivityForm(isEdit = false) {
const base = useBaseForm();
const custom = useCustomForm();
const special = useSpecialForm();
// 合并所有配置
const formConfig = computed(() => ({
formItemsConfig: [
...base.formConfig.value.formItemsConfig,
...custom.formConfig.value.formItemsConfig,
...special.formConfig.value.formItemsConfig
]
}));
// 统一处理提交
const handleSubmit = async () => {
const submitData = {
...base.transformBaseData(),
...custom.transformCustomData(),
...special.transformSpecialData()
};
return submitData;
};
return {
formConfig,
handleSubmit
};
}
```

## 3. 使用示例

```js
vue
<script setup>
import { useActivityForm } from './hooks/useActivityForm';
const {
formConfig,
handleSubmit
} = useActivityForm(isEdit);
// 提交处理
const onSubmit = async () => {
const data = await handleSubmit();
await submitAPI(data);
};
</script>
<template>
<tForm
:config="formConfig"
@submit="onSubmit"
/>
</template>
```

## 4. 方案优势

1. 高复用性
   - 通用逻辑抽象
   - 配置化驱动
   - 模块化设计

2. 类型安全
   - TypeScript 支持
   - 完整类型定义
   - 类型推导

3. 可维护性
   - 关注点分离
   - 逻辑清晰
   - 易于扩展

4. 开发效率
   - 减少重复代码
   - 标准化接口
   - 快速开发

## 5. 注意事项

1. 合理划分功能层级
2. 保持接口一致性
3. 注意性能优化
4. 完善错误处理
5. 做好类型定义

## 6. 后续优化

1. 添加更多预设配置
2. 扩展验证规则
3. 优化性能
4. 添加单元测试
5. 完善文档

## 7. 总结

通过合理的抽象和模块化设计,我们可以大大提高表单开发的效率和代码质量。这个方案不仅解决了当前的问题,也为后续的开发提供了良好的基础。