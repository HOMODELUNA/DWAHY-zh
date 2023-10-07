

# 类型

在介绍语法之前，先了解一下涉及的各种类型。我们在介绍中提到过类型有助于保护我们免受XSS攻击。例如，假设我们有一个HTML模板，它应该显示某人的名字。它可能看起来像这样:

Before we jump into syntax, let’s take a look at the various types involved. We mentioned in the introduction that types help protect us from XSS attacks. For example, let’s say that we have an HTML template that should display someone’s name. It might look like this:
```html
<p>Hello, my name is #{name}
```

> 注意:  `{…}` 是 Shakespeare 里变量插值的方法.

`name`应该怎么处理，它的数据类型应该是什么?一种简单的方法是使用文本值，然后逐字插入。但当`name`等于下面这样的值时，会给我们带来很大的问题:
```html
<script src='http://nefarious.com/evil.js'></script>
```
我们想要的是能够对名称编码成一个实体，这样 `<`就变成了`&lt;`。

一种同样naive的方法是简单地对嵌入的每一段文本进行实体编码。如果你有一些从另一个进程生成的预先存在的HTML，会发生什么?例如，在Yesod网站上，所有的Haskell代码片段都是通过一个着色函数运行的，该函数将单词包装在适当的span标签中。如果我们对所有内容进行实体转义，代码片段将完全不可读!

相反，我们有一个`Html`数据类型。为了生成`Html`值，我们有两种api可选。`ToMarkup`类型类提供了一种将`String`和`Text`通过`toHtml`函数转换为`Html`的方法，在转换过程中自动转义实体。这就是我们想要的命名方法。对于代码片段的示例，我们将使用`preEscapedToMarkup`函数。

当你在Hamlet(Shakespeare对HTML的模板)中使用变量插值时，它会自动对其中的值应用`toHtml`调用。因此，如果你插入一个字符串，它将被实体转义，但如果你提供一个`Html`值，它将显示为未修改。在这个代码片段的例子中，我们可以使用类似`#{preEscapedToMarkup myHaskellHtml}`这样的代码进行插值。

> 注意: 上面提到的`Html`数据类型和函数都由`blaze-html`包提供。这样，Hamlet就可以与其他所有`blaze-html`包交互，并提供生成`blaze-html`值的通用解决方案。此外，我们还可以享受`blaze-html`惊人的性能。

类似地，还有`Css/ToCss`和`Javascript/ToJavascript`。它们在编译时检查文件完整性，以确保我们没有意外地在CSS中插入一些HTML。

> 注意: CSS这里的另一个优势是有一些用于颜色和单位的辅助数据类型。例如:
> ```css
> .red { color: #{colorRed} }
> ```
> 更多细节请参考[Haddock 文档](http://www.stackage.org/package/shakespeare)
