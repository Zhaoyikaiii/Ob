`Overflow` 属性控制的是元素如果太大如何融入到一个区域内部。

- visible (默认): overflow 的部分不会被切掉
- hidden:overflow 部分会被切掉，并且不可见
- scroll: overflow 部分会被切掉，并且剩余部分会有滚动条
- auto: 类似于 scroll，不同的是滚动条只会在必要的时候出现。


### Overflow: visible 
![[Pasted image 20221212152035.png]]

多余的部分会展示。

### Overflow: hidden 

![[Pasted image 20221212152156.png]]

多余部分会被剪掉。

### Overflow: scroll 

![[Pasted image 20221212152244.png]]

### Overflow:auto

![[Pasted image 20221212152317.png]]

不同于 scroll，只会在需要的时候增加滚动条

### overflow-x and overflow-y

这两个属性指定的是，元素在水平和竖直方向上重叠时的行为
```css
div {
	overflow-x: hidden;
	overflow-y: scroll;
}
```

会导致这个元素在竖直方向出现滚动条，在水平方向内容被剪掉。

