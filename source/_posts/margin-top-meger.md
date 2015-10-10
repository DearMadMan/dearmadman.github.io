title: css盒模型 垂直方向magin合并问题
date: 2015-09-16 14:05:38
categories: [other,css]
tags:
---
![](/images/s18.jpg)

今天做设计的同事想学习一下前端的东西，写一些简单的页面，刚好碰到了一些自己也不太深入研究的部分，这里做一下记录。

## 起因
```html
	<div class="nav">
		<div class="time">12:00</div>
	</div>
```
```style
	.nav{
		margin:0 auto;
		width: 980px;
		height: 120px;
		background: green;
	}
	.time{
		width: 80px;
		height: 80px;
		background: red;
		margin-top:10px;
	}
```
上面的代码发现 内部div的margin-top会直接作用到上级nav中，导致time和nav一起偏离上部

## 原因
盒模型css2.1中规定的内容：
>
In this specification, the expression collapsing margins means that adjoining margins (no non-empty content, padding or border areas or clearance separate them) of two or more boxes (which may be next to one another or nested) combine to form a single margin

垂直方向上：毗邻的两个普通盒模型的margin是可以合并的，在没有内容隔阂的两个margin值合并为大者。

以下为详细解答
引用来源：http://blog.sina.com.cn/s/blog_601b97ee0101b8xe.html

外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距。
合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。
外边距合并
外边距合并（叠加）是一个相当简单的概念。但是，在实践中对网页进行布局时，它会造成许多混淆。
简单地说，外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距。合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。
当一个元素出现在另一个元素上面时，第一个元素的下外边距与第二个元素的上外边距会发生合并。请看下图：CSS 外边距合并实例 1
亲自试一试
当一个元素包含在另一个元素中时（假设没有内边距或边框把外边距分隔开），它们的上和/或下外边距也会发生合并。请看下图：
CSS 外边距合并实例 2
亲自试一试
尽管看上去有些奇怪，但是外边距甚至可以与自身发生合并。
假设有一个空元素，它有外边距，但是没有边框或填充。在这种情况下，上外边距与下外边距就碰到了一起，它们会发生合并：
CSS 外边距合并实例 3
如果这个外边距遇到另一个元素的外边距，它还会发生合并：
CSS 外边距合并实例 4
这就是一系列的段落元素占用空间非常小的原因，因为它们的所有外边距都合并到一起，形成了一个小的外边距。
外边距合并初看上去可能有点奇怪，但是实际上，它是有意义的。以由几个段落组成的典型文本页面为例。第一个段落上面的空间等于段落的上外边距。如果没有外边距合并，后续所有段落之间的外边距都将是相邻上外边距和下外边距的和。这意味着段落之间的空间是页面顶部的两倍。如果发生外边距合并，段落之间的上外边距和下外边距就合并在一起，这样各处的距离就一致了。
CSS 外边距合并的实际意义
注释：只有普通文档流中块框的垂直外边距才会发生外边距合并。行内框、浮动框或绝对定位之间的外边距不会合并。
注释：只有普通文档流中块框的垂直外边距才会发生外边距合并。行内框、浮动框或绝对定位之间的外边距不会合并。
　　在css2.1中，水平的margin不会被折叠。 
　　垂直margin可能在一些盒模型中被折叠： 
     1、在常规文档流中，2个或以上的块级盒模型相邻的垂直margin会被折叠。 
　　    最终的margin值计算方法如下： 
        a、全部都为正值，取最大者；
        b、不全是正值，则都取绝对值，然后用正值减去最大值；
        c、没有正值，则都取绝对值，然后用0减去最大值。
   　　 注意：相邻的盒模型可能由DOM元素动态产生并没有相邻或继承关系。 
    2、相邻的盒模型中，如果其中的一个是浮动的（floated），垂直margin不会被折叠，甚至一个浮动的盒模型和它的子元素之间也是这样。 
    3、设置了overflow属性的元素和它的子元素之间的margin不会被折叠（overflow取值为visible除外）。 
    4、设置了绝对定位（position:absolute）的盒模型，垂直margin不会被折叠，甚至和他们的子元素之间也是一样。 
    5、设置了display:inline-block的元素，垂直margin不会被折叠，甚至和他们的子元素之间也是一样。 
    6、如果一个盒模型的上下margin相邻，这时它的margin可能折叠覆盖（collapse through）它。在这种情况下，元素的位置（position）取决于它的相邻元素的margin是否被折叠。 
        a、如果元素的margin和它的父元素的margin-top折叠在一起，盒模型border-top的边界定义和它的父元素相同。
        b、另外，任意元素的父元素不参与margin的折叠，或者说只有父元素的margin-bottom是参与计算的。如果元素的border-top非零，那么元素的border-top边界位置和原来一样。
   　　 一个应用了清除操作的元素的margin-top绝不会和它的块级父元素的margin-bottom折叠。 
    　　注意，那些已经被折叠覆盖的元素的位置对其他已经被折叠的元素的位置没有任何影响；只有在对这些元素的子元素定位时，border-top边界位置才是必需的。 
    7、根元素的垂直margin不会被折叠。
　　　　浮动的块级元素的margin-bottom总是与它后面的浮动块级兄弟元素（floated next in-flow block-level sibling）的margin-top相邻，除非那个同级元素使用了清除操作。 
　　浮 动的块级元素的margin-top和它的第一个浮动块级子元素（floated first in-flow block-level child）的margin-top相邻（如果该元素没有border-top，没有padding-top，并且子元素没有使用清除操作）。 
　　浮动的块级元素的margin-bottom如果符合下列条件，那么它和它的最后一个浮动块级子元素的margin-bottom相邻（如果该元素没有指定padding-bottom或border）： 
    　　a、指定了height:auto
    　　b、min-height小于元素的实际使用高度（height）
    　　c、max-height大于元素的实际使用高度（height）
　　如 果一个元素的min-height属性设置为0，那么它所拥有的margin是相邻的，并且它既没有border-top和border- bottom，也没有padding-top和padding-bottom，它的height属性可以是0或auto，它不能包含一个内联的盒模型 （line box），它所有的浮动子元素（如果有的话）的margin也都是相邻的。 
　　当一个元素拥有的margin折叠了，并且它使用了清除操作，那么它的margin-top会和紧随其后的兄弟元素的相邻margin折叠，但结果是它的margin将无法和其块级父元素的margin-bottom折叠。 
　　折叠操作是以padding、margin、border的值为基础的（即在浏览器解析所有这些值之后），折叠后的margin计算将覆盖已使用的不同margin的值。
如何解决
　　W3C的CSS2.1 定义了几种情况，简单为元素添加边框或者间距（border or padding)就可以不使边距重叠。
　　我们 添加一个小的间距:
 　　#box {background: #F80; margin: 10px; padding: 1px 0;} 

