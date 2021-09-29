# C++命名规范

![\color{red}{常见命名法：}](https://math.jianshu.com/math?formula=%5Ccolor%7Bred%7D%7B%E5%B8%B8%E8%A7%81%E5%91%BD%E5%90%8D%E6%B3%95%EF%BC%9A%7D)

**匈牙利命名法：**基本原则是：变量名＝属性＋类型＋对象描述，其中每一对象的名称都要求有明确含义，可以取对象名字全称或名字的一部分。命名要基于容易记忆容易理解的原则。保证名字的连贯性是非常重要的。
**Camel命名法：**即骆驼式命名法，原因是采用该命名法的名称看起来就像骆驼的驼峰一样高低起伏。
**Camel命名法有两种形式：**混合使用大小写字母和单词之间加下划线，例如runFast和run_fast都属于Camel命名法。
**Pascal命名法：**与Camel命名法类似，不过Pascal命名法的首字母为大写字母。

![\color{red}{命名通则：}](https://math.jianshu.com/math?formula=%5Ccolor%7Bred%7D%7B%E5%91%BD%E5%90%8D%E9%80%9A%E5%88%99%EF%BC%9A%7D)

- 1、在所有命名中，都应使用标准的英文单词或缩写。不得使用拼音或拼音缩写，除非该名字描述的是中文特有的内容，如半角、全角, 声母、韵母等。
- 2、所有命名都应遵循望文知义原则，即名称应含义清晰、明确。
- 3、所有命名都不易过长，应控制在规定的最大长度以内。
- 4、所有命名都应尽量使用全称。
- 5、如果命名使用缩写，则应该使用《通用缩写表》（见附录）中的缩写；原则上不推荐使用《通用缩写表》以外的缩写，如果使用，则必须对其进行注释和说明。

![\color{red}{具体规范：}](https://math.jianshu.com/math?formula=%5Ccolor%7Bred%7D%7B%E5%85%B7%E4%BD%93%E8%A7%84%E8%8C%83%EF%BC%9A%7D)

- 1、工程名：不强制统一。
- 2、文件名： 
  - 基于工程名，开头3个字母应表明与哪一个工程相关。
  - 后面的字母应能够区别不同的功能。
  - 不区分大小写。
  - 长度不限于8.3格式，建议不多于30个字符。
  - 若文件用于定义和实现类，建议文件名与类名保持一致。

- 3、函数名： 
  - 参照 Windows API 的命名规范。
  - 推荐使用动宾结构。函数名应清晰反映函数的功能、用途。
  - 函数名最长不得超过30个字符。
  - 函数名第一个字母必须大写。
  - 全局函数必须以小写前缀"g"开头。
- 4、变量名： 
  - 原则上，变量名的命名遵从匈牙利记法。即：前缀 + 类型 + 变量名
- 1）格式：



```ruby
[m_|s_|g_] type [class name|struct name] variable name
```

- 2）解释： 
  - m_ ： 类的成员变量
  - ms_：类的静态成员变量
  - s_ ：静态全局变量
  - g_ ：普通全局变量
  - 类型缩写（type）
  - char, TCHAR： ch
  - char[]，TCHAR[]： sz
  - string： str
  - bool, BOOL： b
  - int, __int16,__int32,__int64： n
  - long： l
  - double： d
  - float： f
  - BYTE： by
  - WORD： w
  - DWORD： dw
  - unsigned： u
  - function： fn
  - p ：pointer
  - lp ：long pointer

变量名最长不得超过20个字符。

- 5、类名： 
  - 必须以大写"C"开头，后面字母反映具体含义，以清晰表达类的用途和功能为原则。
  - 接口必须以大写"I"开头，代表 Interface 。
  - 当名称由多个单词构成时，每一个单词的第一个字母必须大写。
- 6、结构体名、宏名、枚举名、联合名： 
  - 全部大写。
  - 枚举名加小写前缀"enum"。
    例：



```objectivec
typedef enum _CFILE_OPEN_MODE
{
enumOPEN_READONLY = 0,
enumOPEN_READWRITE = 1,
enumCREATE_ALWAY = 3
 } CFILE_OPEN_MODE;

//·宏名加小写前缀"def"。
```

例：



```cpp
#define defMAXNUMBER 100
```

- 结构名加小写前缀"tag"，之后必须以大写"C"开头。
  例：



```cpp
typedef struct tagKPOINT
{
int x;
int y;
} KPOINT;

//·联合名加小写前缀"uni"。
```

例：



```cpp
typedef union _VARIANT{
char unichVal;
int uninVal;
long unilVal;
float uniftVal;
...
} VARIANT;
```

![\color{red}{C/C++源代码书写规范（试行）}](https://math.jianshu.com/math?formula=%5Ccolor%7Bred%7D%7BC%2FC%2B%2B%E6%BA%90%E4%BB%A3%E7%A0%81%E4%B9%A6%E5%86%99%E8%A7%84%E8%8C%83%EF%BC%88%E8%AF%95%E8%A1%8C%EF%BC%89%7D)

- 1. 在.h/.cpp的开头应有一段格式统一的说明，内容包括：

  - a. 文件名 (FileName)；
  - b. 简短说明文件功能、用途 (Comment)；
  - c. 创建人 (Creater)；
  - d. 文件创建时间 (Date)。

例：



```php
/*!
  *@file
  *@brief
  *@author
  *@date
  */
```

- 1. 除非极其简单，否则对函数应有注释说明。内容包括：功能、入口/出口参数，必要时还可有备注或补充说明。
- 1. 每行代码的长度推荐为80列，最长不得超过120列；折行以对齐为准。

例：



```cpp
HANDLE CSOpenFile(const char cszFileName[],<br>               int nMode);
```

或者：



```xml
BOOL CSReadFile(<br>                HANDLE hFile,<br>               void *pvBuffer,<br>             int nReadSize,<br>              int *pnReadSize<br>             );<br>
```

- 1. 循环、分支代码，判断条件与执行代码不得在同一行上。

例：正确：



```bash
if (n == -2)
    n = 1;
else
    n = 2;
```

不得写做：



```bash
if (n == -2) n = 1;
else n = 2;
```

- 1. 指针的定义，* 号既可以紧接类型，也可以在变量名之前。

例：可写做：



```cpp
int* pnsize;
```

也可写做：



```cpp
int *pnsize;
```

但不得写做：



```cpp
int * pnsize;
```

- 1. 在类的成员函数内调用非成员函数时，在非成员函数名前必须加上“::”。
- 1. 函数入口参数有缺省值时，应注释说明。

例:



```objectivec
BOOL KSSaveToFile(const char cszFileName[],BOOL bCanReplace /* = TRUE */);
```

或者：



```objectivec
BOOL KSSaveToFile(const char cszFileName[],BOOL bCanReplace // = TRUE );
```

- 1. else if 必须写在一行。

- 1. 与‘{’、‘}’有关的各项规定：

  - 9.1‘{’、‘}’应独占一行。在该行内可有注释。

例：正确：



```cpp
for (i = 0; i < cbLine; i++)
{ // .....
    printf("Line %d:", i);
    printf("%s\n", pFileLines);
}
不得写做：

for (i = 0; i < cb; i++)
{printf("Line %d:", i);<br>printf("%s\n", pFileLines);}
```

- - 9.2‘{’必须另起一行，‘{’之后的代码必须缩进一个Tab。‘{’与‘}’必须在同一列上。

例：正确：



```xml
if (i > 0)
{<br>　　m = 1;<br>　　n++;
}
```

不得写做：



```undefined
if (i > 0) {
m = 1;
n++;
}
```

例外：



```kotlin
if (i == 0)
{ ASSERT(FALSE); return; }
```

- - 9.3 在循环、分支之后若只有一行代码，虽然可省略‘{’、‘}’，但不推荐这么做。若省略后可能引起歧义，则必须加上‘{’、‘}’。

- 1. 与空格有关的各项规定。

  - 10.1 在所有两目、三目运算符的两边都必须有空格。在单目运算符两端不必空格。但在‘->’、‘::’、‘.’、‘[’、‘]’等运算符前后，及‘&’（取地址）、‘*’（取值）等运算符之后不得有空格。

例：正确：



```cpp
int n = 0, nTemp;
for (int i = nMinLine; i <= nMaxLine; i++)
```

不得写做：



```cpp
int n=0, nTemp;
for ( int i=nMinLine; i<=nMaxLine; i++ )
```

- - 10.2 for、while、if 等关键词之后应有1个空格，再接‘(’，之后无空格；在结尾的‘)’前不得有空格。

例：正确：



```undefined
if (-2 == n)
```

不得写做：



```undefined
if(-2 == n)
```

或



```undefined
if ( -2 == n )
```

- - 10.3 调用函数、宏时，‘(’、‘)’前后不得有空格。

例：正确：



```bash
printf("%d\n", nIndex);
```

不得写做：



```bash
printf ("%d\n", nIndex);

printf( "%d\n", nIndex );
```

- - 10.4 类型强制转换时，‘(’‘)’前后不得有空格

例：可写做：



```undefined
(KSFile*)pFile;
```

也可写做：



```undefined
(KSFile *)pFile
```

不得写做：



```undefined
( KSFile* )pFile

( KSFile * ) pFile
```

- 1. 与缩进有关的各项规定

  - 11.1 缩进以 Tab 为单位。1 个 Tab 为 4 个空格
  - 11.2 下列情况，代码缩进一个 Tab: 
    - 1. 函数体相对函数名及‘{’、‘}’。

例：



```cpp
int Power(int x)
{
　　return (x * x);
}
```

- - - 1. if、else、for、while、do 等之后的代码。
- - - 1. 一行之内写不下，折行之后的代码，应在合理的位置进行折行。若有 + - * / 等运算符，则运算符应在上一行末尾，而不应在下一行的行首。
- - 11.3 下列情况，不必缩进：switch 之后的 case、default。

例：



```cpp
switch (nID)
{
case ID_PLAY:
　　......
　　break;
case ID_STOP:
　　......
　　break;
default:
　　......
　　break;
} 
```