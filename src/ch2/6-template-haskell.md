# 模板Haskell

模板Haskell (Template Haskell)是一种代码生成方法。我们在Yesod的许多地方使用它来减少样板代码，并确保生成的代码是正确的。模板Haskell本质上是生成Haskell抽象语法树(AST)的Haskell程序。

> 注意: 实际上，TH还有更强大的功能，因为它实际上可以内省(introspect)代码。然而，我们在Yesod不使用这些功能。

不幸的是,编写代码可能很棘手.类型安全帮不了你太多,很可能你编写的TH会生成无法编译的代码。不过这只是Yesod开发人员的问题，而不是你用户的问题。在开发过程中，我们使用大量单元测试来确保生成的代码是正确的。作为用户，你需要做的就是调用这些已经存在的函数。例如，要包含外部定义的Hamlet模板(第4章讨论过)，可以这样写:
```haskell
$(hamletFile "myfile.hamlet")
```
紧跟在括号后面的美元符号告诉GHC，后面是一个模板Haskell函数。然后编译器运行其中的代码并生成Haskell AST，然后对其进行编译。是的，这种事甚至能用来[玩玄学](http://bit.ly/haskell-temp)。

一个不错的技巧是TH代码允许执行任意的IO操作，因此我们可以将一些输入放在外部文件中，并在编译时对其进行解析。一个例子是在编译时就检查HTML、CSS和JavaScript模板。

如果模板Haskell代码用于生成声明，并且放在文件的顶层，可以去掉美元符号和括号。即:
```haskell
{-# LANGUAGE TemplateHaskell #-}
-- 正常函数定义,没什么特别的
myFunction = ...
-- 包含了一些 TH 代码
$(myThCode)
-- 等价于上面
myThCode
```

看看模板Haskell为您生成了什么代码会加深你的理解。你可以使用`-ddump-splices` GHC选项。

> 注意: Template Haskell还有许多其他特性没有在这里介绍。有关更多信息，请参阅[Haskell wiki页面](QuasiQuotes)。

模板Haskell引入了一种称为 _阶段限制(stage restriction)_ 的东西，本质上是指模板Haskell splice之前的代码不能引用模板Haskell中或其后的代码。这有时需要你重新排列代码。同样的限制也适用于准引号(QuasiQuotes)。

开箱即用，Yesod擅长使用代码生成来避免样板，但是不带模板Haskell而使用Yesod是完全可以接受的。[第20章]()有更多相关信息。

<!-- TODO: 第20章写完之后添加链接 -->