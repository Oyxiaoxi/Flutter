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

    var 可以定义变量, 同时 Dart 属于动态类型语言，支持闭包。  
    number 分为 int，double。 Dart 中没有 float 类型。  
    Dart 下只有 bool 型可以用于 if 等判断。  
    DART中，switch 支持 String 类型。
