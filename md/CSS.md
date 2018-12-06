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
## float 相关

- `float` 定义：指定一个元素应沿其容器的左侧或右侧放置，允许文本和内联元素环绕它。该元素从网页的正常流动中移除，但仍然保持部分的流动性（与 `absolute` 相反）。

### 清除浮动方法：
- 增加一个子元素，设置 `clear: both;`
- 给最后一个元素设置伪元素 `:after{ content: '', clear: both }`
- 设置父元素 `overflow: hidden auto;`

## margin 合并

- 相邻的两个块级元素，间距是两者 `margin` 的最大值。
- 父子两个块级元素： 若第一个子元素的 `top` 上没有 `border` 或者 `padding` ,上方没有 `inlineBlock` 清除浮动的话，那么父子两元素的 `margin` 将合并，取两者的最大值。
