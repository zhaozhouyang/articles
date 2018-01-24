# 基本文本和字体样式

## 字体
### 字体种类
##### 网页安全字体
说到字体可用性，只有某几个字体通常可以应用到所有系统，因此可以毫无顾忌地使用。这些都是所谓的 网页安全字体.  
大多数时候，作为网页开发者，我们希望对用于显示我们的文本内容的字体有更具体的控制。问题在于，需要一个方法来知道当前正在浏览我们的网站网页的电脑，它有哪些可用字体。我们并不是总能在每种情况下都知道这一点，但是网络安全字体在几乎所有最常用的操作系统（Windows，Mac，最常见的Linux发行版，Android和iOS版本）中都可用。  
实际的Web安全字体列表将随着操作系统的发展而改变，但是可以认为下面的字体是网页安全的，至少对于现在来说 (它们中的许多都非常流行，这要感谢微软在90年代末和21世纪初期的倡议Core fonts for the Web )：

字体名称 | 字体类型 | 注意
------- | ------- | ----
Arial |	sans-serif	| 通常认为最佳做法还是添加 Helvetica 作为 Arial 的首选替代品，尽管它们的字体面几乎相同，但 Helvetica 被认为具有更好的形状，即使Arial更广泛地可用。
Courier New	| monospace	| 某些操作系统有一个 Courier New 字体的替代（可能较旧的）版本叫Courier。使用Courier作为Courier New的首选替代方案，被认为是最佳做法。
Georgia	| serif |
Times New Roman	| serif |某些操作系统有一个 Times New Roman 字体的替代（可能较旧的）版本叫 Times。使用Times作为Times New Roman的首选替代方案，被认为是最佳做法。
Trebuchet MS | sans-serif | 您应该小心使用这种字体——它在移动操作系统上并不广泛。
Verdana | sans-serif	 

##### 默认字体
CSS 定义了 5 个常用的字体名称:  serif, sans-serif, monospace, cursive,和 fantasy. 这些都是非常通用的，当使用这些通用名称时，使用的字体完全取决于每个浏览器，而且它们所运行的每个操作系统也会有所不同。这是一种糟糕的情况，浏览器会尽力提供一个看上去合适的字体。 serif, sans-serif 和 monospace 是比较好预测的，默认的情况应该比较合理，另一方面，cursive 和 fantasy 是不太好预测的，我们建议使用它们的时候应该稍微注意一些，多多测试。  

五个名称定义如下：

名称 | 定义
-|-
serif | 有衬线的字体 (在一些字体的结尾，你看到的那些华丽的和其他的细节)
sans-serif	| 没有衬线的字体。
monospace	| 每个字符具有相同宽度的字体，通常用于代码列表。
cursive	| 用于模拟笔迹的字体，具有流动的连接笔画。
fantasy	| 用来装饰的字体

### 字体大小
在我们之前的模块中的CSS values and units 文章，我们回顾了length and size units. 字体大小 (通过 font-size 属性设置) 可以取大多数这些单位的值 (以及其他，比如百分比 percentages)，然而你在调整字体大小时，最常用的单位是：

- px (像素): 将像素的值赋予给你的文本。这是一个绝对单位， 它导致了在任何情况下，页面上的文本所计算出来的像素值都是一样的。
- ems: 1em 等于我们设计的当前元素的父元素上设置的字体大小 (更加具体的话，比如包含在父元素中的大写字母 M 的宽度) 如果你有大量设置了不同字体大小的嵌套元素，这可能会变得棘手, 但它是可行的，如下图所示。为什么要使用这个麻烦的单位呢? 当你习惯这样做时，那么就会变得很自然，你可以使用em调整任何东西的大小，不只是文本。你可以有一个单位全部都使用 em 的网站，这样维护起来会很简单。
- rems: 这个单位的效果和 ems 差不多，除了 1rem 等于 HTML 中的根元素的字体大小， (i.e. <html>) ，而不是父元素。这可以让你更容易计算字体大小，但是遗憾的是， rems 不支持 Internet Explorer 8 和以下的版本。如果你的项目需要支持较老的浏览器，你可以坚持使用ems 或 px, 或者是 polyfill 就像 REM-unit-polyfill.

元素的 font-size 属性是从该元素的父元素继承的。所以这一切都是从整个文档的根元素——<html>开始，浏览器的 font-size 标准设置的值为 16px。在根元素中的任何段落 (或者那些浏览器没有设置默认大小的元素)，会有一个最终的大小值：16px。其他元素也许有默认的大小，比如 h1 元素有一个 2ems 的默认值，所以它的最终大小值为 32px。当你开始更改嵌套元素的字体大小时，事情会变得棘手。比如，如果你有一个 article 元素在你的页面上，然后设置它的 font-size 为 1.5ems (通过计算，可以得到大小为 24px)，然后想让 article 元素中的段落获得一个计算值为 20px 的大小，那么你应该使用多少 em。

当调整你的文本大小时，将文档(document)的基础  font-size 设置为10px往往是个不错的主意，这样之后的计算会变得简单，所需要的 (r)em 值就是想得到的像素的值除以 10，而不是 16。做完这个之后，你可以简单地调整在你的 HTML 中你想调整的不同类型文本的字体大小。在样式表的指定区域列出所有font-size的规则集是一个好主意，这样它们就可以很容易被找到。

### 文字阴影
你可以为你的文本应用阴影，使用 text-shadow 属性。这最多需要 4 个值，如下例所示：
```
text-shadow: 4px 4px 5px red;
```
4 个属性如下:
1. 阴影与原始文本的水平偏移，可以使用大多数的 CSS 单位 length and size units, 但是 px 是比较合适的。这个值必须指定。
2. 阴影与原始文本的垂直偏移;效果基本上就像水平偏移，除了它向上/向下移动阴影，而不是左/右。这个值必须指定。
3. 模糊半径 - 更高的值意味着阴影分散得更广泛。如果不包含此值，则默认为0，这意味着没有模糊。可以使用大多数的 CSS 单位 length and size units.
4. 阴影的基础颜色，可以使用大多数的 CSS 颜色单位 CSS color unit. 如果没有指定，默认为 black.

##### 多种阴影
您可以通过包含以逗号分隔的多个阴影值，将多个阴影应用于同一文本，例如：
```
text-shadow: -1px -1px 1px #aaa,
             0px 4px 1px rgba(0,0,0,0.5),
             4px 4px 5px rgba(0,0,0,0.7),
             0px 0px 7px rgba(0,0,0,0.4);
```
更多阴影示例：
https://www.sitepoint.com/moonlighting-css-text-shadow/

# 链接
### 链接状态
- Link (没有访问过的): 这是链接的默认状态，当它没有处在其他状态的时候，它可以使用:link 伪类来应用样式。
Visited: 这个链接已经被访问过了(存在于浏览器的历史纪录), 它可以使用 :visited 伪类来应用样式。
- Hover: 当用户的鼠标光标刚好停留在这个链接，它可以使用 :hover 伪类来应用样式。
- Focus: 一个链接当它被选中的时候 (比如通过键盘的 Tab  移动到这个链接的时候，或者使用编程的方法来选中这个链接 HTMLElement.focus()) 它可以使用 :focus 伪类来应用样式。
- Active: 一个链接当它被激活的时候 (比如被点击的时候)，它可以使用 :active 伪类来应用样式。

```
a {

}


a:link {

}

a:visited {

}

a:focus {

}

a:hover {

}

a:active {

}
```
这个顺序是重要的，因为链接的样式是建立在另一个样式之上的，比如第一个规则的样式会应用到所有后续的样式，如果当一个链接被激活 (activated) 的时候，它也是处于悬停 (hover) 状态的。如果你搞错了顺序，那么就可能不会产生正确的效果。要记住这个顺序，你可以尝试这样帮助记忆：**L**o**V**e **F**ears **HA**te.

# Web字体
```
@font-face {
  font-family: "myFont";
  src: url("myFont.ttf");
}
```
### 查找字体
<html>
<ul>
 <li>免费的字体经销商：这是一个可以下载免费字体的网站(可能还有一些许可条件，比如对字体创建者的信赖)。比如： <a href="https://www.fontsquirrel.com/" class="external external-icon" rel="noopener">Font Squirre</a>，<a href="http://www.dafont.com/" class="external external-icon" rel="noopener">dafont</a>&nbsp;和&nbsp;<a href="https://everythingfonts.com/" class="external external-icon" rel="noopener">Everything Fonts</a>。</li>
 <li>收费的字体经销商：这是一个收费则字体可用的网站，例如<a href="http://www.fonts.com/" class="external external-icon" rel="noopener">fonts.com</a>或<a href="http://www.myfonts.com/" class="external external-icon" rel="noopener">myfonts.com</a>。您也可以直接从字体铸造厂中购买字体，例如<a href="https://www.linotype.com/" class="external external-icon" rel="noopener">Linotype</a>，<a href="http://www.monotype.com" class="external external-icon" rel="noopener">Monotype</a>&nbsp;或&nbsp;<a href="http://www.exljbris.com/" class="external external-icon" rel="noopener">Exljbris</a>。</li>
 <li>在线字体服务：这是一个存储和为你提供字体的网站，它使整个过程更容易。更多细节见<a href="#Using_an_online_font_service">Using an online font service</a>。</li>
</ul>
</html>

1. 将解压缩的目录重命名为简易的目录，比如fonts
2. 打开 stylesheet.css 文件，把包含在你的网页中的 @font-face块复制到你的 web-font-start.css 文件—— 你需要把它们放在最上面，在你的CSS之前，因为字体需要导入才能在你的网站上使用。
3. 每个url()函数指向一个我们想要导入到我们的CSS中的字体文件——我们需要确保文件的路径是正确的，因此，在每个路径的开头添加fonts/ （必要时进行调整）。
4. 现在，您可以在字体栈中使用这些字体，就像任何web安全或默认的系统字体一样。
例如：
```
font-family: 'zantrokeregular', serif;
```
```
@font-face {
  font-family: 'ciclefina';
  src: url('fonts/cicle_fina-webfont.eot');
  src: url('fonts/cicle_fina-webfont.eot?#iefix') format('embedded-opentype'),
         url('fonts/cicle_fina-webfont.woff2') format('woff2'),
         url('fonts/cicle_fina-webfont.woff') format('woff'),
         url('fonts/cicle_fina-webfont.ttf') format('truetype'),
         url('fonts/cicle_fina-webfont.svg#ciclefina') format('svg');
  font-weight: normal;
  font-style: normal;
}
```

### 使用在线字体服务
https://www.google.com/fonts
