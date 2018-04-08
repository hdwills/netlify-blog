---
title: PDF.js 本地部署
date: 2017-03-05 22:00:00
description: PDF.js 本地部署
category: Tech
tags: PDF.js
---

## 项目需求
1. 我们产品的桌面版有输出 pdf 功能，并且能借助浏览器预览和打印，好的是 chrome 和 firefox 中都集成了 PDF.js 这样就能很好的实现这些功能。

2. 然后新的需求又来了，PDF.js 在浏览器中保持默认的设置，功能比较齐全，然后还想设置一些限制在其中，就感觉特别乏力了，最终也不了了之。

3. 接着又是一波新需求，在手机中要能查看 pdf。好了，这下不得不考虑将 PDF.js 集成到项目中了。

## 实现细节

### 基础
项目地址：https://github.com/mozilla/pdf.js

> PDF.js、pdf.js是一款使用HTML5 Canvas安全地渲染PDF文件以及遵从网页标准的网页浏览器渲染PDF文件的JavaScript库。

这里有详细的说明文档，如果不想自己编译的话直接去 releases 里下载合适的版本即可，下载解压后的目录如下：
```
./pdfjs-1.10.88-dist
  ├── build/
  ├── web/ 这里根据项目需求，最终会更换成`mobile-viewer`目录里的资源
  └── LICENSE
```
打开各目录看了之后，其中`web`目录中就是 PDF.js 将要用到的预览容器和部分资源，而 `viewer.html` 就是 PDF.js 浏览要用到的页面，是成品哦！`build`里面是核心的库文件。这时你直接打开 `viewer.html` 后直接看到的就是最终效果。

如果不能正常浏览，就需要在 web 服务器中，才能正常浏览。如果没有可以借助文档中，编译的那部分，借助 node.js 和 gulp 很快也能搭起来，顺便也可以自己编译。而且也能看到后续我们需要的 mobile 版本。

```
$ git clone git://github.com/mozilla/pdf.js.git
$ cd pdf.js
$ npm install -g gulp-cli
$ npm install
$ gulp server

...

$ gulp server
[11:33:40] Using gulpfile D:\Documents\GitHub\pdf.js\gulpfile.js
[11:33:40] Starting 'server'...

### Starting local server
Server running at http://localhost:8888/
```

这时在浏览器中就能看到预览效果，[PDF.js Online demo](https://mozilla.github.io/pdf.js/web/viewer.html)。不得不服，这么强大的东西，各设备适配还是很棒的！

1. 桌面版    
![pdfjs_desktop](http://7xtjgk.com1.z0.glb.clouddn.com/pdfjs_desktop.png)

2. 桌面版在手机端的效果  
![pdfjs_desktop_portrait](http://7xtjgk.com1.z0.glb.clouddn.com/pdfjs_desktop_portrait.png)
![pdfjs_desktop_landscape](http://7xtjgk.com1.z0.glb.clouddn.com/pdfjs_desktop_landscape.png)

再回顾下刚才用 clone 的文件，其中 `example` 文件夹里列出来了各种应用场景下的例子，你会惊喜的看到 `mobile-viewer` 文件夹，直接去浏览即可看到移动端的适配效果。

3. 手机版  
![pdfjs_desktop_portrait](http://7xtjgk.com1.z0.glb.clouddn.com/pdfjs_mobile_portrait.png)
![pdfjs_desktop_landscape](http://7xtjgk.com1.z0.glb.clouddn.com/pdfjs_mobile_landscape.png)

`mobile-viewer` 文件夹里对应的各文件，就是手机版要用到的，也就是最终要合进项目中的。

### 进阶
以上这些预览效果都是目录里自带的文件`compressed.tracemonkey-pldi-09.pdf`，根据实际的渲染效果和等待时间，各设备略有差异。

通过搜索`viwer.js`中可以发现这个文件的出处：
```
var DEFAULT_URL = '../../web/compressed.tracemonkey-pldi-09.pdf';
```

这里是固定文件的访问。如果想在自己的网站内用 PDF.js 打开任意 pdf 文件，可以这样调用：    
[Setup PDF.js in a website](https://github.com/mozilla/pdf.js/wiki/Setup-pdf.js-in-a-website)
```
<a href="/web/viewer.html?file=%2Fyourpdf.pdf">Open yourpdf.pdf with PDF.js</a>
```
我们的应用场景是，这个页面内既能浏览 pdf，还有别的内容要展示。这样一来好端端的页面就被破坏了 ╮(╯▽╰)╭。目前就尝试着在这个页面内潜入 iframe，在 iframe 中潜入了 `viewer.html` 然后在`file`参数上传入 pdf 的路径……再根据实际情况，对皮肤也做了定制，最终效果图就不展示了 /(ㄒoㄒ)/~~

### 问题
常见的有跨域问题，这些搜索之后都能解决。但是我们的服务器端也配置过了，直接浏览传入`file`参数之后，还是不能访问。后来换用 iframe 调用之后没出现过跨域问题，但是又有了新的问题`file origin does not match viewer's`，这是一项安全措施，大致意思是说你打开的协议和 url 的协议不匹配，难道是因为那个 file 什么的？将这部分注释掉即可。

- [Allow foriegn origin URLs only for hosted viewers.](https://github.com/mozilla/pdf.js/pull/6916)
- [Frequently Asked Questions](https://github.com/mozilla/pdf.js/wiki/Frequently-Asked-Questions)

好了，内测貌似没什么问题了，接着又加了部分需求。准备上线测试，结果发现 PDF.js 在安卓版的微信中打开时，当前页面会直接关闭，然而高贵的 iOS 却好好的，搜了一下相关问题，真的是太可怕了，到目前为止，这个问题还在继续研究中！因为同一部手机，有的数据就会意外关闭，有的却能正常浏览。