现在这个东西已经过时了，但是还是记录一下，万一什么时候就用上了呢

----

## IE 中的 haslayout

特别注意：```hasLayout``` 在 ```IE 8``` 及之后的 ```IE``` 版本中已经被抛弃，所以在实际开发中只需针对 ```IE 8``` 以下的浏览器为某些元素触发 ```hasLayout```

```Layout``` 是 ```IE``` 浏览器渲染引擎的一个内部组成部分，在 ```IE``` 浏览器中，一个元素要么自己对自身的内容进行组织和计算大小， 要么依赖于包含块来计算尺寸和组织内容，为了协调这两种方式的矛盾，渲染引擎采用了 ```"hasLayout"``` 属性，属性值可以为 ```true``` 或 ```false```

当一个元素的 ```"hasLayout"``` 属性值为 ```true``` 时，我们说这个元素有一个布局（```layout```），或拥有布局，可以通过 ```hasLayout``` 属性来判断一个元素是否拥有 ```layout```，如 ```object.currentStyle.hasLayout```

```hasLayout``` 与 ```BFC``` 有很多相似之处，但 ```hasLayout``` 的概念会更容易理解

在 ```Internet Explorer``` 中，元素使用 "布局" 概念来控制尺寸和定位，分为拥有布局和没有布局两种情况

* 拥有布局的元素由它控制本身及其子元素的尺寸和定位

* 而没有布局的元素则通过父元素（最近的拥有布局的祖先元素）来控制尺寸和定位

而一个元素是否拥有布局则由 ```hasLayout``` 属性告知浏览器，它是个布尔型变量，```true``` 代表元素拥有布局，```false``` 代表元素没有布局，简而言之，```hasLayout``` 只是一个 ```IE``` 下专有的属性，```hasLayout``` 为 ```true``` 的元素浏览器会赋予它一系列的效果



## 怎样触发 layout

一个元素触发 ```hasLayout``` 会影响一个元素的尺寸和定位，这样会消耗更多的系统资源，因此 ```IE``` 设计者默认只为一部分的元素触发 ```hasLayout``` （即默认有部分元素会触发 ```hasLayout```，这与 ```BFC``` 基本完全由开发者通过特定 ```CSS``` 触发并不一样），这部分元素如下：

```html
<html>, <body>
<table>, <tr>, <th>, <td>
<img>
<hr>
<input>, <button>, <select>, <textarea>, <fieldset>, <legend>
<iframe>, <embed>, <object>, <applet>
<marquee>
```

除了 ```IE``` 默认会触发 ```hasLayout``` 的元素外，```Web``` 开发者还可以使用特定的 ```CSS``` 触发元素的 ```hasLayout```

通过为元素设置以下任一 ```CSS``` ，可以触发 ```hasLayout``` （即把元素的 ```hasLayout``` 属性设置为 ```true```）

```css
display: inline-block

height: (除 auto 外任何值)

width: (除 auto 外任何值)

float: (left 或 right)

position: absolute

writing-mode: tb-rl

zoom: (除 normal 外任意值)

min-height: (任意值)

min-width: (任意值)

max-height: (除 none 外任意值)

max-width: (除 none 外任意值)

overflow: (除 visible 外任意值，仅用于块级元素)

overflow-x: (除 visible 外任意值，仅用于块级元素)

overflow-y: (除 visible 外任意值，仅用于块级元素)

position: fixed
```

对于内联元素（可以是默认被浏览器认为是内联元素的 ```span``` 元素，也可以是设置了 ```display: inline``` 的元素），```width``` 和 ```height``` 只在 ```IE5.x``` 下和 ```IE6``` 或更新版本的 ```quirks``` 模式下能触发元素的 ```hasLayout```，但是对于 ```IE6```，如果浏览器运行于标准兼容模式下，内联元素会忽略 ```width``` 或 ```height``` 属性，所以设置 ```width``` 或 ```height``` 不能在此种情况下令该元素触发 ```hasLayout```

但 ```zoom``` 除了在 ```IE 5.0``` 中外，总是能触发 ```hasLayout```，```zoom``` 用于设置或检索元素的缩放比例，为元素设置 ```zoom: 1``` 既可以触发元素的 ```hasLayout``` 同时不会对元素造成多余的影响，因此综合考虑浏览器之间的兼容和对元素的影响， 建议使用 ```zoom: 1``` 来触发元素的 ```hasLayout``` 


## 解决的问题

```hasLayout``` 表现出来的特性跟 ```BFC``` 很相似，所以可以认为是 ```IE``` 中的 ```BFC```，规则几乎都遵循，所以在之前 ```BFC``` 当作遇到的问题在IE里都可以通过触发 ```hasLayout``` 来解决

虽然 ```hasLayout``` 也会像 ```BFC``` 那样影响着元素的尺寸和定位，但它却又不是一套完整的标准，并且由于它默认只为某些元素触发，这导致了 ```IE``` 下很多前端开发的 ```bugs```

触发 ```hasLayout``` 更大的意义在于解决一些 ```IE``` 下的 ```bugs```，而不是利用它的一些 "副作用" 来达到某些效果，另外由于触发 ```hasLayout``` 的元素会出现一些跟触发 ```BFC``` 的元素相似的效果，因此为了统一元素在 ```IE``` 与支持 ```BFC``` 的浏览器下的表现，建议为触发了 ```BFC``` 的元素同时触发 ```hasLayout``` ，当然还需要考虑实际的情况，也有可能只需触发其中一个就可以达到表现统一