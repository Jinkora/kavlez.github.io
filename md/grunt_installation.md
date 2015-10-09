发现周围有些人对前端存在偏见。  
他们认为前端只是用没那么复杂的技术对着界面调来调去，一点点打磨，最后做出一个没什么实用价值的“花瓶”。  
其实，前端的技术栈并不简单，比如我们可以用**Grunt**进行一些自动化操作。  
这里简单记录下Grunt的安装，希望对大家有帮助。



## Node.js
我们用到的很多组建都是依赖Node.js构建的，所以首先要安装Node.js。  
安装Node.js几乎没什么难度，可以直接进Node.js的[官网](https://nodejs.org/)，或者使用`brew`,`apt-get`,`yum`等进行安装。

安装结束后，执行一下命令，检查输出是否正确:  

	$ node --version
	v0.12.7
	
`npm`会跟着Node.js安装进来，我们需要经常执行`npm install`之类的操作。  
所以如果之前没有解除过`npm`，你可能需要过一次[Getting Started](https://docs.npmjs.com/)。



## Grunt

进入Grunt的[官网](http://gruntjs.com/)看到一头野猪，Grunt将自己定义为**Javascript Task Runner**，而我们通过Grunt执行的就是一个个**task**。  


先install再说，注意是**grunt-cli**

	npm install -g grunt-cli

看看是否成功安装

	Ezra:~ Kavlez$ grunt --version
	grunt-cli v0.1.13
	
<br/>
该怎么用起来呢？这里列出简单步骤。

* 创建一个目录，在这个目录下试试

		mkdir my-grunt
		cd my-grunt
		

* 执行`npm init`来创建**package.json**，会出现一些填写选项的提示，接着执行`npm install`来安装需要的组件即可，比如：

		npm install grunt-contrib-uglify --save-dev

			
* 执行`vim package.json`查看，会发现grunt-contrib-uglify呗加入到了配置文件中

		 "devDependencies": {
	      "grunt": "^0.4.5",
	      "grunt-contrib-uglify": "^0.9.2"
	    }
	    
	       
* package.json并不是用来配置任务的，我们要在**Gruntfile.js**中配置任务，比如这样：

		module.exports = function(grunt) {
		
		    grunt.initConfig({
		        pkg: grunt.file.readJSON('package.json'),
		        uglify: {
		            build: {
		                src: 'omg.js',
		                dest: 'omg.min.js'
		            }
		        }
		    });
		
		    grunt.loadNpmTasks('grunt-contrib-uglify');
		    grunt.registerTask('default', ['uglify']);
		
		};
		

	    
* [grunt-contrib-uglify](https://www.npmjs.com/package/grunt-contrib-uglify)可以用来压缩源文件，我们创建一个omg.js并写入以下内容来试试uglify:

		'use strict';

		function omg(){
			alert('omg!!!!!!');
		}
		
* 执行任务！ 如果顺利的话，我们可以在目录下看到omg.min.js。

		grunt uglify
		
		
<br>
仅靠一头野猪，我们仍无法避免一些重复的操作。  
于是我们需要别的小伙伴，比如一只鸟。

## Bower 

Bower就是那只鸟，我是说logo。  
[Bower](http://bower.io/)在官网做了如下介绍：
> Web sites are made of lots of things — frameworks, libraries, assets, utilities, and rainbows. Bower manages all these things for you.

也就是为我们管理各种各样的依赖，比如框架、库、工具等。  
<br>
安装方法依然简单，直接`npm`

	$ npm install -g bower

安装结束后，查看版本检查是否成功

	$ bower -v
	1.5.1

<br>
使用方法非常简单，这里简单举个例子，比如我们要在工程中引入一个backbone:

	$ bower install backbone

bower是如何根据名称找到backbone的?
我们可以到[http://bower.io/search/](http://bower.io/search/)看看。  
backbone是使用"backbone"这个名字在bower进行注册的。  


此外，我们还有以下几种方式引入依赖:

	# GitHub shorthand
	$ bower install desandro/masonry
	# Git endpoint
	$ bower install git://github.com/user/package.git
	# URL
	$ bower install http://example.com/script.js


<br>
你可能会有这样的疑问，**npm 和 bower都可以做依赖管理，那它们的区别是什么?**

这里，我引用下stackoverflow的答案:

> 
npm is most commonly used for managing Node.js modules, but it works for the front-end too when combined with Browserify and/or $ npm dedupe.

> Bower is created solely for the front-end and is optimized with that in mind. The biggest difference is that npm does nested dependency tree (size heavy) while Bower requires a flat dependency tree (puts the burden of dependency resolution on the user).

> A nested dependency tree means that your dependencies can have its own dependencies which can have their own, and so on. This is really great on the server where you don't have to care much about space and latency. It lets you not have to care about dependency conflicts as all your dependencies use e.g. their own version of Underscore. This obviously doesn't work that well on the front-end. Imagine a site having to download three copies of jQuery.

> The reason many projects use both is that they use Bower for front-end packages and npm for developer tools like Yeoman, Grunt, Gulp, JSHint, CoffeeScript, etc.

> All package managers have many downsides. You just have to pick which you can live with.

相信这段答案会在使用`npm`和`bower`过程中更有体会  :D  
答案中提到了**Yeoman**，这是干什么用的？


## Yeoman

[yeoman](http://yeoman.io/)整合了最佳实践和常用工具。  
创建项目时，我们可以用来生成项目文件、代码结构。  

直接使用`npm`直接进行安装，注意这里的名字是**yo**，而不是**yoeman**。

    $ npm install -g yo

可能并不顺利。  

<br>
看到有些朋友建议使用`sudo`执行安装，事实上这样会带来一些权限问题，比如一些模块会无法加载，或者每次执行一些命令时都需要加上`sudo`。  
我们可以参考 [https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md](https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md)来解决这个问题。

<br/>
在此简单引用下这个帖子的内容，过程如下：

	$ mkdir "${HOME}/.npm-packages"

在**.bashrc**(或者可能是**.bash_profile**)加入以下代码:

	NPM_PACKAGES="${HOME}/.npm-packages"
	NODE_PATH="$NPM_PACKAGES/lib/node_modules:$NODE_PATH"
	
	PATH="$NPM_PACKAGES/bin:$PATH"
	
	# 根据自己的情况设置manpath
	unset MANPATH
	MANPATH="$NPM_PACKAGES/share/man:$(manpath)"

<br/>
在**.npmrc**加入以下代码:

	prefix=~/.npm-packages

最后，不要忘了`source`:

	$ source ~/.bashrc

<br>
如果是使用**brew**安装node，可能会出现其他问题，比如:   
[http://yeoman.io/learning/faq.html#yo-command-not-found](yo-command-not-found)
[https://github.com/yeoman/yeoman/issues/466#issuecomment-8602733](yo-command-not-found)


如果出现提示：

	sh: yodoctor: command not found

可以执行以下命令解决:

	$ npm config set unsafe-perm true
<br>	
Yeoman有个东西叫**generator**，我们可以让Yeoman根据特定的generator生成相应的工程。
[http://yeoman.io/generators/](http://yeoman.io/generators/)中为我们列出了目前可用的generator。  

这里以列表中的第一条——**AngularJS**的generator为例，简单记录下使用步骤。

*	首先安装generator，列表中AngularJS的generator名称为**angular**，在名称前面加上**generator_**便是在npm中注册的名称。

		npm install -g generator-angular

*  为自己的工程创建一个目录并进入。

		mkdir yo-my-angular

* 执行一下`yo`，开始生成。

		yo angular yo-my-angular
		
* Yeoman会提示以下一些问题，根据需要选择即可，剩下的便是稍等片刻。

		
		? Would you like to use Sass (with Compass)? Yes
		? Would you like to include Bootstrap? Yes
		? Would you like to use the Sass version of Bootstrap? Yes
		? Which modules would you like to include? (Press <space> to select)
		
* 如果执行正常，Yeoman最后会提示**Done, without errors.**		
				
<br>
当然，我们可以忽略Yeoman，而直接使用Grunt，并一点点编写相关任务的配置。  
我们可能只是需要一些简单的功能，但Yeoman提供的**最佳实践**会给我们带来更多的启发，谁会跟更好的方法过不去呢？
