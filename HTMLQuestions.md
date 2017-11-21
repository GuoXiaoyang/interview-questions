## HTML相关问题

#### doctype(文档类型) 的作用是什么？

声明文档类型，比如HTML或者XHTML；DOCTYPE声明告诉类似的代码校验器或者浏览器应该按照什么规则集解析文档，这些“规则”就是W3C发表的文档类型定义（DTD）中包含的规则<sup><a href="http://www.jianshu.com/p/c3dcdad42e6d">1</a></sup>。

---

#### 浏览器标准模式 (standards mode) 、几乎标准模式（almost standards mode）和怪异模式 (quirks mode) 之间的区别是什么？

由于常用Chrome，所以总是在标准模式下。

模式是浏览器渲染页面使用的规则，由DOCTYPE指定。当微软开始产生与标准兼容的浏览器时，他们希望确保向后兼容性。为了实现这一点，他们IE6.0以后的版本在浏览器内嵌了两种表现模式： Standards Mode（标准模式或Strict Mode）和Quirks mode（怪异模式或兼容模式Compatibility Mode）。在标准模式中，浏览器根据W3C所定的规范来显示页面；而在怪异模式中，页面将以IE5，甚至IE4的显示页面的方式来表现，以保持以前的网 页能正常显示。

怪异模式主要体现在盒模型与W3C标准非一致。

---

#### HTML 和 XHTML 有什么区别？

XHTML是XML的子集，相比HTML语法更为严格，比如元素一定要自闭合。总结<sup><a href="https://www.sitepoint.com/differences-html-xhtml/">2</a></sup>：

**HTML:**

> - 不严格要求有起始和结束标签.
> - 只有几个空元素才要求自闭合，如` br, img, link`
> - 标签和属性不区分大小写
> - 属性可以不用引号引用
> - 一些属性值可以为空，如`checked, disabled`
> - 一些特殊字符不用转义
> - 文档需要生命HTML5 DOCTYPE

**XHTML:**

> - 所有的元素要求起始标签
> - 非空有起始标签的元素必须包含结束标签
> - 所有的元素都要自闭合
> - 标签和属性区分大小写
> - 属性必须使用双引号
> - 属性不能有空值
> - 特殊字符需要转义

---

#### 如果页面使用 'application/xhtml+xml' 会有什么问题吗？

这个是真不清楚，所以做了个实验，我们用`Hello World`来看看到底会怎样。

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">

<head>
  <title>Document</title>
</head>

<body>
  <p>Hello World!</p>
</body>

</html>
```



嗯，在Chrome与Safari下一切正常😓。参考W3C，主要在IE下会有问题：

浏览器在解析XML时会使用XML解析器。但是IE一直到IE8，都不支持XML，遇到XML时会显示下载对话框，而不是正常渲染，所以很多服务端返回XML时会使用text／html的MIME 类型。<sup><a href="https://neal.codes/blog/front-end-interview-questions-html#fn:3">3</a></sup>

---

#### 如果网页内容需要支持多语言，你会怎么做？

使用utf-8编码，使用`lang`属性来表示网站语言或元素语言；服务端设置`Content-Language`。

之前用React-Intl时，需要在JS中先导入所需语言，然后配置翻译文件，按需加载。

---

#### 在设计和开发多语言网站时，有哪些问题你必须要考虑？

* 首先当然是内容上的翻译，注意日期／时间／单复数／用词习惯／当地习俗
* 根据用户地阅读习惯适配网页，比如阅读方向／断句／字体等
* 最好用户可自行选择语言

---

#### 使用 `data- `属性的好处是什么？

使HTML能够存储自定义的数据，提供JS进行扩展操作。元素上的data属性在JS中可以用元素的dataset属性获取。

---

#### 如果把 HTML5 看作做一个开放平台，那它的构建模块有哪些？

考虑页面内容结构的话，HTML包含以下重要元素：

- `<article>`
- `<aside>`
- `<audio>`
- `<canvas>`
- `<figcaption>`
- `<figure>`
- `<footer>`
- `<header>`
- `<hgroup>`
- `<output>`
- `<section>`
- `<video>`

---

#### 请描述 cookies、sessionStorage 和 localStorage 的区别。

三者都是浏览器存储用户数据的方式。

* cookies

  大小有限，一般4kb；会过期；每次请求都会被客户端发送给服务端；偏向存储用户与服务端的会话数据

* sessionStorage

  存储空间2.5M+；浏览器不会主动将数据发送到服务端；只与当前页面相关，关闭页面则数据清除

* localStorage

  基本与sessionStorage一致，但数据与网站相关，同源页面共享数据，关闭页面不会清除数据

---

#### 请解释 `<script>`、`<script async>` 和` <script defer>` 的区别。

指定浏览器渲染页面时JS脚本的加载顺序。

* `<script>` 页面按顺序加载脚本，加载完成后继续加载其后的文档（阻塞式加载）
* `<script async>` 异步加载，与页面上其他内容一起加载，加载完成即执行；只对外部脚本有效
* ` <script defer>` 异步加载，但脚本延迟到文档完全被解析和显示之后再执行；只对外部脚本有效。不会影响页面的构造。

---

#### 为什么通常推荐将 CSS `<link> `放置在 `<head></head>` 之间，而将 JS `<script>` 放置在 `</body>` 之前？你知道有哪些例外吗？

* CSS放在`<head>`标签内是先加载css再加载文档，避免页面样式闪烁
* JS放在`</body>` 之前，是因为`<script>`是同步加载，会阻塞文档的渲染，且JS不应该在文档加载之前执行

---

#### 什么是渐进式渲染 (progressive rendering)？

这个没有用过。

顾名思义，就是不用等待内容全部下载完再渲染，而是下载一部分立即渲染该部分；包括图片的加载，可以先加载低分辨率再切换高分辨率。

总之，有内容>无内容，提升用户体验

---

#### 你用过哪些不同的 HTML 模板语言？

目前来说，只有过Node端使用[Pug](https://www.npmjs.com/package/pug)的体验

---

#### 参考资料

[1] http://www.jianshu.com/p/c3dcdad42e6d

[2] https://www.sitepoint.com/differences-html-xhtml/

[3] https://neal.codes/blog/front-end-interview-questions-html#fn:3