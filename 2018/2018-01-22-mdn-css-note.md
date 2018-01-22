MDN CSS Note

# CSS如何工作

### CSS 实际上如何工作？
![image](https://raw.githubusercontent.com/zhaozhouyang/markdown-photots/master/css/QQ20180122-102239%402x.png)
当浏览器显示文档时，它必须将文档的内容与其样式信息结合。它分两个阶段处理文档：  
1. 浏览器将 HTML 和 CSS 转化成 DOM （文档对象模型）。DOM在计算机内存中表示文档。它把文档内容和其样式结合在一起。
2. 浏览器显示 DOM 的内容。

### 如何将你的 CSS 应用到你的 HTML 上
1. 外部样式表（External stylesheet）
2. 内部样式表（Internal stylesheet）
3. 内联样式（Inline styles）

# CSS选择器
### 简单选择器（Simple selectors）
通过元素类型、class 或 id 匹配一个或多个元素。
1. 类型选择器（Type selectors aka element selectors）
2. 类选择器（Class selectors）
3. ID 选择器
4. 通用选择器（Universal selector）
```
* {
  padding: 5px;
  border: 1px solid black;
  background: rgba(255,0,0,0.25)
}
```

### 属性选择器（Attribute selectors）
通过 属性 / 属性值 匹配一个或多个元素。
1. 存在和值（Presence and value）属性选择器
- [attr]：该选择器选择包含 attr 属性的所有元素，不论 attr 的值为何。
- [attr=val]：该选择器仅选择 attr 属性被赋值为 val 的所有元素。
- [attr~=val]：该选择器仅选择 attr 属性的值（以空格间隔出多个值）中有包含 val 值的所有元素，比如位于被空格分隔的多个类（class）中的一个类。
2. 子串值（Substring value）属性选择器
- [attr|=val] : 选择attr属性的值以val（包括val）或val-开头的元素（-用来处理语言编码）。
- [attr^=val] : 选择attr属性的值以val开头（包括val）的元素。
- [attr$=val] : 选择attr属性的值以val结尾（包括val）的元素。
- [attr*=val] : 选择attr属性的值中包含字符串val的元素。

### 伪类（Pseudo-classes）
匹配处于确定状态的一个或多个元素，比如被鼠标指针悬停的元素，或当前被选中或未选中的复选框，或元素是DOM树中一父节点的第一个子节点。
```
a:visited {
  color: blue;
}
```

### 伪元素（Pseudo-elements）
匹配处于相关的确定位置的一个或多个元素，例如每个段落的第一个字，或者某个元素之前生成的内容。
伪元素（Pseudo-element）跟伪类很像，但它们又有不同的地方。它们都是关键字 —— 但这次伪元素前缀是两个冒号 (::) —— 同样是添加到选择器后面达到指定某个元素的某个部分。
- ::after
- ::before
- ::first-letter
- ::first-line
- ::selection
- ::backdrop

```
[href^=http]::after {
  content: '⤴';
}
```

### 组合器（Combinators）
这里不仅仅是选择器本身，还有以有效的方式组合两个或更多的选择器用于非常特定的选择的方法。例如，你可以只选择divs的直系子节点的段落，或者直接跟在headings后面的段落。

Combinators | Select
---|---
A,B |	匹配满足A（和/或）B的任意元素（参见下方 同一规则集上的多个选择器）.
A B |	匹配任意元素，满足条件：B是A的后代结点（B是A的子节点，或者A的子节点的子节点）
A > B |	匹配任意元素，满足条件：B是A的直接子节点
A + B |	匹配任意元素，满足条件：B是A的下一个兄弟节点（AB有相同的父结点，并且B紧跟在A的后面）
A ~ B |	匹配任意元素，满足条件：B是A之后的兄弟节点中的任意一个（AB有相同的父节点，B在A之后，但不一定是紧挨着A）

### 多用选择器（Multiple selectors）
这些也不是单独的选择器；这个思路是将以逗号分隔开的多个选择器放在一个CSS规则下面， 以将一组声明应用于由这些选择器选择的所有元素。

# CSS的值和单位
### 数值
#### 绝对单位
- px
- mm, cm, in: 毫米（Millimeters），厘米（centimeters），英寸（inches）
- pt, pc: 点（Points (1/72 of an inch)）， 十二点活字（ picas (12 points.)）

#### 相对单位
也有相对单位，他们是相对于当前元素的字号（ font-size ）或者视口（viewport ）尺寸。
- em:1em与当前元素的字体大小相同（更具体地说，一个大写字母M的宽度）。CSS样式被应用之前，浏览器给网页设置的默认基础字体大小是16像素，这意味着对一个元素来说1em的计算值默认为16像素。但是要小心—em单位是会继承父元素的字体大小，所以如果在父元素上设置了不同的字体大小，em的像素值就会变得复杂。现在不要过于担心这个问题，我们将在后面的文章和模块中更详细地介绍继承和字体大小设置。em是Web开发中最常用的相对单位。
- ex, ch: 分别是小写x的高度和数字0的宽度。这些并不像em那样被普遍使用或很好地被支持。
- rem: REM（root em）和em以同样的方式工作，但它总是等于默认基础字体大小的尺寸；继承的字体大小将不起作用，这听起来像一个更好的选择比em，虽然在旧版本的IE上不被支持（查看关于跨浏览器支持 Debugging CSS.)
- vw, vh: 分别是视口宽度的1/100和视口高度的1/100，其次，它不像rem那样被广泛支持。

#### 无单位
- 0
- 行高

### 百分比
[MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Values_and_units)
### 颜色
[MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Values_and_units)
### 函数
[MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Values_and_units)

# 层叠和继承（Cascade and inheritance）
## 层叠
CSS 是 Cascading Style Sheets 的缩写，这暗示层叠（cascade）的概念是很重要的。在最基本的层面上，它表明CSS规则的顺序很重要，但它比那更复杂。什么选择器在层叠中胜出取决于三个因素（这些都是按重量级顺序排列的——前面的的一种会否决后一种）：
1. 重要性（Importance）  
2. 专用性（Specificity）  
3. 源代码次序（Source order）

#### 重要性（Importance）  
要注意一个CSS声明的重要性取决于它被指定在什么样式表内——用户可以设置自定义样式表覆盖开发商的样式，例如用户可能有视力障碍，想设置字体大小对所有网页的访问是双倍的正常大小，以便更容易阅读。
相互冲突的声明将按以下顺序适用，后一种将覆盖先前的声明：  
1. 在用户代理样式表的声明 (例如：浏览器在没有其他声明的默认样式).
2. 用户样式表中的普通声明（由用户设置的自定义样式）。
3. 作者样式表中的普通声明（这是我们设置的样式，Web开发人员）。
4. 作者样式表中的重要声明
5. 用户样式表中的重要声明

#### 专用性（Specificity）  
专用性基本上是衡量选择器的具体程度的一种方法——它能匹配多少元素。  
元素选择器具有很低的专用性。类选择器具有更高的专用性，所以将战胜元素选择器。ID选择器有甚至更高的专用性, 所以将战胜类选择器. 战胜ID选择器的唯一方法是使用 !important。  
一个选择器具有的专用性的量是用四种不同的值（或组件）来衡量的，它们可以被认为是千位，百位，十位和个位——在四个列中的四个简单数字：  
1. 千位：如果声明是在style 属性中该列加1分（这样的声明没有选择器，所以它们的专用性总是1000。）否则为0。
2. 百位：在整个选择器中每包含一个ID选择器就在该列中加1分。
3. 十位：在整个选择器中每包含一个类选择器、属性选择器、或者伪类就在该列中加1分。
4. 个位：在整个选择器中每包含一个元素选择器或伪元素就在该列中加1分。

下表显示了几个示例。试着通过这些，并确保你理解他们为什么具有我们给予他们的专用性。  

选择器 | 千位 | 百位 | 十位 | 个位 | 合计值
---|---
h1 | 0 | 0 | 0 | 1 | 0001
#important | 0 | 1 | 0 | 0 | 0100
h1 + p::first-letter | 0 | 0 | 0 | 3 | 0003
li > a[href*="en-US"] > .inline-warning | 0 | 0 | 2 | 2 | 0022
#important div > div > a:hover, 在一个元素的 style 属性里 | 1 | 1 | 1 | 3 | 1113

#### 源代码次序（Source order）
如果多个相互竞争的选择器具有相同的重要性和专用性，那么第三个因素将帮助决定哪一个规则获胜——后面的规则将战胜先前的规则。

## 继承
应用于某个元素的一些属性值将由该元素的子元素继承，而有些则不会。
- 例如，对 font-family 和 color 进行继承是有意义的，因为这使得您可以很容易地设置一个站点范围的基本字体，方法是应用一个字体到 <html> 元素；然后，您可以在需要的地方覆盖单个元素的字体。如果要在每个元素上分别设置基本字体，那就太麻烦了。
- 再如，让 margin，padding，border 和 background-image 不被继承是有意义的。想象一下，如果您将这些属性设置在一个容器元素上，并将它们继承到每个子元素，然后不得不将它们全部放在每个单独的元素上，那么将会出现的样式/布局混乱。
### 控制继承
- inherit： 该值将应用到选定元素的属性值设置为与其父元素一样。
- initial ：该值将应用到选定元素的属性值设置为与浏览器默认样式表中该元素设置的值一样。如果浏览器默认样式表中没有设置值，并且该属性是自然继承的，那么该属性值就被设置为 inherit。
- unset ：该值将属性重置为其自然值，即如果属性是自然继承的，那么它就表现得像 inherit，否则就是表现得像 initial。
> 注意: initial 和 unset 不被IE支持。
