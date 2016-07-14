# 给Rails做前端，你需要知道的知识(v0.1)

## 前言

不一定每个项目都会做前后端分离，因此如果你给rails项目做前端，有些东西你是必须要了解的。这篇文章会教你如何给rails集成前端，让你看到rails的view时候不会一头雾水。

## 概览

本文将包含以下内容：

* 一点ruby知识
* erb是什么
* erb标签
* view helper方法
* js、css和图片

## 一、一点ruby知识
虽然ruby对程序员很友好，但是有些写法可能会让你感到困惑。

* 变量

	在rails的view中一般只会出现ruby的实例变量，以`@`开头
	
	```ruby
	@foo = "instance variable"
	print @foo						#=> "instance variable"
	```
	
* 方法
	
	在ruby中，方法调用可以省略括号，像这样：
	
	```ruby
	# 下面两种写法是等价的
	func1("a")
	func1 "a"
	
	# 多个参数同样也是用,隔开
	func2("a", "b")
	func2 "a", "b"
	```
	
	ruby可以给方法传递一个hash，在rails提供的view helper方法中很常见，像这样
	
	```ruby
	# ruby中描述hash的两种写法，是等价的，等于传入了一个{ a: 1, b: 2 }的hash
	func a: 1, b: 2
	func :a => 1, :b => 2
	```
	
* 字符串插值
	
	ruby用`#{}`在字符串内插值：
	
	```ruby
	# 双引号才能进行插值(字符串还有其他写法，但是一般在view中不会见到)
	print "1 + 1 is #{ 1 + 1 }" 		#=> "1 + 1 is 2"
	```

	
* each迭代器
	
	view中也会经常要对数组对象进行迭代，常见的是`each`方法。
	
	在ruby中，代码块有两种写法：
	
	```ruby
	{
	  "This is a block"
	}
	
	do
	  "This is also a block"
	end
	```
	
	`each`方法可以接受一个代码块作为参数，对数组每个元素执行代码块中的代码：
	
	```ruby
	["a", "b", "c"].each do |i|
	  # i代表数组每个元素，由代码块指定其名称
	  print i
	end
	
	# 上述代码将输出"abc"
	```
	
	Hash对象也可以进行迭代：
	
	```ruby
	{a: 1, b: 2}.each do |key, value|
	  print "key is #{key}, value is #{value}. "
	end
	
	# 上述代码将输出"key is a, value is 1. key is b, value is 2."
	```

## 二、erb是什么

目前我们项目的view都使用erb，[erb](http://apidock.com/ruby/ERB)是ruby的模版，当你需要使用根据程序动态生成的文件时(如html)，可以在这些文件中嵌入ruby代码。rails在返回这个html前会先用解析器执行这个文件的ruby代码，并进行替换。view文件中以erb后缀结尾的，rails都会进行解析。比如说我们有这样一个文件example.html.erb：

```erb
<!--解析前-->
<h1><%= 1 + 1 %></h1>
```

rails返回的html文件

```html
<!--解析后-->
<h1>2</h1>
```

## 三、erb标签
erb在解析是通过这些标签来分辨哪些是需要执行的代码

* `<% "your code here" %>`

	在`<% %>`内的代码会被执行，但不会被输出到文件中，比如：
	
	```erb
	<!--解析前-->
	<h1><% @foo = "foo" %></h1>
	```
	
	```erb
	<!--解析后-->
	<h1></h1>
	```

	上面例子中，`@foo`确实会被赋值(代码会执行)，解析器删去标签及其内容后就不会做其他事了。
	
* `<%= "your code here" %>`
	
	在`<%= %>`内的代码会被执行，且解析器会将标签即内容替换成执行结果。像之前的例子一样：
	
	```erb
	<!--解析前-->
	<h1><%= 1 + 1 %></h1>
	```
	
	```html
	<!--解析后-->
	<h1>2</h1>
	```

* `<%= "your code here" -%>`
	
	`<%= -%>`用的比较少，`-`表示该标签结束后不会换行，如：
	
	```erb
	<!--解析前-->
	<h1><%= "a" -%>
	<%= "b" %></h1>
	```

	```erb
	<!--解析后-->
	<h1>ab</h1>
	```
	
* `<%# "your code here" %>`

	`<%# %>`内包含代码也不会执行，作注释使用

## 四、view helper方法

Rails提供了很多view的helper方法。常用的有[`link_to`](http://apidock.com/rails/ActionView/Helpers/UrlHelper/link_to)，[`form_for`](http://apidock.com/rails/ActionView/Helpers/FormHelper/form_for)和[`image_tag`](http://apidock.com/rails/ActionView/Helpers/AssetTagHelper/image_tag)等，其实只是生成一段html，遇到了这些helper方法可以查[文档](http://apidock.com/rails)。

## 五、js、css和图片

### Asset Pipeline

在这之前，首先要了解[Asset Pipeline](http://guides.rubyonrails.org/asset_pipeline.html)这个东西。我们的项目使用Asset Pipeline打包js、css文件。对于生产环境的Rails应用，用户浏览网页的时候将会返回合并后的js、css文件，文件名中会包含一个哈希值以防止浏览器缓存，像这样：

```
global-908e25f4bf641868d8683022a5b62f54.css
```

对于图片的文件名，同样也会加上一个hash值。

### js、css和图片放哪里

Asset Pipeline在打包的时候，会把Rails项目中的这三个目录看作成同一个目录。但是我们放置的时候一般有一下规则：

```bash
# 这个目录用来放置自己编写的js、css等文件
app/assets/

# 这个目录用来放置自己编写的js、css库
lib/assets/

# 这个目录用来放置要引入的第三方库
vendor/assets/
```

要把哪些文件打包到合并之后的js、css文件都是可以自己配置的。下面说的是rails的默认行为，具体配置文件也是可以根据需要配置，但是一般不会。配置文件内容如下：

```javascript
// 引用js的配置在这里: app/assets/javascripts/application.js

//= require jquery
//= require jquery_ujs
//= require turbolinks
//= require_tree .
```

这里`require`、`require_tree`是为assets pipeline打包时指定要打包的文件，具体作用为：

* `require <path>` 加载特定的文件，且不会重复加载(就算在配置上加载了多次)
* `include <path>` 加载特定的文件，会重复加载
* `require_directory <path>` 加载特定路径，不包含其子目录
* `require_tree <path>` 加载特定路径，包含其子目录
* `require_self` 在加载其他文件前先载入自己文件的内容

....
更多请查看[这里](https://github.com/rails/sprockets#sprockets-directives)

对于css的配置文件，也跟上面类似：

```css
/*
* 引用css的配置在这里: app/assets/stylesheets/application.css
*
*= require_tree .
*= require_self
*/
```

### 如何引用图片、音频等资源

前面提到，Asset Pipeline编译后的文件会在文件名加上一个hash值，所以如果你直接引用图片的原文件名是无法引用这些文件的。因此Rails(实际上是Sporket)提供了这些helpers来引用assets资源。你可以使用[image_path](http://apidock.com/rails/ActionView/Helpers/AssetUrlHelper/image_path)、[audio_path](http://apidock.com/rails/ActionView/Helpers/AssetUrlHelper/audio_path)、[video_path](http://apidock.com/rails/ActionView/Helpers/AssetUrlHelper/video_path)等，要注意的是，例如image_path，传入的参数是相对路径的话，默认指`assets/images/`目录下，生成路径为`host/assets/images/your_path`，而使用绝对路径如`/your_path`时，url指向`host/your_path`，另外也可以使用一个完整的url。

### 单独引用的js、css文件

你也可以不把js、css文件打包到全局文件中。将你的js和css文件目录为`assets/path/to/yourjsfile.js`和`assets/path/to/yourcssfile.css`，在erb文件中这样引用：

```erb
<%= javascript_include_tag 'path/to/yourjsfile.js' %>
<%= stylesheet_link_tag 'path/to/yourcssfile.css' %>
```

在开发环境的默认配置下，到这一步你就能看到效果，但是要在生产环境上正常运行，你必须将这两个文件也添加到assets pipeline的编译路径中。

```ruby
# config/initializers/assets.rb

# 在这段中的%w()中加入你的文件路径，以空格隔开
Rails.application.config.assets.precompile += %w( path/to/yourjsfile.js path/to/yourcssfile.css )
```

## 最后

这篇文章注重具体问题，讲述的都是在给rails做前端开发时可能会产生的疑惑，也没有太过深入的作解释。如果你对Ruby on Rails有兴趣，请务必试着学一下:)。


