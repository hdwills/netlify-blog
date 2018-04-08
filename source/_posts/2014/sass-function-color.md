---
title: Sass 学习笔记 - 颜色函数
date: 2014-06-17 19:00:00
description: Sass 学习笔记 - 颜色函数
category: Tech
tags: Sass
---
这篇主要是记录下日常工作中常用的颜色函数，虽然不是射击狮但是要有一颗爱设计的心！

自从开始在项目中使用 Sass 之后，对其各个基本功能都得接触到。熟练使用工具，事半功倍提高搬砖效率，老板要是看到我这话，会给加薪吗？

在接手设计稿之后，不一定各处都给你标注的非常具体详细。难免有时候得自己动手，借助别的工具去获取颜色值，然后再相应的去拓展。有时候也会用到比如相近色、互补色，调节饱和度、亮度……是不是听着都专业！毕竟不是以设计为主，其实我常用的就是 `lighten()`、`darken()`、`saturate()`，`desaturate()` 这几个而已 >_<，这个主要是用在 `background`、`border`、`:hover`，要是在以前，我肯定要在取色工具里，然后在基础色上调节，自从有了颜色函数之后，就省事儿多了，继续停留在编辑器里搞定。

[Sass 颜色函数文档](http://sass-lang.com/documentation/Sass/Script/Functions.html)、[Compass 颜色函数文档](http://compass-style.org/reference/compass/helpers/colors/)，下面就介绍几个我常用的：

```css
// 以下使用的是定义的这些颜色变量，在颜色函数中既可以使用变量，也可以使用颜色值
$primary: #eb594a;
$dark: #2e3740;
$price: #f13536;
```

##### darken($color, $amount) & lighten($color, $amount)
```css
color: darken($primary, 15%); // 亮度降低15%
background-color: lighten($dark, 10%); // 亮度提高10%
// === Compile ===
.func1 {
  color: #d12817;
  background-color: #43505e; }
```

##### desaturate($color, $amount) & saturate($color, $amount)
```css
color: desaturate($primary, 15%); // 饱和度降低15%
background-color: saturate($dark, 10%); // 饱和度提高10%
// === Compile ===
.func2 {
  color: #dc6559;
  background-color: #293746; }
```

##### complement($color) 互补 & adjust-hue($color, $degrees) 色调调节
```css
color: complement($price); // 基于 HSL 颜色标准将原色值旋转 180 度后获得相对应的颜色值
background: adjust-hue($price, 180deg); // 改变色调的度数改变颜色值，这里的 180deg 正好和上面的 complement 具有相同的功能
// === Compile ===
.func3 {
  color: #35f1f0; }
.func4 {
  color: #35f1f0; }
```

##### invert($color) 反色
反色函数制作红、绿、蓝的颜色值的反向，不可以改变透明度。
```css
color: invert($price);
// === Compile ===
.func5 {
  color: #0ecac9; }
```

##### transparentize($color, $amount) 透明化 & fade-out($color, $amount) 渐隐
这里在看文档的时候，有点不太明白，等明天在调色板里看看效果对比吧。

和下面介绍的函数效果正好相反，效果是让颜色变得更加不透明，但是为何还要加个参数呢，大概是在原有色值上“加重”这么多？
```css
color: transparentize($primary, .3);
background-color: fade-out($primary, .3);
// === Compile ===
.fun6 {
  color: rgba(235, 89, 74, 0.7);
  background-color: rgba(235, 89, 74, 0.7); }
```

##### rgba($red, $green, $blue, $alpha)
在看了编译之后的结果，是不是感觉和 rgba() 函数的效果一致呢？好，那就顺带说一下这个函数吧：
```css
color: rgba($primary, .3);
// === Compile ===
color: rgba(235, 89, 74, 0.3);
```

##### opacify($color, $amount) 不透明化 & fade-in($color, $amount) 渐现
实现的效果正好和上面的函数相反
```css
color: fade-in($primary, .3);
background-color: opacify($primary, .3);
// === Compile ===
.fun7 {
  color: #eb594a;
  background-color: #eb594a; }
```

以上这 5 个函数都需要传递 alpha 值，必须小与 1（0 为完全透明）。不知大家发现了没，其中那两对效果相反的函数，其中透明化函数，传值和编译后的透明度值互补；而不透明化函数，传值后编译的结果却和原色值相同，不传值就会编译报语法错误。这个是个疑问，在书中翻了一下，只有很随意的 demo，没有做详细解释。看来还是要多实践，不然书就是白看了。去翻了下官方文档，才发现了其中的缘由：其中 color 这个参数，若不是以 rgba 格式写出 alpha 的值，那就默认是 1；与后面给出的 amount 参数相加或者相减，即使超过 1，结果还是 16 进制的色值。

```css
color: fade-in(rgba(235, 89, 74, .3), .1);
background-color: opacify(rgba(235, 89, 74, .9), .3);
// === Compile ===
color: rgba(235, 89, 74, 0.4);
background-color: #eb594a;
```
##### grayscale($color)
将参数里的颜色值转换为相应的灰度颜色
```css
color: grayscale(rgba(235, 89, 74, .3));
// === Compile ===
color: rgba(155, 155, 155, 0.3);
```

简单的函数简介就先记录这么几个，还有几个参数稍多的函数，目测下面这几个感觉更负责，伴随着功能那就更强大了！有兴趣的可以去官方文档围观学习。
##### mix($color1, $color2, [$weight])
##### scale-color($color, [$red], [$green], [$blue], [$saturation], [$lightness], [$alpha])
##### adjust-color($color, [$red], [$green], [$blue], [$hue], [$saturation], [$lightness], [$alpha])