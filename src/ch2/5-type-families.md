# 类型族(Type Families)

类型族的基本思想是声明两个不同类型之间的某种关联。假设我们要编写一个函数，安全地接收列表的第一个元素。但我们不希望它只对列表有效,而希望它对待`ByteString`像对待`Word8`列表一样。为此，我们需要引入一些关联类型来指定特定类型的内容:

```haskell
{-# LANGUAGE TypeFamilies, OverloadedStrings #-}
import Data.Word (Word8)
import qualified Data.ByteString as S
import Data.ByteString.Char8 () -- 取得一个单独的 IsString 实例

class SafeHead a where
  type Content a
  safeHead :: a -> Maybe (Content a)

instance SafeHead [a] where
  type Content [a] = a
  safeHead [] = Nothing
  safeHead (x:_) = Just x

instance SafeHead S.ByteString where
  type Content S.ByteString = Word8
  safeHead bs
    | S.null bs = Nothing
    | otherwise = Just $ S.head bs

main :: IO ()
main = do
  print $ safeHead ("" :: String)
  print $ safeHead ("hello" :: String)
  print $ safeHead ("" :: S.ByteString)
  print $ safeHead ("hello" :: S.ByteString)
```

新语法是在`class`和`instance`中添加`type`声明。我们也可以用`data`声明，这将创建一个新的数据类型，而不是引用现有的数据类型。

> 注意: 还有其他方法可以在typeclass的上下文之外使用关联类型。有关类型族的更多信息，请参见[Haskell wiki页面](http://bit.ly/ghc-type-fam)。