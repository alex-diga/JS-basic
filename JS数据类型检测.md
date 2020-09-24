# JS 数据类型检测

- 对于原始类型来说，除了 null 都可以调用 typeof 显示正确的类型。

```javascript
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof Symbol() // 'symbol'
```

- 对于引用数据类型，除了函数之外，都会显示"object"

```javascript
typeof [] // 'object'
typeof {} // 'object'
typeof console.log // 'function'
```

- 因此采用 typeof 判断对象数据类型是不合适的，采用 instanceof 会更好，instanceof 的原理是基于原型链的查询，只要处于原型链中，判断永远为 true

```javascript
const Person = function () {}
const p1 = new Person()
p1 instanceof Person // true

var str1 = 'hello world'
str1 instanceof String // false

var str2 = new String('hello world')
str2 instanceof String // true
```

- 手动实现 instanceof 功能

**核心: 原型链的向上查找。**

```javascript
function myInstanceof(left, right) {
	//基本数据类型直接返回false
	if (typeof left !== 'object' || left === null) return false
	//getProtypeOf是Object对象自带的一个方法，能够拿到参数的原型对象
	let proto = Object.getPrototypeOf(left)
	while (true) {
		//查找到尽头，还没找到
		if (proto == null) return false
		//找到相同的原型对象
		if (proto == right.prototype) return true
		proto = Object.getPrototypeOf(proto)
	}
}

// 测试
console.log(myInstanceof('111', String)) //false
console.log(myInstanceof(new String('111'), String)) //true
```

## Object.is 和===的区别

Object 在严格等于的基础上修复了一些特殊情况下的失误，具体来说就是+0 和-0，NaN 和 NaN。 源码如下

```javascript
function is(x, y) {
  if (x === y) {
    //运行到1/x === 1/y的时候x和y都为0，但是1/+0 = +Infinity， 1/-0 = -Infinity, 是不一样的
    return x !== 0 || y !== 0 || 1 / x === 1 / y;
  } else {
    //NaN===NaN是false,这是不对的，我们在这里做一个拦截，x !== x，那么一定是 NaN, y 同理
    //两个都是NaN的时候返回true
    return x !== x && y !== y;
  }
```
