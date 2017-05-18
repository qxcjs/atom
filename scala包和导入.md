[TOC]

Scala默认导入:
`java.lang._`
`scala_`
`scala.Predef`对象下的所有成员也会被隐式导入。

## 包
1. 花括号的包名中封装一个或多个类
2. 一个类中可以有多个包
3. 包中可以嵌套包

## 导入
1. 导入多个类
```scala
import java.io.{File,IOException,FileNotFoundException}
import java.io._
```
2. 随处添加import语句，包括类的头部，类或对象的内部，一个方法或者一段代码块中
3. 引入类，包或者对象
4. 引入时隐藏并且重命名所引入的成员
```scala
import java.util.{ArrayList => JavaList}

val list = new JavaList[String]
```
5. 静态导入
```scala
import java.lang.Math._
```

## 特质
1. 除非实现特质的类是一个抽象类，否则它必须实现特质的所有抽象方法和抽象字段。
2. 特质中可以包含实现方法和抽象方法，实际字段和抽象字段。
3. 一个特质的字段可以声明为var或者val。在一个子类（或子特质）中覆写特质中的var字段时，不需要override关键字，但覆写特质中的val字段时，需要使用override关键字。
4. 通过继承来限制特质的使用范围
5. 限定特质只可用于指定类型的子类
6. 限定特质只能被添加到拥有指定方法的类型中
