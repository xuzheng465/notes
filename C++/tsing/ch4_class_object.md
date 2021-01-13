#### 类的成员函数

* 在类中声明函数原型
* 可以在类外给出函数体实现，并在函数名前使用类名加以限定
* 也可以直接在类中给出函数体， 形成**内联成员函数**
* 允许声明重载函数和带默认参数值的函数

#### 内联成员函数

* 为了提高运行时的效率
* 不要有复杂结构
  * 循环 switch
* 在类中声明内联成员函数的方法：
  * 将函数体放在类的声明中
  * 类外实现函数时，使用inline关键字
* 



#### 构造函数

用于描述初始化算法

#### 隐含生成的构造函数

如果程序中未定义构造函数，编译器将在需要时自动生成一个默认构造函数

* 参数列表为空，不为数据成员设置初始值；
* 如果类内定义了成员的初始值，则使用内类定义的初始值。
* 如果没有定义类内的初始值，则以默认方式初始化
* 基本类型的数据默认初始化的值时不确定的





* 如果程序中已定义构造函数，默认情况下编译器就不再隐含生成默认构造函数。
* 如果此时依然希望编译器隐含生成默认构造函数，可以使用 `=default`

```c++
class Clock {
  public:
  Clock(int newH, int newM, int newS);
  void setTime(int newH, int newM, int newS);
  void showTime();
  private:
  int hour, minute, second;
};
Clock::Clock(int newH, int newM, int newS):hour(newH), minute(newM), second(newS) {}

int main() {
  Clock c(0,0,0); //自动调用构造函数
  c.showTime();
  return 0;
}
```



#### 复制构造函数

```c++
int main() {
  Point a;
  Point b(a); // 情况1: 用a初始化b，第一次调用拷贝构造函数
  cout << b.getX() << endl;
  func1(b); // 情况2: 对象b作为func1的实参， 第二次调用拷贝构造函数
  b = fun2(); // 情况3: 函数的返回值是类对象，函数返回时，调用拷贝构造函数。
  cout << b.getX() << endl;
  return 0;
}
```



#### 构造组合类对象时的初始化次序

* 首先对构造函数初始化列表中列出的成员（包括基本类型成员和对象成员）进行初始化，初始化次序时成员在类体中定义的次序。
* 成员对象构造函数调用顺序：**按对象成员的声明顺序，先声明者先构造。**
* 初始化列表中未出现的成员对象，调用用默认构造函数（即无形参的）初始化
* 处理完初始化列表之后，再执行构造函数的函数体。



#### 前向引用声明

```c++
class B;
class A {
  public:
  void f(B b);
};
class B {
  public:
  void g(A a);
};

```

类应该先声明，后使用

如果需要再某个类的声明之前，引用该类，则应进行前向引用声明。

前向引用声明只为程序引入一个标识符，但具体声明在其他地方



* 虽然这解决一些问题，但他不是万能的
* 在提供一个完整的类声明之前，不能声明该类的对象，也不能在内联成员函数中使用该类的对象。
* 当使用前向引用声明时，只能使用被声明的符号，而不能涉及类的任何细节

```c++
class Fred; // 前向引用声明
class Barney {
  Fred x; // 错误： 类Fred的声明尚不完善
};
class Fred {
  Barney y;
};

```

前向引用声明某个类后，可在之后的其他类的成员函数中将该类作为参数类型使用



#### 类的组合



```c++
#include <iostream>

#include <cmath>

using namespace std;

class Point
{ //Point类定义

public:
    Point(int xx = 0, int yy = 0)
    {
        x = xx;
        y = yy;
    }

    Point(Point &p);
    int getX() { return x; }
    int getY() { return y; }

private:
    int x, y;
};

Point::Point(Point &p)
{ //复制构造函数的实现
    x = p.x;
    y = p.y;
    cout << "Calling the copy constructor of Point" << endl;
}

//类的组合

class Line
{ //Line类的定义

public: //外部接口
    Line(Point xp1, Point xp2);
    Line(Line &l);
    double getLen() { return len; }

private:          //私有数据成员
    Point p1, p2; //Point类的对象p1,p2
    double len;
};

//组合类的构造函数

Line::Line(Point xp1, Point xp2) : p1(xp1), p2(xp2)
{
    cout << "Calling constructor of Line" << endl;
    double x = static_cast<double>(p1.getX() - p2.getX());
    double y = static_cast<double>(p1.getY() - p2.getY());
    len = sqrt(x * x + y * y);
}

Line::Line(Line &l) : p1(l.p1), p2(l.p2)
{ //组合类的复制构造函数
    cout << "Calling the copy constructor of Line" << endl;
    len = l.len;
}

//主函数

int main()
{
    Point myp1(1, 1), myp2(4, 5); //建立Point类的对象
    Line line(myp1, myp2);        //建立Line类的对象
    // 形参和实参结合 执行两次拷贝构造函数
    // Line的构造函数 要把myp1和myp2的值给p1 p2所以还需要执行两次拷贝构造函数
    Line line2(line); //利用复制构造函数建立一个新对象
    cout << "The length of the line is: ";
    cout << line.getLen() << endl;
    cout << "The length of the line2 is: ";
    cout << line2.getLen() << endl;
    return 0;
}
```

```
Calling the copy constructor of Point
Calling the copy constructor of Point
Calling the copy constructor of Point
Calling the copy constructor of Point
Calling constructor of Line
Calling the copy constructor of Point
Calling the copy constructor of Point
Calling the copy constructor of Line
The length of the line is: 5
```



#### 结构体

结构体是一种特殊形态的类

* 与类的唯一区别：类的缺省访问权限时private，结构体的缺省访问权限时public
* 结构体存在的主要原因：与C语言保持兼容
* 什么时候用结构体而不用类
* 定义主要用来**保存数据**、而没有什么操作的类型
* 人们习惯将结构体的数据成员设为公有，因此这时用结构体更方便

```c++
struct 结构体名称 {
  公有成员
    protected:
  保护型成员
    private:
  私有成员
}
```

```c++
#include <iostream>

using namespace std;

struct Student {	//学生信息结构体
	int num;		//学号
	string name;	//姓名，字符串对象，将在第6章详细介绍
	char sex;		//性别
	int age;		//年龄
};

int main() {
	Student stu = { 97001, "Lin Lin", 'F', 19 };
	cout << "Num:  " << stu.num << endl;
	cout << "Name: " << stu.name << endl;
	cout << "Sex:  " << stu.sex << endl;
	cout << "Age:  " << stu.age << endl;
	return 0;
}

运行结果：
Num:  97001
Name: Lin Lin
Sex:  F
Age:  19
```



#### 联合体

存储空间的共用

每一时刻只有一个起作用

```c++
union 联合体名称 {
    公有成员
protected:
    保护型成员
private:
    私有成员
};
```

特点

* 成员共用同一组内存单元
* 任何两个成员不会同时有效

联合体的内存分配

```c++
union Mark { // 表示成绩的联合体
  char grade; // 等级制 A B C F
  bool pass;	// 是否通过课程的成绩
  int percent; // 百分制的成绩
};
```

```c++
#include <string>
#include <iostream>
using namespace std;
class ExamInfo {
  private:
  string name;	// 课程名称
  enum { GRADE, PASS, PERCENTAGE } mode; // 计分方式
  union {
    char grade; // 等级制
    bool pass;
    int percent;
  };
  public:
  ExamInfo (string name, char grade) : name(name), mode(GRADE), grade(grade) {}
  ExamInfo (string name, bool pass) : name(name), mode(PASS), pass(pass) {}
  ExamInfo (string name, int percent) : name(name), mode(PERCENTAGE), percent(percent) {}
  void show();
};

void ExamInfo::show() {
  cout << name << ": ";
  switch (mode) {
    case GRADE: cout << grade; break;
    case PASS: cout << (pass?"PASS":"FAIL"); break;
    case PERCENTAGE: cout << percent; break;
  }
  cout << endl;
}

int main() {
  ExamInfo course1("English", 'B');
	ExamInfo course2("Calculus", true);
	ExamInfo course3("C++ Programming", 85);
	course1.show();
	course2.show();
	course3.show();
	return 0;
}
```



