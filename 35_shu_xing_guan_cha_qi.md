# 3.5 属性观察器
# 属性观察器

计算变量没有必要成为其他存储变量的看门者。因为Swift有另外一个更加明智的做法，可以给一个存储类型变量函数式的注入*setter*--*setter*观察器。这些代码在其他代码设置一个存储型变量后会调用这些函数。

给一个变量声明属性观察器的语法和声明计算变量的语法类似；你可以定义*willSet*和*didSet*函数：

```swift
var s = "what" {
  willSet {
    print(newValue)//系统的
  }
 
  didSet {
    print(oldValue)//系统的
  }
}
```
1. 这个变量只能使用*var*声明。可以被初始化，然后紧跟着花括号。
2. 这里的*willSet*函数是跟在*willSet*之后的花括号函数体。当给这个变量赋值的时候会调用这个函数，在这个变量还没有改变值之前。
3. 默认的，*willSet*在收到新值的时候会自动命名为*newValue*。你可以改变这个名字，在关键字*willSet*后面跟着圆括号，括号内写局部变量的名字。此时，变量的旧值仍然在，此时你可以获取变量的就值。
4. *didSet*函数也是在关键字之后的花括号内定义，当给这个变量赋值时，当这个变量被赋值成功后，调用这个函数。
5. 默认的，*didSet*函数会收到旧值，这个值已经被变量新值替换。你可以改变这个局部变量的名字，在*didSet*后添加大括号,里面写上局部变量的名字。这里可以获取新的变量值，更重要的是，*didSet*可以给一个存储变量设置其他值。


> **注意：**
> 当存储属性初始化的时候或者*didSet*函数改变存储属性值的时候，属性观察函数不会被调用。

实际上，在大量的实践中，我使用属性观察器而非计算属性。

下面是Apple的一个例子，证明了一个经典的场景--当一个属性改变是改变界面
```swift
var detailItem: AnyObject? {
  didSet {
    //更新界面
    self.configureView()
  }
}
```
这个是一个View类的实例属性。每次这个实例属性的值改变时，我们都需要改变界面，因为界面的一部分工作就是展示这个值。所以我每次改变这个属性的值后都会调用这个实例方法。这个实例方法读取这个实例属性的值，然后在界面展示出来。

在我的代码中有个例子不仅仅改变界面，我们还要修改穿件来的值：
```swift
var angle: CGFloat = 0 {
  didSet {
    if self.angle<0 {
      self.angle = 0
    }
    if self.angle > 5 {
      self.angle = 5
    }//改变界面
    self.transform = CGAffineTransformMakeRotation(self.angle)
  }
}
```


> **小点：**
> 计算属性不能设置属性观察器。但是它也不需要，因为有*setter*方法，因此任何额外的操作都可以在*setter*函数中完成。
























