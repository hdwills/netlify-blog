---
title: Grunt - imagemin图像压缩
date: 2014-06-24 21:00:00
description: 前端构建部分引入了 Grunt，本文介绍其中的常用插件 grunt-contrib-imagemin 。
category: Tech
tags: Grunt
---
项目前端构建部分引入了 [Grunt](http://www.gruntjs.com/)，便于各项处理任务的自动化管理和运行，重复性的任务都交给工具去完成，代码编译、规范校验、单元测试等，一次配置终生受用！他会按照你预先设定好的顺序自动执行各个任务。

关于 Grunt 的前世今生我就不做介绍了，这里只做使用介绍，也是项目开发中的部分经验积累和记录。

项目上线前，都要对网站进行各项测试，前端这部分我们首要考虑的是对图片的压缩，因为我用 PageSpeed 检测的时候，这部分直接飙红了，提示压缩后最大限度降低有效负荷，这都是在开发时，切图没有输出最优图片而导致。以前处理方式，或许会在工具里重新输出图片，还有 [Yahoo - Smush.it](http://www.smushit.com/ysmush.it/) 上传处理等……这些工具在很大程度上也帮我们不少忙，但难免在你开发的时候要多工具的来回切换，输出保存等。今天我们借助 Grunt 就能很轻松的完成这些任务，而且是在你不经意之间就会默默的完成，是不是很人性、很省心！

```javascript
// 安装插件
npm install grunt-contrib-imagemin --save-dev
// 加载插件(Gruntfile.js)
grunt.loadNpmTasks('grunt-contrib-imagemin');
// 配置插件(图片压缩)
imagemin: {
    dynamic: {
        options: {
            optimizationLevel: 3 // png图片优化水平，3是默认值，取值区间0-7
        },
        files: [
            {
                expand: true, // 开启动态扩展
                cwd: "images/", // 当前工作路径
                src: ["**/*.{png,jpg,gif}"], // 要出处理的文件格式(images下的所有png,jpg,gif)
                dest: "images/" // 输出目录(直接覆盖原图)
            }
        ]
    }
},
```

配置好之后，运行 grunt imagemin
```
Running "imagemin:dynamic" (imagemin) task
✔ images/about_1.jpg (already optimized)
...
✔ images/sprites-s4e83c27da3.png (saved 10.92 kB - 8%)
...
Minified 69 images (saved 255.81 kB)
```

在安装插件的时候，会出现 `pre-build...failed` 相关字眼的错误，就试试删除后，再重新安装。或者先安装 `npm install jpegtran-bin --save-dev`，应该是这个和 `imagemin` 有依赖关系。

其中 `option` 里还有一些高级配置，关于 `optimizationLevel` 也有详细的解释，有兴趣的可以详细阅读。[grunt-contrib-imagemin](https://github.com/gruntjs/grunt-contrib-imagemin)。

1. Grunt 中文社区 - [http://gruntjs.org/](http://gruntjs.org/)
2. Grunt 官网 - [http://gruntjs.com/](http://gruntjs.com/)