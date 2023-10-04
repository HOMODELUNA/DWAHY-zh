# 准引号(QuasiQuotes)
准引号(QuasiQuotes)是模板Haskell的一个小扩展，它可以让我们在Haskell源文件中嵌入任意内容。例如，我们前面提到的hamletFile TH函数，它从外部文件读取模板内容。我们还编写了一个名为hamlet的准quoter对象，它内联地接受内容作为参数:
```haskell
{-# LANGUAGE QuasiQuotes #-}

[hamlet|<p>This is quasi-quoted Hamlet.|]
```

语法使用方括号和竖杠来分隔。quasiquoter的名字在左括号和第一个竖杠之间，内容在竖杠之间。

在本书中，我们会经常使用准引号，而不是TH驱动的外部文件，因为前者更容易复制粘贴。但是，在生产环境中，除了最短的输入之外，都推荐使用外部文件，因为这样可以很好地分离非Haskell语法与Haskell代码.
