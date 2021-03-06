#项目脚手架

##grunt-init

Grunt-init是一个被用于自动创建项目的脚手架工具. 它将基于当前环境构建一个完整的目录结构并解决一些问题. 依赖于模板的选择以及解决所遇到的问题来创建确切的文件和内容.

注意: 这个独立实用的程序用于Grunt内置'init'任务. 在[从0.3升级到0.4](https://github.com/gruntjs/grunt/wiki/Upgrading-from-0.3-to-0.4)指南中可以查看更多关于它变化的信息.

##安装

以便使用grunt-init, 你应该在全局安装它

    npm install -g grunt-init

这将把`grunt-init`命令安置到你的系统路径, 并允许从任何地方运行它.

*注意: 你可能需要使用sudo或者作为超级管理员运行shell命令执行此操作.* 

##用法

+ 使用`grunt-init --help`来获取程序帮助以及列出可用模板清单.   
+ 使用`grunt-init TEMPLATE`基于可用模板创建一个项目
+ 使用`grunt-init /path/to/TEMPLATE`基于任意位置的模板创建一个项目

注意大多数的模板都在当前目录中生成它们的文件, 因此,如果你不想覆盖现有的文件, 那么一定要定改变到一个新目录中.

##安装模板

一旦模板被安装到你的`~/.grunt-init/`目录中(在Window上是`%USERPROFILE%\.grunt-init\`), 那么它们将可以通过grunt-init使用. 建议你使用git将模板克隆到目录中. 例如, [grunt-init-jquery](https://github.com/gruntjs/grunt-init-jquery)模板可以这样安装:

    git clone git@github.com:gruntjs/grunt-init-jquery.git ~/.grunt-init/jquery

*注意: 如果你向往是模板在本地像"foobarbaz"这样使用, 你应该指定`~/.grunt-init/foobarbaz`之后再克隆. Grunt-init将使用实际的模板目录名存在于它的`~/.grunt-init/`目录中.*

一些Grunt官方维护的模板:

+ [grunt-init-commonjs](https://github.com/gruntjs/grunt-init-commonjs) - 创建一个包含Nodeunit单元测试的commonjs模块. ([sample "generated" repo](https://github.com/gruntjs/grunt-init-commonjs-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-commonjs-sample#project-creation-transcript))

+ [grunt-init-gruntfile](https://github.com/gruntjs/grunt-init-gruntfile) - 创建一个基础的Gruntfile. ([sample "generated" repo](https://github.com/gruntjs/grunt-init-gruntfile-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-gruntfile-sample#project-creation-transcript))

+ [grunt-init-gruntplugin](https://github.com/gruntjs/grunt-init-gruntplugin) - 创建一个包含Nodeunit单元测试的Grunt插件. ([sample "generated" repo](https://github.com/gruntjs/grunt-init-gruntplugin-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-gruntplugin-sample#project-creation-transcript))

+ [grunt-init-jquery](https://github.com/gruntjs/grunt-init-jquery) - 创建一个包含QUnit单元测试的jQuery插件. ([sample "generated" repo](https://github.com/gruntjs/grunt-init-jquery-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-jquery-sample#project-creation-transcript))

+ [grunt-init-node](https://github.com/gruntjs/grunt-init-node) - 创建一个包含Nodeunit单元测试的Node.js模块. ([sample "generated" repo](https://github.com/gruntjs/grunt-init-node-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-node-sample#project-creation-transcript))

##自定义模板

你可以创建和使用自定义模板. 你的模板必须遵循上述模板相同的结构.

一个名为`my-template`的模板样本要遵循一下常规文件结构:

+ `my-template/template.js` - 主模板文件.
+ `my-template/rename.json` - 用于处理模板的, 模板特定重命名规则.
+ `my-template/root/` - 复制文件的目标位置.

假设这些文件存在于`/path/to/my-template`中, 那么命令`grunt-init /path/to/my-template`将用于处理模板. 这个目录中应该存在多个命名唯一的模板.

此外, 如果你把这个自定义模板放在你的`~/.grunt-init/`目录中(在Windows上是`%USERPROFILE%\.grunt-init\`), 那么只使用`grunt-init my-template`命令它将自动可用.

###复制文件

运行初始化模板时, 只要模板使用`init.filesToCopy`和`init.copyAndProcess`方法, 任何位于`root/`子目录中的文件都将被复制到当前目录.

注意所有被复制的文件都将被处理为模板, 任何`{% %}`模板都会被处理为`props`数据对象集, 除非设置了`noProcess`选项. 来看看[jquery template](https://github.com/gruntjs/grunt-init-jquery)的例子.

###重命名或者排除模板文件

`rename.json`用于描述`sourcepath`到`destpath`的重命名映射. `sourcepath`必须是相对于`root/`目录的文件要被复制的路径, 但是`destpath`值可以包含`{% %}`模板, 用于描述目标路径是什么.

如果`destpath`被指定为`false`, 那么文件将不会被复制. 此外, `srcpath`还支持模式匹配.

##指定默认提示信息

每个初始化提示都有一个硬编码的默认值或者查找当前环境以尝试确定该缺省值. 如果你想覆盖一个特定提示信息的默认值, 你可以在OS X或者Linux的`~/.grunt-init/defaults.json`或者Windows的`%USERPROFILE%\.grunt-init\defaults.json`文件中选择性的做到这些.

例如, 由于我希望使用一个与众不同的名字来替代默认的名字, 并希望排除我的邮箱地址, 同时我还希望自动指定一个作者的url, 我的`defaults.json`看起来是这样的.

    {
        "author_name": "\"Cowboy\" Ben Alman",
        "author_email": "none",
        "author_url": "http://benalman.com/"
    }    

*注意: 即使记录了所有的内置提示信息, 你可以在[源代码](https://github.com/gruntjs/grunt-init/blob/master/tasks/init.js)中找到他们的名字和默认值.*

##定义一个初始化模板

###exports.description

当用户运行`grunt init`或者`grunt-init`来显示所有的有效初始化模板时, 这个剪短的模板描述的模板名称也会随着一起显示.

    exports.description = descriptionString;
        
###exports.notes

如果指定了, 这个可选的扩展描述将会在任何提示信息显示之前显示出来. 这是给用户提供一些解释命名约定帮助的很好的位置, 这些提示可能时必选的也可能是可选的, 等等.

    exports.notes = noteString;

###exports.warnOn

如果这个(推荐指定)可选的通配模式或者通配模式数组匹配了, Grunt将终止并导致一个警告信息, 用户可以使用`--force`来覆盖这个默认行为. 这对于初始化模板可能覆盖现有文件的情况来说是非常有用的.

    exports.warnOn = wildcardPattern;
    
然而最常见的值是`'*'`, 匹配任意文件或者目录. 使用[minimatch](https://github.com/isaacs/minimatch)通配符模式具有很大的灵活性. 例如:

    exports.warnOn = 'Gruntfile.js'; // 警告Gruntfile.js文件.
    exports.warnOn = '*.js'; //警告任意.js文件
    exports.warnOn = '*'; //警告任意非点文件或目录
    exports.warnOn = '.*'; //警告任意点文件或目录
    exports.warnOn = '{.*, *}'; //警告任意文件或目录
    exports.warnOn = '!*/**'; //警告任意目录中的文件
    exports.warnOn = '*.{png,gif,jpg}'; //警告任意图片文件
    //最后一个例子的另一种形式
    exports.warnOn = ['*.png','*.gif','*.jpg'];

###exports.template

虽然`exports`属性定义在该函数的外面, 而所实际的初始化代码指定在内部. 这个函数接收传递三个参数, `grunt`参数是一个grunt的引用, 包含所有的[grunt方法和库](http://gruntjs.com/api/grunt). `init`参数是一个包含指定给初始化模板的方法和属性的对象. `done`参数是当初始化模板执行完成时必须调用的函数.

    exports.template = function(grunt, init, done){
        //查看"Inside an init template"一节
    };

##初始化模板的内部

###init.addLicenseFiles

给files对象添加适当命名的许可协议证书文件

    var files = {};
    var licenses = ['MIT'];
    init.addLicenseFiles(files, licenses);
    // files === {'LICENSE-MIT': 'licenses/LICENSE-MIT'}    

###init.availableLicenses

返回一个可用许可协议证书的数组

    var licenses = init.availableLicenses();
    // licenses = ['Apache-2.0', 'GPL-2.0', 'MIT', 'MPL-2.0']    

###init.copy

给定的绝对或者相对源文件路径, 和一个可选的相对目标文件路径, 复制一个文件时, 通过传递的回调函数可选的处理它.

    init.copy(srcpath[, destpath], options);

###init.copyAndProcess

遍历所传递对象中的所有文件, 将源文件复制到目标路径, 并处理内容.

    init.copyAndProcess(files, props[, options]);

###init.defaults

用户在`defaults.json`中指定的默认初始值.

    init.defaults    

###init.destpath

目标文件绝对路径.

    init.destpath();    

###init.expand

与[grunt.file.expand](https://github.com/gruntjs/grunt/wiki/grunt.file#wiki-grunt-file-expand)相同.

返回一个独一无二的给定通配模式所匹配的所有文件或目录路径数组. 这个方法接收都好分割的通配符模式或者数组形式的通配符模式. 如果路径匹配模式以`!`开始, 将被排除在返回数组之外. 模式是按顺序处理的, 所以包含和排除的顺序是很明显的.

    init.expand([options, ] patterns);

###init.filesToCopy

返回一个包含要复制文件的源文件绝对路径和相对的目标路径的对象, 并按照`rename.json`(如果存在)中的规则重命名(或者省略).

    var files = init.filesToCopy(props);
    /* files === { '.gitignore': 'template/root/.gitignore,
       'jshintrc': 'template/root/.jshintrc',
       'Gruntfile.js': 'template/root/Gruntfile.js',
       'README.md': 'template/root/README.md',
       'test/test_test.js': 'template/root/test/name_test.js'
       } */    

###init.getFile

获取单一的任务文件路径.

    init.getFile(filepath[, ...]);

###init.getTemplates

返回一个包含所有可用模板的对象.

    init.getTemplates()

###init.initSearchDirs

在初始化目录中搜索初始化模板. `template`是模板的地址. 还将包括`~/.grunt-init`和grunt-init中的核心初始化任务.

    init.initSearchDirs([filename]);

###init.process

启动程序, 提示开始输入.

    init.process(options, prompts, done);
    
    init.process({}, [
        //Prompt for these values
        init.prompt('name'),
        init.prompt('description'),
        init.prompt('version')
    ], function(err, props){
        // All finished, do something with the properties
    });

###init.prompt

给用户一个提示值

    init.prompt(name[, default])

###init.prompts

所有提示信息对象

    var prompts = init.prompts();

###init.readDefaults

读取任务文件(如果存在)中JSON默认值, 并将它们合并到一个数据对象.

    init.readDefaults(filepath[, ...])

###init.renames

模板的重命名规则

    var renames = init.renames;
    //renames = {'test/name_test.js': 'test/{%= name%}_test.js' }

###init.searchDirs

搜索模板的目录数组

    var dirs = init.searchDirs;
    /* dirs = ['/Users/shama/.grunt-init',
    '/usr/local/lib/node_modules/grunt-init/templates' ] */

###init.srcpath

根据文件名搜索初始化模板路径并返回一个绝对路径

    init.srcpath(filepath[, ...]);

###init.userDir

返回用户模板目录的绝对路径

    var dir = init.userDir();
    //dir === '/Users/shama/.grunt-init'

###init.writePackageJSON

在目标目录保存一个`package.json`文件. 回调函数可以用于后置处理属性的添加/移除/其他操作.

    init.writePackageJSON(filename, props[, callback]);
    
##内置提示

###author_email

用于`package.json`中的作者邮箱地址. 会尝试从用户的git配置中找到一个默认值.

###author_name

用于`package.json`中的作者全名和版权信息. 会尝试从用户的git配置中找到一个默认值.

###author_url

用于`package.json`中公开作者网站的URL.

###bin

项目跟目录中cli脚本的相对路径.

###bugs

用于项目问题跟踪的公开URL. 如果项目有一个Github仓库, 将自动指向项目Github的问题跟踪模块.

###description

项目的描述. 用于`package.json`或者README文件中.

###grunt_version

项目必需的有效Grunt版本范围描述符.

###homepage

指向项目首页的公开URL. 如果是Github仓库将默认指向Github的url.

###jquery_version

如果是jQuery项目, 项目必须的jQuery版本. 必须是一个有效的版本范围描述器.

###licenses

项目许可协议证书. 多个许可协议证书使用空格分割. 内置的许可协议有: `MIT`, `MPL-2.0`, `GPL-2.0`和`Apache-2.0`. 可以使用[init.addLicenseFiles](http://gruntjs.com/#initaddlicensefiles)添加自定义许可协议证书.

###main

项目主入口点. 默认是`lib`目录中的项目名称.

###name

项目名称. 在项目模版中将会大量使用, 默认指向当前工作目录.

###node_version

项目必需的Node.js版本. 必须是一个有效的版本范围描述符.

###npm_test

项目中运行测试的命令. 默认是`grunt`.

###repository

项目的git仓库. 默认猜测为一个github上的url.

###title

一个人类可读的项目名称. 默认是改变过的更多人可读的实际项目名称.

###version

项目的版本. 默认是第一个有效的版本, `0.1.0`.
    