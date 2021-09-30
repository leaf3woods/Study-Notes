# [C#中dll调用方法](http://www.cnblogs.com/Asuphy/p/4206623.html) 

**一、   DLL与应用程序**   

动态链接库（也称为DLL，即为“Dynamic Link Library”的缩写）是Microsoft Windows最重要的组成要素之一，打开Windows系统文件夹，你会发现文件夹中有很多DLL文件，Windows就是将一些主要的系统功能以DLL模块的形式实现。   

动态链接库是不能直接执行的，也不能接收消息，它只是一个独立的文件，其中包含能被程序或其它DLL调用来完成一定操作的函数(方法。注：C#中一般称为“方法”)，但这些函数不是执行程序本身的一部分，而是根据进程的需要按需载入，此时才能发挥作用。   

DLL只有在应用程序需要时才被系统加载到进程的虚拟空间中，成为调用进程的一部分，此时该DLL也只能被该进程的线程访问，它的句柄可以被调用进程所使用，而调用进程的句柄也可以被该DLL所使用。在内存中，一个DLL只有一个实例，且它的编制与具体的编程语言和编译器都没有关系，所以可以通过DLL来实现混合语言编程。DLL函数中的代码所创建的任何对象（包括变量）都归调用它的线程或进程所有。   

下面列出了当程序使用 DLL 时提供的一些优点：[1]   

**1)    使用较少的资源**   

当多个程序使用同一个函数库时，DLL 可以减少在磁盘和物理内存中加载的代码的重复量。这不仅可以大大影响在前台运行的程序，而且可以大大影响其他在 Windows 操作系统上运行的程序。   

**2)    推广模块式体系结构**   

DLL 有助于促进模块式程序的开发。这可以帮助您开发要求提供多个语言版本的大型程序或要求具有模块式体系结构的程序。模块式程序的一个示例是具有多个可以在运行时动态加载的模块的计帐程序。   

**3)    简化部署和安装**   

当 DLL 中的函数需要更新或修复时，部署和安装 DLL 不要求重新建立程序与该 DLL 的链接。此外，如果多个程序使用同一个 DLL，那么多个程序都将从该更新或修复中获益。当您使用定期更新或修复的第三方 DLL 时，此问题可能会更频繁地出现。   

**二、   DLL的调用**   

每种编程语言调用DLL的方法都不尽相同，在此只对用C#调用DLL的方法进行介绍。首先,您需要了解什么是托管,什么是非托管。一般可以认为：非托管代码主要是基于win 32平台开发的DLL，activeX的组件，托管代码是基于.net平台开发的。如果您想深入了解托管与非托管的关系与区别，及它们的运行机制，请您自行查找资料，本文件在此不作讨论。   

**(一)   调用DLL中的非托管函数一般方法**   

**首先**，应该在C#语言源程序中声明外部方法，其基本形式是：   

[DLLImport(“DLL文件”)]   

修饰符 extern 返回变量类型 方法名称 （参数列表）   

**其中**：   

DLL文件：包含定义外部方法的库文件。   

修饰符： 访问修饰符，除了abstract以外在声明方法时可以使用的修饰符。   

返回变量类型：在DLL文件中你需调用方法的返回变量类型。   

方法名称：在DLL文件中你需调用方法的名称。   

参数列表：在DLL文件中你需调用方法的列表。   

**注意**：需要在程序声明中使用System.Runtime.InteropServices命名空间。   

   DllImport只能放置在方法声明上。   

DLL文件必须位于程序当前目录或系统定义的查询路径中（即：系统环境变量中Path所设置的路径）。   

返回变量类型、方法名称、参数列表一定要与DLL文件中的定义相一致。    

若要使用其它函数名，可以使用EntryPoint属性设置，如：   

```c#
[DllImport("user32.dll", EntryPoint="MessageBoxA")]   
static extern int MsgBox(int hWnd, string msg, string caption, int type);   
```

其它可选的 DllImportAttribute 属性：   

CharSet 指示用在入口点中的字符集，如：

```
CharSet=CharSet.Ansi;
```

SetLastError 指示方法是否保留 Win32"上一错误"，如：

```
SetLastError=true；  
```

ExactSpelling 指示 EntryPoint 是否必须与指示的入口点的拼写完全匹配，如：

```
ExactSpelling=false；   
```

PreserveSig指示方法的签名应当被保留还是被转换， 如：

```
PreserveSig=true；   
```

CallingConvention指示入口点的调用约定， 如：

```c#
CallingConvention=CallingConvention.Winapi；     
```

此外，关于“数据封送处理”及“封送数字和逻辑标量”请参阅其它一些文章[2]。   

**C#例子：**   

\1.    启动VS.NET，新建一个项目，项目名称为“Tzb”，模板为“Windows 应用程序”。   

\2.    在“工具箱”的“ Windows 窗体”项中双击“Button”项，向“Form1”窗体中添加一个按钮。   

\3.    改变按钮的属性：Name为 “B1”，Text为 “用DllImport调用DLL弹出提示框”，并将按钮B1调整到适当大小，移到适当位置。   

\4.    在类视图中双击“Form1”，打开“Form1．cs”代码视图，在“namespace Tzb”上面输入“using System.Runtime.InteropServices;”，以导入该命名空间。   

\5.    在“Form1．cs［设计］”视图中双击按钮B1，在“B1_Click”方法上面使用关键字 static 和 extern 声明方法“MsgBox”，将 DllImport 属性附加到该方法，这里我们要使用的是“user32．dll”中的“MessageBoxA”函数，具体代码如下：   

```c#
[DllImport("user32.dll", EntryPoint="MessageBoxA")]
static extern int MsgBox(int hWnd, string msg, string caption, int type);
```

然后在“B1_Click”方法体内添加如下代码，以调用方法“MsgBox”：    

```c#
MsgBox(0," 这就是用 DllImport 调用 DLL 弹出的提示框哦！ "," 挑战杯 ",0x30);
```

\6.    按“F5”运行该程序，并点击按钮B1，便弹出如下提示框：   

![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image005.gif)    

**(二)   动态装载、调用DLL中的非托管函数**   

在上面已经说明了如何用DllImport调用DLL中的非托管函数，但是这个是全局的函数，假若DLL中的非托管函数有一个静态变量S，每次调用这个函数的时候，静态变量S就自动加1。结果，当需要重新计数时，就不能得出想要的结果。下面将用例子说明：   

**1.    DLL的创建**   

**1)**    启动Visual C++ 6.0；   

**2)**    新建一个“Win32 Dynamic-Link Library”工程，工程名称为“Count”；   

**3)**    在“Dll kind”选择界面中选择“A simple dll project”；   

**4)**    打开Count.cpp，添加如下代码：   

```c#
// 导出函数，使用“ _stdcall ” 标准调用
extern "C" _declspec(dllexport)int _stdcall count(int init);

int _stdcall count(int init)
{
    count 函数，使用参数 init 初始化静态的整形变量 S ，并使 S 自加 1 后返回该值
    static int S = init;
    S++;
    return S;
}
```

**5)**    按“F7”进行编译，得到Count.dll（在工程目录下的Debug文件夹中）。         

**2.     用DllImport调用DLL中的count函数**   

**1)**    打开项目“Tzb”，向“Form1”窗体中添加一个按钮。   

**2)**    改变按钮的属性：Name为 “B2”，Text为 “用DllImport调用DLL中count函数”，并将按钮B1调整到适当大小，移到适当位置。   

**3)**    打开“Form1．cs”代码视图，使用关键字 static 和 extern 声明方法“count”，并使其具有来自 Count.dll 的导出函数count的实现，代码如下：   

```c#
[DllImport("Count.dll")]
static extern int count(int init);       
```

**4)**    在“Form1．cs［设计］”视图中双击按钮B2，在“B2_Click”方法体内添加如下代码：   

```c#
MessageBox.Show(" 用 DllImport 调用 DLL 中的 count 函数， \n 传入的实参为 0 ，得到的结果是： " + count(0).ToString(), " 挑战杯 ");
MessageBox.Show(" 用 DllImport 调用 DLL 中的 count 函数， \n 传入的实参为 10 ，得到的结果是： " + count(10).ToString() + "\n 结果可不是想要的 11 哦！！！ ", " 挑战杯 ");
MessageBox.Show(" 所得结果表明： \n 用 DllImport 调用 DLL 中的非托管 \n 函数是全局的、静态的函数！！！ ", " 挑战杯 "); 
```

**5)**    把Count.dll复制到项目“Tzb”的bin\Debug文件夹中，按“F5”运行该程序，并点击按钮B2，便弹出如下三个提示框：        

![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image013.gif) ![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image014.gif) ![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image015.gif)   

第1个提示框显示的是调用“count(0)”的结果，第2个提示框显示的是调用“count(10)”的结果，由所得结果可以证明“用DllImport调用DLL中的非托管函数是全局的、静态的函数”。所以，有时候并不能达到我们目的，因此我们需要使用下面所介绍的方法：C#动态调用DLL中的函数。     

**3.    C#动态调用DLL中的函数**   

因为C#中使用DllImport是不能像动态load/unload assembly那样，所以只能借助API函数了。在kernel32.dll中，与动态库调用有关的函数包括[3]：   

①LoadLibrary（或MFC 的AfxLoadLibrary），装载动态库。   

②GetProcAddress，获取要引入的函数，将符号名或标识号转换为DLL内部地址。   

③FreeLibrary（或MFC的AfxFreeLibrary），释放动态链接库。   

它们的原型分别是：   

HMODULE LoadLibrary(LPCTSTR lpFileName);   

FARPROC GetProcAddress(HMODULE hModule, LPCWSTR lpProcName);   

BOOL FreeLibrary(HMODULE hModule);       

现在，我们可以用IntPtr hModule=LoadLibrary(“Count.dll”);来获得Dll的句柄,

用IntPtr farProc=GetProcAddress(hModule,”_count@4”);来获得函数的入口地址。   

但是，知道函数的入口地址后，怎样调用这个函数呢？因为在C#中是没有函数指针的，没有像C++那样的函数指针调用方式来调用函数，所以我们得借助其它方法。经过研究，发现我们可以通过结合使用System.Reflection.Emit及System.Reflection.Assembly里的类和函数达到我们的目的。为了以后使用方便及实现代码的复用，我们可以编写一个类。   

**1)    dld类的编写：**   

\1.    打开项目“Tzb”，打开类视图，右击“Tzb”，选择“添加”-->“类”，类名设置为“dld”，即dynamic loading dll 的每个单词的开头字母。   

\2.    添加所需的命名空间及声明参数传递方式枚举：   

```c#
using System.Runtime.InteropServices; // 用 DllImport 需用此 命名空间
using System.Reflection; // 使用 Assembly 类需用此 命名空间
using System.Reflection.Emit; // 使用 ILGenerator 需用此 命名空间
```

​     在“public class dld”上面添加如下代码声明参数传递方式枚举：   

```c#
/// <summary>
///参数传递方式枚举 ,ByValue 表示值传递 ,ByRef 表示址传递
/// </summary>
public enum ModePass
{       
    ByValue = 0x0001,
    ByRef = 0x0002
}      
```

\3.    声明LoadLibrary、GetProcAddress、FreeLibrary及私有变量hModule和farProc：   

```c#
/// <summary>      
/// 原型是 :HMODULE LoadLibrary(LPCTSTR lpFileName);      
/// </summary>       
/// <param name="lpFileName">DLL 文件名 </param>
/// <returns> 函数库模块的句柄 </returns>
[DllImport("kernel32.dll")]
static extern IntPtr LoadLibrary(string lpFileName);
/// < summary >
/// 原型是 : FARPROC GetProcAddress(HMODULE hModule, LPCWSTR lpProcName);
/// </summary>
/// <param name="hModule"> 包含需调用函数的函数库模块的句柄 </param>
/// <param name="lpProcName"> 调用函数的名称 </param>
/// <returns> 函数指针 </returns>
[DllImport("kernel32.dll")]
static extern IntPtr GetProcAddress(IntPtr hModule, string lpProcName);
/// <summary>
/// 原型是 : BOOL FreeLibrary(HMODULE hModule);
/// </summary>       /// <param name="hModule"> 需释放的函数库模块的句柄 </param>
/// <returns> 是否已释放指定的 Dll</returns>
[DllImport("kernel32",EntryPoint="FreeLibrary",SetLastError=true)]
static extern bool FreeLibrary(IntPtr hModule);
/// <summary>
/// Loadlibrary 返回的函数库模块的句柄
/// </summary>
private IntPtr hModule = IntPtr.Zero;
/// <summary>
/// GetProcAddress 返回的函数指针
/// </summary>
private IntPtr farProc = IntPtr.Zero;   
```



\4.    添加LoadDll方法，并为了调用时方便，重载了这个方法：   

```c#
/// <summary>
/// 装载 Dll
/// </summary>
/// <param name="lpFileName">DLL 文件名 </param>
public void LoadDll(string lpFileName)
{
    hModule = LoadLibrary(lpFileName);
    if (hModule == IntPtr.Zero)
        throw (new Exception(" 没有找到 :" + lpFileName + "."));
}        
```

​     若已有已装载Dll的句柄，可以使用LoadDll方法的第二个版本：   

```c#
public void LoadDll(IntPtr HMODULE)
{
    if (HMODULE == IntPtr.Zero)
        throw (new Exception(" 所传入的函数库模块的句柄 HMODULE 为空 ."));
    hModule = HMODULE;
}
```

\5.    添加LoadFun方法，并为了调用时方便，也重载了这个方法，方法的具体代码及注释如下：   

```c#
/// <summary>
/// 获得函数指针
/// </summary>
/// <param name="lpProcName"> 调用函数的名称 </param>
public void LoadFun(string lpProcName)
{
    // 若函数库模块的句柄为空，则抛出异常
    if (hModule == IntPtr.Zero)
        throw (new Exception(" 函数库模块的句柄为空 , 请确保已进行 LoadDll 操作 !"));
    // 取得函数指针
    farProc = GetProcAddress(hModule, lpProcName);
    // 若函数指针，则抛出异常
    if (farProc == IntPtr.Zero)
        throw (new Exception(" 没有找到 :" + lpProcName + " 这个函数的入口点 "));
}
```

```c#
/// <summary>
/// 获得函数指针
/// </summary>
/// <param name="lpProcName"> 调用函数的名称 </param>
public void LoadFun(string lpProcName)
{
    // 若函数库模块的句柄为空，则抛出异常
    if (hModule == IntPtr.Zero)
        throw (new Exception(" 函数库模块的句柄为空 , 请确保已进行 LoadDll 操作 !"));
    // 取得函数指针
    farProc = GetProcAddress(hModule, lpProcName);
    // 若函数指针，则抛出异常
    if (farProc == IntPtr.Zero)
        throw (new Exception(" 没有找到 :" + lpProcName + " 这个函数的入口点 "));
}
```

​     

\6.    添加UnLoadDll及Invoke方法，Invoke方法也进行了重载：   

```c#
/// <summary>
/// 卸载 Dll
/// </summary>
public void UnLoadDll()
{
    FreeLibrary(hModule);
    hModule = IntPtr.Zero;
    farProc = IntPtr.Zero;
}
```

​     Invoke方法的第一个版本：   

```c#
/// <summary>
/// 调用所设定的函数
/// </summary>
/// <param name="ObjArray_Parameter"> 实参 </param>      
/// <param name="TypeArray_ParameterType"> 实参类型 </param>       
/// <param name="ModePassArray_Parameter"> 实参传送方式 </param>      
/// <param name="Type_Return"> 返回类型 </param>     
/// <returns> 返回所调用函数的 object</returns>      
public object Invoke(object[] ObjArray_Parameter, Type[] TypeArray_ParameterType, ModePass[] ModePassArray_Parameter, Type Type_Return)
{
    // 下面 3 个 if 是进行安全检查 , 若不能通过 , 则抛出异常
    if (hModule == IntPtr.Zero)
        throw (new Exception(" 函数库模块的句柄为空 , 请确保已进行 LoadDll 操作 !"));
    if (farProc == IntPtr.Zero)
        throw (new Exception(" 函数指针为空 , 请确保已进行 LoadFun 操作 !"));
    if (ObjArray_Parameter.Length != ModePassArray_Parameter.Length)
        throw (new Exception(" 参数个数及其传递方式的个数不匹配 ."));
    // 下面是创建 MyAssemblyName 对象并设置其 Name 属性
    AssemblyName MyAssemblyName = new AssemblyName();
    MyAssemblyName.Name = "InvokeFun";
    // 生成单模块配件
    AssemblyBuilder MyAssemblyBuilder = AppDomain.CurrentDomain.DefineDynamicAssembly(MyAssemblyName, AssemblyBuilderAccess.Run);
    ModuleBuilder MyModuleBuilder = MyAssemblyBuilder.DefineDynamicModule("InvokeDll");
    // 定义要调用的方法 , 方法名为“ MyFun ”，返回类型是“ Type_Return ”参数类型是“ TypeArray_ParameterType ”
    MethodBuilder MyMethodBuilder = MyModuleBuilder.DefineGlobalMethod("MyFun", MethodAttributes.Public | MethodAttributes.Static, Type_Return, TypeArray_ParameterType);
    // 获取一个 ILGenerator ，用于发送所需的 IL
    ILGenerator IL = MyMethodBuilder.GetILGenerator();
    int i;
    for (i = 0; i < ObjArray_Parameter.Length; i++)
    {
        // 用循环将参数依次压入堆栈
        switch (ModePassArray_Parameter[i])
        {
            case ModePass.ByValue:
                IL.Emit(OpCodes.Ldarg, i);
                break;
            case ModePass.ByRef:
                IL.Emit(OpCodes.Ldarga, i);
                break;
            default:
                throw (new Exception(" 第 " + (i + 1).ToString() + " 个参数没有给定正确的传递方式 ."));
        }
    }
    if (IntPtr.Size == 4)
    {
        // 判断处理器类型
        IL.Emit(OpCodes.Ldc_I4, farProc.ToInt32());
    }
    else if (IntPtr.Size == 8)
    {
        IL.Emit(OpCodes.Ldc_I8, farProc.ToInt64());
    }
    else
    {
        throw new PlatformNotSupportedException();
    }
    IL.EmitCalli(OpCodes.Calli, CallingConvention.StdCall, Type_Return, TypeArray_ParameterType);
    IL.Emit(OpCodes.Ret);
    // 返回值
    MyModuleBuilder.CreateGlobalFunctions();
    // 取得方法信息
    MethodInfo MyMethodInfo = MyModuleBuilder.GetMethod("MyFun");
    return MyMethodInfo.Invoke(null, ObjArray_Parameter);// 调用方法，并返回其值
}
```

​     Invoke方法的第二个版本，它是调用了第一个版本的：   

```c#
/// <summary>      
///  调用所设定的函数
/// </summary>       
/// <param name="IntPtr_Function"> 函数指针 </param>
/// <param name="ObjArray_Parameter"> 实参 </param>       
/// <param name="TypeArray_ParameterType"> 实参类型 </param>       
/// <param name="ModePassArray_Parameter"> 实参传送方式 </param> 
/// <param name="Type_Return"> 返回类型 </param>  
/// <returns> 返回所调用函数的 object</returns>       
public object Invoke(IntPtr IntPtr_Function, object[] ObjArray_Parameter, Type[] TypeArray_ParameterType, ModePass[] ModePassArray_Parameter, Type Type_Return)
{
    // 下面 2 个 if 是进行安全检查 , 若不能通过 , 则抛出异常
    if (hModule == IntPtr.Zero)
        throw (new Exception(" 函数库模块的句柄为空 , 请确保已进行 LoadDll 操作 !"));
    if (IntPtr_Function == IntPtr.Zero) 
        throw (new Exception(" 函数指针 IntPtr_Function 为空 !"));
    farProc = IntPtr_Function;
    return Invoke(ObjArray_Parameter, TypeArray_ParameterType, ModePassArray_Parameter, Type_Return);
}
```

​             

**2)    dld类的使用：**   

1． 打开项目“Tzb”，向“Form1”窗体中添加三个按钮。Name 和Text属性分别为 “B3”、“用LoadLibrary方法装载Count.dll”，“B4”、“调用count方法”，“B5”、“卸载Count.dll”，并调整到适当的大小及位置。   

2． 在“Form1．cs［设计］”视图中双击按钮B3，在“B3_Click”方法体上面添加代码，创建一个dld类实例：   

```c#
/// <summary>
/// 创建一个 dld 类对象
/// </summary>
private dld myfun=new dld();
```

 3． 在“B3_Click”方法体内添加如下代码：   

```c#
myfun.LoadDll("Count.dll"); // 加载 "Count.dll"  
myfun.LoadFun("_count@4"); // 调入函数 count, "_count@4" 是它的入口，可通过 Depends 查看    
```

4． “Form1．cs［设计］”视图中双击按钮B4，在“B4_Click”方法体内添加如下代码：   

```c#
object[] Parameters = new object[] { (int)0 }; // 实参为 0

Type[] ParameterTypes = new Type[] { typeof(int) };// 实参类型为 int

ModePass[] themode = new ModePass[] { ModePass.ByValue }; // 传送方式为值传

Type Type_Return = typeof(int); // 返回类型为 int

// 弹出提示框，显示调用 myfun.Invoke 方法的结果，即调用 count 函数
MessageBox.Show(" 这是您装载该 Dll 后第 " +
    myfun.Invoke(Parameters, ParameterTypes, themode, Type_Return).ToString() +
    " 次点击此按钮。 ", " 挑战杯 ");
```

5． “Form1．cs［设计］”视图中双击按钮B5，在“B5_Click”方法体内添加如下代码：   

```c#
myfun.UnLoadDll();       
```

6． 按“F5”运行该程序，并先点击按钮B3以加载“Count.dll”，接着点击按钮B4三次以调用3次“count(0)”，先后弹出的提示框如下：   

![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image045.gif)![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image046.gif)![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image047.gif)

​     这三个提示框所得出的结果说明了静态变量S 经初始化后，再传入实参“0”也不会改变其值为“0”。   

7． 点击按钮B5以卸载“Count.dll”，再点击按钮B3进行装载“Count.dll”，再点击按钮B4查看调用了“count(0)”的结果：   

![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image048.gif)

从弹出的提示框所显示的结果可以看到又开始重新计数了，也就是实现了DLL的动态装载与卸载了。   

#### (三)  调用托管DLL一般方法 

C# 调用托管DLL是很简单的，只要在“解决方案资源管理器”中的需要调用DLL的项目下用鼠标右击“引用”，并选择“添加引用”，然后选择已列出的DLL或通过浏览来选择DLL文件，最后需要用using 导入相关的命名空间。   

#### (四)   动态调用托管DLL   

C# 动态调用托管DLL也需要借助System.Reflection.Assembly里的类和方法，主要使用了Assembly.LoadFrom。现在，用例子说明：   

   首先，启动VS.NET，新建一个Visual C# 项目，使用的模板为“类库”，名称为“CsCount”，并在类“Class1”中添加静态整型变量S及方法count：   

```c#
// 由于 static 不能修饰方法体内的变量，所以需放在这里，且初始化值为 int.MinValue
static int S = int.MinValue;
public int count(int init)
{
    // 判断 S 是否等于 int.MinValue ，是的话把 init 赋值给 S
    if (S == int.MinValue) S = init; S++;
    //S 自增 1
    return S; // 返回 S
}
```

​	然后，打开项目“Tzb”，向“Form1”窗体中添加一个按钮，Name属性为“B6”，Text属性为“用Assembly类来动态调用托管DLL”，调整到适当大小和位置，双击按钮B6，转入代码视图，先导入命名空间：using System.Reflection; 接着添加Invoke方法和B6_Click方法代码：    

```c#
private object Invoke(string lpFileName, string Namespace, string ClassName, string lpProcName, object[] ObjArray_Parameter)
{
    try
    { 
        // 载入程序集
        Assembly MyAssembly = Assembly.LoadFrom(lpFileName);
        Type[] type = MyAssembly.GetTypes();
        foreach (Type t in type)
        {
            // 查找要调用的命名空间及类
            if (t.Namespace == Namespace && t.Name == ClassName)
            {
                // 查找要调用的方法并进行调用
             	MethodInfo m = t.GetMethod(lpProcName);
                if (m != null)
                {
                    object o = Activator.CreateInstance(t);
                    return m.Invoke(o, ObjArray_Parameter);
                }
                else MessageBox.Show(" 装载出错 !");
            }
        }
    }// try
    catch (System.NullReferenceException e)
    {
        MessageBox.Show(e.Message);
    }//catch
    return (object)0;
}// Invoke   
```

“B6_Click”方法体内代码如下：   

```c#
// 显示 count(0) 返回的值       
MessageBox.Show(" 这是您第 "+Invoke("CsCount.dll","CsCount","Class1","count",new object[]{(int)0}).ToString()+" 次点击此按钮。 "," 挑战杯 ");       
```

最后，把项目“CsCount”的bin\Debug文件夹中的CsCount.dll复制到项目“Tzb”的bin\Debug文件夹中，按“F5”运行该程序，并点击按钮B6三次，将会弹出3个提示框，内容分别是“这是您第 1次点击此按钮。”、“这是您第 2次点击此按钮。”、“这是您第 3次点击此按钮。”，由此知道了静态变量S在这里的作用。    

## (五\) C#程序嵌入DLL的调用 

   DLL文件作为资源嵌入在C#程序中，我们只要读取该资源文件并以“byte[]”返回，然后就用“Assembly Load(byte[]);”得到DLL中的程序集，最后就可以像上面的Invoke方法那样对DLL中的方法进行调用。当然不用上面方法也可以，如用接口实现动态调用，但DLL中必须有该接口的定义并且程序中也要有该接口的定义；也可用反射发送实现动态调用[4]。现在我只对像上面的Invoke方法那样对DLL中的方法进行调用进行讨论，为了以后使用方便及实现代码的复用，我们可以结合上一个编写一个类。   

**1)    ldfs类的编写：**   

在项目“Tzb”中新建一个名为ldfs的类，意为“load dll from resource”，请注意，在这个类中“resource”不只是嵌入在EXE程序中的资源，它也可以是硬盘上任意一个DLL文件，这是因为ldfs的类中的方法LoadDll有些特别，就是先从程序的内嵌的资源中查找需加载的DLL，如果找不到，就查找硬盘上的。   

首先导入所需的命名空间：   

```c#
using System.IO; // 对文件的读写需要用到此命名空间       

using System.Reflection; // 使用 Assembly 类需用此命名空间       

using System.Reflection.Emit; // 使用 ILGenerator 需用此命名空间       
```

声明一静态变量MyAssembly：   

// 记录要导入的程序集      

```c#
 static Assembly MyAssembly;      
```

添加LoadDll方法：     

```c#
private byte[] LoadDll(string lpFileName)
{
    Assembly NowAssembly = Assembly.GetEntryAssembly(); 
    Stream fs = null;

    try
    {
        // 尝试读取资源中的 DLL      
        fs = NowAssembly.GetManifestResourceStream(NowAssembly.GetName().Name + "." + lpFileName);
    }
    finally
    {
        // 如果资源没有所需的 DLL ，就查看硬盘上有没有，有的话就读取      
        if (fs == null && !File.Exists(lpFileName))
            throw (new Exception(" 找不到文件 :" + lpFileName));
        else if (fs == null && File.Exists(lpFileName))
        { 
            FileStream Fs = new FileStream(lpFileName, FileMode.Open); 
            fs = (Stream)Fs; 
        }
    }
    byte[] buffer = new byte[(int)fs.Length]; 
    fs.Read(buffer, 0, buffer.Length); 
    fs.Close(); 
    return buffer; // 以 byte[] 返回读到的 DLL
}
```

​	添加UnLoadDll方法来卸载DLL：   

```c#
public void UnLoadDll()
{
    // 使 MyAssembly 指空
    MyAssembly = null;
}
```

​	添加Invoke方法来进行对DLL中方法的调用，其原理大体上和“Form1．cs”中的方法Invoke相同，不过这里用的是“Assembly.Load”，而且用了静态变量MyAssembly来保存已加载的DLL，如果已加载的话就不再加载，如果还没加载或者已加载的不同现在要加载的DLL就进行加载，其代码如下所示：    

```c#
public object Invoke(string lpFileName, string Namespace, string ClassName, string lpProcName, object[] ObjArray_Parameter)
{
    try
    {
        // 判断 MyAssembly 是否为空或 MyAssembly 的命名空间不等于要调用方法的命名空间，
        // 如果条件为真，就用 Assembly.Load 加载所需 DLL 作为程序集
        if(MyAssembly==null||MyAssembly.GetName().Name!=Namespace)       
            MyAssembly=Assembly.Load(LoadDll(lpFileName));       
        Type[] type = MyAssembly.GetTypes();
        foreach (Type t in type)
        {
            if (t.Namespace == Namespace && t.Name == ClassName)
            {
                MethodInfo m = t.GetMethod(lpProcName);
                if (m != null)
                {// 调用并返回       
                    object o = Activator.CreateInstance(t);
                    return m.Invoke(o, ObjArray_Parameter);
                }
                else
                    System.Windows.Forms.MessageBox.Show(" 装载出错 !");
            }
        }
    }
    catch (System.NullReferenceException e) { System.Windows.Forms.MessageBox.Show(e.Message); }
    return (object)0;
}
```

  

**2)    ldfs类的使用：**   

1． 把CsCount.dll作为“嵌入的资源”添加到项目“Tzb”中。   

2． 向“Form1”窗体中添加两个按钮，Name和Text属性分别为“B7”、“ldfs.Invoke调用 count”；“B8”、“UnLoadDll”，并将它们调整到适当大小和位置。   

3． 打开“Form1．cs”代码视图，添加一个ldfs实例：   

```c#
// 添加一个 ldfs 实例 tmp       
private ldfs tmp=new ldfs();      
```

4． 在“Form1．cs［设计］”视图中双击按钮B7，在“B1_Click”方法体内添加如下代码：   

```c#
// 调用 count(0), 并使用期提示框显示其返回值       
MessageBox.Show(" 这是您第"+tmp.Invoke("CsCount.dll",
"CsCount","Class1","count",new object[]{(int)0}).ToString()+
" 次点击此按钮。 "," 挑战杯 ");       
```

5． 在“Form1．cs［设计］”视图中双击按钮B7，在“B1_Click”方法体内添加如下代码：    

```c#
// 卸载 DLL       tmp.UnLoadDll();       
```

6． “F5”运行该程序，并先点击按钮B7三次，接着点击按钮B8，最后再点击按钮B7，此时发现又开始重新计数了，情况和“dld类的使用”类似，也就是也实现了DLL的动态装载与卸载了。       

**三、   结论**   

​	使用DLL有很多优点，如：节省内存和减少交换操作；开发大型程序时可以把某些模块分配给程序员，程序员可以用任何一门他所熟悉的语言把该模块编译成DLL文件，这样可以提高代码的复用，大大减轻程序员的工作量。当然DLL也有一些不足，如在提要中提及的问题。所以，如何灵活地调用DLL应该是每位程序员所熟知的。   

​	C# 语言有很多优点，越来越多的人开始使用它来编程。但是，C#还有一些不足，如对不少的底层操作是无能为力的，只能通过调用Win32 DLL 或C++等编写的DLL；另外，一般认为C#程序的保密性不够强，因为它容易被Reflector 反编译而得到部分源码，所以需要使用混合编程加强C#程序的保密性，而把DLL嵌入C#程序并实现动态调用的方法是比较理想的方法，因为可以把DLL文件先用某一算法进行加密甚至压缩后再作为资源文件添加到C#程序中，在程序运行时才用某一算法进行解压解密后才进行加载，所以即使用反编译软件，也只能得到一个资源文件，且这个资源文件是用一个复杂算法进行加密过的，不可能再次对资源文件中的内容进行反编译，从而大大加强了代码的保密性。