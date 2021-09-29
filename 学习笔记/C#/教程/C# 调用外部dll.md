# [C# 调用外部dll](http://www.cnblogs.com/kevin-top/archive/2010/06/04/1751425.html)

 

**一、**   **DLL****与应用程序**

动态链接库（也称为DLL，即为“Dynamic Link Library”的缩写）是Microsoft Windows最重要的组成要素之一，打开Windows系统文件夹，你会发现文件夹中有很多DLL文件，Windows就是将一些主要的系统功能以DLL模块的形式实现。

动态链接库是不能直接执行的，也不能接收消息，它只是一个独立的文件，其中包含能被程序或其它DLL调用来完成一定操作的函数(方法。注：C#中一般称为“方法”)，但这些函数不是执行程序本身的一部分，而是根据进程的需要按需载入，此时才能发挥作用。

DLL只有在应用程序需要时才被系统加载到进程的虚拟空间中，成为调用进程的一部分，此时该DLL也只能被该进程的线程访问，它的句柄可以被调用进程所使用，而调用进程的句柄也可以被该DLL所使用。在内存中，一个DLL只有一个实例，且它的编制与具体的编程语言和编译器都没有关系，所以可以通过DLL来实现混合语言编程。DLL函数中的代码所创建的任何对象（包括变量）都归调用它的线程或进程所有。

下面列出了当程序使用 DLL 时提供的一些优点：[1]

**1)**    **使用较少的资源**

当多个程序使用同一个函数库时，DLL 可以减少在磁盘和物理内存中加载的代码的重复量。这不仅可以大大影响在前台运行的程序，而且可以大大影响其他在 Windows 操作系统上运行的程序。

**2)**    **推广模块式体系结构**

DLL 有助于促进模块式程序的开发。这可以帮助您开发要求提供多个语言版本的大型程序或要求具有模块式体系结构的程序。模块式程序的一个示例是具有多个可以在运行时动态加载的模块的计帐程序。

**3)**    **简化部署和安装**

当 DLL 中的函数需要更新或修复时，部署和安装 DLL 不要求重新建立程序与该 DLL 的链接。此外，如果多个程序使用同一个 DLL，那么多个程序都将从该更新或修复中获益。当您使用定期更新或修复的第三方 DLL 时，此问题可能会更频繁地出现。

**二、**   **DLL****的调用**

每种编程语言调用DLL的方法都不尽相同，在此只对用C#调用DLL的方法进行介绍。首先,您需要了解什么是托管,什么是非托管。一般可以认为：非托管代码主要是基于win 32平台开发的DLL，activeX的组件，托管代码是基于.net平台开发的。如果您想深入了解托管与非托管的关系与区别，及它们的运行机制，请您自行查找资料，本文件在此不作讨论。

**(一)**   **调用****DLL****中的非托管函数一般方法**

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

[DllImport("user32.dll", EntryPoint="MessageBoxA")]

static extern int MsgBox(int hWnd, string msg, string caption, int type);

其它可选的 DllImportAttribute 属性：

CharSet 指示用在入口点中的字符集，如：CharSet=CharSet.Ansi；

SetLastError 指示方法是否保留 Win32"上一错误"，如：SetLastError=true；

ExactSpelling 指示 EntryPoint 是否必须与指示的入口点的拼写完全匹配，如：ExactSpelling=false；

PreserveSig指示方法的签名应当被保留还是被转换， 如：PreserveSig=true；

CallingConvention指示入口点的调用约定， 如：CallingConvention=CallingConvention.Winapi；

 

此外，关于“数据封送处理”及“封送数字和逻辑标量”请参阅其它一些文章[2]。

**C#****例子：**

\1.    启动VS.NET，新建一个项目，项目名称为“Tzb”，模板为“Windows 应用程序”。

\2.    在“工具箱”的“ Windows 窗体”项中双击“Button”项，向“Form1”窗体中添加一个按钮。

\3.    改变按钮的属性：Name为 “B1”，Text为 “用DllImport调用DLL弹出提示框”，并将按钮B1调整到适当大小，移到适当位置。

\4.    在类视图中双击“Form1”，打开“Form1．cs”代码视图，在“namespace Tzb”上面输入“using System.Runtime.InteropServices;”，以导入该命名空间。

\5.    在“Form1．cs［设计］”视图中双击按钮B1，在“B1_Click”方法上面使用关键字 static 和 extern 声明方法“MsgBox”，将 DllImport 属性附加到该方法，这里我们要使用的是“user32．dll”中的“MessageBoxA”函数，具体代码如下：

 

[DllImport("user32.dll", EntryPoint="MessageBoxA")]static extern int MsgBox(int hWnd, string msg, string caption, int type);

 

然后在“B1_Click”方法体内添加如下代码，以调用方法“MsgBox”：

 

MsgBox(0," 这就是用 DllImport 调用 DLL 弹出的提示框哦！ "," 挑战杯 ",0x30);

 

 

\6.    按“F5”运行该程序，并点击按钮B1，便弹出如下提示框：

![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image005.gif)

 

**(二)**   **动态装载、调用****DLL****中的非托管函数**

在上面已经说明了如何用DllImport调用DLL中的非托管函数，但是这个是全局的函数，假若DLL中的非托管函数有一个静态变量S，每次调用这个函数的时候，静态变量S就自动加1。结果，当需要重新计数时，就不能得出想要的结果。下面将用例子说明：

**1.**    **DLL****的创建**

**1)**    启动Visual C++ 6.0；

**2)**    新建一个“Win32 Dynamic-Link Library”工程，工程名称为“Count”；

**3)**    在“Dll kind”选择界面中选择“A simple dll project”；

**4)**    打开Count.cpp，添加如下代码：

 

// 导出函数，使用“ _stdcall ” 标准调用extern "C" _declspec(dllexport)int _stdcall count(int init);

int _stdcall count(int init){//count 函数，使用参数 init 初始化静态的整形变量 S ，并使 S 自加 1 后返回该值static int S=init;S++;return S;}

 

**5)**    按“F7”进行编译，得到Count.dll（在工程目录下的Debug文件夹中）。

 

**2.**     **用DllImport调用DLL中的count函数**

**1)**    打开项目“Tzb”，向“Form1”窗体中添加一个按钮。

**2)**    改变按钮的属性：Name为 “B2”，Text为 “用DllImport调用DLL中count函数”，并将按钮B1调整到适当大小，移到适当位置。

**3)**    打开“Form1．cs”代码视图，使用关键字 static 和 extern 声明方法“count”，并使其具有来自 Count.dll 的导出函数count的实现，代码如下：

 

 

[DllImport("Count.dll")]static extern int count(int init);

 

**4)**    在“Form1．cs［设计］”视图中双击按钮B2，在“B2_Click”方法体内添加如下代码：

 

MessageBox.Show(" 用 DllImport 调用 DLL 中的 count 函数， \n 传入的实参为 0 ，得到的结果是： "+count(0).ToString()," 挑战杯 ");MessageBox.Show(" 用 DllImport 调用 DLL 中的 count 函数， \n 传入的实参为 10 ，得到的结果是： "+count(10).ToString()+"\n 结果可不是想要的 11 哦！！！ "," 挑战杯 ");MessageBox.Show(" 所得结果表明： \n 用 DllImport 调用 DLL 中的非托管 \n 函数是全局的、静态的函数！！！ "," 挑战杯 ");

 

 

**5)**    把Count.dll复制到项目“Tzb”的bin\Debug文件夹中，按“F5”运行该程序，并点击按钮B2，便弹出如下三个提示框：

 

![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image013.gif) ![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image014.gif) ![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image015.gif)

第1个提示框显示的是调用“count(0)”的结果，第2个提示框显示的是调用“count(10)”的结果，由所得结果可以证明“用DllImport调用DLL中的非托管函数是全局的、静态的函数”。所以，有时候并不能达到我们目的，因此我们需要使用下面所介绍的方法：C#动态调用DLL中的函数。

 

  

**3.**    **C#****动态调用****DLL****中的函数**

因为C#中使用DllImport是不能像动态load/unload assembly那样，所以只能借助API函数了。在kernel32.dll中，与动态库调用有关的函数包括[3]：

①LoadLibrary（或MFC 的AfxLoadLibrary），装载动态库。

②GetProcAddress，获取要引入的函数，将符号名或标识号转换为DLL内部地址。

③FreeLibrary（或MFC的AfxFreeLibrary），释放动态链接库。

它们的原型分别是：

HMODULE LoadLibrary(LPCTSTR lpFileName);

FARPROC GetProcAddress(HMODULE hModule, LPCWSTR lpProcName);

BOOL FreeLibrary(HMODULE hModule);

 

现在，我们可以用IntPtr hModule=LoadLibrary(“Count.dll”);来获得Dll的句柄,用IntPtr farProc=GetProcAddress(hModule,”_count@4”);来获得函数的入口地址。

但是，知道函数的入口地址后，怎样调用这个函数呢？因为在C#中是没有函数指针的，没有像C++那样的函数指针调用方式来调用函数，所以我们得借助其它方法。经过研究，发现我们可以通过结合使用System.Reflection.Emit及System.Reflection.Assembly里的类和函数达到我们的目的。为了以后使用方便及实现代码的复用，我们可以编写一个类。

**1)**    **dld****类的编写：**

\1.    打开项目“Tzb”，打开类视图，右击“Tzb”，选择“添加”-->“类”，类名设置为“dld”，即dynamic loading dll 的每个单词的开头字母。

\2.    添加所需的命名空间及声明参数传递方式枚举：

 

using System.Runtime.InteropServices; // 用 DllImport 需用此 命名空间using System.Reflection; // 使用 Assembly 类需用此 命名空间using System.Reflection.Emit; // 使用 ILGenerator 需用此 命名空间

 

​     在“public class dld”上面添加如下代码声明参数传递方式枚举：

 

/// <summary>/// 参数传递方式枚举 ,ByValue 表示值传递 ,ByRef 表示址传递/// </summary>public enum ModePass{ByValue = 0x0001,ByRef = 0x0002}

 

 

\3.    声明LoadLibrary、GetProcAddress、FreeLibrary及私有变量hModule和farProc：

 

/// <summary>/// 原型是 :HMODULE LoadLibrary(LPCTSTR lpFileName);/// </summary>/// <param name="lpFileName">DLL 文件名 </param>/// <returns> 函数库模块的句柄 </returns>[DllImport("kernel32.dll")]static extern IntPtr LoadLibrary(string lpFileName);/// <summary>/// 原型是 : FARPROC GetProcAddress(HMODULE hModule, LPCWSTR lpProcName);/// </summary>/// <param name="hModule"> 包含需调用函数的函数库模块的句柄 </param>/// <param name="lpProcName"> 调用函数的名称 </param>/// <returns> 函数指针 </returns>[DllImport("kernel32.dll")]static extern IntPtr GetProcAddress(IntPtr hModule, string lpProcName);/// <summary>/// 原型是 : BOOL FreeLibrary(HMODULE hModule);/// </summary>/// <param name="hModule"> 需释放的函数库模块的句柄 </param>/// <returns> 是否已释放指定的 Dll</returns>[DllImport("kernel32",EntryPoint="FreeLibrary",SetLastError=true)]static extern bool FreeLibrary(IntPtr hModule);/// <summary>/// Loadlibrary 返回的函数库模块的句柄/// </summary>private IntPtr hModule=IntPtr.Zero;/// <summary>/// GetProcAddress 返回的函数指针/// </summary>private IntPtr farProc=IntPtr.Zero;

 

 

\4.    添加LoadDll方法，并为了调用时方便，重载了这个方法：

 

 

/// <summary>/// 装载 Dll/// </summary>/// <param name="lpFileName">DLL 文件名 </param>public void LoadDll(string lpFileName){hModule=LoadLibrary(lpFileName);if(hModule==IntPtr.Zero)throw(new Exception(" 没有找到 :"+lpFileName+"." ));}

 

 

​     若已有已装载Dll的句柄，可以使用LoadDll方法的第二个版本：

 

public void LoadDll(IntPtr HMODULE){if(HMODULE==IntPtr.Zero)throw(new Exception(" 所传入的函数库模块的句柄 HMODULE 为空 ." ));hModule=HMODULE;}

 

 

\5.    添加LoadFun方法，并为了调用时方便，也重载了这个方法，方法的具体代码及注释如下：

 

/// <summary>/// 获得函数指针/// </summary>/// <param name="lpProcName"> 调用函数的名称 </param>public void LoadFun(string lpProcName){ // 若函数库模块的句柄为空，则抛出异常if(hModule==IntPtr.Zero)throw(new Exception(" 函数库模块的句柄为空 , 请确保已进行 LoadDll 操作 !"));// 取得函数指针farProc = GetProcAddress(hModule,lpProcName);// 若函数指针，则抛出异常if(farProc==IntPtr.Zero)throw(new Exception(" 没有找到 :"+lpProcName+" 这个函数的入口点 "));}/// <summary>/// 获得函数指针/// </summary>/// <param name="lpFileName"> 包含需调用函数的 DLL 文件名 </param>/// <param name="lpProcName"> 调用函数的名称 </param>public void LoadFun(string lpFileName,string lpProcName){ // 取得函数库模块的句柄hModule=LoadLibrary(lpFileName);// 若函数库模块的句柄为空，则抛出异常if(hModule==IntPtr.Zero)throw(new Exception(" 没有找到 :"+lpFileName+"." ));// 取得函数指针farProc = GetProcAddress(hModule,lpProcName);// 若函数指针，则抛出异常if(farProc==IntPtr.Zero)throw(new Exception(" 没有找到 :"+lpProcName+" 这个函数的入口点 "));}

 

 

\6.    添加UnLoadDll及Invoke方法，Invoke方法也进行了重载：

 

/// <summary>/// 卸载 Dll/// </summary>public void UnLoadDll(){FreeLibrary(hModule);hModule=IntPtr.Zero;farProc=IntPtr.Zero;}

 

 

​     Invoke方法的第一个版本：

 

/// <summary>/// 调用所设定的函数/// </summary>/// <param name="ObjArray_Parameter"> 实参 </param>/// <param name="TypeArray_ParameterType"> 实参类型 </param>/// <param name="ModePassArray_Parameter"> 实参传送方式 </param>/// <param name="Type_Return"> 返回类型 </param>/// <returns> 返回所调用函数的 object</returns>public object Invoke(object[] ObjArray_Parameter,Type[] TypeArray_ParameterType,ModePass[] ModePassArray_Parameter,Type Type_Return){// 下面 3 个 if 是进行安全检查 , 若不能通过 , 则抛出异常if(hModule==IntPtr.Zero)throw(new Exception(" 函数库模块的句柄为空 , 请确保已进行 LoadDll 操作 !"));if(farProc==IntPtr.Zero)throw(new Exception(" 函数指针为空 , 请确保已进行 LoadFun 操作 !" ) );if(ObjArray_Parameter.Length!=ModePassArray_Parameter.Length)throw(new Exception(" 参数个数及其传递方式的个数不匹配 ." ) );// 下面是创建 MyAssemblyName 对象并设置其 Name 属性AssemblyName MyAssemblyName = new AssemblyName();MyAssemblyName.Name = "InvokeFun";// 生成单模块配件AssemblyBuilder MyAssemblyBuilder =AppDomain.CurrentDomain.DefineDynamicAssembly(MyAssemblyName,AssemblyBuilderAccess.Run);ModuleBuilder MyModuleBuilder =MyAssemblyBuilder.DefineDynamicModule("InvokeDll");// 定义要调用的方法 , 方法名为“ MyFun ”，返回类型是“ Type_Return ”参数类型是“ TypeArray_ParameterType ”MethodBuilder MyMethodBuilder =MyModuleBuilder.DefineGlobalMethod("MyFun",MethodAttributes.Public| MethodAttributes.Static,Type_Return,TypeArray_ParameterType);// 获取一个 ILGenerator ，用于发送所需的 ILILGenerator IL = MyMethodBuilder.GetILGenerator();int i;for (i = 0; i < ObjArray_Parameter.Length; i++){// 用循环将参数依次压入堆栈switch (ModePassArray_Parameter[i]){case ModePass.ByValue:IL.Emit(OpCodes.Ldarg, i);break;case ModePass.ByRef:IL.Emit(OpCodes.Ldarga, i);break;default:throw(new Exception(" 第 " +(i+1).ToString() + " 个参数没有给定正确的传递方式 ." ) );}}if (IntPtr.Size == 4) {// 判断处理器类型IL.Emit(OpCodes.Ldc_I4, farProc.ToInt32());}else if (IntPtr.Size == 8){IL.Emit(OpCodes.Ldc_I8, farProc.ToInt64());}else{throw new PlatformNotSupportedException();}IL.EmitCalli(OpCodes.Calli,CallingConvention.StdCall,Type_Return,TypeArray_ParameterType);IL.Emit(OpCodes.Ret); // 返回值MyModuleBuilder.CreateGlobalFunctions();// 取得方法信息MethodInfo MyMethodInfo = MyModuleBuilder.GetMethod("MyFun");return MyMethodInfo.Invoke(null, ObjArray_Parameter);// 调用方法，并返回其值}

 

 

​     Invoke方法的第二个版本，它是调用了第一个版本的：

 

/// <summary>/// 调用所设定的函数/// </summary>/// <param name="IntPtr_Function"> 函数指针 </param>/// <param name="ObjArray_Parameter"> 实参 </param>/// <param name="TypeArray_ParameterType"> 实参类型 </param>/// <param name="ModePassArray_Parameter"> 实参传送方式 </param>/// <param name="Type_Return"> 返回类型 </param>/// <returns> 返回所调用函数的 object</returns>public object Invoke(IntPtr IntPtr_Function,object[] ObjArray_Parameter,Type[] TypeArray_ParameterType,ModePass[] ModePassArray_Parameter,Type Type_Return){// 下面 2 个 if 是进行安全检查 , 若不能通过 , 则抛出异常if(hModule==IntPtr.Zero)throw(new Exception(" 函数库模块的句柄为空 , 请确保已进行 LoadDll 操作 !"));if(IntPtr_Function==IntPtr.Zero)throw(new Exception(" 函数指针 IntPtr_Function 为空 !" ) );farProc=IntPtr_Function;return Invoke(ObjArray_Parameter,TypeArray_ParameterType,ModePassArray_Parameter,Type_Return);}

 

 

 

**2)**    **dld****类的使用：**

1． 打开项目“Tzb”，向“Form1”窗体中添加三个按钮。Name 和Text属性分别为 “B3”、“用LoadLibrary方法装载Count.dll”，“B4”、“调用count方法”，“B5”、“卸载Count.dll”，并调整到适当的大小及位置。

2． 在“Form1．cs［设计］”视图中双击按钮B3，在“B3_Click”方法体上面添加代码，创建一个dld类实例：

 

/// <summary>/// 创建一个 dld 类对象/// </summary>private dld myfun=new dld();

 

 

 3． 在“B3_Click”方法体内添加如下代码：

 

myfun.LoadDll("Count.dll"); // 加载 "Count.dll"myfun.LoadFun("_count@4"); // 调入函数 count, "_count@4" 是它的入口，可通过 Depends 查看

 

 

4． “Form1．cs［设计］”视图中双击按钮B4，在“B4_Click”方法体内添加如下代码：

 

object[] Parameters = new object[]{(int)0}; // 实参为 0Type[] ParameterTypes = new Type[]{typeof(int)}; // 实参类型为 intModePass[] themode=new ModePass[]{ModePass.ByValue}; // 传送方式为值传Type Type_Return = typeof(int); // 返回类型为 int// 弹出提示框，显示调用 myfun.Invoke 方法的结果，即调用 count 函数MessageBox.Show(" 这是您装载该 Dll 后第 "+myfun.Invoke(Parameters,ParameterTypes,themode,Type_Return).ToString()+" 次点击此按钮。 "," 挑战杯 ");

 

 


 

5． “Form1．cs［设计］”视图中双击按钮B5，在“B5_Click”方法体内添加如下代码：

 

myfun.UnLoadDll();

 

6． 按“F5”运行该程序，并先点击按钮B3以加载“Count.dll”，接着点击按钮B4三次以调用3次“count(0)”，先后弹出的提示框如下：

![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image045.gif)![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image046.gif)![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image047.gif)

​     这三个提示框所得出的结果说明了静态变量S 经初始化后，再传入实参“0”也不会改变其值为“0”。

7． 点击按钮B5以卸载“Count.dll”，再点击按钮B3进行装载“Count.dll”，再点击按钮B4查看调用了“count(0)”的结果：

![img](http://blog.csdn.net/images/blog_csdn_net/pansiom/image048.gif)

从弹出的提示框所显示的结果可以看到又开始重新计数了，也就是实现了DLL的动态装载与卸载了。

 

**(三)   \**调用托管\*\*DLL\*\*一般方法\*\*\*\*\****

C# 调用托管DLL是很简单的，只要在“解决方案资源管理器”中的需要调用DLL的项目下用鼠标右击“引用”，并选择“添加引用”，然后选择已列出的DLL或通过浏览来选择DLL文件，最后需要用using 导入相关的命名空间。

**(四)   \**动态调用托管\*\*DLL\*\**\***

C# 动态调用托管DLL也需要借助System.Reflection.Assembly里的类和方法，主要使用了Assembly.LoadFrom。现在，用例子说明：

   首先，启动VS.NET，新建一个Visual C# 项目，使用的模板为“类库”，名称为“CsCount”，并在类“Class1”中添加静态整型变量S及方法count：

 

// 由于 static 不能修饰方法体内的变量，所以需放在这里，且初始化值为 int.MinValuestatic int S=int.MinValue;public int count(int init){// 判断 S 是否等于 int.MinValue ，是的话把 init 赋值给 Sif(S==int.MinValue) S=init;S++; //S 自增 1return S; // 返回 S}

 

 

然后，打开项目“Tzb”，向“Form1”窗体中添加一个按钮，Name属性为“B6”，Text属性为“用Assembly类来动态调用托管DLL”，调整到适当大小和位置，双击按钮B6，转入代码视图，先导入命名空间：using System.Reflection; 接着添加Invoke方法和B6_Click方法代码：

 

private object Invoke(string lpFileName,string Namespace,string ClassName,string lpProcName,object[] ObjArray_Parameter){Try { // 载入程序集Assembly MyAssembly=Assembly.LoadFrom(lpFileName);Type[] type=MyAssembly.GetTypes();foreach(Type t in type){// 查找要调用的命名空间及类if(t.Namespace==Namespace&&t.Name==ClassName){// 查找要调用的方法并进行调用MethodInfo m=t.GetMethod(lpProcName);if(m!=null){object o=Activator.CreateInstance(t);return m.Invoke(o,ObjArray_Parameter);}else MessageBox.Show(" 装载出错 !");}}}//trycatch(System.NullReferenceException e){MessageBox.Show(e.Message);}//catchreturn (object)0;}// Invoke

 

 

“B6_Click”方法体内代码如下：

 

// 显示 count(0) 返回的值MessageBox.Show(" 这是您第 "+Invoke("CsCount.dll","CsCount","Class1","count",new object[]{(int)0}).ToString()+" 次点击此按钮。 "," 挑战杯 ");

 

 

最后，把项目“CsCount”的bin\Debug文件夹中的CsCount.dll复制到项目“Tzb”的bin\Debug文件夹中，按“F5”运行该程序，并点击按钮B6三次，将会弹出3个提示框，内容分别是“这是您第 1次点击此按钮。”、“这是您第 2次点击此按钮。”、“这是您第 3次点击此按钮。”，由此知道了静态变量S在这里的作用。

 

**(\**五\*\*) C#\*\*程序\*\*嵌入\*\*\*\*DLL\*\*\*\*的调用\*\*\*\*\*\**\***

   DLL文件作为资源嵌入在C#程序中，我们只要读取该资源文件并以“byte[]”返回，然后就用“Assembly Load(byte[]);”得到DLL中的程序集，最后就可以像上面的Invoke方法那样对DLL中的方法进行调用。当然不用上面方法也可以，如用接口实现动态调用，但DLL中必须有该接口的定义并且程序中也要有该接口的定义；也可用反射发送实现动态调用[4]。现在我只对像上面的Invoke方法那样对DLL中的方法进行调用进行讨论，为了以后使用方便及实现代码的复用，我们可以结合上一个编写一个类。

**1)**    **ldfs****类的编写：**

在项目“Tzb”中新建一个名为ldfs的类，意为“load dll from resource”，请注意，在这个类中“resource”不只是嵌入在EXE程序中的资源，它也可以是硬盘上任意一个DLL文件，这是因为ldfs的类中的方法LoadDll有些特别，就是先从程序的内嵌的资源中查找需加载的DLL，如果找不到，就查找硬盘上的。

首先导入所需的命名空间：

 

using System.IO; // 对文件的读写需要用到此命名空间using System.Reflection; // 使用 Assembly 类需用此命名空间using System.Reflection.Emit; // 使用 ILGenerator 需用此命名空间

 

声明一静态变量MyAssembly：

 

// 记录要导入的程序集static Assembly MyAssembly;

 

添加LoadDll方法：

 

private byte[] LoadDll(string lpFileName){Assembly NowAssembly = Assembly.GetEntryAssembly();Stream fs=null;try{// 尝试读取资源中的 DLLfs = NowAssembly.GetManifestResourceStream(NowAssembly.GetName().Name+"."+lpFileName);}finally{// 如果资源没有所需的 DLL ，就查看硬盘上有没有，有的话就读取if (fs==null&&!File.Exists(lpFileName)) throw(new Exception(" 找不到文件 :"+lpFileName));else if(fs==null&&File.Exists(lpFileName)){FileStream Fs = new FileStream(lpFileName, FileMode.Open);fs=(Stream)Fs;}}byte[] buffer = new byte[(int) fs.Length];fs.Read(buffer, 0, buffer.Length);fs.Close();return buffer; // 以 byte[] 返回读到的 DLL}

 

添加UnLoadDll方法来卸载DLL：

 

public void UnLoadDll(){// 使 MyAssembly 指空MyAssembly=null;}

 

添加Invoke方法来进行对DLL中方法的调用，其原理大体上和“Form1．cs”中的方法Invoke相同，不过这里用的是“Assembly.Load”，而且用了静态变量MyAssembly来保存已加载的DLL，如果已加载的话就不再加载，如果还没加载或者已加载的不同现在要加载的DLL就进行加载，其代码如下所示：

 

public object Invoke(string lpFileName,string Namespace,string ClassName,string lpProcName,object[] ObjArray_Parameter){try{// 判断 MyAssembly 是否为空或 MyAssembly 的命名空间不等于要调用方法的命名空间，如果条件为真，就用 Assembly.Load 加载所需 DLL 作为程序集if(MyAssembly==null||MyAssembly.GetName().Name!=Namespace)MyAssembly=Assembly.Load(LoadDll(lpFileName));Type[] type=MyAssembly.GetTypes();foreach(Type t in type){if(t.Namespace==Namespace&&t.Name==ClassName){MethodInfo m=t.GetMethod(lpProcName);if(m!=null){// 调用并返回object o=Activator.CreateInstance(t);return m.Invoke(o,ObjArray_Parameter);}elseSystem.Windows.Forms.MessageBox.Show(" 装载出错 !");}}}catch(System.NullReferenceException e){System.Windows.Forms.MessageBox.Show(e.Message);}return (object)0;}

 

 

 

**2)**    **ldfs****类的使用：**

1． 把CsCount.dll作为“嵌入的资源”添加到项目“Tzb”中。

2． 向“Form1”窗体中添加两个按钮，Name和Text属性分别为“B7”、“ldfs.Invoke调用count”；“B8”、“UnLoadDll”，并将它们调整到适当大小和位置。

3． 打开“Form1．cs”代码视图，添加一个ldfs实例：

 

// 添加一个 ldfs 实例 tmpprivate ldfs tmp=new ldfs();

 

4． 在“Form1．cs［设计］”视图中双击按钮B7，在“B1_Click”方法体内添加如下代码：

 

// 调用 count(0), 并使用期提示框显示其返回值MessageBox.Show(" 这是您第 "+tmp.Invoke("CsCount.dll","CsCount","Class1","count",new object[]{(int)0}).ToString()+" 次点击此按钮。 "," 挑战杯 ");

 

5． 在“Form1．cs［设计］”视图中双击按钮B7，在“B1_Click”方法体内添加如下代码：

 

// 卸载 DLLtmp.UnLoadDll();

 

6． “F5”运行该程序，并先点击按钮B7三次，接着点击按钮B8，最后再点击按钮B7，此时发现又开始重新计数了，情况和“dld类的使用”类似，也就是也实现了DLL的动态装载与卸载了。

  说明：以上所用到的所有源代码详见附件1:Form1.cs、附件2:dld.cs、附件3:ldfs.cs、附件4:Count.cpp、附件5:Class1.cs。

 

**三、**   **结****论**

使用DLL有很多优点，如：节省内存和减少交换操作；开发大型程序时可以把某些模块分配给程序员，程序员可以用任何一门他所熟悉的语言把该模块编译成DLL文件，这样可以提高代码的复用，大大减轻程序员的工作量。当然DLL也有一些不足，如在提要中提及的问题。所以，如何灵活地调用DLL应该是每位程序员所熟知的。

C# 语言有很多优点，越来越多的人开始使用它来编程。但是，C#还有一些不足，如对不少的底层操作是无能为力的，只能通过调用Win32 DLL 或C++等编写的DLL；另外，一般认为C#程序的保密性不够强，因为它容易被Reflector 反编译而得到部分源码，所以需要使用混合编程加强C#程序的保密性，而把DLL嵌入C#程序并实现动态调用的方法是比较理想的方法，因为可以把DLL文件先用某一算法进行加密甚至压缩后再作为资源文件添加到C#程序中，在程序运行时才用某一算法进行解压解密后才进行加载，所以即使用反编译软件，也只能得到一个资源文件，且这个资源文件是用一个复杂算法进行加密过的，不可能再次对资源文件中的内容进行反编译，从而大大加强了代码的保密性。

[好文要顶](javascript:void(0);)         [关注我](javascript:void(0);)     [收藏该文](javascript:void(0);)     [![img](https://common.cnblogs.com/images/icon_weibo_24.png)](javascript:void(0);)     [![img](https://common.cnblogs.com/images/wechat.png)](javascript:void(0);) 

[![img](https://pic.cnblogs.com/face/sample_face.gif)](https://home.cnblogs.com/u/lhyqzx/)

[_ali](https://home.cnblogs.com/u/lhyqzx/)
[关注 - 7](https://home.cnblogs.com/u/lhyqzx/followees/)
[粉丝 - 4](https://home.cnblogs.com/u/lhyqzx/followers/)         

[+加关注](javascript:void(0);)     

5     

0     

[« ](https://www.cnblogs.com/lhyqzx/p/5962826.html) 上一篇：    [基于RBAC的权限设计模型](https://www.cnblogs.com/lhyqzx/p/5962826.html)     
[» ](https://www.cnblogs.com/lhyqzx/p/5972057.html) 下一篇：    [SQL Select结果增加自增自段（网转）](https://www.cnblogs.com/lhyqzx/p/5972057.html) 

​            posted on  2016-10-17 16:56 [_ali](https://www.cnblogs.com/lhyqzx/) 阅读(79651) 评论(2) [编辑](https://i.cnblogs.com/EditPosts.aspx?postid=5970406) [收藏](javascript:void(0)) [举报](javascript:void(0))         





[刷新评论](javascript:void(0);)[刷新页面](#)[返回顶部](#top)

​    登录后才能查看或发表评论，立即 [登录](javascript:void(0);) 或者     [逛逛](https://www.cnblogs.com/) 博客园首页 

[【推荐】百度智能云超值优惠：新用户首购云服务器1核1G低至69元/年](https://cloud.baidu.com/campaign/bccdiscount/index.html?track=cp:bokeyuan|pf:pc|pp:H-bokeyuan-21bccsanqi-neiyewenzilian|pu:21bccsanqi-neiyewenzilian|ci:21bcc|kw:10370321)
[【推荐】跨平台组态\工控\仿真\CAD 50万行C++源码全开放免费下载！](http://www.uccpsoft.com/index.htm)
[【推荐】阿里云云大使特惠：新用户购ECS服务器1核2G最低价87元/年](https://www.aliyun.com/minisite/goods?taskCode=yds2021-09zy&recordId=883117&userCode=swh7dvlt)
[【推荐】和开发者在一起：华为开发者社区，入驻博客园科技品牌专区](https://brands.cnblogs.com/huawei)
[【推广】园子与爱卡汽车爱宝险合作，随手就可以买一份的百万医疗保险](https://www.cnblogs.com/cmt/p/15147135.html)

[![img](https://img2020.cnblogs.com/blog/35695/202108/35695-20210817131346802-930113090.jpg)              ](https://c.gridsumdissector.com/r/?gid=gad_545_ph4hkwzt&ck=32&adk=442&autorefresh=__AUTOREFRESH__)

**编辑推荐：** 
· [理解ASP.NET Core - Host](https://www.cnblogs.com/xiaoxiaotank/p/15273093.html)     
· [妙用 background 实现花式文字效果](https://www.cnblogs.com/coco1s/p/15292365.html)     
· [Go 并发编程 -- 正确使用 goroutine](https://www.cnblogs.com/failymao/p/15270302.html)     
· [前端瓦片地图加载之塞尔达传说旷野之息](https://www.cnblogs.com/dragonir/p/15265372.html)     
· [技术管理进阶 —— 关于成本优化与利益分配机制](https://www.cnblogs.com/yexiaochai/p/15260657.html)     

**最新新闻**：     
 ·          [「爱因斯坦」变网红？这个仿生机器人连老年斑都不放过（2021-09-18 15:52）](http://news.cnblogs.com/n/702545/)         
 ·          [波士顿动力机器狗再进化：已学会自主规划路线（2021-09-18 15:32）](http://news.cnblogs.com/n/702544/)         
 ·          [当AI触角伸向教育和医疗，背后在下一步大棋！（2021-09-18 15:21）](http://news.cnblogs.com/n/702543/)         
 ·          [苹果官网崩了！iPhone13预售秒光，跑分甩安卓旗舰整整一代（2021-09-18 15:13）](http://news.cnblogs.com/n/702542/)         
 ·          [华为下矿不挖煤，鸿蒙搭台不唱戏（2021-09-18 15:00）](http://news.cnblogs.com/n/702541/)         
    » [更多新闻...](https://news.cnblogs.com/) 

### 导航

- [博客园](https://www.cnblogs.com/)     
- [首页](https://www.cnblogs.com/lhyqzx/)     
- [新随笔](https://i.cnblogs.com/EditPosts.aspx?opt=1)     
- [联系](https://msg.cnblogs.com/send/_ali)     
- [订阅](javascript:void(0))         [![订阅](/skins/anothereon001/images/xml.gif) ](https://www.cnblogs.com/lhyqzx/rss/)
- [管理](https://i.cnblogs.com/)     

| [<](javascript:void(0);)                             2021年9月[>](javascript:void(0);) |      |      |      |      |                                                              |      |
| ------------------------------------------------------------ | ---- | ---- | ---- | ---- | ------------------------------------------------------------ | ---- |
| 日                                                           | 一   | 二   | 三   | 四   | 五                                                           | 六   |
| 29                                                           | 30   | 31   | 1    | 2    | 3                                                            | 4    |
| 5                                                            | 6    | 7    | 8    | 9    | [10](https://www.cnblogs.com/lhyqzx/archive/2021/09/10.html) | 11   |
| 12                                                           | 13   | 14   | 15   | 16   | 17                                                           | 18   |
| 19                                                           | 20   | 21   | 22   | 23   | 24                                                           | 25   |
| 26                                                           | 27   | 28   | 29   | 30   | 1                                                            | 2    |
| 3                                                            | 4    | 5    | 6    | 7    | 8                                                            | 9    |

### 公告

​        昵称：         [            _ali         ](https://home.cnblogs.com/u/lhyqzx/)
​        园龄：         [            6年10个月         ](https://home.cnblogs.com/u/lhyqzx/)
​        粉丝：         [            4         ](https://home.cnblogs.com/u/lhyqzx/followers/)
​        关注：         [            7         ](https://home.cnblogs.com/u/lhyqzx/followees/)

[+加关注](javascript:void(0))

### 搜索

​             

​             

### 常用链接 

- [我的随笔](https://www.cnblogs.com/lhyqzx/p/)
- [我的评论](https://www.cnblogs.com/lhyqzx/MyComments.html)
- [我的参与](https://www.cnblogs.com/lhyqzx/OtherPosts.html)
- [最新评论](https://www.cnblogs.com/lhyqzx/RecentComments.html)
- [我的标签](https://www.cnblogs.com/lhyqzx/tag/)

### 我的标签

- [dev(14)](https://www.cnblogs.com/lhyqzx/tag/dev/)         
- [c#(4)](https://www.cnblogs.com/lhyqzx/tag/c%23/)         
- [sql(2)](https://www.cnblogs.com/lhyqzx/tag/sql/)         
- [wcf(2)](https://www.cnblogs.com/lhyqzx/tag/wcf/)         
- [net(1)](https://www.cnblogs.com/lhyqzx/tag/net/)         
- [iis(1)](https://www.cnblogs.com/lhyqzx/tag/iis/)         
- [debug error(1)](https://www.cnblogs.com/lhyqzx/tag/debug error/)         
- [多语言系统设计(1)](https://www.cnblogs.com/lhyqzx/tag/多语言系统设计/)         

###         随笔分类     

- [    c#(6) ](https://www.cnblogs.com/lhyqzx/category/954215.html)
- [    ibatisnet分章(1) ](https://www.cnblogs.com/lhyqzx/category/776419.html)
- [    sql(2) ](https://www.cnblogs.com/lhyqzx/category/999525.html)
- [    设计模式(1) ](https://www.cnblogs.com/lhyqzx/category/776422.html)

###         随笔档案     

- [    2021年9月(1) ](https://www.cnblogs.com/lhyqzx/archive/2021/09.html)
- [    2020年8月(1) ](https://www.cnblogs.com/lhyqzx/archive/2020/08.html)
- [    2019年10月(1) ](https://www.cnblogs.com/lhyqzx/archive/2019/10.html)
- [    2018年11月(1) ](https://www.cnblogs.com/lhyqzx/archive/2018/11.html)
- [    2018年10月(3) ](https://www.cnblogs.com/lhyqzx/archive/2018/10.html)
- [    2018年7月(1) ](https://www.cnblogs.com/lhyqzx/archive/2018/07.html)
- [    2018年6月(2) ](https://www.cnblogs.com/lhyqzx/archive/2018/06.html)
- [    2018年4月(1) ](https://www.cnblogs.com/lhyqzx/archive/2018/04.html)
- [    2017年11月(3) ](https://www.cnblogs.com/lhyqzx/archive/2017/11.html)
- [    2017年9月(2) ](https://www.cnblogs.com/lhyqzx/archive/2017/09.html)
- [    2017年7月(5) ](https://www.cnblogs.com/lhyqzx/archive/2017/07.html)
- [    2017年6月(8) ](https://www.cnblogs.com/lhyqzx/archive/2017/06.html)
- [    2017年5月(11) ](https://www.cnblogs.com/lhyqzx/archive/2017/05.html)
- [    2017年4月(1) ](https://www.cnblogs.com/lhyqzx/archive/2017/04.html)
- [    2017年3月(1) ](https://www.cnblogs.com/lhyqzx/archive/2017/03.html)
- [    2017年2月(2) ](https://www.cnblogs.com/lhyqzx/archive/2017/02.html)
- [    2016年11月(1) ](https://www.cnblogs.com/lhyqzx/archive/2016/11.html)
- [    2016年10月(8) ](https://www.cnblogs.com/lhyqzx/archive/2016/10.html)
- [    2016年9月(10) ](https://www.cnblogs.com/lhyqzx/archive/2016/09.html)
- [    2016年8月(1) ](https://www.cnblogs.com/lhyqzx/archive/2016/08.html)
- [    2016年7月(1) ](https://www.cnblogs.com/lhyqzx/archive/2016/07.html)
- [    2016年5月(2) ](https://www.cnblogs.com/lhyqzx/archive/2016/05.html)
- [    2016年4月(5) ](https://www.cnblogs.com/lhyqzx/archive/2016/04.html)
- [    2016年2月(1) ](https://www.cnblogs.com/lhyqzx/archive/2016/02.html)
- [    2016年1月(8) ](https://www.cnblogs.com/lhyqzx/archive/2016/01.html)
- [更多](javascript:void(0))

### 最新评论

- [1. Re:C#打印条码BarTender SDK打印之路和离开之路（web平凡之路）(转)](https://www.cnblogs.com/lhyqzx/p/11646974.html)
- 想问一下楼主这个bartender可以实现客户端调用服务端的模板吗
- --like1
- [2. Re:C# 调用外部dll（转）](https://www.cnblogs.com/lhyqzx/p/5970406.html)
- 赞👍！
- --暴走的锅巴
- [3. Re:RBAC用户权限管理数据库设计](https://www.cnblogs.com/lhyqzx/p/5962811.html)
- 写的很好啊，又是有具体的实现代码就更好了。
- --Umyng
- [4. Re:遍历datatable的方法](https://www.cnblogs.com/lhyqzx/p/5893022.html)
- 就知道复制 粘贴  请问这个方法有用吗？
- --dark_passion
- [5. Re:C# 调用外部dll（转）](https://www.cnblogs.com/lhyqzx/p/5970406.html)
- 学习了。
- --枫De忧殇

### 阅读排行榜

- [                            1. C# 调用外部dll（转）(79651)                         ](https://www.cnblogs.com/lhyqzx/p/5970406.html)
- [                            2. C#中的自定义控件中的属性、事件及一些相关特性的总结（转）(22093)                         ](https://www.cnblogs.com/lhyqzx/p/7003240.html)
- [                            3. 基于RBAC的权限设计模型(20541)                         ](https://www.cnblogs.com/lhyqzx/p/5962826.html)
- [                            4. DEV组件LookupEdit,ComboBoxEdit绑定数据源(14094)                         ](https://www.cnblogs.com/lhyqzx/p/5454028.html)
- [                            5. C#键盘事件处理(来源网上)(10878)                         ](https://www.cnblogs.com/lhyqzx/p/5955440.html)

### 评论排行榜

- [                            1. C# 调用外部dll（转）(2)                         ](https://www.cnblogs.com/lhyqzx/p/5970406.html)
- [                            2. C#打印条码BarTender SDK打印之路和离开之路（web平凡之路）(转)(1)                         ](https://www.cnblogs.com/lhyqzx/p/11646974.html)
- [                            3. RBAC用户权限管理数据库设计(1)                         ](https://www.cnblogs.com/lhyqzx/p/5962811.html)
- [                            4. 遍历datatable的方法(1)                         ](https://www.cnblogs.com/lhyqzx/p/5893022.html)

### 推荐排行榜

- [                                1. C# 调用外部dll（转）(5)                             ](https://www.cnblogs.com/lhyqzx/p/5970406.html)
- [                                2. C#中的自定义控件中的属性、事件及一些相关特性的总结（转）(2)                             ](https://www.cnblogs.com/lhyqzx/p/7003240.html)
- [                                3. SqlCommand对象-Transaction事务的使用(1)                             ](https://www.cnblogs.com/lhyqzx/p/6440959.html)
- [                                4. C#键盘事件处理(来源网上)(1)                             ](https://www.cnblogs.com/lhyqzx/p/5955440.html)
- [                                5. C#创建datatable （转）(1)                             ](https://www.cnblogs.com/lhyqzx/p/5894870.html)

​	Powered by: 	 
[博客园](https://www.cnblogs.com/)	 
​	Copyright © 2021 _ali 
Powered by .NET 6 on Kubernetes 