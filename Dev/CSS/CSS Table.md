### Table Borders 

- border 属性可以修改边框的样式
```css
table,th,td {
 border: 1px solid;
}
```

![[Pasted image 20221212094925.png]]

我们可以看到这个表格的边框有两条，如果希望只留下一条边框可以设置
```css
table {
	border-collapse :collapse
}
```

如果只希望表格有边框例如这样:
![[Pasted image 20221212095056.png]]

可以只给 table 增加 border 样式属性
```css
table {
	border: 1px solid
}
```

### CSS Table Alignment

#### 水平方向对齐方式

`text-align` 设置的是文字在内容 (th, td) 中的对齐方式 (center, left, right), 默认是靠左边对齐的。

#### 垂直方向对齐方式

`vertical-align` 设置的是文字在内容（th, td）垂直方向的对齐方式 (top, bottom, middle (默认))，

#### Table Padding

设置表格再内容和边框之间的空间可以通过 `padding` 指定。
```css
th,td {
	padding: 15px,
	text-align:left
}
```

#### 水平方向的分隔条

```css
th,td {
	border-bottom:1px solid #ddd
}
```

#### 鼠标浮动在表格上的样式

```css
tr:hover {
	background-color:coral;
}
```

#### 条纹表格

```css
tr：nth-child(even) {
	background-color:#f2f2f2
}
```

#### 表头样式

```css
th {
	background-color:#04AA6D
	color:white
}
```


