# C#虚函数virtual

#### 介绍

在某基类中声明 virtual 并在一个或多个派生类中被重新定义的成员函数称为虚函数

虚函数的作用就是实现多态性(Polymorphism)，多态性是将接口与实现进行分离

#### 使用

1. 当调用一个对象的函数时，系统会直接去检查这个对象声明定义的类，即声明类，看所调用的函数是否为虚函数。

2. 如果不是虚函数，那么它就直接执行该函数。而如果有virtual关键字，也就是一个虚函数，那么这个时候它就不会立刻执行该函数了，而是转去检查对象的实例类。

3. 在这个实例类里，他会检查这个实例类的定义中是否有重新实现该虚函数（通过override关键字），如果有，则马上执行该实例类中的这个重新实现的函数。而如果没有，系统会不停地往上找实例类的父类，并对父类重复刚才在实例类里的检查，直到找到第一个重载了该虚函数的父类为止，然后执行该父类里重载后的函数。

#### 解释

**此时编译器具体的检查的流程如下**

1. **当调用函数时，系统会直接去检查申明类，看所调用的函数是否为虚函数；**
2. **如果不是，那么它就直接执行该函数。如果是virtual函数，则转去检查对象的实例类。**
3. **在实例类中，若有override的函数，则执行该函数，如果没有，则依次上溯，按照同样步骤对父类进行检查，知道找到第一个override了此函数的父类，然后执行该父类中的函数。**



##### 一、虚方法的概念 

​	在C#中，虚方法就是可以被子类重写的方法，如果子类重写了虚方法，则在运行时将运行重写的逻辑；如果子类没有重写虚方法，则在运行时将运行父类的逻辑。虚函数在编译期间是不被静态编译的，它的相对地址是不确定的，它会根据运行时期对象实例来动态判断要调用的函数，其中那个申明时定义的类叫申明类，那个执行时实例化的类叫实例类。虚方法是用virtual关键字声明的 

```
A a = new B(); 其中A是申明类，B是实例类。
```

##### 二、虚方法的特点 

​	虚方法前不允许有static,abstract,或override修饰符虚方法不能是私有的（因为在子类中要进行重写），所以不能使用private修饰符不能在声明虚方法的同时指定重写虚方法，因为重写方法只能重写基类的虚方法，也就是要提前在基类中声明虚方法，所以virtual关键字和override关键字不能同时使用。 

##### 三、虚方法的作用 

​	允许派生类(即其子类)对父类进行扩展。虚方法是多态特性的一种体现。增加开发过程中的可维护性，条理清晰明了。 

##### 四、虚方法的声明 

​	在父类中: 

```
public virtual 返回类型 方法名()
{
	方法体;
}
```

在子类中: 

```
public override 返回值类型 方法名()
{
	base.父类方法;
	方法体;
}
```

##### 五、虚方法的执行 

一般方法在编译时就静态地编译到了执行文件中，其相对地址在程序运行期间是不发生变化的，也就是写死了的！ 
虚函数在编译期间是不被静态编译的，它的相对地址是不确定的，它会根据运行时期对象实例来动态判断要调用的函数。 
具体的检查流程如下： 
当调用一个对象的方法时，系统会直接去检查这个对象申明定义的类，即申明类，看所调用的方法是否为虚方法。如果不是虚函数，那么它就直接执行该函数。如果有virtual关键字，也就是一个虚方法，那么这个时候它就不会立刻执行该函数了，而是转去检查对象的实例类。在这个实例类里，它会检查这个实例类的定义中是否有实现该虚方法（通过new关键字）或是否有重新实现该虚方法（通过override关键字），如果有，那么OK，它就不会再找了，而马上执行该实例类中的这个重新实现的方法。而如果没有的话，系统就会不停地往上找实例类的父类，并对父类重复刚才在实例类里的检查，直到找到第一个重载了该虚方法的父类为止，然后执行该父类里重载后的方法。 
下面我们通过具体实例看看： 例1： 

```
	class A
    {
        public virtual void Getreturn()
        {
            Console.WriteLine("我是A类的虚方法");
        }
    }
	class Program
    {
        static void Main(string[] args)
        {
            A a = new A();//定义一个a这个A类的对象,这个A就是a的申明类，实例化a对象,A是a的实例类
            a.Getreturn();
            Console.ReadLine();
        }
    }
```


过程：1.先检查申明类A 2.检查到是Getreturn是虚拟方法 3.转去检查实例类A，结果是它本身 4.执行实例类A中实现Getreturn的方法 5.输出结果我是A类的虚方法 
例2： 

```
	class A
    {
        public virtual void Getreturn()
        {
            Console.WriteLine("我是A类的虚方法");
        }
    }
    class B : A
    {
        public override void Getreturn() //重新实现了虚方法 
        {
            Console.WriteLine("我是B类重写后的方法");
        }
    }
	class Program
    {
        static void Main(string[] args)
        {
            A a = new B();//定义一个a这个A类的对象,这个A就是a的申明类，实例化a对象,B是a的实例类
            a.Getreturn();
            Console.ReadLine();
        }
    }
```

过程：1.先检查申明类A 2.检查到是虚拟方法 3.转去检查实例类B，有重写的方法 4.执行实例类B中的方法 5.输出结果我是B类重写后的方法 
例3： 

```
	class A
    {
        public virtual void Getreturn()
        {
            Console.WriteLine("我是A类的虚方法");
        }
    }
    class B : A
    {
        public override void Getreturn() //重新实现了虚方法 
        {
            Console.WriteLine("我是B类重写后的方法");
        }
    }
```

```c#
    class C : B
    {   
    }
    class Program
    {
        static void Main(string[] args)
        {
            A a = new C();//定义一个a这个A类的对象,这个A就是a的申明类，实例化a对象,C是a的实例类
            a.Getreturn();
            Console.ReadLine();
        }
    }
```
过程：1.先检查申明类A 2.检查到是虚拟方法 3.转去检查实例类C，无重写的方法 4.转去检查类C的父类B，有重写的方法5.执行父类B中的Getreturn方法 6.输出结果我是B类重写后的方法 
例4： 	

```c#
class A
    {
        public virtual void Getreturn()
        {
            Console.WriteLine("我是A类的虚方法");
        }
    }
    class B : A
    {
        public new void Getreturn() //覆盖父类里的同名函数，而不是重新实现  
        {
            Console.WriteLine("我是B类New方法");
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            A a = new B();//定义一个a这个A类的对象,这个A就是a的申明类，实例化a对象,B是a的实例类
            a.Getreturn();
            Console.ReadLine();
        }
    }
```

过程：1.先检查申明类A 2.检查到是虚拟方法 3.转去检查实例类B，无重写的（这个地方要注意了，虽然B里有实现Getreturn()，但没有使用override关键字，所以不会被认为是重写） 4.转去检查类B的父类A，就是它本身 5.执行父类A中的Getreturn方法 6.输出结果我是A类的虚方法 
例5： 

```c#
class A
    {
        public virtual void Getreturn()
        {
            Console.WriteLine("我是A类的虚方法");
        }
    }
    class B : A
    {
        public new void Getreturn() //覆盖父类里的同名函数，而不是重新实现  
        {
            Console.WriteLine("我是B类New方法");
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            B b = new B();//定义一个b这个B类的对象,这个B就是b的申明类，实例化b对象,B是b的实例类
            b.Getreturn();
            Console.ReadLine();
        }
    }
```

过程：1.先检查申明类B 2.检查到不是虚拟方法 3.执行B类里的Getreturn() 4.输出结果我是B类New方法 
可以使用抽象方法重写基类中的虚方法 

```c#
	class A
    {
        public virtual void Getreturn()
        {
            Console.WriteLine("A类的虚方法");   
        }  
    }
    abstract class B : A    
    {
        public abstract override void Getreturn();//使用override修饰符,表示抽象重写了基类中该函数的实现
    }
    abstract class C : A
    {
        public abstract new void Getreturn();//使用new修饰符显式声明,表示隐藏了基类中该函数的实现
    }
```


密封类可以重写基类中的虚方法(基类中的虚方法将隐式的转化为非虚方法，但密封类本身不能再增加新的虚方法) 
	

```c#
class A
    {
        public virtual void Getreturn()
        {
            Console.WriteLine("A类的虚方法");
        }
    }
    sealed class Program:A
    {
        public override void Getreturn()
        {
            Console.WriteLine("Program类重写后的方法");
        }
        static void Main(string[] args)
        {
            Program p = new Program();
            p.Getreturn();
            Console.ReadLine();
        }
    }
```

##### 六、虚拟类的规则 

虚拟类其实指的是正常类中的虚拟方法，所以虚拟类可以直接使用实例虚拟方法是在方法前加virtual关键