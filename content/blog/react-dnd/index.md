---

     title: react-dnd
     date: 2021-08-11T13:16:24.457Z
     description: # react-dn

---

## backends

> 1. 一些可插拔的(拖拽接口)
> 2. 在底层，backends 做的事情就是把`DOM事件`转换成 React DnD 可以处理的`Redux actions`

## Items and Types

> 1. Items: 是 React-dnd 描述被拖动对象的抽象(`不是dom也不是Component`)，用纯对象描述拖拽数据，可以帮助你`解耦组件和使组件间`互无干扰。
> 2. Type: 是一个字符串（或一个 symbol）在应用中唯一标识 items 的完整类型

## Monitors

> 1. 暴露`DnD`内部的状态给组件,通过定义`collect`收集函数来收集这些状态

## Connectors

> 1. backend 掌控的是`DOM事件`，而组件用的是`React（虚拟DOM）描述DOM`，那么 backend 怎么知道哪些 DOM 节点应该被监听勒？通过输入 connectors。
> 2. Hooks 通过`ref`来指定`拖拽目标`和`drop目标`

## Drag Sources and Drop Targets

> 1. 拖拽源: 每个`拖拽源`都需要`注册一个特定的类型(type)`，并且必须实现一个通过组件 props 生成项的方法
>
> 2. 放置目标: 唯一的区别在于一个放置目标`可以同时注册几个项类型`，而不是提供一个项，它能掌控自己的 hover 和 drop 事件

## useDrag

> 1. Item:

## useDrop
