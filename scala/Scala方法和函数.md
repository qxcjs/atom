[TOC]
## 一、方法、函数、过程

![](../assets/markdown-img-paste-20170506170450895.png)

### 1. 方法

```scala
//定义了一个函数，函数中使用return返回结果
scala> def add(a:Int,b:Int):Int={return a+b}
add: (a: Int, b: Int)Int

scala> add(1,2)
res3: Int = 3

//可以省去return，scala会将最后一个执行语句
//作为函数的返回值
scala> def add(a:Int,b:Int):Int={a+b}
add: (a: Int, b: Int)Int

//省去返回值类型，scala会自动进行类型推断，只有一个表达式时可以省略函数体的大括号
scala> def add(a:Int,b:Int)=a+b
add: (a: Int, b: Int)Int

scala> add(1,2)
res4: Int = 3
```

### 2. 函数
方法对对象进行操作，函数不是。在java中只能使用静态方法来模拟函数。

- 函数定义

```scala
//函数名，参数，函数体
//块中的`最后一个表达式`的值就是函数的返回值
def fac(n:Int)={
  var r = 1
  for(i <- 1 to n) r=r*i
  r
  print(r)
}
println(fac(3)) // 结果 6()
```

你必须给出所有参数的类型。不过，只要函数不是递归的，就不需要指定返回值类型。Scala编译器可以通过=号右边的表达式类型推断出返回类型。

```scala
def abs(x:Double)=if (x>0) x else -x
println(abs(10.0)) // 结果 10.0
```

对于递归函数，我们必须指定返回类型。

```
def fac(n:Int):Int = if(n<=0) 1 else n*fac(n-1)
println(fac(3)) //结果 6
```

### 3. 过程
&emsp;&emsp;Scala对于不返回值的函数有特殊的表示法。如果函数体包含在花括号当中但是没有前面的=号，那么返回类型就是Unit。这样的函数被称作过程(procedure)。过程不返回值，我们调用它仅仅是为了它的副作用。如单纯的打印:

```scala
--------
hello
-------

    box("hello")
    def box(s:String){
      var border = "-" * s.length +"--\n";
      println(border + s +"\n"+ border)
    }
```

## 二、 参数
### 1. 默认参数和使用参数名
在调用某些函数时并不显示的给出所有参数值，对于这些函数可以使用默认参数

```scala
def decorate(str:String, left:String="[", right:String="]")= left + str + right

//直接使用默认参数
println(decorate("hello")) // 结果 [hello]

//修改默认参数
println(decorate("hello","(",")")) // 结果 (hello)

// 在提供参数时指定参数名称，参数无顺序
println(decorate(left="<",str="hello",right=">")) //结果 <hello>

// 可以混用未命名参数和带名参数
```

### 2. 可变参数
**注意：一个方法只允许有一个可变参数，且该参数必须是方法签名中的最后一个参数。**

```scala
def sum(args:Int*)={
  var result=0
  for(arg <- args) result +=arg
  result
}
println(sum(3,4,5)) //结果 12
//这个方法可以传入0-多个参数。
```
函数实际得到的是一个SEQ的参数，SEQ可以使用for循环来访问每一个元素。

**如果已经有一个值得序列，则不能直接将它传入上述函数。**如下是不对的：

```scala
val s = sum(1 to 3) //错误，因为 1 to 3 是Range(1,2,3)类型
```
- **使用_*来适配一个序列**
如果sum函数被调用时传入的是单个参数，那么该参数必须是单个整数，而不是一个整数区间。解决这个问题的办法是告诉编译器希望这个参数被当作参数序列处理。追加":_*"：

```scala
print(sum(1 to 3:_*)) // 结果 6
```
- **递归函数中对于可变参数的用法**

```scala
    def recursiveSum(args:Int*):Int={
      if(args.length==0) 0
      else args.head + recursiveSum(args.tail:_*)
    }

    println(recursiveSum(1 to 100 :_*)) // 结果 5050
```

## 三、函数式编程
### 1. 函数字面量（匿名函数）
以下就是一个最简单的函数字面量，接受一个Int类型参数，返回两倍于参数的值。
```scala
(i:Int) => {i*2}
```
**将匿名函数作为参数传入到方法中，或者赋值给一个变量。**
```scala
val x = List.range(1, 10)
val evens = x.filter((i: Int) => i % 2 == 0)
println(evens.toString()) // List(2, 4, 6, 8)
//(i: Int) => i % 2 == 0 即函数字面量
```

scala可以推断出是一个Int类型变量，所以Int声明可以去掉
```scala
val evens = x.filter(i => i % 2 == 0)
```

当参数在函数中只出现一次时，scala允许使用“_”通配符替换变量名，从而简化代码
```scala
val evens = x.filter(_ % 2 == 0)
```

**例如使用匿名函数和foreach方法打印列表中的每一个元素**
```scala
x.foreach((i:Int)=>println(i))
```
简化过程如下：
1. 忽略Int声明
```scala
x.foreach(i=>println(i))
```
2. 只有一个参数，使用“_”代替
```scala
x.foreach(println(_))
```
3. 如果一个函数字面量只有一个语句，并且该语句只接受一个参数，那么参数不需要特别指定，也不需要显示声明
```scala
x.foreach(println)
```

### 2. 将函数作为变量
将函数作为变量传递。
```
val double = (i:Int) => {i*2}
double(2)//4
double(4)//8
```

声明函数字面量至少有两种方式。
**隐式推断下列函数的返回值为Boolean类型**：
```scala
val f = (i:Int) => {i % 2 == 0}
```

**显示声明函数返回值为Boolean类型**：
```scala
//定义一个接受Int类型参数，返回Boolean类型的值的函数，以下为同一函数的不同形式，由繁到简
val f:(Int) => Boolean = i => { i % 2 == 0 }
val f:Int => Boolean = i => {i % 2 == 0 }
val f:Int => Boolean = i => i % 2 == 0
val f:Int => Boolean = _ % 2 == 0
```

**接受两个参数的函数**
```scala
// implicit approach
val add = (x:Int,y:Int) => {x + y}
val add = (x:Int,y:Int) => x + y

// explicit approach
val add:(Int,Int) => Int = (x,y) => {x + y}
val add:(Int,Int) => Int = (x,y) => x + y
```
函数体的大括号不是必须的，但是函数体包含一个以上的表达式时，一定要使用大括号

- **像匿名函数一样使用方法**
```scala
def modMethod(i:Int) = i % 2 == 0
def modMethod(i:Int) = { i % 2 == 0 }
def modMethod(i:Int): Boolean =  i % 2 == 0
def modMethod(i:Int): Boolean =  {i % 2 == 0}
```

任一个方法都可以传入集合方法，这些方法期望接受一个函数作为参数，该函数接受Int类型返回Boolean类型值，如List[Int]的filter方法：
```scala
val list = List.range(1,10)
list.filter(modMethod) // List(2,4,6,8)

// 或者直接定义匿名函数
val modFunction = (i:Int) => i%2==0
list.filter(modFunction) // List(2,4,6,8)
list.filter((i:Int) => i%2==0) // List(2,4,6,8)
```

- 将已存在的函数/方法赋给函数变量
```scala
//也叫部分应用函数
val c = scala.math.cos _
c(0)
// 接受两个参数
val p = scala.math.pow(_,_)
p(2,2)
```

### 3. 定义接受简单函数作为参数的方法
解决办法：
1. 定义方法，包括期望接受的函数参数的签名
2. 定义满足这个签名的一个或者多个函数
3. 将函数做为参数传递给方法

示例：
1. 定义一个executeFunction的方法，该方法接受一个函数作为参数。参数名为callback，它是一个函数，没有输入参数，也没有返回值：
```scala
def executeFunction(callback:() => Unit){
  callback()
}
```
**callback:()**：语法定义了一个无参函数。如果函数有参数，器类型应当在括号中列出。
**=>Unit**：部分代码表明该方法没有返回值。
2. 定义一个函数匹配这个签名
```scala
val sayHello = () => println("hello")
```
3. 调用executeFunction
```scala
executeFunction(sayHello) // hello
```

基本语法：
```scala
def executeFunction(f:() => Unit){
  f()
}
// 语法格式
//parameterName:(parameterType(s)) => returnType

//parameterName就是f
//parameterType为空因为函数不接受任何参数
//returnType为Unit是因为不希望该函数有值返回

//接受String参数返回Int值得函数
executeFunction(f:String => Int)
executeFunction(f:(String) => Int)
//接受多个参数
executeFunction(f:(Int,Int) => Int)
```

**传入函数外的其他参数**
```scala
def executeAndPrint(f:(Int,Int) => Int,x:Int,y:Int){
  val result = f(x,y)
  println(result)
}
//创建两个函数，一个求和函数，一个求乘积函数，都符合executeAndPrint方法的函数参数的签名
val sum = (x:Int,y:Int) => x + y
val multiply = (x:Int,y:Int) => x * y
//直接调用executeAndPrint方法，传入不同的函数参数，而不需要关心实际运行的算法是什么
executeAndPrint(sum,2,9) // 11
executeAndPrint(multiply,2,2) // 4

// 比如还可以扩展更多函数，从Function0到Function22，只需要签名是正确的，传入方法或者函数都可以。
```

### 4. 使用闭包


### 5. 部分应用函数（偏应用函数、Partial Applied Function）
**部分应用函数：**指一个函数有N个参数, 而我们为其提供少于N个参数, 那就得到了一个部分应用函数

通过如下的方式减少重复向一个函数传入变量：给函数传入通用变量，创建新函数并且预加载那些值，然后使用新的函数，传入它需要的唯一变量。

例如：
```scala
val sum = (a:Int,b:Int,c:Int) => a + b + c
//sum只是一个普通的函数。但是在函数调用时，只传入前两个参数，不提供第三个参数：
val f = sum(1,2,_:Int)
//因为第三个参数没有值，变量f成为一个部分应用函数
f(3) // 6
```

**优点**：
通过绑定一些参数————通常是某种形式的局部变量————简历部分应用函数，留下其余的参数等待传入，可以让编程变得容易些
```scala
//给html 添加前缀和后缀
def wrap(prefix:String,html:String,suffix:String) = {
  prefix + html + suffix
}

//指定前缀和后缀
def wrapWithDiv = wrap("<div>",_:String,"</div>")
def wrapWithH1 = wrap("<h1>",_:String,"</h1>")
```

### 6. 创建返回函数的函数
从函数或者方法返回一个函数

1. 定义一个接受String参数返回String值的匿名函数
```scala
(s:String) => { prefix + " " + s}
```
2. 在另一个函数体内返回那个匿名函数
```scala
def saySomething(prefix:String) = (s:String) => {
  prefix + " " + s
}
//返回的是匿名函数就好比将匿名函数赋值给变量，f等同于匿名函数
var f = saySomething("hello")
//f: String => String = <function1>
f("liss") // hello liss
```

使用示例：
```scala
//使用模式匹配，类似工厂模式
//创建一个greeting方法，可以根据指定的语言不同，返回对应的问候方式
def greeting(language:String) = (name:String) => {
  language match {
    case "english" => "hello" + name
    case "china" => "李好" + name
  }
}
val enHello = greeting("english")
val cnHello = greeting("china")
enHello("liss")
cnHello("wt")
```

### 7. 偏函数（PartialFunction）
偏函数是这样一种函数，对于能赋予的每个可能的输入值，它不提供结果。它只为可能的数据的子集提供结果并且定义它能处理的数据。在scala中，可以查询一个偏函数，看它是否可以处理一个特定的值。使用PartialFunction创建

### 8. 柯里化（Currying）
允许别人一会在你的函数上应用一些参数，然后又应用另外的一些参数。

**1) 定义一个柯里化函数**
```scala
//首先我们定义一个函数:
def add(x:Int,y:Int)=x+y`

//那么我们应用的时候，应该是这样用：add(1,2) ，现在我们把这个函数变一下形：

def add(x:Int)(y:Int) = x + y
//那么我们应用的时候，应该是这样用：add(1)(2),最后结果都一样是3.这种方式（过程）就叫柯里化。
```

**2) 柯里化函数的实现过程**
柯里化实现起来很简单，简单的说就是将每一部分参数单独提出来。下面是演变过程。
```scala
// 实质上最先演变成这样一个方法
def add(x:Int)=(y:Int)=>x+y // add: (x: Int)Int => Int
//那么这个函数是什么意思？ 接收一个x为参数，返回一个匿名函数，该匿名函数的定义是：接收一个Int型参数y，函数体为x+y。现在我们来对这个方法进行调用。

val result = add(1) // result: Int => Int = <function1>
//返回一个result.那result的值应该是一个匿名函数：(y:Int)=>1+y
//所以为了得到结果，我们继续调用result。

val sum = result(2)
// 最后打印出来的结果就是3。所以我们就看到最一开始的调用方式

add(1)(3)
// 实际上是连着调用了2个函数，是一种简化的写法。
```

**3) 柯里化(currying)函数和部分应用函数(partial applied)的区别**
**柯里化**是将多参数函数分解成多个但参数的函数，即将 function(x, y, z){...} 分解成 function(x){ lambda(y){ lambda(z){...} } }。由于每次都是传入一个参数后获得包含其余参数的函数所以需要使用 function(x)(y)(z) 完成完整调用。柯里化函数非常像责任链模式，参数必须依照定义的顺序由前之后传入，每个参数完成部分工作。

**部分应用**是只传入部分参数从而获得包含其余参数的函数，即将 function(x, y, z){...} 变成 function(x, _, _)。本质上部分应用函数用于固定多参数函数的某几个参数，从而在调用时不用每次都传入同样的参数。部分应用函数的参数之间没有任何关系，所以可以部分应用任意位置的参数。

- 示例

```scala
// 下面代码定义一个普通方法multiply1和一个currying方法multiply2，并将其转换相应的函数类型：
def multiply1(x: Int, y:Int, z:Int) = x * y * z
val partialAppliedMultiply = multiply1 _
//partialAppliedMultiply: (Int, Int, Int) => Int = <function3>，转换成一个接受3个参数的匿名函数，在调用时需要提供所有的三个参数
partialAppliedMultiply(1,2,3) // 6

def multiply2(x: Int)(y: Int)(z: Int) = x * y * z
val curryingMultiply = multiply2 _
//curryingMultiply: Int => (Int => (Int => Int)) = <function1>，实际还是一个柯里化函数
//在调用时，curryingMultiply可以依次传入各个参数，而partialAppliedMultiply在传入部分参数时，必须显示指定剩余参数的占位符：

val curryingMultiply1 = curryingMultiply(1)
//类型：Int => (Int => Int)

val partialAppliedMultiply1 = partialAppliedMultiply(1, _:Int, _: Int)
//类型：(Int, Int) => Int
//另外，curryingMultiply1的类型仍然是currying类型，而partialAppliedMultiply1的类型仍然是普通函数类型。

//柯里化函数转换为普通函数
val curryingMultiply = multiply2(_:Int)(_:Int)(_:Int)
```

## 四、方法和函数的区别
**Scala中的方法**跟Java的方法一样，方法是组成类的一部分。方法有名字、类型签名，有时方法上还有注解，以及方法的功能实现代码（字节码）。

**Scala中的函数**是一个完整的对象。Scala中用22个特质（trait）抽象出了函数的概念。这22特质从Function1到Function22

**1) 方法不能作为单独的表达式而存在（参数为空的方法除外），而函数可以**

```scala
// 方法
def m(x:Int) = 2*x
m // 直接报错

// 函数
val f = (x:Int) => 2*x
f
```
在如上的例子中，我们首先定义了一个方法m，接着有定义了一个函数f。接着我们把函数名（函数值）当作最终表达式来用，由于f本身就是

一个对象（实现了FunctionN特质的对象），所以这种使用方式是完全正确的。但是我们把方法名当成最终表达式来使用的话，就会出错。

**2) 函数必须要有参数列表，而方法可以没有参数列表**
```scala
//方法
def m = 100 // m: Int
def m() = 100 // m: ()Int

//函数
val f = () => 100
f: () => Int = <function0>
val f =  => 100 // 报错
```
在如上的例子中，方法m接受零个参数，所以可以省略参数列表。而函数f不能省略参数列表

**3) 方法名是方法调用，而函数名只是代表函数对象本身**

**4) 在需要函数的地方，如果传递一个方法，会自动进行ETA展开（把方法转换为函数）**
在函数出现的地方我们可以提供一个方法。这是因为，如果期望出现函数的地方我们提供了一个方法的话，该方法就会自动被转换成函数。该行为被称为ETA expansion。

注意：
1. 期望出现函数的地方，我们可以使用方法。
2. 不期望出现函数的地方，方法并不会自动转换成函数
3. 在scala中操作符被解释成方法：
> - 前缀操作符：op obj 被解释称obj.op，如 -7
> - 中缀操作符：obj1 op obj2被解释称obj1.op(obj2)，如 1 + 2
> - 后缀操作符：obj op被解释称obj.op，如 7 toLong

可以在方法名后面加一个下划线强制变成函数。 注意： **方法名与下划线之间至少有一个空格，和方法的参数个数没有关系。**
```
def m(x: Int,y: Int): Int = x * y // m: (x: Int, y: Int)Int

val f = m _ // f: (Int, Int) => Int = <function2>

f(2,3) // res1: Int = 6
```

[difference-between-method-and-function-in-scala](http://stackoverflow.com/questions/2529184/difference-between-method-and-function-in-scala "difference-between-method-and-function-in-scala")
