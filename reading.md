### C++基础

#### sizeof关键字

sizeof 是 C 语言中的运算符，用来计算一个类型/对象的所占用的内存大小

计算技巧：

- **指针的大小永远是固定的**，取决于处理器位数，32位就是 4 字节，64位就是 8 字节

- 数组作为函数参数时会退化为指针，大小要按指针的计算

- struct 结构体要考虑字节对齐，因为结构体或类成员变量具有**具有不同类型**时，需进行**成员变量的对齐**，目的是**减少访存指令周期，提高CPU存储速度**

  - 例：

  ```c++
  struct MyStruct {
      int a; //4，跟最长的对齐，+1
      double b; //8
      char c; //1，跟最长的对齐，+3
  };
  sizeof(MyStruct) == 24; //8+8+8
  ```

- 字符串数组要算上末尾的 '\0'

#### sizeof 和 strlen

- strlen是一个 C 标准库中的函数，用于计算 C 风格字符串（以空字符 '\0' 结尾的字符数组）的长度，即**不包括结尾的空字符**的字符个数
- sizeof是C++编译期间计算的操作符，用于计算数据类型或对象所占用的字节数

#### const关键字

1. 修饰变量：当const修饰变量时，该变量被视为只读，即不能被修改

2. 修饰函数参数：当const修饰函数参数时，表示函数内部不会修改该参数的值，可以使代码更加安全，尤其是引用作参数时，如果确定不会修改引用，那就一定要使用const引用

3. 修饰函数返回值：当 const修饰函数返回值时，表示函数的返回值为只读，不能被修改

4. 修饰指针或引用：用于声明指针本身为只读变量或者指向只读变量的指针

   **指针常量**：修饰的是指针所指向的变量，而不是指针本身，即指针本身可以被修改，但不能通过指针修改所指向的变量`const int* p;`

   **常量指针**：修饰的是指针本身，使得指针本身成为只读变量`int* const p;`

   只读指针指向只读变量：同时修饰了指针本身和指针所指向的变量，使得指针本身和所指向的变量都成为只读变量

   常量引用：引用一个只读变量的引用，因此不能通过常量引用修改变量的值`const int&b = a;`

5. 修饰成员函数：表示该函数不会修改对象的状态（就是不会修改成员变量）

#### static关键字

1. 修饰全局变量：将变量的作用域限定在当前文件中，使得其他文件无法访问该变量
2. 修饰局部变量：使得变量在函数调用结束后不会被销毁，而是一直存在于内存中，下次调用该函数时可以继续使用；同时，static修饰的局部变量的作用域仅限于函数内部，所以其他函数内部无法访问该变量
3. 修饰函数：将函数的作用域限定在当前文件中，使得其他文件无法访问该函数
4. **修饰成员变量和函数**：可以使得它们在所有类对象中共享，且不需要创建对象就可以直接访问（静态成员变量和函数属于类而不是类对象）

#### volatile关键字

用于修饰变量，提醒编译器使用 volatile 声明的变量随时有可能改变，因此编译器在代码编译时就不会对该变量进行某些激进的优化，故而编译生成的程序在每次存储或读取该变量时，都会直接从内存地址中读取数据

volatile声明的变量三种特性：**易变的、不可优化的、顺序执行的**

#### 字节序

- 大端：高位低地址，低位高地址（符合人类阅读习惯），也是网络字节序
- 小端：低位低地址，高位高地址，大多数的内存字节序

#### struct和class的区别

在C++中，struct 类似于 class，既可以包含成员变量，又可以包含成员函数

不同点：

- **class 中类中的成员默认都是 private 属性的**
- **struct 中结构体中的成员默认都是 public 属性的**
- **class 继承默认是 private 继承，而 struct 继承默认是 public 继承**
- **class 可以用于定义模板参数，struct 不能用于定义模板参数**

#### 宏定义（define）和内联函数（inline）的区别

define：

- 用于定义常量和宏函数，编译时只是做文本替换，不涉及类型检查，容易导致错误

inline：

- 在函数前面加上inline关键字，编译器会将内联函数调用处用函数体进行替换，避免了函数调用的开销。优点是类型安全、可调试、可优化，但函数体会被复制多次，占用代码空间，可能导致代码膨胀以及编译时间增加，可执行二进制文件过大

#### 宏定义（define）和typedef的区别

1. 语法和实现机制：define预处理器只做文本替换，不做类型检查；typedef是在编译阶段处理的，有更严格的类型检查
2. 作用域限制：宏定义没有作用域限制，只要在宏定义之后的地方，就可以使用宏；typedef 遵循 C++ 的作用域规则，可以受到命名空间、类等结构的作用域限制
3. 模板支持：宏定义不支持模板，因此不能用于定义模板类型别名；typedef可以与模板结合使用

#### explicit的作用

通常用于构造函数的声明中，用于防止隐式类型转换。当将一个参数传递给构造函数时，如果构造函数声明中使用了 explicit 关键字，则只能使用显式转换进行转换，而不能进行隐式转换

#### extern的作用

extern 用于指示变量或函数的定义在另一个源文件中，并在当前源文件中声明。 说明该符号具有外部链接(external linkage)属性

#### mutable的作用

用于修饰类的成员变量，表示该成员变量即使在一个const成员函数中也可以被修改

#### 几种类型转换

static_cast：`static_cast<new type>(expression)`，和 C 语言 () 做强制类型转换基本是等价的

dynamic_cast：`dynamic_cast<new type>(expression)`，主要应用于父子类层次结构中的安全类型转换

主要应用场景：

- **向下类型转换**，即将基类指针转为派生类或引用，dynamic_cast可以确保类型兼容性，若转换失败可以返回空指针或抛出异常
- 用于多态类型检查：处理多态对象时，dynamic_cast可以用来确定对象的实际类型，要使dynamic_cast有效，**基类至少需要一个虚函数**，因为只有在基类存在虚函数(虚函数表)的情况下才有可能将基类指针转化为子类

const_cast：`dynamic_cast<new type>(expression)`，new_type必须是一个指针、引用或者指向对象类型成员的指针

主要应用场景：

- 修改const对象：删除const属性
- const对象调用非const成员函数

reinterpret_cast：用于在不同类型之间做低级别的转换，主要使指针类型之间的转换

#### auto关键字推导的规则

用法：根据初始化表达式自动推断声明变量的类型

```c++
auto f=3.14;      //double
auto s("hello");  //const char*
auto z = new auto(9); // int*
auto x1 = 5, x2 = 5.0, x3='r';//错误，必须是初始化为同一类型
std::vector<int> vect; 
for(auto it = vect.begin(); it != vect.end(); ++it)
{  //it的类型是std::vector<int>::iterator
   std::cin >> *it;
 }
```

规则：如果初始化表达式是引用，首先去除引用；然后，如果剩下的初始化表达式有顶层的const/volatile，也去除

如果auto关键字带&则声明为引用类型，不去除

### C++面向对象

#### 面向对象三大特性

- 封装
- 继承
- 多态

#### 重载、重写和隐藏的区别

重载：指相同作用域内拥有相同的方法名，但具有不同的参数类型或数量的方法。重载根据参数不同来调用不同的函数

重写：派生类重新定义基类的方法

隐藏：派生类的函数屏蔽了与其同名的基类函数

#### 深拷贝和浅拷贝

浅拷贝：仅复制对象的基本类型成员和指针成员的值，而不复制指针所指向的内存，可能导致两个对象共享相同的资源，导致内存泄漏。浅拷贝即直接按bit位复制，基本等价于memcpy()函数

深拷贝：不仅复制对象的基本类型成员和指针成员的值，还复制指针所指向的内存，因此两个对象不会共享相同的资源。深拷贝通常需要显式实现拷贝构造函数和赋值运算符重载

#### C++多态的实现方式

多态：同一个函数或者操作在不同的对象上有不同的表现形式

C++多态的实现方式：**虚函数、纯虚函数和模板函数**

虚函数、纯虚函数实现的多态叫**动态多态**，模板函数、重载等实现的叫**静态多态**

动态多态必须满足两个条件：

- 通过基类指针或引用调用虚函数
- 被调用的函数是虚函数，必须完成对基类虚函数的重写

例：

```cpp
class Shape {
   public:
      virtual int area() = 0;
};

class Rectangle: public Shape {
   public:
      int area () { //子类重写父类虚函数
         cout << "Rectangle class area :"; 
         return (width * height); 
      }
};

class Triangle: public Shape{
   public:
      int area () { 
         cout << "Triangle class area :"; 
         return (width * height / 2); 
      }
};

int main() {
   Shape *shape;
   Rectangle rec(10,7);
   Triangle  tri(10,5);

   shape = &rec; //父类指针指向子类对象
   shape->area();

   shape = &tri;
   shape->area();

   return 0;
}
```

模板函数可以根据传递参数的不同类型，自动生成相应类型的函数代码，调用的函数**在编译期就确定下来的叫做静态多态**

#### this指针

指向当前对象的指针，可以用来访问类的成员变量和成员函数，以及在成员函数中引用当前对象

static函数不能访问成员变量：因为static函数是静态成员函数，属于类而不是类对象，没有this指针，不能访问任何非静态成员变量

#### C++虚函数

虚函数定义：在基类定义的一个未实现的函数，一般加上virtual关键字，目的是通过基类访问派生类定义的函数

虚函数表：每个类都有一个虚函数表，其中包含了该类中所有虚函数的地址。当一个类包含虚函数时，编译器会在该类的内部生成一个虚函数表。虚函数表是在程序运行时创建的，用于动态地查找和调用正确的虚函数版本

虚表指针：一个指向虚函数表的指针。它是每个对象的隐藏成员之一，用于指示该对象所属的类的虚函数表。当通过指向基类的指针调用虚函数时，实际上是通过该指针指向的对象的虚表指针来查找和调用对应的虚函数

动态多态底层原理：**C++动态多态性是通过虚函数实现的**，当基类指针或引用指向一个派生类对象时，调用虚函数时，实际上会调用派生类中的虚函数，而不是基类中的虚函数

#### C++纯虚函数

定义：基类中声明但没有实现的虚函数，作用是定义了一种接口，需要派生类来实现

**包含纯虚函数的类称为抽象类**

虚函数不代表函数为不被实现的函数，是**为了允许基类的指针来调用子类的这个函数**，纯虚函数才是没有被实现的函数

### C++内存管理

#### 内存是什么

内存就是计算机的存储空间，用于存储程序的指令、数据和状态，在C语言中，内存被组织成一系列的字节，每个字节有唯一的地址，变量和数据结构存储在这些字节中

#### C++内存分区

代码区、全局/静态存储区、栈区、堆区和常量区

#### 指针和引用的区别

1. 指针是一个变量，保存了另一个变量的地址；引用是另一个变量的别名，与原变量共享内存地址
2. 指针可以被重新赋值；引用初始化后不能更改
3. 指针可以为nullptr；引用必须绑定一个变量
4. 指针可以解引用获取或修改其指向变量的值；引用无需解引用

#### 指针传递、值传递、引用传递

值传递：将实参的值传递给形参，函数内对形参的修改不会影响到实参

指针传递：将实参的地址传递给形参，函数内对形参的修改会影响到实参

引用传递：将实参的引用传递给形参，函数内对形参的修改会影响到实参

#### 智能指针

- unique_ptr：独占所有权的智能指针，保证指向的内存只能由一个unique_ptr拥有，不能共享，当unique_ptr超出作用域时，所指向的内存会自动释放
- shared_ptr：共享所有权的智能指针，允许多个shared_ptr指向同一个对象，当最后一个shared_ptr超出作用域时，所指向的内存才会释放

#### weak_ptr

弱引用智能指针，主要作用是解决循环引用问题、观察hared_ptr对象而不影响引用计数，以及在需要时提供对底层资源的访问

1. 解决循环引用问题：当两个或多个shared_ptr对象互相引用时，会导致循环引用，这种情况下引用计数永远不会为0，从而导致内存泄漏
2. 观察shared_ptr对象：监视shared_ptr对象的生命周期，它不会增加引用计数，因此不会影响资源的释放

#### malloc、new、free、delete的区别

简单对比：

- malloc/free是C语言函数，new/delete是C++运算符
- malloc只分配内存，new会分配内存并调用对象的构造函数来初始化对象
- malloc返回一个void*指针，new返回一个指向对象类型的指针

#### 野指针、空悬指针

野指针：未被初始化或已被释放的指针，值是不确定的，可能指向任意内存地址

空悬指针：指向已经被释放的内存的指针，仍然具有以前分配的内存地址，但这块内存可能被其他对象或数据占用

### 设计模式

#### 单例模式及其实现

单例模式：保证类的实例化对象只有一个，并提供一个它的全局访问点

应用场景：

- 表示文件系统的类
- 打印机打印程序的实例（同时只能有一个打印作业输出到打印机）

实现方式：

通过全局或静态变量的形式实现

- **默认的构造函数、拷贝构造函数、赋值构造函数声明为私有的**，这样禁止在类的外部创建该对象
- 全局访问点也要定义成静态类型的成员函数，没有参数，返回该类的指针类型。因为使用实例化对象的时候是通过类直接调用该函数，并不是先创建一个该类的对象，通过对象调用

分类：

- 懒汉模式：知道第一次用到类的实例时才去实例化（需要用到互斥锁或者内部静态变量来保证线程安全）
- 饿汉模式：类定义的时候就实例化（线程安全）

饿汉模式实现：

```c++
class Singleton {
public:
	Singleton(const Singleton& temp) = delete;
	Singleton& operator=(const Singleton& temp) = delete;
	static Singleton *GetInstance()
	{
		return &singleton;
	}
private:
	static Singleton singleton;
	Singleton() {};
	~Singleton() {};
};
Singleton Singleton::singleton;
```

#### 工厂模式及其实现

工厂模式：分为简单工厂模式、工厂方法模式、抽象工厂模式

简单工厂模式：

- 主要用于创建对象。用一个工厂来根据输入的条件产生不同的类，然后根据不同类的虚函数得到不同的结果
- 应用场景：适用于不同的情况创建不同类，只需传入工厂类的参数即可，无需了解具体实现方法。

- 三个主要角色：工厂类、抽象产品、具体产品
  - 抽象产品：描述产品的通用行为
  - 具体产品：实现抽象产品接口或继承抽象产品类
  - 工厂类：创建产品，根据传递的不同参数创建不同的产品实例

工厂方法模式：

- 在简单工厂模式基础上，引入了抽象工厂和具体工厂的概念，每个具体工厂只负责创建一个具体产品，添加新的产品只需添加新的工厂类而无需修改原来的代码
- 四个角色：抽象工厂、具体工厂、抽象产品、具体产品
  - 抽象工厂：一个接口，包含一个抽象的工厂方法
  - 具体工厂：实现抽象工厂接口，创建具体产品
  - 抽象产品：定义产品的接口
  - 具体产品：实现抽象产品接口，是工厂创建的对象

抽象工厂模式：

- 提供了一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。包含多个抽象产品接口，多个具体产品类，一个抽象工厂接口和多个具体工厂，每个具体工厂负责创建一组相关的产品
- 抽象产品接口：定义产品的接口，可以定义多个抽象产品接口，比如说沙发、椅子、茶几都是抽象产品
- 具体产品接口：实现抽象产品接口，产品的具体实现，古典风格和沙发和现代风格的沙发都是具体产品
- 抽象工厂接口：声明一组用于创建产品的方法，每个方法对应一个产品
- 具体工厂类：实现抽象工厂接口，负责创建一组具体产品的对象

区别：

- 简单工厂模式：一个工厂方法创建所有具体产品
- 工厂方法模式：一个工厂方法创建一个具体产品
- 抽象工厂模式：一个工厂方法创建一类具体产品

#### 观察者模式及其实现

观察者模式（发布-订阅模式）：一种一（主题）对多（观察者）的关系，让多个观察对象能同时监听一个被观察对象，被观察对象状态发生变化时，会通知所有的观察对象，使他们能够更新自己的状态

两种角色：

- 观察者：内部包含被观察者对象，当被观察者对象的状态发生变化时，更新自己的状态（接收通知更新状态）
- 被观察者：内部包含所有观察者对象，当状态发生变化时通知所有的观察者更新自己的状态（发送通知）

应用场景：**一个对象的状态变化会影响到其他对象，并且希望这些对象在状态变化时能够自动更新的情况**

- 当一个对象的改变需要同时改变其他对象，且不知道具体有多少对象有待改变时，应该考虑使用观察者模式
- 一个抽象模型有两个方面，其中一方面依赖于另一方面，这时可以用观察者模式将这两者封装在独立的对象中使它们各自独立地改变和复用

四个角色：

- 主题：一般是一个接口，提供方法用于**注册、删除和通知观察者**，通常也包含一个状态，当状态发生改变时，通知所有的观察者
- 观察者：实现一个接口，包含一个更新方法，在接收主题通知时执行对应的操作
- 具体主题：主题的具体实现，维护一个观察者列表，包含了观察者的注册、删除和通知方法
- 具体观察者：观察者接口的具体实现，每个具体观察者都注册到具体主题中，当主题状态变化并通知到具体观察者，具体观察者进行处理

### Django

#### Django的架构模式

Django遵循MVT（模型视图模板）架构模式

- Model：负责业务对象与数据库的映射（ORM）
- View：根据用户的请求返回相关模板和内容的请求处理程序
- Template：包含网页布局的文本文件（如 HTML 文件），以及有关如何显示数据的逻辑

还有一个urls分发器，将一个个url请求分发给不同的view处理，view再调用相应的model和template

#### Django、Flask、FastAPI的对比

- Django：全功能的web框架，基于MVT的开发环境。内置了很多功能，包括ORM、表单处理、认证系统、管理后台等。适合构建大型和复杂的web应用程序
- Flask：轻量级web框架，注重简洁和灵活性。核心基于Werkzeug WSGI工具和Jinja2模板，相比于Django提供了强大的扩展机制，适合构建小型或中型以及需要高度定制的项目
- FastAPI：专注于构建API，显著特点是能够自动生成交互式的API文档，适合构建高性能的API服务，尤其适合处理大量请求的情况

#### 什么是wsgi、uwsgi、uWSGI

wsgi：Web Server Gate Interface，web服务器网关接口。是python的一种web服务器和应用框架之间的简单通用的接口标准规范，即规定了请求的url到后台处理函数之间该如何实现，Django、Flask都是再wsgi之上抽象了一层，使得更加专注于业务设计，避免过多地耗费精力在网络协议之间解析和转换

实现wsgi协议的模块：

- wsgiref：本质上是一个socket服务端，用于接收用户请求（django）
- werkzeug：本质上是一个socket服务端，用于接收用户请求（flask）

uwsgi：也是一种通信协议，是uWSGI服务器的独占协议，用于定义传输信息的类型

uWSGI：是一个web服务器，实现了WSGI协议、uWSGI协议和http协议

![](images\image-20240530154811260.png)

#### Django请求的生命周期

1. wsgi，封装请求后交给web框架
2. 中间件，对请求进行校验或在请求对象中添加其他相关数据，如csrf、request.session等
3. 路由匹配，根据浏览器发送的url去匹配响应的视图函数
4. 视图函数，在视图函数中进行业务逻辑处理，可能设计ORM、template渲染
5. 中间件，对响应的数据进行处理
6. wsgi，发送响应请求到浏览器

![](images\image-20240530162643807.png)

#### Django MIDDLEWARE的作用和应用场景

中间件是介于request和response处理之间的一道处理过程，用于在全局范围内改变Django的输入和输出，简单来说就是帮助我们在视图函数执行之前修改HttpRequest对象，执行之后修改返回的HttpResponse对象

例如：

- Django项目中默认启用了csrf保护，每次请求时通过csrf中间件检查请求中是否有正确的token值
- 当用户在页面上发送请求时，可以通过自定义的中间件判断用户是否登录，未登录则去登录

#### 列举Django中间件的常用方法

- process_request：当一个http请求到达Django时，会调用这个方法。可以执行一些预处理逻辑，例如身份验证、日志记录、请求数据的修改等
- process_view：确定了将要使用的视图函数之后被调用
- process_response：视图处理完请求并生成响应时，会调用这个方法，可以对响应进行处理，例如添加额外的头部信息、修改内容等

#### Django重定向如何实现，用的什么状态码

使用HttpResponseRedirect：`from django.http import HttpResponseRedirect`

状态码301、302：

- 相同点：都表示重定向，浏览器拿到服务器返回的这个状态码后会自动跳转到一个新的url地址
- 不同点：
  - 301是永久重定向，表示旧地址的资源已经永久转移了
  - 302是临时重定向，表示旧地址的资源还在，可以访问

#### Django中CSRF保护机制是什么，怎么实现

CSRF（Cross-site request forgery）跨站请求伪造，是一种网络攻击，**攻击者利用用户已经登录的身份向网站发送恶意请求**。

1. 生成csrf token：当用户访问包含表单的页面时，Django会生成一个唯一的csrf token，并将其存储在session状态以及cookie中，这个token通常是随机生成的字符串
2. 表单中包含csrf token：在处理包含表单的页面时，Django会将生成的csrf token添加到表单中作为隐藏字段或自定义的请求头，当用户提交表单时将这个token一起提交到服务器
3. 验证csrf token：在服务器端接收到请求后，Django 自动检查请求中的 csrf token是否与用户会话中存储的token匹配

#### Django本身提供了runserver，为什么不能用来部署

- runserver是调试Django时经常用到的运行方式，使用的是Django自带的WSGI Server运行，主要在测试和开发中使用，**runserver开启的方式是单进程**
- uWSGI是一个web服务器，实现了WSGI、uWSGI、HTTP等协议，搭配Nginx组成生产环境，能够将用户请求与项目应用隔离，实现真正的网站部署。**这种方式支持的并发量更高，方便管理多进程**，发挥多核优势，提升性能

#### cookie和session的区别

cookie：某些网站为了辨别用户身份，进行session跟踪而存储在用户本地终端上的数据，**由用户客户端计算机暂时或永久保存的信息**

![](images\image-20240603101057535.png)

session：也是一种记录客户端状态的机制，但**session保存在服务器上**，客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上，客户端浏览器再次访问时只需要从该session中查找该客户的状态就可以了

Django中session默认保存在数据库中的django_session表

#### Django的Form和ModelForm的作用

- Form：在前端生成HTML代码、对数据做有效性校验、返回校验信息并展示
- ModelForm：根据模型生成Form组件，并且可以操作数据库

#### Django ORM中三种能写sql语句的方法

- extra：结果集修改器，一种提供额外查询参数的机制
- raw：执行原始SQL并返回模型实例对象
- excute：直接执行自定义SQL

### GORM

#### 如何定义GORM模型

在GORM中通过定义结构体来定义模型，每个结构体表示一个数据库表，可以把结构体字段和数据库表字段做一一映射

- 定义模型struct
- 定义模型表名（通过TableName函数返回一个当前模型绑定的表名字）
- 添加数据
- 查询数据

#### GORM模型字段标签的作用是什么

在GORM中通过在模型的结构体字段上添加标签来指定列名、列类型等元数据信息。例如：定义表名、主键、非空、唯一键、列默认值以及其他元数据信息。标签的作用是让GORM可以自动识别struct模型的一些配置信息

#### 怎么使用GORM进行事务操作

1. 调用DB对象的Begin方法开启一个事务
2. 在事务中执行相应的操作，包括插入、更新、删除等
3. 如果操作成功，调用Commit方法提交事务，如果操作失败或出现异常，调用Rollback方法回滚事务

```go
// 开启事务
tx := db.Begin()

// 在事务中执行操作，注意下面使用的是tx，执行操作不是db
if err := tx.Create(&User{Name: "Alice"}).Error; err != nil {
    // 操作失败，回滚事务
    tx.Rollback()
    log.Fatal(err)
}

if err := tx.Create(&User{Name: "Bob"}).Error; err != nil {
    // 操作失败，回滚事务
    tx.Rollback()
    log.Fatal(err)
}

// 操作成功，提交事务
if err := tx.Commit().Error; err != nil {
    log.Fatal(err)
}
```

#### GORM怎么做关联查询

GORM支持多种类型的关联查询，例如一对一、一对多、多对多等，主要是通过定义模型关联关系，然后通过HasOne、BelongsTo、HasMany、Preload等方法实现关联查询

#### GORM的N+1查询问题是什么，怎么解决

N+1查询问题是指查询一个对象的列表数据时，GORM会首先查询列表中的对象id，然后循环生成单独的SQL去查询对象的详细数据。这会导致SQL查询过多问题，例如查询10条订单数据，因为订单模型关联了用户模型，GORM会产生10条SQL去查询用户数据，再加上查询订单数据那一条SQL，就是10 + 1 = 11条SQL，这就是N+1问题，解决N+1问题主要是通过**Preload(预加载)**实现

Preload是一种用于在查询时预加载关联数据的方法。当你使用Preload时，GORM会在执行主查询时一并加载相关联的数据，以避免在之后的代码中多次查询数据库，从而提高查询效率

#### GORM怎么实现软删除

GORM实现软删除可以在模型中添加一个DeletedAt字段，记录被删除数据的时间，并在模型的BeforeDelete钩子中自动设置该字段为当前时间，这样每次删除数据会自动维护DeletedAt字段数据，然后在GORM连接配置打开SoftDelete软删除，查询数据时会自动过滤已经删除的数据

- 定义模型，新增DeleteAt字段
- 通过BeforeDelete钩子，自动维护DeletedAt字段
- 开启软删除

#### GORM的HOOK机制

GORM的HOOK机制是在模型的生命周期中提供了钩子函数，可以在各个时刻对模型进行操作。通过HOOK机制，可以在增删改查过程中执行一些额外的操作，常见的使用场景包括自动填充创建时间和更新时间、软删除、数据加密等

例：

```go
// 定义用户struct
type User struct {
    ID        uint   `gorm:"primaryKey"`
    Name      string `gorm:"not null"`
    Email     string `gorm:"unique; not null"`
    Password  string `gorm:"not null"`
    CreatedAt time.Time
    UpdatedAt time.Time
}

// 添加记录之前执行BeforeCreate
func (u *User) BeforeCreate(tx *gorm.DB) error {
    // 在创建新用户之前，对密码进行加密
    hashedPassword, err := bcrypt.GenerateFromPassword([]byte(u.Password), bcrypt.DefaultCost)
    if err != nil {
        return err
    }
    u.Password = string(hashedPassword)
    return nil
}

// 添加用户之后执行
func (u *User) AfterCreate(tx *gorm.DB) error {
    // 在创建新用户之后，给用户发送欢迎邮件
    emailContent := fmt.Sprintf("Dear %s,\n\nThank you for signing up!", u.Name)
    err := sendWelcomeEmail(u.Email, emailContent)
    if err != nil {
        return err
    }
    return nil
}

// 创建一个新用户
func CreateUser(name string, email string, password string) error {
    user := &User{
        Name:     name,
        Email:    email,
        Password: password,
    }
    result := db.Create(user)
    if result.Error != nil {
        return result.Error
    }
    return nil
}
```

#### GORM查询一条数据，没查到会怎样

当First、Last、Take方法找不到记录时，GORM会返回ErrRecordNotFound错误。实际开发中通过检测Error来判断

```go
err := db.Take(&food).Error
if errors.Is(err, gorm.ErrRecordNotFound) {
    fmt.Println("查询不到数据")
} else if err != nil {
//如果err不等于record not found错误，又不等于nil，那说明sql执行失败了。
	fmt.Println("查询失败", err)
}
```

#### GORM更新数据为零值的问题

通过结构体变量更新字段值，gorm库会忽略零值字段，即字段值等于0、nil、""、false这些值。如果想更新零值，可以使用map替代结构体

#### GORM更新表达式

`UPDATE foods SET stock = stock + 1 WHERE id='2'`这样的带计算表达式的更新语句GORM怎么实现：**使用Expr语句设置表达式**

```go
//等价于: UPDATE `foods` SET `stock` = stock + 1  WHERE `foods`.`id` = '2'
db.Model(&food).Update("stock", gorm.Expr("stock + 1"))
```

#### GORM怎么自动建表

GORM支持Migration特性，支持根据Go Struct结构体自动生成对应的表结构，若表已经存在不会重复创建。主要是通过AutoMigrate函数快速建表

```go
// 根据User结构体，自动创建表结构.
db.AutoMigrate(&User{})

// 一次创建User、Product、Order三个结构体对应的表结构
db.AutoMigrate(&User{}, &Product{}, &Order{})

// 可以通过Set设置附加参数，下面设置表的存储引擎为InnoDB
db.Set("gorm:table_options", "ENGINE=InnoDB").AutoMigrate(&User{})
```

### Gin

#### Gin里面的路由转发都用到了哪些功能

Gin中的路由系统用于接收请求，并将请求转发给注册的中间件或请求处理器来处理

- 路由注册：使用HTTP方法函数（`GET()`、`POST()`）等将具体的路由和处理函数绑定在一起
- 路由组：使用`Group()`方法创建路由组，将一组相关的路由进行分组管理
- 中间件：使用`User()`方法添加中间件到路由或路由组，用于处理请求前或请求后的逻辑，实现身份验证、日志记录、错误处理等
- 路由匹配：Gin使用trie的路由匹配算法，快速地将请求路径和注册的路由规则进行匹配，找到处理函数或中间件

#### Gin里的中间件

分类：全局中间件、路由组中间件、单个路由中间件

- 全局中间件：设置之后对全局的路由都起作用，通过默认路由来设置

  - ```go
    router := gin.New()
    //一次设置多个中间件 
    router.Use(Logger(), Recovery())
    //一次设置一个中间件
    router.Use(gin.Logger())
    router.Use(gin.Recovery())
    ```

- 路由组中间件：仅对路由组下的路由起作用

  - ```go
    //声明路由组的同时设置路由组中间件
    authorized := router.Group("/", CookieMiddleware())
    
    //先声明路由组然后再通过user设置
    authorized := router.Group("/")
    authorized.Use(CookieMiddleware())
    ```

- 单个路由中间件：仅对一个路由起作用

  - ```go
    //单个路由设置单个中间件
    router.GET("/login", LoginMiddleware, handler)
    
    //单个路由设置多个中间件
    router.GET("/mid", TimeMiddleware, CookieMiddleware(), handler)
    ```

中间件方法：

- `c.Next()`：在程序进入中间件的时候需要先进行一些处理，然后去执行核心业务，在执行完核心业务后再来执行该中间件

  - ```go
    router.GET("/mid", TimeMiddleware, handler)
    // 记录函数执行的时间中间件
    func TimeMiddleware(ctx *gin.Context) {
    	fmt.Println("------------TimeMiddleware 计时开始------------")
    	start := time.Now()
    	ctx.Next()
    	Since := time.Since(start)
    	fmt.Println(Since)
    	fmt.Println("++++++++++++TimeMiddleware 计时结束++++++++++++")
    }
    func handler(ctx *gin.Context) {
    	fmt.Println("#############正常的handler执行开始#############")
    	ctx.JSON(http.StatusOK, gin.H{
    		"ziop": "ziop",
    	})
    }
    ```

  - ![](images\image-20240626140923421.png)

- `c.Abort()`：中止中间件，不再执行

  - ```go
    router.GET("/login", LoginMiddleware, handler)
    func LoginMiddleware(ctx *gin.Context) {
    	fmt.Println("------------LoginMiddleware 计时开始------------")
    	login := ctx.Query("login")
    	if login != "" {
    		ctx.Next()
    		fmt.Println("Hi, " + login + ",欢迎访问ziop的小屋")
    	} else {
    		ctx.Abort()
    		fmt.Println("未登录，拒绝访问")
    	}
    	fmt.Println("++++++++++++LoginMiddleware 计时结束++++++++++++")
    }
    ```

#### 介绍一下Gin里常用的包

- bind包：将url的查询参数、http的Header、body中提交的数据格式（如form、json、xml等）绑定到go的结构体中去，方便接下来的处理

  - ```go
    type queryHeader struct {
    	Myheader string `header:"myheader"`
    	Mydemo   string `header:"mydemo"`
    }
    
    type queryBody struct {
    	Name string `json:"name"`
    	Age  int    `json:"age"`
    	Sex  int    `json:"sex"`
    }
    
    type queryParameter struct {
    	Year  int `form:"year"`
    	Month int `form:"month"`
    }
    
    type queryUri struct {
    	Id   int    `uri:"id"`
    	Name string `uri:"name"`
    }
    
    func bindUri(c *gin.Context) {
    	var q queryUri
    	err := c.ShouldBindUri(&q) //将uri路由参数绑定到结构体q中
    	if err != nil {
    		c.JSON(http.StatusBadRequest, gin.H{
    			"result": err.Error(),
    		})
    		return
    	}
    	c.JSON(http.StatusOK, gin.H{
    		"result": "bing successfully",
    		"uri":    q,
    	})
    }
    
    func bindQuery(c *gin.Context) {
    	var q queryParameter
    	err := c.ShouldBindQuery(&q) //将url查询参数绑定到结构体q中
    	if err != nil {
    		c.JSON(http.StatusBadRequest, gin.H{
    			"result": err.Error(),
    		})
    		return
    	}
    	c.JSON(http.StatusOK, gin.H{
    		"result": "bind sussessfully",
    		"query":  q,
    	})
    }
    
    func bindBody(c *gin.Context) {
    	var q queryBody
    	err := c.ShouldBindJSON(&q) //将请求体中的JSON数据绑定到结构体
    	if err != nil {
    		c.JSON(http.StatusBadRequest, gin.H{
    			"result": err.Error(),
    		})
    		return
    	}
    	c.JSON(http.StatusOK, gin.H{
    		"result": "bind sucssessfuly",
    		"body":   q,
    	})
    }
    
    func bindHead(c *gin.Context) {
    	var q queryHeader
    	err := c.ShouldBindHeader(&q) //将请求头中的数据绑定到结构体
    	if err != nil {
    		c.JSON(http.StatusBadRequest, gin.H{
    			"result": err.Error(),
    		})
    		return
    	}
    	c.JSON(http.StatusOK, gin.H{
    		"result": "bind sucssessfuly",
    		"header": q,
    	})
    }
    
    func main() {
    	srv := gin.Default()
    	srv.GET("/binding/:id/:name", bindUri)
        srv.GET("/binding/query", bindQuery)
    	srv.GET("/binding/body", bindBody)
    	srv.GET("/binding/header", bindHead)
    	srv.Run(":8080")
    }
    ```

- render包：渲染HTTP响应的工具包，能将数据渲染为JSON、XML、YAML等格式并发送到客户端以及HTML模板与数据结合，生成HTML响应

#### Gin框架实现请求地址映射到方法的过程

1. **创建Gin路由引擎**

   ```go
   r:=gin.Default()
   ```

2. **定义路由和处理函数**：在Gin中，使用HTTP方法（如GET、POST、DELETE等）和url模式来匹配请求的url，每个路由都映射到一个处理函数

   ```go
   r.GET("/user/:name", func(c *gin.Context) {
   		name := c.Param("name")
   		c.String(http.StatusOK, "hello %s", name)
   	})
   ```

3. **请求和分发处理**：当收到HTTP请求时，Gin会根据请求的HTTP方法和url路径来查找匹配的路由定义，并将请求映射到相应的处理函数，处理函数接收一个`*gin.Context`参数，该参数包含有关请求和响应的信息，可以用来读取请求参数、设置响应头、返回JSON数据等



### Go

#### new和make的区别

new和make时两个不同的内建函数，用于分配内存空间

new：

- `ptr=new(Type)`，用于创建指定类型的零值，并返回指向该类型的**指针**

make：

- 创建切片、映射和通道等类型，并返回初始化后的**实例**
  - `slice:=make([]Type,length,capacity)`
  - `m := make(map[KeyType]ValueType)`
  - `ch := make(chan Type)`

```go
slice := make([]int, 5) // 创建一个长度为5的int类型切片
m := make(map[string]int) // 创建一个映射，键类型为string，值类型为int
ch := make(chan bool) // 创建一个通道，元素类型为bool
```

#### append函数

原型：`func append(slice []Type, elems ...Type) []Type`，用于向切片（slice）追加元素

- 切片空间足够：直接将元素添加到切片末尾
- 切片空间不足：分配一个新的底层数组，然后将原始元素和新元素复制到新数组中

append的第二个参数不能直接使用slice，需使用...操作符将切片变成可变参数列表

```go
func main() {
    s1 := []int{1, 2, 3}
    s2 := []int{4, 5}
    s1 = append(s1, s2) //error,s1=append(s1,s2...)
    fmt.Println(s1)
}
```

#### 类型别名和类型定义的区别

```go
package main

import "fmt"

type MyInt1 int //新类型MyInt1
type MyInt2 = int //int的类型别名MyInt2

func main() {
    var i int =0
    var i1 MyInt1 = i //int型不能赋值给MyInt1类型，可以强转 var i1 MyInt1 = MyInt1(i)
    var i2 MyInt2 = i
    fmt.Println(i1,i2)
}
```

#### init函数

- init()函数是程序执行前做包的初始化的函数，比如初始化包里的变量等
- 一个包可以出现多个init函数，一个源文件也可以包含多个init函数
- 同个包中的init函数的执行顺序没有明确定义，但不同包的init函数是根据包导入的依赖关系决定的
- init函数在代码中不能被显示调用，不能被引用（赋值给函数、变量）
- 引入包不能出现循环引用）

![](images\image-20240621105429051.png)

#### 类型选择

类型选择是一种特殊的控制结构，用于根据接口变量中存储的实际类型执行不同的操作，**类型选择通常与接口类型（interface {}）一起使用**

```go
package main

import (
    "fmt"
)

func findType(i interface{}) {
    switch v := i.(type) {
    case int:
        fmt.Println("Integer type")
    case float64:
        fmt.Println("Float type")
    case string:
        fmt.Println("String type")
    default:
        fmt.Printf("Unknown type %T\n", v)
    }
}

func main() {
    findType(10)          // 输出 Integer type
    findType(3.14)        // 输出 Float type
    findType("Hello")     // 输出 String type
    findType([]int{1, 2}) // 输出 Unknown type []int
}
```

#### cap函数

cap()函数适用于切片类型（array、slice、channel，map不适用），用于获取切片的容量，即切片底层数组的大小

#### defer函数

- defer的作用：用于**延迟执行**某个函数调用直到包含defer语句的函数执行完毕。这种机制通常用于确保在函数执行结束时执行一些清理工作，或者在函数返回前修改某些状态
- 延迟执行时机：被defer延迟执行的函数调用会在包含defer的函数执行完毕时才执行，无论函数是正常返回还是发生了panic，会按**后进先出**的顺序执行（先声明的后执行）
- 参数求值时机：defer延迟执行的函数调用会在**defer语句声明时**记录其参数的值，而不是在实际执行时才重新计算

```go
func hello(i int) {  
    fmt.Println(i)
}
func main() {  
    i := 5
    defer hello(i) //5
    i = i + 10
}
```

#### 可变函数参数

可变函数参数是一个占位符，可以将1个或多个参数赋值给这个占位符，不管实际的参数数量是多少，都可以交给可变参数来处理

例如：`func sum(nums ...int)int {}`

- 参数个数动态变化：sum(1,2,3) sum(1,2,3,4,5)

- 使用...运算符以变量...的形式进行参数传递，这里的变量必须是与可变参数类型相同的slice，最常用的形式是在内置函数append里：

  - ```go
    result := []int{1, 3}
    data := []int{5, 7, 9}
    result = append(result, data...) // result == []int{1, 3, 5, 7, 9}
    ```

#### 切片slice

```go
type slice struct{
	array unsafe.Pointer //指向底层数组的指针，8bytes
	len int //切片的长度，8bytes
	cap int //切片的容量，8bytes
}
```

go语言中，**切片是对数组的一个引用**，是一个长度可变的序列，可以通过数组或者其他切片表达式来截取。切片的长度是它包含的元素的个数，容量是切片的起始元素开始到其底层数组末尾的元素个数，可以通过`len()`和`cap()`来获取切片的长度和容量

```go
func main() {
    s := [3]int{1, 2, 3}
    a := s[:0]
    b := s[:2]
    c := s[1:2:cap(s)]
    fmt.Println(len(a), cap(a)) //0 3 省略容量，默认为底层数组的容量
	fmt.Println(len(b), cap(b)) //2 3
	fmt.Println(len(c), cap(c)) //1 2 容量为3-1
}
```

内置的make函数创建一个指定元素类型、长度和容量的slice，容量可以省略（这种情况下容量等于长度）

```go
make([]T, len)
make([]T, len, cap) // same as make([]T, cap)[:len]
```



#### defer执行原理

1. defer在defer语句处执行，defer的执行结果是把defer后面的函数压入栈，等待return或panic之后，再按**先进后出**的顺序执行被defer的函数
2. defer的函数参数是在**执行defer**时计算的，defer的函数中的变量是在**执行函数**时计算的

执行顺序分两步：

1. 执行defer，计算函数的入参值，并传递给函数，但不执行函数，而是将函数压入栈
2. 函数return或者panic后，执行压入栈的函数，函数中变量的值会被计算

含有defer表达式时，函数的返回过程：

**先给返回值赋值，再调用defer表达式，最后返回结果**

#### Go的垃圾回收机制（GC）

**内存管理的组件**：

![](images\image-20240704151331200.png)

用户程序（Mutator）通过内存分配器（Allocator）在堆上申请内存，垃圾收集器（Collector）负责回收堆上的内存空间

**标记清除法**：

1. 标记阶段：从根对象出发查找并标记堆中的所有存活的对象
2. 清除阶段：遍历堆中的全部对象，回收未被标记的垃圾对象并将回收的内存加入空闲链表

缺点：STW（stop the world），标记和清除过程中会暂停程序，用户程序在垃圾收集过程中不能执行

**三色标记法**：

根对象：能从全局可达的地方访问到的对象，垃圾回收器会从根对象开始，通过遍历根对象的引用关系，逐步追踪并标记所有可达的对象，任何未被标记的对象都会被认为是垃圾，最终被回收

- 白色对象：潜在的垃圾，其内存可能会被垃圾收集器回收
- 黑色对象：活跃的对象，包括不存在任何引用外部指针的对象以及从根对象可达的对象
- 灰色对象：活跃的对象，因为存在指向白色对象的外部指针，垃圾收集器会扫描这些对象的子对象

1. 新创建的对象，默认标记为白色
2. 每次gc回收开始，从根节点开始遍历所有对象，把遍历到的对象从白色集合放入灰色集合
3. 遍历灰色集合，将灰色对象引用的对象从白色集合放入灰色集合，之后将此灰色对象放入黑色集合
4. 重复第三步，直到无灰色对象
5. 回收所有的白色对象，即回收垃圾

#### map的实现原理

Go的map是一个指针，占8bytes，指向hmap结构体，map底层是基于**哈希表+链地址法**存储的

特点：

- 无序：键没有固定的顺序
- 唯一：键必须是唯一的，如果重复赋值同一个键，后者会覆盖前者
- 动态增长：map可以动态增长，不需要预先声明容量
- 引用类型：类似于指针，可以修改原始数据

hmap结构体：

```go
type hmap struct {
    count     int //当前map中的元素数量（len(map)的返回值）
	flags     uint8 //表示现在是否处于正在写入的状态
    B         uint8 //表示当前哈希表持有的buckets数量，其实是对数（len(buckets)==2^B）
	noverflow uint16 //溢出桶的数量
	hash0     uint32 //生成hash的随机数种子

	buckets    unsafe.Pointer //指向buckets数组的指针
	oldbuckets unsafe.Pointer //若发生扩容，其是指向老的buckets数组的指针，是新的的一半
	nevacuate  uintptr //表示扩容进度，小于此地址的buckets代表已迁移完成

	extra *mapextra //存储溢出桶的指针
}

type mapextra struct {
	overflow    *[]*bmap
	oldoverflow *[]*bmap
	nextOverflow *bmap
}
```

![](images\image-20240704171303463.png)

bmap就是桶，每个bmap能存储8个键值对，当哈希表中存储的数据过多时，单个桶已经装满时就会使用nextOverflow中桶存储溢出的数据

#### Goroutine的实现原理

goroutine的概念类似于线程，是Go语言的协程（用户态线程），goroutine是由Go的运行时（runtime）调度和管理的，使用goroutine只需要在调用函数时在前面加上go关键字，就可以为函数创建一个goroutine，一个goroutine必定对应一个函数，可以创建多个goroutine去执行相同的函数

**goroutine调度**

GPM（Goroutine Processor Machine）是Go语言的一套goroutine调度系统，区别于操作系统调度os线程

- G：Goroutine，存放goroutine本身的信息以及与所在的P绑定的信息
- P：Processor，管理一组goroutine队列，存储当前goroutine运行的上下文环境（函数指针、堆栈地址及地址边界），对自己管理的goroutine队列做一些调度
- M：Machine，Go运行时（runtime）对os内核线程的虚拟，M与内核线程是一一映射的关系，一个goroutine是要放到M上执行的

优势：os线程是由os内核来调度的，goroutine则是由**Go运行时（runtime）自己的调度器调度的**，这个调度器使用一个称为**m:n调度器**的技术（复用m个goroutine到n个os线程）。特点就是**goroutine的调度是在用户态完成的**，不涉及内核态和用户态之间的频繁切换，包括内存的分配与释放都是在用户态维护一大块内存池，不涉及malloc函数，成本比os调度线程低得多。另一方面利用了多核的硬件资源，近似的把若干goroutine均分在物理线程上，再加上本身goroutine的超轻量，保证了go调度方面的性能

#### Goroutine和线程的区别

1. 一个线程可以有多个协程
2. 线程、进程都是同步机制，协程是异步
3. 协程可以保留上一次调用时的状态
4. 协程需要线程来承载运行（协程可以理解为用户态线程）

#### channel类型

channel是一种引用类型，里面的内容可读可写，遵循先进先出的规则，保证收发数据的顺序，声明channel时需要指定元素类型

```go
 var ch1 chan int   // 声明一个传递整型的通道
 var ch2 chan bool  // 声明一个传递布尔型的通道
 var ch3 chan []int // 声明一个传递int切片的通道
```

声明后的channel要make函数初始化后才能使用

```go
ch := make(chan int)
```

**channel操作**

channel有发送、接收和关闭三种操作

```go
ch := make(chan int)
ch <- 10 //把10发送到ch中
x := <-ch //从ch中接收值并赋给x
<-ch //直接接收值，忽略输出
close(ch) //关闭ch
```

关闭后的channel有以下特点：

1. 对关闭的channel再发送值会panic
2. 对关闭的channel再接收值会一直获取值直至channel为空
3. 对关闭的且没有值的channel执行接收操作会得到对应类型的零值
4. 关闭一个已经关闭的channel会panic



### 面试题总结

**C++语言的特点**

面向对象、三大特性、模板可复用性高、新特性（nullptr、智能指针、auto关键字、匿名函数）

**C++中struct和class的区别**

struct：数据结构集合、默认public、公有继承

class：对象数据的封装、默认private、私有继承、可以用于定义模板参数

**C++结构体和C结构体的区别**

成员函数：C无，C++可以有；静态成员：C无，C++可以有；访问控制：C默认public，C++可修改；继承关系：C不支持继承，C++可以有；初始化：C不能直接初始化数据成员，C++可以

**C++从代码到可执行二进制文件的过程**

**预处理、编译、汇编、链接**

预处理：展开宏定义、处理#include、过滤注释

编译：生成汇编代码

汇编：将汇编代码转为机器指令

链接：将不同的源文件生成的目标文件进行链接生成可执行程序

- 静态链接：链接的时候就把要调用的函数或过程链接到了可执行文件中，生成的静态链接库：.lib .a
- 动态链接：执行的过程中再去找要链接的函数，生成的可执行文件中没有函数代码，只包含函数的重定向信息，生成的动态链接库：.dll .so

**static关键字的作用**

全局静态变量和局部静态变量、静态函数、类的静态成员变量和函数（属于类，没有this指针）

**数组和指针的区别**

数组：多个相同类型数据的集合，数组名是数组首元素的地址

指针：相当于变量，但存放的是其他变量的地址

区别：赋值、存储方式、sizeof

**什么是函数指针**

概念：指向函数的指针变量，存放的是函数的入口地址

应用场景：回调

**什么是野指针**

概念：指向位置不确定的指针

产生原因：释放内存后未置空

**静态变量、全局变量、局部变量的特点及使用场景**

全局变量：全局作用域、静态存储区

静态全局变量：全局作用域、静态存储区

局部变量：局部作用域、栈上

静态局部变量：局部作用域、栈上

**内联函数和宏函数的区别**

宏函数：不是函数，预编译时文本替换、没有类型检查

内联函数：是函数，编译时代码插入、有类型检查

**new和malloc的区别以及实现原理**

1. new是操作符、malloc是函数
2. new会调用构造函数，malloc没有
3. new不用指定内存大小，返回指针不用强转；malloc需要指定申请内存的大小，返回的指针需要强转成需要的类型
4. new发生错误时抛出异常；malloc返回null
5. malloc底层：当开辟的空间小于128K时，调用brk()，否则调用mmap()
6. new底层：创建新对象、调用构造函数、指向构造函数、返回新对象

**C++有几种传值方式，之间的区别是什么**

值传递、引用传递、指针传递

值传递不会影响实参的值，引用传递和指针传递会

**堆和栈的区别**

栈由操作系统分配释放、堆由程序员分配释放

**C++内存管理**

内存分配方式：堆、栈、自由存储区、全局存储区、常量存储区

**进程地址空间**

0~3G：用户空间，从低地址到高地址：代码区、data（已初始化的全局变量）、bss（未初始化或初始化未0的全局变量）、堆区、共享区、栈区

3~4G：内核空间

**什么是内存泄漏**

申请了一块内存，但使用完没有释放

- new和malloc之后，没有delete或者free
- 子类继承父类，但父类的析构函数不是虚函数

**什么是面向对象**

一种编程思想，将对象的属性和操作属性的方法封装到一起，能够更快地开发程序，减少了重复代码

**面向对象的三大特性**

封装：将数据和操作数据的方法结合，隐藏对象的属性和实现细节，对外只暴露公开接口来和对象交互

继承：使用现有类的功能，无需在重新编写原来的类的情况下对这些功能进行扩展

多态：同一个函数或者操作在不同的对象上有不同的表现形式

**重载和重写的区别**

重载：函数的参数类型、个数或者返回值不同

重写：子类重写父类虚函数

**构造函数有几种，分别有什么作用**

默认构造、有参构造、拷贝构造、移动构造

默认构造和有参构造：定义类的对象时，完成对象的初始化工作

拷贝构造：用来复制本类的对象到另一个对象

移动构造：将一个对象的资源转移到另一个对象

**深拷贝和浅拷贝，如何实现深拷贝**

浅拷贝：就是值拷贝，将原对象的值拷贝到另一个对象，本质上原对象和新的对象共用同一份实体，地址还是相同的

深拷贝：拷贝时先开辟出一块和原对象一样大小的空间，再将内容拷贝过去，就有两个指针指向不同的位置，里面的内容是一样的，深拷贝可以避免浅拷贝带来的内存重复释放的问题

**C++多态**

多态：同一个函数或过程在不同的对象上表现出不同的形式

-  静态多态：一般指重载，在编译期间完成，编译器根据实参的类型来决定调用哪一个函数
- 动态多态：运行时完成，基类实现虚函数，派生类重写基类虚函数，基类指针指向派生类对象

**为什么要虚析构，不能虚构造**

一般将父类的析构函数设为虚析构，在父类指针指向子类对象时，释放父类指针可以释放的空间，防止内存泄漏

虚函数的实现是根据虚函数表来的，虚函数表的地址是存储在对象的内存空间中，而构造函数就是用来实例化对象为对象分配内存空间的（悖论）

**什么是虚继承，解决什么问题**

虚继承是解决多重继承的一种手段，从不同途径继承来的同意基类在子类中会存在多份拷贝，会导致浪费存储空间和二义性

**虚函数和纯虚函数**

虚函数：加了virtual关键字的函数，是实现动态多态的机制，父类声明虚函数，子类重写父类虚函数，父类指针调用实际的子类成员函数；通过虚函数表实现，即类的虚函数的地址表，父类指针通过这张表来调用实际的子类函数

纯虚函数：在虚函数的基础上，基类中没有定义，加了=0的函数，有纯虚函数的类为抽象类，不能实例化对象

**哪些函数不能被声明为虚函数**

普通函数：没有意义

构造函数：悖论

内联函数：编译时需要在代码中展开，而虚函数是运行时才动态绑定的

静态成员函数：属于类，没有this指针，无法动态绑定

友元函数：不支持友元函数的继承，没有继承特性的函数没有虚函数的说法

**STL的基本组成部分**

容器：一种数据结构，以模板类的方式提供

算法：操作容器中数据的模板函数

迭代器：访问容器中对象的方法，类似于指针

仿函数：函数对象，重载了函数调用运算符

适配器：接口类，专门用来修改现有类的接口，提供一种新的接口

空间配置器：为STL提供空间配置的系统，包括对象的创建与销毁、内存的获取与释放

**vector扩容**

- 分配新内存空间
- 拷贝元素
- 释放旧空间

**STL常见的容器以及实现原理**

顺序容器：容器非排序的，插入位置与元素的值无关，包括vector、deque、list

- vector：采用一维数组实现，元素在内存中连续存放
- deque：采用双向队列实现，元素在内存中连续存放
- list：采用双向链表实现，元素存放在堆中

关联容器：元素是排序的，插入任何元素都按照排序规则来确定位置，包括set/multiset、map/multimap

- map/set/multimap/multiset：采用红黑树实现
- unordered_map/unordered_set/unordered_multimap/unordered_multiset：采用哈希表实现

容器适配器：封装了基本的容器，使之具备新的功能，包括stack、queue、priority_queue

**STL中迭代器的作用**

迭代器不是指针，是类模板，表现得像指针，提供一种方法可以顺序访问容器对象中的各个元素，但又不暴露其内部表示

**迭代器是怎么删除元素的（迭代器失效）**

对于顺序容器vector等来说：erase过后，当前元素及之后的元素的迭代器失效，后面的元素都往前移一位，erase返回下一个有效的迭代器

对于关联容器map、set来说：erase过后，由于其底层结构是红黑树，只有当前元素的迭代器失效，不会影响后面的元素，记录下一个元素的迭代器即可

对于list来说：由于使用了不连续的内存，erase过后也会返回下一个有效的迭代器，上面两种方法都适用

**STL中resize和reserve的区别**

resize既分配了空间也创建了对象，reverse只是为容器预留空间，并没有创建对象，要通过insert或者push_back等创建对象

**string和char*的区别**

- string是C++标准库中的字符串类；char*是一种字符指针类型
- string内部通过动态分配内存来管理字符串；char*只是一个指针，通常指向以'\0'结尾的字符数组
- string可以通过c_str()转为char\*；char*可以通过string的构造函数或赋值运算符转为string

**push_back()和emplace_back()的区别**

如果要将一个临时对象加到容器末尾，push_back需要构造临时对象再将其拷贝到容器末尾，而emplace_back是直接在容器末尾构造对象，没有拷贝的步骤

**C++11的新特性有哪些**

- 统一的初始化方法：初始化列表可以用于任何类型对象的初始化
- 成员变量默认初始化：好处是不需要构造函数初始化成员变量
- auto关键字：定义变量，编译器根据其初始值推断类型，例```auto x = 10```
- decltype：根据表达式的类型推导变量的类型，例```decltype(10.8) x = 5.5```
- 智能指针：包括shared_ptr、unique_ptr、weak_ptr
- 空指针nullptr：替代null专门用于初始化空类型指针
- 基于范围的for循环
- 右值引用和move语义：对右值建立引用，右值引用具有移动语义，移动语义可以将资源（堆、系统对象等）通过浅拷贝从一个对象转移到另一个对象这样就能减少不必要的临时对象的创建、拷贝以及销毁
- lambda表达式：匿名函数

**C++智能指针如何实现**

使用了RAII（Resource Acquisition Is Initialization）技术，免去手动释放指针指向资源的步骤，希望能自动释放内存资源。底层而言，就是通过封装一个类，使用析构函数来释放资源

**C++11中的智能指针有哪些，分别解决什么问题**

shared_ptr：共享的智能指针，允许多个智能指针指向同一个对象，使用引用计数来表示资源被几个指针共享，当引用计数减为0时，资源被释放

unique_ptr：独占的智能指针，同一时间只允许一个智能指针指向该对象

weak_ptr：弱引用智能指针，配合shared_ptr工作，它的构造和析构不会引起引用计数的增加或减少，主要用于解决shared_ptr相互引用时造成的死锁问题

**简述右值引用和移动语义**

左值：能取地址的值，右值：不能取地址的值，包括纯右值（常量、临时对象和表达式的结果）和将亡值（一种特殊的右值引用，表示它的资源即将被移动到另一个对象中）

右值往往是没有名称的，使用它只能借助引用的方式，对右值进行修改就需要右值引用

移动语义：移动语义可以将资源（堆、系统对象等）通过浅拷贝从一个对象转移到另一个对象这样就能减少不必要的临时对象的创建、拷贝以及销毁

`std::move()`：将任意类型的左值转为其类型的右值引用

**Linux中查看进程状态的命令、查看内存使用情况的命令**

查看进程运行状态：```ps -aux | grep pid```

查看内存使用情况：```free -m```

**文件权限怎么修改**

chmod命令，9个权限三个是一组，-rwxrwxrwx，三组分别是所属用户、同组的用户、其他用户

**常见的Linux命令**

cd ls grep cp mv rm ps kill tar cat top（查看操作系统信息，如进程、CPU占用率、内存信息等） pwd

**软链接和硬链接的区别**

软链接：相当于快捷方式，创建命令```ln -s file file.s```

硬链接：新的文件名，但不占用磁盘空间，映射到原来的磁盘地址上，每个文件都有硬链接计数，创建命令```ln file file.hard```

**静态库和动态库的区别**

静态库和动态库：两种库文件形式，用于代码重用和共享

- 静态库编译时加载、动态库运行时加载
- 静态库代码装载速度快，执行速度比动态库快
- 动态库更节省内存，可执行文件体积小
- 静态库：.a .lib；动态库：.so .dll

**简述GDB调试命令，什么是条件断点、多进程下如何调试**

GDB调试的是可执行文件，在gcc编译时加入-g，例```gcc test.c -o a.out -g```，再```gdb ./a.out```

常见的命令：list/l b run/r next/n step/s print/p continue finish quit

条件断点：break if 条件 以条件表达式设置断点

多进程下调试：```set follow-fork-mode child```调试子进程，```set follow-fork-mode parent```调试父进程

**什么是大端小端，如何判断大端小端**

大端：高位存在低地址，网络字节序

小端：低位存在低地址，主机字节序

**进程调度算法有哪些**

先来先服务、短作业优先、高优先级优先调度、时间片轮转、多级反馈队列

非抢占式：系统一旦把处理机分配给就绪队列中优先权最高的进程后，该进程便一直执行下去直至完成；或因某事件使该进程放弃处理机时，系统才重新分配，主要适用于批处理系统或对实时性要求不高的系统

抢占式：同样是先把处理机分配给优先权最高的进程，在执行期间若有优先权更高的进程，进程调度程序便立即停止，重新将处理机分配给优先权最高的进程，主要适用于实时性要求较高的系统

**操作系统如何申请以及管理内存**

物理内存：寄存器、高速缓存、主存、磁盘

虚拟内存：即进程地址空间，虚拟内存与物理内存之间存在映射关系，通过页表寻址完成虚拟地址到物理地址的转换

**Linux系统态与用户态，什么时候进入系统态**

内核态与用户态是操作系统运行的两种级别

内核态（系统态）：最高权限，能访问所有系统指令

用户态：只能访问一部分指令

**LRU算法**

用于缓存淘汰，即淘汰最近最少使用的缓存

实现方式：双向链表+哈希表，哈希表存储key和双向链表节点，越靠近链表头部的节点表示最近使用过，利用伪头节点和伪尾节点方便插入和移动操作

```c++
//双向链表
struct DlinkedNode {
    int key,value;//存储键和值
    DlinkedNode* prev;
    DlinkedNode* next;
    DlinkedNode():key(0),value(0),prev(nullptr),next(nullptr){};
    DlinkedNode(int _key,int _value):key(_key),value(_value),prev(nullptr),next(nullptr){};
};

class LRUCache {
private:
    unordered_map<int,DlinkedNode*> cache;
    DlinkedNode* head; //伪头节点
    DlinkedNode* tail; //伪尾节点
    int _size; //链表大小
    int _capacity; //最大容量
    //添加到头部
    void addToHead(DlinkedNode* node)
    {
        auto tmp=head->next;
        head->next=node;
        node->prev=head;
        node->next=tmp;
        tmp->prev=node;
    }
    //删除节点（仅修改指向）
    void removeNode(DlinkedNode* node)
    {
        node->prev->next=node->next;
        node->next->prev=node->prev;
    }
    //将节点移到头部
    void moveToHead(DlinkedNode* node)
    {
        removeNode(node); //先修改指向
        addToHead(node); //再添加到头部
    }
    //删除尾部节点
    DlinkedNode* removeTail()
    {
        DlinkedNode* node=tail->prev;
        removeNode(node);
        return node;
    }
public:
    LRUCache(int capacity) {
        _size = 0;
        _capacity = capacity;
        head= new DlinkedNode();
        tail = new DlinkedNode();
        head->next=tail;
        tail->prev=head;
    }
    
    int get(int key) {
        if(cache.find(key) == cache.end())
            return -1;
        //将对应的节点移到头部（表示最近使用过）
        DlinkedNode* node = cache[key];
        moveToHead(node);
        return node->value;
    }
    
    void put(int key, int value) {
        //若key不存在
        if(cache.find(key) == cache.end())
        {
            //创建新节点
            DlinkedNode* node=new DlinkedNode(key,value);
            //加入哈希表
            cache.insert(make_pair(key,node));
            //加入到链表头部
            addToHead(node);
            ++_size;
            //若超出最大容量则删除尾部节点
            if(_size > _capacity)
            {
                DlinkedNode* rem = removeTail();
                cache.erase(rem->key);
                delete rem;
                --_size;
            }
        }
        else
        {
            //通过哈希表找到节点
            DlinkedNode* node=cache[key];
            //修改value
            node->value=value;
            //移到头部
            moveToHead(node);
        }
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

**什么是页表，为什么需要页表**

页表是虚拟内存的概念，**操作系统虚拟内存到物理内存的映射表称为页表**

原因：进行分页，可以减小虚拟内存对应物理内存的映射表大小

**什么是缺页中断**

缺页异常：malloc和mmap函数在分配内存时只是建立了进程虚拟地址空间，并没有分配虚拟内存对应的物理内存，当进程访问到没有建立映射关系的虚拟内存时就会触发缺页异常，引起缺页中断

缺页中断：操作系统根据页表中外存地址在外存中找到所缺的那一页，将其调入内存

缺页中断与普通中断一样需要经历的四个步骤：保护CPU现场、分析中断原因、转入缺页中断处理程序、恢复CPU现场继续执行

**虚拟内存分布，什么时候会由用户态陷入内核态**

即进程地址空间

什么时候进入内核态：系统调用、异常、设备中断

**为什么需要虚拟内存**

- 进程地址空间不隔离，没有权限保护
- 内存使用效率低：内存空间不足时，要将其他程序暂时拷贝到磁盘，大量的换入换出使得内存使用效率低下
- 程序运行的地址不确定

**使用虚拟内存的好处是什么**

- 扩大地址空间：每个进程独占4G空间，虽然真实的物理内存没那么多
- 内存保护：防止不同进程对物理内存的争夺，对特定内存地址进行写保护
- 内存共享，方便进程通信
- 避免内存碎片：物理内存可能不连续，但映射到虚拟内存上的可以连续

**虚拟地址到物理地址的映射**

三级页表转换

1. 逻辑地址转线性地址：段起始地址+段内偏移地址=线性地址
2. 线性地址转物理地址：32位线性地址被划分位三部分：页目录索引、页表索引、页内偏移
   1. 页目录地址+页目录索引=页表地址
   2. 页表地址+页表索引=页地址
   3. 页地址+页内偏移=物理地址

**并发和并行**

并行：对于多个CPU，多个进程同时运行

并发：对于单个CPU，某个时刻只有一个进程在运行，CPU在多个不同线程间来回切换，宏观上看起来有多个进程在同时运行

**进程、线程、协程分别是什么，区别是什么**

进程：程序是指令、数据及其组织形式的集合，进程是运行的程序

线程：微进程，一个进程里更小的执行单位，一个进程里可以包含多个线程并发执行

协程：微线程，可以在子程序内部执行、内部中断，转而执行别的子程序

进程与线程的区别：

- 一个线程属于进程，一个进程包含多个线程
- 进程是系统资源分配的基本单位，线程是CPU调度的基本单位
- 进程执行时拥有独立的内存单元，多个线程共享进程的内存，但每个线程有自己的栈段和寄存器组

线程和协程的区别：

- 协程执行效率极高，协程直接操作栈没有内核切换的开销
- 协程不需要多线程的锁机制，因为多个协程从属于同一个线程，不存在写变量冲突
- 一个线程可以有多个协程

**fork函数**

作用：创建子进程，对于父进程，返回子进程的pid；对于子进程，调用成功返回0，失败返回-1

**什么是孤儿进程、僵尸进程，如何解决僵尸进程**

孤儿进程：父进程退出后，子进程还在运行，则子进程成为孤儿进程。孤儿进程将被init进程收养并完成状态收集工作

僵尸进程：子进程退出后，父进程没有调用wait或waitpid函数回收子进程，造成子进程的PCB残留在内核中，成为僵尸进程

解决僵尸进程：使用kill命令

```c
ps aux | grep Z //查看僵尸进程
kill -s SIGCHLD pid（父进程）
```

**进程间通信的方式有哪些**

管道、系统IPC（消息队列、信号量、信号、内存共享）、套接字

**进程有多少种状态**

创建、就绪、执行、阻塞、终止

**进程通信中管道实现的原理是什么**

**操作系统在内核开辟的一块用于通信的缓冲区即为管道**，负责两个进程之间的**单向通信**，本质是一种文件

管道原型：

```c++
＃include <unistd.h>  
int pipe(int fd[2]);  
```

实现步骤：

1. 父进程pipe开辟管道，得到两个pid指向管道两端
2. 父进程fork子进程，子进程也有两个pid指向同一管道
3. 一个读一个写

**mmap的原理及使用场景**

原理：是一种内存映射文件的方法，即将一个文件映射到进程的地址空间，实现文件磁盘地址到进程虚拟地址空间的对应关系。实现之后就可以通过地址（指针）操作这块内存，同时可以回写到磁盘文件上，避免了使用read、write函数

使用场景：同一块区域频繁读写、用户空间和内核空间高效交互、进程间共享内存及相互通信、高效的大规模数据传输

**协程是轻量级线程，轻量级表现在哪**

- 协程调用跟切换比线程效率高：协程无锁机制，可以不加锁地访问全局变量
- 协程占用内存少

**线程同步的方式有哪些**

互斥锁、信号量、条件变量、读写锁

**什么是死锁，产生的条件，如何解决**

死锁：多个进程在执行过程中，因争夺资源而造成互相等待

产生的条件：

- 互斥条件：进程所分配到的资源不允许其他进程访问
- 请求保持条件：进程获得一定资源后，又对其他资源发出请求，但其他资源被其他进程占有，而进程又不会释放自己已占有的资源
- 不可剥夺条件：进程已获得的资源只能自己释放
- 环路等待条件

如何解决：

- 资源一次性分配
- 可剥夺资源
- 资源有序分配

**简述互斥锁机制，互斥锁与读写锁的区别**

互斥锁机制：用于保证在任何时刻，都只有一个线程能访问该对象。当获取锁失败时，线程进入睡眠，等待锁释放时被唤醒。互斥锁泛指Linux中所有的锁机制

互斥锁与读写锁的区别：互斥锁同一时间只允许一个线程访问对象，无论读写；读写锁同一时间只允许一个写者，可以有多个读者

**什么是信号量，有什么作用**

概念：本质是一个计数器，用于多进程对共享数据对象的获取，主要用来保护共享资源，保证统一时刻只有一个进程独享资源

原理：P操作（获取资源）和V操作（唤醒线程）

**线程池的设计思路，为什么要创建线程池**

组成：

- 任务队列
- n个工作的线程（任务队列的消费者）
- 1个管理者线程

为什么：创建线程和销毁线程的开销比较大，创建线程池是为了提高工作效率

**IO多路复用技术有哪些，分别是什么**

IO多路复用技术有select、poll、epoll，多路IO复用就是通过一种机制，可以监视多个文件描述符，一旦某个文件描述符就绪（一般是读就绪或者写就绪），能够通知应用程序进行相应的读写操作

- select：传统的IO多路复用技术，使用select()系统调用来同时监视多个文件描述符的状态，主要是通过文件描述符集合fd_set（位图）来进行监听，有最大监听上限1024的限制
- poll：改进的IO多路复用技术，使用poll()系统调用来监视多个文件描述符的状态，与select不同的是poll没有最大监听上限的限制，是通过结构数组fds来存储文件描述符的状态
- epoll：Linux特有的多路IO复用技术，通过epoll机制监视多个文件描述符的状态，采用红黑树来管理待检测的文件描述符集合，并且使用回调机制来处理读写事件，更加高效

**select，epoll的使用场景和区别**

简言之：select和epoll最大的区别在于epoll能知道具体是哪几个文件描述符有读写请求，而select是告诉你有读写请求（产生动静），需要再一次去对比才能知道是哪几个文件描述符

- select是把产生动静的fd放入集合，然后再重新创建数组进行对比，而epoll通过epoll_wait的传出参数events就能得到产生动静的fd
- select有最大并发限制1024，因为一个进程打开的fd是有上限的

**epoll水平触发与边缘触发的区别**

LT：水平触发，关心的是缓冲区的状态，只要有数据epoll_wait就会返回它的事件，提醒应用程序去操作

ET：边缘触发，关心的是缓冲区状态的变化，只有数据到来时才会触发，不管缓冲区内的数据是否读完

**Reactor和Proactor模式**

Reactor：同步IO，应用程序需要自己读取或写入数据

Proactor：异步IO，应用程序不需要用户自己接收数据，OS会将数据从内核拷贝到用户区

**同步与异步，阻塞与非阻塞的区别**

同步：是所有的操作都做完，才返回给用户结果，即**写完数据库**之后，**再响应用户**

异步：不用等所有操作都做完，就响应用户请求，即**先响应用户请求**，然后**慢慢去写数据库**

阻塞：调用者调用了某个函数，等待这个函数返回，期间什么也不做

非阻塞：非阻塞等待，每隔一段时间就去检查IO事件是否就绪，没有就绪就可以做其他事情

**socket编程中客户端和服务端用到哪些函数**

服务端：socket、bind、listen、accept

客户端：socket、connect、read、write

**简述TCP三次握手和四次挥手的过程**

三次握手：

- 建立连接时，客户端向服务器发送SYN包，请求建立连接
- 服务端收到客户端的SYN包，回复一个ACK包确认收到，同时发送一个SYN包给客户端
- 客户端再回复一个ACK包给服务端

四次挥手：

- 客户端发送FIN包请求终止连接
- 服务端收到FIN包，发送ACK包，关闭客户端到服务端的连接（半关闭）
- 服务端发送FIN包，表示可以断开连接
- 客户端收到FIN包后，发送ACK包确认收到，等待2MSL后确保服务端没有数据发过来，则彻底断开连接

**TCP为什么要三次握手，两次行不行**

不行，因为要实现可靠传输，通信的双方必须维护一个序列号，以表示发送出去的数据包中哪些是对方已经收到的。若只有两次握手的话只有连接发起方的起始序列号能被确认

**简述TCP和UDP的区别**

- TCP是面向连接的；UDP是无连接的
- TCP保证可靠传输，即数据是按序发送按序接收的，提供超时重传保证可靠性；UDP不保证可靠传输
- TCP有流量控制和拥塞控制；UDP没有，网络拥堵不影响发送端的发送速率
- TCP是面向字节流的；UDP面向报文

**TCP可靠性的保证**

检验和、序列号/超时重传、最大报文长度、滑动窗口控制

**简述什么是MSL，为什么客户端要等2MSL的时间才能完全关闭**

MSL是最长报文段寿命，指任何报文在网络上能够存在的最长时间

为了保证客户端发送的最后一个ACK报文能够到达服务器。因为这个报文有可能丢失，若客户端发送完后不等待2MSL就直接关闭，一旦这个ACK报文丢失，服务端就不能正常关闭

**端到端和点到点的区别**

端到端：针对传输层而言，指的是主机中两个进程之间的通信

点到点：针对网络层或链路层而已，指一台机器到另一台机器之间的通信，不涉及进程或程序的概念

**简述http和https的区别**

- http是超文本传输协议，是明文传输；https是具有安全性的ssl加密传输协议
- http的连接很简单，是无状态的；https是ssl+http构建的加密传输、身份认证的网络协议，比http安全

**说说http方法有哪些**

- get：请求访问已经被url识别的资源，可以通过url传参给服务器
- post：传输信息给服务器，一般用于提交信息
- put：传输文件
- head：获取报文首部，与get方法类似，只是不返回报文主体，一般用于验证url是否有效
- delete：删除文件
- options：查询url支持的http方法

**介绍一下SQL中的聚合函数**

- COUNT()：统计数据表中包含的记录行的总数，或者根据查询结果返回列中包含的数据行数
  - COUNT(*)计算表中总的行数，不管某列是否有数值或者为空值
  - COUNT(字段名)计算指定列下总的行数，计算时将忽略空值的行
- AVG()：计算返回的行数和每一行数据的和，求得指定列数据的平均值
- SUM()：求总和的函数，返回指定列值的总和
- MAX()、MIN()：返回指定列中的最大值最小值

**表和表是怎么关联的**

内连接、外连接

**WHERE和HAVING的区别**

WHERE是一个约束声明，使用WHERE约束来自数据库的数据，WHERE是在结果返回之前起作用的，WHERE中不能使用聚合函数

HAVING是一个过滤声明，是在查询返回结果集以后对查询结果进行的过滤操作，在HAVING中可以使用聚合函数

**说一说对MySQL索引的理解**

索引是一个单独的、存储在磁盘上的数据库结构，包含着对数据表里所有记录的引用指针。使用索引可以快速找出在某个或多个列中有一特定值的行，所有MySQL列类型都可以被索引，对相关列使用索引是提高查询操作速度的最佳途径

MySQL中索引的存储类型有两种，即BTREE和HASH

优点：

- 通过创建唯一索引，可以保证数据库表中每一行数据的唯一性
- 可以大大加快数据的查询速度，这也是创建索引的主要原因
- 在实现数据的参考完整性方面，可以加速表和表之间的连接
- 在使用分组和排序子句进行数据查询时，也可以显著减少查询中分组和排序的时间

缺点：

- 创建索引和维护索引要耗费时间，并且随着数据量的增加所耗费的时间也会增加
- 索引需要占磁盘空间，除了数据表占数据空间之外，每一个索引还要占一定的物理空间，如果有大量的索引，索引文件可能比数据文件更快达到最大文件尺寸
- 当对表中的数据进行增加、删除和修改的时候，索引也要动态地维护，这样就降低了数据的维护速度

**索引有哪几种**

- 普通索引和唯一索引：普通索引是MySQL中的基本索引类型，允许在定义索引的列中插入重复值和空值；唯一索引要求索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一；主键索引是一种特殊的唯一索引，不允许有空值
- 单列索引和组合索引
- 全文索引
- 空间索引

















