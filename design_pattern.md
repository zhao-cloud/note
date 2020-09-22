设计模式遵循的7大原则
1. 单一职责原则
  一个类只应该负责一个职责，如果A类负责两个不同的职责：职责1，职责2。当职责1需求变更而改变A时，可能造成职责2的执行错误，所以需要将A类的粒度分解为A1,A2
2.接口隔离原则
  A类通过接口interface依赖（使用）B类（B类实现了interface），但是并不是使用interface中所有的抽象方法，需要将interface拆开成使用的方法interface1 和 未使用的方法interface2 然后B类实现   interface1. 总之我们希望A类通过接口使用B类时，interface是最小的
3.依赖倒转原则
  高层模块不要依赖低层模块，二者都应该依赖其抽象。面向接口编程
  依赖关系传递的三种方法：1 接口传递  2 构造方法传递  3 setter方法传递
4.里氏替换原则
  如果对每个类型为T1的对象o1,都有类型为T2的对象o2,使得以T1定义的所有程序P在所有的对象o1都代换成o2时，程序P的行为没有发生变化，那么类型T2是类型T1的子类型。换句话说，所有引用基类的地方必须能够透明地使用其子类的对象。在继承时，遵循里氏替换原则，在子类中尽量不要重写父类的方法。
5.开闭原则（核心原则）
  一个软件实体如类，模块和函数应该对扩展开放（对提供方），对修改关闭（对使用方）。用抽象构建框架，用实现扩展细节。当软件需要变化时，尽量通过扩展软件实体的行为来实现变化，而不是通过修改已有的代码来实现变化。
6.迪米特法则
  一个对象应该对其他对象保持最少的了解。类与类关系越密切，耦合度越大。迪米特法则又叫最少知道原则，即一个类对自己依赖的类知道的越少越好。也就时说对于被依赖的类不管多么复杂，都尽量将逻辑封装在类的内部。对外除了提供的public方法，不对外泄露任何信息。迪米特法则还有个更简单的定义：只与直接的朋友通信 成员变量 方法参数 方法返回值 都是直接朋友 局部变量不是直接朋友
7.合成复用原则

核心思想
  找出应用中可能需要变化之处，把它们独立出来，不要和那些不需要变化的代码混到一起。
  针对接口变成，而不是针对实现变成
  为了交互对象之间的松耦合设计而努力

UML--unified modeling language(统一建模语言)
Dependency  依赖：使用  只要类中用到了对方就存在依赖关系
Association 关联：如果一个类使用了另一个类 那么他们就是关联关系 关联关系是一种特殊的依赖关系 关联关系具有指向性，多重性
Generalization 泛化：继承  是依赖关系中的一种特殊情况
Realization 实现
Aggregation 聚合:A类里有一个成员变量B类，B类是通过set方法进行传递的就认为A类聚合了B类 A的产生B并不一定会产生，A消亡并不会引起B的消亡，即A与B可以分离
Composite   组合：A类里有一个成员变量B类，A的产生B一定会产生，A消亡会引起B的消亡 同生共死

设计模式
  单例模式：即采取一定的方法保证整个软件系统中，对某个类只能存在一个对象实例，并且该类只提供一个取得对象实例的方法（这种方法一般都是静态方法）。
    1.饿汉式（静态常量）
      步骤：1）构造器私有化（防止使用new关键字创建对象）
           2）在类的内部创建对象
           3）向外暴露一个静态的公共方法用于返回该类的对象实例。getInstance
           public class A{
           private A(){}
           private final static A instance = new A();
           public static A getInstance(){
           return instance;
              }
           }
           缺点：类加载的时候就创建，没有懒加载，造成内存的浪费
     2.饿汉式（静态代码块）
          public class A{
           private A(){}
           static{//在静态代码块中，创建实例对象
              instance = new A();
           }
           private static A instance;
           public static A getInstance(){
           return instance;
              }
           }
           缺点：类加载的时候就创建，没有懒加载，造成内存的浪费
      3.懒汉式（线程不安全）
          public class A{
             private static A instance;
             private A(){}
             //提供一个静态公有方法，当使用到该方法时，才去创建instance
             //即懒汉式
             public static A getInstance(){
                if(instance == null){
                    instance = new A();
                }
                return instance;
                }
           }
           优点：可以实现懒加载方式
           缺点：线程不安全，实际开发中不能使用该方式，有潜在的风险
      4.懒汉式（线程安全，同步方法）
          public class A{
            private static A instance;
            private A(){}
            //加入同步代码，解决线程不安全问题
             public static sychronized A getInstance(){
                if(instance == null){
                    instance = new A();
                }
                return instance;
                }
           }
           优点：可以实现懒加载方式,线程安全
           缺点：每次获取实例都要进行线程同步，效率地下。实际开发中不推荐使用该方法
       5.双重检查（推荐使用）
          public class A{
            //volatitle可以使修改立即生效到内存，是一个轻量级的同步锁
            private static volatitle A instance;
            private A(){}
            //双重检查
             public static A getInstance(){
                if(instance == null){
                    syncronized(A.class){
                        if(instance == null){
                            instance = new A();
                         }
                    }
                }
                return instance;
                }
           }
        6.静态内部类（当类加载的时候，静态内部类不会被加载，当使用静态内部类时会进行线程安全的加载,推荐使用）
          public class A{
            private static A instance;
            private A(){}
            private static class AInstance{
              private static final A INSTANCE = new A();
            }
             public static A getInstance(){
                return AInstance.INSTANCE;
             }
           }
        7.枚举（推荐使用）
          enum A{
              INSTANCE;
              public void sayOk(){
                system.out.println("ok...");
              }
          }
          优点：借助JDK1.5添加的枚举来实现单例。不仅能避免线程同步问题，而且还能防止反序列化重新创建对象。
          
  简单工厂模式(静态工厂模式)
  工厂方法模式
      定义了一个创建对象的抽象方法，由子类决定要实例化的的类。工厂方法模式将对象实例化推迟到子类。即将项目的实例化工程抽象成抽象方法，在不同的子类中具体实现。
  抽象工厂模式
      定义了一个interface用于创建相关或有依赖关系的对象簇，而无需指明具体的类。
  原型模式
      用原型实例指定创建种类，并且通过拷贝这些原型，创建新的对象。原型模式是一种创建型设计模式，允许一个对象再创建另一个可定制的对象，无需知道创建细节
      工作原理：通过一个原型对象传给那个要发动创建的对象，这个要发动创建的对象通过请求原型对象拷贝他们自己来实现创建，即 对象.clone()
      深拷贝：1.重写clone方法实现深拷贝  2.通过对象序列化实现深拷贝（推荐）
      序列化实现demo：
        public class A implement CloneAble,serializable{
          private int a;
          private String b;
          private C c;
          
          //浅拷贝
          @override
          public Object clone{
            A aa= null;
            try{
              aa = (A)super.clone();
            }catch(Exception e){
              e.printStackTrace();
            }
          }
          
          //深拷贝
          public A deepCopy(){
            ByteArrayOutputStream bos = null;
            ObjectOutputStream oos = null;
            ByteArrayInputStream bis = null;
            ObjectInputStream ois = null;
            try {
              //序列化
              bos = new ByteArrayOutputStream();
              oos = new ObjectOutputStream(bos);
              //将当前对象以对象流的方式输出
              oos.writeObject(this);
              //反序列化
              bis = new ByteArrayInputStream(bos.toByteArray());
              ois = new ObjectInputStream(bis);
              B copyObj = (B)ois.readObject();
              return copyObj;
            } catch(Exception e){
              e.printStackTrace();
              return null;
            }finally{
              try{
              bos.close();
              oos.close();
              bis.close();
              ois.close();
              }catch(Exception e2){
              e2.printStackTrace();
              }
            }
          }
        }
  建造者模式（build pattern）
      又名生成器模式,是一种对象构建模式。它可以将复杂对象的建造过程抽象出来（抽象类别），使这个抽象过程的不同实现方法可以构造出不同表现（属性）的对象。建造者模式是一步一步创建一个复杂的对象，他允许用户只通过指定复杂对象的类型和内容就可以构建他们，用户不需要知道内部的具体构建细节。
      建造者模式中的四个角色：
          1.product(产品角色):一个具体的产品对象。
          2.builder(抽象建造者):创建一个product对象的各个部件指定的接口/抽象类。
          3.concreteBuilder(具体建造者):实现接口，构建和装配各个部件。
          4.director(指挥者):构建一个使用builder接口的对象。他主要是用于创建一个复杂的对象。它主要有两个作用，一是：隔离了客户与对象的生产过程，二是：负责控制产品对象的生产过程。适用范围是产品之间差异性不大
  适配器模式(Adapter Pattern)
      将某个类的接口转换成客户期望的另一个接口表示，主要目的是兼容性，让原本接口不匹配不能一起工作的两个类可以协同工作。其别名为包装器(Wrapper),适配器模式属于结构模型模式，主要分为三类：类适配器模式、对象适配器模式、接口适配器模式
  桥接模式（bridge）-结构型设计模式
      将实现与接口抽象放在两个不同的类层次中，使两个层次可独立给改变。桥接模式是基于类的最小实际原则，通过使用封装、聚合及继承等行为让不同的类承担不同的职责。它的主要特点是把抽象与行为实现分离开来，从而可以保持各个部分的独立性以及应对他们的功能扩展













