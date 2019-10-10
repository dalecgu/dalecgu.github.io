---
title: 基于Hexo搭建个人博客
tags:
  - Hexo
categories:
  - 前端
date: 2019-10-10 22:14:37
updated: 2019-10-10 22:14:37
---


# 一、背景

搭建环境是写博客必不可少的一个步骤。本文搭建了基于Hexo的博客环境，描述了安装到部署的全过程，提供了常见问题的解决方案。搭建过程需要依赖git、node和npm，安装完这些依赖可以通过以下命令验证，正确返回版本信息即安装成功。

```sh
git version
node --version
npm --version
```

# 二、Hexo的安装与使用

## 1. Hexo的安装

使用`npm install hexo-cli -g`一行命令即可完成hexo的安装，安装完成后可以通过`hexo version`验证。

## 2. Hexo的使用

hexo的使用方法可以参考[Hexo官方文档](https://hexo.io)。以下命令可以快速创建一个博客，并在本地运行Server预览博客。

```sh
hexo init blog
cd blog
npm install
hexo server
```

博客的配置在_config.yml，细节可以参考[Hexo配置](https://hexo.io/docs/configuration)。hexo的常用命令如下，其他命令可以参考[Hexo命令](https://hexo.io/docs/commands)。

```sh
hexo new [layout] <title> #例如hexo new draft test创建名为test的草稿
hexo publish #将草稿正式发布
hexo server #本地预览博客
hexo generate #生成静态文件，只需部署该命令生成的文件
```

# 三、fexo主题的安装与使用

fexo是一个既好看又好用的hexo主题。

## 1. fexo主题的安装

```sh
cd blog
git clone git@github.com:forsigner/fexo.git themes/fexo
```

fexo的安装很简单，只需要把代码clone到themes目录下，然后把_config.yml中theme的配置改为fexo。

## 2. fexo主题的使用

使用master分支的fexo，配置时需要修改theme/fexo/_config.yml。在更新fexo时可能会因为配置项的改变导致冲突，更方便的做法是利用hexo 3的新特性Data Files将主题和配置分离。使用该方法需要切换到fexo的dev分支，然后在博客的source目录下新增_data文件夹，最后复制theme/fexo/_config.yml为source/_data/fexo.yml。之后只需要修改fexo.yml完成配置。fexo的配置可以参考[fexo配置](http://forsigner.com/2016/03/10/fexo-doc-zh-cn/)。

# 四、部署

我的博客使用git进行版本控制，源代码存储在GitHub，部署在GitHub Pages。使用持续集成可以简化博客的部署流程，将修改push到远程仓库后，Travis CI会完成部署工作。配置方法可以参考[easyhexo](https://easyhexo.com/1-Hexo-install-and-config/1-5-continuous-integration.html)。

# 五、插件

## 1. 如何提供RSS订阅

使用`npm install hexo-generator-feed --save`安装插件，再调用`hexo generate`，会在public目录生成atom.xml文件。在RSS订阅器里指定该文件的路径，例如`https://dalecgu.github.io/atom.xml`，就可以完成博客的订阅。`hexo-generator-feed`和新版的hexo有兼容性问题，可以参考[issue43](https://github.com/hexojs/hexo-generator-feed/issues/43)。

## 2. 如何被Google收录

如果好不容易写的博客连搜索引擎都搜索不到，被人看到的几率就更加低了。为了让Google收录我们的博客，需要先生成网站的sitemap。只需要`npm install hexo-generator-sitemap --save`安装插件，再调用`hexo generate`，应该会在public目录生成sitemap.xml文件。

然后需要在[Google SearchConsole](https://search.google.com/search-console/index)上提交sitemap。
1. 第一次登入Google SearchConsole时会提示Add property，选择URL prefix，然后输入博客路径，例如`https://dalecgu.github.io`
2. 选择一种方式验证你是该路径的所有者，我选择的是通过html文件认证，只需要把Google提供的html文件放在public目录，然后点击Verify
3. 验证通过后，点击左边Sitemaps按钮，然后填写sitemap文件的路径，例如`https://dalecgu.github.io/sitemap.xml`
4. 然后点击左边Coverage按钮，会提示数据处理中，等待数天，这里会显示Google收录了哪些页面