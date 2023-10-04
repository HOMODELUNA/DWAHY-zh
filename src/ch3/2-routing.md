# Routing

与大多数现代web框架一样，Yesod遵循前置控制器模式。这意味着对Yesod应用程序的每个请求都进入同一处，并从那里进行路由。与此相反，在PHP和ASP等系统中，您通常创建许多不同的文件，web服务器自动将请求定向到相关文件。

此外，Yesod使用声明式风格来指定路由。在我们之前的例子中，它看起来是这样的:
```haskell
mkYesod "HelloWorld" [parseRoutes|
/ HomeR GET
|]
```
> 注意: `mkYesod` 是模板 Haskell 函数, 而 `parseRoutes` 是准引号.

换句话说，上述代码只是在Hello World应用程序中创建了一个称为`homeR`的路由。它应该监听 `/` (应用的根目录)的请求,并回答`GET`请求。我们把`HomeR`称为资源(resource)，这也是`R`后缀的来源。

> 注意: 资源名称上的R后缀只是一种惯例，但它是一种普遍遵循的惯例。它使代码更容易阅读和理解。

这里,`mkYesod`这个TH函数生成了相当多的代码:一个路由数据类型，解析/渲染函数，一个分发函数和一些辅助类型。我们会在[第7章]()更详细地讨论这个问题，但通过使用GHC选项`-ddump-splices`，我们可以立即看到生成的代码。下面是它的简化版本:

<!-- TODO: 第七章链接 -->

```haskell
instance RenderRoute HelloWorld where
  data Route HelloWorld = HomeR
    deriving (Show, Eq, Read)
  renderRoute HomeR = ([], [])

instance ParseRoute HelloWorld where
  parseRoute ([], _) = Just HomeR
  parseRoute _ = Nothing

instance YesodDispatch HelloWorld where
  yesodDispatch env req =
  yesodRunner handler env mroute req
    where
      mroute = parseRoute (pathInfo req, textQueryString req)
      handler =
        case mroute of
          Nothing -> notFound
          Just HomeR ->
            case requestMethod req of
              "GET" -> getHomeR
              _ -> badMethod

type Handler = HandlerT HelloWorld IO
```

> 注意: 除了使用`-ddump-splices`之外，为应用程序生成Haddock文档，查看生成了哪些函数和数据类型,这也是有益的.

我们可以看到`RenderRoute`类型类定义了一个 _关联数据类型_ ，为我们的应用程序提供了路由。在这个简单的例子中，我们只有一个路由:`HomeR`。在实际的应用程序中，我们将有更多的路由，它们将比我们的`HomeR`更复杂。

`renderRoute`获取一个路由，并将其转换为路径部分和查询字符串参数。同样，我们的例子很简单，因此代码也同样简单:两个值都是空列表。

`ParseRoute`提供了逆函数`parseRoute`。在这里我们看到了依赖模板Haskell的第一个强大动机:它确保路由的解析和渲染正确对应,而这组代码在手写时很容易变得难以保持同步。通过依赖代码生成，我们让编译器(和Yesod)为我们处理这些细节。

`YesodDispatch`提供了一种获取输入请求并将其传递给适当处理程序函数的方法。这个过程本质上是:

1. 解析请求。
2. 选择一个处理函数(handler)。
3. 运行该handler。

代码生成遵循一种简单的格式，用于将路由与处理程序函数名匹配，我将在下一节中描述。

最后，我们用一个简单的类型别名定义`Handler`，这使我们的代码更容易编写。

这里发生的事情比我们描述的要多得多。生成的调度代码实际上使用[观察模式(View Patterns)](https://gitlab.haskell.org/ghc/ghc/-/wikis/view-patterns)语言扩展来提高效率;此外，还有更多的类型类的实例被创建，有其他情况需要处理，如子站点(subsite)。本书后面会详细介绍，特别是[第18章]()。
<!-- TODO:链接 -->
