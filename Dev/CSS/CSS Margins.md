Margin 用于给元素外边框创建空间。

- `margin-top`
- `margin-right`
- `margin-bottom`
- `margin-left`

值可以为:
- `auto` 由浏览器计算 margin 的值
- `length` 指定一个外边框的大小
- `%` 指定一个相对于元素宽度的外边框大小
- `inherit` 继承父元素的外边框大小

- auto 
	- 你可以指定给一个元素的 margin 为 auto 将这个元素居中

### Margin Collapse 

```css
h1 {  margin: 0 0 50px 0;}  
  
h2 {  margin: 20px 0 0 0;}
```

分别给 h1 和 h2 的下方和上方设置了外边距为 50px 和 20px。实际上在 h1 和 h2 之间的高度只有 50px
![[Pasted image 20221207094301.png]]

需要注意的是这种情况只会发生在上下方向不会发生在水平方向。

