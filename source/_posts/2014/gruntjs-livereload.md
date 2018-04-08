---
title: Grunt - LiveReload页面自动刷新
date: 2014-06-30 22:00:00
description: 前端构建部分引入了 Grunt，本文介绍其中的常用功能自动刷新一系列需要用到的插件，分别是 grunt-contrib-connect, grunt-open, grunt-contrib-watch。
category: Tech
tags: Grunt
---
在整理这篇文章的时候，默默的看了下键盘上的`F5`，是不是闪闪发亮，油头粉面的。和他一样的当然也少不了 `ctrl` `alt` `s`。因为传统的流程就是编辑器里 codding，ctrl+s，浏览器里 F5 刷新浏览，即使是你熟练于心、一气呵成。但是当你使用了自动刷新之后再配上强大的编辑器，剩下的只是专心 codding 了，若是配备双屏显示器，那效率绝对成倍成直线飙升。

先介绍下要用到的插件，具体安装就不单独说明了，每个插件也都有对应的文档说明：

- grunt-contrib-connect
	静态文件服务器
    ```javascript
    // Gruntfile.js
    
    // 配合 livereload 使用，建立本地静态调试环境
    // https://www.npmjs.org/package/grunt-contrib-connect
    connect: {
        options: {
            port: 11011,
            hostname: "localhost",
            base: ".",
            livereload: 35729
        },
        all: {
            options: {
                open: true,
                base: [
                    "."
                ]
            }
        }
    },
    ```

- grunt-contrib-watch
	监视文件的变化
    ```javascript
    // Gruntfile.js

    // 资源监控，主要用在开发时自动刷新浏览器
    // https://www.npmjs.org/package/grunt-contrib-watch
    watch: {
        options: {
            spawn: false
        },
        style: {
            files: ["sass/**/*.scss"],
            tasks: ["compass"]
        },
        livereload: {
            files: [
                "./theme*/*.html",
                "./theme*/*.css"
            ],
            options: {
                livereload: '<%= connect.options.livereload %>' // this port must be same with the connect livereload port
            }
        }
    },
    ```
- 配置任务
    ```javascript
    // Gruntfile.js
    
    grunt.registerTask("livereload", ["connect:all", "watch"]);
    ```
    
通过这次记录，回过头又翻了一下官方文档，发现在目前这个 basic 配置只是勉强能用，还有很多需要改进提升的地方。因为插件也在不断更新，加入了更多的功能。在 `connect` 和 `watch` 的主页看到，功能还是很丰富的。特别是 `content` 里已经有了 `open` 功能，那样的话就会少用到一个插件。

通过最近在 Grunt 中的尝试，用在项目中只是略微在开发过程中提升了效率，也只是基础使用，有空了看看说明文档，发现更多高级的功能，还是很不错的。比如有时候插件在更新之后会加入新特性，而自己的配置文件只是起初对于项目适用之后，就不会再做更新和改进，思想还是停留在起初到头来并没有多少提升，还是着重要培养思想。

更新：后来看了文档，grunt-contrib-watch 已经内置 Livereload，以上的内容我做了修改。