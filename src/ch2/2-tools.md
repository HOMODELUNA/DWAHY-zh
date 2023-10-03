# 工具

Haskell开发主要需要两个工具。Glasgow Haskell编译器(Glasgow Haskell Compiler, GHC)是标准的Haskell编译器，也是Yesod官方支持的唯一编译器。你还需要Cabal，这是标准的Haskell构建工具。我们不仅使用Cabal构建我们的本地代码，而且它可以自动从Hackage (Haskell包存储库)下载和安装依赖项。

Yesod网站提供了最新的[快速入门指南](http://www.yesodweb.com/page/quickstart)，包括如何安装和配置各种工具的信息。强烈建议您遵循这些说明。特别地，指南中的步骤步骤利用[Stackage](http://www.stackage.org/)来避免许多常见的依赖解析问题。

如果你决定自己安装工具，请确保避免这些常见的陷阱:

- Yesod附带的一些JavaScript工具安装时需要构建工具`alex` 和 `happy`，这些可以通过`cabal install alex happy`添加。

- Cabal将可执行文件安装到用户指定的目录，该目录需要添加到您的`PATH`中。确切的位置特定于操作系统。请确保[添加正确的目录](http://www.stackage.org/install)。

- 在Windows，很难从源代码安装`network`包，因为它需要一个POSIX shell。安装[Haskell Platform](http://hackage.haskell.org/platform/)可以避免这个问题。

- 在Mac OS X上，有多个可用的C预处理器:一个来自Clang，一个来自GCC。许多Haskell库都依赖于GCC预处理器。同样的,Haskell Platform正确地设置了一切。

- 一些Linux发行版——特别是Ubuntu——通常GHC和Haskell Platform的包会过时。这些可能不再被当前版本的Yesod支持。请查看快速入门指南中的最低版本要求。

确保你已经安装了所有必要的系统库。这通常由Haskell平台自动处理，但在Linux发行版上可能需要额外的工作。如果你收到关于缺少库的错误消息，通常只需要apt-get install或yum install相关的库。

正确设置好工具链后，还需要安装一些Haskell库。对于本书的绝大部分内容，下面的命令将安装你需要的所有库:
```
cabal update && cabal install yesod yesod-bin persistent-sqlite yesod-static
```
同样，请参阅[快速入门指南](http://www.yesodweb.com/page/quickstart)以获取最新和准确的信息。
