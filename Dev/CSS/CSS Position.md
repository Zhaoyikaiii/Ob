
## CSS Position

`position` 指定的是元素的定位类型
- `static`
- `relative`
- `fixed`
- `absolute`
- `sticky`

### position:static

CSS 默认的定位方式为 static, 采取 static 定位的元素不会被属性 `top`, `bottom`, `left`, `right` 属性影响。

### position: relative 

相对定位是相对于其正常的位置，设置 `top`, `right`, `bottom`, `left` 属性会使得元素调整偏移正常的位置。并且其他元素也不会受到影响。

### position: fixed 

fixed 定位方式是相对于视口（viewport）。意味着，如果一个元素被设定为了固定定位方式，会使得这个元素被固定在屏幕上，不管屏幕是否进行了滚动。并且，`top`, `right`, `bottom`, `left` 也可以指定这个元素相对于屏幕左上角的位置。
```css
div.fixed {
	position:fixed,
	bottom:0,
	right:0,
	width:300px,
	border:3px solid green
}
```

这个 css 语句会使得在屏幕的右下角出现一个 fixed 定位定位的宽度为 300px 的边框为绿色的元素。


### position: absolute 

绝对定位不同于固定定位是依赖于视口，绝对定位的定位方式是依赖于最近的一个父元素。
如果一个绝对定位的元素不存在一个父元素，这个元素的定位会依赖于 document body。
需要注意的是，绝对定位的元素会使得元素离开正常的文档流会使得与一些元素重叠。
```css
div.relative {
	position:relative;
	width:400px;
	height:200px;
	border:3px solid #73AD21;
}

div.absolute {
	position:absolute;
	top:80px;
	right:0;
	width:200px;
	height:100px;
	border:3px solid #73AD21;
}
```

最终的效果是这样的:
![[Pasted image 20221212140135.png]]

### position: sticky 

如果一个元素采取了黏性定位，这个元素的定位会取决于用户的滚动条的位置。
一个黏性定位的元素的定位方式会在 `relative` 和 `fixed` 之间进行切换。这取决于滚动条的位置。如果滚动条有偏移，这个黏性的元素就像是 `fixed` 固定在父元素上。如果没有偏移，元素就会相对定位于父元素上。

需要注意的是：
- IE 并不支持黏性定位。
- Safari 需要一个 `-webkit-prefix` 的前缀类似于：`position:-webkit-sticky`
- 并且需要指定 `top`, `right`, `bottom`, `left` 中的一个来保证黏性定位生效。例如 ![[Pasted image 20221212141042.png]]
滚动条是在竖直方向的，我们需要指定 `top:0` 

### example

例子，如何将文本固定于一张图片外
![[Pasted image 20221212141206.png]]

```css
.container {
	relative
}

.topLeft {
	position: absolute;
	top:8px;
	left:16px;
	font-size:18px
}

img {
	width:100%;
	height:auto;
	opacity:0.3
}
```


