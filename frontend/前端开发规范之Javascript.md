#web前端规范之Javascript

###命名规范
* 常量名: 全部大写并单词间用下划线分隔

    	如：CSS_BTN_CLOSE、TXT_LOADING
    	
* 对象的属性或方法名：驼峰式

    	如：init、bindEvent、updatePosition
	    示例：Dialog.prototype = {
                init: function () {},
                bindEvent: function () {},
                updatePosition: function () {}
             };
       
* 类名（构造器）：小驼峰式但首字母大写

	    如：Current、DefaultConfig

* 函数名：驼峰

	    如：current()、defaultConfig()

* 变量名：驼峰式

	    如：current、defaultConfig

* 私有变量名：驼峰式但需要用_开头

	    如：_current、_defaultConfig
	    
###编码规范
* "()"前后需要跟空格

* "="前后需要跟空格

* ","后面需要跟空格

* JSON对象需格式化对象参数

* if、while、for、do语句的执行体用"{}"括起来

* "{}"格式如下

		if (a==1) {
    		//代码
		};
	
* 避免额外的逗号

		var arr = [1,2,3,];
	
###在==时，则会有一些让人难以理解的陷阱

	(function () {
    	var undefined;
    	undefined == null; // true
    	1 == true; //true
    	2 == true; // false
    	0 == false; // true
    	0 == ''; // true
    	NaN == NaN;// false
    	[] == false; // true
    	[] == ![]; // true
	})();
	
###下面类型的对象不建议用new构造

	new Number
	new String
	new Boolean
	new Object //用{}代替
	new Array //用[]代替
	
###从string到number的转换，使用parseInt，必须显式指定第二个参数的进制

	/** 推荐写法*/
	var a = '1';
	var aa = parseInt(a,10);
	typeof(a); //"string"
	console.log(a); //'1'
	typeof(aa); //"number"
	console.log(aa); //1

###从float到integer的转换

	/** 推荐写法*/
	Math.floor/Math.round/Math.ceil
	/** 不推荐写法*/
	parseInt
	
###注重HTML分离, 减小reflow, 注重性能

1. 使用 documentFragment 对象在内存里操作 DOM。

2. 先把 DOM 给 display:none (有一次 repaint)，然后你想怎么改就怎么改。比如修改 100 次，然后再把他显示出来。

###不要一条一条地修改 DOM 的样式

	// 不好的写法
	var left = 10,
	top = 10;
	el.style.left = left + "px";
	el.style.top  = top  + "px";
	// 推荐写法
	el.className += " theclassname";
		
	
	
	
	









