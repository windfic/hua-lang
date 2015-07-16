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


### 动态特性

随着动态语言的大行其道，动态修补程序成了一个很愉快和需求量很大的需求。

在我看来，主要有三种使用动态语言的需求：
* 热补丁，犯错之后修改
* 领域过于复杂，需要临时分析
* 因为我想偷懒

静态语言嵌入虚拟机的方式实现动态特性是个不错的方案。

为了能够更好地自动处理数据，存放元数据是必要的手段，元数据是动态反射机制的基础。

### 安全模型

安全模型，在大多数的编程语言中都是没有涉及的领域。但安全问题却无处不在。

所以，为语言添加安全模型，是一项必须的需求。

安全模型涉及面很大，总的来说有三条。一、保护自身不被窥视和控制。二、确保外部数据的真实性。三、防止操作者犯错。

安全模型：用一句话描述就是，限制的区域外即为脏。那么脏的东西要洗，干净的东西要打包。

首先是自我限制，即行为目的描述
然后是越限判断，即启动机制描述
最后是前后处理，即保护动作描述


## 三、别让我思考

程序是给别人看的，更是给自己看的，明确、正确是第一要义。别让我思考。

### 异步回调
异步开发中的回调模式是有点“反人类”的。
由于是异步调用，所以几乎不能完整表达你的思路，
写异步程序时，你不是在绞尽脑汁，就是在不断调试。

协程（比如LUA的coroutine）是解决这一问题的有效方式。

### 并发编程模型

并发，是现代编程语言最重要的特性。有效利用多核和支持分布式编程等。

简化而有效的并发编程模型是核心。

模型描述：
有一个套间，有一间客厅，有指定间卧室。每间卧室有一名住客。客厅共用，卧室私有。

可以向住客发布任务。处理每个任务时，住客不会被中断，直到主动停止，开始或继续下一个任务。

切换任务时，只需记录一个书签。停止任务后，将任务放在任务区。
客户物品被占用时，记录一个住客的书签。并将物品放在任务区。

当套间指定为进程模型时，任务区在卧室，客厅只有信件。住客只能处理自己的任务。
当套间指定为线程模型时，任务区在客厅，卧室只有书签。住客可以共享所有任务。

由于任务由住客自由分配，所以任务发布者只需面对套间。无需管理住客，但可以限制住客数量。

### 分布式编程模型

虽然分布式编程并非语言层面的东西，但是分布式和并发是解决大规模处理问题的两个必要组成部分：分布式解决任务分发问题，并行解决任务协作问题。所以建立一个分布式模型还是很有加分效果的。

MapReduce做为现有主流模型，在大规模使用，但它的缺点也显而易见。

对编程而言，MapReduce的缺点是不够自由。比如你接手一份工作，需要众人合作完成，那么一人分一块，最后合并的方式，只能说是最普通的一种。一种比较理想的方案是能够有统筹的过程。

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

### 自动测试

自动测试是一个悖论，在测试代码的正确性无法保证的前提下，用代码测试代码本来就是一个无解的过程。但如果把需求和设计的内容转化成为一种代码，其正确性由设计者本身承担，我们只需要验证设计与编码是否一致就可以了。

但这衍生出一个伦理问题：由系统分析员做功能测试，由架构设计师做集成测试，由设计师做单元测试，OMG，高贵的设计师怎么能干这个！


## 五、糖果

很多人都有自己的小癖好，而且认为理所当然。然而，这些并没有什么卵用。就像哄孩子的糖果一样，就是一点小甜头而已。

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
