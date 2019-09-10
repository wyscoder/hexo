---
title: HEXO搭建博客（搭建在github上面）
date: 2019-08-10 15:23:04
tags: 博客
categories: 博客
---
## 前言

学长给的几个教程链接

 https://blog.csdn.net/u013332124/article/details/80680156 

https://hexo.io/zh-cn/docs/ 

http://theme-next.iissnan.com/getting-started.html#validate-next-theme 

 https://blog.csdn.net/llmmll08/article/details/70246150 

## 1.什么是hexo

​	Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。 

## 2.安装

### 1.前提

​	必须有Node.js 和 Git, 还有Github账号，都具备之后就开始了

### 2.创建github pages页面

1. 首先创建仓库（repository ），然后仓库名字必须得是 yourname.github.io 这个，比如我的github名称是wyscoder，所以仓库名称就是 wyscoder.github.io
2. 然后记住这个仓库的git地址 https://github.com/wyscoder/wyscoder.github.io

### 3.安装hexo

1. 这个是需要node.js和git作为前置的，如果没有就去下载node.js和git
2. 接下来就只需要npm就可以完成安装了
3. 使用这个命令进行安装hexo
 `$ npm install -g hexo-cli`
4. 等待下载完毕就行

### 4. 建站

1. 安装hexo完成之后，执行下面命令就会在指定文件夹创建需要的文件

   ```
   node.js
   $ hexo init <folder>
   $ cd <folder>
   $ npm install
   ```

   新建完成后，指定文件夹的目录如下： 

   ```
   .
   ├── _config.yml
   ├── package.json
   ├── scaffolds
   ├── source
   |   ├── _drafts
   |   └── _posts
   └── themes
   ```

   #### _config.yml

   网站的 [配置](https://hexo.io/zh-cn/docs/configuration) 信息，您可以在此配置大部分的参数。 

   具体参数信息请查看官方文档

   <https://hexo.io/zh-cn/docs/configuration> 

   #### package.json

   应用程序的信息。[EJS](https://ejs.co/), [Stylus](http://learnboost.github.io/stylus/) 和 [Markdown](http://daringfireball.net/projects/markdown/) renderer 已默认安装，您可以自由移除。 

   ```json
   package.json
   {
     "name": "hexo-site",
     "version": "0.0.0",
     "private": true,
     "hexo": {
       "version": ""
     },
     "dependencies": {
       "hexo": "^3.8.0",
       "hexo-generator-archive": "^0.1.5",
       "hexo-generator-category": "^0.1.3",
       "hexo-generator-index": "^0.2.1",
       "hexo-generator-tag": "^0.2.0",
       "hexo-renderer-ejs": "^0.3.1",
       "hexo-renderer-stylus": "^0.3.3",
       "hexo-renderer-marked": "^0.3.2",
       "hexo-server": "^0.3.3"
     }
   }
   ```

   ### scaffolds

   [模版](https://hexo.io/zh-cn/docs/writing) 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。

   Hexo的模板是指在新建的markdown文件中默认填充的内容。例如，如果您修改scaffold/post.md中的Front-matter内容，那么每次新建一篇文章时都会包含这个修改。

   ### source

   资源文件夹是存放用户资源的地方。除 `_posts` 文件夹之外，开头命名为 `_` (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 `public` 文件夹，而其他文件会被拷贝过去。 

   ### themes

   [主题](https://hexo.io/zh-cn/docs/themes) 文件夹。Hexo 会根据主题来生成静态页面。 

### 5.上传

站点生成后,就可以开始写文章了。dos界面下,进入所在站点目录，输入`hexo new [layout] <title>`命令。hexo会自动帮你生成一个 `<title>.md` 的文件。然后你就可以在这个文件上编写你的博客内容了。 

写完博客后,我们先试着在本地部署一下服务。还是在dos命令下，进入站点目录。一次输入: 

```
 hexo clean   # 清除缓存,之后会经常用到
 hexo g     # 生成站点静态文件
 hexo s     # 部署服务
```

上面是本地部署，之后就可以通过localhost:4000来访问博客了

但是如果想让别人访问你的还需要提交到git上面

打开cmd 然后输入`npm install hexo-deployer-git --save`安装git工具 

开站点目录下面的配置文件_config.yml(用任意编辑器),配置deploy参数。一开始配置文件是这样的: 

```
deploy: 
type: 
```

我们把它改成我们的git仓库地址。 

```
deploy:
  type: git
  repository: ssh://git@github.com/wyscoder/wyscoder.github.io
  branch: master
```

修改好之后执行

```
hexo clean 
hexo g 
hexo d # 部署到远程仓库 
```

令全部执行完后。我们就可以访问我们的博客网站了。https://wyscoder.github.io

### 6.问题

在使用`hexo d`提交的时候出现了几个问题

1. 首先是你得使用git命令来设置用户和邮箱

   ```git config --global user.name "nameVal" ```

   ```git config --global user.email "eamil@qq.com" ```

2. 其中我还测试了一下那个git的基本命令，都是卡在提交那一点，原因是没有密匙
3. 如果电脑上没有ssh密匙我建议还是要安装一下

### 7.美化博客：使用nexT主题

 	博客是搭建好了, 但是我们发现hexo的默认主题风格比较丑。好在hexo的主题是可定制的。所以我们可以更换别人已经做好的主题。nexT就是其中一个比较强大的主题。下面简单的教大家怎么切换到这个主题。

​	先去nexT的github页面下载nexT项目。https://github.com/iissnan/hexo-theme-next。然后放到站点目录下面的一个themes文件夹中。解压。
	解压后会得到一个hexo-theme-next-master文件夹。重命名成next。
	修改站点配置文件_config.yml里面的theme参数,修改值为next。

​	重新清除缓存,生成新的资源文件，然后部署,主题就切换成next了。

