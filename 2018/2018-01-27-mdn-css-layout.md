# 简介
## 正常的布局流
- position 属性 — 正常布局流中，默认为 static ，使用其它值会引起元素不同的布局方式，例如将元素固定到浏览器视口的左上角。
- 浮动——应用 float 值，诸如 left 能够让块级元素互相并排成一行，而不是一个堆叠在另一个上面。
- display 属性——标准值 block, inline or inline-block 会改变元素在正常布局流中的行为方式(见 Types of CSS boxes )，而一些不常见或特殊的值允许我们使用完全不同的方式进行布局，使用工具比如Flexbox。

## 浮动
- left — 将元素浮动到左侧。
- right — 将元素浮动到右侧。
- none — 默认值, 不浮动。
- inherit — 继承父元素的浮动属性。

### 清除浮动
clear 属性。当你把这个应用到一个元素上时，它主要意味着"此处停止浮动"——这个元素和源码中后面的元素将不浮动，除非您稍后将一个新的float声明应用到此后的另一个元素。
```
footer {
  clear: both;
}
```

### 浮动问题
- 由于外边距和边界引入的额外宽度，一行容纳不下三列了，因此第三列下降到另外两列之下。  
box-sizing: border-box, 通过更改盒模型来拯救我们，盒子的宽度取值为 content + padding + border，而不仅是之前的content——所以当增加内边距或边界的宽度时，不会使盒子更宽——而是会使内容调整得更窄。


- 我们来试着清除页脚浮动的同时给出一些顶部外边距（ margin-top ）
  ````
  footer {
    clear: both;
    margin-top: 4%;
  }
  ```
  然而，这不起作用 ——浮动的元素存在于正常的文档布局流之外，在某些方面的行为相当奇怪：
  - 首先，他们在父元素中所占的面积的有效高度为0 ——尝试在浏览器中加载 1_three-column-layout.html 并用开发工具查看 body 的高度，你将会看到我们的意思是什么——所报告的正文高度只有 h1 的高度 。这个可以通过很多方式解决，但是我们所依赖的是在父容器的底部清除浮动，如我们在我们的当前示例所做的那样。 如果检查当前示例中正文的高度，您应该看它的高度是行为本身。
  - 其次，非浮动元素的外边距不能用于它们和浮动元素之间来创建空间——这是我们在这里眼前的问题，我们将在下面实施修复。
  - 解决：在上面加一个clearFix (clear: both), 在这个的下面再继续margin-top就ok了

## 定位（Position）
- 静态定位(Static positioning)是每个元素默认的属性——它表示“将元素放在文档布局流的默认位置——没有什么特殊的地方”。
- 相对定位(Relative positioning)允许我们相对元素在正常的文档流中的位置移动它——包括将两个元素叠放在页面上。这对于微调和精准设计(design pinpointing)非常有用。
- 绝对定位(Absolute positioning)将元素完全从页面的正常布局流中移出，类似将它单独放在一个图层中. 我们可以将元素相对于页面的 <html> 元素边缘固定，或者相对于离元素最近的被定位的祖先元素(ancestor element)。绝对定位在创建复杂布局效果时非常有用，例如通过标签显示和隐藏的内容面板或者通过按钮控制滑动到屏幕中的信息面板.
- 固定定位(Fixed positioning)与绝对定位非常类似，除了它是将一个元素相对浏览器视口固定，而不是相对另外一个元素。 在创建类似页面滚动总是处于页面上方的导航菜单时非常有用。

# 弹性盒子 Flexbox
[阮一峰Flex语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)  
[阮一峰Flex实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

# 网格布局 Grids
[AntD栅格](https://ant.design/components/grid-cn/)
