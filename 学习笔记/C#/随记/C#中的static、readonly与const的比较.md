# C#中的static、readonly与const的比较

```csharp
 C#中有两种常量类型，分别为readonly(运行时常量)与const(编译时常量)，本文将就这两种类型的不同特性进行比较并说明各自的适用场景。

工作原理

    readonly为运行时常量，程序运行时进行赋值，赋值完成后便无法更改，因此也有人称其为只读变量。

    const为编译时常量，程序编译时将对常量值进行解析，并将所有常量引用替换为相应值。



    下面声明两个常量：



 



public static readonly int A = 2; //A为运行时常量



public const int B = 3; //B为编译时常量



下面的表达式：



 



int C = A + B;



经过编译后与下面的形式等价：



 



int C = A + 3;



可以看到，其中的const常量B被替换成字面量3，而readonly常量A则保持引用方式。



声明及初始化



    readonly常量只能声明为类字段，支持实例类型或静态类型，可以在声明的同时初始化或者在构造函数中进行初始化，初始化完成后便无法更改。



    const常量除了可以声明为类字段之外，还可以声明为方法中的局部常量，默认为静态类型(无需用static修饰，否则将导致编译错误)，但必须在声明的同时完成初始化。



 



数据类型支持



    由于const常量在编译时将被替换为字面量，使得其取值类型受到了一定限制。const常量只能被赋予数字(整数、浮点数)、字符串以及枚举类型。下面的代码无法通过编译：



 



public const DateTime D = DateTime.MinValue;



改成readonly就可以正常编译：



 



public readonly DateTime D = DateTime.MinValue;



可维护性



    readonly以引用方式进行工作，某个常量更新后，所有引用该常量的地方均能得到更新后的值。



    const的情况要稍稍复杂些，特别是跨程序集调用：



 



public class Class1



{



    public static readonly int A = 2; //A为运行时常量



    public const int B = 3; //B为编译时常量



}



 



public class Class2



{



    public static int C = Class1.A + Class1.B; //变量C的值为A、B之和 



}



 



Console.WriteLine(Class2.C); //输出"5"



假设Class1与Class2位于两个不同的程序集，现在更改Class1中的常量值：



 



public class Class1



{



    public static readonly int A = 4; //A为运行时常量



    public const int B = 5; //B为编译时常量



}



 编译Class1并部署（注意：这时并没有重新编译Class2），再次查看变量C的值：



 



Console.WriteLine(Class2.C); //输出"7"



结果可能有点出乎意料，让我们来仔细观察变量C的赋值表达式：



 



public static int C = Class1.A + Class1.B;



编译后与下面的形式等价： 



 



 



 



public static int C = Class1.A + 3;



     因此不管常量B的值如何变，对最终结果都不会产生影响。虽说重新编译Class2即可解决这个问题，但至少让我们看到了const可能带来的维护问题。



 



性能比较



    const直接以字面量形式参与运算，性能要略高于readonly，但对于一般应用而言，这种性能上的差别可以说是微乎其微。



 



适用场景



    在下面两种情况下：



    a.取值永久不变(比如圆周率、一天包含的小时数、地球的半径等)



    b.对程序性能要求非常苛刻



    可以使用const常量，除此之外的其他情况都应该优先采用readonly常量。
```

 

 

**C#中的static 和Java中的static**

简单，两者用法完全是一致的。从两方面讨论：

**1.** 变量是属于类的，不是实例级别的。只能通过类名调用，不能通过实例调用。

**2.** 如果在定义时就赋值了，那么在类初始化的时候，最先完成所有静态变量的赋值。但是要注意，所有静态变量的初始化顺序是无法确定的。



**C# 中的const 和Java中的finnal**

很长一段时间我一直认为两者是相同的作用，无非是变量初始化后不能更改，即只能在定义时或者构造函数中赋值。然而这仅仅只是片面的，下面将为大家详细分析：

**1.修饰变量**

准确的说C#中的const 等价于 Java中的static final，也就是说，Java中final不具有static的功能。而C#中的const具有static的功能。因此在C#中 public static const string 等将于 public const string。

**2.修饰类和方法**

此时Java中的final类似C#中的sealed，就是说，final修饰的类不能被继承，final修饰的方法不能被覆盖。

而C#中的const不能修饰类和方法。

**问题：**

**1.** 私有静态成员的作用（private static 变量）

字面表示私有的，类外不能使用；静态的，全局变量。看上去很矛盾，又不能被类外使用，要全局的有什么用。问得好，类中全局也是很有意义的，例如 private static int a = 5，那么就可以保证变量a在类的初始化过程中将被优先初始化（在构造函数执行之前）。这样如果对象A的初始化需要对象B的实例，那么就可以用这种申明，以保证在类A在构造函数中能够使用类B的实例。同时private又能够保证类B的实例只能在类A中使用，起到很好的密封作用。

**2.** 私有最终成员作用（private final 变量）

在类构造函数完成前必须对该成员完成初始化，一旦定义不许更改；该成员只能在本类中使用。实例，子类中都不能使用。

private static final修饰的成员在申明的时就被赋值，保证在构造函数中可以被使用，一个被private static final修饰的成员通常表示其他组件的一个实例，且变量是类中的全局变量。

private final     修饰的成员在构造中被赋值，表示它是该类全局的私有成员变量，且该类的构造需要传入他们的初始值，才能完成类的初始化。


  

**C# const和static readonly区别**

const: 用const修饰符声明的成员叫常量，是在编译期初始化并嵌入到客户端程序 
 static readonly: 用static readonly修饰符声明的成员依然是变量，只不过具有和常量类似的使用方法：通过类进行访问、初始化后不可以修改。但与常量不同的是这种变量是在运行期初始化。

C# const和static readonly区别示例：

```
 using System;  using System.Collections.Generic;  using System.Text;     namespace Example02Lib  {  public class Class1  {  public const String strConst = "Const";  public static readonly String strStaticReadonly = "StaticReadonly";  //public const String strConst = "Const Changed";  //public static readonly String strStaticReadonly = "StaticReadonly Changed";  }  } 
```

客户端代码：

```
 using System;  using System.Collections.Generic;  using System.Text;  using Example02Lib;   namespace Example02  {  class Program  {  static void Main(string[] args)  {  //修改Example02中Class1的strConst初始值后，只编译Example02Lib项目  //然后到资源管理器里把新编译的Example02Lib.dll拷贝Example02.exe所在的目录，
执行Example02.exe  //切不可在IDE里直接调试运行因为这会重新编译整个解决方案！！   //可以看到strConst的输出没有改变，而strStaticReadonly的输出已经改变  //表明Const变量是在编译期初始化并嵌入到客户端程序，而StaticReadonly是在运行时初始化的  Console.WriteLine("strConst : {0}", Class1.strConst);  Console.WriteLine("strStaticReadonly : {0}", Class1.strStaticReadonly);   Console.ReadLine();  }  }  } 
```

修改后的示例：

```
 using System;  using System.Collections.Generic;  using System.Text;     namespace Example02Lib  {  public class Class1  {  //public const String strConst = "Const";  //public static readonly String strStaticReadonly = "StaticReadonly";  public const String strConst = "Const Changed";  public static readonly String strStaticReadonly = "StaticReadonly Changed";  }  } 
```