# C# 委托（Delegate）

C# 中的委托（Delegate）类似于 C 或 C++ 中函数的指针。**委托（Delegate）** 是存有对某个方法的引用的一种引用类型变量。引用可在运行时被改变。

##### 声明委托

```c#
delegate <return type> <delegate-name> <parameter list>
```

##### 实例化委托

```c#
public delegate void printString(string s);
...
printString ps1 = new printString(WriteToScreen);
printString ps2 = new printString(WriteToFile);
```

例子

```c#
using System;

delegate int NumberChanger(int n);
namespace DelegateAppl
{
   class TestDelegate
   {
      static int num = 10;
      public static int AddNum(int p)
      {
         num += p;
         return num;
      }

      public static int MultNum(int q)
      {
         num *= q;
         return num;
      }
      public static int getNum()
      {
         return num;
      }

      static void Main(string[] args)
      {
         // 创建委托实例
         NumberChanger nc1 = new NumberChanger(AddNum);
         NumberChanger nc2 = new NumberChanger(MultNum);
         // 使用委托对象调用方法
         nc1(25);
         Console.WriteLine("Value of Num: {0}", getNum());
         nc2(5);
         Console.WriteLine("Value of Num: {0}", getNum());
         Console.ReadKey();
      }
   }
}
```

- *委托可以调用多个方法，即一个委托变量可以引用多个函数，称为多路广播*
- *可以使用+=和-=运算符实现方法的增加和减少*
- *无返回值的委托，引用了多少个方法就会执行多少个方法。有返回值的委托同样会执行多个引用的方法，但返回的值是最后一个方法的返回值*

尚未完全理解

委托用于实现，线程切换