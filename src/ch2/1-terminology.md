# 术语

即使那些熟悉Haskell语言的人偶尔也会对术语感到困惑。我们来确定一些贯穿本书的基本术语。

_数据类型(Data Type)_ : 这是像Haskell这样的强类型语言的核心构建块之一。有些数据类型(例如Int)可以被视为基本值，而其他数据类型则在基本值的基础上构建更复杂的值。例如，你可以这样表示一个人:
```haskell
data Person = Person Text Int
```

这里，`Text`给出的是人的名字，`Int`给出的是人的年龄。由于简单，这个特定的示例类型将在本书中反复出现。

有三种创建新数据类型的方法:

- `type`声明，如`type GearCount = Int`。这只是为现有类型创建了一个同义词。类型系统不会阻止你在需要`GearCount`处使用`Int`。使用它可以使代码更能自解释。

- `newtype`声明，如`newtype Make = Make Text`。在这种情况下，不能用`Text`充当`Make`,编译器会阻止你。`newtype`包装器在编译期间检查完就消失掉，不会引入运行时开销。

- `data`声明，如`Person`。还可以创建代数数据类型(adt)------ 例如，`data Vehicle = Bicycle GearCount | Car Make Model`。

_数据构造器(Data Constructor)_ : 在我们的例子中，`Person`、`Make`、`Bicycle`和`Car`都是数据构造器。



_类型构造器(Type Constructor)_ : 在我们的例子中，`Person`、`Make`和`Vehicle`都是类型构造器。



_类型变量(Type Variable)_ : 考虑数据类型`data Maybe a =Some a | Nothing`。在本例中，`a`是类型变量。



> 注意 : 在`Person`和`Make`数据类型中，我们的数据类型和数据构造器具有相同的名称。这是处理只有一个数据构造器的数据类型时的常见做法。但这不是必需的;你可以给数据类型和数据构造函数起不同的名字。
