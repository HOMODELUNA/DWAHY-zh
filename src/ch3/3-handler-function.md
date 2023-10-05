# Handler Function

# 处理函数
我们有一个名为`HomeR`的路由，它响应`GET`请求。你如何定义你的回应?你编写了一个处理程序函数。Yesod遵循这些函数的标准命名方案:小写的方法名(例如，`GET`变成`get`)后跟路由名。在这个例子中，函数名是`getHomeR`。

您在Yesod中编写的大部分代码都存在于处理函数中。这是处理用户输入、执行数据库查询和创建响应的地方。在我们简单的例子中，我们使用`defaultLayout`函数创建一个回应。这个函数封装了网站模板中提供的内容。默认情况下，它会生成一个带有doctype, `<html>`, `<head>`, 和 `<body>` tag的HTML文件。我们将在第6章看到，这个函数可以被重写以做更多的事情。

在我们的例子中，我们传递`[whamlet|Hello, World!]`到`defaultLayout`。`whamlet`是另一个 quasiquoter。在本例中，它将hamlet语法转换为一个部件(Widget)。Hamlet是Yesod的默认HTML模板引擎。与它的兄弟Cassius、Lucius和Julius配合使用，您可以创建完全类型安全并经过编译时检查的HTML、CSS和JavaScript。我们将在[第4章]()中详细介绍这方面的内容。

Widget是Yesod的另一个基石。它们允许你创建由HTML、CSS和JavaScript组成的网站模块化组件，并在整个网站中重用它们。

[第5章]()将更深入地介绍部件。

<!-- TODO: 链接 -->

