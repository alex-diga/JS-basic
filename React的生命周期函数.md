# React 的生命周期函数

> 生命周期函数指在某一个时刻组件会自动调用执行的函数

![生命周期流程](https://user-gold-cdn.xitu.io/2019/6/17/16b64de56b7fdaad?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 1. Initialization

首先是 Initialization,初始化 state 和 props 的数据，在 constructor 函数中会接收 props、初始化 state 和一些方法

```javascript
constructor(props) {
    super(props);
    this.state = {
        inputValue: '',
        list: []
    }
    this.handleBtnClick = this.handleBtnClick.bind(this)
}
```

## 2. Mounting

然后是组件的挂载阶段.

什么是挂载？挂载是组件第一次被放到页面上的时候

#### - componentWillMount()

在组件即将被挂载到页面的时候自动执行

#### - render()

组件挂载阶段

#### - componentDidMount()

组件挂载到页面之后执行

> 注意: componentWillMount 和 componentDidMount 在组件的生命周期执行一次

## 3. Updation

更新包括 props 的更新和 state 的更新。 两者具有一些共同的生命周期钩子。

#### - shouldComponentUpdate()

组件在更新前，会自动被执行，这个钩子函数返回一个布尔值，来决定组件之后是否被更新。

#### - componentWillUpdate()

在组件更新之前，它会自动执行，但是他在 shouldComponentUpdate 之后执行。 如果 shouldComponentUpdate 返回 true，它会执行，否则不会执行。

#### - render()

将新虚拟 DOM 与原来的进行比对(diff)，然后修改真实 DOM。

#### - componentDidUpdate()

组件更新之后立即执行。

#### - componentWillRecieveProps

> props 更新特有

如果一个组件从父组件接受了数据，只要父组件的 render 函数被重新执行了，那么这个 componentWillRecieveProps 才会被执行。这个生命周期函数不是太常用。

## 4. Unmounting

#### - componentWillUnmount()

在组件即将被移除的时候执行。

## 生命周期函数的使用场景

### 1. 避免子组件不必要的重新渲染

当父组件的状态发生改变时，会自动调用 render 函数，从而调用子组件的 render 函数，但是在有些时候父组件这些改变的状态和子组件没有关系，因此子组件没有必要重新调用 render，浪费浏览器性能

```javascript
//接受两个参数，分别是新传进来的props和state
shouldComponentUpdate(nextProps, nextState) {
    //进行相关变量的比对，如果不一样则更新
    if(nextProps.xxx !== this.props.xxx){
        return true;
    }
    return false;
    //也可以简化为:
    //return nextProps.xxx !== this.props.xxx ? true : false;
}
```

### 2. 发送 Ajax 请求

最好在 componentDidMount 里面发送 Ajax 请求， 一般而言 Ajax 数据只需要获取一次即可。

- 如果放在 componentWillMount 里面，以后如果用到了 ReactNative 或者服务端同构，会和其他的技术产生冲突;
- 如果放在 render 函数里面，事实上 render 在组件的生命周期中是经常被执行的，那么这个请求也会发送非常多次，也并不合理。
