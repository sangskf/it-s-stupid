# Java函数式编程

## Lambda表达式的使用前提(@FunctionalInterface)
1. 必须保证有一个接口,而且其中的抽象方法有且仅有一个
2. 必须具有上下文环境

```java
public class MainApp {

    public static void main(String[] args) {

        sum((a, b) -> a - b);

        Calclator calclator = (a, b) -> a + b;
        System.out.println(calclator.sum(1,2));
    }


    public static void sum(Calclator cal) {
        System.out.println(cal.sum(1223, 12312));
    }

}
```
## 接口中可以定义具体方法和静态方法

* 接口中的静态方法（可以作为工厂使用）static
* 接口中的默认方法 default
* 接口中的私有方法 private （包括私有普通方法和私有静态方法）
* 接口中的常量


## 方法引用
