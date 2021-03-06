###CSS垂直居中对齐

在CSS的布局中，相对于元素的水平居中来说，如何实现元素的垂直居中一直是一个老大难问题，因为CSS中没有明确的用来实现垂直居中的方式。以下是对于CSS中垂直居中实现的方法的一个总结：

1. 使用`line-height`属性和`vertial-align`属性

	`vertical-align`属性指定了行内元素(`display:inline`或`display:inline-block`元素)或表格单元格(`display:table-cell`)元素的垂直对齐方式；`line-height`属性用于控制行间距
	
		<div class="wrapper">
			<span class="inner_el">这是内联元素的垂直居中对齐</span>
		</div>
		
		.wrapper{
			line-height:40px;
			height:40px;
		}
		
		.wrapper .inner_el{
			vertical-align:middle;
		}I

	要求：
	1. 对齐元素的`line-height`和`height`属性设置为相等
	2. 对齐元素为文本或者是内联元素，并且设置`vertical-align:middle`
	
	适用于纯文本，内联元素，不限个数。

2. 使用`padding`属性

		<div class="wrapper">
			<span class="inner_el">任意元素的垂直居中对齐</span>
		</div>
		
		.wrapper{
			padding-top:20px;
			padding-bottom:20px;
		}
		
	要求：		
	1. 父元素的`padding-top`属性和`padding-bottom`属性设置为等值
	
	适用于任何单行元素，对于内联子元素不限个数。

3. 使用`position`绝对布局和负`margin-top`

		<div class="wrapper">
			<div class="inner_el">绝对布局元素的垂直居中</div>
		</div>
		
		.wrapper{
			position:relative;
		}
		
		.wrapper .inner_el{
			position:absolute;
			height:200px;
			top:50%;
			margin-top:-100px;
		}
	要求：
		1. 对齐元素的父元素指定布局(`position`为`relative`,`fixed`,`absolute`任何一个)
		2. 对齐元素为绝对布局,`position:absolute`
		3. 对齐元素设置`top:50%;`
		4. 对齐元素定高并且`margin-top`属性设置为一半高的复值
		
	适用于单个块级定高元素。

4. 使用`position`绝对布局和负`transform`

		<div class="wrapper">
			<div class="inner_el">绝对布局元素的垂直居中</div>
		</div>
		
		.wrapper{
			position:relative;
		}
		
		.wrapper .inner_el{
			position:absolute;/* 或relative */
			top:50%;
			transform:translateY(-50%);
		}

	要求：
	1. 对齐元素的`position`属性为`absolute`或者`relative`(如果定位元素的`posiiton:absolute;`,其父元素需要时定位元素)
	2. 设置对齐元素的`top:50%;`
	3. 设置对齐元素的`transform:translateY(-50%);`

	适用于单个块级元素，不限高度。


5. 使用`table`布局


		<div class="wrapper">
			<span class="inner_el">table布局元素的垂直居中</span>
		</div>
		
		.wrapper{
			display:table-cell;
			vertical-align:middle;
		}
		
		
	要求：
	1. 设置定位元素的父元素的`display:table-cell;`且`vertical-align:middle;`
	
	适用于任何元素	

6. 使用`position`绝对布局和`margin:auto 0;`

		<div class="wrapper">
			<span class="inner_el">绝对布局元素的垂直居中</span>
		</div>		
		
		.wrapper{
			position:relative;
		}
		.wrapper .inner_el{
			position:absolute;
			top:0;
			bottom:0;
			margin-top: auto;
			margin-bottom:auto;
		}
	要求
	1. 定位元素为绝对布局,`position:absolute;`并且`top:0;bottom:0`,之后设置其`margin-top:auto;margin-bottom:auto;`
	2. 定位元素需要高度固定
	3. 父元素需要是定位元素
	
	适用于单个元素对齐，不限高度
	
7. 使用Flexbox布局


		<div class="wrapper">
			<span class="inner_el">Flexbox垂直居中</span>
		</div>		
		
		.wrapper{
			display:flex; /* flexbox布局 */
			flex-direction: row; /* 子元素横排 */
			flex-wrap: wrap; /* 允许子元素换行 */
			align-items: center; /* 垂直居中对齐子元素 */
		}
	要求		
	
	1. 垂直居中元素的父元素为flexbox布局`display:flex`
	2. 指定元素横排且垂直居中，`flex-direction: row; align-items: center;`
	3. 必要时允许子元素换行，`flex-wrap: wrap; `
	
	适用于任意个数的元素，不限元素高度。

8. 使用绝对布局和`calc()`函数

		<div class="wrapper">
			<span class="inner_el">Flexbox垂直居中</span>
		</div>
		
		.wrapper{
		     position:relative;
		 }
		 .wrapper .inner_el{
		     position:absolute;
		     height:200px;
		     top:calc(50% - 100px);     
		 }
	要求:
	1. 父元素为布局元素
	2. 居中元素为绝对布局元素并且定高
	3. 指定居中元素的`top`属性值为`50% - height/2`;
		





###参考

+ [Understanding vertical-align, or "How (Not) To Vertically Center Content"][1]
+ [6 CSS Tricks to Align Content Vertically][2]
+ [Absolute Horizontal And Vertical Centering In CSS][3]
+ [MDN vertical-align][4]
+ [MDN line-height][5]
+ [CSS Line-Height][6]
+ [CSS Flexible Box Layout][7]
+ [Advanced layouts with flexbox][8]
+ [Using CSS flexible boxes][9]
+ [THE ULTIMATE FLEXBOX CHEAT SHEET][10]
+ [A Complete Guide to Flexbox][11]
+ [CENTERING THINGS][12]
+ [THE COMPLETE GUIDE TO CENTERING A DIV][13]
+ [Centering in CSS: A Complete Guide][14]
+ [How To Center Anything With CSS][15]

[1]: http://phrogz.net/css/vertical-align/index.html
[2]: http://www.hongkiat.com/blog/css-tricks-vertical-align-content/
[3]: http://www.smashingmagazine.com/2013/08/absolute-horizontal-vertical-centering-css/
[4]: https://developer.mozilla.org/en-US/docs/Web/CSS/vertical-align
[5]: https://developer.mozilla.org/en-US/docs/Web/CSS/line-height
[6]: http://www.slideshare.net/maxdesign/line-height
[7]: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout
[8]: https://developer.mozilla.org/en-US/docs/Inbox/Advanced_layouts_with_flexbox
[9]: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Using_CSS_flexible_boxes
[10]: http://www.sketchingwithcss.com/samplechapter/cheatsheet.html
[11]: https://css-tricks.com/snippets/css/a-guide-to-flexbox/
[12]: http://www.w3.org/Style/Examples/007/center.en.html
[13]: http://www.tipue.com/blog/center-a-div/
[14]: https://css-tricks.com/centering-css-complete-guide/
[15]: https://codemyviews.com/blog/how-to-center-anything-with-css









