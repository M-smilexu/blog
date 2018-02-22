---
title: "前后端联调解决方案"
date: 2015-09-04 20:35:17
categories: [coding, debug]
layout: post
description: "现在后端语言有java, php ... 不同项目开发环境和依赖也会有很大不同，为了节约开发成本，前端电脑一般没有后端开发的环境，代码也不在一个项目里，所以联调就是一个比较关键的点了？怎么联调？"
---

现在后端语言有java, php, 不同项目开发环境和依赖也会有很大不同，为了节约开发成本，前端电脑一般不安装后端开发环境，代码也不在一个项目里，所以联调就是一个比较关键的点了？怎么联调？

在前后分离的开发模式下，交互一般都是通过ajax到后端请求数据，前端在开发前期，一般都会通过约定的数据格式先在本地模拟数据进行界面渲染，开发结束，联调的时候就需要直接从后端获取数据来渲染界面，看看在真实数据下代码和界面是否运行正常。那么联调的时候必然会涉及一个问题：跨域。其实接下来的前后端联调解决方案解决的问题就是ajax怎么跨域？
 
    // local

    $.ajax({url: '/api/test.json'});

    // 跨域: 如后端地址 http://www.mogujie.org/api/test.json
    // 直接运行会报错: No 'Access-Control-Allow-Origin' header is present on the requested resource. 
    // Origin 'http://localhost:3000' is therefore not allowed access. 
    // The response had HTTP status code 403.

    $.ajax({url: 'http://www.mogujie.org/api/test.json'});
 
这里提供两种解决方案： 
 
* 1、开启chrome canary的跨域功能;
* 2、利用browser-sync的middleware:proxy-middleware转发.
 
### 1、开启chrome canary的跨域功能

在此之前先解释几个问题: 
 
* 1.1、Chromium 和 Chrome 的区别;
* 1.2、Chromium 和 Chrome 有哪些版本.
 
**1.1、Chromium 和 Chrome的区别**
**Chromium**
 
* 谷歌的开源项目，由开源社区维护。
* 国产的所有 “双核浏览器”，都是基于 Chromium 开发的，甚至 Chrome 也是基于它。
* 我们下载的 Chromium 浏览器都是其源码未经修改的直接编译版本。
 
**Chrome**
 
* 基于 Chromium，但是最重要的一点是它是闭源的！
* 所以有这样的一种说法：谷歌把核心技术都保留在了之家的 Chrome 中。
 
**上面两者的区别**
 
* 事实上 Chrome 相比 Chromium，支持了一些商业的收费插件，这些是不会出现在开源软件中的： H.264编码、mp3编码（我知道的是这两个）
* 此外 Chrome 内置了 Flash，Chromium 需要额外安装，安装方法也很简单的：<a href="http://www.zhihu.com/question/32223811/answer/60456561">安装方法</a>
* 听说在网页渲染方面 Chrome 也悄悄有一些特别的优化，也许是这样的吧。
* Chromium 的内核版本比 Chrome 明显领先，新的技术都是先在 Chromium 上应用，但是说 Chromium 是新功能的试水版本，这是不准确的。
* Chrome 明显集成了更多的谷歌服务（RanBinNuan），同时也有更多的限制，比如目前使用 Chrome 需要一定手段才能安装非商店的扩展，一旦被发现还会永远禁用，但 Chromium 就没有这些限制！
 
**1.2、Chromium 和 Chrome 有哪些版本**
**Chromium 的版本**
对于我们使用者就两个：开源项目的镜像版（10 分钟一次更新），稳定版（大约 1 小时一次更新）
**Chrome 的版本**
一共 4 个，新版发布速度递增，新功能数量递增，稳定性递减：
 
* Stable 稳定版（几月一次更新）
* Beta 测试版（1 月一次更新）
* Dev 开发者版（1 星期一次更新）
* **Canary 金丝雀版（脚步几乎同步 Chromium，天天更新）图标采用了特别的土豪金版神奇宝贝球。**
 

Canary版本是比Dev版本更快，而比Chromium稍慢的Chrome版本，与Chromium版相比Canary版本可以自动更新，比Dev版更新的频率更快。

Canary版本还有一个重要特性是允许跟系统里其它Chrome版本共同存在。（稳定版和Beta / Dev 相互之间不可共存）

Google建议广大技术爱好者与开发人员安装Chrome Canary版本，它会为你提供最新的Chrome功能和升级。Chrome开发团队Mark Larson指出：“Canary 版将比开发者版本的升级频繁很多，我们在努力使它像每日开发版本那样获得更新。从Canary版本用户那里获得的数据（尤其是崩溃数据）将帮助我们更快得发现并修复问题。”

有点扯远了，理清这些关系之后，回到我们的主题：开启chrome canary的跨域功能

第一步：先安装chrome canary版本， google chrome canary 官网地址，根据提示下载安装

第二步：打开终端运行如下命令,开启chrome canary的跨越功能

 
    open -a /Applications/Google\ Chrome\ Canary.app --args --disable-web-security
 
打开chrome canary，你会看到如下效果，说明开启成功了，接下来你就可以在这个浏览器上调试了
<img src="/images/canary.png" />
再运行如下代码就不会报错了
 
    $.ajax({url: 'http://www.mogujie.org/api/test.json'});
 
普通chrome浏览器理论上也可以开启跨域功能，运行命令如下(我试了几次，没有开启成功，使用canary是一定能开启成功的。)
 
    open -a /Applications/Google\ Chrome.app --args --disable-web-security
 
如果接口比较少的时候，这种方式比较简单，效率也不会影响；但是当接口数量比较多时，你联调完成之后可能还需要合并代码：即把前后端代码整合在一起，那又要把联调的时候的url地址改回到原来的地址如：http://www.mogujie.org/api/test.json ——> /api/test.json；这又是比较浪费时间的活，所以推荐下面这种解决方案。 

### 2、利用 browser-sync 的 middleware:proxy-middleware 转发.

第一步: 如果你没有node环境得先安装node
 
    brew update

    brew install node
 

第二步: 安装gulp

 
    npm install -g gulp
 

进入前端开发目录，建立gulpfile.js文件, 配置内容如下
 
    var gulp = require('gulp');
    var url = require('url');
    var proxy = require('proxy-middleware');
    var browserSync = require('browser-sync'); 

    // browser-sync task for starting the server.
    gulp.task('browser-sync', function() {
        var proxyOptions = url.parse('http://www.mogujie.org/');
        proxyOptions.route = '/api';
        // requests to `/api/x/y/z` are proxied to `http://www.mogujie.org/`

        browserSync({
            open: true,
            port: 3000,
            server: {
                baseDir: "./",
                middleware: [proxy(proxyOptions)]
            }
        });
    });

    // Default Task
    gulp.task('default', ['browser-sync']);
 

先安装依赖

 
    npm install gulp url proxy-middleware browser-sync --save-dev
 

接着运行 **gulp**
代码中所有带有**/api**的http会被转发到**http://www.mogujie.org/**

<img src="/images/browser-sync.png" />

在开发前，前后端最好先约定ajax请求地址都带有某个路径，如**/api**;便于区分ajax http请求和普通http请求。

这样在调试结束就可以愉快地开始合代码了！


