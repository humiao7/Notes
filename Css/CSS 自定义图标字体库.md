# CSS自定义图标字体库

## 一、字体格式

目前最主要的几种网络字体 (web font) 格式包括 

* woff

* svg

* eot

* otf/ttf

### 1、woff

woff 是 Web Open Font Format 几个词的首字母简写。这种字体格式专门用于网上，由Mozilla联合其它几大组织共同开发。woff 字体通常比其它字体加载的要快些，因为使用了 OpenType (OTF) 和 TrueType (TTF) 字体里的存储结构和压缩算法。这种字体格式还可以加入元信息和授权信息。这种字体格式有君临天下的趋势，因为所有的现代浏览器都开始支持这种字体格式。【支持的浏览器：IE9+, Firefox3.5+, Chrome6+, Safari3.6+, Opera11.1+】

### 2、svg / svgz

Scalable Vector Graphics (Font). SVG 是一种用矢量图格式改进的字体格式，体积上比矢量图更小，适合在手机设备上使用。【支持的浏览器：Chrome4+, Safari3.1+, Opera10.0+, iOS Mobile Safari3.2+】

### 3、eot

Embedded Open Type。这是微软创造的字体格式。这种格式只在 IE6-IE8 里使用。【支持的浏览器：IE4+】

### 4、otf / ttf

OpenType Font 和 TrueType Font。这种格式容易被复制(非法的)，这才催生了 WOFF 字体格式。然而，OpenType 有很多独特的地方，受到很多设计者的喜爱。【支持的浏览器：IE9+, Firefox3.5+, Chrome4+, Safari3+, Opera10+, iOS Mobile Safari4.2+】

## 二、@font-face 规则

从 CSS3 开始，提供了新的 @font-face 规则用于在 CSS 样式中使用任何第三方字体。

### 1、@font-face 规则用法

在 @font-face 规则中，你必须首先定义字体的名称（比如 icomoon），然后指向该字体文件。

```css
@font-face {
    font-family: icomoon;
    src: url(../fonts/icomon.eot);
}
```

@font-face 引入字体规则后，可以直接通过 css 的 font-family 属性进行设置，比如为 html 元素使用字体

```html
<style>
    @font-face {
        font-family: icomoon;
        src: url(../fonts/icomon.eot);
    }
    .title {
        font-family: icomoon;
    }
</style>
```

### 2、兼容性处理

因为各个浏览器对字体格式的兼容性不一致，开发时需要考虑的全面，常用的一种方式就是将各个格式的字体都引入进来，从而实现对所有浏览器的支持。

常见的兼容性写法

```css
@font-face {
    font-family: icomoon;
    src: url(../fonts/icomoon.eot);
    src: url(../fonts/icomoon.eot#iefix) format("embedded-opentype"),
        url(../fonts/icomoon.ttf) format("truetype"),
        url(../fonts/icomoon.woff) format("woff"),
        url(../fonts/icomoon.svg#icomoon) format("svg");
    font-weight: normal;
    font-style: normal;
}
```

### 3、定义字体库图标

第一步先定义一个公共的 class 选择器，.iconfont，并设置 .iconfont 的 font-family 属性为自定义的字体。下一步定义图标 class 属性，通常需要用 content 属性配合 :before 及 :after 伪元素使用，来插入相应的图标内容。

```css
.iconfont {
    font-family: icomoon !important;
}

.css-icon-wifi:before {
    content: "\E953"
}
```

