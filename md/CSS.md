# CSS 部分

## px em rem 的区别

- `px` 是像素，绝对单位
- `em` 是相对长度单位，相对于当前对象内文本的字体尺寸。如果未对当前行内文本的字体尺寸进行设置，则相对于浏览器的默认字体尺寸。它会继承父级元素的字体大小，因此并不是一个固定的值。
- `rem` 是CSS3新增的一个相对单位（root em，根em），使用rem为元素设定字体大小时，仍然是相对大小，但相对的只是HTML根元素。
- 总结：`rem` 最优秀，通过它既可以做到只修改根元素字体大小就可以成比例的调整所有字体大小，又可以避免字体大小逐层复合的连锁反应。

## 水平居中

- children 宽度确定

```css
children {
    width: 200px;
    margin: 0 auto;
}
```

- flex 方法1：

```css
parent {
    display: flex;
    justity-content: center;
}
```

- flex 方法2：

```css
parent {
    position: relative
}

children {
    position: absolute
    left: 50%;
    trnasform: translateX(-50%);
}
```

## 垂直居中

- 单行文本居中，`line-height` 与 `div` 高度保持一样就行了。

```css
div {
    line-height: 30px;
    height: 30px;
}
```
- flex 方法

```css
parent {
    position: relative
}

children {
    position: absolute
    top: 50%;
    trnasform: translateY(-50%);
}
```
## position

`position` 用于网页元素的定位，可设置 `static`/`relative`/`absolute`/`fixed` 这些值，其中 `static` 是默认值，不用介绍。

### 定位上下文

`relative` 元素的定位永远是相对于元素自身位置的，和其他元素没关系，也不会影响其他元素。

![relative_position.png](https://raw.githubusercontent.com/wangwenyue/Pic-bed/master/relative_position.png)

`fixed` 元素的定位是相对于 `window` （或者 `iframe`）边界的，和其他元素没有关系。但是它具有破坏性，会导致其他元素位置的变化。

![fixed_position.png](https://raw.githubusercontent.com/wangwenyue/Pic-bed/master/fixed_position.png)

`absolute` 的定位相对于前两者要复杂许多。如果为 `absolute` 设置了 `top`、`left`，浏览器会根据什么去确定它的纵向和横向的偏移量呢？答案是浏览器会递归查找该元素的所有父元素，如果找到一个设置了 `position`: `relative` / `absolute` / `fixed` 的元素，就以该元素为基准定位，如果没找到，就以浏览器边界定位。如下两个图所示：

![absolute_position_1.png](https://raw.githubusercontent.com/wangwenyue/Pic-bed/master/absolute_position_1.png)

![absolute_position_2.png](https://raw.githubusercontent.com/wangwenyue/Pic-bed/master/absolute_position_2.png)

## 布局

- 两栏布局

```html
<div style="float:left; width:200px;"></div>
<div style="margin-left: 200px;"></div>
```
- 三栏布局

```html
<div style="float:left; width:200px;"></div>
<div style="float:right; width:200px;"></div>
<div style="margin-left: 200px; margin-right: 200px;"></div>
```
- flex 布局

[参考链接](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

## float 相关

- `float` 定义：指定一个元素应沿其容器的左侧或右侧放置，允许文本和内联元素环绕它。该元素从网页的正常流动中移除，但仍然保持部分的流动性（与 `absolute` 相反）。

### 清除浮动方法：

[详见](https://github.com/wangwenyue/CSS-me/blob/master/example/%E6%B8%85%E9%99%A4%E6%B5%AE%E5%8A%A8.html)

```css
.clearfix::after {
    content: '';
    display: block;
    clear: both;
}
```

## margin 合并

- 相邻的两个块级元素，间距是两者 `margin` 的最大值。
- 父子两个块级元素： 若第一个子元素的 `top` 上没有 `border` 或者 `padding` ,上方没有 `inlineBlock` 清除浮动的话，那么父子两元素的 `margin` 将合并，取两者的最大值。
