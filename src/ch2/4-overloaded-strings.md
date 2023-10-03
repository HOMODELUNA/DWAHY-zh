# 重载字符串(Overloaded Strings)

`"hello"`是什么类型?传统上，它是`String`类型，而`String`定义为`String = [Char]`。不幸的是，这有一些缺陷:

- 这用来表示文本数据时非常低效。我们需要为每个`cons`单元分配额外的内存，每个字符本身还占用一个完整的机器字长。
- 有时，我们有数据类似字符串但实际上不是文本，例如`ByteString`和`HTML`。

为了解决这些限制，GHC有一个名为`OverloaddStrings`的语言扩展。启用后，字面量字符串不再是单态类型`String`;而是是`IsString a -> a`，其中`IsString`定义为:

```haskell
class IsString a where
  fromString :: String -> a
```

Haskell中有很多类型都有`IsString`实例，比如`Text`(一种更高效的打包`String`类型)、`ByteString`和`Html`。实际上，本书中的每个示例都假定启用着这个语言扩展。

不幸的是，这个扩展有一个缺点:它有时会混淆GHC的类型检查器。例如，假设我们使用以下代码:
```haskell
{-# LANGUAGE OverloadedStrings, TypeSynonymInstances, FlexibleInstances #-}
import Data.Text (Text)
class DoSomething a where
  something :: a -> IO ()

instance DoSomething String where
  something _ = putStrLn "String"

instance DoSomething Text where
  something _ = putStrLn "Text"

myFunc :: IO ()
  myFunc = something "hello"
```
这个程序会打印出`String`还是`Text`?目前还不清楚。因此，你需要给出一个显式的类型注解，指定`"hello"`应该被视为字符串还是文本。

> 注意: 在某些情况下，你可以通过使用`ExtendedDefaultRules`语言扩展来克服这些问题，不过我们将在本书中显式说明类型，而不依赖于默认值。

 
