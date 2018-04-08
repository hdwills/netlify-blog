---
title: grunt-spritesmith 使用笔记
date: 2015-11-05 12:00:00
description: Spritesheet making utility
category: Tech
tags: 
  - Compass
  - Sprite
  - Sass
  - Grunt
  - spritesmith
---
好久以前介绍了 [利用 Compass 生成 Sprite 图片][1]，后来在使用中也表现良好，后来对于 retina 屏幕做适配，发现其并不能满足需求，尝试了一番工具之后，找到这个工具。在一番适配之后，发现能满足目前的需求，状况还不错。
其实在长期使用 Grunt 之后，发现这些类似的构建工具的确能够帮我处理很多繁琐的任务，但是前期的学习成本对我来说还是有的。而且对于这些工具，一旦有适合你需求的插件，那当然没问题；若是并不能满足你的需求，要么新找插件、要么自己写一个吧 <(￣ˇ￣)/，其实更多的精力还是放在代码本身好了，下面接入正题：
## 安装
```javascript
npm install grunt-spritesmith --save-dev
```
## 设置
可根据 [官方文档][2] 和自身项目情况，配置 `Gruntfile.js`
```javascript
// Spritesheet making utility
sprite: {
  front: {
    // 源文件地址
    src: '<%= config.src %>/images/sprites/*.png',
    // 对应的 2x 图片文件
    retinaSrcFilter: '<%= config.src %>/images/sprites/*@2x.png',
    // 生成的 sprites 文件
    dest: '<%= config.src %>/images/sprites.png',
    // 生成的 sprites-2x 文件
    retinaDest: '<%= config.src %>/images/sprites-2x.png',
    // 输出的 scss 文件，这里也可以输出 css 文件
    destCss: '<%= config.src %>/sass/_sprites.scss',
    // 针对需自定义的部分，做适应的配置，这里加了特殊的前缀
    cssVarMap: function (sprite) {
      sprite.name = 'icons-ipad-' + sprite.name;
    }
  }
}
```
文章里用到的相关代码，已经放在 [Gist][3] 上了。
```shell
Running "sprite:front" (sprite) task
Files "source/sass/_sprites.scss", "source/images/sprites.png", "source/images/sprites-2x.png" created.

Done, without errors.
```
![grunt-spritesmith-handbook-1.png][4]
![grunt-spritesmith-handbook-2.png][5]
这时候去看生成的 `_sprites.scss` 文件，这里为了方便管理 `css` 文件，所以输出了 `scss` 文件。然后使用的时候直接调用即可，项目中的图标是全部调用的，所以对应的代码如下，生成的 `scss` 文件中有官方的注释，很好理解：
```javascript
@import sprite.scss;
@include retina-sprites($retina-groups);
```
## 问题
这时候一切看起来都很美好，貌似问题已经得到了解决。但是问题又来了，在生成的 `_sprites.scss` 中你会看到，每一个图标对应的语句中，都有 `$icons-ipad-rightnav-xxx-image: '../images/sprites.png';`，在编译后的 `css` 文件中每个图标 class 都会有 `background-image: url(../images/sprites.png);`，这个就是 **node_modules/grunt-spritesmith/node_modules/spritesheet-templates/lib/templates** 里的模板文件在起作用。模板文件提供了很多种类型的，我这里使用的是 `scss` 相关的，所以 [Gist][3] 里暂且只提供了对应的修改内容。
```css
.icons-ipad-rightnav-fav {
  background-image: url(../images/sprites.png);
  background-position: -24px 0px;
  width: 24px;
  height: 24px;
}
@media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
  .icons-ipad-rightnav-fav {
    background-image: url(../images/sprites-2x.png);
    background-size: 48px 48px;
  }
}

.icons-ipad-rightnav-line {
  background-image: url(../images/sprites.png);
  background-position: 0px -24px;
  width: 24px;
  height: 24px;
}
@media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
  .icons-ipad-rightnav-line {
    background-image: url(../images/sprites-2x.png);
    background-size: 48px 48px;
  }
}
```
现在的目的就是想和 [利用 Compass 生成 Sprite 图片][1] 这里编译后生成的那种，公共属性只出现一次。
```css
.icons-sprite, .icons-bell, .icons-briefcase, .icons-globe, .icons-table_tennis, .icons-tree {
  background: url('../images/icons-s274ef0b305.png') no-repeat;
}
.icons-bell {
  background-position: 0 0;
  height: 24px;
  width: 24px;
}
.icons-briefcase {
  background-position: -44px 0;
  height: 21px;
  width: 24px;
}
```
在查阅了一番资料后，还是没能很好的解决这个问题，而且对于自定义模板 `cssTemplate` 这部分也没有很好的理解，所以感觉解决方案没有那么完美。
## 解决方案
对应上面的问题，大概看了下 **node_modules/grunt-spritesmith/node_modules/spritesheet-templates/lib/templates** 里对应的 `scss` 相关模板，再查阅了部分资料，暂时有了如下的解决方案：
1. **node_modules/grunt-spritesmith/node_modules/spritesheet-templates/lib/templates/scss.template.handlebars** 在这里面加入相应的代码，使生成的 `scss` 文件中，各个图标 `clsss` 只在这里输出，其中用到了属性选择符。

    ```handlebars
    {{#block "sprites-imageurl"}}
    i[class*="-ipad-"] {
      background-image: url({{{spritesheet.image}}});
      @media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
        background-image: url({{{retina_spritesheet.image}}});
      }
    }
    {{/block}}
    ```
2. 在 `scss.template.handlebars`，`scss_retina.template.handlebars` 里删掉 `name_image` 相关的配置项；在 `scss.template.handlebars` 里删掉 `sprite-image($sprite)` 相关的 `mixin` 语句。
3. 结合前面的 Grunt task，其中的 `cssVarMap` 就是和第 1 步中定义的 `block` 里的 class 名字是相互对应的。
4. 再次去生成 `scss` 文件，再编译输出，来看看最终的效果
    ![grunt-spritesmith-handbook-3.png][8]

## 相关资料
1. stackoverflow 上的相关提问 [Output a single base css class to specify background-image][6]
2. grunt-spritesmith issus [Css rule optimization][7]

暂且就这么多了，还是需要多多钻研。或许会有更好的解决办法，比如作者在上面的 issus 里提到的用 `cssTemplate`，这样不用动源模板，即使多人开发，也不会有影响。

[1]: http://hdwill.info/post/2014/compass-sprite
[2]: https://github.com/Ensighten/grunt-spritesmith#getting-started
[3]: https://gist.github.com/hdwills/82d76d5c1b715b861143d19961c40558
[4]: http://7xtjgk.com2.z0.glb.clouddn.com/grunt-spritesmith-handbook-1.png
[5]: http://7xtjgk.com2.z0.glb.clouddn.com/grunt-spritesmith-handbook-2.png
[6]: http://stackoverflow.com/questions/28253983/output-a-single-base-css-class-to-specify-background-image
[7]: https://github.com/Ensighten/grunt-spritesmith/issues/103
[8]: http://7xtjgk.com2.z0.glb.clouddn.com/grunt-spritesmith-handbook-3.png