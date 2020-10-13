<!--
 * @Author: your name
 * @Date: 2020-09-28 18:37:08
 * @LastEditTime: 2020-10-13 09:56:25
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \WisdomAuditWebc:\Users\Administrator\Desktop\JS基础\React Hooks.md
-->

# React Hooks

React Hooks 的意思是，组件尽量写成纯函数，如果需要外部功能和副作用，就用钩子把外部代码"钩"进来。React Hooks 就是那些钩子。

## useStates

**状态钩子**

`useState()`用于为函数组件引入状态（state）。纯函数不能有状态，所以把状态放在钩子里面。

## useEffect

**副作用钩子**

`useEffect()`用来引入具有副作用的操作，最常见的就是向服务器请求数据。以前，放在`componentDidMount`里面的代码，现在可以放在`useEffect()`。

## useContext

**共享状态钩子**

```javascript
// 创建上下文, context.js
import React, { createContext, useState } from 'react'
import { Counter } './counter'
export const countContext = createContext()

export default () => {
  const [count, setCount] = useState(0)
  return (
    <div>
      <CountContext.Provider value={count, setCount}>
        <Counter />
      </CountContext.Provider>
    </div>
  )
}
// 使用上下文, counter.js
import React, { useContext } from 'react'
import { CountContext } from './context'
export default () => {
  const { count, setCount } = useContext(CountContext)
  return (
    <div>
      <h2>{count}</h2>
      <span onClick={ () => setCount(count + 1) }>increment</span>
    </div>
  )
}
```

## useReducer

**状态管理器通信**

```javascript
// 定义reducer
const initState = { count: 1, color: '' }
const reducer = function(state, {type, payload }) {
  switch (type) {
    case 'increment':
      return { ...state, count: state.count + 1 }
    case 'theme':
      return { ...state, color: payload.color }
    default:
      throw new Error()
  }
}
...
// 组件调用
const [state, dispatch] = useReducer(reducer, initState)

function increment() {
  dispatch({ type: 'increment' })
}

function changeTheme() {
  dispatch({ type: 'theme', payload: {color: 'red' } })
}
```

### 与`useState`的区别

- 当 state 状态值结构比较复杂时，使用 useReducer 更有优势。
- 使用 useState 获取的 setState 方法更新数据时是异步的；而使用 useReducer 获取的 dispatch 方法更新数据是同步的。

## useRef

`useRef` 返回一个可变的 ref 对象，其 .current 属性被初始化为传入的参数（initialValue）。**返回的 ref 对象在组件的整个生命周期内保持不变**。

`useRef`的作用： 

- ### 用法一
```javascript
// 获取DOM元素的节点 获取子组件的实例 渲染周期之间共享数据的存储（state不能存储跨渲染周期的数据，因为state的保存会触发组件重渲染）
import React, { useEffect, useRef } from 'react'

export default () => {
  const ref = useRef()

  useEffect(() => {
    console.log(ref.current)
  }, [])

  return <h1 ref={ref}>ref dom</h1>
}
```
- ### 用法二
```javascript
//  useRef() 生成的 ref 的 current 中存入元素、字符串
import React, { useEffect, useRef } from 'react'

export default () => {
  // 使用 useRef 创建 inputEl 
  const inputEl = useRef(null)
  const [text, updateText] = useState('')
  // 使用 useRef 创建 textRef 
  const textRef = useRef()
  useEffect(() => {
    // 将 text 值存入 textRef.current 中
    textRef.current = text
    console.log('textRef.current：', textRef.current)
  }， [text])
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.value = "Hello, useRef"
  }
  return (
    <>
      {/* 保存 input 的 ref 到 inputEl */}
      <input ref={ inputEl } type="text" />
      <button onClick={ onButtonClick }>在 input 上展示文字</button>
      <br />
      <input value={text} onChange={e => updateText(e.target.value)} />
    </>
  )
}
```
- ### 用法三
```javascript
// 使用 useRef 来实现实时得到新的值，可以很好的解决闭包带来的不方便性
import React, { useRef, useState } from 'react'

export default () => {
  const [count, setCount] = useState(0)
  const lastestCount = useRef()
  lastestCount.current = count
  //const lastestCount = useRef(count) // 直接传入count, lastestCount.current的值永远是0
  function handleAlertClick() {
    setTimeout(() => {
      alert(`You clicked on ${lastestCount.current}`)
    }, 3000)
  }
  return (
    <div>
      <p>Yout click {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
      <button onClick={handleAlertClick}>Show alert</button>
    </div>
  )
}
```
> useRef 返回的都是相同的引用，参数在第一个传入进去的时候已经赋值给了 current 属性，返回了一个实例回来，后续因为已经有了实例了，所以会直接将原来的实例返回，传入的参数也就不再起作用了。

### `createRef`和`useRef`的区别
```javascript
const App = () => {
  const [renderIndex, setRenderIndex] = React.useState(1)
  const refFromUseRef = React.useRef()
  const refFromCreateRef = createRef()
  if (!refFromUseRef.current) {
    refFromUseRef.current = renderIndex
  }
  if (!refFromCreateRef.current) {
    refFromCreateRef.current = renderIndex
  }
  return (
    <>
      <p>Current render index: {renderIndex}</p>
      <p>
        <b>refFromUseRef</b> value: {refFromUseRef.current}
      </p>
      <p>
        <b>refFromCreateRef</b> value:{refFromCreateRef.current}
      </p>

      <button onClick={() => setRenderIndex(prev => prev + 1)}>
        Cause re-render
      </button>
    </>
  )
}
```
这两者对应 ref 的引用其实是有着本质区别的：createRef 每次渲染都会返回一个新的引用，而 useRef 每次都会返回相同的引用。

## useCallback

把内联回调函数及依赖项数组作为参数传入 useCallback，它将返回该回调函数的 memoized 版本，该回调函数仅在某个依赖项改变时才会更新。当你把回调函数传递给经过优化的并使用引用相等性去避免非必要渲染（例如 shouldComponentUpdate）的子组件时，它将非常有用。

> `useCallback(fn, deps)` 相当于 `useMemo(() => fn, deps)`。

详细参阅[React Hooks 第一期：聊聊 useCallback](https://zhuanlan.zhihu.com/p/56975681)

### 回调ref
```javascript
// callback ref
function MeasureExample() {
  const [height, setHeight] = useState(0)

  const measuredRef = useCallback(node => {
    if (node !== null) {
      setHeight(node.getBoundingClientRect().height)
    }
  }, [])

  return (
    <>
      <h1 ref={measuredRef}>Hello, world</h1>
      <h2>The above header is {Math.round(height)}px tall</h2>
    </>
  );
}

// 抽取逻辑为hook
function useClientRect() {
  const [rect, setRect] = useState(null)
  const ref = useCallback(node => {
    if (node !== null) {
      setRect(node.getBoundingClientRect())
    }
  }, [])
  return [rect, ref]
}

```

## useMemo

把“创建”函数和依赖项数组作为参数传入 `useMemo`，它仅会在某个依赖项改变时才重新计算 memoized 值。`这种优化有助于避免在每次渲染时都进行高开销的计算`。

`React.memo()`是判断一个函数组件的渲染是否重复执行,根据传入的props是否改变为判断条件。

`useMemo()`是定义一段函数逻辑是否重复执行，根据第二个参数`依赖项`来判断

> `useMemo(() => fn, deps)`跟`useCallback(fn, deps)`效果一样

### 基本用法
```javascript
const increase = useMemo(() => {
  if(value > 2) return value + 1
}, [value])
```
### 优化子组件的渲染
```javascript
import React from 'react'
// ExampleA
const ExampleA = ({ text }) => {
  console.log('Example A：', 'render')
  return <div>Example A 组件：{ text }</div>
}
// ExampleB
const ExampleB =  ({ text }) => {
  console.log('Example B：', 'render')
  return <div>Example B 组件：{ text }</div>
}
export default () => {
  const [a, setA] = useState('ExampleA')
  const [b, setB] = useState('ExampleB')
  return (
    <div>
      <ExampleA text={ a } />
      <ExampleB text={ b } />
      <button onClick={ () => setA('修改后的 ExampleA') }>修改传给 ExampleA 的属性</button>
      <button onClick={ () => setB('修改后的 ExampleB') }>修改传给 ExampleB 的属性</button>
    </div>
  )
}
```
> 此时我们点击不同的按钮，控制台都只会打印一条输出，改变 a 或者 b，A 和 B 组件都只有一个会重新渲染。

![](https://react.docschina.org/docs/hooks-reference.html)
![](https://juejin.im/post/6844903955923746830)
![](https://juejin.im/post/6850037283535880205)
![](https://juejin.im/post/6844903814349193229)