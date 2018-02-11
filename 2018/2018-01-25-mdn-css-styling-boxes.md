# 盒模型
## 概要
![image](https://raw.githubusercontent.com/zhaozhouyang/markdown-photots/master/css/css-box-model.png)
一些值得记住的点：

> - 如果盒子的高度被设置为百分比长度，那么盒子高度不会遵循这个设置了的百分比长度，而是总会采用盒子内容的高度，除非给它设置了一个绝对高度（例如，像素或者 em）。这比把页面上每个盒子的高度默认设置为视口高度的 100% 更方便。
> - 边界（border）也会忽略百分比宽度设置。
> - 外边距（margin）有一个特殊的行为，称为 外边距塌陷： 当两个盒子挨在一起时，二者之间的距离为两个挨着的外边距中最大的那个值，而不是二者的和。

#### 背景剪裁
盒背景由颜色和图像组成，堆叠在彼此之间(background-color，background-image)。它们被应用于一个盒并在该盒子下画。默认情况下，背景延伸到边界的外边缘。这通常很好，但在某些情况下可能会令人烦恼(如果您有一个平铺的背景图片，您只想扩展到内容的边缘呢?)这种行为可以通过设置background-clip属性。
#### 轮廓(Outline)
盒子的 outline 看起来像边界，但是它不是盒模型的一部分。它表现得像边界，但是是画在盒子之上，不会修改盒子的大小（具体来说，ouline 是画在边界框之外，外边距区域之内）。

## 背景样式
#### 背景颜色
你会经常使用 background-color属性：
- 首先，大多数元素的默认背景颜色不是white (白色，这可能如你所料) 而是transparent（透明）——因此，如果您将一个元素的背景颜色设置为一些有趣的东西，但是希望它的子元素是白色的，那么您就必须明确地设置它。
- 此外，设置背景颜色作为后备也是很重要的。背景颜色在各处都得到了支持，而背景梯度等更奇异的特性只在较新的浏览器中得到支持，加上背景图像可能由于某种原因无法加载。因此，设置基本的背景颜色和指定这些特性是一个好主意，因此无论如何，元素的内容都是可读的。
#### 背景图像
> 需要记住的一件重要的事情是，由于背景图像是使用CSS设置的，并且出现在内容的背景中，它们将会在屏幕阅读器之类的辅助技术中不可见。它们不是内容图片，只是为了装饰，如果你想在你的页面上包含一个图片，这是内容的一部分，那么你应该用<img>元素来做。

#### 背景重复
background-repeat
- no-repeat: 图像将不会重复:它只会显示一次。
- repeat-x: 图像将在整个背景中水平地重复。
- repeat-y: 图像会在背景下垂直地重复。

#### 背景位置
background-position  
该属性可以接受许多不同的值类型，最常用的是：
- 像px这样的绝对值——比如 background-position: 200px 25px.
- 像rems 这样的相对值——比如 background-position: 20rem 2.5rem.
- 百分比 ——比如 background-position: 90% 25%.
- 关键字——比如 background-position: right center. 这两个值是直观的，可以分别取值比如 left，center， right和 top，center， bottom。

可以混合并匹配这些值，比如 background-position: 99% center。还要注意，如果您只指定一个值，那么该值将被假定为水平值，而垂直值将默认为center。

#### 背景图像：渐变
目前有两种类型的渐变——线性渐变(从一条直线到另一条直线)和径向渐变(从一个点发散出来)。  
第二种方法比较复杂，也不太常用。  
线性渐变是通过linear-gradient()函数传入的，它是一个background-image属性的值。函数至少需要用逗号分隔的三个参数——背景中渐变的方向，开始的颜色和结尾的颜色。例如：
```
background-image: linear-gradient(to bottom, orange, yellow);
```
这个渐变将从上到下，从顶部的橙色开始，然后平稳过渡到底部的黄色。可以使用关键字来指定方向 （to bottom，to right， to bottom right等）， 或角度值 (0deg相当于 to top，90deg 相当于 to right，直到 360deg，它再次相当于 to top ）。  
也可以在颜色定义的渐变中指定其他的点——这些被称为颜色站点(color stops)，浏览器会计算出每一组颜色站点之间的颜色渐变。比如：  
```
background-image: linear-gradient(to bottom, yellow, orange 40%, yellow);
```
这个渐变会从上到下运行，从黄色开始，向下渐变到橙色的40%，然后再回到黄色，达到100%。您可以指定任意多个颜色站点，您也可以使用其他的单位来指定这些颜色站点的位置，例如rem，px等。  
注意，您还可以使用repeating-linear-gradient()函数来设置一个重复的线性渐变。它的工作方式完全相同，只不过你设置的模式会不断重复，直到背景的边沿。例如：
```
background-image: repeating-linear-gradient(to right, yellow, orange 25px, yellow 50px);
```

#### 背景附着
[DEMO](http://mdn.github.io/learning-area/css/styling-boxes/backgrounds/background-attachment.html)  
指定当内容滚动时它们是如何滚动的。这是使用background-attachment属性来控制的，该属性可以使用以下值：
- scroll: 这将把背景修改为页面视图，因此它将在页面滚动时滚动。注意，我们说的是视图，而不是元素——如果你滚动实际的背景设置的元素，而不是页面，背景不会滚动。
- fixed: 这可以在页面的位置上固定背景，所以当页面滚动时，它不会滚动，不管你是滚动页面还是背景设置的元素，它都会保持在相同的位置。
- local:这个值后来被添加了(它只在Internet Explorer 9+中得到支持，而其他的则在IE4+中得到支持)，因为scroll值相当混乱，并且在许多情况下并没有真正做您想要的事情。  local 值将背景设置为它所设置的元素的背景，因此当您滚动元素时，背景会随之滚动。

#### 多个背景
[MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Styling_boxes/%E8%83%8C%E6%99%AF)

#### 背景尺寸
[MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Styling_boxes/%E8%83%8C%E6%99%AF)

## 边框
#### 边框半径
在不同的角落放置不同大小的边界半径, 您可以指定两个，三个或四个值, 就像您使用 padding and margin一样:
```
/* 1st value is top left and bottom right corners,
   2nd value is top right and bottom left  */
border-radius: 20px 10px;
/* 1st value is top left corner, 2nd value is top right
   and bottom left, 3rd value is bottom right  */
border-radius: 20px 10px 50px;
/* top left, top right, bottom right, bottom left */
border-radius: 20px 10px 50px 0;
```
作为最后一点，您还可以创建椭圆形角（x半径与y半径不同）。两个不同的半径用正斜杠（/）分隔，您可以将其与值的任意组合组合。例如:
```
border-radius: 10px / 20px;
border-radius: 10px 30px / 20px 40px;
```
> 注意 您可以使用任何长度单位来指定边界半径，例如 ： px, rems, %.

#### 边界图像
border-image图像使实现复杂的图形边界变得容易得多，即使必须在现代浏览器中才能实现(Internet Explorer 11+支持它，以及其他现代浏览器)。让我们来看看它是如何工作的。  
当图像用于边界图像时，浏览器将图像分割为9个部分，如下一个图像所示：
![image](https://raw.githubusercontent.com/zhaozhouyang/markdown-photots/master/css/css-border-image.png)
这些角的图像会被插入到你的边界的角落里，而顶部、右边、底部和左边的部分将被用来填充你的边界的相应边(通过拉伸或重复)。我们需要告诉浏览器让这些片的大小正确  
——例如，这个图像是160px，还有一个4 x 4的网格，所以每个片都需要40px。
> 只需要告诉角落的大小，四个角落的黑色圆点每个是40px*40px

## 表格
```
tbody tr:nth-child(odd) {
  background-color: #ff33cc;
}

tbody tr:nth-child(even) {
  background-color: #e495e4;
}
```

## 盒子阴影
- 还可以在单个box-shadow声明中指定多个框阴影，用逗号分隔
- 与text-shadow不同，box-shadow有一个inset关键字可用——把它放在一个影子声明的开始，使它变成一个内部阴影，而不是一个外部阴影。

### Filters（过滤器）
基本上，过滤器可以应用在任何元素上，块元素（block）或者行内元素（inline）——你只需要使用filter属性，并且给他一个特定的过滤函数的值。有些可用的过滤器选项和其他CSS特性做的事情十分相似，例如drop-shadow()的工作方式以及产生的效果和 box-shadow 或text-shadow十分相似。然而过滤器真正出色的地方在于，它们作用于盒（box）内内容（content）的确切形状，而不仅仅将盒子本身作为一个大的块，这看起来会更棒，即使他们可能不会总是变成你想要的模样。  
其他需要注意的事:
- 过滤器很新——它们可以被大多数的现代的浏览器支持，包括Microsoft Edge，但它们一点也不能被IE浏览器支持。因此如果你在你的设计中使用过滤器，你需要确保你的内容即使没有过滤器也是可用的。
- 你会看到我们在filter属性中通过-webkit-前缀包含了一个版本信息，这被称为一个 Vendor Prefix，有时会被浏览器使用，以在一个新特性完整实现之前，当它与无前缀版本没有冲突的时候支持并实验这个特性。Vendor prefixes永远都不被指望着被web开发人员使用，但是它们有时候确实会被在产品页面中使用，即当实验性的特性确实被需要时。在这个实例中，Chrome/Safari/Opera 目前要求这些属性的-webkit-版本，而Edge 和 Firefox则使用后者，无前缀版本。

## Vendor_Prefix 浏览器引擎前缀
主流浏览器引擎前缀:
- -webkit- (谷歌, Safari, 新版Opera浏览器等)
- -moz- (火狐浏览器)
- -o- (旧版Opera浏览器等)
- -ms- (IE浏览器 和 Edge浏览器)

## Blend modes（混合模式）
[MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Styling_boxes/Advanced_box_effects)
