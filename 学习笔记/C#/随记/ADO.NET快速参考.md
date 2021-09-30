# [ADO.NET](https://www.cnblogs.com/best/p/7714500.html)



**目录**

- [一、ADO.NET概要](#_label0)

- [二、ADO.NET的组成](#_label1)

- [三、Connection连接对象](#_label2)

- - [3.1、连接字符串](#_lab2_2_0)

  - - [3.1.1、SQL Server连接字符串](#_label3_2_0_0)
    - [3.1.2、Access连接字符串](#_label3_2_0_1)
    - [3.1.3、MySQL连接字符串](#_label3_2_0_2)
    - [3.1.4、DB2连接字符串](#_label3_2_0_3)
    - [3.1.5、Oracle连接字符串](#_label3_2_0_4)

  - [3.2、连接到数据库](#_lab2_2_1)

  - [3.3、示例](#_lab2_2_2)

  - - [3.3.1、创建数据库与表](#_label3_2_2_0)
    - [ 3.3.2、创建窗体项目MyCar](#_label3_2_2_1)
    - [3.3.3、连接到数据](#_label3_2_2_2)

- [四、Command对象](#_label3)

- - [4.1、ExecuteNonQuery](#_lab2_3_0)

  - - [4.1.1、拼接字符串](#_label3_3_0_0)
    - [4.1.2、参数](#_label3_3_0_1)
    - [4.1.3、删除](#_label3_3_0_2)

  - [4.2、ExecuteScalar ()](#_lab2_3_1)

- [五、ExecuteReader获得数据](#_label4)

- - [5.1、使用ExecuteReader实现数据查询](#_lab2_4_0)
  - [5.2、实体类](#_lab2_4_1)
  - [5.3、DataGridView展示数据](#_lab2_4_2)
  - [5.4、删除功能](#_lab2_4_3)
  - [5.5、编辑功能](#_lab2_4_4)

- [六、综合示例](#_label5)

- - [6.1、创建数据库与表](#_lab2_5_0)

  - - [6.1.1、创建HR数据库](#_label3_5_0_0)
    - [6.1.2、创建Emp员工表](#_label3_5_0_1)

  - [6.2、创建项目与实体类](#_lab2_5_1)

  - - [6.2.1、创建项目](#_label3_5_1_0)
    - [6.2.2、创建实体类](#_label3_5_1_1)
    - [6.2.3、封装数据访问](#_label3_5_1_2)

  - [6.3、实现展示功能](#_lab2_5_2)

  - [6.4、实现新增功能](#_lab2_5_3)

  - [6.5、实现删除功能](#_lab2_5_4)

  - [6.6、实现编辑功能](#_lab2_5_5)

  - [6.7、实现搜索功能](#_lab2_5_6)

  - [6.8、所有代码与扩展](#_lab2_5_7)

- [七、作业](#_label6)

- - [7.1、大作业](#_lab2_6_0)
  - [7.2、第1次小作业](#_lab2_6_1)
  - [7.3、第2次小作业](#_lab2_6_2)

- [八、视频与资料下载](#_label7)

# 一、ADO.NET概要

ADO.NET是.NET框架中的重要组件，主要用于完成C#应用程序访问数据库。

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023090555785-1366516311.png)

# 二、ADO.NET的组成

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023091053051-396149880.png)

```c#
①System.Data  → DataTable，DataSet，DataRow，DataColumn，DataRelation，Constraint，DataColumnMapping，DataTableMapping
②System.Data.Coummon     → 各种数据访问类的基类和接口
③System.Data.SqlClient   → 对Sql Server进行操作的数据访问类
  主要有：   a) SqlConnection            → 数据库连接器
            b) SqlCommand               → 数据库命名对象
            c) SqlCommandBuilder        → 生存SQL命令
            d) SqlDataReader            → 数据读取器
            e) SqlDataAdapter           → 数据适配器，填充DataSet
            f) SqlParameter             → 为存储过程定义参数
            g) SqlTransaction           → 数据库事物
```

# 三、Connection连接对象

Connection对象也称为数据库连接对象，Connection对象的功能是负责对数据源的连接。所有Connection对象的基类都是DbConnection类。 

## **3.1、连接字符串**

基本语法：数据源(Data Source)+数据库名称(Initial Catalog)+用户名(User ID)+密码(Password)

### 3.1.1、SQL Server连接字符串

标准安全连接： 

Data Source=.;Initial Catalog=myDataBase;User Id=myUsername;Password=myPassword;或者

Server=myServerAddress;Database=myDataBase;User Id=myUsername;Password=myPassword;Trusted_Connection=False;

可信连接：

Data Source=myServerAddress;Initial Catalog=myDataBase;Integrated Security=SSPI;或者

Server=myServerAddress;Database=myDatabase;Trusted_Connection=True; 

### 3.1.2、Access连接字符串

Provider=Microsoft.Jet.OLEDB.4.0;Data Source=C:\myDatabase.mdb;User Id=admin;Password=;

### 3.1.3、MySQL连接字符串

Server=myServerAddress;Database=myDatabase;Uid=myUsername;Pwd=myPassword;

### 3.1.4、DB2连接字符串

Server=myAddress:myPortNumber;Database=myDatabase;UID=myUsername;PWD=myPassword;

### 3.1.5、Oracle连接字符串

Data Source=TORCL;User Id=myUsername;Password=myPassword; 

在VS中获得连接字符串并连接到数据库：

工具->连接到数据库

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023102955613-1462009058.png)

选择SQLServer

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023103034644-781864166.png)

继续

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023103108644-248861927.png)

如上图，填写好相关信息

在高级中可以查看连接字符串的所有信息

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023103402301-1309721307.png)

在VS中可以实现数据库管理：

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023103552863-1051673337.png)

## 3.2、连接到数据库

Connection对象有两个重要属性： 

（1）ConnectionString：表示用于打开 SQL Server 数据库的字符串； 
（2）State：表示 Connection 的状态，有Closed和Open两种状态。 

Connection对象有两个重要方法：

（1）Open()方法：指示打开数据库；

（2）Close()方法：指示关闭数据库。

```c#
//创建连接对象1
using (SqlConnection conn1 = new SqlConnection("连接字符串"))    
{        
    conn1.Open();       
}
```

## 3.3、示例

### 3.3.1、创建数据库与表

```c#
/**创建数据库*/
create database MyCar;
go
use MyCar;
/**创建表*/
create table Car
(
Id int primary key identity(1,1),  --编号
Title nvarchar(128) not null,  --车名
Speed int default(0),  --车速
Info ntext --详细
)

/**添加数据*/
insert into Car(Title,Speed,Info)
select 'BYD',130,'比亚迪' union
select 'BMW',160,'宝马' union
select 'Benz',160,'奔驰' 

/**查询*/
SELECT [Id]
      ,[Title]
      ,[Speed]
      ,[Info]
  FROM [MyCar].[dbo].[Car]
GO
```

###  3.3.2、创建窗体项目MyCar

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023111111160-163134555.png)

### 3.3.3、连接到数据

创建连接对象，打开数据

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;

using System.Data.SqlClient;

namespace MyCar
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void btnConnection_Click(object sender, EventArgs e)
        {
            //创建连接对象，指定连接字符串参数
            SqlConnection conn = new SqlConnection("Data Source=.;Initial Catalog=MyCar;User Id=sa;Password=sa;");
            //打开数据
            conn.Open();
            MessageBox.Show("打开成功，状态"+conn.State);
            conn.Close();
            MessageBox.Show("关闭数据库成功");
        }
    }
}
```

执行结果：

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023112007348-499460254.png)

# 四、Command对象

Command对象也称为数据库命令对象，Command对象主要执行包括添加、删除、修改及查询数据的操作的命令。也可以用来执行存储过程。用于执行存储过程时需要将Command对象的CommandType 属性设置为CommandType.StoredProcedure，默认情况下CommandType 属性为CommandType.Text，表示执行的是普通SQL语句。
Command主要有三个方法： 

## 4.1、ExecuteNonQuery

**ExecuteNonQuery()：**执行一个SQL语句，返回受影响的行数，这个方法主要用于执行对数据库执行增加、更新、删除操作，注意查询的时候不是调用这个方法。用于完成insert，delete，update操作。

```c#
        //新增
        private void btnAdd_Click(object sender, EventArgs e)
        {
            //创建连接对象，并使用using释放（关闭），连接用完后会被自动关闭
             using (SqlConnection conn=new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                 //打开连接
                conn.Open();
                 //将执行的sql
                String sql = "INSERT INTO Car([Title] ,[Speed] ,[Info]) VALUES('奇瑞' ,190,'国产轿车')";
                 //创建命令对象，指定要执行sql语句与连接对象conn
                SqlCommand cmd = new SqlCommand(sql,conn);
                 //执行,返回影响行数
                int rows=cmd.ExecuteNonQuery();
                if (rows > 0) MessageBox.Show("新增成功！");
            }

            //using 相当如下代码，确保连接对象一定会关闭
            //SqlConnection conn=null;
            //try
            //{
            //    conn=new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar");
            //}
            //finally
            //{
            //    conn.Close();
            //}
        }
```

执行结果：

**![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023142501644-897744257.png)**

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023142521019-1990609235.png)

### 4.1.1、拼接字符串

```c#
        private void btnAdd_Click(object sender, EventArgs e)
        {
            //创建连接对象，并使用using释放（关闭），连接用完后会被自动关闭
             using (SqlConnection conn=new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                 //打开连接
                conn.Open();
                 //将执行的sql
                String sql =String.Format("INSERT INTO Car([Title] ,[Speed] ,[Info]) VALUES('{0}' ,{1},'{2}')"
                    ,txtTitle.Text,txtSpeed.Text,txtInfo.Text);
                 //创建命令对象，指定要执行sql语句与连接对象conn
                SqlCommand cmd = new SqlCommand(sql,conn);
                 //执行,返回影响行数
                int rows=cmd.ExecuteNonQuery();
                if (rows > 0) MessageBox.Show("新增成功！");
            }
        }
```

执行：

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023150914832-1330077027.png)

### 4.1.2、参数

如果直接拼接字符串会存在安全隐患，使用参数可以解决问题。

```c#
        private void btnAdd_Click(object sender, EventArgs e)
        {
            //创建连接对象，并使用using释放（关闭），连接用完后会被自动关闭
             using (SqlConnection conn=new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                 //打开连接
                conn.Open();
                 //将执行的sql
                String sql = "INSERT INTO  Car([Title] ,[Speed] ,[Info]) VALUES(@Ttile,@Speed,@Info)";
                 //创建命令对象，指定要执行sql语句与连接对象conn
                SqlCommand cmd = new SqlCommand(sql,conn);
                 //指定参数
                cmd.Parameters.Add(new SqlParameter("@Ttile", txtTitle.Text));
                cmd.Parameters.Add(new SqlParameter("@Speed", txtSpeed.Text));
                cmd.Parameters.Add(new SqlParameter("@Info", txtInfo.Text));
                 //执行,返回影响行数
                int rows=cmd.ExecuteNonQuery();
                if (rows > 0) MessageBox.Show("新增成功！");
            }
        }
```

执行结果：

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023151631316-661121542.png)

### 4.1.3、删除

这里的示例是insert，如果想执行delete与update代码是一样的，只是变化了SQL。

示例：

```c#
        /// <summary>
        /// 删除
        /// </summary>
        private void btnDelete_Click(object sender, EventArgs e)
        {
            //创建连接对象，并使用using释放（关闭），连接用完后会被自动关闭
            using (SqlConnection conn = new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                //打开连接
                conn.Open();
                //将执行的sql
                String sql = "delete from Car where Title=@Title";
                //创建命令对象，指定要执行sql语句与连接对象conn
                SqlCommand cmd = new SqlCommand(sql, conn);
                //指定参数
                cmd.Parameters.Add(new SqlParameter("@Title", txtTitle.Text));
                //执行,返回影响行数
                int rows = cmd.ExecuteNonQuery();
                MessageBox.Show("删除成功"+rows+"行！");
            }
        }
```

执行结果：

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023155917691-1334457773.png)

## **4.2、ExecuteScalar ()**

**ExecuteScalar ()**从数据库检索单个值。这个方法主要用于统计操作。ExecuteScalar ()这个方法是针对SQL语句执行的结果是一行一列的结果集，这个方法只返回查询结果集的第一行第一列。

executeScalar主要用于查询单行单列的值，如聚合函数（count,max,min,agv,sum）。

 ![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023160349176-28343421.png)

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023161232144-676857705.png)

示例：

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;

using System.Data.SqlClient;

namespace MyCar
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            GetCount();
        }

        private void btnConnection_Click(object sender, EventArgs e)
        {
            //创建连接对象，指定连接字符串参数
            SqlConnection conn = new SqlConnection("Data Source=.;Initial Catalog=MyCar;User Id=sa;Password=sa;");
            //打开数据
            conn.Open();
            MessageBox.Show("打开成功，状态" + conn.State);
            conn.Close();
            MessageBox.Show("关闭数据库成功");
        }

        //新增
        private void btnAdd_Click(object sender, EventArgs e)
        {
            //创建连接对象，并使用using释放（关闭），连接用完后会被自动关闭
            using (SqlConnection conn = new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                //打开连接
                conn.Open();
                //将执行的sql
                String sql = "INSERT INTO Car(Title ,Speed ,Info) VALUES(@Ttile,@Speed,@Info)";
                //创建命令对象，指定要执行sql语句与连接对象conn
                SqlCommand cmd = new SqlCommand(sql, conn);
                //指定参数
                cmd.Parameters.Add(new SqlParameter("@Ttile", txtTitle.Text));
                cmd.Parameters.Add(new SqlParameter("@Speed", txtSpeed.Text));
                cmd.Parameters.Add(new SqlParameter("@Info", txtInfo.Text));
                //执行,返回影响行数
                int rows = cmd.ExecuteNonQuery();
                if (rows > 0) { MessageBox.Show("新增成功！"); GetCount(); }
            }
        }

        /// <summary>
        /// 删除
        /// </summary>
        private void btnDelete_Click(object sender, EventArgs e)
        {
            //创建连接对象，并使用using释放（关闭），连接用完后会被自动关闭
            using (SqlConnection conn = new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                //打开连接
                conn.Open();
                //将执行的sql
                String sql = "delete from Car where Title=@Title";
                //创建命令对象，指定要执行sql语句与连接对象conn
                SqlCommand cmd = new SqlCommand(sql, conn);
                //指定参数
                cmd.Parameters.Add(new SqlParameter("@Title", txtTitle.Text));
                //执行,返回影响行数
                int rows = cmd.ExecuteNonQuery();
                MessageBox.Show("删除成功" + rows + "行！");
            }
        }

        /// <summary>
        /// 查询单行单列的值
        /// </summary>
        private void btnScalar_Click(object sender, EventArgs e)
        {
            GetCount();
        }

        private void GetCount()
        {
            //创建连接对象，并使用using释放（关闭），连接用完后会被自动关闭
            using (SqlConnection conn = new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                //打开连接
                conn.Open();
                //将执行的sql
                String sql = "select COUNT(*) from Car";
                //创建命令对象，指定要执行sql语句与连接对象conn
                SqlCommand cmd = new SqlCommand(sql, conn);
                //执行查询返回单行单列的值，Object类型
                Object result = cmd.ExecuteScalar();
                //显示结果到标签
                lblCount.Text = result.ToString();
            }
        }
    }
}
```

运行结果：

 ![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023161315754-1968733520.png)

可能返回NULL值，需要对结果进行判断，如下：

```c#
object my = cmd.ExecuteScalar();
if (object.Equals(my,null))  //可以使用Equals进行Null值的判断，易读性强
  Console.WriteLine("Not Data");
else
  Console.WriteLine("Yes"); 
```

# 五、**ExecuteReader获得数据**

ExecuteReader用于实现只进只读的高效数据查询。

ExecuteReader：返回一个SqlDataReader对象，可以通过这个对象来检查查询结果，它提供了只进只读的执行方式，即从结果中读取一行之后，移动到另一行，则前一行就无法再用。有一点要注意的是执行之后，要等到手动去调用Read()方法之后，DataReader对象才会移动到结果集的第一行，同时此方法也返回一个Bool值，表明下一行是否可用，返回True则可用，返回False则到达结果集末尾。

使用DataReader可以提高执行效率，有两种方式可以提高代码的性能：

一种是基于序号的查找

一个是使用适当的Get方法来查找。因为查询出来的结果一般都不会改变，除非再次改动查询语句，因此可以通过定位列的位置来查找记录。用这种方法有一个问题，就是可能知道一列的名称而不知道其所在的位置，这个问题的解决方案是通过调用DataReader 对象的GetOrdinal()方法，此方法接收一个列名并返回此列名所在的列号。

## 5.1、使用ExecuteReader实现数据查询

示例代码：

```c#
        //查询
        private void btnQuery_Click(object sender, EventArgs e)
        {
            //创建连接对象，并使用using释放（关闭），连接用完后会被自动关闭
            using (SqlConnection conn = new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                //打开连接
                conn.Open();
                //将执行的sql
                String sql = "select Id,Title,Speed,Info from Car";
                //创建命令对象，指定要执行sql语句与连接对象conn
                SqlCommand cmd = new SqlCommand(sql, conn);
                //执行查询返回结果集
                SqlDataReader sdr = cmd.ExecuteReader();
                //下移游标，读取一行，如果没有数据了则返回false
                while (sdr.Read())
                {
                    Console.WriteLine("编号：" + sdr["Id"] + "，车名：" + sdr["Title"] + "，速度：" + sdr["Speed"]);
                }
            }
        }
```

运行结果：

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171024101625973-1892314878.png)

## 5.2、实体类

实体类用于封装及映射数据。

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace MyCar
{
    /// <summary>
    /// 汽车实体类
    /// </summary>
    public class Car
    {
        /// <summary>
        /// 编号
        /// </summary>
        public int Id { get; set; }
        /// <summary>
        /// 车名
        /// </summary>
        public String Title { get; set; }
        /// <summary>
        /// 速度
        /// </summary>
        public int Speed { get; set; }
        /// <summary>
        /// 详细
        /// </summary>
        public String Info { get; set; }
    }
}
```

## 5.3、DataGridView展示数据

示例代码：

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;

using System.Data.SqlClient;

namespace MyCar
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            GetCount();
            BindData();

            List<User> users = new List<User>();

            User tom = new User();
            tom.Name = "Tom";
            tom.Age = "18";
            users.Add(tom);

            User rose = new User();
            rose.Name = "Rose";
            rose.Age = "88";
            users.Add(rose);

            dataGridView1.DataSource = users;
        }

        private void btnConnection_Click(object sender, EventArgs e)
        {
            //创建连接对象，指定连接字符串参数
            SqlConnection conn = new SqlConnection("Data Source=.;Initial Catalog=MyCar;User Id=sa;Password=sa;");
            //打开数据
            conn.Open();
            MessageBox.Show("打开成功，状态" + conn.State);
            conn.Close();
            MessageBox.Show("关闭数据库成功");
        }

        //新增
        private void btnAdd_Click(object sender, EventArgs e)
        {
            //创建连接对象，并使用using释放（关闭），连接用完后会被自动关闭
            using (SqlConnection conn = new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                //打开连接
                conn.Open();
                //将执行的sql
                String sql = "INSERT INTO Car(Title ,Speed ,Info) VALUES(@Ttile,@Speed,@Info)";
                //创建命令对象，指定要执行sql语句与连接对象conn
                SqlCommand cmd = new SqlCommand(sql, conn);
                //指定参数
                cmd.Parameters.Add(new SqlParameter("@Ttile", txtTitle.Text));
                cmd.Parameters.Add(new SqlParameter("@Speed", txtSpeed.Text));
                cmd.Parameters.Add(new SqlParameter("@Info", txtInfo.Text));
                //执行,返回影响行数
                int rows = cmd.ExecuteNonQuery();
                if (rows > 0) { MessageBox.Show("新增成功！"); GetCount(); BindData(); }
            }
        }

        /// <summary>
        /// 删除
        /// </summary>
        private void btnDelete_Click(object sender, EventArgs e)
        {
            //创建连接对象，并使用using释放（关闭），连接用完后会被自动关闭
            using (SqlConnection conn = new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                //打开连接
                conn.Open();
                //将执行的sql
                String sql = "delete from Car where Title=@Title";
                //创建命令对象，指定要执行sql语句与连接对象conn
                SqlCommand cmd = new SqlCommand(sql, conn);
                //指定参数
                cmd.Parameters.Add(new SqlParameter("@Title", txtTitle.Text));
                //执行,返回影响行数
                int rows = cmd.ExecuteNonQuery();
                MessageBox.Show("删除成功" + rows + "行！");
            }
        }

        /// <summary>
        /// 查询单行单列的值
        /// </summary>
        private void btnScalar_Click(object sender, EventArgs e)
        {
            GetCount();
        }

        private void GetCount()
        {
            //创建连接对象，并使用using释放（关闭），连接用完后会被自动关闭
            using (SqlConnection conn = new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                //打开连接
                conn.Open();
                //将执行的sql
                String sql = "select COUNT(*) from Car";
                //创建命令对象，指定要执行sql语句与连接对象conn
                SqlCommand cmd = new SqlCommand(sql, conn);
                //执行查询返回单行单列的值，Object类型
                Object result = cmd.ExecuteScalar();
                //显示结果到标签
                lblCount.Text = result.ToString();
            }
        }

        //查询
        private void btnQuery_Click(object sender, EventArgs e)
        {
            BindData();
        }

        private void BindData()
        {
            //定义一个集合，用于存放汽车对象
            List<Car> cars = new List<Car>();

            //创建连接对象，并使用using释放（关闭），连接用完后会被自动关闭
            using (SqlConnection conn = new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                //打开连接
                conn.Open();
                //将执行的sql
                String sql = "select Id,Title,Speed,Info from Car";
                //创建命令对象，指定要执行sql语句与连接对象conn
                SqlCommand cmd = new SqlCommand(sql, conn);
                //执行查询返回结果集
                SqlDataReader sdr = cmd.ExecuteReader();
                //下移游标，读取一行，如果没有数据了则返回false
                while (sdr.Read())
                {
                    //每一行记录表示一辆车，则实例化一个汽车对象
                    Car car = new Car();
                    car.Id = Convert.ToInt32(sdr["Id"]);  //取得数据库中当前行的Id转换成int类型给对象的Id属性赋值
                    car.Title = sdr["Title"] + "";
                    car.Speed = Convert.ToInt32(sdr["Speed"]);
                    car.Info = sdr["Info"] + "";
                    cars.Add(car);  //将汽车对象添加到集合中
                }
                //绑定数据到控件
                dgvCar.DataSource = cars;
                sdr.Close();  //关闭
            }
        }
    }
}
```

运行结果：

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171024111639816-143590460.png)

## 5.4、删除功能

示例代码：

```c#
        /// <summary>
        /// 删除
        /// </summary>
        private void btnDelete_Click(object sender, EventArgs e)
        {
            //SelectedRows选中的行,[0]行，[0]列，Value值
            int id = Convert.ToInt32(dgvCar.SelectedRows[0].Cells[0].Value);
            //创建连接对象
            using (SqlConnection conn = new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                conn.Open();  //打开连接
                string sql = "delete from Car where Id=@Id";
                SqlCommand cmd = new SqlCommand(sql, conn);  //sql命令对象
                cmd.Parameters.Add(new SqlParameter("@Id",id));  //指定参数
                int rows = cmd.ExecuteNonQuery();  //执行并返回影响行数
                MessageBox.Show("删除成功"+rows+"行！");
                BindCar();  //重新绑定
            }
        }
```

运行结果：

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171024153749348-623150887.png)

## 5.5、编辑功能

示例代码：

FormCar.cs编辑按钮

```c#
        /// <summary>
        /// 编辑
        /// </summary>
        private void btnEdit_Click(object sender, EventArgs e)
        {
            //获得当前选择行的索引
            int index = dgvCar.SelectedRows[0].Index;
            //从集合中获得索引对应的汽车对象
            Car car = cars[index];

            FormEdit edit = new FormEdit();
            edit.car = car;
            edit.ShowDialog();  //打开模式窗口
            BindCar();  //重新绑定
        }
```

FormEidt.cs代码：

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Data.SqlClient;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;

namespace MyCar
{
    public partial class FormEdit : Form
    {

        /// <summary>
        /// 要编辑的汽车对象
        /// </summary>
        public Car car { get; set; }

        public FormEdit()
        {
            InitializeComponent();
        }

        private void FormEdit_Load(object sender, EventArgs e)
        {
            lblId.Text = car.Id+"";
            txtTitle.Text = car.Title;
            txtSpeed.Text = car.Speed+"";
            txtInfo.Text = car.Info;
        }

        //保存
        private void btnSave_Click(object sender, EventArgs e)
        {
            //创建连接对象
            using (SqlConnection conn = new SqlConnection("server=.;uid=sa;pwd=sa;database=MyCar"))
            {
                conn.Open();  //打开连接
                string sql = "update Car set Title=@Title,Speed=@Speed,Info=@Info where Id=@Id";
                SqlCommand cmd = new SqlCommand(sql, conn);  //sql命令对象
                cmd.Parameters.Add(new SqlParameter("@Id", car.Id));  //指定参数
                cmd.Parameters.Add(new SqlParameter("@Title", txtTitle.Text));  //指定参数
                cmd.Parameters.Add(new SqlParameter("@Speed", txtSpeed.Text));  //指定参数
                cmd.Parameters.Add(new SqlParameter("@Info", txtInfo.Text));  //指定参数
                int rows = cmd.ExecuteNonQuery();  //执行并返回影响行数
                MessageBox.Show("修改成功" + rows + "行！");
            }
        }
    }
}
```

运行结果：

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171024160051160-1603651918.png)

# 六、综合示例

完成一个人事管理系统（HR）中的员工（Emp）管理模块，要求实现如下功能：

## 6.1、创建数据库与表

### 6.1.1、创建HR数据库

```
--创建数据库
create database HR;
```

结果：

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171026094433348-137482140.png)

### 6.1.2、创建Emp员工表

Emp员工表（编号Id、姓名Name、电话Phone、身高Height、备注Memo）

```
--创建数据库
create database HR;
use HR;
--Emp员工表（编号Id、姓名Name、电话Phone、身高Height、备注Memo）
--创建表
create table Emp
(
Id int primary key identity(100000,1),  --编号
Name nvarchar(32) not null,  --姓名
Phone varchar(32), --电话
Height int, -- 身高
Memo ntext --备注
)
--添加数据
insert into Emp(Name,Phone,Height,Memo) values('李天明','13723887780',158,'单身');
--查询
select Id,Name,Phone,Height,Memo from Emp;
--删除
delete from Emp where Id=100000
--修改
update Emp set Name='李地明',Phone='13723887789',Height=149 where Id=100001
```

结果：

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171026095627144-2015889045.png)

## 6.2、创建项目与实体类

### 6.2.1、创建项目

这里同样创建一个WinForms窗体项目

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171026095718473-684990205.png)

### 6.2.2、创建实体类

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace HR.Models
{
    /// <summary>
    /// 员工
    /// </summary>
    public class Emp
    {
        /// <summary>
        /// 编号
        /// </summary>
        public int Id { get; set; }
        /// <summary>
        /// 姓名
        /// </summary>
        public String Name { get; set; }
        /// <summary>
        /// 电话
        /// </summary>
        public String Phone { get; set; }
        /// <summary>
        /// 身高
        /// </summary>
        public int Height { get; set; }
        /// <summary>
        /// 备注
        /// </summary>
        public String Memo { get; set; }

    }
}
```

### 6.2.3、封装数据访问

为了避免重复的数据访问代码，这里我们封装了一个数据库访问工具类：SqlHelper

```c#
using System;
using System.Collections.Generic;
using System.Data.SqlClient;
using System.Data;
using System.Linq;
using System.Text;

namespace HR.Utils
{
    /// <summary>
    /// 用于访问SQLServer数据库的工具类
    /// </summary>
    public class SqlHelper
    {
        /// <summary>
        /// 连接字符串 write once,only once!  
        /// </summary>
        public static String connString = "server=.;uid=sa;pwd=sa;database=HR";


        /// <summary>
        /// 完成增，删，改
        /// </summary>
        /// <param name="sql">将要执行的sql</param>
        /// <param name="ps">可变参数，指定sql中的参数</param>
        /// <returns>影响行数</returns>
        public static int Execute(String sql, params SqlParameter[] ps)
        {
            using (SqlConnection conn = new SqlConnection(connString))
            {
                //打开连接
                conn.Open();
                //创建命令对象，指定sql与连接对象conn
                SqlCommand cmd = new SqlCommand(sql, conn);
                //指定参数
                if (ps != null) cmd.Parameters.AddRange(ps);
                //执行sql命令，返回影响行数
                return cmd.ExecuteNonQuery();
            }
        }

        /// <summary>
        /// 执行查询，返回SqlDataReader，一定要关闭
        /// </summary>
        /// <param name="sql">将要执行的sql</param>
        /// <param name="ps">可变参数，指定sql中的参数</param>
        /// <returns>SqlDataReader结果集</returns>
        public static SqlDataReader Reader(String sql, params SqlParameter[] ps)
        {
            //定义一个连接对象，指定连接字符串using,sa sa MyCar .
            SqlConnection conn = new SqlConnection(connString);
            //打开数据库
            conn.Open();
            //定义命令对象，指定要执行的sql与conn连接参数
            SqlCommand cmd = new SqlCommand(sql, conn);
            //指定参数
            if (ps != null) cmd.Parameters.AddRange(ps);
            //执行SQL查询，返回结果集给sdr，关闭reader时也关闭连接
            return cmd.ExecuteReader(CommandBehavior.CloseConnection);
        }

    }
}
```

调用办法：

增删改：

```c#
            int rows = SqlHelper.Execute("delete from Emp where Id=@id",new SqlParameter("id",100002));
            MessageBox.Show(rows+"");
```

查询：

```c#
            SqlDataReader sdr = SqlHelper.Reader("select * from Emp where Id=@id",new SqlParameter("id",100003));
            if (sdr.Read())
            {
                MessageBox.Show(sdr["Name"]+"");
            }
            sdr.Close();
```

## 6.3、实现展示功能

示例代码：

```c#
        #region 绑定员工信息到网格
        public void BindData()
        {
            emps = new List<Emp>();
            //执行查询获得结果集
            SqlDataReader sdr = SqlHelper.Reader("select Id,Name,Phone,Height,Memo from Emp where Name like @Name",new SqlParameter("@Name",'%'+txtName.Text+"%"));
            while (sdr.Read()) 
            {
                Emp emp = new Emp();
                emp.Id = Convert.ToInt32(sdr["Id"]);
                emp.Name = sdr["Name"] + "";
                emp.Phone = sdr["Phone"] + "";
                emp.Height = Convert.ToInt32(sdr["Height"]);
                emp.Memo = sdr["Memo"] + "";
                emps.Add(emp);
            }
            sdr.Close();
            dgvEmp.DataSource = emps;
        }
        #endregion
```

运行结果：

 ![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171026113458988-1627015964.png)

## 6.4、实现新增功能

示例代码：

按钮事件：

```c#
FormAdd add = new FormAdd();
add.ShowDialog();
BindData();
```

新增窗口：

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;

using HR.Utils;
using System.Data.SqlClient;

namespace HR
{
    public partial class FormAdd : Form
    {
        public FormAdd()
        {
            InitializeComponent();
        }

        private void btnSave_Click(object sender, EventArgs e)
        {
            string sql = "insert into Emp(Name,Phone,Height,Memo) values(@Name,@Phone,@Height,@Memo);";
            int rows = SqlHelper.Execute(sql,
                new SqlParameter("@Name", txtName.Text),
                new SqlParameter("@Phone", txtPhone.Text),
                new SqlParameter("@Height", txtHeight.Text),
                new SqlParameter("@Memo", txtMemo.Text));
            MessageBox.Show("新增成功"+rows+"行!");
        }
    }
}
```

运行结果：

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171026113633629-866688590.png)

## 6.5、实现删除功能

示例代码：

```c#
        #region 删除
        private void btnDelete_Click(object sender, EventArgs e)
        {
            int id =Convert.ToInt32(dgvEmp.SelectedRows[0].Cells[0].Value);
            int rows = SqlHelper.Execute("delete from Emp where Id=@Id",new SqlParameter("@Id",id));
            MessageBox.Show("删除成功"+rows+"行");
            BindData();
        }
        #endregion
```

运行结果：

 ![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171026113706176-260767037.png)

## 6.6、实现编辑功能

示例代码：

按钮事件：

```c#
            //取得索引
            int index=dgvEmp.SelectedRows[0].Index;
            FormEdit edit = new FormEdit();
            edit.emp = emps[index];
            edit.ShowDialog();
            BindData(); 
```

窗体代码：

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;

using System.Data.SqlClient;
using HR.Utils;
using HR.Models;

namespace HR
{
    public partial class FormEdit : Form
    {
        public FormEdit()
        {
            InitializeComponent();
        }

        public Emp emp { get; set; }

        private void btnSave_Click(object sender, EventArgs e)
        {
            string sql = "update Emp set Name=@Name,Phone=@Phone,Height=@Height,Memo=@Memo Where Id=@Id";
            int rows = SqlHelper.Execute(sql,
                new SqlParameter("@Name", txtName.Text),
                new SqlParameter("@Phone", txtPhone.Text),
                new SqlParameter("@Height", txtHeight.Text),
                new SqlParameter("@Memo", txtMemo.Text),
                new SqlParameter("@Id", emp.Id));
                            
            MessageBox.Show("修改成功" + rows + "行!");
        }

        private void FormEdit_Load(object sender, EventArgs e)
        {
            txtHeight.Text = emp.Height + "";
            txtMemo.Text = emp.Memo;
            txtName.Text = emp.Name;
            txtPhone.Text = emp.Phone;
        }
    }
}
```

运行结果：

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171026114152519-1546115984.png)

## 6.7、实现搜索功能

示例代码：

按钮：

```c#
        #region 查询
        private void btnQuery_Click(object sender, EventArgs e)
        {
            BindData();
        }
        #endregion
```

BindData()方法

```c#
        #region 绑定员工信息到网格
        public void BindData()
        {
            emps = new List<Emp>();
            //执行查询获得结果集
            SqlDataReader sdr = SqlHelper.Reader("select Id,Name,Phone,Height,Memo from Emp where Name like @Name",new SqlParameter("@Name",'%'+txtName.Text+"%"));
            while (sdr.Read()) 
            {
                Emp emp = new Emp();
                emp.Id = Convert.ToInt32(sdr["Id"]);
                emp.Name = sdr["Name"] + "";
                emp.Phone = sdr["Phone"] + "";
                emp.Height = Convert.ToInt32(sdr["Height"]);
                emp.Memo = sdr["Memo"] + "";
                emps.Add(emp);
            }
            sdr.Close();
            dgvEmp.DataSource = emps;
        }
        #endregion
```

运行结果：

 ![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171026113728410-2009628031.png)

## 6.8、所有代码与扩展

![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171026121321348-656077150.png)

FormMain.cs

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```c#
using HR.Utils;
using System.Data.SqlClient;

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;

namespace HR
{
    public partial class FormMain : Form
    {
        public FormMain()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
        }

        /// <summary>
        /// 员工管理菜单项
        /// </summary>
        private void tsmiEmp_Click(object sender, EventArgs e)
        {
            FormEmp emp = new FormEmp();
            emp.MdiParent = this;
            emp.Show();
        }
    }
}
```

View Code

FormEmp.cs

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```c#
using HR.Models;
using HR.Utils;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Data.SqlClient;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;

namespace HR
{
    public partial class FormEmp : Form
    {
        List<Emp> emps = null;

        public FormEmp()
        {
            InitializeComponent();
            BindData();
        }

        #region 绑定员工信息到网格
        public void BindData()
        {
            emps = new List<Emp>();
            //执行查询获得结果集
            SqlDataReader sdr = SqlHelper.Reader("select Id,Name,Phone,Height,Memo from Emp where Name like @Name",new SqlParameter("@Name",'%'+txtName.Text+"%"));
            while (sdr.Read()) 
            {
                Emp emp = new Emp();
                emp.Id = Convert.ToInt32(sdr["Id"]);
                emp.Name = sdr["Name"] + "";
                emp.Phone = sdr["Phone"] + "";
                emp.Height = Convert.ToInt32(sdr["Height"]);
                emp.Memo = sdr["Memo"] + "";
                emps.Add(emp);
            }
            sdr.Close();
            dgvEmp.DataSource = emps;
        }
        #endregion

        #region 新增
        private void btnAdd_Click(object sender, EventArgs e)
        {
            FormAdd add = new FormAdd();
            add.ShowDialog();
            BindData();
        }
        #endregion

        #region 删除
        private void btnDelete_Click(object sender, EventArgs e)
        {
            int id =Convert.ToInt32(dgvEmp.SelectedRows[0].Cells[0].Value);
            int rows = SqlHelper.Execute("delete from Emp where Id=@Id",new SqlParameter("@Id",id));
            MessageBox.Show("删除成功"+rows+"行");
            BindData();
        }
        #endregion

        #region 修改
        private void btnEdit_Click(object sender, EventArgs e)
        {
            //取得索引
            int index=dgvEmp.SelectedRows[0].Index;
            FormEdit edit = new FormEdit();
            edit.emp = emps[index];
            edit.ShowDialog();
            BindData();
        }
        #endregion

        #region 查询
        private void btnQuery_Click(object sender, EventArgs e)
        {
            BindData();
        }
        #endregion
    }
}
```

View Code

FormAdd.cs

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;

using HR.Utils;
using System.Data.SqlClient;

namespace HR
{
    public partial class FormAdd : Form
    {
        public FormAdd()
        {
            InitializeComponent();
        }

        private void btnSave_Click(object sender, EventArgs e)
        {
            string sql = "insert into Emp(Name,Phone,Height,Memo) values(@Name,@Phone,@Height,@Memo);";
            int rows = SqlHelper.Execute(sql,
                new SqlParameter("@Name", txtName.Text),
                new SqlParameter("@Phone", txtPhone.Text),
                new SqlParameter("@Height", txtHeight.Text),
                new SqlParameter("@Memo", txtMemo.Text));
            MessageBox.Show("新增成功"+rows+"行!");
        }
    }
}
```

View Code

FormEdit.cs

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;

using System.Data.SqlClient;
using HR.Utils;
using HR.Models;

namespace HR
{
    public partial class FormEdit : Form
    {
        public FormEdit()
        {
            InitializeComponent();
        }

        public Emp emp { get; set; }

        private void btnSave_Click(object sender, EventArgs e)
        {
            string sql = "update Emp set Name=@Name,Phone=@Phone,Height=@Height,Memo=@Memo Where Id=@Id";
            int rows = SqlHelper.Execute(sql,
                new SqlParameter("@Name", txtName.Text),
                new SqlParameter("@Phone", txtPhone.Text),
                new SqlParameter("@Height", txtHeight.Text),
                new SqlParameter("@Memo", txtMemo.Text),
                new SqlParameter("@Id", emp.Id));
                            
            MessageBox.Show("修改成功" + rows + "行!");
        }

        private void FormEdit_Load(object sender, EventArgs e)
        {
            txtHeight.Text = emp.Height + "";
            txtMemo.Text = emp.Memo;
            txtName.Text = emp.Name;
            txtPhone.Text = emp.Phone;
        }
    }
}
```

View Code

扩展：多删除（一次选择多行删除）、多条件组合搜索、分页。

# 七、作业

## 7.1、大作业

实现一个产品管理系统（GoMall），完成对产品(Product)的维护。

1)、创建一个数据库GoMall

2)、定义一个表产口Product（编号Id,名称Name,价格Price,详细Details)，添加5个测试数据

3)、创建一个Winform项目，名称为GoMall

4)、完成展示功能

5)、完成新增功能

6)、完成删除功能

7)、完成修改功能

8)、扩展功能，添加一个类型表，在产品中添加类型外键；实现按名称搜索功能。

## 7.2、第1次小作业

1、使用ADO.NET实现增加，删除操作，要求3个字段以上，同时用拼接字符串与带参数的两种办法。

2、使用ExecuteScalar实现单行单列值的查询。

## 7.3、第2次小作业

1、使用ADO.NET实现“展示”功能。

2、使用ADO.NET实现“删除”功能。

3、使用ADO.NET实现“编辑”功能。

# 八、视频与资料下载

示例：https://coding.net/u/zhangguo5/p/ADO.NET/git

视频：[地址https://www.bilibili.com/video/av15651626/](https://www.bilibili.com/video/av15651626/)

[![img](https://images2017.cnblogs.com/blog/63651/201710/63651-20171023122435582-2000633457.png)](https://www.bilibili.com/video/av15651626/)    