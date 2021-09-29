# C# get和set访问器

###### 																															获取和设置字段（属性）的值

定义属性的语法形式如下

```c#
public    数据类型    属性名
{
    get
    {
        获取属性的语句块;
        return 值;
    }
    set
    {
        设置属性得到语句块;
    }
}
```

用于获取私有成员变量

##### Q&A

###### ***Q***: .net中有 public static ConnectionStringSettingsCollection ConnectionStrings { get; }怎么解释?  

***A***:此为简略写法

```c#
class Student
{
    public Student(string name, int id)
    {
        this.Name = name;
        this.Id = id;
    }
    //这个写法是可读写自动属性
    public string Name{get; set;}
    //这个写法是只读自动属性
    public string Id{get;}
}
```

```c#
class Student
{
    private string _naem;
    private int _id;
    public Student(string name, int id)
    {
        this._name = name;
        this._id = id;
    }
    
    public string Name 
    {
        get{ return this._name;} 
        set { this._name = value;}
    }
    public string Id
    {
      get { return this._id; }
    }
}
```

上下实际上是完全等效的