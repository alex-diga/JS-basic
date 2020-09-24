# JS 数据类型转换

- 转换成数字
- 转换成布尔值
- 转换成字符串

![具体转换规则](https://user-gold-cdn.xitu.io/2019/10/20/16de9512eaf1158a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

> Boolean 转字符串"这行结果指的是 true 转字符串的例子

## [] == ![]

1. == 中，左右两边都需要转换为数字然后进行比较

2. `![]` 首先是转换为布尔值，`[]`作为一个引用类型转换为布尔值为 true。
   因此`![]`为 false，进而在转换成数字，变为 0

3. 0 == 0 ， 结果为 true

## == 和 ===有什么区别

> ===叫做严格相等，是指：左右两边不仅值要相等，类型也要相等，例如'1'===1 的结果是 false，因为一边是 string，另一边是 number。

> ==不像===那样严格，对于一般情况，只要值相等，就返回 true。

### 但==还涉及一些类型转换，它的转换规则如下：

- 两边的类型是否相同，相同的话就比较值的大小，例如 1==2，返回 false
- 判断的是否是 null 和 undefined，是的话就返回 true
- 判断的类型是否是 String 和 Number，是的话，把 String 类型转换成 Number，再进行比较
- 判断其中一方是否是 Boolean，是的话就把 Boolean 转换成 Number，再进行比较
- 如果其中一方为 Object，且另一方为 String、Number 或者 Symbol，会将 Object 转换成字符串，再进行比较

## 对象转原始类型是根据什么流程运行的

> 对象转原始类型，会调用内置的`ToPrimitive`函数，对于该函数而言，其逻辑如下：

1. 如果 Symbol.toPrimitive()方法，优先调用再返回
2. 调用 valueOf()，如果转换为原始类型，则返回
3. 调用 toString()，如果转换为原始类型，则返回
4. 如果都没有返回原始类型，会报错

```javascript
var obj = {
	value: 3,
	valueOf() {
		return 4
	},
	toString() {
		return '5'
	},
	[Symbol.toPrimitive]() {
		return 6
	},
}
console.log(obj + 1) // 输出7
```

> ### 如何让 if(a == 1 && a == 2)条件成立

```javascript
var a = {
	value: 0,
	valueOf: function () {
		this.value++
		return this.value
	},
}
console.log(a == 1 && a == 2) //true
```
