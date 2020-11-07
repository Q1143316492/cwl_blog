---
title: 基于hexo+next的github博客搭建
date: 2020-05-30 14:47:14
tags:
    - hexo
    - 博客搭建
categories: 工具
---

# 使用hexo+github部署博客



[toc]



# 1.0 概述

毕业后的第一篇博客。把博客从博客园搬到github.io。博客园写markdown也太难受了。。。

使用hexo把博客部署到github上。



# 2.0 本地初步安装



- 电脑要安装git

- 下载安装node js 环境

  去node js 官网 https://nodejs.org/en/download/ 下载安装包，下一步下一步balabala。。。 

  在终端敲·`npm -v` 能看到东西，就成功了。

- 配置镜像

  npm是一个包管理工具。由于npm可能会到国外下载东西。网速会比较慢。所以一般会配置一个镜像。

  安装cnpm，在终端敲以下命令

  ```bash
  npm install -g cnpm --registry=https://registry.npm.taobao.org
  ```

  然后敲`cnpm -v`可以看到东西就成功了。cnpm和npm用法差不多。只是cnpm会走国内的源

- 简单安装hexo

  我们来到hexo官网。https://hexo.io/zh-cn/ 。首页就有教你怎么配置。

  ```bash
  npm install hexo-cli -g
  hexo init blog
  cd blog
  npm install
  hexo server
  ```

  注意把npm换成cnpm。其他不变。我们安装hexo脚手架。

  ```bash
  cnpm install hexo-cli -g
  ```

  

  - 使用`hexo init cwl_blog`会创建一个blog的博客目录。我在这一步下载有点卡o(︶︿︶)o 

  - `cd cwl_blog`  目录结构是这样的。博客根目录。

    ```
    admin@DESKTOP-LB2NRTM MINGW64 /g/0.workspace/blog/cwl_blog
    $ ls
    _config.yml    package.json       scaffolds/  themes/
    node_modules/  package-lock.json  source/
    ```

  - `cnpm install` 我也不知道这东西安装了啥。。。
  - `hexo server` 部署。然后回提示

  ```
  admin@DESKTOP-LB2NRTM MINGW64 /g/0.workspace/blog/cwl_blog
  $ hexo server
  INFO  Start processing
  INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
  ```

  服务就已经起来了。在浏览器敲 `http://localhost:4000/`就能访问了。



# 3.0 博客样式



我选next这个模板。懒+菜，写不出好看的模板

- 下载主题

https://github.com/iissnan/hexo-theme-next

根目录下面有themes文件夹放主题。下一级的每个文件夹是一个主题。把相关主题copy到下面即可。

在github说明下。第一步命令是在themes目录下创建next文件夹放文件。



第二个命令是下载命令。瞟了一眼里面的`themes/next`是下载到哪里。注意敲`curl`的位置。

```bash
mkdir themes/next
curl -s https://api.github.com/repos/iissnan/hexo-theme-next/releases/latest | grep tarball_url | cut -d '"' -f 4 | wget -i - -O- | tar -zx -C themes/next --strip-components=1
```



but，我的windows都没有curl这个东西。我就直接在next文件夹下面 git clone 了。

```bash
git clone https://github.com/iissnan/hexo-theme-next.git
```



clone 后目录里面的东西如下

```bash
admin@DESKTOP-LB2NRTM MINGW64 /g/0.workspace/blog/cwl_blog/themes/next
$ ls
_config.yml      hexo-theme-next-master.zip  LICENSE       README.md  test/
bower.json       languages/                  package.json  scripts/
gulpfile.coffee  layout/                     README.cn.md  source/
```



- 配置主题

在根目录下 `_config.yml`文件打开

找到`theme`那一行。修改成对应的名字。注意冒号后的空格

```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

重新部署在本地看下效果

```
hexo clean
hexo g
hexo s
```



# 4.0 部署到github.io



- 首先有个github仓库
- 创建一个仓库，比如我的是`Q1143316492.github.io`。换成`你的github名字.github.io`

- 回到我们博客根目录下。装个部署插件。

```
cnpm install --save hexo-deployer-git
```

安装后

```
admin@DESKTOP-LB2NRTM MINGW64 /g/0.workspace/blog/cwl_blog
$ cnpm install --save hexo-deployer-git
platform unsupported hexo-deployer-git@2.1.0 › hexo-fs@2.0.1 › chokidar@3.4.0 › fsevents@~2.1.2 Package require os(darwin) not compatible with your platform(win32)
[fsevents@~2.1.2] optional install error: Package require os(darwin) not compatible with your platform(win32)
√ Installed 1 packages
√ Linked 78 latest versions
√ Run 0 scripts
Recently updated (since 2020-05-22): 2 packages (detail see file G:\0.workspace\blog\cwl_blog\node_modules\.recently_updates.txt)
  Today:
    → hexo-deployer-git@2.1.0 › hexo-util@1.9.1 › strip-indent@3.0.0 › min-indent@^1.0.0(1.0.1) (03:20:06)
√ All packages installed (80 packages installed from npm registry, used 12s(network 12s), speed 222.78kB/s, json 79(195.52kB), tarball 2.52MB)
```

- 配置一下，又到了根目录的 `_config.yml`里面

最下面原来是这样的

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: ''

```

配置后。注意冒号后面的空格

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/Q1143316492/Q1143316492.github.io.git
  branch: master

```

写完保存。在终端中D的意思是Deployment

```
hexo clean
hexo g
hexo d
```

然后就可以在公网

`https://Q1143316492.github.io/`访问了。这里的Q1143316492是你的github名。



# 5.0 页面进一步配置

next主题的官方手册

http://theme-next.iissnan.com/getting-started.html#select-language

bilibili的某教程

https://www.bilibili.com/video/BV16W411t7mq?p=20

基本就是按着开始文档弄一遍。需要注意的是`_config.yml`文件里面的空格问题

在配置menu的时候。`Key: /link/ || icon`这个 || 前面加了个空格就连接不过去了



```
# When running the site in a subdirectory (e.g. domain.tld/blog), remove the leading slash from link value (/archives -> archives).
# Usage: `Key: /link/ || icon`
# Key is the name of menu item. If translate for this menu will find in languages - this translate will be loaded; if not - Key name will be used. Key is case-senstive.
# Value before `||` delimeter is the target link.
# Value after `||` delimeter is the name of FontAwesome icon. If icon (with or without delimeter) is not specified, question icon will be loaded.
menu:
  home: /|| home
  about: /about/|| user
  tags: /tags/|| tags
  categories: /categories|| th
  archives: /archives/|| archive
  # schedule: /schedule/ || calendar
  # sitemap: /sitemap.xml || sitemap
  # commonweal: /404/ || heartbeat
```



要不是我认得 %20 是 URL 序列化的东西，翻译过来是空格。直接弃坑了。。。



其中几个部分比较有意思的部分

- 无后端的评论功能配置。基于valine

  https://valine.js.org/quickstart.html

  注册登入后，进入控制台。创建应用

  找设置里面的应用keys。里面有app id 和 app key。粘到next里面的。注意enable默认是false来着

  ```
  valine:
    enable: true
    appid: # your leancloud application appid
    appkey: # your leancloud application appkey
  ```

  评论内容修改要到 leancloud存储里面手动删数据了。




- 搜索功能

1, 安装插件，在根目录敲命令

```
 cnpm install hexo-generator-searchdb --save
```

2, 在更目录 _config.yml 任意地方加代码

```
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

3, 在 next主题下的_config.yml。修改

```
# Local search
local_search:
  enable: true
```






# 6.0 写博客



在博客根目录下 `source/_posts` 是放博客的地方。到该目录敲`hexo n 文章名`

注意下面tags和categories会影响博客的标签功能

```
---
title: 基于hexo+next的github博客搭建
date: 2020-05-30 14:47:14
tags:
    - hexo
    - 博客搭建
categories: 工具
---
```





# 7.0 命令速记



在source/_post目录下



新建文章

```
hexo n
```



本地查看

```
hexo clean
hexo g
hexo s
```



远程部署

```
hexo clean
hexo g
hexo d
```

