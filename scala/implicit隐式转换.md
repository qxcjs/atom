[TOC]
### 使用方式
1. 将方法或变量标记为implicit
2. 将方法的参数列表标记为implicit
3. 将类标记为implicit

Scala支持两种形式的隐式转换：
隐式值：用于给方法提供参数
隐式视图：用于类型间转换或使针对某类型的方法能调用成功

### 隐式函数的应用
我们可以随便的打开scala函数的一些内置定义，比如我们最常用的map函数中->符号，看起来很像PHP等语言。
但实际上->确实是一个ArrowAssoc类的方法，它位于scala源码中的Predef.scala中。下面是这个类的定义：
```scala
final class ArrowAssoc[A](val __leftOfArrow: A) extends AnyVal {  
  // `__leftOfArrow` must be a public val to allow inlining. The val  
  // used to be called `x`, but now goes by `__leftOfArrow`, as that  
  // reduces the chances of a user's writing `foo.__leftOfArrow` and  
  // being confused why they get an ambiguous implicit conversion  
  // error. (`foo.x` used to produce this error since both  
  // any2Ensuring and any2ArrowAssoc pimped an `x` onto everything)  
  @deprecated("Use `__leftOfArrow` instead", "2.10.0")  
  def x = __leftOfArrow  

  @inline def -> [B](y: B): Tuple2[A, B] = Tuple2(__leftOfArrow, y)  
  def →[B](y: B): Tuple2[A, B] = ->(y)  
}  
@inline implicit def any2ArrowAssoc[A](x: A): ArrowAssoc[A] = new ArrowAssoc(x)  
```
我们看到def ->[B] (y :B)返回的其实是一个Tuple2[A,B]类型。
我们定义一个Map:
```scala
scala> val mp = Map(1->"game1",2->"game_2")  
mp: scala.collection.immutable.Map[Int,String] = Map(1 -> game1, 2 -> game_2)  
```
这里 1->"game1"其实是1.->("game_1")的简写。
这里怎么能让整数类型1能有->方法呢。
这里其实any2ArrowAssoc隐式函数起作用了，这里接受的参数[A]是泛型的，所以int也不例外。
调用的是：将整型的1 implicit转换为 ArrowAssoc(1)
看下构造方法，将1当作__leftOfArrow传入。
->方法的真正实现是生产一个Tuple2类型的对象(__leftOfArrow,y ) 等价于(1, "game_id")
这就是一个典型的隐式转换应用。

其它还有很多类似的隐式转换，都在Predef.scala中：
例如：Int，Long，Double都是AnyVal的子类，这三个类型之间没有继承的关系，不能直接相互转换。
在Java里，我们声明Long的时候要在末尾加上一个L，来声明它是long。
但在scala里，我们不需要考虑那么多，只需要：
```scala
scala> val l:Long = 10  
l: Long = 10  
```
这就是implicit函数做到的，这也是scala类型推断的一部分，灵活，简洁。
其实这里调用是：
val l : Long = int2long(10)

### 参考文章
1. [implicit详解](http://www.jianshu.com/p/1d119c937015)
