## Flutter - Notes

### 环境配置
```bash
git clone -b beta https://github.com/flutter/flutter.git

# Flutter ~/.zshrc 增加
export PATH=$HOME/flutter/bin:$PATH

# Flutter CN ~/.zshrc 配置源为国内
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn

# which
which flutter && flutter doctor
```

### Dart语言下的Flutter

* 基本类型  

    var 可以定义变量, 同时 Dart 属于`动态类型`语言，支持闭包。  

    number 分为 int，double。 Dart 中`没有 float 类型`。  

    Dart 下`只有 bool 型可`以用于 if 等判断。  

    DART中，switch 支持 String 类型。

* 变量

    Dart 不需要给变量设置 setter getter 方法， 这和 kotlin 等类似。Dart 中所有的基础类型、类等都继承 Object ，默认值是 NULL， 自带 getter 和 setter ，而`如果是 final 或者 const 的话`，那么它只有一个 `getter` 方法。  

    Dart 中 final 和 const 表示常量，比如 final name = 'GSY'; const value= 1000000; 同时 static const 组合代表了`静态常量`。其中 const 的值在编译期确定，final 的值要到编译时才确定。  

    Dart 下的数值，在作为`字符串`使用时，是需要`显式指定`的。比如：int i = 0; print("aaaa" + i); 这样并不支持，需要 print("aaaa" + i.toString()); 这样使用。所以*在使用动态类型时，需要注意不要把 number 类型当做 String 使用*。  

    Dart 中`数组等于列表`，所以 var list = []; 和 List list = new List() 可以简单看做一样。

  
