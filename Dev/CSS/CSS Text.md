## Text Color

用于设置字体的颜色


## Text Alignment

- `text-align`
- `text-align-last`
- `direction`
- `unicode-bidi`
- `vertical-align`

### Text-align 
用于设置元素在水平方向的对齐方式
可以为：
- left (默认如果文档方向为从左到右)
- right
- center
- justify

如果被设置为 justify 每个文字之间的间距会相同

### Text-align-last 
用于设置文本最后一行的对齐方式

### Text-Direction

`direction` 和 `unicode-bidi ` 被用来设置文本的方向


### Vertical Alignment 

被用来设置元素在竖直方向的对齐方式

##  Text Decoration

- `text-decoration-line`
	- `overline`
	- `line-through`
	- `underline`
	- `overline underline`
- `text-decoration-color`
- `text-decoration-style`
	- `text-decoration-thickness` 指定厚度
- `text-decoration`


## Text Transformation

text Transformation 被用来设置文本的大小写 `uppercase` / `lowercase` / `capitalize`

## Text Spacing

- `text-indent` 用来指定第一个字母的缩进
- `letter-spacing` 指定字母之间的空隙
- `line-height` 指定行之间的空隙
- `word-spacing` 指定单词之间的空隙
- `white-space`
	- `normal`
	- `nowrap`
	- `pre`
	- `pre-line`
	- `pre-wrap`

## Text Shadow

```css
h1 {  text-shadow: 2px 2px;}
```

第一个参数指定的是水平方向的阴影
第二个参数指定的是竖直方向的阴影
第三个参数指定阴影的颜色
也可以从上方开始，按照顺时针的方向指定不同方向文字的阴影。
```css
h1 {
  color: white;
  text-shadow: 1px 1px 2px black, 0 0 25px blue, 0 0 5px darkblue;
}
```

![[Pasted image 20221207165604.png]]

