---
layout: element
title: Element Plus的el-input如何获取焦点
date: 2023-10-13 14:02:54
tags: ['Vue', 'Element Plus']
---
Element Plus的官方文档说focus可以使el-input获取焦点但没有说具体如何使用
## 如何聚焦
先建立一个ref对象
```
const inputRef = ref()
```
给el-input加上ref属性
```
<el-input v-model="input" ref="inputRef"></el-input>
```
定义一个方法触发
```
const focusInput = () => { 
  nextTick(() => { inputRef.value.focus() }) 
}
```