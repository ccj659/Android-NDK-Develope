#C++学习笔记1(结构体,命名空间,标准输入输出,引用,函数,构造函数)

##1.C++是对C的增强,C++能够调用C的代码.C是面向过程,注重如何实现算法.而C++则是面向对象,注重如何更好地使用方法.
##2.结构体
```c
在C++中，考虑到C语言到C++语言过渡的连续性，对结构体进行了扩展
，C++的结构体可以包含函数，这样，C++的结构体也具有类的功能
，与class不同的是，结构体包含的函数默认为public，而不是private。
struct Student{
  char name[];
  int age;

}
```

##3.命名空间
```c
//命名空间为了更好地区分不同地方调用不同的变量
//命名空间中可以有结构体,也可以内嵌命名空间
//标准命名空间(包含很多标准的定义)
#include <stdarg.h>

//standard

namespace a{
 int num=1;
 struct Student{
		char name[20];
		int age;
	};
}
namespace b{
int num=2;
namespace bc{
		int c = 90;		
	}
}

//例如 这里的num值完全不同
void main(){
cout<<a::num<<endl;
cout<<b::num<<endl;
system("pause");
}
```
##3.引用&
```c
//引用是指变量的别名(既内存空间的别名),而指针则是变量的地址.
//主要功能为:作为参数,或者返回值.
int a=2;
int &pre_a=a;
cout<<pre_a<<endl;
/**
*引用和指针的区别:
1.引用必须初始化,而指针不必.
2.引用初始化后就不能改变,而指针可以随便指向.
3.引用没有 const，指针有 const，const 的指针不可变；
4.sizeof() 引用,表示引用所指向的变量(对象)所占内存的大小,而指针则为指针本身(指针地址)的大小

*/

//示例:
```c
struct Teacher{
	char name[20];
	int age;
};

void myprint(Teacher *t){
	cout << t->name << "," << t->age << endl;
}

void myprint2(Teacher &t){
	cout << t.name << "," << t.age << endl;
	t.age = 21;
}

void main(){
	Teacher t;

	Teacher *p = NULL;
	//报错，防止不报错，进行非空判断
	myprint(p);

	//引用不能为空，没法传进去
	Teacher &t2 = NULL;
	myprint2(t2);

	system("pause");
}


```
##4.函数
###4.1函数基本
```c
//函数默认参数
/*
void myprint(int x, int y = 9, int z = 8){
	cout << x << endl;
}
//重载
void myprint(int x,bool ret){
	cout << x << endl;
}

void main(){
	myprint(20);

	system("pause");
}
*/

//可变参数
//int...
/*
void func(int i,...)
{
	//可变参数指针
	va_list args_p;
	//开始读取可变参数，i是最后一个固定参数
	va_start(args_p,i);
	int a = va_arg(args_p,int);
	char b = va_arg(args_p, char);
	int c = va_arg(args_p, int);
	cout << a << endl;
	cout << b << endl;
	cout << c << endl;
	//结束
	va_end(args_p);
}


void main(){
	func(9,20,'b',30);

	system("pause");
}
*/

//循环读取
/*
void func(int i,...)
{
	//可变参数指针
	va_list args_p;
	//开始读取可变参数，i是最后一个固定参数
	va_start(args_p,i);
	int value;
	while (1){
		value = va_arg(args_p,int);
		if (value <= 0){
			break;
		}
		cout << value << endl;
	}

	//结束
	va_end(args_p);
}

void main(){
	func(9, 20, 40, 30);

	system("pause");
}
```
###4.2函数指针和指针函数
- 辨别方式就是看函数名前面的指针*号有没有被括号（）包含，如果被包含就是函数指针，反之则是指针函数。一个是指针变量，一个是函数
	
```c
void MyFun(int x) /* 这里定义一个MyFun函数 */
{
	printf("%d\n", x);
}
typedef char *fun(int);

char *func_imp(int x){

	static char *name[] = { "Illegal day",
		"Monday",
		"Tuesday",
		"Wednesday",
		"Thursday",
		"Friday",
		"Saturday",
		"Sunday" };

	return ((x > 0 && x < 8) ? name[x] : name[0]);
}
void main(){
	//函数指针(即函数的指针):
	cout << "********函数指针********" << endl;
	//1.函数指针的声明		
	void(*func_p)(int);
	//2.将指针指向函数 进行调用
	func_p = &MyFun; /* 将MyFun函数的地址赋给FunP变量 */
	//3.指针函数的调用
	func_p(2);
	cout << "********指针函数********" << endl;
	//指针函数(即返回类型带指针的函数):
	//1.声明指针
		char *p;//指针,
	//2.fun_imp的返回值作为
		p = func_imp(2);
	//3.
		cout << p << endl;
	/*辨别方式就是看函数名前面的指针*号有没有被括号（）包含，如果被包含就是函数指针，反之则是指针函数。
	* 一个是指针变量，一个是函数
	*/
	system("pause");
}

```

##5.构造函数
//构造函数分为:无参,有惨,析构,拷贝.
//在类中,初始化时,会默认调用无参构造,当有 有参构造函数时,会直接调用有参构造函数.

```c
#define _CRT_SECURE_NO_WARNINGS
#include"stdio.h"
#include <iostream>
#include"MyTeacher.h"
using namespace std;
//引用作为,参数返回值,
class Teacher{
private :
	char* name;
	int age;

public ://构造函数类似于java

	//默认调用无参构造
	Teacher(){
		this->name = (char*)malloc(100);
		//动态内存 ,需要用strcpy
		strcpy(name, "jack verson");
		cout << "构造函数" << endl;
	}
	//代餐构造
	Teacher(char* name ,int age ){
		this->name = name;
		this->age = age;
		cout << "有参构造函数" << endl;

	}
	//拷贝构造函数 //指针的地址拷贝(浅拷贝)
	//1.被调用场景-->声明的赋值(,座位参数传入 座位返回值的,给变量初始化赋值)
	Teacher(const Teacher &obj){//长引用 
		this->name = obj.name;
		this->age = obj.age;
		cout << "拷贝构造函数" << endl;
	}
	//深拷贝需要重写析构函数 ,变为深拷贝,拷贝指针指向的内容(用malloc)
	//
	Teacher(const Teacher &obj){
		//复制name属性
		int len = strlen(obj.name);
		this->name = (char*)malloc(len+1);
		strcpy(this->name,obj.name);
		this->age = obj.age;
	}
	//析构函数-->当对象要被释放时 调用  作用为善后处理
	//析构函数 没有参数
	~Teacher(){
		cout << "析构函数" << endl;
		free(this->name);
	};
	void myPrint(){
		cout << name << "##" << age << endl;
	}

};
void func(){
	Teacher t1("rose", 20);

	Teacher t2 = t1;
	t2.myprint();
}

void main(){
	func();

	system("pause");
}
```




