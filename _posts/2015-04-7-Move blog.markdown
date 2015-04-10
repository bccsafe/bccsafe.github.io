---
layout: post
title: Blog搬家
categories:
- others
---

很久之前就计划着给Blog换个窝了，一直没能如愿，这次终于是下定决心了，从`虚拟主机 + WordPress` 换到了 `Github + jekyll`，总结下让我心动的因素

* Wp过于臃肿，已经不是当年的那个他了
* 在Wp上编辑文章很吃力，反观MarkDown格式的书写风格很适合我
* 原Blog太丑，而如今采用的主题[lanyon](https://github.com/poole/lanyon)简洁大气，可能也跟我的审美观变化有关系...
 
二年半个月不到的时间内，写了了不下八十篇文章，回望当初可能只是一时冲动，只想做点笔记或是什么。这些博文如今看来有些幼稚，但却承载着一段属于我的曾经，这一路走来不易，想想还是留了下来，删除了个别几篇，关于`TDcefBrowser`的会重新编辑后放出 
 
##导入原Blog文章
1. 在WordPress里导出xml格式的数据
2. 使用[Exitwp](https://github.com/thomasf/exitwp)将xml格式转换成markdown格式
3. 为每个分类目录名加上前缀`wp-`，方便之后做文章归档

##添加分类目录
即要保持之前的文章分类目录，又需要支持现有的，因此为之前的分类目录加上前缀`wp-`，在两个页面中判断即可

**注意请将下面的`\%`替换成`%`，这里为了防止jekyll解释[liquid](https://github.com/Shopify/liquid)标记语言，特别加了符号**

当前文章归档页面：

	{\% for category in	 site.categories \%}
	{\% if category[0] contains 'wp_' \%}
	{\% else \%}
	<h2>{{ category | first }}({{ category | last | size }})</h2> 
	<ul>
    	{\% for post in category.last \%}
        	<li>{{ post.date | date:"%Y/%m/%d"}}
        	<a 	href="{{ post.url }}">{{ post.title }}</a></li>
    {\% endfor \%}
	</ul>
	{\% endif \%}
	{\% endfor \%}

原Blog文章归档：

	{\% for category in site.categories \%}
	{\% if category[0] contains 'wp_' \%}
	<h2>{{ category | first }}({{ category | last | size }})</h2> 
	<ul>	
		{\% for post in category.last \%}     
			<li>{{ post.date | date:"%Y/%m/%d"}}
			<a href="{{ post.url }}">{{ post.title }}</a></li>    
		{\% endfor \%}
	</ul>
	{\% endif \%}
	{\% endfor \%}
	
##语法高亮
尝试了JS方法来实现语法高亮，发现对Delphi的支持并不是很好，或多或少都有问题，最后还是不得已采用了官方的`Pygments`方案，几乎很完美，缺点就是破坏了MarkDown格式，在需要语法高亮的代码前后得加上如下字符
	
	``` language
	Your Code
	```
	
##关于老Blog
不会更新老Blog了，新的学习笔记，想法，点点滴滴都会在这里记录下来，但可以继续用[wp.bccsafe.com](http://wp.bccsafe.com)访问，直到我的虚拟主机到期，应该还有2，3年，就挂着好了