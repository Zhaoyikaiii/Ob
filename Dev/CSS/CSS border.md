## border

### border-style

- `dotted` 
- `dashed`
- `solid`
- `double`
- `groove`
- `ridge`
- `inset`
- `outset`
- `none`
- `hidden`
![[Pasted image 20221207092604.png]]

### border-width

The `border-width` property specifies the width of the four borders.
The `border-width` property can have from one to four values (for the top border, right border, bottom border, and the left border):

### border-color

The `border-color` property is used to set the color of the four 
borders.

The color can be set by:

-   name - specify a color name, like "red"
-   HEX - specify a HEX value, like "#ff0000"
-   RGB - specify a RGB value, like "rgb(255,0,0)"
-   HSL - specify a HSL value, like "hsl(0, 100%, 50%)"
-   transparent

The `border-color` property can have from one to four values (for the top border, right border, bottom border, and the left border).


### border-sides

```css
/* Four values */  
p {  border-style: dotted solid double dashed;}  
  
/* Three values */  
p {  border-style: dotted solid double;}  
  
/* Two values */  
p {  border-style: dotted solid;}  
  
/* One value */  
p {  border-style: dotted;}
```

会依次按照从上开始顺时针的方向设置边框的样式，如果缺少某一个方向的样式会采用对应方向的样式。

### border-radius

这个样式用于给边框加上圆形的边框。
