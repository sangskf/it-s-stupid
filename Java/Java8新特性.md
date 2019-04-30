#JAVA8新特性

* **default拓展方法**

  default关键字为接口声明添加非抽象方法的实现,也称为拓展方法

  ```java
  public interface Formula {
      void doSomething();
  
      default void before() {
          System.out.println("我是拓展方法");
      }
  }
  
  public class FormulaService implements Formula {
  
      @Override
      public void doSomething() {
          before();
          System.out.println("我是override方法");
      }
  }
  ```

* **Lambda表达式**

  java8引入函数式编程，Lambda则是函数式编程的基础。

  ```java
  List<String> list = Arrays.asList("a", "b", "c");
  
  // java8之前
  Collections.sort(list, new Comparator<String>(){
      @Override
      public int compare(String o1, String o2) {
          return o1.compareTo(o2);
      }
  });
  
  // lambda
  Collections.sort(list, (String o1, String o2) -> {
              return o2.compareTo(o1);
  });
  
  // 简洁lambda
  Collections.sort(list, (String o1, String o2) ->  o2.compareTo(o1));
  
  // 更简洁lambda
  Collections.sort(list, (o1, o2) ->  o2.compareTo(o1));
  
  // 也可以这样写
  Collections.sort(list, Comparator.comparing(String::toString));
  ```

* **函数式接口**

  一个函数式接口有且只能有一个抽象方法申明，其中该注意的是 @FunctionalInterface 注解，此时如果在接口中定义了第二个抽象方法，编译器将会抛出异常。当然如果不加该注解也不是不行，如果接口中有多个抽象方法，而你又使用了lambda表达式，则在调用处会抛出异常。

  ```java
  @FunctionalInterface
  public interface Formula<F,T>{
      T convert(F var1);
  }
  
  Formula<String,Integer> function = (var1 -> Integer.valueOf(var1));
  Integer var2 = function.convert("1000"); 
  ```

* **方法和构造函数引用**

  方法引用的标准语法是 `类名:方法名`

  | 类型                             | 示例                              |
  | -------------------------------- | --------------------------------- |
  | 引用静态方法                     | targetClass :: staticMethodName   |
  | 引用某个对象的实例方法           | targetClass :: instanceMethodName |
  | 引用某个类型的任意对象的实例方法 | targetType :: methodName          |
  | 引用构造方法                     | className :: new                  |

 1. **引用静态方法**

    ```java
    Formula<String,Integer> function = (Integer::valueOf);
    Integer var2 = function.convert("1000");
    ```

 2. **引用某个类型的任意对象的实例方法**

    ```java
    public static void main(String[] args) {
       String[] array = {"贱明", "学友"};
       Arrays.sort(array, String::compareTo);
    }
    ```

 3. **引用构造方法**

    ```java
    // 定义工厂
    interface PersonFactory<P extends Person>{
        P create(String name);
    }
    // Person类的构造方法
    public Person(String name) {
        this.name = name;
    }
    // 创建
    PersonFactory<Person> factory = Person::new;
    factory.create("贱明");
    ```

* **Lambda的范围**

  lambda可以访问局部对应的外部区域的局部final变量，以及成员变量和静态变量。

  1. **访问成员变量**

     ```java
     public void doSomething(){
         final String p1 = "贱明";
         final String p2 = "学友";
         Formula function = (person1, person2) -> p1.compareTo(p2);
     }
     ```

     与java8以下版本不同的是，p1 p2你可以不修饰成final 也不会报错，但是如果你想修改他们，编译器则会告诉你这是不被允许的。

  2. **访问成员变量和静态变量**

     ```java
     public void doSomething(){;
        Formula function = (person1, person2) ->{
            dehua = "贱明";
            xueyou="学友";
            return dehua.compareTo(xueyou);
        };
     }
     ```

* **内置函数式接口**

  ​		java8 api中提供了很多内置函数式接口，而且有些接口其实在Google Guava中已经实现了，很大程	度的降低了程序员的工作负担。

  1. **Predicates**
     Predicate是一个布尔类型的函数，该函数只有一个输入参数，他包含了多种默认实现。

     ```java
     public static void main(String[] args) {
        Predicate<String> predicate = (s) -> s.contains("贱明");
        String var1 = "牛贱明";
     
        predicate.test(var1);              // true
        predicate.negate().test(var1);     // false
     
        Predicate<Boolean> nonNull = Objects::nonNull;
        Predicate<Boolean> isNull = Objects::isNull;
     
        Predicate<String> isEmpty = String::isEmpty;
        Predicate<String> isNotEmpty = isEmpty.negate();
     
        isNotEmpty.and(isEmpty).test(var1);
     }
     ```

  2. **Functions**

     Function接口接收一个参数，并返回单一的结果。默认方法可以将多个函数串在一起

     ```java
     public static void main(String[] args) {
        Function<String, Integer> toInteger = Integer::valueOf;
        Function<String, String> backToString = toInteger.andThen(String::valueOf);
     
        System.out.println(toInteger.apply("123"));
        System.out.println(backToString.apply("123"));
     }
     ```

  3. **Suppliers**

     Supplier接口产生一个给定类型的结果。与Function不同的是，Supplier没有输入参数。

     ```java
     Supplier<Person> personSupplier = Person::new;
     Person p = personSupplier.get();   // new Person
     ```

  4. **Consumers**

     Consumer代表了在一个输入参数上需要进行的操作。

     ```java
     Consumer<Person> greeter = (p) -> System.out.println("Hello, " + p.getName());
     greeter.accept(new Person("贱明"));
     ```

  5. **Comparators**

     Comparator接口在早期的Java版本中非常著名。Java 8 为这个接口添加了不同的默认方法。

     ```java
     Comparator<Person> comparator = (p1, p2) -> p1.getName().compareTo(p2.getName());
     
     Person p1 = new Person("贱明");
     Person p2 = new Person("学友");
     
     System.out.println(comparator.compare(p1, p2));             // > 1105
     System.out.println(comparator.reversed().compare(p1, p2));  // < -1105
     ```

  6. **Optionals**

     Optional不是一个函数式接口，而是一个精巧的工具接口，用来防止NullPointerException产生。
     Optional是一个简单的值容器，这个值可以是null，也可以是non-null。考虑到一个方法可能会返回一个non-null的值，也可能返回一个空值。为了不直接返回null，我们在Java 8中就返回一个Optional。

     ```java
     Optional<String> optional = Optional.of("贱明");
     
     System.out.println(optional.isPresent()); // true      
     System.out.println(optional.get()); // 贱明   
     System.out.println(optional.orElse("学友")); // 贱明    
         
     optional.ifPresent((s) -> System.out.println(s.charAt(0))); 
     ```

     

  