---
post_url: java-foundation
title: Lambda表达式
date: 2020-02-12 10:03:42
categories:
- java基础
tags:
- lambda
---

## Lambda表达式简介
Lambda表达式是jdk8的一个新特性，就是一个匿名函数。使用Lambda表达式可以对一个接口进行简洁的实现，写出更优雅的 Java 代码，尤其在集合的遍历和其他集合操作中，可以极大地优化代码结构。
接下来我们看一个例子，简单了解一下Lambda。

```java
public class LambdaDemo {

    public static void main(String[] args) {

        //1.接口实现
        Compare compare1 = new Compare1();

        //2.匿名内部类实现
        Compare compare2 = new Compare() {
            @Override
            public int compare(int a, int b) {
                return a - b;
            }
        };

        //3.lambda实现
        Compare compare3 = (a , b) -> a - b;

    }

}

class Compare1 implements Compare{

    @Override
    public int compare(int a, int b) {
        return a - b ;
    }
}

interface Compare{
    int compare(int a , int b);
}
```

## Lambda表达式对接口的要求
使用Lambda表达式可以对接口进行简单的实现，但是并不是所有的接口都能用Lambda表达式来实现，Lambda要求要实现的接口只能有一个需要被实现的方法。

## @FunctionalInterface
@FunctionalInterface注解用来修饰函数式接口，要求接口只有一个需要实现的方法，比如Runnable接口用这个注解标识。如果有多个方法的话就会报错。
![17yYuj.md.png](https://s2.ax1x.com/2020/02/12/17yYuj.md.png)

编译错误
![176Crj.md.png](https://s2.ax1x.com/2020/02/12/176Crj.md.png)


## Lambda表达式语法
我们先定义六个接口，后面的操作都在这六个接口里面进行。

```java
/**
 * @Title: NoneReturnNoParam
 * @Description: 无返回值无参数
 * @Author <a href="mailto:chenxb1993@126.com">陈晓博</a>
 * @Date 2020-02-12 11:25
 * @Version V1.0
 */ 
@FunctionalInterface
public interface NoneReturnNoParam {

    int test();

}
```
```java
/**
 * @Title: NoneReturnOneParam
 * @Description: 无返回值一个参数
 * @Author <a href="mailto:chenxb1993@126.com">陈晓博</a>
 * @Date 2020-02-12 11:25
 * @Version V1.0
 */
@FunctionalInterface
public interface NoneReturnOneParam {

    int test(int a);

}
```
```java
/**
 * @Title: NoneReturnMutiParam
 * @Description: 无返回值多个参数
 * @Author <a href="mailto:chenxb1993@126.com">陈晓博</a>
 * @Date 2020-02-12 11:25
 * @Version V1.0
 */
@FunctionalInterface
public interface NoneReturnMutiParam {

    int test(int a , int b);

}
```

```java
/**
 * @Title: OneReturnNoParam
 * @Description: 一个返回值无参数
 * @Author <a href="mailto:chenxb1993@126.com">陈晓博</a>
 * @Date 2020-02-12 11:25
 * @Version V1.0
 */
@FunctionalInterface
public interface OneReturnNoParam {

    int test();

}
```
```java
/**
 * @Title: OneReturnOneParam
 * @Description: 一个返回值一个参数
 * @Author <a href="mailto:chenxb1993@126.com">陈晓博</a>
 * @Date 2020-02-12 11:25
 * @Version V1.0
 */
@FunctionalInterface
public interface OneReturnOneParam {

    int test(int a);

}
```
```java
/**
 * @Title: OneReturnMutiParam
 * @Description: 一个返回值多个参数
 * @Author <a href="mailto:chenxb1993@126.com">陈晓博</a>
 * @Date 2020-02-12 11:25
 * @Version V1.0
 */
@FunctionalInterface
public interface OneReturnMutiParam {

    int test(int a , int b);

}
```

Lambda表达式语法形式为 () -> {}, ()描述参数列表 ， {}描述方法体 ， -> 是Lambda运算符，读作 goes to。

```java
import com.chenxiaobo.lambda.interfaces.*;

/**
 * @Title: LambdaTest
 * @Description: LambdaTest
 * @Author <a href="mailto:chenxb1993@126.com">陈晓博</a>
 * @Date 2020-02-12 11:42
 * @Version V1.0
 */
public class LambdaTest {

    public static void main(String [] args){

        //无参数无返回值
        NoneReturnNoParam noneReturnNoParam = () -> {
            System.out.println("无参数无返回值");
        };
        noneReturnNoParam.test();

        //一个参数无返回值
        NoneReturnOneParam noneReturnOneParam = (int a) -> {
            System.out.println("一个参数无返回值：" + a );
        };
        noneReturnOneParam.test(99);

        //多个参数无返回值
        NoneReturnMutiParam noneReturnMutiParam = (int a , int b) -> {
            System.out.println("一个参数无返回值：" + a + "--" + b );
        };
        noneReturnMutiParam.test(8,9);

        //无参数一个返回值
        OneReturnNoParam oneReturnNoParam = () -> {
            System.out.println("无参数一个返回值");
            return 88;
        };
        System.out.println(oneReturnNoParam.test());

        //一个参数一个返回值
        OneReturnOneParam oneReturnOneParam = (int a) -> {
            System.out.println("一个参数一个返回值：" + a);
            return a;
        };
        System.out.println(oneReturnOneParam.test(7));

        //多个参数一个返回值
        OneReturnMutiParam oneReturnMutiParam = (int a , int b) -> {
            System.out.println("多个参数一个返回值：" + a + "--" + b );
            return a + b;
        };
        System.out.println(oneReturnMutiParam.test(2, 6));


    }

}

```

## Lambda表达式语法简化

1.省略参数类型，必须所有的参数类型同时省略
2.如果有一个参数的话可以省略括号
3.如果方法体只有一行语，可以省略大括号
4.如果方法体只有一行语，并且是返回语句，则可以省略大括号和return

```java
import com.chenxiaobo.lambda.interfaces.*;

/**
 * @Title: LambdaTest1
 * @Description: LambdaTest1
 * @Author <a href="mailto:chenxb1993@126.com">陈晓博</a>
 * @Date 2020-02-12 11:42
 * @Version V1.0
 */
public class LambdaTest1 {

    public static void main(String [] args){

        //多个参数无返回值
        //省略参数类型，必须所有的参数类型同时省略
        NoneReturnMutiParam noneReturnMutiParam = ( a ,  b) -> {
            System.out.println("一个参数无返回值：" + a + "--" + b );
        };
        noneReturnMutiParam.test(8,9);

        //一个参数无返回值
        //如果有一个参数的话可以省略括号
        NoneReturnOneParam noneReturnOneParam = a -> {
            System.out.println("一个参数无返回值：" + a );
        };
        noneReturnOneParam.test(99);

        //无参数无返回值
        //如果方法体只有一行语，可以省略大括号
        NoneReturnNoParam noneReturnNoParam = () -> System.out.println("无参数无返回值");
        noneReturnNoParam.test();

        //多个参数一个返回值
        //如果方法体只有一行语，并且是返回语句，则可以省略大括号和return
        OneReturnMutiParam oneReturnMutiParam = (int a , int b) ->  a + b ;
        System.out.println(oneReturnMutiParam.test(2, 6));
        
    }

}
```

## Lambda表达式方法引用
我们可以直接引用已经实现了的方法，不是必须每次都重写匿名内部类。
语法：方法的隶属者:方法名。
静态方法发隶属者为当前类，普通方法的隶属者为类的对象。
方法的参数类型和数量需要与接口定义的一致，返回值类型要与接口定义的一致。
```java
/**
 * @Title: LambdaTest2
 * @Description: LambdaTest2
 * @Author <a href="mailto:chenxb1993@126.com">陈晓博</a>
 * @Date 2020-02-12 13:22
 * @Version V1.0
 */
public class LambdaTest2 {

    public static void main(String [] args){
        
        //静态方法发隶属者为当前类
        OneReturnMutiParam oneReturnMutiParam = LambdaTest2::add;
        System.out.println(oneReturnMutiParam.test(5, 6));

        //普通方法的隶属者为类的对象
        LambdaTest2 lambdaTest2 = new LambdaTest2();
        OneReturnMutiParam oneReturnMutiParam1 = lambdaTest2::sub;
        System.out.println(oneReturnMutiParam1.test(9, 6));

    }

    private static int add(int a , int b){
        return a + b ;
    }

    private int sub(int a , int b){
        return a - b ;
    }
}
```

## Lambda表达式构造方法引用
通过 类名::new 的方式来实例化对象，然后调用方法返回对象。
```java
/**
 * @Title: LambdaTest3
 * @Description: LambdaTest3
 * @Author <a href="mailto:chenxb1993@126.com">陈晓博</a>
 * @Date 2020-02-12 下午2:31
 * @Version V1.0
 */
public class LambdaTest3 {

    public static void main(String [] args){

        //重写匿名内部类
        DogCreator dogCreator = () -> new Dog();
        dogCreator.getDog();

        //无参构造方法引用
        DogCreator dogCreator1 = Dog::new;
        dogCreator1.getDog();

        //有参构造方法引用
        DogCreator2 dogCreator2 = Dog::new;
        dogCreator2.getDog(1,"小狗" , 8);
    }

}

@FunctionalInterface
interface DogCreator{

    Dog getDog();

}

@FunctionalInterface
interface DogCreator2{

    Dog getDog(Integer id , String name , Integer age);

}
```

## 集合排序
```java
/**
 * @Title: LambdaTest4
 * @Description: LambdaTest4
 * @Author <a href="mailto:chenxb1993@126.com">陈晓博</a>
 * @Date 2020-02-12 下午2:42
 * @Version V1.0
 */
public class LambdaTest4 {

    public static void main(String [] args){
        List<Dog> list = new ArrayList<>();
        list.add(new Dog(1,"张三",8));
        list.add(new Dog(2,"李四",5));
        list.add(new Dog(3,"王五",10));
        list.add(new Dog(4,"冯六",2));
        list.sort((o1, o2) -> o1.getAge() - o2.getAge());
        System.out.println(list);
    }

}
```
![1HV1xA.md.png](https://s2.ax1x.com/2020/02/12/1HV1xA.md.png)

## 集合遍历
我们可以用foreach方法遍历元素，Consumer 接口是 jdk 为我们提供的一个函数式接口。
![1HljW6.md.png](https://s2.ax1x.com/2020/02/12/1HljW6.md.png)
```java
/**
 * @Title: LambdaTest5
 * @Description: LambdaTest5
 * @Author <a href="mailto:chenxb1993@126.com">陈晓博</a>
 * @Date 2020-02-12 下午3:36
 * @Version V1.0
 */
public class LambdaTest5 {

    public static void main(String [] args){
        List<Integer> list = new ArrayList<>();
        Collections.addAll(list,3,4,5,2,4,6,7,8,11,1);
        
        //遍历全部
        list.forEach(System.out::println);

        System.out.println("========");

        //遍历偶数
        list.forEach(ele -> {
            if (ele % 2 == 0){
                System.out.println(ele);
            }
        });

    }

}
```

## Lambda闭包问题
这个问题我们在匿名内部类中也会存在，如果我们把注释放开会报错，提示我们 num 是 final 不能被改变。我们虽然没有标识 num 为 final，但是在编译期间虚拟机会帮我们加上 final。
```java
/**
 * @Title: LambdaTest6
 * @Description: 闭包
 * @Author <a href="mailto:chenxb1993@126.com">陈晓博</a>
 * @Date 2020-02-12 下午3:45
 * @Version V1.0
 */
public class LambdaTest6 {

    public static void main(String [] args){

        int num = 10;

        Supplier<Integer> supplier = () -> {
//            num = num + 1;
            return num;
        };

        System.out.println(supplier.get());
    }

}
```