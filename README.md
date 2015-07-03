# hua-lang 花语言

泛型编程，在C++语言的模板技术中，指的是针对不同数据结构的代码生成，因此有“代码膨胀”问题。
在JAVA等现代语言的实现中，变成了不同指针的自动转换问题，这实际上是解决问题的两个方案

C++是让代码来适应数据，所以一份数据生成一份代码，速度自然快，但体积大。
JAVA是让数据来适应代码，所以同一份代码可以处理所有的数据，通过类型擦除，用无类型指针来访问，所以速度相对较慢。
第一种方案针对数据，可以称之为数据泛型，第二种方案针对接口，称之为接口泛型。接口泛型可以不显示指定数据类型，比如GO语言就是这样做的，但是这就会有类型安全的问题了。

第三种泛型，就是普遍使用的继承。核心是可以使用父类对象的代码，也适用于子类对象。
这实际还是一种接口泛型的变体。这与前两种泛型解决的又是两个方面的问题。
前两种是解决容器问题，数据的外部操作问题。后一种是解决对象的内部操作问题。

C++的继承采用虚函数表实现“多态”。即不同对象的内部操作不相同，但调用方的步骤一致。
但，同样是“多态”，却不一定要用继承来实现。
比如JAVA中不支持多重继承，却可以实现多个接口。GO语言中不支持继承，用类似MIXIN的方式表达。

所以，第三种泛型的核心在接口。继承只是重用代码的一种方式。
比如JAVA中继承是代码资源的重用，只有继承者可用。对类的外部是封闭的，对类的内部是开放的。
所以在JAVA中的有两点缺陷，第一：实现接口要完全重写。第二：每个有不同动作的类，处理要分别实现。

MIXIN是一个更为普遍的重用抽象。在每个数据结构（并非一定是类）中混入一块来达到重用效果。对外是封闭的，对内相对于其它块也是封闭的。形成一个循环嵌套的大圈套小圈的结构，来实现充分的代码重用。


* 万能结构符[]
* Type[] 数组
* Type[Type] 泛型
* Var[] 指针
* Var[Type] 指针运算

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
