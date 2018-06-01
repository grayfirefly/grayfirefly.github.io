---
title: 创建一个Vue项目
---

### 安装NPM
由于NPM是随同NodeJS一起安装的包管理工具，而大部分新版本的nodejs都集成了NPM，因此可以去[nodejs官网](http://nodejs.cn/)下载最新的版本进行安装。本人安装的是Windows版本，因此需要配置环境变量。以下是熟悉的套路：

    此电脑-右键属性-高级系统设置-环境变量
    
	1.新建HOME变量

		NODE_HOME：D:\Program Files\nodejs // 此处为软件安装目录
	
	2.将变量加入到Path变量下
		
		Path：%NODE_HOME%

	3.配置完毕后，windows + R 输入cmd打开Dos窗口，输入如下命令：
	
		npm -v
本机执行结果如下：
	
	C:\Users\Hasee>npm -v
	5.6.0

	C:\Users\Hasee>


如果不能正常显示出版本，则检查软件是否安装正确及环境变量是否配置正确。

### 配置全局环境变量，
#### 创建全局安装目录
在NPM的后续使用中，可能会遇到各种的命令找不到的问题，需要再配置各种各样的环境变量，为了防止这种问题的发生，进行全局变量的配置。打开Dos窗口，由于我的nodejs的安装目录为D:\Program Files\nodejs，执行的时候要填对目录，输入以下命令：

	npm config set prefix "D:\Program Files\nodejs\node_global"
    
	npm config set cache "D:\Program Files\nodejs\node_cache"
	
这样就可以把后面用到的工具全局安装的装到node_global里并且可以只需要配置一次环境变量就可以直接用命令使用，例如后续用到的vue和webpack。
#### 配置全局环境变量
执行完命令之后，可以进行环境变量的修改，这样就可以保证以后利用npm安装的一些工具可以不用在累计配置环境变量，因上述已经添加过，NODE_HOME不变，修改Path，如下：
	
	Path:%NODE_HOME%\;%NODE_HOME%\node_modules;%NODE_HOME%\node_global;

#### 安装Vue
本次安装采用NPM安装,打开Dos窗口（以下全在Dos窗口中执行，不再详细叙述），因为npm在国内下载速度很慢，正如maven远程仓库一样，访问国外的资源很慢，这里采用taobao镜像，执行完下列命令后，可以用cnpm来代替npm命令。
	
	npm install -g cnpm –registry=https://registry.npm.taobao.org

下面开始上正菜，上一步可有可无，如果运气好，网速好的可以忽略上步...

安装vue-cli脚手架,执行这一步，可以顺带安装vue,一举两得,npm和cnpm二选一
	
	(npm)cnpm install -g vue-cli   

安装webpack，这里进行全局安装即可,至于webpack是什么玩意，可以理解为打包工具，想要了解更深，请访问[https://webpack.js.org/](https://webpack.js.org/)。

	// 全局安装
	(npm)cnpm install -g webpack

	// 安装到你的项目目录，本次不执行
	(npm)cnpm install --save-dev webpack

	// 查看vue安装结果,如果
	vue -V 

由于安装之后，在查看webpack的版本时候，根据提示需要安装webpack-cli，可根据提示进行输入命令。也可以退出直接安装webpack-cli，命令如下：

	(npm)cnpm install -D webpack-cli

输入以下命令查看是否安装成功
	
	// 查看webpack
	webpack -v

### 创建vue项目
#### 初始化项目
在Dos窗口下，由于本机每次创建项目都卡死，有可能版本不稳定导致的，或者其他原因。因此，使用了gitBash窗口，以下都是，不在累述，进入工作空间（凭自己喜好创建），输入以下命令：
	
	$vue init webpack vue-demo

根据提示输入创建项目的信息，本着能躺着绝不坐着的习性，一路回车。创建成功后，可以到项目的工作空间查看项目的情况，它会自动创建一坨vue项目的文件。

#### 运行项目

	// 进入项目目录
	$cd vue_demo

	// 运行项目
	$cnpm run dev

在浏览器地址栏输入上述运行的结果，例如，我这里是http://localhost:8080，查看结果。
