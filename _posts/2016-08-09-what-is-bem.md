---
layout: post
title:  "BEM 与模块化开发"
date:   2016-08-09 20:00
#categories: 前端 模块化
---


>BEM是一种html文档class属性（类名）的`命名模式`，也是一种模快化开发的最佳实践。通常一个页面可以划分为多个彼此独立或嵌套的区块，我们称之为组件。这种逻辑和功能上独立的功能块，封装行为`（JS实现）`，`模板（HTML）`和`样式（CSS）`以及其他实现技术。组件化的目标是保持组件的独立性和复用性,是面向对象思想的体现。

本文翻译自 [bem.info](https://en.bem.info/methodology/key-concepts/)

### Block（块）

Block指页面上一个逻辑上或功能上独立的组件，等同于一个[web Components](http://sentsin.com/web/1089.html)。Block封装了组件的`行为（JS）`，`模板（HTML）`和`样式（CSS）`等其他与组件实现相关的技术。Block的独立性使其能够被复用，从而减少不必要的代码书写，促进项目的有序进行。

<!--more-->

### Block Features（特点）

#### 嵌套结构

Block能嵌套使用，比如，在下图所示中，html文档的`head block`包含了`logo block`、`menu block`、`search block`和`auth block`。

![块的嵌套](https://en.bem.info/bT4kI2nbzXYfpj7hGbSGzdguTgE.png)

#### 位置无关

Block能够在页面内、页面之间甚至项目之间进行移动，block的独立性特点决定了这种移动不会改变其行为和外表。
比如，互换`logo block`和`auth block`的位置只需改变block在文档中的顺序即可，而不需要修改js和css。

![块的移动](https://en.bem.info/7TtrK0Xj7AR7zLHeV7rDQJF8jkI.png)

### 能被复用

一个页面内可以包含多个`block`实例。

![块在页面内复用](https://en.bem.info/c-k0hGArGcvClQjQmt8J2uLwVqU.png)

### Element(元素)

一个`Element`是`block`的组成部分，不能脱离`block`单独使用。

比如，一个菜单项不能够脱离菜单这个上下文，因此是一个`element`。

![elements](https://en.bem.info/kTs-O0UleMeD8J2I1DPm_KtS8_o.png)

### Modifier

`Modifier`能够使用在`Block`和`Element`两种`BEM entity`(实体)之上。

它是可选的。

`modifier`和`html属性`类似，能够为相同的`block`或`element`赋予不的样式。

比如，通过使用`modifier`，可以使页头和页脚功能相同的导航菜单呈现不同的表现形式。

![modifier](https://en.bem.info/ZXEdAN-ZAJfX2yv4BwpqUZSY5R0.png)

### BEM entity（实体）

`blocks`,`elements`和`modifiers`被泛称作 `bem entity`。

`bem entity` 既可以指代具体的某个`block`、`element`、`modifiers`，也可以泛指此三者。

### entity mix（实体混入）

指一个`dom`节点挂载多个`bem entity` 特征。

实体混入能够做到：

* 在多个 `bem entities` 间共享行为和样式，而不必重写代码。
* 整合多个 `bem entities` 形成一个更大的语义化的 `bem entity`。

现在考虑一下如何混入一个`block`和另一个block的`element`。

假设存在这样的需求：项目中已经存在`link block`的实现，而现在需要将`menu block`的 `link element` 格式化为`link block`。下面是解决方案。

* 方案一：为 `menu block`的 `link element` 添加 `modifiers`，实现该 modifiers，就必须要复制 `link block` 的样式和行为代码。
* 方案二：在`menu block` 的 `link element` 中混入`link block`  ，从而使得`menu block` 的 `link element` 具备`link block`的行为和外观

### BEM tree(树)

指从`blocks`、`elements`、`modifiers`层面描述web页面结构的一种手段。`bem tree`是`DOM tree`的一种抽象 ，描述了`bem entities`的名字以及它们的状态、顺序、嵌套关系和其他辅助内容。

在真实的项目中，`bem tree`能够通过多种格式化方法来描述其结构。

考虑这样一个`dom tree`：

> 本文认为原文中存在错误，按照bem中的定义，search-form中的input应该是element，而不是block,故在翻译中做了修正，如有不当，请自行参照原文分析。


    <header class="header">
    	<img class="logo">
    	<form class="search-form">
        	<input type="input" class="search-form__kwd">
        	<button type="button" class="class="search-form__submit""></button>
    	</form>
    	<ul class="lang-switcher">
        	<li class="lang-switcher__item">
            	<a class="lang-switcher__link" href="url">en</a>
        	</li>
        	<li class="lang-switcher__item">
            	<a class="lang-switcher__link" href="url">ru</a>
        	</li>
    	</ul>
	</header>

对应的`bem tree`看起来是这样的：

    header
        |--logo
        |--search-form
            |--search-form__kwd
			|--search-form__submit
        |--lang-switcher
            |--lang-switcher__item
				|--lang-switcher__link
			|--lang-switcher__item
				|--lang-switcher__link

同样，`bem tree`的`XML`和`BEMJSON`格式描述如下：

#### XML

	<block:header>
		<block:logo/>
		<block:search-form>
			<ele:kwd/>
			<ele:submit/>
		</block:search-form>
		<block:lang-switcher>
			<ele:item>
				<ele:link/>
			</ele:item>
			<ele:item>
				<ele:link/>
			</ele:item>
		</block:lang-switcher>
	</block:header>


#### BEMJSON

    {
        "block": "header",
        "content": [{
            "block": "logo"
        }, {
            "block": "search-form",
            "content": [{
                "ele": "kwd"
            }, {
                "ele": "submit"
            }]
        }, {
            "block": "lang-switcher",
            "content": [{
                "ele": "item",
                "content": [{
                    "ele": "link"
                }]
            }, {
                "ele": "item",
                "content": [{
                    "ele": "link"
                }]
            }]
        }]
    }

### BEM Implementation(实现)

Bem entity 的实现涵盖如下几个方面：

* `behavior`(行为)
* `appearance`(外观)
* `tests`(测试)
* `templates`(模板)
* `documentation`(文档)
* `description of dependencies`(依赖)
* `additional data` (e.g., images)(其他数据，比如图片)

### Implementation technology(实现所需技术)

实现Bem需要通过如下一些技术：

* behavior — `JavaScript`, `CoffeeScript`
* appearance — `CSS`, `Stylus`, `Sass`
* templates — `BEMHTML`, `BH`, `Pug`, `Handlebars`, `XSL`
* documentation — `Markdown`, `Wiki`, `XML`.

比如，如果 `appearance`(外观)是通过 `css` 来编写的，那就意味着 `Bem` 采用了 `CSS` 技术。同样，如果 `documentation`(文档)是用 `markdown` 写的，那就是采用了的 `markdown` 技术。

### redefinition(块的重定义)

指按不同级别添加新的功能，来最终实现一个block。

### Redefinition level(重定义级别)

指一组Bem entities和它们的部分实现

block的可以分成多个级别来最终实现完整功能。每一个级别都可以通过扩展或覆盖原来的实现方法来实现新的功能。采用这种策略，block能够按照预定的顺序执行每一个独立级别中所采用的技术来形成自身的功能。

![块的级别](https://en.bem.info/Apy7W6xrYqpmppQybAjSPJLmips.png)

实现Bem的每一种技术都可以被重定义。

比如，项目通过引入库文件实现一个块，作为一个独立的层，引入的库提供了预定义的block实现。而项目级别的block则在另一个层次被定义。

如果根据项目需求，要改变外部库预定义block的外观，我们并不需要改变库文件中的css规则或者复制库级别定义的规则到项目级别定义的规则中去，只需要在项目级别添加新的css规则。通过构建，block继承了库级别的定义，也表现出了项目级别定义的样式规则。
