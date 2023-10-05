# 资源和类型安全URL

在Hello, World应用程序中，我们只定义了一个资源(`HomeR`)，但现实生活中的web应用程序通常远不止如此，它包含多个页面。让我们来看另一个例子:

```haskell
{-# LANGUAGE OverloadedStrings #-}
{-# LANGUAGE QuasiQuotes #-}
{-# LANGUAGE TemplateHaskell #-}
{-# LANGUAGE TypeFamilies #-}
import Yesod

data Links = Links

mkYesod "Links" [parseRoutes|
/ HomeR GET
/page1 Page1R GET
/page2 Page2R GET
|]

instance Yesod Links

getHomeR = defaultLayout [whamlet|<a href=@{Page1R}>Go to page 1!|]
getPage1R = defaultLayout [whamlet|<a href=@{Page2R}>Go to page 2!|]
getPage2R = defaultLayout [whamlet|<a href=@{HomeR}>Go home!|]

main = warp 3000 Links
```

总的来说，这与Hello, World非常相似。现在，我们的基础是`Links`，而不是`HelloWorld`。除了`HomeR`资源，我们还添加了`Page1R`和`Page2R`。因此，我们还添加了两个处理函数:`getPage1R`和`getPage2R`。

唯一真正的新特征是在whamlet准引号里面。我们将在[第4章]()深入研究语法，但我们可以看到以下代码创建了一个到`Page1R`资源的链接

<!-- TODO: 链接 -->

```html
<a href=@{Page1R}>Go to page 1!
```

这里要注意的重要事情是`Page1R`是一个数据构造器。通过将每个资源都做成数据构造函器，我们有了一个称为类型安全URL的功能。为了创建URL,我们只是简单地创建一个普通的Haskell值，而不是手动将字符串拼接在一起。通过使用@符号插值(`@{...}`)，Yesod在向用户发送内容之前自动将这些值渲染为文本URL。通过查看`-ddump-splices`的输出，我们可以看到这是如何实现的:
```haskell
instance RenderRoute Links where
  data Route Links = HomeR | Page1R | Page2R
    deriving (Show, Eq, Read)

  renderRoute HomeR = ([], [])
  renderRoute Page1R = (["page1"], [])
  renderRoute Page2R = (["page2"], [])
```
在`Links`的关联类型`Route`中，我们为`Page1R`和`Page2R`提供了额外的构造函数。我们现在也对renderRoute的返回值有了一个更好的了解。元组的第一部分给出了给定路由的路径片段。第二部分给出了查询字符串参数;对于几乎所有的用例，这将是一个空列表。

类型安全的URL的价值再怎么高估也不为过。在开发应用程序时，它们为您提供了极大的灵活性和健壮性。你可以随意移动URL而不会破坏链接。在第7章中，我们会看到路由可以接受参数，比如以博客文章ID为参数的博客条目URL。

假设你想要从基于数字post ID的路由切换到年/月/标题的设置。在传统的web框架中，你需要遍历博客文章路由的每个引用，并适当地进行更新。如果你错过了一个，运行时就会出现404。在Yesod中，你所要做的就是更新路由并编译: GHC将查明需要更正的每一行代码。
