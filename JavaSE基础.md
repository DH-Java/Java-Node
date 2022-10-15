# Java基础

## 目录

一、数据类型

- 基本类型
- 包装类型
- 缓存池

二、String

- 概览
- 不可变的好处
- String、StringBuffer and  StringBuilder
- String Pool
- new  String("abc")

三、运算

- 参数传递
- float 与 double
- 隐式类型转换
- switch

四、关键字

- final
- static

五、Object通用方法

- 概览
- equals（）
- hashCode（）
- toString（）
- clone（）

六、继承

- 访问权限
- 抽象类接口
- super
- 重写与重载

七、反射

八、异常

九、泛型

十、注解

十一、特性

- Java各版本的新特性
- Java与c++的区别
- JRE or JDK
- 参考资料



## 一、数据类型

### 基本类型

- byte/8
- char/16
- short/16
- int/32
- float/32
- long/64
- double/64
- boolean/~



boolean只有两个值：true 和 false，可以使用1bit来存储，但是具体大小没有明确规定。JVM会在编译时期将boolean类型的数据转换为int，使用1来表示true，0表示false。JVM支持boolean数组，但是是通过读写byte数组来实现的



### 包装类型

基本类型都有对应的包装类型，基本类型与其对应的包装类型之间的赋值使用自动装箱与拆箱完成

```Java
Integer x  = 2; //装箱 调用了Integer.valueof(2)
int y = x;	    //拆箱 调用了X.intValue()
```



### 缓存池

new Integer(123)与Integer.valueOf(123)的区别在于：

- new Integer(123)        每次都会新建一个对象
- Integer.valueOf(123)  会使用缓存池中的对象，多次调用会取得同一个对象的引用

```java
 Integer x = new Integer(123);
 Integer y = new Integer(123);
 System.out.println(x == y);//false
 Integer z = Integer.valueOf(123);
 Integer k = Integer.valueOf(123);
 System.out.println(z==k);//true
```

valueOf（）方法的实现比较简单，就是先判断值是否在缓存池中，如果在的话就直接返回缓存池的内容。

```Java
  public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
  }
```

在Java8中，Integer缓存池的大小默认为-128~127。

```Java
 private static class IntegerCache {
        static final int low = -128;
        static final int high;
        static final Integer cache[];

        static {
            // high value may be configured by property
            int h = 127;
            String integerCacheHighPropValue =
                sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
            if (integerCacheHighPropValue != null) {
                try {
                    int i = parseInt(integerCacheHighPropValue);
                    i = Math.max(i, 127);
                    // Maximum array size is Integer.MAX_VALUE
                    h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
                } catch( NumberFormatException nfe) {
                    // If the property cannot be parsed into an int, ignore it.
                }
            }
            high = h;

            cache = new Integer[(high - low) + 1];
            int j = low;
            for(int k = 0; k < cache.length; k++){
                          cache[k] = new Integer(j++);

            // range [-128, 127] must be interned (JLS7 5.1.7)
            assert IntegerCache.high >= 127;
       
      }
```

编译器会在自动装箱过程中调用valueOf()方法，因此多个值相同且值在缓存池范围内的Integer实例使用自动装箱来创建，那么就会引用相同的对象。

```java
Integer m = 123;
Integer n = 123;
System.out.println(m==n)//true
```

基本类型对应的缓存池如下：

- boolean values true and false
- all byte values
- short values between -128 and 127
- int values between -128 and 127
- char in the range \u0000 to \u007F



在使用这些基本类型对应的包装类型时，如果该数值范围在缓冲池范围内，就可以直接使用缓冲池中的对象。



在jdk1.8所有的的数值缓冲池中，Integer的缓冲池IntegerCache很特殊，这个缓冲池的下界是-128，上界是127，但是上界是可调的，在启动JVM的时候，通过 -XX：AutoBoxCaCheMax=<size>来指定这个缓冲池的大小，该选项在JVM初始化的时候会设定一个名为java.lang.IntegerCaChe.high系统属性，然后IntegerCache初始化的时候就会读取该系统属性来决定上界



## 二、String

### 概览

String被声明为final，因此它不可被继承的。（Integer等包装类也不能被继承）

在Java8中，String内部使用char数组存储数据。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
```

在Java9之后，String类的实现改用byte数组存储字符串，同时使用coder来标识使用了那种编码

```Java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
    private final byte coder；
```

value数组被声明为final，这意味着value数组初始化之后就不能在引用其他数组。并且String内部没有改变value数组的方法，因此可以保证String不可变。



**不可变的好处**



**1、可以缓存hash值**



因为String的hash值经常被使用，例如String用做HashMap的key。不可变的特性可以使得hash值也不可变，因此只需要进行一次计算。



**2、String Pool的需要**



如果一个String 对象已经被创建过了，那么就会从String Pool中取得引用。只有String是不可变的，才可能使用String Pool。

![](C:/Users/Admin/Pictures/Camera%20Roll/QQ%E6%88%AA%E5%9B%BE20220919181937.png)

**3、安全性**



String经常作为参数，String不可变性可以保证参数不可变。例如在作为网络连接参数的情况下如果String是可变的，那么在网络连接过程中，string被改变，改变String的哪一方以为现在连接的是其他主机，而实际情况却不一定是。



**4、线程安全**

String不可变性天生具备线程安全，可以在多个线程中安全的使用。





### String、StringBuffer and StringBuilder

**1、可变性**



- String不可变
- StringBuffer 和StringBuilder可变

**2、线程安全**

- String不可变，因此是线程安全的
- StringBuilder不是线程安全的
- StringBuffer是线程安全的，内部使用synchronized进行同步  



### **String Pool**

字符串常量池（String Pool） 保存着所有字符串字面量（literal strings）,这些字面量在编译时期就确定。不仅如此，还可以使用String的intern（）方法在运行过程将字符串添加到String Pool中。



当一个字符串调用intern()方法时，如果String Pool中已经存在

一个字符串和该字符串值相等(使用equals()方法进行确定)，那么就会返回String Pool中字符串的引用；否则，就会在String Pool中添加一个新的字符串，并返回这个新字符串的引用



下面示例中，s1和s2采用new String()的方式新建了两个不同字符串，而s3和s4是通过s1.intern()和s2.intern()方法取得同一个字符串引用。intern()首先把"aaa"放到String Pool中，然后返回这个字符串引用，因此s3和s4引用的是同一个字符串。

```Java
       String s1 = new String("123");
        String s2 = new String("123");
        System.out.println(s1==s2);//false
        String s3 = s1.intern();
        String s4 = s2.intern();
        System.out.println(s3==s4);//true
```

如果是采用"bbb"这种字面量的形式创建字符串，会自动地将字符串放入String Pool中。

```java 
String s5 = "bbb";
String s6 = "bbb";
System.out.println(s5==s6); //true
```

在Java7之前，String Pool被放在运行时常量池中，它属于永久代。而在Java7，String Pool 被移到堆中。这是因为永久代的空间有限，在大量使用字符串的场景下会导致OutOfMemoryError错误。



### new String("abc")

使用这个方式一共会创建两个字符串对象（前提是String Pool还没有"abc"字符串对象）。

- "abc"属于字符串字面量，因此编译时期会在String Pool中创建一个字符串对象，指向这个"abc"字符串字面量；
- 而使用new的方式会在堆中创建一个字符串对象。



创建一个测试类，其main方法中使用这种方式来创建字符串对象。

```java 
public  class NewStringTest {
		public static void main（String[] args）{
			String s = new String("abc");
		}
}
```

使用Javap -verbose进行反编译，得到以下内容：

![](C:/Users/Admin/Pictures/Camera%20Roll/QQ%E6%88%AA%E5%9B%BE20220919182150.png)

在Constant Pool中，#19存储这字符串字面量"abc",#3是String Pool的字符串对象，它指向#19这个字符串字面量。在main中，0：行使用new #2在堆中创建一个字符串对象，并且使用ldc#3将String Pool中的字符串对象作为String构造函数的参数。



以下是String构造函数的源码，可以看到，在将一个字符串对象作为另一个字符串对象的构造函数参数时，并不会完全复制value数组内容，而是都会指向同一个value数组。

```java 
public String (String original){
		this.value = original.value;
		this.hash = original.hash;
}
```



## 三、运算

**参数传递*

Java的参数是以值传递的形式传入方法中，而不是引用传递。



以下代码Dog dog 的dog是一个指针，存储的是对象的地址。在将一个参数传入一个方法时，本质上是将对象的地址以值的方式传递到形参中。

```Java
public class Dog {
    String name;

    public Dog(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
    String getObjectAddress(){
        return super.toString();
    }
}
```

在方法中改变对象的字段值会改变原对象该字段值，因为引用的是同一个对象。

```Java
public class PassByValueExample {
    public static void main(String[] args) {
       Dog dog = new Dog("A");
       func(dog);
        System.out.println(dog.getName());
    }

    private static void func(Dog dog) {
        dog.setName("B");
    }
}
```

但是在方法中将指针引用了其他对象，那么此时方法里和方法外的两个指针指向了不同的对象，在一个指针改变其所指向的内容对另一个指针所指向的对象没有影响。

```Java
public class PassByValueExample {
    public static void main(String[] args) {
       Dog dog = new Dog("A");
        System.out.println(dog.getObjectAddress());
       func(dog);
        System.out.println(dog.getObjectAddress());
        System.out.println(dog.getName());
    }

    private static void func(Dog dog) {
        System.out.println(dog.getObjectAddress());
        dog.setName("B");
        System.out.println(dog.getObjectAddress());
        System.out.println(dog.getName());
    }
}
```

**float与double**

Java不能隐式执行向下转型，因为这会使得精度降低。

1.1字面量属于double类型，不能直接将1.1直接赋值给float变量，因为这是向下转型。

```java
//  float f = 1.1；
```

1.1f字面量才是float类型。

```java 
float f = 1.1f;
```

**隐式类型转换**

因为字面量1是int类型，它比short类型精度要高，因此不能隐式地将int类型向下转型为short类型。

```java
short s1 =1；
// s1 = s1 +1;
```

但是使用+=或者++运算符会执行隐式类型转换。

```java
s1 += 1;
s1++;
```

上面的语句相当于将s1 + 1的计算结果进行了向下转型；



**switch**

从Java7开始，可以在switch条件判断语句中使用String对象。

```java
String s = "a";
switch(s){
	case "a":
		System.out.println("aaa");
		break;
	case "b":
		System.out.println("bbb");
		break;
}
```

switch不支持long、float、double，是因为switch的设计初衷是对那些只有少数几个值的类型进行等值判断，如果值过于复制，那么还是用if比较合适。

![](C:/Users/Admin/Pictures/Camera%20Roll/QQ%E6%88%AA%E5%9B%BE20220919191247.png)



## 四、关键字

### **final**

**1、数据**



声明数据为常量，可以是编译时常量，也可以是在运行时被初始化后不能被改变的常量。

- 对于基本类型，final使数值不变；
- 对于引用类型，final使引用不变，也就不能引用其他对象，但是被引用的对象本身是可以修改的。

```java
final int x = 1;
// x = 2; //cannot assign value to final variable 'x'
final A y = new A();
y.a = 1;
```

**2.方法**

声明方法不能被子类重写。



private方法隐式地指定为final，如果在子类中定义的方法和基类中的一个private方法签名相同，此时子类的方法不是重写基类方法，而是在子类中定义了一个新的方法。



**3.类**

声明类不允许被继承



### **stiatic**



**1.静态变量**



- 静态变量：又称为类变量，也就是说这个变量属于类的，类所有的实例都共享静态变量，可以直接通过类名来访问它。静态变量在内存中只存在一份。
- 实例变量：每创建一个实例就会产生一个实例变量，它与该实例同生共死。

```Java
public class  A{
    private int x;				//实例变量
    private static int y;		  //静态变量

    public static void main(String[] args) {
        A a = new A();
        int x = a.x;
        int y = A.y;
    }
}
```

**2.静态变量**

静态方法在类加载的时候就存在了，它不依赖于任何实例。所以静态方法必须有实现，也就是说它不能是抽象方法。

```java
public abstract class A {
		public static void func1(){
			
		}
		// public abstract static void func2();   //Illegal combination of modifiers : 'abstract'  and 'static'
}
```

只能访问所属类的静态字段和静态方法，方法中不能有this和super关键字，因此这两个关键字与具体对象关联。

```java
public class A {
	private static int x;
	private int y;
	
	public static void func1(){
		int a = x;
		//int b = y;  //Non-static field 'y' connot be referenced from a static context
		// int b = this.y;   //'A.this' cannot be referenced from a static context
	}
}
```

**3.静态代码块**

静态语句块在类初始化时运行一次。

```java 
public class A {
	static {
		System.out.println("123");
	}
	
	public static void main(String[] args){
		A a1 = new A();
		A a2 = new A();
	}
}
```

**4.静态内部类**

非静态内部类依赖于外部类的实例，也就是说需要先创建外部类实例，才能用这个实例去创建非静态内部类。而静态内部类不需要。

```java 
public class OuterClass {
	class InnerClass{
		
	}
	static class StaticInnerClass {
		
	}
	public static void main(String[] args){
		//InnerClass innerClass = new InnerClass(); //'OuterClass.this' cannot be referenced from  a static context
	OuterClass outerClass =	new OuterClass();
	InnerClass innnerClass = outerClass.new InnerClass();
	StaticInnerClass staticInnerClass = new StaticInnerClass();
	}
} 
```

静态内部类不能访问外部类的非静态的变量和方法。

**5.静态导包**

在使用静态变量和方法时不用在指明ClassName，从而简化代码，但可读性大大降低。

```java 
import static com.xxx.ClassName.*
```

**6.初始化顺序**

静态变量和静态语句块优先于实例变量和普通语句块，静态变量和静态语句块的初始化顺序取决于它们在代码中的顺序。

```java 
public static String staticField = "静态变量"；
```

```java
static {
	System.out.println("静态语句块");
}
```

```Java
public String field = "实例变量"；
```

```Java
{
	System.out.println("普通语句块")
}
```

最后才是构造函数的初始化。

```Java
public InitialOrderTest(){
	System.out.println("构造函数");
}
```

存在继承的情况下，初始化顺序为：

- 父类（静态变量、静态语句块）
- 子类（静态变量、静态语句块）
- 父类（实例变量、普通语句块）
- 父类（构造函数）
- 子类（实例变量、普通语句块）
- 子类（构造函数）



## **五、Object通用方法**

概览

```Java
public native int hashCode();

public boolean equals(Object obj)
    
protected native Object clone() throws CloneNotSupportedException    

public String toString()
    
public final native Class<?> getClass();

protected void finalize() throws Throwable { }

public final native void notify();

public final native void notifyAll();

public final native void wait(long timeout) throws InterruptedException;

 public final void wait(long timeout, int nanos) throws InterruptedException 
     
 public final void wait() throws InterruptedException 
```

### **equals()**

**1、等价关系**

两个对象具有等价关系，需要满足以下五个条件：

I 自反性

```java
x.equals(x); //true
```

II 对称性

```java
x.equals(y) == y.equals(x); //true
```

III 传递性

```java
if(x.equals(y) && y.equals(z))
	x.equals(z); //true
```

IV 一致性

多次调用equals（）方法结果不变

```java
x.equals(y) == x.equals(y); //true
```

V 与null的比较

对任何不是null的对象x调用x.equals(null)结果都为false

```java
x.equals(null); //false;
```

**2.等价与相等**

- 对于基本类型，==判断两个值是否相等，基本类型没有equals()方法。
- 对于引用类型，==判断两个变量是否引用同一个对象，而equals()判断引用的对象是否等价。

```java
Integer x = new Integer(1);
Integer y = new Integer(1);
System,out.println(x.equals(y)); //true
System,out.println(x==y); //false
```

**3.实现**

- 检查是否为同一个对象的引用，如果是直接返回true；
- 检查是否是同一个类型，如果不是，直接返回false；
- 将Object对象进行转型；
- 判断每个关键域是否相等。

```java
public class EqualExample {
	private int x;
	private int y;
	private int z;
	
	public EqualExample (int x,int y,int z){
		this.x = x;
		this.y = y;
		this.z = z;
	}
	
	@Override
	public boolean equals(Object o){
		if(this ==o) return true;
		if(o == null || getClass() !=o.getClass()) return false;
		
		EqualExample that = (EqualExample) o;
		if(x != that.x) return false;
		if(y !=that.y) return false;
		return z ==that.z;
	}
}
```

### hashCode（）

hashCode()返回哈希值，而equals()是用来判断两个对象是否等价。等价的两个对象散列值一定相同，但是散列值相同的两个对象不一定等价，这是因为计算哈希值具有随机性，两个值不同的对象可能计算出相同的哈希值。



在覆盖equals()方法时应当总是覆盖hashCode()方法，保证等价的两个对象哈希值也相等。



HashSet 和HashMap等集合类使用了hashCode()方法来计算对象应该存储的位置，因此要将对象添加到这些集合类中，需要让对应的类实现hashCode()方法。



下面的代码中，新建了两个等价的对象，并将它们添加到HashSet中。我们希望将这两个对象当成一样的，只在集合中添加一个对象。但是EqualExample没有实现hashCode()方法，因此这两个对象的哈希值是不同的，最终导致集合添加了两个等价的对象。

```java
EqualExample e1 = new EqualExample(1,1,1);
EqualExample e1 = new EqualExample(1,1,1);
System.out.prinltn(e1.equals(e2)); //true
HashSet<EqualExample> set = new HashSet<>();
set.add(e1);
set.add(e2);
System.out.println(set.size()); //2
```

理想的哈希函数应当具有均匀性，即不相等的对象应当均匀分布到所有可能的哈希值上。这就要求了哈希函数要把所有域的值都考虑进来。可以将每个域都当成R进制的某一位，然后组成一个R进制的整数。



R一般取31，因为它是一个奇素数，如果是偶数的话，当出现乘法溢出，信息就会丢失，因为与2相乘相当于向左移一位，最左边的位丢失。并且一个数与31相乘可以转换成移位和减法：31*x ==(x<<5)-x,编译器会自动进行这个优化。

```java 
@Override
	public int hashCode(){
		int result = 17;
		result = 31 *result +x;
		result = 31 *result +y;
		result = 31 *result +z;
		return result
		
	}
```

### **toString()**

默认返回ToStringExample@4554617c这种形式，其中@后面的数值为散列码的无符号十六进制表示。

```java
public class ToStringExample {
	private int number;
	public ToStringExample(int number){
		this.number = number;
	}
}
```

```java
ToStringExample example = new ToStringExample(123);
System.out.println(example.toString());
```

```java
ToStringExample@4554617c
```

### clone()

**1.cloneable**

clone()是Object的protected方法，它不是public，一个类不显式去重写clone(),其他类就不能直接去调用该类实例的clone()方法。

```java
public class CloneExample [
	private int a;
	private int b;
	
]

CloneExample e1 = new CloneExample();
//CloneExample e2 = e1.clone();  //'clone()' has protected access in 'java.lang.Object'
```

重写clone()得到以下实现：

```java
public class CloneExample {
	private int a;
	private int b;
	
	@Override
	public CloneExample clone() throws CloneNotSupportedException{
	return (CloneExample) super.clone();

	}
}
```

```java
CloneExample e1 = new CloneExample();
try {
	CloneExample e2 = e1.clone();
	
}catch(CloneNotSupportedException e){
	e.printStackTrace();
}
```

```java
java.lang.CloneNotSupportedException:CloneExample
```

以上抛出了CloneNotSupportedException，这是因为CloneExample没有实现Cloneable接口。



应该注意的是，clone()方法并不是Cloneable接口的方法，而是Object的一个protected方法。Cloneable接口只是规定，如果一个没有实现Cloneable接口又调用了clone()方法，就会抛出CloneNotSupportedException。

```java
public class CloneExample implements Cloneable {
	private int a;
	private int b;
	
	@Overrode
	public Object clone() throws CloneNotSupportedException {
		return super.clone();
	}
}
```

### **浅拷贝**

拷贝对象和原始对象的引用类型引用同一个对象。

```java
public class ShallowCloneExample implements Cloneable {
	private int[] arr;
	
	public ShallowCloneExample(){
		arr = new int[10];
		for(int i = 0;i<arr.length;i++){
			arr[i] = i;
		}
	}
	public void set(int index,int value){
		arr[index] =value;
	}
	public int get(int index){
		return arr[index];
	}
	@Override
	protected ShallowCloneExample clone() throws CloneNotSupportedException{
		return (ShallowCloneExample) super.clone();
	}
}
```

```java
ShallowCloneEx eq = new ShallowCloneExapmple();
ShallowCloneExample e2 = null;
try {
	e2=e1.clone();
}catch (CloneNotSupportedException e){
	e.printStackTrace();
}
e1.set(2,222);
System.out.println(e2.get(2)); ///222
```

### 深拷贝

拷贝对象和原始对象的引用类型引用不同对象。

```java
public class DeeoCloneExample implements Cloneable {
	private int[] arr;
	public DeepCloneExample(){
		arr = new int[10];
		for(int i = 0; i<arr.length;i++){
			arr[i] = i;
		}
	}
	public void set(int inde,int value){
		arr[index] = value;
	}
	public int get(int index){
		return arr[index];
	}
	@Override
	protected DeepCloneExample clone() throws CloneNotSupportedException {
		DeepCloneExample result = (DeepCloneExample) super.clone();
		result.arr = new int[arr.length];
		for(int i = 0;i<arr.length;i++){
			result.arr[i] = arr[i];
		}
		return result;
	}
}
```

```JAVA
DeepCloneExample e1 = new DeepCloneExample();
DeepCloneExample e2 = null;
try {
	e2 = e1.clone();
}catch( CloneNotSupportedException e){
	e.orintStackTrace();
}
e1.set(2,222);
System.out.println(e2.get(2)); ///222
```

### clone()的替代方案

使用clone()方法来拷贝一个对象既复杂又有风险，它会抛出异常，并且还需要类型转换。EffectiveJava书上讲到，最好不要去使用clone(),可以使用拷贝函数或者拷贝工厂来拷贝一个对象。

```java
public class ClloneConstructorExample {
    private int[] arr;
    
    public CloneConstructorExample(){
        arr = new int[10];
        for(int i = 0; i<arr.length;i++){
            arr[i] = i;
        }
    }
    public CloneConstructorExample(CloneConstructorExample original) {
        arr = new int[original.arr.length];
        for(int i=0;i<original.arr.length;i++){
            arr[i] = original.arr[i];
        }
    }
    public void set(int index,int value){
        arr[index] = value;
    }
    public int get(int index){
        return arr[index];
    }
}
```

```java
CloneConstructorExample e1 = new CloneConstructorExample();
CloneConstructorExample e2 = new CloneConstructorExample(e1);
e1.set(2,222);
System.out.println(e2.get(2)); //2
```

## 六、继承

**访问权限**

Java中有三个访问权限修饰符: private 、 protected 以及 public，r如果不加访问修饰符，表示包级可见。

可以对类或类中的成员（字段和方法）加上访问修饰符。

- 类可见表示其他类可以用这个类创建实例对象。
- 成员可见表示其他类可以用这个类的实例对象访问到该成员；

protected用于修饰成员，表示在继承体系中成员对于子类可见，但是这个访问修饰符对于类没有意义。

设计良好的模块会隐藏所有的实现细节，把它的API与它的实现清晰地隔离开来。模块之间只通过它们的API进行通信，一个模块不需要知道其他模块的内部工作情况，这个概念被称为信息隐藏或封装。因此访问权限应当尽可能地使每个类或者成员不被外界访问。



如果子类的方法重写了父类的方法，那么子类中该方法的访问级别不允许低于父类的访问级别。这是为了确保可以使用父类实例的地方都可以使用子类实例去代替，也就是确保满足里氏替换原则。

字段决不能是公有的，因为这么做的话就失去了对这个字段修改行为的控制，客户端可以对其随意修改。例如下面的例子中，AccessExample拥有id公有字段，如果在某个时刻，我们想要使用int存储id字段，那么就需要修改所有的客户端代码。
