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
