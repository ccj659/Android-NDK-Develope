#C++学习笔记字符串string、vector_deque、queue,multiset、map、multimap、容器拷贝问题(复制粘贴,方便后面翻阅)


##1.string操作
```c
#include <iostream>
#include <string>
#include <algorithm> //算法

using namespace std;

//STL standard template libary 标准模(mu)板库 C++一部分，编译器自带
//Android NDK支持
//java.lang java.util包中API，java的一部分

//string初始化

void main()
{
	//string由c字符串衍生过来的
	string s1 = "craig david";
	string s2("7 days");
	string s3 = s2;
	string s4(10,'a');

	cout << s4 << endl;

	system("pause");
}

//string遍历
void main()
{
	string s1 = "craig david";
	//			 ^
	//1 数组方式
	for (int i = 0; i < s1.length(); i++)
	{
		cout << s1[i] << endl;
	}
	//2 迭代器指针
	for (string::iterator it = s1.begin(); it != s1.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
	//3 at函数(charAt)
	// 可能会抛出异常
	try
	{
		for (int i = 0; i < s1.length() + 3; i++)
		{
			cout << s1.at(i) << " ";
		}
	}
	catch (...)
	{
		cout << "异常" << endl;
	}


	system("pause");
}


//string字符串->c字符串转换
void main()
{
	//string -> char*
	string s1 = "walking away";
	const char* c = s1.c_str();
	printf("%s\n",c);

	//
	string s2 = c;

	//string->char[]
	//从string中赋值字符到char[]
	char arr[50] = {0};
	s1.copy(arr,4,0);
	
	cout << arr << endl;

	system("pause");
}


//字符串拼接
void main()
{
	string s1 = "alan";
	string s2 = "jackson";
	
	//1.
	string s3 = s1 + s2;

	string s4 = " pray";

	//2.
	s3.append(s4);

	cout << s3 << endl;

	system("pause");
}

//字符串查找替换
void main()
{
	string s1 = "apple google apple iphone";
	//从0开始查找"google"的位置
	int idx = s1.find("google", 0);
	cout << idx << endl;

	//统计apple出现的次数
	int idx_app = s1.find("apple",0);
	//npos大于任何有效下标的值
	int num = 0;
	while (idx_app != string::npos)
	{
		num++;
		cout << "找到的索引:" << idx_app << endl;
		idx_app+=5;
		idx_app = s1.find("apple", idx_app);
	}

	cout << num << endl;
	system("pause");
}

void main()
{
	string s1 = "apple google apple iphone";
	//0-5（不包含5）替换为jobs
	s1.replace(0, 5, "jobs");
	cout << s1 << endl;

	//所有apple替换为jobs
	int idx = s1.find("apple", 0);
	while (idx != string::npos)
	{
		s1.replace(idx, 5, "jobs");
		idx += 5;
		idx = s1.find("apple", idx);
	}

	cout << s1 << endl;
	system("pause");
}

//删除（截取）和插入
void main()
{
	string s1 = "apple google apple iphone";
	//删除a，找到a所在的指针
	string::iterator it = find(s1.begin(),s1.end(),'g');
	//只能删除一个字符
	s1.erase(it);
	
	//开头末尾插入字符串
	s1.insert(0, "macos");
	s1.insert(s1.length(), " facebook");

	cout << s1 << endl;
	system("pause");
}

//java StringBuffer才可变
//String 不可变
//大小写转换
void main()
{
	string s1 = "JASON";
	//原始字符串的起始地址，原始字符串的结束地址, 目标字符串的起始地址, 函数名称
	transform(s1.begin(), s1.end()-1,s1.begin(), tolower);
	cout << s1 << endl;


	transform(s1.begin(), s1.end() - 1, s1.begin(), toupper);
	cout << s1 << endl;

	system("pause");
}


```
##2.容器vector
```c
//容器
//Vector
//初始化
#include <vector>
void printVector(vector<int> &v)
{
	//通过数组的方式遍历
	for (int i = 0; i < v.size(); i++)
	{
		cout << v[i] << endl;
	}
}
void main()
{
	//1.
	vector<int> v1;
	v1.push_back(20);
	v1.push_back(40);
	v1.push_back(15);
	v1.push_back(7);

	//2.
	vector<int> v2 = v1;

	//3.部分复制
	vector<int> v3(v1.begin(),v1.begin()+2);
	
	printVector(v3);
	

	system("pause");
}

//添加 删除
void main()
{
	//添加到结尾
	vector<int> v1;
	v1.push_back(20);
	v1.push_back(40);
	v1.push_back(15);
	v1.push_back(7);

	//访问头部
	v1.front() = 11;
	//访问尾部
	v1.back() = 90;

	//删除结尾的元素
	//v1.pop_back();
	while (v1.size() > 0)
	{
		cout << "末尾的元素：" << v1.back() << endl;
		v1.pop_back();
	}

	printVector(v1);

	system("pause");
}

//数组的方式
void main()
{
	vector<int> v1;
	v1.push_back(20);
	v1.push_back(40);
	v1.push_back(15);
	v1.push_back(7);

	v1[2] = v1[2] +10;

	//容器等价于动态数组	
	vector<int> v2(10);
	for (int i = 0; i < v2.size(); i++)
	{
		v2[i] = i + 1;
	}

	printVector(v2);

	system("pause");
}

//迭代器遍历
//迭代器的种类（正向，反向迭代器）
void main()
{
	vector<int> v1;
	v1.push_back(20);
	v1.push_back(40);
	v1.push_back(15);
	v1.push_back(7);
	//正向
	for (vector<int>::iterator it = v1.begin(); it < v1.end(); it++)
	{
		cout << *it << endl;
	}
	cout << "-----------------" << endl;
	//反向迭代
	for (vector<int>::reverse_iterator it = v1.rbegin(); it < v1.rend(); it++)
	{
		cout << *it << endl;
	}

	system("pause");
}

//删除
void main()
{
	vector<int> v1(10);
	for (int i = 0; i < v1.size(); i++)
	{
		v1[i] = i + 1;
	}

	//删除指定位置
	vector<int>::iterator it = v1.begin();
	it += 3;
	v1.erase(it);

	//distance(v1.begin(), it);

	//删除区间
	v1.erase(v1.begin(), v1.begin() + 3);

	for (vector<int>::iterator it = v1.begin(); it < v1.end(); it++)
	{
		if (*it == 5)
		{		
			printf("%x\n", it);
			vector<int>::iterator tmp = v1.erase(it); //注意以后开发中编译器版本问题
			printf("%x,%x\n",it,tmp);
		}
	}

	//插入
	v1.insert(v1.begin() + 2, 100);
	v1.insert(v1.end() - 1, 200);

	printVector(v1);

	system("pause");
}

```

##3.队列deque
```c
//双向队列
#include <deque>
void printDeque(deque<int>& q)
{
	for (int i = 0; i < q.size(); i++)
	{
		cout << q[i] << endl;
	}
}

void main()
{
	deque<int> d1;
	//添加到尾部
	d1.push_back(2);
	d1.push_back(10);
	//添加到头部
	d1.push_front(-90);
	d1.push_front(-30);	

	//printDeque(d1);

	//cout << d1.front() << endl;
	//cout << d1.back() << endl;

	//两个方向弹出
	//d1.pop_back();
	//d1.pop_front();

	printDeque(d1);

	//查找第一个-90元素索引位置，无需遍历
	deque<int>::iterator it = find(d1.begin(),d1.end(),-90);
	if (it != d1.end())
	{
		int idx = distance(d1.begin(), it);
		cout << "索引位置为：" << idx << endl;
	}


	system("pause");
}
//队列（没有迭代器）
/*
void main()
{
	queue<int> q;
	q.push(78);
	q.push(18);
	q.push(20);
	q.push(33);
	
	//q.front();
	//q.back();
	while (!q.empty())
	{
		int tmp = q.front();
		cout << tmp << endl;
		q.pop();
	}	
	system("pause");
}
*/

//优先级队列
/*
#include <functional>

void main()
{
	//默认 最大值优先级
	priority_queue<int> pq1;
	pq1.push(12);
	pq1.push(3);
	pq1.push(40);
	pq1.push(15);

	while (!pq1.empty())
	{
		int tmp = pq1.top();
		cout << tmp << endl;
		pq1.pop();
	}

	cout << "----------" << endl;
	//最小值优先级队列
	priority_queue<int, vector<int>,greater<int>> pq2;
	pq2.push(12);
	pq2.push(3);
	pq2.push(40);
	pq2.push(15);

	while (!pq2.empty())
	{
		int tmp = pq2.top();
		cout << tmp << endl;
		pq2.pop();
	}

	system("pause");
}
*/
```


##4.栈stack

```c
//栈
/*
#include <stack>
void main()
{
	stack<int> s;
	for (int i = 0; i < 10; i++)
	{
		s.push(i+1);
	}
	
	while (!s.empty())
	{
		int tmp = s.top();
		cout << tmp << endl;
		s.pop();
	}
	
	system("pause");
}
*/

```


##5.集合list,set,map
```c
//list
#include <list>
void printList(list<int>& lst)
{
	//迭代器
	//没有重载“<”运算符
	for (list<int>::iterator it = lst.begin(); it != lst.end(); it++)
	{
		cout << *it << endl;
	}
}
//基本操作
/*
void main()
{
	list<int> lst;
	for (int i = 0; i < 10; i++)
	{
		//尾部插入元素
		lst.push_back(i);
	}

	//头部插入元素
	lst.push_front(80);
	lst.push_front(90);

	list<int>::iterator it = lst.begin();
	it++;
	cout << *it << endl;
	//it = it + 3; 注意：不支持随机访问		

	printList(lst);

	system("pause");
}
*/


//删除
/*
void main()
{
	list<int> lst;
	for (int i = 0; i < 10; i++)
	{
		//尾部插入元素
		lst.push_back(i);
	}

	list<int>::iterator it = lst.begin();
	//删除
	it++;
	//删除第二个元素
	//lst.erase(it);

	//删除区间（已经被删除了元素不能再删除）
	list<int>::iterator it_begin = lst.begin();
	list<int>::iterator it_end = lst.begin();
	it_end++;
	it_end++;
	it_end++;
	lst.erase(it_begin, it_end);

	//直接根据内容删除元素
	lst.remove(5);

	printList(lst);

	system("pause");
}
*/

//list插入（应用：频繁的修改）
//vector（应用：随机访问v[100]）
/*
void main()
{
	list<int> lst;
	for (int i = 0; i < 10; i++)
	{
		//尾部插入元素
		lst.push_back(i);
	}

	list<int>::iterator it = lst.begin();
	it++;
	lst.insert(it, 100);

	printList(lst);
	system("pause");
}
*/

//set 元素唯一 默认从小到大
#include <set>

void printSet(set<int> &s)
{
	for (set<int>::iterator it = s.begin(); it != s.end(); it++)
	{
		cout << *it << endl;
	}
}
/*
void main()
{
	set<int> s;
	//添加元素
	for (int i = 0; i < 10; i++)
	{
		s.insert(i+1);
	}
	s.insert(20);
	s.insert(15);
	s.insert(15);

	//删除
	set<int>::iterator it = s.begin();
	it++;
	s.erase(it);	

	printSet(s);
	system("pause");
}
*/

//元素按照从大到小排列
/*
#include <functional>
void main()
{
	//同Java中：Map<String,List<String>> 
	set<int,greater<int>> s;
	s.insert(10);
	s.insert(5);
	s.insert(20);
	s.insert(99);

	for (set<int,greater<int>>::iterator it = s.begin(); it != s.end(); it++)
	{
		cout << *it << endl;
	}

	system("pause");
}
*/

//元素类型为Teacher对象，按照年龄排序
/*
class Teacher
{
public:
	Teacher(char* name, int age)
	{
		this->name = name;
		this->age = age;
	}

	void print()
	{
		cout << name << "," << age << endl;
	}	

public:
	char* name;
	int age;
};

//自定义排序规则
//仿函数
struct MyAgeSorter
{
	bool operator()(const Teacher &left, const Teacher &right)
	{
		return left.age < right.age;
	}
};

void main()
{
	set<Teacher, MyAgeSorter> s;
	s.insert(Teacher("jack",18));
	s.insert(Teacher("rose", 20));
	s.insert(Teacher("jason", 22));
	s.insert(Teacher("alan", 5));
	//s.insert(Teacher("jimy", 5)); //不会插入

	for (set<Teacher>::iterator it = s.begin(); it != s.end(); it++)
	{
		cout << (*it).name << "," << (*it).age << endl;
	}

	system("pause");
}
*/

//set查找
/*
void main()
{
	set<int> s;
	//添加元素
	for (int i = 0; i < 10; i++)
	{
		s.insert(i + 1);
	}

	//printSet(s);

	//等于4的元素指针
	set<int>::iterator s_4 = s.lower_bound(4); 
	//cout << *s_4 << endl;
	//大于4的元素指针
	set<int>::iterator s_5 = s.upper_bound(4);
	//cout << *s_5 << endl;

	//一次性获取等于4的元素指针，和大于4的元素指针\
	//BasicNameValuePair
	pair<set<int>::iterator, set<int>::iterator> p = s.equal_range(4);
	cout << *p.first << endl;
	cout << *p.second << endl;
	system("pause");
}
*/


//multiset 允许重复的元素
/*
void main()
{
	multiset<int> s;
	s.insert(2);
	s.insert(8);
	s.insert(2);
	s.insert(8);

	for (multiset<int>::iterator it = s.begin(); it != s.end(); it++)
	{
		cout <<  *it << endl;
	}

	system("pause");
}
*/


//map添加元素的方式
#include <map>
#include <string>
/*
void main()
{
	//key -> value
	//1.
	map<int, string> map1;
	map1.insert(pair<int, string>(1,"jack"));
	map1.insert(pair<int, string>(2, "rose"));

	//2
	map1.insert(make_pair(3,"jason"));

	//3
	map1.insert(map<int,string>::value_type(4,"alan"));

	//4
	map1[5] = "jimmy"; //map["NO1"] = 90;

	//前三种方式，如果key已经存在，重复添加会报错
	//第四种方式，如果key已经存在，重复添加会覆盖
	
	//遍历输出
	for (map<int, string>::iterator it = map1.begin(); it != map1.end(); it++)
	{
		cout << it->first << "," << it->second << endl;		
	}


	system("pause");
}

*/

void printMap(map<int, string> &map1)
{
	for (map<int, string>::iterator it = map1.begin(); it != map1.end(); it++)
	{
		cout << it->first << "," << it->second << endl;
	}
}

//删除
/*
void main()
{
	map<int, string> map1;
	map1.insert(pair<int, string>(1, "jack"));
	map1.insert(pair<int, string>(2, "rose"));
	map1.insert(pair<int, string>(3, "jason"));	

	map<int, string>::iterator it = map1.begin();
	it++;
	map1.erase(it);

	printMap(map1);

	system("pause");
}
*/

//添加的结果
/*
void main()
{
	map<int, string> map1;
	map1.insert(pair<int, string>(1, "jack"));
	map1.insert(pair<int, string>(2, "rose"));
	map1.insert(pair<int, string>(3, "jason"));
	//获取添加的结果（first元素指针，second 是否成功）
	pair<map<int, string>::iterator, bool> res = map1.insert(pair<int, string>(3, "alan"));
	if (res.second)
	{
		cout << "添加成功" << endl;
	}
	else
	{
		cout << "添加失败" << endl;
	}

	printMap(map1);

	system("pause");
}
*/


//查找
/*
void main()
{
	map<int, string> map1;
	map1.insert(pair<int, string>(1, "jack"));
	map1.insert(pair<int, string>(2, "rose"));
	map1.insert(pair<int, string>(3, "jason"));	

	printMap(map1);

	cout << "---------" << endl;

	//获取key等于大于5的元素的值
	pair<map<int, string>::iterator, map<int, string>::iterator> p = map1.equal_range(2);
	if (p.first != map1.end()){
		//等于2的元素key value
		cout << p.first->first << p.first->second << endl;

		//大于2的元素key value
		cout << p.second->first << p.second->second << endl;
	}

	system("pause");
}
*/

//一个key对应多个value
//一个部门多个员工
//multimap
/*
class Employee
{
public:
	Employee(char* name,int age)
	{
		this->name = name;
		this->age = age;
	}

public:
	char* name;
	int age;
};

void main()
{
	multimap<string, Employee> map1;
	
	//开发部
	map1.insert(make_pair("开发", Employee("搁浅", 20)));
	map1.insert(make_pair("开发", Employee("彪哥", 20)));

	//财务
	map1.insert(make_pair("财务", Employee("小颖", 16)));
	map1.insert(make_pair("财务", Employee("rose", 20)));

	//销售
	map1.insert(make_pair("销售", Employee("阿呆", 30)));
	map1.insert(make_pair("销售", Employee("呵呵", 30)));

	//遍历输出
	for (multimap<string, Employee>::iterator it = map1.begin(); it != map1.end(); it++)
	{
		cout << it->first << "," << it->second.name  << "," << it->second.age << endl;
	}

	cout << "----------------" << endl;
	//只获取“财务”部的员工
	//获取“财务部”员工的个数，key对应的value的个数
	int num = map1.count("财务");
	multimap<string, Employee>::iterator it = map1.find("财务");
	int c = 0; //控制循环的次数
	while (it != map1.end() && c < num)
	{		
		cout << it->first << "," << it->second.name << "," << it->second.age << endl;
		it++;
		c++;
	}

	system("pause");
}
*/
//深拷贝与浅拷贝
/*
class Employee
{
public:
	//构造函数
	Employee(char* name, int age)
	{		
		this->name = new char[strlen(name) + 1];
		strcpy(this->name, name);
		this->age = age;
	}

	//析构函数
	~Employee()
	{
		if (this->name != NULL)
		{
			delete[] this->name;
			this->name = NULL;
			this->age = 0;
		}
	}
	//拷贝构造函数
	//Employee e = 
	Employee(const Employee &obj)
	{
		this->name = new char[strlen(obj.name) + 1];
		strcpy(this->name, obj.name);
		this->age = obj.age;
	}

	//重载=
	//e1 = e2;
	Employee& operator=(const Employee &obj)
	{
		//释放旧的内存
		if (this->name != NULL)
		{
			delete[] this->name;
			this->name = NULL;
			this->age = 0;
		}

		//重新分配
		this->name = new char[strlen(obj.name) + 1];
		strcpy(this->name, obj.name);
		this->age = obj.age;

		return *this;
	}

public:	
	char* name;
	int age;
};

void func()
{
	vector<Employee> v1;
	Employee e1("jack",20);	
	v1.push_back(e1);
}

void main()
{
	//vector<Employee> v1;
	//Employee e1("jack",20);
	//将e1拷贝到vector中
	//v1.push_back(e1);

	func();

	system("pause");
}
*/

```
