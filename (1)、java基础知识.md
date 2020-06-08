## （一）Java基础知识
#### 1.==和equals和hashCode的区别
> 关系操作符== 比较值本身，用于基础数据类型时比较的是值是否相等，作用于对象引用时比较的是对象引用的地址;  
equals用于对象引用，不可用于基础数据类型，比较两个对象引用是否指向同一个对象；    
hashCode用于鉴定两个对象是否相等，Object类中的hashCode方法返回对象在内存中地址转换成的一个int值；  
重写equals()方法必须重写hashCode()方法，否则该类可能无法与基于散列的集合一起正常工作。

#### 2.java的基础数据类型各占多少字节数
数据类型 | 位数 | 字节 | 最小值 | 最大值
:---|:---|:---:|:---:|:---:
byte|8|1|-128|127
char|16|2|unicode 0|unicode 2<sup>16</sup> - 1
short|16|2|-2<sup>15</sup>|2<sup>15</sup> - 1
int|32|4|-2<sup>31</sup>|2<sup>31</sup> - 1
float|32|4|-32位IEEEE 754单精度范围|32位IEEEE 754单精度范围
double|64|8|-64位IEEEE 754单精度范围|64位IEEEE 754单精度范围
long|64|8|-2<sup>63<sup>|2<sup>63<sup> - 1
boolean|1|1/8|/|/

#### 3.int与integer的区别
> int是基本数据类型，默认值为0；  
Integer是基于int的封装类，默认值为null，使用时需要实例化；  

```java
 public static void main(String[] agrs) {
        Integer a = -128;
        Integer b = -128;
        Integer c = 128;
        Integer d = 128;

        // Integer类中有缓存[-128,127]的值
        // 在区间内的赋值都会返回同个对象
        // 在区间外的赋值都会采用new的方式创建一个新对象返回
        System.out.println("a == b ? " + (a == b)); // true
        System.out.println("c == d ? " + (c == d)); // false
    }
```

#### 4.谈谈对java多态的理解
> 多态是java的三大特性之一（封装、继承、多态），指的是同个对象在不同使用环境产生的不同状态。  
多态产生的前提条件：继承关系、方法重写、父类指向子类的对象引用。  
总结：  
1-父类的成员变量、静态成员变量、静态方法不会被子类覆盖；  
2-子类只有重写父类的非静态方法，才可能被调用，重写方法里用到的成员变量均使用子类的

```java
// 父类
public class Father {

    public int number = 1;
    public static int sNumber = 11;

    public Father() {
        System.out.println("Father(): Father");
    }

    public void method() {
        System.out.println("Father:method(): number=" + number);
        System.out.println("Father:method(): sNumber=" + sNumber);
    }

    public static void staticMethod() {
        System.out.println("Father:staticMethod(): Father");
    }
}

// 子类
public class Child extends Father {

    public int number = 2;
    public static int sNumber = 22;
    public int c = 100;

    public Child() {
        System.out.println("Child(): Child");
    }

    @Override
    public void method() {
        System.out.println("Child:method(): number=" + number);
        System.out.println("Father:method(): sNumber=" + sNumber);
    }

    public static void staticMethod() {
        System.out.println("Child:staticMethod(): Child");
    }
}

public static void main(String[] agrs) {
    Father father = new Father();
    father.method();
    father.staticMethod();

    Child child = new Child();
    child.method();
    child.staticMethod();
    System.out.println("child.number = " + child.number);
    System.out.println("child.sNumber = " + child.sNumber);

    Father father1 = new Child();
    father1.method();
    father1.staticMethod();
    System.out.println("father1.number = " + father1.number);
    System.out.println("father1.sNumber = " + father1.sNumber);
    }

// 输出
// Father(): Father
// Father:method(): number=1
// Father:method(): sNumber=11
// Father:staticMethod(): Father
// Father(): Father
// Child(): Child
// Child:method(): number=2
// Father:method(): sNumber=22
// Child:staticMethod(): Child
// child.number = 2
// child.sNumber = 22
// Father(): Father
// Child(): Child
// Child:method(): number=2
// Father:method(): sNumber=22
// Father:staticMethod(): Father
// father1.number = 1
// father1.sNumber = 11
```

#### 5.String、StringBuffer、StringBuilder区别
> String类，不可继承，创建后内容不可变（为了操作安全），有常量池（只针对常量+运算）  
```java
    String some = "some";
    String thing = "thing";
    String something = "some" + "thing";
    String one = some + thing;
    String two = "something";
    String three = some + "thing";
    
    System.out.println("one == two ? " + (one == two));  // false
    System.out.println("one == something ? " + (one == something)); // false
    System.out.println("two == something ? " + (two == something)); // true
    System.out.println("three == something ? " + (three == something)); // false
    System.out.println("one == three ? " + (one == three)); // false
    System.out.println("two == three ? " + (two == three)); // false
```

StringBuilder类，实现了Appendable接口，可拼接内容，线程不安全  
StringBuffer类，实现了Appendable接口，也可拼接内容，方法加了同步锁，线程安全  


#### 6.什么是内部类？内部类的作用
> 成员内部类： 定义在类的内部，可以无条件访问类中的所有成员属性及成员方法（包括private成员和静态成员），当成员内部类拥有和外部类同名的成员变量或者方法时，会发生隐藏现象，即默认情况下访问的是成员内部类的成员。  

> 局部内部类：定义在一个方法体内或作用域内，和成员内部类的区别在于其作用范围仅在方法体内或作用域内。  

> 匿名内部类：定义在方法体内的没有名字的类。一般匿名内部类主要用于实现部分只是使用一次且功能相对简单的任务。

> 静态内部类：指被声明为static的内部类，他可以不依赖内部类而实例，而通常的内部类需要实例化外部类，从而实例化。静态内部类不可以有与外部类有相同的类名。不能访问外部类的普通成员变量，但是可以访问静态成员变量和静态方法（包括私有类型）。
一个静态内部类去掉static 就是成员内部类，他可以自由的引用外部类的属性和方法，无论是静态还是非静态，但是不可以有静态属性和方法。

#### 7.抽象类和接口区别
参数  |抽象类|接口
:----:|------|---
默认的方法实现|它可以有默认的方法实现|接口完全是抽象的。它根本不存在方法的实现。
实现|子类使用extends关键字来继承抽象类。如果子类不是抽象类的话，它需要提供抽象类中所有声明的方法的实现。|子类使用关键字implements来实现接口。它需要提供接口中所有声明的方法的实现。
构造器|抽象类可以有构造|接口不能有构造器。
与正常Java类的区别|除了你不能实例化抽象类之外，它和普通Java类没有任何区别|接口是完全不同的类型。
访问修饰符|抽象方法可以有public、protected和default这些修饰符|接口方法默认修饰符是public。你不可以使用其它修饰符。
main方法|抽象方法可以有main方法并且我们可以运行它|接口没有main方法，因此我们不能运行它。
多继承|抽象方法可以继承一个类和实现多个接口|接口只可以继承一个或多个其它接口。
速度|它比接口速度要快|接口是稍微有点慢的，因为它需要时间去寻找在类中实现的方法。
添加新方法|如果你往抽象类中添加新的方法，你可以给它提供默认的实现。因此你不需要改变你现在的代码。|如果你往接口中添加方法，那么你必须改变实现该接口的类。

#### 8.抽象类的意义
- 抽象类淋漓尽致地呈现了Java语言“封装、继承、多态”的三大特性；
- 抽象类的本质就是模板设计，为子类提供一个公共的模板，提高代码复用和维护效率；
- 抽象类是指被abstract修饰的类，其不能直接被实例化，继承抽象类的非抽象子类必须实现父类中定义的全部抽象方法；
- 抽象类不能实例化，意味着继承它的非抽象子类必须实现方法，若子类中都有相同的特征，就可利用此特性将子类中公共的部分抽象出来，提高代码复用和维护效率

- 为子类定义一个公共的类型，如父类为动物，子类为猫、狗、羊；
- 为子类定义公共的成员变量及方法，如动物所属物种，动物的叫声；

#### 9.抽象类与接口的应用场景

> ***抽象类***：  
使用模板设计的思想，为子类定义公共的模板（方法及变量），提高代码复用和维护效率，用于单继承场景。  
当一类组件中具备相似的特点但实现该特点的方式不一样时，便可使用抽象类进行模板设计，将公共部分抽象出来。比如动物都有吼叫的特点，但是吼叫的方式各不一样，便可抽取出动物-吼叫()的方法，由具体的动物类去实现该特点。

> ***接口***  
接口用于定义一种标准或能力，提供一种类与类之间协调的方式，只关心类的能力而不关心其实现，用于多继承场景。  
当需要描述一类组件具备某些能力或遵循某些标准时，便可使用接口。  
如实体类可被序列化，可定义Serilizable；控件可被点击，定义Clickable；
协同开发时，上层与下层并行开发，层与层之间都要遵循特定的通信标准，便可用接口来定义这些标准。

#### 10.抽象类是否可以没有方法和属性？
> 可以，java的定义是有抽象方法的类一定是抽象类，但是抽象类中不一定要有抽象方法。

#### 11.接口的意义
> 接口用于定义一种标准或能力，提供一种类与类之间协调的方式，只关心类的能力而不关心其实现，用于多继承场景。  
当需要描述一类组件具备某些能力或遵循某些标准时，便可使用接口。  
如实体类可被序列化，可定义Serilizable；控件可被点击，定义Clickable；
协同开发时，上层与下层并行开发，层与层之间都要遵循特定的通信标准，便可用接口来定义这些标准。

#### 12.泛型中extends和super的区别
> <? extends T> 定义“上界通配符（Upper Bounds Wildcards）”，向下限定，即传入的类必须是T的子类。

> <? super T> 定义“下界通配符（Lower Bounds Wildcards）”，向上限定，即传入的类必须是T的父类。

**PECS（Producer Extends Consumer Super）原则**
- 频繁往外读取内容的，适合用上界Extends。
- 经常往里插入的，适合用下界Super。

```java
public class GenericityTest {

    //Lev 1
    class Food {}

    //Lev 2
    class Fruit extends Food {}

    class Meat extends Food {}

    //Lev 3
    class Apple extends Fruit {}

    class Banana extends Fruit {}

    class Pork extends Meat {}

    class Beef extends Meat {}

    //Lev 4
    class RedApple extends Apple {}

    class GreenApple extends Apple {}

    class Plate<T> {
        private T item;

        public Plate(T t) {
            item = t;
        }

        public void set(T t) {
            item = t;
        }

        public T get() {
            return item;
        }
    }

    public void main(String[] agrs) {
        // 上界<? extends T>不能往里存，只能往外取
        Plate<? extends Fruit> p = new Plate<>(new Apple());
        // 不能存入任何元素
        p.set(new Fruit());    // Error
        p.set(new Apple());    // Error

        // 读取出来的东西只能存放在Fruit或它的基类里。
        Fruit newFruit0 = p.get();
        Object newFruit1 = p.get();
        Apple newFruit2 = p.get();    // Error


        // 下界<? super T>不影响往里存，但往外取只能放在Object对象里
        Plate<? super Fruit> p1 = new Plate<>(new Fruit());
        // 存入元素正常
        p1.set(new Fruit());
        p1.set(new Apple());

        // 读取出来的东西只能存放在Object类里。
        Apple newFruit3 = p1.get();    // Error
        Fruit newFruit4 = p1.get();    // Error
        Object newFruit5 = p1.get();
    }
}

```

#### 13.父类的静态方法能否被子类重写
> 不能，父类的静态方法能被子类继承，但是不能被重写。  

子类中若定义了与父类中相同的静态成员变量和静态成员方法：
- 创建实例时声明的是子类Child child = new Child()，运行时将会调用子类重写后的变量及方法，如果想调用父类的变量和方法则需用“类名.变量”或“类名.方法”调用。
- 创建实例时声明的是父类Father father = new Father()，运行时将会调用父类的变量和方法

```java
public class PolymorphicTest {

    static class Fruit {

        static String color = "五颜六色";
        static String shape = "各种形状";

        static public void call() {
            System.out.println("这是一个水果");
        }

        static public void test() {
            System.out.println("这是没有被子类覆盖的方法");
        }
    }

    static class Banana extends Fruit {

        static String color = "黄色";

        static public void call() {
            System.out.println("这是一个香蕉");
        }

        public static void main(String[] args) {

            Banana banana = new Banana();
            banana.test();     // 这是没有被子类覆盖的方法
            banana.call();     // 这是一个香蕉
            Fruit.call();      // 这是一个水果

            Fruit fruit = new Banana();
            fruit.test();  // 这是没有被子类覆盖的方法
            fruit.call();  // 这是一个水果

            System.out.println(banana.shape + " " + banana.color);   // 各种形状 黄色
            System.out.println(fruit.shape + " " + fruit.color);   // 各种形状 五颜六色
        }
    }
}
```


#### 14.进程和线程的区别
**进程与线程的区别**  
- 进程是资源分配的最小单位，线程是程序执行的最小单位（资源调度的最小单位)  
- 进程有自己的独立地址空间，每启动一个进程，系统就会为它分配地址空间，建立数据表来维护代码段、堆栈段和数据段，这种操作非常昂贵。  而线程是共享进程中的数据的，使用相同的地址空间，因此CPU切换一个线程的花费远比进程要小很多，同时创建一个线程的开销也比进程要小很多。  
- 线程之间的通信更方便，同一进程下的线程共享全局变量、静态变量等数据，而进程之间的通信需要以通信的方式（IPC)进行。不过如何处理好同步与互斥是编写多线程程序的难点。  
- 但是多进程程序更健壮，多线程程序只要有一个线程死掉，整个进程也死掉了，而一个进程死掉并不会对另外一个进程造成影响，因为进程有自己独立的地址空间。

**进程与线程的资源**  
- 堆：是大家共有的空间，分全局堆和局部堆。全局堆就是所有没有分配的空间，局部堆就是用户分配的空间。堆在操作系统对进程初始化的时候分配，运行过程中也可以向系统要额外的堆，但是记得用完了要还给操作系统，要不然就是内存泄漏。  
- 栈：是个线程独有的，保存其运行状态和局部自动变量的。栈在线程开始的时候初始化，每个线程的栈互相独立，因此，栈是thread safe的。操作系统在切换线程的时候会自动的切换栈，就是切换　ＳＳ／ＥＳＰ寄存器。栈空间不需要在高级语言里面显式的分配和释放。

**进程与线程的同步**
- 进程：无名管道、有名管道、信号、共享内存、消息队列、信号量
- 线程：互斥量、读写锁、自旋锁、线程信号、条件变量

#### 15.final，finally，finalize的区别
> ***final***  
用于声明属性、方法和类，分别表示属性不可变、方法不可覆盖、类不可被继承（不能再派生出新的子类）  

> ***finally***  
作为异常处理的一部分，它只能用在try/catch语句中，并且附带着一个语句块，表示这段语句最终一定被执行，经常被用在需要释放资源的情况下。  

```java
public class FinallyTest {

    public static void main(String[] agrs) {
        System.out.println(test(null));
        System.out.println(test("hello"));
        System.out.println(test(new Child()));

        // NullPointerException
        // 11
        // IndexOutOfBoundsException
        // 11
        // Father(): Father
        // Child(): Child
        // ClassCastException
    }

    public static int test(Object content) {
        try {
            String string = (String) content;
            System.out.println(string.charAt(50));
        } catch (NullPointerException e) {
            System.out.println("NullPointerException");
            return 1;
        } catch (IndexOutOfBoundsException ex) {
            System.out.println("IndexOutOfBoundsException");
            throw ex;
        } catch (ClassCastException e) {
            System.out.println("ClassCastException");
            System.exit(0);
        } finally {
            return 11;
        }
    }
}
```

> ***finalize***  
Object类的一个方法，在垃圾回收器执行时会调用被回收对象的finalize()方法，可以覆盖此方法来实现对其他资源的回收。一旦垃圾回收器准备好释放对象占用的空间，将首先调用其finalize()方法，并且在下一次垃圾回收动作发生时，才会真正回收对象占用的内存。一般不需要调用，除非需要在此声明周期函数中进行一些必要的资源释放，如socket连接关闭等。


#### 16.序列化的方式
https://blog.csdn.net/ahuqihua/article/details/81331316
> **序列化** 可以将对象转化成一个字节序列，便于存储。  
**反序列化** 将序列化的字节序列还原。  

优点：可以实现对象的"持久性”，所谓持久性就是指对象的生命周期不取决于程序。  

**序列化条件**  
所需类：ObjectInputStream和ObjectOutputStream  
方法： readObject()和writeObject();

序列化的方式：
- 隐式序列化，实现Serializable接口，会自动序列化非static和transient修饰的成员变量。注意要指定特定的serialVersionUID，因为序列化时会将此值一并写入二进制文件中，反序列化时会将此值与目标转化类的此值进行对比，若不一致可能导致反序列化异常，当不指定时，虚拟机会自动根据类的成员变量动态生成一个hash值，当类发生改变时此动态生成的值会变动，因此需要手动指定此值；
- 显示序列化，实现Externalizable接口，实现readExternal()和writeExternal()方法，此序列化的方式优点在于可控制序列化的字段;
- 显示+隐式方式，实现Serializable接口，并添加writeObject()和readObject()方法。

当父类实现了序列化，子类便默认已实现了序列化，类中使用到的成员变量也默认需要实现序列化，否则会抛出NotSerializableException异常。当序列化的类需要给Collection用时，需要实现Comparable或Compartor

```java
public class SerializeTest {

    static class Student implements Serializable, Comparable {
        private static final long serialVersionUID = 6042446528831502447L;
        String name;
        int grade;
        public static int QQ = 1234;
        private transient String address = "CHINA";

        public Student() {
        }

        public Student(String name, int grade) {
            this.name = name;
            this.grade = grade;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getGrade() {
            return grade;
        }

        public void setGrade(int grade) {
            this.grade = grade;
        }

        @Override
        public String toString() {
            return "Student{" +
                    "name='" + name + '\'' +
                    ", grade=" + grade +
                    ", address='" + address + '\'' +
                    ", QQ=" + QQ +
                    '}';
        }
        
        @Override
        public int compareTo(Object o) {
            if (o == null) {
                return 1;
            }
            
            Student student = (Student) o;
            if ( this.grade > student.grade) {
                return 1;
            } else if ( this.grade == student.grade ) {
                return 0;
            }
            
            return -1;
        }
    }

    static class Teacher implements Externalizable {

        String name;
        int age;
        Student[] students;

        public Teacher() {
        }

        public Teacher(String name, int age, Student[] students) {
            this.name = name;
            this.age = age;
            this.students = students;
        }

        @Override
        public void writeExternal(ObjectOutput out) throws IOException {
            out.writeObject(name);
            out.writeInt(age);
            out.writeObject(students);
        }

        @Override
        public void readExternal(ObjectInput in) throws ClassNotFoundException, IOException {
            name = (String) in.readObject();
            age = in.readInt();
            students = (Student[]) in.readObject();
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getAge() {
            return age;
        }

        public void setAge(int age) {
            this.age = age;
        }

        public Student[] getStudents() {
            return students;
        }

        public void setStudents(Student[] students) {
            this.students = students;
        }

        @Override
        public String toString() {
            return "Teacher{" +
                    "name='" + name + '\'' +
                    ", age=" + age +
                    ", students=" + Arrays.toString(students) +
                    '}';
        }
    }

    static class ExStudent implements Serializable {

        private static final long serialVersionUID = -5177095588972122289L;
        String name;
        transient int grade;
        static int QQ = 666;

        public ExStudent(String name, int grade) {
            this.name = name;
            this.grade = grade;
        }

        private void writeObject(ObjectOutputStream stream) throws IOException {
            stream.defaultWriteObject();
            stream.writeInt(grade);  // 注释后反序列化时grade为0
            stream.writeInt(QQ);
        }

        private void readObject(ObjectInputStream stream) throws ClassNotFoundException, IOException {
            stream.defaultReadObject();
            grade = stream.readInt(); // 注释后反序列化时grade为0
            QQ = stream.readInt();
        }

        @Override
        public String toString() {
            return "ExStudent{" +
                    "name='" + name + '\'' +
                    ", grade=" + grade +
                    ", QQ=" + QQ +
                    '}';
        }
    }

    public static void main(String[] args) throws IOException, ClassNotFoundException {
        //创建可序列化对象
        Student stu = new Student("张三", 3);
        System.out.println("原来的对象：" + stu);

        //创建序列化输出流
        ByteArrayOutputStream buff = new ByteArrayOutputStream();
        ObjectOutputStream out = new ObjectOutputStream(buff);
        //将序列化对象存入缓冲区
        out.writeObject(stu);
        //修改相关值
        Student.QQ = 6666; // 发现打印结果QQ的值被改变
        stu.setGrade(4);   // 发现值没有被改变
        //从缓冲区取回被序列化的对象
        ObjectInputStream in = new ObjectInputStream(new ByteArrayInputStream(buff.toByteArray()));
        Student newStu = (Student) in.readObject();
        System.out.println("序列化后取出的对象：" + newStu.toString() + "\n");

        Teacher teacher = new Teacher("李老师", 29, new Student[]{stu, new Student("王五", 3)});
        System.out.println("原来的对象：" + teacher.toString());
        ObjectOutputStream o = new ObjectOutputStream(new FileOutputStream("F://teacher.txt"));
        o.writeObject(teacher);
        o.close();
        //获得对象
        ObjectInputStream ins = new ObjectInputStream(new FileInputStream("F://teacher.txt"));
        teacher = (Teacher) ins.readObject();
        System.out.println("序列化后取出的对象: " + teacher.toString() + "\n");

        ExStudent exStu = new ExStudent("李四", 4);
        exStu.QQ = 999;
        System.out.println("原来的对象：" + exStu);
        //创建序列化输出流
        ByteArrayOutputStream buff1 = new ByteArrayOutputStream();
        ObjectOutputStream out1 = new ObjectOutputStream(buff1);
        out1.writeObject(exStu);
        //从缓冲区取回被序列化的对象
        ObjectInputStream in1 = new ObjectInputStream(new ByteArrayInputStream(buff1.toByteArray()));
        ExStudent newExStudent = (ExStudent) in1.readObject();
        System.out.println("序列化后取出的对象：" + newExStudent.toString() + "\n");
    }
    
    // output
    // 原来的对象：Student{name='张三', grade=3, address='CHINA', QQ=1234}
    // 序列化后取出的对象：Student{name='张三', grade=3, address='null', QQ=6666}
    // 原来的对象：Teacher{name='李老师', age=29, students=[Student{name='张三', grade=4, address='CHINA', QQ=6666}, Student{name='王五', grade=3, address='CHINA', QQ=6666}]}
    // 序列化后取出的对象: Teacher{name='李老师', age=29, students=[Student{name='张三', grade=4, address='null', QQ=6666}, Student{name='王五', grade=3, address='null', QQ=6666}]}
    // 原来的对象：ExStudent{name='李四', grade=4, QQ=999}
    // 序列化后取出的对象：ExStudent{name='李四', grade=4, QQ=999}
}
```

#### 17.Serializable 和Parcelable 的区别
Serializable查看上文描述。  
> Parcelable接口是Android特有的api，它的出现是为了解决Serializable在序列化的过程中消耗资源严重的问题，但是因为本身使用需要手动处理序列化和反序列化过程，会与具体的代码绑定，使用较为繁琐，一般只获取内存数据的时候使用。
Parcelable依赖于Parcel，Parcel的意思是包装，实现原理是在内存中建立一块共享数据块，序列化和反序列化均是操作这一块的数据，如此来实现。

- Serializable的读写操作是磁盘IO，Parcelable的读写操作是在内存，Serializable需要频繁的IO操作，且是使用反射实现，而Parcelable自己实现封装和解封操作不需要用到反射，数据也存放在native内存中，因此效率要高很多。
- 当一个实体类需要频繁用于数据传输时，建议实现Parcelable接口。

```java
public class Entity implements Parcelable {
    int number;
    String content;
    Student student;

    protected Entity(Parcel in) {
        number = in.readInt();
        content = in.readString();
        student = (Student) in.readSerializable();
        }

    public static final Creator<Entity> CREATOR = new Creator<Entity>() {
        @Override
        public Entity createFromParcel(Parcel in) {
            return new Entity(in);
        }

        @Override
        public Entity[] newArray(int size) {
                return new Entity[size];
        }
    };

    @Override
    public int describeContents() {
        return 0; // 一般为0即可
    }

    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeInt(number);
        dest.writeString(content);
        dest.writeSerializable(student);
    }
}
```

#### 18.静态属性和静态方法是否可以被继承？是否可以被重写？以及原因？
**能被继承，不能被重写。**  
静态属性和静态方法均属于类，不属于类实例，访问均通过类名.xx来访问；
当父类引用指向子类时，使用对象引用的属性或方法，均是使用父类的属性或方法，因此不能被重写。但要注意的时，当对象声明和实例化均是子类时，调用的是子类的属性和方法。

#### 19.静态内部类的设计意图
**成员内部类**
- 成员内部类中不能存在任何static修饰的变量和方法
- 成员内部类时依附于外围类的，只能先创建了外围类才能够创建内部类

**静态内部类**  
非晶态内部类在编译完成后会隐含地保存着一个指向外围类的引用，这使得它能访问外围类的所有方法和属性，而静态内部类却没有。
这也意味着，静态内部类的创建不需要依赖于外围类，且不能使用任何外围类的非static成员变量及方法。  

静态内部类的实例化操作更方便，需要创建外围类实例便可实例化，节省资源。

#### 21.谈谈对kotlin的理解
https://www.jianshu.com/p/0bae35037270

#### 22.闭包和局部内部类的区别
> **闭包**  
函数A的内部定义了一个内部函数B，内部函数B能访问函数A的变量，函数B和其访问的变量便构成了闭包。简言之闭包就是一个能访问函数内部变量的函数。  

> **局部内部类**  
局部内部类就像是方法里面的一个局部变量一样，是不能有public、protected、private以及static修饰符的。

Java的内部类可以定义在方法内部，当内部类使用方法内的变量时实际上就形成了闭包。
其本质就是内部类不活了所在方法内的变量，这个被捕获的变量不能随着方法的执行完毕而消失，
即其生命周期延长了，因为内部类的实例可能还会用到这个变量，所以需要把变量定义为final
（final修饰的常量不会随着方法的执行完毕而消失，JDK8下不加final修饰也能编译通过，因为编译器自动加上了）。

**闭包的用处**
- 读取函数内部的变量
- 函数内部变量的值始终保存在内存钟，不会在外层函数调用后被自动清除

**优点**  
- 变量能长期驻扎在内存中  
- 避免全局变量的污染
- 私有成员的存在

**缺点**  
常驻内存，会增大内存的使用量，使用不当会造成内存泄漏，因此不能滥用闭包。


#### 23.string转换成integer的方式及原理
https://blog.csdn.net/nobody_1/article/details/91488686
```java
 public static int parseInt(String s, int radix)
                throws NumberFormatException
    {
        /*
         * WARNING: This method may be invoked early during VM initialization
         * before IntegerCache is initialized. Care must be taken to not use
         * the valueOf method.
         */

        if (s == null) {
            // Android-changed: Improve exception message for parseInt.
            throw new NumberFormatException("s == null");
        }

        if (radix < Character.MIN_RADIX) {
            throw new NumberFormatException("radix " + radix +
                                            " less than Character.MIN_RADIX");
        }

        if (radix > Character.MAX_RADIX) {
            throw new NumberFormatException("radix " + radix +
                                            " greater than Character.MAX_RADIX");
        }

        int result = 0;
        boolean negative = false;
        int i = 0, len = s.length();
        int limit = -Integer.MAX_VALUE;
        int multmin;
        int digit;

        if (len > 0) {
            char firstChar = s.charAt(0);
            if (firstChar < '0') { // Possible leading "+" or "-"
                if (firstChar == '-') {
                    negative = true;
                    limit = Integer.MIN_VALUE;
                } else if (firstChar != '+')
                    throw NumberFormatException.forInputString(s);

                if (len == 1) // Cannot have lone "+" or "-"
                    throw NumberFormatException.forInputString(s);
                i++;
            }
            multmin = limit / radix;
            while (i < len) {
                // Accumulating negatively avoids surprises near MAX_VALUE
                digit = Character.digit(s.charAt(i++),radix);
                if (digit < 0) {
                    throw NumberFormatException.forInputString(s);
                }
                if (result < multmin) {
                    throw NumberFormatException.forInputString(s);
                }
                result *= radix;
                if (result < limit + digit) {
                    throw NumberFormatException.forInputString(s);
                }
                result -= digit;
            }
        } else {
            throw NumberFormatException.forInputString(s);
        }
        return negative ? result : -result;
    }
```

