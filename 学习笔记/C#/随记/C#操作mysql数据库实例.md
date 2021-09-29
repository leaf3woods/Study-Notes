# C#操作mysql数据库实例

本文主要分享的是使用C#简单快捷的操作Mysql数据库的完整实现过程。

------

## 目录



- C操作mysql数据库实例
  - [目录](#目录)
  - 引用MySqlData库
    - [管理NuGet程序包](#管理nuget程序包)
    - [安装程序包 MySqlData](#安装程序包-mysqldata)
  - 创建Mysql连接工具类
    - [添加引用](#添加引用)
    - [完整代码](#完整代码)
  - 测试数据库
    - [数据库信息](#数据库信息)
    - [测试数据表](#测试数据表)
  - 实例代码及说明
    - [界面设计](#界面设计)
    - [完整代码](#完整代码-1)
    - [程序运行结果](#程序运行结果)
  - [结束语](#结束语)



## 引用MySql.Data库

### 管理NuGet程序包

- 在 `引用` 中选择 `管理NuGet程序包` 
  ![管理NuGet程序包](https://img-blog.csdn.net/20170427135239391?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGdqMTIzeGo=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 安装程序包 MySql.Data

- 在联机中搜索 `mysql` ，选择安装程序包 `MySql.Data` 
  ![安装程序包](https://img-blog.csdn.net/20170427135512643?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGdqMTIzeGo=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 创建Mysql连接工具类

### 添加引用

创建类`MysqlConnector` 并添加引用：

```
using MySql.Data.MySqlClient;
```

### 完整代码

```c#
public class MysqlConnector
{
    string server = null;
    string userid = null;
    string password = null;
    string database = null;
    string port = "3306";
    string charset = "utf-8";

    public MysqlConnector() { }
    public MysqlConnector SetServer(string server)
    {
        this.server = server;
        return this;
    }

    public MysqlConnector SetUserID(string userid)
    {
        this.userid = userid;
        return this;
    }

    public MysqlConnector SetDataBase(string database)
    {
        this.database = database;
        return this;
    }

    public MysqlConnector SetPassword(string password)
    {
        this.password = password;
        return this;
    }
    public MysqlConnector SetPort(string port)
    {
        this.port = port;
        return this;
    }
    public MysqlConnector SetCharset(string charset)
    {
        this.charset = charset;
        return this;
    }



    #region  建立MySql数据库连接
    /// <summary>
    /// 建立数据库连接.
    /// </summary>
    /// <returns>返回MySqlConnection对象</returns>
    private MySqlConnection GetMysqlConnection()
    {
        string M_str_sqlcon = string.Format("server={0};user id={1};password={2};database={3};port={4};Charset={5}", server, userid, password, database, port, charset);
        MySqlConnection myCon = new MySqlConnection(M_str_sqlcon);
        return myCon;
    }
    #endregion

    #region  执行MySqlCommand命令
    /// <summary>
    /// 执行MySqlCommand
    /// </summary>
    /// <param name="M_str_sqlstr">SQL语句</param>
    public void ExeUpdate(string M_str_sqlstr)
    {
        MySqlConnection mysqlcon = this.GetMysqlConnection();
        mysqlcon.Open();
        MySqlCommand mysqlcom = new MySqlCommand(M_str_sqlstr, mysqlcon);
        mysqlcom.ExecuteNonQuery();
        mysqlcom.Dispose();
        mysqlcon.Close();
        mysqlcon.Dispose();
    }
    #endregion

    #region  创建MySqlDataReader对象
    /// <summary>
    /// 创建一个MySqlDataReader对象
    /// </summary>
    /// <param name="M_str_sqlstr">SQL语句</param>
    /// <returns>返回MySqlDataReader对象</returns>
    public MySqlDataReader ExeQuery(string M_str_sqlstr)
    {
        Console.WriteLine(M_str_sqlstr);
        MySqlConnection mysqlcon = this.GetMysqlConnection();
        MySqlCommand mysqlcom = new MySqlCommand(M_str_sqlstr, mysqlcon);
        mysqlcon.Open();
        MySqlDataReader mysqlread = mysqlcom.ExecuteReader(CommandBehavior.CloseConnection);
        return mysqlread;
    }
    #endregion
```

## 测试数据库

### 数据库信息

我创建的 **测试数据库** 信息如下：

| item     | value    |
| -------- | -------- |
| 数据库名 | testdb   |
| 测试用户 | testuser |
| 口令     | 123456   |

### 测试数据表

创建一个数据表 **user** ，数据如下：

| id   | sname    | age  |
| ---- | -------- | ---- |
| 1    | 张三     | 18   |
| 2    | 李四     | 19   |
| 3    | 王二麻子 | 20   |

## 实例代码及说明

### 界面设计

我使用的是 **WinForm窗体程序** 进行演示，界面设计如下： 
![界面设计](https://img-blog.csdn.net/20170427143351083?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGdqMTIzeGo=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 完整代码

```c#
public partial class Form1 : Form
{
    //实例化连接对象
    MysqlConnector mc = new MysqlConnector();

    public Form1()
    {
        InitializeComponent();

        //设置数据库连接参数
        mc.SetServer("127.0.0.1")
          .SetDataBase("testdb")
          .SetUserID("testuser")
          .SetPassword("123456")
          .SetPort("3306")
          .SetCharset("utf8");
    }

    private void button1_Click(object sender, EventArgs e)
    {
        string sql = textBox1.Text;
        string result = "";
        //执行查询
        MySqlDataReader reader = mc.ExeQuery(sql);
        while (reader.Read())
        {
            for (int i = 0; i < reader.FieldCount; i++)
            {
                result += reader.GetName(i) + "\t" + reader.GetValue(i) + "\r\n";
            }
        }
        textBox2.Text = result;
        //执行增删改等操作
        //mc.ExeUpdate(sql);
    }
}
```



### 程序运行结果

执行语句 `select * from user`，程序运行截图： 
![运行截图](https://img-blog.csdn.net/20170427144201844?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGdqMTIzeGo=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 结束语

第一次这么认真写博客，以后还会继续的，关于C#其实我是初学者，很多代码的Java味比较重，在实现上有可能绕了远路，还请批评指正。 
 因为最近做了一个C#操作Mysql的小项目，我也是边学边做，总结了一点小工具，稍后会继续与大家分享，感谢您的阅读。