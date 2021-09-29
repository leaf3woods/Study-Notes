**描述**

用于指导git commit的message的编写，好的统一的message编写格式可以使每次git的变动、合并的内容变得一目了然。

**基本格式**

[总体修改类型]($[影响范围]) [修改描述]

 

[变动类型标识] [变动描述]

[变动类型标识] [变动描述]

…

 

[其他说明]

 

**名词解释**

**总体修改类型**是使用一个英文单词来说明，本次修改总体上属于重构、bug修复、增加新功能还是别的什么。大致分为下列几种情况：

Refactor：代码重构（重写代码或在原有代码上进行修改，但不是bug的修复，也不是增加新功能，比如说修改原有逻辑等）

Feat：增加新功能

Fix：修复bug

Docs：增加、修改或删除文档

Sytle：修改代码结构（不影响程序运行，如美化代码等）

Chore：构建过程或辅助工具的变动

Test： 新增测试程序

 

**影响范围**是指本次修改会影响到的内容，如设备互联可以写服务名称，OA可以写修改的DEE所在的应用或表单，其他模块可以根据自身的情况进行编写。也可以将STK编号写在此处。

 

**修改描述**用较小的篇幅简单总结一下本次修改的内容或目的。

 

**变动类型标识**是用git提供的一种表情符号的方式，在每一条变动前写一个标识符，以简单明了地表现出每一条变动的类型，常用的类型如下：

:newspaper: 表示新建、删除、移动或修改文件夹![img](file:///C:\Users\yesen\AppData\Local\Temp\msohtmlclip1\01\clip_image002.jpg)

:books: 表示新建、移动、修改或删除文件![img](file:///C:\Users\yesen\AppData\Local\Temp\msohtmlclip1\01\clip_image004.jpg)

:ambulance: 表示修复程序bug ![img](file:///C:\Users\yesen\AppData\Local\Temp\msohtmlclip1\01\clip_image006.jpg)

:umbrella: 表示新建测试程序![img](file:///C:\Users\yesen\AppData\Local\Temp\msohtmlclip1\01\clip_image008.jpg)

:sparkles: 表示新增功能![img](file:///C:\Users\yesen\AppData\Local\Temp\msohtmlclip1\01\clip_image010.jpg)

:tractor: 表示修改或重构代码 ![img](file:///C:\Users\yesen\AppData\Local\Temp\msohtmlclip1\01\clip_image012.jpg)

 

**修改描述**用较小的篇幅简单说明一下单条变动内容。

 

**其他说明**是一些其他的，特别的说明性内容。一般不需要写

 

**举例说明**

```
需要新增一个服务，名称为GKGAlarmDataDealService，在125服务器上运行，用于将125服务器的数据库内，GKG数据的报警码进行转码。提交了一个STK，编号为5072
```

git commit说明：

**Feat($server125****、****STKID:5072) new service for exchanging GKG alarm data on mysql running on server 125**

:newspaper: create new document edgink_core/DailyTestOrDemoForC#/GKGAlarmDataDealService

:books: upload GKGAlarmDataDealService/bin/Debug

:sparkles: GKGAlarmDataDealService/Alarm.cs 存放报警信息的对象类

:sparkles: GKGAlarmDataDealService/AlarmDictionary.cs 存放报警信息对象的数据字典，用来查询数据

:sparkles: GKGAlarmDataDealService/ConfigChecker.cs 配置文件检查工具

:sparkles: GKGAlarmDataDealService/ConfigReader.cs 配置文件读取工具

:sparkles: GKGAlarmDataDealService/GKGAlarmDataDealService.cs 服务主体程序

:sparkles: GKGAlarmDataDealService/GlobalParams.cs 全局变量存取类

:sparkles: GKGAlarmDataDealService/Job.cs 定时任务

:sparkles: GKGAlarmDataDealService/MaxDateChecker.cs 最大日期配置文件检查工具

:sparkles: GKGAlarmDataDealService/MaxDateOperator.cs 最大日期配置文件读写工具

在git上呈现的效果：

![image-20210928080510458](C:\Users\yesen\AppData\Roaming\Typora\typora-user-images\image-20210928080510458.png)

这样就能很清晰得看出本次修改的目的是新增一个服务，也能看出服务的大致功能是什么。

能一眼就看出本次变动新增了一个GKGAlarmDataDealService文件夹，在此文件夹中上传了bin文件，新增了许多cs文件。

对应的stk任务编号为5072，也可以通过编号查阅stk得知修改的内容和具体需求。

**举例说明**

参考：https://blog.csdn.net/u011870547/article/details/81203419

![image-20210928080438799](C:\Users\yesen\AppData\Roaming\Typora\typora-user-images\image-20210928080438799.png)

![image-20210928080452319](C:\Users\yesen\AppData\Roaming\Typora\typora-user-images\image-20210928080452319.png)

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

更多表情符：

![https://img-blog.csdn.net/20180725151044665?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE4NzA1NDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70](file:///C:\Users\yesen\AppData\Local\Temp\msohtmlclip1\01\clip_image016.gif)

![https://img-blog.csdn.net/20180725151057830?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE4NzA1NDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70](file:///C:\Users\yesen\AppData\Local\Temp\msohtmlclip1\01\clip_image018.gif)