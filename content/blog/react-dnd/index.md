# react-dnd

## 概念

**backends**

> 1. 一些可插拔的(拖拽接口): 可以使用移动端的API实现拖拽
> 2. 在底层，backends做的事情就是把`DOM事件`转换成React DnD可以处理的`Redux actions`

**Items and Types**

> 1. Items: 是React-dnd描述被拖动对象的抽象(`不是dom也不是Component`)，用纯对象描述拖拽数据，可以帮助你`解耦组件和使组件间`互无干扰。
> 2. Type: 是一个字符串（或一个symbol）在应用中唯一标识items的完整类型

**Monitors**

> 1. 暴露`DnD`内部的状态给组件,通过定义`collect`收集函数来收集这些状态

**Connectors**

> 1. backend掌控的是`DOM事件`，而组件用的是`React（虚拟DOM）描述DOM`，那么backend怎么知道哪些DOM节点应该被监听勒？通过输入connectors。
> 2. Hooks 通过`ref`来指定`拖拽目标`和`drop目标`

**Drag Sources and Drop Targets**

> 1. 拖拽源: 每个`拖拽源`都需要`注册一个特定的类型(type)`，并且必须实现一个通过组件props生成项的方法
>
> 2. 放置目标: 唯一的区别在于一个放置目标`可以同时注册几个项类型`，而不是提供一个项，它能掌控自己的hover和drop事件

##  基本使用

**安装**

```shell
npm install react-dnd react-dnd-html5-backend
```

**Common Components**

> `DndProvider`： https://react-dnd.github.io/react-dnd/docs/api/dnd-provider

```tsx
import { StrictMode } from 'react'
import ReactDOM from 'react-dom'
import App from './App'
// HTML5Backend: 提供H5的 拖拽API 操作
import { HTML5Backend } from 'react-dnd-html5-backend'
import { DndProvider } from 'react-dnd'
ReactDOM.render(
  <StrictMode>
    { {/*  通过 backend属性注入  HTML5Backend */}
    <DndProvider backend={HTML5Backend}>
      <App />
    </DndProvider>
  </StrictMode>,
  document.getElementById('root')
)
```

**Drag Sources**


```tsx
const Box = () => {
  const [count, setCount] = useState(0)
  const [{ isDraging }, drag, dragPreview] = useDrag(() => ({
    type: 'box',
    item: {
      //拖拽数据
      count
    },
    collect: monitor => ({
      isDraging: monitor.isDragging()
    })
  }),[count])
  const text = isDraging ? '正在拖拽' : count;
  const increase  = useMemo(()=>()=>{
    setCount(count + 1)
  },[count])
  return (
    <>
      <DragPreviewImage src={pointIcon} connect={dragPreview}></DragPreviewImage>
      <div 
      ref={drag}
      onClick={increase}
      className='box'>{text}</div>
    </>
  )
}
```

**Drop Target**

```tsx
function useBoardList(){
  const [boxList, setBoxList] = useState<string[]>([])
  const [{item},drop] =  useDrop(()=>({
    accept:'box',
    collect(monitor){
      return {
         item:monitor.getDropResult()
      }
    }
  }),[])
  useEffect(() => {
     if(item){
     setBoxList([...boxList,item.count])
     }
  }, [item])
  return [boxList,drop] as const
}

const Board = () => {
  const [boxList,drop] = useBoardList()
  const listRender = useMemo(() => {
    return boxList.map(it=>{
      return <div className='box'>{it}</div>
    })
  },[boxList])
  
  return <div
   ref={drop}
   className='board'>{listRender}</div>
}
```



# 参考

[官网]: https://react-dnd.github.io/react-dnd/docs/api/hooks-overview

