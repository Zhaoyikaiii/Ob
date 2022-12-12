## float

`float` 属性用于指定元素如何定位和规格化排版。

例如，让一个图像浮动于文本之间

```html
<!DOCTYPE html>
<html>
<head>
<style>
img {
  float: right;
}
</style>
</head>
<body>

<h2>Float Right</h2>

<p>In this example, the image will float to the right in the paragraph, and the text in the paragraph will wrap around the image.</p>

<p><img src="pineapple.jpg" alt="Pineapple" style="width:170px;height:170px;margin-left:15px;">
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus imperdiet, nulla et dictum interdum, nisi lorem egestas odio, vitae scelerisque enim ligula venenatis dolor. Maecenas nisl est, ultrices nec congue eget, auctor vitae massa. Fusce luctus vestibulum augue ut aliquet. Mauris ante ligula, facilisis sed ornare eu, lobortis in odio. Praesent convallis urna a lacus interdum ut hendrerit risus congue. Nunc sagittis dictum nisi, sed ullamcorper ipsum dignissim ac. In at libero sed nunc venenatis imperdiet sed ornare turpis. Donec vitae dui eget tellus gravida venenatis. Integer fringilla congue eros non fermentum. Sed dapibus pulvinar nibh tempor porta. Cras ac leo purus. Mauris quis diam velit.</p>

</body>
</html>

```

float 
- left
- right 
- none
- inherit

### 相邻浮动

默认的三个块级元素 div 会在顶部顺序排放，如果我们给 div 增加样式 `float:left` 会让他们从左往右依次排列。

```css
div {  float: left;  
  padding: 15px;}  
  
.div1 {  background: red;}  
  
.div2 {  background: yellow;}  
  
.div3 {  background: green;}
```

## clear 
如果一个元素是浮动的，我们希望其他的元素位于这个元素下方，我们可以消除来自某个方向上的浮动。

```html
<!DOCTYPE html>
<html>
<head>
<style>
.div1 {
  float: left;
  padding: 10px;
  border: 3px solid #73AD21;
}

.div2 {
  padding: 10px;
  border: 3px solid red;
}

.div3 {
  float: left;
  padding: 10px;  
  border: 3px solid #73AD21;
}

.div4 {
  padding: 10px;
  border: 3px solid red;
  clear: left;
}
</style>
</head>
<body>

<h2>Without clear</h2>
<div class="div1">div1</div>
<div class="div2">div2 - Notice that div2 is after div1 in the HTML code. However, since div1 floats to the left, the text in div2 flows around div1.</div>
<br><br>

<h2>With clear</h2>
<div class="div3">div3</div>
<div class="div4">div4 - Here, clear: left; moves div4 down below the floating div3. The value "left" clears elements floated to the left. You can also clear "right" and "both".</div>

</body>
</html>
```

![[Pasted image 20221212153528.png]]

带有 float 的元素会依次排列，如 `without clear`
带有 clear 的元素会消除指定方向上的浮动

![[Pasted image 20221212153937.png]]

默认的一个浮动的元素总是高于容器内部的元素，会溢出容器。

我们可以在容器通过指定 `overfloa:auto` 来消除溢出。

### Float Example 

#### Grid of Boxes/Equal Width Boxes 

```css
* {
	box-sizing:border-box;
}
.box {
	float:left;
	width:33.3%;
	padding:50px;
}
```

#### 等高的盒子

![[Pasted image 20221212160430.png]]

在上面，我们知道了如何将盒子并列排布，并且宽度相同，但是创建一列等高度的盒子并不容易。
最简单的方式就是给盒子设置一个固定的高度。
但是，我们并不能保证给个盒子内部的内容都能装得下。
![[Pasted image 20221212160730.png]]
例如我们在手机端就看到有些内容超出了这个盒子。

#### flexbox

```html
<!DOCTYPE html>
<html>
<head>
<style>
.flex-container {
  display: flex;
  flex-wrap: nowrap;
  background-color: DodgerBlue;
}

.flex-container .box {
  background-color: #f1f1f1;
  width: 50%;
  margin: 10px;
  text-align: center;
  line-height: 75px;
  font-size: 30px;
}
</style>
</head>
<body>

<h1>Flexible Boxes</h1>

<div class="flex-container">
  <div class="box">Box 1 - This is some text to make sure that the content gets really tall. This is some text to make sure that the content gets really tall.</div>
  <div class="box">Box 2 - My height will follow Box 1.</div>
</div>

<p>Try to resize the browser window to see the flexible layout.</p>
<p><strong>Note:</strong> Flexbox is not supported in Internet Explorer 10 or earlier versions.</p>

</body>
</html>
```

![[Pasted image 20221212160906.png]]


