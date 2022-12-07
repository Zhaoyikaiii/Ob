不同于 border, Padding 设置的是元素内容与元素边框之间的距离。

- `padding-top`
- `padding-right`
- `padding-bottom`
- `padding-left`

### Padding 和 Width

- Width 设置的是元素的内容的宽度, 如果一个元素已经指定了宽度 (`width`) 再额外的去指定它的内边距 (`padding`) 会在 `wdith` 的基础上变更。

```css
div {  width: 300px;  
  padding: 25px;
}
```

- 没有指定额外的样式这个 div 的最终的宽度为 `350px`

- 为了确保这个元素的宽度为设置的 `300px` 我们需要加上一个样式 `box-sizing:border-box`
```css
div {  width: 300px;  
  padding: 25px;  
  box-sizing: border-box;
  }
```