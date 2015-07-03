# hua-lang 花语言



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
