# hua-lang 花语言

花语言设计思路

## 一、代码只写一次

什么代码最令人不快，就是大段COPY，稍稍改动几个字的代码。代码只写一次就好。

### 泛型编程
泛型编程，在C++语言的模板技术中，指的是针对不同数据结构的代码生成，因此有“代码膨胀”问题。
在JAVA等现代语言的实现中，变成了不同指针的自动转换问题，这实际上是解决问题的两个方案

C++是让代码来适应数据，所以一份数据生成一份代码，速度自然快，但体积大。
JAVA是让数据来适应代码，所以同一份代码可以处理所有的数据，通过类型擦除，用无类型指针来访问，所以速度相对较慢。
第一种方案针对数据，可以称之为数据泛型，第二种方案针对接口，称之为接口泛型。接口泛型可以不显示指定数据类型，比如GO语言就是这样做的，但是这就会有类型安全的问题了。

### 继承
第三种泛型，就是普遍使用的继承。核心是可以使用父类对象的代码，也适用于子类对象。
这实际还是一种接口泛型的变体。这与前两种泛型解决的又是两个方面的问题。
前两种是解决容器，外部操作问题。后一种是解决对象的内部操作问题。

C++的继承采用虚函数表实现“多态”。即不同对象的内部操作不相同，但调用方的步骤一致。
但，同样是“多态”，却不一定要用继承来实现。
比如JAVA中不支持多重继承，却可以实现多个接口。GO语言中不支持继承，用类似MIXIN的方式表达。

所以，第三种泛型的核心在接口。继承只是重用代码的一种方式。
比如JAVA中继承是代码资源的重用，只有继承者可用。对类的外部是封闭的，对类的内部是开放的。
所以在JAVA中的有两点缺陷，第一：实现接口要完全重写。第二：每个有不同动作的类，处理要分别实现。

MIXIN是一个更为普遍的重用抽象。在每个数据结构（并非一定是类）中混入一块来达到重用效果。对外是封闭的，对内相对于其它块也是封闭的。形成一个循环嵌套的大圈套小圈的结构，来实现充分的代码重用。

## 二、自动补全

人们总喜欢省略，在意思表达完整的情况下，尽可能省略。
所以一个要求一丝不苟的语言，总会有些人不满意，比如PASCAL，比如GO。

### 垃圾回收
垃圾回收是自动补全需求中最大的也是最直接的表现。
内存被分为两块，一块是栈，一块是堆。因为栈会自动释放，所以垃圾回收是针对堆而言的。
垃圾回收算法很多，最早的是引用计数回收，比较新的拷贝回收。但是这都是你看不见的技术底层。
关键的是怎样使用一种语言，产生的垃圾会最少，或是最容易被清除。
这才是我们应该研究的课题。

我们知道，数据库中有事务的概念，一条事务中所修改的数据，可以一次提交，也可以一次回滚。
编程中我们也有类型概念，叫任务完成后清理，任务中的所有内存全部存放在指定区域，一次性清空。

分代收集算法，数据的生存周期也是有明显的区别的，一种是特别短，一种是比较长，一种是永久。针对不同的数据分别处理。

静态分析算法，编译期即可生成代码释放。

### ZD补全域

空指针是JAVA中最常见的异常，出现空指针，无非是没有初始化。
野指针是C、C++中最觉的异常，出现野指针，无非是没有清理。

为了避免各种疏忽和遗漏，语言设计者们想了很多办法，RUST中的生存时间、指针类型比较典型。

资源占用标志，所有权转移，协程（任务）事务ID，都是补全的基础。

#### 元数据

为了能够更好地自动处理数据，存放元数据是必要的手段，元数据是动态反射机制的基础。

## 三、脑筋急转弯

程序是给别人看的，更是给自己看的，明确、正确是第一要义。别让我思考。

### 异步回调
异步开发中的回调模式是有点“反人类”的。
由于是异步调用，所以几乎不能完整表达你的思路，
写异步程序时，你不是在绞尽脑汁，就是在不断调试。

协程（比如LUA的coroutine）是解决这一问题的有效方式。

### 指针与引用

指针是编程理念中，很难理解和掌握的部分。

JAVA为了消灭指针，引入了很多说法，一，值类型与引用类型。二，值传递与引用传递。三，封包与解包。

* int:var :var 值类型
* int@var @var 引用类型
* 
* 万能结构符[]
* Type[] 数组
* Type[Type] 泛型
* Var[] 指针
* Var[Type] 指针运算


## 四、韩信点兵

中国有句古话，叫韩信点兵，多多益善。这里面包含的意义是“绝对的掌控”。如何帮助程序员对他的程序有“绝对的掌控”，这是个美妙的问题。

### 结构化

结构化方法的基本要点是：自顶向下、逐步求精、模块化设计、结构化编码。

#### 蓝图

蓝图就是：自顶向下、逐步求精。这是一种方法论。但蓝图把抽象的东西具体化。

## 五、糖果

### 小甜头


### 恶趣味


## 六、其它

### 名字

### 历史


```

struct pair {
  int:num1;
  int:num2;
}

proc int:plus(pair[]:para) {
  int:ret = para.num1+para.num2;
  return ret;
}

struct object {
  int:p1;
  int:p2;
  int:p3;
}

object var;
var.p1=1;
var.p2=2;
var.p3=3;

var[p1,p2].plus();

```


```
struct sa {
  int:a;
  int:b;
}

struct sb {
  mixin sa:c;
  int:d;
}

```
