---
layout: post
title: idea debug的使用 
tag:	
  - idea
  - java
  - java基础
---





### 一 回顾

```
1.求出数组中最大值
  A.静态初始化一个数组
  B.定义一个默认的最大值(数组中的第一个元素)
  C.使用循环来依次与最大值来进行比较
2.数组中元素控制反转
  A.置换的次数 数组的长度/2
  B.索引的规则  i  ==>数组的长度-1-i
  C.使用循环来交换数组中的元素 两个变量来交换值
3.查找数组中指定区间指定数的索引值
  A.参数列表  数组   开始的索引  结束的索引   查找的元素
  B.使用循环来进行一一查找
4.排序-冒泡-从小到大进行排序
  A.外层循环是控制轮数
  B.内层循环是控制次数
  C.外层循环 i  内层循环 数组长度-1-i
5.向数组中插入元素
    A.找到插入值的位置
    B.使用循环来从前往后移动元素
    C.插入元素 根据索引来进行插入
 6.吃货订单管理系统  
```

### 二 项目

#### 2.1 签收订单

step01 需求

![image-20220216090919925](day07.assets/image-20220216090919925.png)

step02 分析

![image-20220216091207242](day07.assets/image-20220216091207242.png)

step03 代码

```java
//定义一个标记
                    boolean orderFalg =false;
                    System.out.println("3.签收订单");
                    System.out.println("请输入签收订单的编号");
                    int orderId =input.nextInt();
                    //使用循环来查找订单号是否存在
                    for (int i=0;i<names.length;i++) {
                        if (names[i] !=null && (orderId-1==i) && states[i]==0){
                            System.out.println("签收成功");
                            //将订单的状态赋值1
                            states[i]=1;
                            orderFalg =true;
                            break;
                        }
                    }
                    if (!orderFalg){
                        System.out.println("签收失败");
                    }

```

#### 2.2 删除订单

step01 需求

![image-20220216094340589](day07.assets/image-20220216094340589.png)

step02 分析

![image-20220216095107186](day07.assets/image-20220216095107186.png)

step03 代码

```java
 //定一个状态
                    boolean deleteFlag = false;
                    System.out.println("4.删除订单");
                    System.out.println("请输入删除订单的编号");
                    int deleteId  =input.nextInt();
                    //使用循环来查询
                    for (int i=0;i<names.length;i++) {
                        if (names[i] !=null && (deleteId-1==i) && states[i]==1){
                            //将元素从后往前移
                            for (int j =deleteId-1;j<names.length-1;j++){
                                names[j]=names[j+1];
                                diskMsg[j]=diskMsg[j+1];
                                times[j]=times[j+1];
                                address[j]=address[j+1];
                                money[j]=money[j+1];
                                states[j]=states[j+1];
                            }
                            //将最后一个元素设置为null
                             int index = names.length-1;
                             names[index]=null;
                             diskMsg[index]=null;
                             times[index]=null;
                             address[index]=null;
                             money[index]=0;
                             states[index]=0;
                             System.out.println("删除成功");
                             deleteFlag= true;
                             break;
                        }
                    }
                    if(!deleteFlag){
                        System.out.println("删除订单失败");
                    }
```

#### 2.3 我要点赞

step01 需求

![image-20220216103348088](day07.assets/image-20220216103348088.png)

step02 分析

```
A.需要将菜品的信息进行展示
B.将对应数组的赞数进行修改
```

step03 代码

```java 
 System.out.println("5.我要点赞");
                    System.out.println("1.序号\t2.菜名\t3.单价\t4.点赞数");
                    for (int j=0;j<disk.length;j++) {
                        System.out.println((j+1)+"\t"+disk[j]+"\t"+price[j]+"\t"+nums[j]);
                    }
                    System.out.println("请输入需要点赞的菜名序号");
                    int  addId= input.nextInt();
                    nums[addId-1]++;
```

### 三 idea介绍

#### 3.1 idea的简介 参考素材文档

#### 3.2 idea 模块化开发

![image-20220216105004224](day07.assets/image-20220216105004224.png)

#### 3.3 idea 创建模块化项目

step01 

![image-20220216105213754](day07.assets/image-20220216105213754.png)

step02

![image-20220216105236150](day07.assets/image-20220216105236150.png)

step03

![image-20220216105308168](day07.assets/image-20220216105308168.png)

#### 3.4 debug调试(重点)

##### 3.4.1 步骤

```
1.设置断点 设置在可能出现问题的代码位置处 设置在第一行
2.使用debug方式来运行项目
3.一步一步执行 查看运行的流程 以及变量的执行的结果
4.解决问题
```

##### 3.4.2 演示

代码

```java
public class Test01 {
    public static void main(String[] args) {
        //需求 使用while 打印3遍好好学习 天天向上
        //初始化语句
        int i=1;
        while (i<=3){
            System.out.println("第"+i+"遍好好学习,天天向上");
        }

    }
}
```

step01 设置断点

![image-20220216111524468](day07.assets/image-20220216111524468.png)

step02  使用debug方式来运行

![image-20220216111615794](day07.assets/image-20220216111615794.png)

step03  debug 控制台的介绍

![image-20220216112844693](day07.assets/image-20220216112844693.png)

#### 3.5 idea 快捷键

```
2、psvm 提示主方法
3、sout 提示打印语句
4、ctrl+alt+l 格式化代码
5、ctrl+d 快速向下复制当前行
6、ctrl+y 删除当前行
7、alt+enter 智能提示
8、alt+insert 快速生成代码
9、ctrl+alt+t 快速包裹一段代码
10、ctrl+alt+v 快速添加引用
11、ctrl和+或者- 收缩或者展开单个方法
12、ctrl+Alt+shift+u 查看类关系图
13、ctrl+alt+m 选中的代码转换为方法
```

### 四 面向对象

#### 4.1面向对象的概念

```
1.java是基于面向对象来进行编程 面向对象是一种开发思想 在java中万物皆对象  一切事物都可以使用对象来进行描述
2.面向对象思想与面向过程思想
  面向过程思想:注重于步骤 注重于过程 每一步都是亲力亲为(员工)
  面向对象思想:注重与解决问题的主体 注重于结果 注重对象 不注重于过程(老板) 偷懒
  面向对象与面向过程是相辅相成的  面向过程是面向对象的基础
3.例子:洗衣服
     面向过程 : 准备一个盆子 ==>放入衣服与洗衣液==>倒水==>揉揉 ==>冲洗==>拧干==>晾嗮 亲力亲为
     面向对象 : 洗衣机(对象) ==>衣服放入洗衣机 ==>晾嗮
4.面向对象好处:
   A.更符合人的思考的方式
   B.由执行者变成指挥者
   C.简化程序的设计
5.面向对象的三大特征: 封装  继承   多态
```

开发的例子: 打印数组中元素int [] nums={2,3,4,5}  ==> [2,3,4,5]

面向过程

```
public class Test03 {
    //int [] nums={2,3,4,5}  ==> [2,3,4,5]
    public static void main(String[] args) {
        String s="";
        int [] nums={2,3,4,5};
        for (int i=0;i<nums.length;i++) {
            if (i==0) {
                s+="["+nums[i]+",";
            }else if(i==nums.length-1) {
                s+=nums[i]+"]";
            }else{
                s+=nums[i]+",";
            }

        }
        System.out.println(s);
    }
}

```

面向对象

```java
import java.util.Arrays;

public class Test04 {
    public static void main(String[] args) {
        int [] nums={2,3,4,5};
        //借助于jdk提供的对象
        System.out.println(Arrays.toString(nums));
    }
}

```

#### 4.2类的概念(重点)

```
1.类:是一组属性与行为的集合
2.属性:描述事物的特征 一般是名称或者是形容词
3.行为:描述事物执行的动作 一般是动词
4.例子:
   狗类: 属性：名字 品种  颜色  年龄  行为:吃  叫  睡 ....
   人类: 属性: 名字 性别  年龄  爱好  行为 吃  睡  玩.....
```

#### 4.3对象概念(重点)

```
1.对象的概念:对象是类的具体的表现  对象肯定有类的属性与行为
2.例子：
   人类(抽象) ==> 何海鹏(具体)
   狗类(抽象) ==> 多多(具体)
3.类与对象关系:
   类是对象的抽象 对象是类的具体的表现
```

![image-20220216143436474](day07.assets/image-20220216143436474.png)

#### 4.4定义类(重点)

```
1.语法:
   访问修饰符  class  类名 {
       //属性  成员变量
       //行为  成员方法
   }
2.例子:
   public class  Dog {
      //成员变量
       String name;
       int age;
       //成员方法
       public  void eat(){};  
   }
3.成员变量与成员方法
  成员变量:只是位置发生了改变 定义在类中 方法外
  成员方法:去除关键字static
```

代码

```java
/**
 * 定义一个狗类
 */
public class Dog {
    //成员变量
    String  name;
    String  kind;
    int age;

    //成员方法
    public  void  eat(String name){
        System.out.println("吃"+name);
    }
    public  void sleep(){
        System.out.println("正在睡觉.....");
    }

}

```

#### 4.5 实例化对象(重点)

```
1.语法:类名 对象名 = new  类名();
2.注意点：
   A.前后的类名必须一致
   B.对象名可以任意的定义 通俗易懂
   C.后面的类名需要加上小括号
3.例子:实例化狗对象
      Dog  d = new  Dog();
4.成员变量的赋值与取值
   赋值语法: 对象名.成员变量名=值    例子: d.name="多多";
   取值语法: 对象名.成员变量名       例子: d.name
5.成员方法访问:
    语法: 对象名.方法名(参数列表) 例子: d.eat()
```

代码

```java
public class Test05 {
    public static void main(String[] args) {
        //实例化对象
        Dog d  = new Dog();
        //给成员变量赋值
        d.name="多多";
        d.age=2;
        d.kind="泰迪";
        //取值
        System.out.println(d.name);
        System.out.println(d.age);
        System.out.println(d.kind);
        //调用方法
        d.eat("骨头.....");
        d.sleep();


    }
}

```

#### 4.6成员变量默认值

|              | 整数类型 |       0        |
| :----------: | :------: | :------------: |
|              | 小数类型 |      0.0       |
| 基本数据类型 | 布尔类型 |     false      |
|              | 字符类型 | '\u0000'(空格) |
| 引用数据类型 |  字符串  |      null      |

#### 4.7  案例

step01 需求:定义一个手机类   实例化不用的手机   给成员变量赋值 调用方法

step02 分析

```
属性:  品牌  颜色 价格
行为： 打电话  发短信
```

step03 定义手机类

```java 
/**
 * 定义手机类
 */
public class Phone {
    //成员变量
    String brand; //品牌
    String  color;//颜色
    double price;//价格

    //成员方法
    public  void  cell(String who){
        System.out.println("我正在给"+who+"打电话谈一个小目标的事情");
    }

    public  void  send(){
        System.out.println("聪哥正在给网红小姐姐发短信");
    }



}

```

step03 实例化手机对象

```java
public class Test06 {
    public static void main(String[] args) {
        //实例化对象
        Phone huawei = new Phone();
        //给成员变量来进行赋值
        huawei.brand="华为";
        huawei.color="黑色";
        huawei.price=8888;
        //获取值
        System.out.println(huawei.brand);
        System.out.println(huawei.color);
        System.out.println(huawei.price);
        //调用方法
        huawei.send();
        huawei.cell("王建林");
        System.out.println("======================");
        Phone apple = new Phone();
        //赋值
        apple.brand="苹果13";
        apple.color="粉色";
        apple.price=8848;
        //取值
        System.out.println(apple.brand);
        System.out.println(apple.color);
        System.out.println(apple.price);
        //调用方法
        apple.cell("宿华");
        apple.send();

    }
}

```

### 五 内存图(重点)

#### 5.1 一个对象创建的内存图

![image-20220216165712115](day07.assets/image-20220216165712115.png)

#### 5.2 两个栈内存的引用指向同一个对象

![image-20220216172452577](day07.assets/image-20220216172452577.png)

#### 5.3 方法参数是引用数据类型的内存图

![image-20220216174006262](day07.assets/image-20220216174006262.png)