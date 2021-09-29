# [c#命名规范](https://www.cnblogs.com/paulhe/p/4638799.html)

　　注重代码书写的程序员应该是一个比较有修养的人，下面这些命名规则不一定要绝对遵从，但值得参考。在应用规则

时要进行合理的判断。

**Pascal** **规则（帕斯卡命名）
**每个单词开头的字母大写(如 TestCounter).
 
**Camel** **规则(大驼峰和小驼峰命名)
**除了第一个单词外的其他单词的开头字母大写. 如. testCounter.

**Upper** **规则
**仅用于一两个字符长的常量的缩写命名,超过三个字符长度应该应用Pascal规则.例如：

```
public class Math
{
　　public const PI = ...
　　public const E = ...
　　public const FeigenBaumNumber = ...
}
```

  
具体的规则总结如下：
 
**类命名指导**

\- 类名应该为名词及名词短语，尽可能使用完整的词
\- 使用Pascal规则
\- 在适当的地方，使用复合单词命名派生的类。派生类名称的第二个部分应当是基类的名称。

  　例如，ApplicationException 对于从名为 Exception 的类派生的类是适当的名称，原因是 ApplicationException

 是一种 Exception。

 请在应用该规则时进行合理的判断。

 　　例如，Button 对于从 Control 派生的类是适当的名称。尽管按钮是一种控件，但是将 Control 作为类名称的一部

 分将使名称不必要地加长。
 
**接口命名规则**

\- 接口名称应该为名词及名词短语或者描述其行为的形容词，尽可能使用完整的词。
 
**枚举命名规则**

\- 对于 Enum 类型和值名称使用 Pascal 大小写
\- 少用缩写
\- 不要在 Enum 类型名称上使用 Enum 后缀
\- 对大多数 Enum 类型使用单数名称，但是对作为位域的 Enum 类型使用复数名称。
\- 总是将 FlagsAttribute 添加到位域 Enum 类型
 
 **变量命名**

\- 使用 Camel 命名规则

\- 在简单的循环语句中计数器变量使用 i, j, k, l, m, n

**方法命名**

\- 使用Pascal规则
\- 对方法名采用一致的动词/宾语或宾语/动词顺序

 　　例如，将动词置于前面时，所使用的名称诸如 InsertWidget 和 InsertSprocket；将宾语置于前面时，所使用的名

 称诸如 WidgetInsert 和 SprocketInsert，在此推荐前者。

\- 不要在方法中重复类的名称

　　  例如，如果某个类已命名为 Book，则不要将某个方法称为 Book.CloseBook，而可以将方法命名为 Book.Close。
 
**属性命名**

\- 名称应该为名词及名词短语
\- 使用Pascal规则
\- 对于bool型属性或者变量使用Is（is）作为前缀，不要使用Flag后缀，例如应该使用IsDeleted,而不要使用DeleteFlag
 
**集合命名**

\- 名称应该为名词及名词短语
\- 使用Pascal规则
\- 名称后面追加“Collection”

（个人觉得代表集合的类后面加Collection,代表集合的变量后面加List）
 
**事件命名**

\- event handlers命名使用 EventHandler 后缀
\- 两个参数分别使用 sender 及 e
\- 使用Pascal规则
\- 事件参数使用EventArgs 后缀
\- 事件命名使用语法时态反映其激发的状态，例如 Changed，Changing
\- 考虑使用动词命名. 变量命名
 

**其它常用的编码规则** 

\- 代码的缩进。要用Tab，而不要用space
\- 局部变量的名称要有意义。不要用x，y，z等等（除用于For循环变量中可使用i，j，k，l，m，n）
\- 所有的成员变量声明在类的顶端，用一个换行把它和方法分开
\- 用有意义的名字命名namespace，如：产品名、公司名
\- 把相似的内容放在一起，比如数据成员、属性、方法、事件等，并适当的使用#region…#endregion

自定义的属性以Attribute结尾

```
public class AuthorAttribute : Attribute
{
} 
```

自定义的异常以Exception结尾

```
public class AppException : Exception
{
}
```

 

命名规范的总结用表格表示如下：

**与类相关：**

| 标识符           | 大小写 | 示例                                                  |
| ---------------- | ------ | ----------------------------------------------------- |
| 类/结构          | Pascal | AppDomain                                             |
| 枚举类型         | Pascal | ErrorLevel                                            |
| 枚举值           | Pascal | FatalError                                            |
| 事件             | Pascal | ValueChange                                           |
| 异常类           | Pascal | WebException注意 总是以 Exception 后缀结尾。          |
| 只读的静态字段   | Pascal | RedValue                                              |
| 接口             | Pascal | IDisposable注意 总是以 I 前缀开始。                   |
| 集合             | Pascal | CustomerCollection 注意 总是以Collection结束          |
| 方法             | Pascal | ToString                                              |
| 命名空间         | Pascal | System.Drawing                                        |
| 参数             | Camel  | typeName                                              |
| 属性             | Pascal | BackColor                                             |
| 受保护的实例字段 | Camel  | redValue注意 很少使用。属性优于使用受保护的实例字段。 |
| 公共实例字段     | Pascal | RedValue注意 很少使用。属性优于使用公共实例字段。     |

**与变量命名相关（**根据不同的数据类型前缀+首字母大写的变量描述**）**

| 类型     | 前缀 | 示例               |
| -------- | ---- | ------------------ |
| Array    | arr  | arrShoppingList    |
| Boolean  | bln  | blnIsPostBack      |
| Byte     | byt  | bytPixelValue      |
| Char     | chr  | chrDelimiter       |
| DateTime | dtm  | dtmStartDate       |
| Decimal  | dec  | decAverageHeight   |
| Double   | dbl  | dblSizeofUniverse  |
| Integer  | int  | intRowCounter      |
| Long     | lng  | lngBillGatesIncome |
| Object   | obj  | objReturnValue     |
| Short    | shr  | shrAverage         |
| Single   | sng  | sngMaximum         |
| String   | str  | strFirstName       |

**与ADO.NET有关**

| 数据类型     | 数据类型简写 | 标准命名举例        |
| ------------ | ------------ | ------------------- |
| Connection   | con          | conNorthwind        |
| Command      | cmd          | cmdReturnProducts   |
| Parameter    | parm         | parmProductID       |
| DataAdapter  | dad          | dadProducts         |
| DataReader   | dtr          | dtrProducts         |
| DataSet      | dst          | dstNorthWind        |
| DataTable    | dtbl         | dtblProduct         |
| DataRow      | drow         | drowRow98           |
| DataColumn   | dcol         | dcolProductID       |
| DataRelation | drel         | drelMasterDetail    |
| DataView     | dvw          | dvwFilteredProducts |

 **与页面控件有关(html标签)**

| 数据类型          | 数据类型简写 | 标准命名举例 |
| ----------------- | ------------ | ------------ |
| Label             | lbl          | lblMessage   |
| LinkLabel         | llbl         | llblToday    |
| Button            | btn          | btnSave      |
| TextBox           | txt          | txtName      |
| MainMenu          | mmnu         | mmnuFile     |
| CheckBox          | chk          | chkStock     |
| RadioButton       | rbtn         | rbtnSelected |
| GroupBox          | gbx          | gbxMain      |
| PictureBox        | pic          | picImage     |
| Panel             | pnl          | pnlBody      |
| DataGrid          | dgrd         | dgrdView     |
| ListBox           | lst          | lstProducts  |
| CheckedListBox    | clst         | clstChecked  |
| ComboBox          | cbo          | cboMenu      |
| ListView          | lvw          | lvwBrowser   |
| TreeView          | tvw          | tvwType      |
| TabControl        | tctl         | tctlSelected |
| DateTimePicker    | dtp          | dtpStartDate |
| HscrollBar        | hsb          | hsbImage     |
| VscrollBar        | vsb          | vsbImage     |
| Timer             | tmr          | tmrCount     |
| ImageList         | ilst         | ilstImage    |
| ToolBar           | tlb          | tlbManage    |
| StatusBar         | stb          | stbFootPrint |
| OpenFileDialog    | odlg         | odlgFile     |
| SaveFileDialog    | sdlg         | sdlgSave     |
| FoldBrowserDialog | fbdlg        | fgdlgBrowser |
| FontDialog        | fdlg         | fdlgFoot     |
| ColorDialog       | cdlg         | cdlgColor    |
| PrintDialog       | pdlg         | pdlgPrint    |

**与页面控件有关(html标签)**

| 数据类型               | 简写 | 举例                 |
| ---------------------- | ---- | -------------------- |
| AdRotator              | adrt | Example              |
| Button                 | btn  | btnSubmit            |
| Calendar               | cal  | calMettingDates      |
| CheckBox               | chk  | chkBlue              |
| CheckBoxList           | chkl | chklFavColors        |
| CompareValidator       | valc | valcValidAge         |
| CustomValidator        | valx | valxDBCheck          |
| DataGrid               | dgrd | dgrdTitles           |
| DataList               | dlst | dlstTitles           |
| DropDownList           | drop | dropCountries        |
| HyperLink              | lnk  | lnkDetails           |
| Image                  | img  | imgAuntBetty         |
| ImageButton            | ibtn | ibtnSubmit           |
| Label                  | lbl  | lblResults           |
| LinkButton             | lbtn | lbtnSubmit           |
| ListBox                | lst  | lstCountries         |
| Panel                  | pnl  | pnlForm2             |
| PlaceHolder            | plh  | plhFormContents      |
| RadioButton            | rad  | radFemale            |
| RadioButtonList        | radl | radlGender           |
| RangeValidator         | valg | valgAge              |
| Regularexpression_r    | vale | valeEmail_Validator  |
| Repeater               | rpt  | rptQueryResults      |
| RequiredFieldValidator | valr | valrFirstName        |
| Table                  | tbl  | tblCountryCodes      |
| TableCell              | tblc | tblcGermany          |
| TableRow               | tblr | tblrCountry          |
| TextBox                | txt  | txtFirstName         |
| ValidationSummary      | vals | valsFormErrors       |
| XML                    | xmlc | xmlcTransformResults |