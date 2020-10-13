<!--
 * @Author: your name
 * @Date: 2020-09-28 18:37:08
 * @LastEditTime: 2020-10-13 09:56:25
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \WisdomAuditWebc:\Users\Administrator\Desktop\JS基础\React Hooks.md
-->

# React Hooks

React Hooks 的意思是，组件尽量写成纯函数，如果需要外部功能和副作用，就用钩子把外部代码"钩"进来。
React Hooks 就是那些钩子。

## - Basic Hooks

### - useStates

**状态钩子**

`useState()`用于为函数组件引入状态（state）。纯函数不能有状态，所以把状态放在钩子里面。

### - useEffect

**副作用钩子**

`useEffect()`用来引入具有副作用的操作，最常见的就是向服务器请求数据。以前，放在`componentDidMount`里面的代码，现在可以放在`useEffect()`。

### - useContext

**共享状态钩子**

### - useReducer

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
