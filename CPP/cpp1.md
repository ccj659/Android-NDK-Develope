##C++笔记(常用关键字new、delete、static、const、friend,operator+-,内存结构)



##1.new ,delete
```c
	//C++ 通过new(delete)动态内存分配
	//而C则是通过 malloc(free)
	Son* s1 = new Son(12);
	//释放对象的堆内存,当对象是数组时,用delete[] s1;
	delete s1;
```
##2.static
```c

//static 静态成员的 赋值只能在全局变量中 一般用于计数
int Teacher::total = 9;
```

##3.const
```c
//const 常量被const修饰的东西都受到强制保护，能提高程序的健壮性。
//它可以修饰函数的参数、返回值，甚至函数的定义体。
//常函数，修饰的是this
//既不能改变指针的值，又不能改变指针指向的内容
const Teacher* const this

```
##4.friend 友元
```c

//友元--指友好的类 或函数,能让其他类或方法,访问类的内部属性

class A{
	//友元函数
	friend void modify_i(A *p, int a);
private:
	int i;
public:
	A(int i){
		this->i = i;
	}
	void myprint(){
		cout << i << endl;
	}
	
};

//友元函数的实现，在友元函数中可以访问私有的属性
void modify_i(A *p, int a){
	p->i = a;
}

void main(){
	A* a = new A(10);
	a->myprint();

	modify_i(a,20);
	a->myprint();

	system("pause");
}
```

##5.运算符重载

```c
运算符重载-->本质是函数的调用,让对象进行算数操作
//当属性私有时候,通过友元函数进行操作
//类中函数声明
//friend Point operator+(Point &p1, Point &p2);
/*函数的实现
Point operator+(Point &p1, Point &p2){
	Point tmp(p1.x + p2.x, p1.y + p2.y);
	return tmp;
}
*/
class Point{
public:
	int x;
	int y;
public:
	Point(int x = 0, int y = 0){
		this->x = x;
		this->y = y;
	}
	void myprint(){
		cout << x << "," << y << endl;
	}
};

//重载+号
Point operator+(Point &p1, Point &p2){
	Point tmp(p1.x + p2.x, p1.y + p2.y);
	return tmp;
}

//重载-号
Point operator-(Point &p1, Point &p2){
	Point tmp(p1.x - p2.x, p1.y - p2.y);
	return tmp;
}

void main(){
	Point p1(10,20);
	Point p2(20,10);
	//运算符重载,本质是函数调用
	Point p3 = p1 + p2;

	p3.myprint();

	system("pause");
}
```

##6.C++内存结构
//C/C++ 内存分区：栈、堆、全局（静态、全局）、常量区（字符串）、程序代码区
	//普通属性与结构体相同的内存布局

	//JVM Stack（基本数据类型、对象引用）
	//Native Method Stack（本地方法栈）
	//方法区
	
