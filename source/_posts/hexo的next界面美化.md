---
title: hexo的next界面美化
date: 2019-08-10 15:57:0
categories: 博客
tags: 博客
---
## 设置中文

首先查看一下next/theme/language中的语言类型，一般就是zh-CN或者是zh-Hans`这个类型

然后在hexo的配置文件`_config.yml`把写上去就行

## 主题风格

将next中的_config.yml配置文件修改一下

```yml
# Schemes
scheme: Muse
#scheme: Mist
#scheme: Pisces
#scheme: Gemini
```

这个把前面的#去掉换另一种就行，我觉得第三种就挺好看的。个人喜好

## 设置分类

还是在next中的` _config.yml`配置文件修改

```yml
menu:
  home: / || home
  about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat
```

把对应菜单的选项注释去掉就好

然后创建目录,不同标签对应不同的代码

```npm
$ hexo new page categories
```

创建完毕之后，进去修改一下

类似于`categories`这种的

```markdown
---
title: 分类
date: 2014-12-22 12:39:04
categories: Testing #分类名
type: "categories"
---
```

## 添加头像

搜索next中的`_config.yml`配置文件

搜索`Sidebar Avatar`这个关键字，去掉`avatar`前面的`#`

```
# Sidebar Avatar
# in theme directory(source/images): /images/avatar.gif
# in site  directory(source/uploads): /uploads/avatar.gif
avatar: /images/avatar.gif
```

然后把头像放到指定的文件夹

## 设置侧边栏的的社交链接

打开`themes/next/_config.yml`文件，搜索关键字`social`，然后添加社交站点名称与地址即可。 

```
social:
  GitHub: https://github.com/wyscoder|| github
  E-Mail: mailto:714133840@qq.com || envelope
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
  #VK Group: https://vk.com/yourname || vk
  #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
  #YouTube: https://youtube.com/yourname || youtube
  #Instagram: https://instagram.com/yourname || instagram
  #Skype: skype:yourname?call|chat || skype

```

## 修改底部的声明和版权

主题配置文件下，搜索关键字`post_copyright`，`enable`改为`true`： 

```
# Declare license on posts
post_copyright:
  enable: true
  license: CC BY-NC-SA 4.0
  license_url: https://creativecommons.org/licenses/by-nc-sa/4.0/
```

去掉强力驱动

找到hexo根目录>>themes>>next>>layout>>_partials下的footer.swig文件 

然后打开删除

```
{% if theme.footer.powered %}
  <div class="powered-by">{#
  #}{{ __('footer.powered', '<a class="theme-link" target="_blank" href="https://hexo.io">Hexo</a>') }}{#
#}</div>
{% endif %}

{% if theme.footer.powered and theme.footer.theme.enable %}
  <span class="post-meta-divider">|</span>
{% endif %}

{% if theme.footer.theme.enable %}
  <div class="theme-info">{#
  #}{{ __('footer.theme') }} &mdash; {#
  #}<a class="theme-link" target="_blank" href="https://github.com/iissnan/hexo-theme-next">{#
    #}NexT.{{ theme.scheme }}{#
  #}</a>{% if theme.footer.theme.version %} v{{ theme.version }}{% endif %}{#
#}</div>
{% endif %}
```

这部分内容就好

最后修改一下hexo中的配置文件名字就行

## 添加搜索功能

首先安装一下搜索插件

`$ npm install hexo-generator-searchdb --save`

打开`Hexo`站点的`_config.yml`，添加配置 :

```
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

打开`themes/next/_config.yml`，搜索关键字`local_search`，设置为`true`：

```
# Local search
# Dependencies: https://github.com/flashlab/hexo-generator-search
local_search:
  enable: true
  # if auto, trigger search by changing input
  # if manual, trigger search by pressing enter key or search button
  trigger: auto
  # show top n results per article, show all results by setting to -1
  top_n_per_article: 1
```

## 本地站点推送到GitHub上

安装插件

```
$ npm install hexo-deployer-git --save
```

 在`Hexo`站点的`_config.yml`中配置`deploy`：

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: <repository url> #your github.io.git
  branch: master
```

```
$ hexo clean
```

```
$ hexo g -d
```

## 添加网易云音乐

在网易云音乐（网页版）中搜索我们想要插入的音乐，然后点击生成外链播放器 

我放在了侧边栏，在 `themes/next/layout/_custom/sidebar.swig` 文件中增加生成的HTML代码： 

```
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=110 src="//music.163.com/outchain/player?type=0&id=408443429&auto=1&height=66"></iframe>
```

就好了

## 添加背景动画效果



 修改`_layout.swig` 

打开  `next/layout/_layout.swig`
 在 `< /body>`之前添加代码(注意不要放在< /head>的后面)

```
{% if theme.canvas_nest %}
<script type="text/javascript" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
{% endif %}
```

------

### 修改配置文件

打开 `/next/_config.yml`,在里面添加如下代码：(可以放在最后面)

```
# --------------------------------------------------------------
# background settings
# --------------------------------------------------------------
# add canvas-nest effect
# see detail from https://github.com/hustcc/canvas-nest.js
canvas_nest: true
```

到此就结束了，运行 `hexo clean`，然后运行 `hexo g`,然后运行 `hexo s`，最后打开浏览器在浏览器的地址栏输入 `localhost:4000` 就能看到效果了\（￣︶￣）/

------

如果你感觉默认的线条太多的话

可以这么设置====>

在上一步修改  `_layout.swig`中，把刚才的这些代码：

```
{% if theme.canvas_nest %}
<script type="text/javascript" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
{% endif %}
```

改为

```
{% if theme.canvas_nest %}
<script type="text/javascript"
color="0,0,255" opacity='0.7' zIndex="-2" count="99" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
{% endif %}
```

------

### 配置项说明

- `color` ：线条颜色, 默认: `'0,0,0'`；三个数字分别为(R,G,B)
- `opacity`: 线条透明度（0~1）, 默认: `0.5` 
- `count`: 线条的总数量, 默认: `150` 
- `zIndex:` 背景的z-index属性，css属性用于控制所在层的位置, 默认: `-1`

## 添加代码复制功能

首先找到这个目录`themes/next/layout/_third-party/ `

然后在此文件夹下创建名为copy-code.swig的文件，在此文件中输入以下内容： 

```js
  <style>
    .copy-btn {
      display: inline-block;
      padding: 6px 12px;
      font-size: 13px;
      font-weight: 700;
      line-height: 20px;
      color: #333;
      white-space: nowrap;
      vertical-align: middle;
      cursor: pointer;
      background-color: #eee;
      background-image: linear-gradient(#fcfcfc, #eee);
      border: 1px solid #d5d5d5;
      border-radius: 3px;
      user-select: none;
      outline: 0;
    }

    .highlight-wrap .copy-btn {
      transition: opacity .3s ease-in-out;
      opacity: 0;
      padding: 2px 6px;
      position: absolute;
      right: 4px;
      top: 8px;
    }

    .highlight-wrap:hover .copy-btn,
    .highlight-wrap .copy-btn:focus {
      opacity: 1
    }

    .highlight-wrap {
      position: relative;
    }
  </style>

  <script>
    $('.highlight').each(function (i, e) {
      var $wrap = $('<div>').addClass('highlight-wrap')
      $(e).after($wrap)
      $wrap.append($('<button>').addClass('copy-btn').append('复制').on('click', function (e) {
        var code = $(this).parent().find('.code').find('.line').map(function (i, e) {
          return $(e).text()
        }).toArray().join('\n')
        var ta = document.createElement('textarea')
        document.body.appendChild(ta)
        ta.style.position = 'absolute'
        ta.style.top = '0px'
        ta.style.left = '0px'
        ta.value = code
        ta.select()
        ta.focus()
        var result = document.execCommand('copy')
        document.body.removeChild(ta)

        if(result)$(this).text('复制成功')
        else $(this).text('复制失败')

        $(this).blur()
      })).on('mouseleave', function (e) {
        var $b = $(this).find('.copy-btn')
        setTimeout(function () {
          $b.text('复制')
        }, 300)
      }).append(e)
    })
  </script>

```

然后返回上一层目录，即layout文件夹下，编辑`_layout.swig`，

在最底部，`</body>`上面添加

```
{% include '_third-party/copy-code.swig' %}
```

这句话接着就能用了