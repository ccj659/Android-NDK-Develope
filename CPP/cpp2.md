##C++笔记(继承,多态,虚函数,模板函数,异常捕获)

C++面向对象:
<b color="#0fff">在这里推荐大家一个C++的网站:http://www.cplusplus.com/reference/</b>

---

##1.继承
```c
//继承--子类继承父类(儿子继承爸爸),儿子有了爸爸的特性和财产,提高了代码的重用性.

class A  
{  
public:  
void Func1(void);  
void Func2(void);  
};  
class B : public A  
{  
public:  
void Func3(void);  
void Func4(void);  
};  
// Example  
int main()  
{  
B b; // B的一个对象  
b.Func1(); // B 从A 继承了函数Func1  
b.Func2(); // B 从A 继承了函数Func2  
b.Func3();  
b.Func4();  
return 0;  
}  
```

##2.继承对象的实例化
```c
//继承 解决代码的重用性
class Human{
//一般函数在前面
public :
	//继承中 构造函数 需要和
	Human(char* name, int age){
		this->age = age;
		this->name = name;
	}

	void say(){
		cout << "say" << endl;
	}
private:
	char* name;
	int age;
};
//继承的写法,修饰符:按照最小的范围限制子类
//继承的二义性(即指向不明确): 使用virtual 来区别
class Man :public Human{
public:
	Man(char* brother, char* name, int age) :Human(name, age){
		this->brother = brother;
	}

	//
	void chasing(){
		cout << "泡妞" << endl;
	}
private:
	char* brother;

};

void work(Human& h){

	h.say();
}
//构造函数:先父类,再子类
//析构函数: 先子类,再父类
void main(){
	Man m1("ss","sssss",22);
	m1.say();

	//调用可用指针 和引用 直接赋值

	//Human h1 = m1;
	//h1.say();
	system("pause");
}
```

##3.多继承
```c
//学生，既是人，又是公民
class Student : public Person, public Citizen{
	
};
*/
```
##4.继承的访问限制关系

	一句话:谁最低听谁的

| 基类中|     继承方式    =>| 子类中|
| :-------- | --------:| :--: |
| public     | public      |  public        |
| public     | protected      |  protected|
| public     | private|  private|
| protected| public      |  protected|
| protected| protected      |  protected      |
| protected| private|  private|
| private         | public      |  private    |
| private    | protected      |  private    |
| private    | private|  private    |

##5.virtual

###5.1虚继承
```c
//虚继承，不同路径继承来的同名成员只有一份拷贝，解决不明确的问题
/*
class A{
public:
	char* name;
};

class A1 : virtual public A{
	
};

class A2 : virtual public A{

};

class B : public A1, public A2{

};

void main(){
	B b;	
	b.name = "jason";
	//指定父类显示调用
	//b.A1::name = "jason";
	//b.A2::name = "jason";
	system("pause");
}
```
###5.2虚函数
	虚函数
	多态（程序的扩展性）
	动态多态：程序运行过程中，觉得哪一个函数被调用（重写）
	静态多态：重载

	//发生动态的条件：
	1.继承
	2.父类的引用或者指针指向子类的对象
	3.函数的重写
```c
#include "plane.h"
#include "p1.h"
#include "p2.h"
//业务函数
void bizPlay(plane& p){
	p.fly();
	p.land();
}

void main(){
	Plane p1;
	bizPlay(p1);

	//直升飞机
	p2 p22;
	bizPlay(p22);

	p1 p11;
	bizPlay(p11);

	system("pause");
}
```

###5.3纯虚函数(抽象类)
	类似于java
	//1.当一个类具有一个纯虚函数，这个类就是抽象类
	//2.抽象类不能实例化对象
	//3.子类继承抽象类，必须要实现纯虚函数，如果没有，子类也是抽象类
	//抽象类的作用：为了继承约束，根本不知道未来的实现

##6.接口（逻辑上的划分，语法上跟抽象类的写法相同）
	class Drawble{
	virtual void draw();
	};
	
##7.模板template(泛型)
###7.1模板函数
```c
template<typename T>
void myswap(T& a, T& b){
	T tmp = 0;
	tmp = a;
	a = b;
	b = tmp;
}

void main(){
	//根据实际类型，自动推导
	int a = 10, b = 20;
	myswap<int>(a,b);
	cout << a << "," << b << endl;

	char x = 'v', y = 'w';
	myswap(x, y);
	cout << x << "," << y << endl;

	system("pause");
}
```
###7.2模板类
```c
template<class T>
class A{
public :
	A(T a){
		this->a = a;
	}
protected:
	T a;


};

class B :public A < int > {

public :
	B(int a,int b) :A(a){
		
	}

private:
	
	int   b;

};

//模板类继承模板类
template<class T>
class C :public A <T> {
public :
	C(T c,T a) :A<T>(a){
		this->c = c;

	}
protected:
	T c;
};

void main(){
	
	//实例化
	C<int> c(6,3);
	system("pause");
}
```

##8.异常捕获

//类似于java ,throw是抛出,catch是捕捉,
并且分为自定义异常类,继承异常类,实用常用异常类等.
在C++中有 类似于java中的异常类,[详细的点这里](http://www.cplusplus.com/reference/stdexcept/),常用的如下
```
logic_error--Logic error exception (class )
domain_errorDomain --error exception (class )
invalid_argumentInvalid-- argument exception (class )
length_errorLength --error exception (class )
```

```c


void main(){

	try{
		int age = 300;
		if (age > 200){
			//throw myException(); //throw抛出 (最好不要抛出指针产生对象)
			//throw length_error("超出范围");
			throw NullPointException("为空");
		}

	}
	catch(NullPointException p){
		cout << p.what<<endl;
	}
	//catch (char * error){
	//	cout << error << endl;
	//}
	//catch (myException e){
	//
	//}

	//catch (...){//未知异常
	//	
	//};
	//;

	system("pause");

}
```
