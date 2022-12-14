不同于 `display:inline`, 最大的不同在于 
- `display:inline` 没有宽度和高度
- `display:inline-block` 允许设置元素的宽度和高度。
- `display:inline-block` 不会在元素之后添加 `line-break`, 所以元素可以紧挨着。
- `display:block` 在元素之后会有 `line-break`

## 使用 `inline-block` 创建导航链接

```css
.nav {
	background-color:yellow;
	list-style-type:none；
	text-align:center;
	margin:0;
	padding:0;
}

.nav li {
	display:inline-block;
	font-size:20px;
	padding:20px;
}
```