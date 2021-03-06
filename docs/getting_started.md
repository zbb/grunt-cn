#新手上路

Grunt和grunt插件都是通过[npm](https://npmjs.org/), [Node.js](http://nodejs.org/
)包管理器安装和管理的.

这个说明是针对Grunt 0.4.x编写的, 但是在Grunt 0.3.x中仍然有效.只是注意在0.3.x版本中, 插件名和任务配置选项可能不同于'Gruntfile'一节中所展示的那样.

Grunt 0.4.x需要0.8.0及以上版本的Node.js支持.

##安装CLI

**如果你已经在之前全局的安装了Grunt, 首先你需要移除它:**

	npm uninstall -g grunt
	
便于开始, 你可能希望全局安装Grunt的命令行界面(CLI). 你需要使用sudo(OSX, *nix, BSD等)权限或者作为超级管理员(Windows)运行shell命令做到这一点.

	npm install -g grunt cli
	
这将会把`grunt`命令置入你的系统路径中, 并允许你在任何目录中运行它.

注意,安装`grunt-cli`时并没有安装grunt任务运行器! grunt CLI的工作很简单: 运行已安装版本的grunt的`Gruntfile`. 这意味着允许在一台机器上同时安装多个不同版本的Grunt.

##CLI如何工作的

当每次运行`grunt`时, 它都会使用nodejs的`require()`系统查找安装在本地的grunt. 正因为如此, 你可以在你项目的任何子目录中运行`grunt`命令.

如果发现在本地已经安装好的Grunt, CLI就会加载本地安装的grunt库, 然后从你的	`Gruntfile`调用配置, 在它运行时执行你任意的任务请求.

想要真正了解它发生了什么,请[阅读代码](https://github.com/gruntjs/grunt-cli/blob/master/bin/grunt). 这个代码很短.

##使用一个现有的grunt项目进行工作

假设你已经安装了Grunt CLI, 并且项目已经配置在`package.json`和`Gruntfile`中, 使用Grunt工作是很容易的.

1. 定位到项目的根目录.
2. 使用`npm install`安装项目依赖.
3. 使用`grunt`运行Grunt.

这是真的. 安装好的Grunt任务可以通过运行`grunt -help`列出来, 并且以此作为一个项目文档的开始是一个好主意.

##准备一个新的Grunt项目

典型的设置将涉及到添加两个文件到你的项目中: `package.json`和`Gruntfile`.

**package.json**:这个文件被用于通过npm存储发布项目的元素据作为npm模块, 你将在这个文件中列出你项目需要依赖的grunt及grunt插件.

**Gruntfile**:这个文件名为`Gruntfile.js`或者`Gruntfile.coffee`, 它被用来配置或者定义任务以及加载Grunt插件.

这个文件在Grunt 0.3.x版本中被命名为`grunt.js`.

###package.json

`package.json`文件归属于你项目的根目录中, 与`Gruntfile`相邻, 并且应该与你的项目源文件一同被提交. 在同一目录中运行`npm install`时会按照`package.json`文件中列出每一个依赖安装正确的版本.

这里有一些方法在你的项目中创建一个`package.json`文件:

+ 大多数的[grunt-init](http://gruntjs.com/project-scaffolding)模板都会自动创建一个项目特定的`package.json`文件.
+ [npm init](https://npmjs.org/doc/init.html)命令也会创建一个基本的`package.json`文件.
+ 从下面的例子开始, 按需扩展. 但是你要遵循[规范](https://npmjs.org/doc/json.html)…

-----------

    {
        "name": "my-project-name",
        "version": "0.1.0",
        "devDependencies": {
            "grunt": "~0.4.1",
            "grunt-contrib-jshint": "~0.1.1",
            "grunt-contrib-nodeunit": "~0.1.2"
        }
    }
    
##安装Grunt和grunt插件

最简单的方式添加Grunt和Grunt插件到现有的`package.json`中就是使用`npm install <module> --save-dev`命令.这样不仅仅能在本地安装`<module>`, 它还会自动被添加到[依赖](https://npmjs.org/doc/json.html#devDependencies)部分,使用一个[波浪线版本范围](https://npmjs.org/doc/json.html#Tilde-Version-Ranges)

例如, 这将在你的项目目录中安装最新版本的Grunt, 并添加到你的依赖中:

    npm install grunt --save-dev
    
同样的方式可以用来安装grunt插件和其他的node模块. 当你完成之后, 更新过的`package.json`一定要和你的项目一起提交.

##Gruntfile


`Gruntfile.js`或者`Gruntfile.coffee`是归属于你项目跟目录中的一个有效的JavaScript或者CoffeeScript文件, 与`package.json`相邻, 并且应该和你的项目源文件一起被提交.这个文件在Grunt 0.3.x版本中被命名为`grunt.js`.

一个Gruntfile有以下几部分组成:

+ 'wrapper'函数
+ 项目和任务配置
+ Grunt插件和任务加载
+ 定制任务

###一个Gruntfile示例

在下面的Gruntfile中, 项目的元数据从项目的`package.json`文件中被导入到grunt配置中, [grunt-contrib-uglify](http://github.com/gruntjs/grunt-contrib-uglify)插件的`uglify`任务被配置用于压缩一个源代码文件, 并使用该元素据动态的生成一个标语注释. 当在命令行中运行grunt时, 默认的情况下会运行`uglify`任务.

    module.exports = function(grunt){
        
        //项目配置
        grunt.initConfig({
            pkg: grunt.file.readJSON('package.json'),
            uglify: {
                options: {
                    banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
                },
                build: {
                    src: 'src/<%= pkg.name %>.js',
                    dest: 'build/<%= pkg.name %>.min.js'
                }
            }
        });
        //加载提供"uglify"任务的插件
        grunt.loadNpmTask('grunt-contrib-uglify');
        
        //默认任务
        grunt.registerTask('default', ['uglify']);
        
    };
    
现在你已经了解到一个整体的Gruntfile, 让我们来看看它的组成部分.

###"wrapper"函数

每个Gruntfile(以及grunt插件)都使用这个基本的格式, 并且你所有的Grunt代码都必须指定在这个函数里面:

    module.exports = function(grunt){
        //在这里处理grunt相关的事情
    };
    
###项目和任务配置

大多数的Grunt任务依赖于将配置数据定义在一个对象中,并传递给[grunt.initConfig](http://gruntjs.com/grunt#grunt.initconfig)方法.

在这个例子中, `grunt.file.readJSON('package.json')`用于将存储在`package.json`中的JSON元数据导入到grunt配置中. 由于`<% %>`模板字符串可以引用任意的配置属性, 如文件路径配置数据, 文件列表可以指定这种方式用于减少重复.

你可以在配置对象内部存储任意数据, 但它不能与你任务所需要的属性冲突, 否则它将被忽略. 此外, 由于这本质上是JavaScript代码, 你并不限于使用JSON; 你可以在这里使用任意有效的JS. 如果有必要, 你甚至可以以编程的方式生成配置.

跟大多数任务一样, [grunt-contrib-uglify](http://github.com/gruntjs/grunt-contrib-uglify)插件的`uglify`任务认为它的配置数据被指定在与它同名的属性中. 这里指定了一个`banner`选项, 紧接着名为`build`的单一uglify目标用于压缩一个独立的源代码文件到一个独立的目标文件.

    //项目配置
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        uglify: {
            options: {
                banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
            },
            build: {
                src: 'src/<%= pkg.name %>.js',
                dest: 'build/<%= pkg.name %>.min.js'
            }
        }
    });
    
###加载grunt插件和任务

很多常用的任务,如[concatenation](https://github.com/gruntjs/grunt-contrib-concat), [minification](http://github.com/gruntjs/grunt-contrib-uglify)以及[linting](https://github.com/gruntjs/grunt-contrib-jshint)都是可用的[grunt插件](https://github.com/gruntjs). 只要一个插件作为依赖被指定在`package.json`文件中, 并且已经通过`npm install`安装好了, 那么就可以在你的`Gruntfile`内使用一个简单的命令来启用它:

    //加载提供"uglify"任务的插件
    grunt.loadNpmTasks('grunt-contrib-uglify');
    
***注意*: 使用`grunt --help`命令可以列出所有可用的任务列表.

###定制/自定义任务

默认的情况下你可以通过定义一个`default`任务去配置Grunt运行一个或者多个任务. 在下面的例子中, 在命令行中运行没有指定任务的`grunt`将自动运行`uglify`任务. 这与明确运行`grunt uglify`或者等价的`grunt default`的功能一样. 可以在数组中指定任意数量的任务(带或不带参数).

    //默认任务
    grunt.registerTask('default', ['uglify']);
    
如果你的任务需求不需要通过Grunt插件来提供功能, 你可以在`Gruntfile`内部定义自定义任务. 例如, 这个Gruntfile定义了一个完整的自定义的`default`任务, 甚至它并没有利用任务配置:

    module.exports = function(grunt){
        
        //一个非常基本的默认任务
        grunt.registerTask('default', 'Log some stuff.', function(){
            grunt.log.write('Logging some stuff…').ok();
        });
    };

自定义的项目特定任务并不一定需要定义在Gruntfile内部; 他们也可以定义在外部的`.js`文件中, 并且能够通过[grunt的grunt.loadTasks](http://gruntjs.com/grunt#grunt.loadtasks)方法来加载. 

##扩展阅读

+ [安装Grunt](http://gruntjs.com/installing-grunt/)指南中有关于安装特定的, 发布的或者开发中版本的Grunt和Grunt-cli的详细信息.

+ [任务配置](http://gruntjs.com/configuring-tasks/)指南深入讲解了如何在Gruntfile文件内部去配置任务, 目标和选项, 紧接着讲解了模板, 匹配模式和导入外部数据.

+ [创建任务](http://gruntjs.com/creating-tasks/)指南列出了不同类型Grunt任务之间的差异, 并展示了一些简单的任务和配置.

+ 在[开发者文档中](http://gruntjs.com/grunt)可以检出更多的关于编写自定义任务和Grunt插件的信息.

