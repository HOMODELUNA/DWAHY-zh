# Hello, World
让我们开始吧.先编写一个仅仅输出“Hello, World”的简单网页:
```haskell
{-# LANGUAGE OverloadedStrings #-}
{-# LANGUAGE QuasiQuotes #-}
{-# LANGUAGE TemplateHaskell #-}
{-# LANGUAGE TypeFamilies #-}
import Yesod

data HelloWorld = HelloWorld

mkYesod "HelloWorld" [parseRoutes|
/ HomeR GET
|]

instance Yesod HelloWorld

getHomeR :: Handler Html
getHomeR = defaultLayout [whamlet|Hello, World!|]

main :: IO ()
main = warp 3000 HelloWorld
```
将上述代码保存在 _helloworld.hs_ 中。然后用`runhaskell helloworld.hs`运行它。你就会得到一个运行在端口3000上的web服务器。如果您将浏览器指向http://localhost:3000，您将得到以下HTML:

```html
<!DOCTYPE html>
<html><head><title></title></head><body>Hello, World!</body></html>
```
我们将在本章的其余部分中提到这个例子.