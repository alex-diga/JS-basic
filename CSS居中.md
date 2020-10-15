# CSS居中
```html
 <!--子元素是块级元素-->
<div class="parent">
  <div class="son">子元素是块级元素</div>
</div>
<!-#### 子元素是行内元素 -->
<div class="parent">
  <span class="son">子元素是行内元素</span>
</div>
```
## 一、水平居中

### 行内元素
```css
.parent {
  text-align: center;
}
```

### 块级元素

#### 一般居中
```css
.parent {
  margin: 0 auto;
}
```

#### 子元素含 float
```css
.parent {
  width: fit-content;
  margin: 0 auto;
}
.son {
  float: left;
}
```

#### flex弹性盒子
```css
.parent {
  display: flex;
  justify-content: center;
}
```

#### 绝对定位
  
  1. transform
  ```css
  .son {
    position: absolute;
    left: 50%;
    transform: translate(-50%, 0);
  }
  ```

  2. left: 50% 
  ```css
  .son {
    position: absolute;
    width: 宽度;
    left: 50%;
    margin-left: -0.5*宽度
  }
  ```
  3. left/right: 0
  ```css
  .son {
    position: absolute;
    width: 宽度;
    left: 0;
    right: 0;
    margin-left: auto;
  }
  ``` 

## 二、垂直居中

### 行内元素
```css
.parent {
  height: 高度;
}
.son {
  line-height: 高度;
}
```
> 注意：1、子元素 `line-height` 值为父元素 `height` 值。2、单行文本。

### 块级元素

#### 行内块级元素
```css
.parent::after, .son{
  display: inline-block;
  vertical-align: middle;
}
.parent::after{
  content: '';
  height: 100%;
}
```
> 适应 IE7。

#### table
```css
.parent {
  display: table;
}
.son {
  display: table-cell;
  vertical-align: middle;
}
```
##### 优缺点：

- 优点：元素高度可以动态改变, 不需再CSS中定义, 如果父元素没有足够空间时, 该元素内容也不会被截断。

- 缺点：IE6~7, 甚至IE8 beta中无效

#### Flex 弹性盒子
```css
.parent {
  display: flex;
  align-items: center;
}
```
##### 优缺点：
- 优点：1、内容块的宽高任意, 优雅的溢出；2、可用于更复杂高级的布局技术中。

- 缺点：1、IE8/IE9不支持；2、需要浏览器厂商前缀；3、渲染上可能会有一些问题。

#### 绝对定位

1. transform
``` css
.son {
  position: absolute;
  top: 50%;
  transform: translate(0, -50%);
}
```
##### 优缺点：
- 优点：代码少。
- 缺点：1、IE8不支持；2、属性需要追加浏览器厂商前缀；3、可能干扰其他 transform 效果, 某些情形下会出现文本或元素边界渲染模糊的现象。

2. top: 50%
```css
.son {
  position: absolute;
  top: 50%;
  height: 高度;
  margin-top: -0.5高度;
}
```
##### 优缺点：
- 优点：适用于所有浏览器。

- 缺点：父元素空间不够时, 子元素可能不可见(当浏览器窗口缩小时,滚动条不出现时).如果子元素设置了overflow:auto, 则高度不够时, 会出现滚动条。

1. top/bottom: 0
```css
.son {
  position: absolute;
  top: 0;
  bottom: 0;
  margin: auto 0;
}
```
##### 优缺点：
- 优点：简单。
- 缺点：没有足够空间时, 子元素会被截断, 但不会有滚动条。

## 水平垂直居中

1. 仅居中元素定宽高适用
- absolute + 负margin
- absolute + margin auto
- absolute + calc

  
2. 居中元素不定宽高
- absolute + transform
- lineheight
- writing-mode
- table
- css-table
- flex
- grid