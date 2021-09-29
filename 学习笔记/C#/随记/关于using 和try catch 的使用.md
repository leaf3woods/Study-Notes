## 关于using 和try catch 的使用

```c#
private DataTable GetMenuToDataTable(string query, DataTable dt) 
{
    DataBase DB = new DataBase();

    using (SqlConnection conn = new SqlConnection(DB.ConnStr)) 
        { 
        SqlCommand cmd = new SqlCommand(query, conn); 
        SqlDataAdapter ada = new SqlDataAdapter(cmd); ada.Fill(dt); 
        } 
    return dt; 
}
```

相当于以下代码：

```c#
try
{
	obj = new obj();
...
}
finally
{
	obj.dispose();
}
```

1.对象需包含dispose()方法；

2.打开数据库或者访问文件等时候需要打开资源，这个时候你要用using包括资源声明那么clr会为你自动生成try语句防止内存无法收回。

3.定义一个对象生命范围，在范围结束时处理对象。(不过该对象必须实现了IDisposable接口)。其功能和try ,catch,Finally完全相同。
        比如:
        using (SqlConnection cn = new SqlConnection(SqlConnectionString)){......}//数据库连接
        using (SqlDataReader dr = db.GetDataReader(sql)){......}//DataReader

在结束大括号处会关闭并释放这个对象

