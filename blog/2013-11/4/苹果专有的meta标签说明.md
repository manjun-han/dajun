
####苹果专有的meta标签说明


**名称:** apple-mobile-web-app-capable

**目的:** 设置Web App是否运行在全屏模式下(full-screen mode)。

**语法:**

    <meta name="apple-mobile-web-app-capable" content="yes">
    	
**说明:**
    
>content设置为yes，Web Application会运行在全屏模式下。默认会使用Safari显示web内容。通过window.navigator.standalone属性可以判断当前页面是否运行在全屏模式下，该属性为只读属性(read-only)。

  
    
  
    

**名称:** apple-mobile-web-app-status-bar-style

**目的:** 设置Web Application设置状态栏(status bar)的样式。

**语法:**

    <meta name="apple-mobile-web-app-status-bar-style" content="black">
    	
**说明:**
    
>该标签要求首先要设置Web Application运行在全屏模式下。设置content为default，状态栏以常态显示。设置为black，状态栏会变为黑色背景。设置为black-transparent，状态栏会变为黑色透明背景。设置black或default，web内容会显示在状态栏下面，设置为black-transparent，web内容会全屏显示，同时会被状态栏遮盖。默认值为default。
  


**名称:** format-detection

**目的:** 启用或禁用IOS Safari下web页面中电话号码的自动检测。

**语法:**

    <meta name="format-detection" content="telephone=no">
    	
**说明:**
    
>IOS中的Safari默认会检测任何格式类似于电话号码的字符串，并将其作为一个直接拨号link。设置telephone=no会禁用该功能。


**名称:** viewport

**目的:** 在IOS下，改变展示页面使用的逻辑窗口大小。

**语法:**

    <meta name = "viewport" content = "width = 320, initial-scale = 2.3, user-scalable = no">
    	
**说明:**
    
>使用viewport meta关键字可以提升IOS下web内容的展示效果。可以使用viewport meta标签来设置viewport的宽度和初始缩放比。

>例如，如果webpage的宽度小于980px，可以设置viewport的大小来适配网页内容。设计IOS专有的web application，需要将其宽度设置为设备宽度。

>**表-1** 描述了viewport meta支持的关键字和默认值。在viewport meta中设置多属性时，需要使用逗号分隔的赋值列表。规则如下：
>
>* 不要使用分号作为分隔符。
>* 空格也可以作为分隔符使用，但逗号优先。
>* 数值型属性的值包含了非数值型字符串，但以数字开头，那么数字前缀会被当做值来使用。例如1.0x等价于1.0；123x456等价于123。非数字开头的参数，取值为0。
>
>在使用设备尺寸时，不要使用具体数值进行硬编码，相反需要使用**表-2**规定的常量。例如，在横屏下，使用device-width而不是320设置viewport宽度，使用device-height而不是480来设置viewport高度。
>
>不必设置viewport的所有属性。部分属性设置后，IOS上的Safari会推断其余属性的值。例如，设置缩放比(scale)为1.0后，Safari会认为viewport的宽度在竖屏下是device-width，在横屏下是device-height。因此，想要设置viewport的宽度为980px，缩放比为1.0，则需要同时显示设置这两个属性。
>
>例如:
>
>**设置viewport的宽度为设备宽度**
>
>		<meta name = "viewport" content = "width = device-width">
>
>**设置初始缩放比为1.0**
>
>		<meta name = "viewport" content = "initial-scale = 1.0">
>
>**设置初始缩放比并禁用用户的缩放行为**
>
>		<meta name = "viewport" content = "initial-scale = 2.3, user-scalable = no">
>

<table  border="0" cellspacing="0" cellpadding="5" style="width:858px;" width="858">
	<caption > <strong class="caption_number">表-1</strong>
		&nbsp;&nbsp;Viewport属性
	</caption>
	<tbody>
		<tr>
			<th>
				<p>属性</p>
			</th>
			<th>
				<p>描述</p>
			</th>
		</tr>
		<tr>
			<td >
				<p>
					width
				</p>
			</td>
			<td>
				<p>
					viewport的像素宽度. 默认980。取值范围从200-10000像素
				</p>
				<p>
					也可以使用表-2描述的常量来为该属性赋值。
				</p>
				<p>iOS 1.0及以后可用.</p>
			</td>
		</tr>
		<tr>
			<td >
				<p>
					height
				</p>
			</td>
			<td>
				<p>
					viewport的像素高度。默认会根据width属性值以及设备的宽高比来计算高度值。取值范围223到10000像素。
				</p>
				<p>
					也可以使用表-2描述的常量来为该属性赋值。
				</p>
				<p>iOS 1.0及以后可用.</p>
			</td>
		</tr>
		<tr>
			<td >
				<p>
					initial-scale
				</p>
			</td>
			<td>
				<p>
					作为乘数的viewport的初始缩放比，默认会填充页面内容适配显示区来计算值。取值范围取决于
					minimum-scale
					和
					maximum-scale
					属性.
				</p>
				<p>
					该属性只能设置视口的初始缩放比——页面首次显示的viewport的缩放比例。之后用户可以放大或缩小viewport，除非将user-scalable设置为no。缩放范围限定在minimun-scale和maximum-scale之间。
				</p>
				<p>iOS 1.0及以后可用.</p>
			</td>
		</tr>
		<tr>
			<td >
				<p>
					minimum-scale
				</p>
			</td>
			<td>
				<p>
					指定viewport的最小缩放值。默认为0.25，取值范围大于0小于等于10.0。
				</p>
				<p>iOS 1.0及以后可用.</p>
			</td>
		</tr>
		<tr>
			<td >
				<p>
					maximum-scale
				</p>
			</td>
			<td>
				<p>
					指定viewport的最大缩放值。默认为5.0，取值范围大于0小于等于10.0。
				</p>
				<p>iOS 1.0及以后可用.</p>
			</td>
		</tr>
		<tr>
			<td >
				<p>
					user-scalable
				</p>
			</td>
			<td>
				<p>
					决定用户是否可以进行放大或缩小页面操作——亦即是否可以改变viewport的缩放比(scale)。设置yes允许缩放，no禁用缩放，默认为yes。
				</p>
				<p>
					设置user-scalable为no同时会阻止页面在input框中输入文本时的滚动。
				</p>
				<p>iOS 1.0及以后可用.</p>
			</td>
		</tr>
	</tbody>
</table>

<table  border="0" cellspacing="0" cellpadding="5">
	<caption > <strong class="caption_number">表-2</strong>
		&nbsp;&nbsp;指定viewport属性值
	</caption>
	<tbody>
		<tr>
			<th scope="col">
				<p>值</p>
			</th>
			<th scope="col">
				<p>说明</p>
			</th>
		</tr>
		<tr>
			<td >
				<p>
					device-width
				</p>
			</td>
			<td>
				<p>设备的像素宽度。</p>
				<p>OS 1.1.1及以后可用.</p>
			</td>
		</tr>
		<tr>
			<td >
				<p>
					<code>device-height</code>
				</p>
			</td>
			<td>
				<p>设备的像素高度</p>
				<p>OS 1.1.1及以后可用.</p>
			</td>
		</tr>
	</tbody>
</table>



参见:

[Supported Meta Tags](https://developer.apple.com/library/safari/documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html#//apple_ref/doc/uid/TP40008193-SW3)

[Configuring the Viewport](https://developer.apple.com/library/safari/documentation/AppleApplications/Reference/SafariWebContent/UsingtheViewport/UsingtheViewport.html#//apple_ref/doc/uid/TP40006509-SW19)









		    	
