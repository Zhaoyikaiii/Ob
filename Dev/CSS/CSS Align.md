## 元素水平居中

- 居中一个块级元素例如 ('div'), 使用 `margin:auto`。如果给这个块级元素设置一个宽度会阻止这个元素伸长。如果没有宽度，块级元素会自己居中，并且左右宽度（margin）相同。
```css
.center {
	margin:auto;
	width:60%;
	border:3px solid #73AD21;
	padding:10px
}
```

- 居中文本
	- `text-align:center`
- 居中图片
```css
img {
	display:block;
	margin-left:auto;
	margin-right:auto;
	width:40%;
}
```

## 左对齐或者右对齐

### 使用 position

一种使元素左对齐或者右对齐的方式是使用绝对定位。`position:absolute`
```css
.right {
	position:absolute;
	right:0px;
	width:300px;
	border:3px solid #73AD21;
	padding:10px;
}
```

### 使用 float

```css
.right {
	float:right;
	width:300px;
	border:3px solid #73AD21;
}
```

### 清除浮动

如果一个浮动元素的高度比包含这个元素的高度要大，会出现这样的效果:
![[Pasted image 20221214141933.png]]


```css
.clearfix::after {  
  content: "";  
  clear: both;  
  display: table;
}
```

## 垂直居中

### 使用 padding 

```css
.center {
	padding: 70px 0;
	border: 3px solid green
}
```

## 垂直水平居中

1. padding + text-align
```css
.center {
	padding: 70px 0;
	border:3px solid green;
	text-align:center;
}
```

2. line-height + line-height + vertical-align

```css
.center {
	line-height:200px;
	height:200px;
	border:3px solid green;
	text-align:center;
}

.center p {
	line-height:1.5;
	display:inline-block;
	vertical-align:middle
}
```

3. Center Vertically - Using position & transform

```css
.center {
	height:200px;
	position:relative;
	border:3px solid green
}

.center p {
	marin: 0;
	position:absolute;
	top: 50%;
	left: 50%;
	left: 50%;
	transform: translate(-50%,-50%)
}
```

4. Using Flexbox

```css
.center {
	display: flex;
	justify-content: center;
	align-items:center;
	height:200px;
	border:3px solid green;
}
```



