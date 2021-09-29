#### 概述：

​		框架：.net（.netFramework 4.5.2）,语言：C#，应用方向：本地服务，web服务，目标平台：9号服务器 Windows平台

​		服务器信息：10.8.7.9, administrator  1QAZ2wsx

[^测试方法]: 查看安装日志，服务安装日志，怀疑的地方打上log，远程附加进程进行断点调试，域后面加 ’.\‘才能进行远程文件访问。

ps:此为后台进程，需要使用附加进程进行断点调试，可以使用soapUI模拟报文数据发送，

_________________________________________________________________________________________________________________________

1.入口 ：program.cs ,组件初始化

​	服务启动事件，OnStart->init():主机初始化，绑定服务运行METoPowerWebService

​	服务运行地址：url = "http://127.0.0.1:10010/EdgeWcf/METoPower";

2.METoPowerWebService类为接口实现类；接口规定了METoPower函数的返回值类型，传参结构

返回值类型是 自定义结构
    class ResponseData
    {
        [DataMember]
        public List<Tag> Tags { get; set; }
        [DataMember]
        public string Code { get; set; }
        [DataMember]
        public string Message { get; set; }
    }

传参结构为自定义结构 

​	泛型容器list，泛型元素为RequestData类（结构体）

 public class RequestData
    {
        [DataMember]
        public string Site { get; set; }
        [DataMember]
        public string Resource { get; set; }
        [DataMember]
        public string ItemCode { get; set; }
        [DataMember]
        public string ShopOrder { get; set; }
        [DataMember]
        public List<string> ParametricName { get; set; }
    }

请求由serialize方法由对象形式转换为json字符串，并交由InsertIssualDataToSql(requests);进行处理；

3.InsertIssualDataToSql(requests)

主要工作回应request请求；

​	1.解析request为SQL语句：GetSelectSql,获取sql查询语句

select 列 from 表名 as 别名 where 自定=表头 or ....

​	2.获取插入数据sql语句：GetInsertSql提取request内部数据ptricNme,遍历放入字符串数组，拼接SQL语句

insert  into 表名 VALUES（，，，，）

insert into 表名（列1，列2，，，） VALUES（，，，）

​	2.回复:ExcuteIsuueData





