#web前端规范之CSS

###css属性顺序
元素显示属性 —> 元素布局定位属性 –> 元素属性 –> 元素内容属性 –> 其他属性

###id和class命名约定
* id 和 class 的命名基本原则: 内容优先，表现为辅。首先根据内容来命名，如:#title,#icon。如根据内容无法找到合适的命名，可以再结合表现进行命名，如：col-main, col-sub。

* id 和 class 的名称一律小写，多个单词以连字符连接，如：main-wrap。

* 在不影响语意的情况下，id 和 class 的名称 可以适当使用缩写，如: col、nav、 hd，不要自造缩写。

###css书写规范
* 页面内部避免使用style属性。

* css放在head标签中，由link标签引入，使页面的结构与表现分离。

* 为JavaScript预留钩子的命名， 以 j_ 起始， 比如:j_hide， j_show。

* 书写代码前, 考虑并提高样式重复使用率。

* 充分利用html自身属性及样式继承原理减少代码量。

		如：需要一个行内标签时，不应该使用一个设置为display：inline的<div>标签，<span>标签更加合理。 


* 减少影响性能的属性，如：position，float。

* 为大区块样式添加注释，小区块适量注释。

###CSS样式新建或修改尽量遵循以下原则
* 根据新建样式的适用范围分为三级：全站级、产品级、页面级。

* 尽量通过继承和层叠重用已有样式。

* 不要轻易改动全站级CSS。改动后，要经过全面测试。

* 兼容多个浏览器时，将标准属性写在底部。

		-moz-border-radius: 15px; /* Firefox */
		-webkit-border-radius: 15px; /* Safari和Chrome */
		border-radius: 15px; /* Opera 10.5+, 以及使用了IE-CSS3的IE浏览器 *//标准属性
		
* 可用上级节点进行限定。

		.recommend-mod .hd

* 多选择器规则之间换行，即当样式针对多个选择器时每个选择器占一行。
	
		button.btn,
		input.btn,
		input[type="button"] {…};
		
* 选择器层叠不能超过三层

* 避免使用CSS通配符 *

* 尽量使用类选择器 

* 为动画的 HTML 元件使用 fixed 或 absoult 的 position，那么修改他们的 CSS 是会大大减小 reflow

 


###优化CSS选择器

> `#header a { color: #444; };/*CSS选择器是从右边到左边进行匹配*/`

浏览器将检查整个文档中的所有链接和每个链接的父元素，并遍历文档树去查找ID为header的祖先元素，如果找不到header将追溯到文档的根节点，解决方法如下:

* 避免使用通配规则和相邻兄弟选择符、子选择符,、后代选择符、属性选择符等选择器

* 不要限定id选择符，如div#header（提权的除外）

* 不要限定类选择器，如ul.recommend（提权的除外）

* 不要使用 ul li a 这样长的选择符

* 避免使用标签子选择符，如#header > li > a




















