# hua-lang 花语言

泛型编程，在C++语言的模板技术中，指的是针对不同数据结构的代码生成，因此有“代码膨胀”问题。
在JAVA等现代语言的实现中，变成了不同指针的自动转换问题，这实际上是解决问题的两个方案

C++是让代码来适应数据，所以一份数据生成一份代码，速度自然快，但体积大。
JAVA是让数据来适应代码，所以同一份代码可以处理所有的数据，通过类型擦除，用无类型指针来访问，所以速度相对较慢。
第一种方案针对数据，可以称之为数据泛型，第二种方案针对接口，称之为接口泛型。接口泛型可以不显示指定数据类型，比如GO语言就是这样做的，但是这就会有类型安全的问题了。

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
