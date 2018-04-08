---
title: viewport 使用笔记 - 2
date: 2013-12-21 16:00:00
description: viewport，用 CSS 做设备适配
category: Tech
tags:
  - viewport
  - responsive
---

接着上次的 [viewport 使用笔记 - 1](http://hdwill.info/post/2013/viewport-metatag)，文中只说了在 metatag 中设置页面的 viewport，这只是方式一，等于将控制权交给了 HTML。下面就来记录一下另一种在 CSS 中定义的方式 - @viewport，这样一来布局职能就属于 CSS 来掌控了，或许这种设备适配方式才是正确的吧。

> viewport meta标签并不具有“规范性”，即它不是W3C的正式标准，也非Web标准。
> @viewport 目前也处于草案阶段
> via - [W3C - css-device-adapt](http://dev.w3.org/csswg/css-device-adapt/)

## @viewport
使用 @viewport CSS 规则控制 viewport，与在 HTML 中的 meta 标签效果一样。

```css
@media screen and (max-width: 400px) {
    @-ms-viewport{
        width: 320px;
        /* the viewport for small devices is set to 320px  */
    }
    @-o-viewport {
        width: device-width;
    }
    @viewport {
        width: device-width;
    }
}
```
`@viewport` 支持情况，仅 `IE10+, Opera11+, Opera Mobile` 支持，且需要加上私有前缀。

## 说明
用默认 width 的值 `device-width`，来设置 viewport 的宽度。与 meta 标签中设置的 `content="width=device-width"` 效果相同。

上面这个 example 中 `@-ms-viewport` 这里需要着重说明下，**IE10会忽视掉那些宽度小于400像素的元窗口标签（在“snap mode”下）**，添加浏览器前缀，确保各浏览器里的渲染效果是一致的。

## 实践
在项目中使用了 [twitter bootstrap](http://getbootstrap.com/)，里面的 grid system，其中也有 Media queries 这一部分。看下了它对不同终端的划分情况，大致分为 4 个区间，自己对各终端的适配也没有足够的经验，所以在项目的 grid 构建中，也是按照他们的规格去尝试，下面对这一部分的使用记录一下。

* ### 规格说明
    * `bk-col-width: 16px` (栅格宽度)
    * `bk-col-gutter: 5px` (间隙宽度)
    * `bk-screen-sm: 750px` (移动设备)
    * `bk-screen-md: 1000px` (平板，普屏PC)
    * `bk-screen-lg: 1200px` (宽屏设备)

    这些变量已经在 `_grid.scss` 中定义好了，方便后期重新定制。

* ### 初步规划
    为了满足响应式布局，目前做了如上所示的3种规格，适应不同分辨率的终端，以下是具体说明

    * 屏幕尺寸 `max-width: 750px`，即小于或等于 `749px`，横向滚动条出现
    * 屏幕尺寸 `min-width: 750px ~ max-width: 1000px`，即 `750px ~ 999px`，内容区域 `bk-con = 692px(bk-col27)` 此分辨率适应移动终端
    * 屏幕尺寸 `min-width: 1000px ~ max-width: 1200px`，即 `1000px ~ 1199px`，内容区域 `bk-con = 926px(bk-col36)` 平板，普屏PC
    * 屏幕尺寸 `min-width: 1200px`，即大于或等于 `1200px`，内容区域 `bk-con = 1160px(bk-col45)` 宽屏PC
* ### 备注
    后期经过实际的应用测试，发现此栅格并不适合各种场景，特别是 `bk-col-gutter: 5px`，这样一来左右相邻的两个模块间隙只有 `10px`，感觉留白过少，所以此处还是应该再做调整。


## 参考资料
* [Web Platform Docs - @viewport](http://docs.webplatform.org/wiki/css/atrules/@viewport)
* [@-ms-viewport rule](http://msdn.microsoft.com/en-us/library/ie/hh869615.aspx)
* [IE10里的捕捉模式和响应式设计](http://www.csdn.net/article/2013-01-24/2813928-IE-10)

