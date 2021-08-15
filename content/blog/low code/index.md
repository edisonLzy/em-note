---

     title: low code
     date: 2021-08-15T14:35:18.977Z
     description: low code

---

## schema

- 描述组件在画布中的呈现方式

```json
{
   container: {

   },
  block; [

  ]
}
```

- 不同组件使用 `key` 进行区分

## 物料

- 物料注册

```js
function createEditorConfig() {
  const compList = []; // 所有注册的组件
  const compMap = {}; // 渲染时 key的对应关系
  return {
    register: (comp) => {},
  };
}
```

- 通过 provide 提供物料配置给 下文使用

```jsx
setup(){
    provide('config',config)
}
```

```jsx
setup(){
  const config = inject('config')
}
```

## 拖拽

- dragStart dragEnd draggable

```jsx
<div
  draggable
  dragEnd={() => {
    //  清理绑定的事件
  }}
  onDragStart={() => {
    // 开始拖拽
  }}
></div>
```

- dragenter dragover dragleave drop

- 画布中的元素拖拽

1.  定义一个 focus 属性表示是否选中了当前的元素

2.  通过 e.shiftKey 判断是否按住了 shift 再选中

3.  通过一个计算属性,得到所有选中的 block，和没有选中的 block

## TODO

1. 复习 BOM

- offsetX 和 offsetY 是相对于 视口的吗?

- clientX 和 clientY

## get

1.  Vue3 默认支持 jsx 语法

2.  dragover 事件必须禁用默认行为,否则无法触发 drop 事件

3.  e.dateTransfer.dropEffect : 用于控制 鼠标拖拽的图标

```js
e.dateTransfer.dropEffect = "move" | "none";
```

4. emit 派发事件

```jsx
export default defineComponent({
props: {
   modelValue: {
    type: Object
   }
},
emits: ['update:modelValue'] // 更友好的类型提示
setup(props,ctx){
 ctx.emit('update:modelValue', data)
}
})
```

5. vue-template-explore

6. e.shiftKey 用于判断是否按住 是 按住 shift + 点击

7. 三元运算符 结合 数组的 push 简化代码

```js
const c = (d ? a : b).push(e);
```
