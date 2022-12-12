默认的一个元素的宽度和高度的计算方法是：
- 实际的宽度 = width + padding + border 
- 实际的高度 = height + padding + border 
这意味着，如果你给一个元素指定了一个高度或者是宽度，实际上这个元素的宽高可以比你设置的值要大。因为默认情况下，会将元素的 padding 和 border 添加到元素实际的宽高中。

![[Pasted image 20221212155307.png]]

```html
<!DOCTYPE html>
<html>
<head>
<style> 
.div1 {
  width: 300px;
  height: 100px;
  border: 10px solid blue;
  box-sizing:border-box
}

.div2 {
  width: 300px;
  height: 100px;  
  border: 10px solid red;
}
</style>
</head>
<body>

<h1>With box-sizing</h1>

<div class="div1">Both divs are the same size now!</div>
<br>
<h1>Without box-sizing</h1>

<div class="div2">Hooray!</div>

</body>
</html>

```

带有 border-box 的元素的宽高就为实际指定的宽高。