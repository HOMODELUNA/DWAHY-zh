# 总结

每个Yesod应用程序都是围绕一个"基石"数据类型构建的。我们给该数据类型关联上一些资源,并定义一些处理函数，之后Yesod处理所有路由事项。这些资源同时是数据构造函数，它赋予我们类型安全的url。

构建在WAI之上，Yesod应用程序可以运行于许多不同的后端。对于简单的应用程序，`warp`函数提供了一种使用warp web服务器的便捷方式。对于快速开发，使用`yesod devel`是很好的选择。当你准备投入生产环境时，你有充分的力量和灵活性配置 Warp (或任何其他WAI处理程序)，以满足您的需求。

在Yesod中开发中，我们在编码风格上有许多选择:准引用或外部文件，`warp`或`yesod devel`，等等。本书中的示例刻意使用了最容易复制粘贴的选项，但当你开始构建真正的Yesod应用程序时，还有更强大的选项可用。