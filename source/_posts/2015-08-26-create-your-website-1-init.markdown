---

layout: post
title: 制作个人博客站（一）：Mac系统下使用 Octopress + GitHub Pages 搭建个人博客
date: 2015-08-26 23:32:38 +0800
comments: true
categories: Octopress
tags: Octopress, GitHub
keywords: Octopress, GitHub, Mac, GitHub Pages, Aster0id

---

程序员总是喜欢特立独行，搞些定制化得东西才会显得与众不同。看腻了诸如CSDN、网易、新浪等臃肿的博客系统，搭建一个属于自己的博客（或者是个人站点）大致成为了一个高逼格Coder的标配。最近使用 Octopress 搭建了我的个人博客，现在把内容总结出来，希望对大家有所帮助。

本文是建立在你有 Shell 指令基础及 Git 操作基础上。如果这部分还不了解的话，需要自己查阅文档。

在搭建之前需要理解几个术语：

- [Ruby](https://www.ruby-lang.org/zh_cn/)
	- Ruby 是一种编程语言。Octopress 是用 Ruby语言 实现的。我们不需要对它有太多了解，只需要正确安装 Ruby 的环境（Ruby版本必须不低于1.9.3-p0，后面会详细介绍）及按步骤执行指令即可。
- [RubyGems](https://rubygems.org)
	- RubyGems（简称 gems）是一个用于对 Ruby组件进行打包的 Ruby 打包系统。它可以用来查找、安装、升级和卸载软件包。我们也是通过它安装Octopress包的。

- [RVM](https://rvm.io)
	- RVM 是 一款 Ruby 语言安装、管理的工具。我们对 Ruby 的操作是通过它的指令完成的。
- [Jekyll](http://jekyll.bootcss.com/)
	- Jekyll 一个简单的博客形态的静态站点生产机器。Jekyll 有一套模板目录，可以将 Markdown文件（或者Textile）转换为静态网页，并生成一个完整的可发布的静态网站。
	- 同时，我们可以将产生的静态网站布置到 GitHub Pages 上，生成个人博客站点。
	- 想了解更多内容可以查看[中文文档](http://jekyll.bootcss.com/docs/home/)。
- [Octopress](www.github.com/imathis/octopress)
	- Octopress 是基于 Jekyll 的博客框架。他们的关系就像 jQuery 与 js 的关系一样。
	- 它为我们提供了现成的美观的主题模板，并且配置简单，使用方便，大大降低了我们建站的门槛。
- [GitHub](https://www.github.com)
	- GitHub 是全球最热的开源社区，程序界的 Facebook。它为我们提供代码托管服务，以及我们搭建博客所需要的 Pages 服务。
- [GitHub Pages](https://pages.github.com)
	- GitHub Pages 是 GitHub 提供的一项服务。它用于显示托管在 GitHub 上的静态网页。所以我们可以用 Github Pages 搭建博客，当然我们也可以把项目的文档和主页放在上面。



通过以上内容，我们大概能够明白 Octopress 建站的原理：

我们使用基于 Jekyll 的 Octopress 站点生成工具，生成本地的静态网站。然后将静态网站托管到 GitHub 为我们提供的 GitHub Pages 服务上。访问 __username__.github.io 即可显示你的个人博客站了。



---
明白了上面这些内容，下面进行具体的搭建工作：

####<font color="#008000"> 第一步：安装 Ruby </font>
打开终端，安装 RVM ，终端执行指令：

```
$ curl -L https://get.rvm.io | bash -s stable --ruby
```

接下来我们要查看自己的 Ruby 环境

```
$ ruby -v
```

如果你的 Ruby 版本不低于 1.9.3-p0 可以忽略 Ruby 的安装（或升级），直接跳到安装 RubyGems 。
否则，我们执行之后的操作，终端执行指令：

```
$ rvm install 1.9.3
$ rvm use 1.9.3
```

然后安装 RubyGems， 终端执行指令：

```
$ rvm rubygems latest

```

到这里第一步完成。我们可以再执行一次第一条指令 `ruby -v` 来查看当前 Ruby 的版本了。

####<font color="#008000"> 第二步：安装Octopress </font>
因为Mac系统自动git环境，所以我们不需要考虑git的安装。直接将 Octopress的项目clone到本地，在终端执行指令：

```
$ git clone git://github.com/imathis/octopress.git octopress
```
完成后进入 octopress 的目录

```
$ cd octopress 
```

接下来，安装依赖：

```
$ gem install bundler
# 这时你可能会遇到没有权限的问题，那么我们需要加上sudo重新执行，并输入密码。
$ sudo gem install bundler
# 接下来执行：
$ bundle install
```

这时你可能还会遇到问题如下： 

```
Fetching gem metadata from https://rubygems.org/...........
Resolving dependencies...
 
Gem::RemoteFetcher::FetchError: SocketError: getaddrinfo: Name or service not known (https://rubygems.org/gems/rake-10.4.2.gem)
An error occurred while installing rake (10.4.2), and Bundler cannot continue.
Make sure that `gem install rake -v '10.4.2'` succeeds before bundling.

```
这是因为被墙了，解决办法有两个：
一个是，可以使用自己的翻墙工具；
另一个，淘宝做了一个gem的镜像。我们需要在Octopress的文件目录下找到Gemfile文件，将其中的`source 'https://rubygems.org/'`改为`source 'https://ruby.taobao.org/'`

再重新运行`bundle install`就可以了。
这段内容可以参考 [bundle install 提示如下，是需要翻墙解决么](http://www.imooc.com/qadetail/59105)。
 
 下面就可以安装 Octopress 的默认主题了，终端执行指令：
 
 ```
 $ rake install
 ```
这样一个最基本的个人博客站就产生了。
 
{% img /images/create-your-website-1/octopress-init.png %}  

可以看出，现在显示得都是预设值，并不是我们想要的，所以需要修改Octopress目录下的_config.yml文件。

|    文件/目录   | 描述 |
| ------------- | --- |
| `_config.yml` | 保存配置数据。很多配置选项都会直接从命令行中进行设置，但是如果你把那些配置写在这儿，你就不用非要去记住那些命令了。 |

_config.yml 文件共分为3个不发内容

+ Main Configs 
+ Jekyll & Plugins
+ 3rd Party Settings

目前，我们只需要关注第一部分`Main Configs`。

```
# ----------------------- #
#      Main Configs       #
# ----------------------- #

#网站地址
url: http://aster0id.github.io

#网站标题 
title: Aster0id的个人博客

#网站副标题 
subtitle: 每天进步一点点.

#网址作者，通常显示在页尾和每篇文章的尾部
author: Aster0id

#搜索引擎
#simple_search: http://google.com/search

#网站的描述，出现在HTML页面中的 meta 中的 description
#description:

```

对应填入你的个人信息，其中url为必填的，一般填GitHub仓库对应的连接，其内容大致就是 __username__.github.io ，这个地址我们会在后面步骤中获得。


####<font color="#008000"> 第三步：集成 GitHub Pages </font>
在 GitHub 上创建一个代码参考，项目名称命名规则为 __username__.github.io，username必须与用户名称一致。

Ps. 在创建过程中最好不要`添加忽略文件`和`README文件`。因为我们要把本地的 git 仓库同步到 GitHub 远程仓库中。如果再远程仓库中添加了其他文件，需要我们执行 pull 操作。除非你能非常熟练的使用 git ，否则不建议你制造不必要的麻烦。

接下来将本地代码仓库同步到 GitHub 上，执行终端指令：

```
$ rake setup_github_pages
```

它会要求你绑定远程仓库的地址，此时只需要输入即可：


```
$ git@github.com:username/username.github.com.git
```

这样就会将 Octopress 生成的静态站点与 GitHub 进行绑定了。

之后我们创建第一篇文章：

```
rake new_post["title"]
```

生成的新文章在source/_post/目录下，文件名构成为时间和标题的拼音。我们可以用Markdown编辑器对文章进行修改。

之后生成静态站点，终端执行指令：

```
# 生成静态站点
$ rake generate 
```

如果你想预览本地的站点，可以执行终端指令：

```
# 预览静态站点
$ rake preview
```

此时，可以使用浏览器打开 `localhost:4000` 查看效果。如果没有问题可以将静态站点同步到 GitHub 远程仓库中，终端执行指令：

```
# 同步内容
$ rake deploy
```

你会发现我们的静态站点已经被 push 到 GitHub仓库的 master 分支上。稍等几分钟，访问 `username.github.io (或者 username.github.com)`，就会发现你的个人博客站已创建成功了。


如果你还想给自己的本地资源文件（如Markdown文件等内容）也同步到 GitHub 中，可以执行以下指令：

```
$ git add .
$ git commit -m "comment"
$ git push origin source
```

这样我们的资源文件就会同步到 GitHub 的 source 分支了。

<br>
---
现在我们完成了个人博客的初级搭建，足够满足我们的基本需求。之后，我会补充几篇文章，关于`绑定域名`、`更换主题`、`定制主题` 等内容。

<br>

__希望这篇文章对你有所帮助__


