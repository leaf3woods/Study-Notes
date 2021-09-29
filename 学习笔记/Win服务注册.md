# [Windows服务注册](https://www.cnblogs.com/vsnb/p/12894712.html)

 

**1.普通服务注册**

在cmd下可有两种方法打开，net和sc，net用于打开没有被禁用的服务，语法是：
net start 服务名            启动 net start 服务名
                       停止 net stop 服务名
net stop 服务名

用sc可打开被禁用的服务，语法是：
sc config 服务名 start= demand //手动
sc condig 服务名 start= auto   //自动
sc config 服务名 start= disabled //禁用
sc start 服务名
sc stop 服务名
注：1)服务名不一定是你在服务面板看到的那个名，例如，你要打开被禁用的telnet服务，sc config telnet start= auto,报错：[SC] OpenService FAILED 1060，因为telnet的服务名不是telnet而是tlntsvr, sc config tlntsvr start= auto   就OK了，在服务面板里查看telnet属性，从可执行文件的路径里可看到服务程序名，即命令中的服务名。 2)start=后面有空格，少了就有错

sc.exe命令功能列表：　　注：以下命令中。＝号后面都有一个空格，＝号前面没有空格！


　　1.更改服务的启动状态（这是比较有用的一个功能） 　　

　　2.删除服务（除非对自己电脑的软、硬件所需的服务比较清楚，否则不建议删除任何系统服务，特别是基础服务）　　

　　3.停止或启动服务（功能上类似于net stop/start，但速度更快且能停止的服务更多）

　　具体的命令格式如下：　　

　　修改服务启动类型的命令行格式为（特别注意start=后面有一个空格）　　

　　sc config 服务名称 start= demand(设置服务为手动启动) 　　

　　sc config 服务名称 start= disabled(设置服务为禁用) 　　

　　停止/启动服务的命令行格式为 　　

　　sc stop/start 服务名称 　　

　　注意：平时常接触的都是服务的显示名称，而以上所指是服务名称，都可以在控制面板->管理工具->服务里面，双击对应的服务来查询。　　

　　先举例说明一下具体的设置方法： 　　

　　如设置远程注册表服务为手动其格式为 　　

　　sc config RemoteRegistry start= demand 　　

　　设为禁用的格式为：

　　sc config RemoteRegistry start= disabled 　　

　　停止服务则格式为： 　　

　　sc stop RemoteRegistry 　　

　　首先把自己所需设置的服务名称查到之后，按照上面的格式做成批处理文件，重装系统之后只要运行批处理文件即可。　　
以下是我的设置，以XpSp2为蓝本，可比对所用的系统进行增删和修改。注：未加入XpSp2的自动更新、安全中心、防火墙。　

　　sc config Alerter start= demand 　　

　　sc config TrkWks start= demand 　　

　　sc config helpsvc start= demand 　　

　　sc config policyAgent start= demand 　　

　　sc config dmserver start= demand 　　

　　sc config WmdmpmSn start= demand 　　

　　sc config Spooler start= demand 　　

　　sc config RemoteRegistry start= demand 　　

　　sc config NtmsSvc start= demand 　　

　　sc config seclogon start= demand 　　

　　sc config Schedule start= demand 　　

　　sc config WebClient start= demand 　　

　　sc config W32Time start= demand 　　

　　sc config WZCSVC start= demand

　　sc config ERSvc start= demand 　　

　　sc config Themes start= demand 　　

　　sc config FastUserSwitchingCompatibility start= disabled 　　

　　sc config Messenger start= disabled 　　

　　sc config protectedStorage start= disabled

　　sc config SSDpSRV start= disabled 　　

　　sc config TermService start= disabled 　　

　　sc config ShellHWDetection start= disabled 　　

　　如果需要立即关闭服务也可把以下代码跟在上面的代码之后

　　sc stop W32Time 　　

　　sc stop ShellHWDetection 　　

　　sc stop TrkWks

　　sc stop helpsvc

　　sc stop dmserver

　　sc stop policyAgent 　　

　　sc stop Spooler 　　

　　sc stop RemoteRegistry 　　

　　sc stop seclogon 　　

　　sc stop Schedule 　　

　　sc stop WZCSVC

　　sc stop ERSvc 　　

　　sc stop Themes 　　

　　sc stop FastUserSwitchingCompatibility 　　

　　sc stop protectedStorage 　　

　　sc stop SSDpSRV 　　

　　sc stop WebClient 　　

　　最后把修改好之后的代码存为services.cmd，在以后进行服务设置时，直接运行事先保存好的批处理文件就可以做到事半功倍了。　　
　　看到这里，使用Win2000的朋友也不必失望，sc.exe这个命令行工具对Win2000同样适用，可从装有WinXp或者Win2003的机器里面拷贝sc.exe文件，与保存好的批处理文件放在一起，然后执行批处理文件即可。　　

　　对注册表比较熟悉的朋友可能会想到用注册表来设置服务的启动类型，这也是一种可行的方法，本身却有着内在不足。原因是服务启动类型在注册表中对应的键值较长且分散，进行整理不方便直观且易错漏，所以这种方法比较适用于无人值守的安装时使用。

使用案例:

在命令行下启动自动更新服务:

C:\>sc config wuauserv start= auto
[SC] ChangeServiceConfig SUCCESS

C:\>sc start wuauserv

SERVICE_NAME: wuauserv
     TYPE        : 20 WIN32_SHARE_PROCESS
     STATE        : 2 START_PENDING
                 (NOT_STOPPABLE,NOT_PAUSABLE,IGNORES_SHUTDOWN)
     WIN32_EXIT_CODE   : 0 (0x0)
     SERVICE_EXIT_CODE : 0 (0x0)
     CHECKPOINT     : 0x0
     WAIT_HINT      : 0x7d0
     PID         : 1156
     FLAGS        :

C:\>

 

CMD-SC命令详解 2009-10-27 15:18 :\>sc/?
描述:
     SC 是用于与服务控制管理器通信的命令行程序。
用法:
     sc <server> [command] [service name] <option1> <option2>...


     选项 <server> 的格式为 "\\ServerName"
     可以键入 "sc [command]"以获得命令的进一步帮助
     命令:
      query-----------查询服务的状态，
              或枚举服务类型的状态。
      queryex---------查询服务的扩展状态，
              或枚举服务类型的状态。
      start-----------启动服务。
      pause-----------发送 PAUSE 控制请求到服务。
      interrogate-----发送 INTERROGATE 控制请求到服务。
      continue--------发送 CONTINUE 控制请求到服务。
      stop------------发送 STOP 请求到服务。
      config----------(永久地)更改服务的配置。
      description-----更改服务的描述。
      failure---------更改服务失败时所进行的操作。
      qc--------------查询服务的配置信息。
      qdescription----查询服务的描述。
      qfailure--------查询失败服务所进行的操作。
      delete----------(从注册表)删除服务。
      create----------创建服务(将其添加到注册表)。
      control---------发送控制到服务。
      sdshow----------显示服务的安全描述符。
      sdset-----------设置服务的安全描述符。
      GetDisplayName--获取服务的 DisplayName。
      GetKeyName------获取服务的 ServiceKeyName。
      EnumDepend------枚举服务的依存关系。

​     下列命令不查询服务名称:
​     sc <server> <command> <option>
​      boot------------(ok | bad) 表明是否将上一次启动保存为
​              最后所知的好的启动配置
​      Lock------------锁定服务数据库
​      QueryLock-------查询 SCManager 数据库的 LockStatus
示例:
​     sc start MyService
sc config   TrkWks start= DISABLED
sc config   upnphost start= DISABLED
sc config   UPS start= DISABLED
sc config   usprserv start= DISABLED
sc config   VSS start= DISABLED
sc config   W32Time start= DISABLED
sc config   WebClient start= DISABLED
sc config   winmgmt start= DISABLED
sc config   WMConnectCDS start= DISABLED
sc config   WmdmPmSN start= DISABLED
sc config   Wmi start= DISABLED
sc config   WmiApSrv start= DISABLED
sc config   WMPNetworkSvc start= DISABLED
sc config   wscsvc start= DISABLED
sc config   wuauserv start= DISABLED
sc config   WudfSvc start= DISABLED
sc config   WZCSVC start= DISABLED
sc config   xmlprov start= DISABLED



:XP2
echo.
echo  正在备份您的服务，以免优化过出问题了可以及时恢复
echo  备份会生成一个以当前时间命名的BAT（批处理）文件
echo  恢复时只要双击即可,正在备份，请稍等......
@echo off
rem  get current date and time
for /f "tokens=1, 2, 3, 4 delims=-/. " %%j in ('Date /T') do set FILENAME=srv_%%j_%%k_%%l_%%m
for /f "tokens=1, 2 delims=: " %%j in ('TIME /T') do set FILENAME=%FILENAME%_%%j_%%k.bat

rem get all service name
sc query type= service state= all| findstr /r /C:"SERVICE_NAME:" >tmpsrv.txt
echo Save Service Start State In %FILENAME%
rem save service start state into batch file
rem

echo @echo Restore The Service Start State Saved At %TIME% %DATE% >"%FILENAME%"
echo @pause >>"%FILENAME%"

for /f "tokens=2 delims=:" %%j in (tmpsrv.txt) do @( sc qc %%j |findstr  START_TYPE >tmpstype.txt &&  for /f "tokens=4 delims=:_ " %%s in ( tmpstype.txt) do @echo sc config  %%j start= %%s >>"%FILENAME%")
echo @pause >>"%FILENAME%"
del tmpsrv.txt
del tmpstype.txt
echo  备份完成，启动优化服务程序，请稍等....
sc config   Alerter start= DISABLED
sc config   ALG start= DISABLED
sc config   AppMgmt start= DISABLED
sc config   aspnet_state start= DISABLED
sc config   AudioSrv start= AUTO
sc config   BITS start= DISABLED
sc config   Browser start= DISABLED
sc config   CiSvc start= DISABLED
sc config   ClipSrv start= DISABLED
sc config   COMSysApp start= DISABLED
sc config   CryptSvc start= DEMAND
sc config   DcomLaunch start= AUTO
sc config   DF5Serv start= AUTO
sc config   Dhcp start= DISABLED
sc config   dmadmin start= DEMAND
sc config   dmserver start= DEMAND
sc config   Dnscache start= DISABLED
sc config   ERSvc start= DISABLED
sc config   Eventlog start= AUTO
sc config   EventSystem start= DISABLED
sc config   FastUserSwitchingCompatibility start= DISABLED
sc config   helpsvc start= DISABLED
sc config   HidServ start= DISABLED
sc config   HTTPFilter start= DISABLED
sc config   IDriverT start= DEMAND
sc config   ImapiService start= DISABLED
sc config   lanmanserver start= DISABLED
sc config   lanmanworkstation start= AUTO
sc config   LmHosts start= DISABLED
sc config   Messenger start= DISABLED
sc config   mnmsrvc start= DISABLED
sc config   MSDTC start= DISABLED
sc config   MSIServer start= DEMAND
sc config   NetDDE start= DISABLED
sc config   NetDDEdsdm start= DISABLED
sc config   Netlogon start= DISABLED

sc config   Netman start= DEMAND
sc config   Nla start= DISABLED
sc config   NtLmSsp start= DISABLED
sc config   NtmsSvc start= DISABLED
sc config   NVSvc start= DISABLED
sc config   ose start= DISABLED
sc config   PlugPlay start= AUTO
sc config   PolicyAgent start= DISABLED
sc config   ProtectedStorage start= DISABLED
sc config   RasAuto start= DISABLED
sc config   RasMan start= DISABLED
sc config   RDSessMgr start= DISABLED
sc config   RemoteAccess start= DISABLED
sc config   RemoteRegistry start= DISABLED
sc config   RpcLocator start= DEMAND
sc config   RpcSs start= AUTO
sc config   RSVP start= DISABLED
sc config   SamSs start= DISABLED
sc config   SCardSvr start= DISABLED
sc config   Schedule start= DISABLED
sc config   seclogon start= DISABLED
sc config   SENS start= DISABLED
sc config   SharedAccess start= DISABLED
sc config   ShellHWDetection start= DISABLED
sc config   Spooler start= DISABLED
sc config   srservice start= DISABLED
sc config   SSDPSRV start= DISABLED
sc config   stisvc start= DISABLED
sc config   SwPrv start= DISABLED
sc config   SysmonLog start= DISABLED
sc config   TapiSrv start= DISABLED
sc config   TermService start= DISABLED
sc config   Themes start= DISABLED
sc config   TlntSvr start= DISABLED
sc config   TrkWks start= DISABLED
sc config   Telephony start= AUTO
sc config   upnphost start= DISABLED
sc config   UPS start= DISABLED
sc config   usprserv start= DISABLED
sc config   VSS start= DISABLED
sc config   W32Time start= DISABLED
sc config   WebClient start= DISABLED
sc config   winmgmt start= DISABLED
sc config   WMConnectCDS start= DISABLED
sc config   WmdmPmSN start= DISABLED
sc config   Wmi start= DISABLED
sc config   WmiApSrv start= DISABLED
sc config   WMPNetworkSvc start= DISABLED
sc config   wscsvc start= DISABLED
sc config   wuauserv start= DISABLED
sc config   WudfSvc start= DISABLED
sc config   WZCSVC start= DISABLED
sc config   xmlprov start= DISABLED