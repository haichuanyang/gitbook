# syntax basic

```
Chapter 1 C++ introduction and simple order of logic

Programming is a way to control computer. It requires a software environment with a compiler. An Integrated Development Environment (IDE) can be installed and used. Typing speed can be improved with more practice. 
																
A trivial starter C++ program:

 

A. Basic syntax
1.	Define a variable – a variable must be declared first. Its name needs to be unique. Example:
 

Typical variable type and range：
 

max int:2e9; max long long:9e18


1.	Input and output输入输出
Integer input/output 整数的输入输出：
 

String input/output字符串的输入输出：
 

Input out different types of variables输入输出多个不同类型的变量：
 

2.	表达式 expression
Integer add/subtract/multiply/divide整数的加减乘除四则运算：
 
 

浮点数（小数）的运算：floating point(decimal point) operations
 

整型变量的自增、自减：integer type self-increment and self-decrement
 

变量的类型转换：casting a data type
 


3.	顺序语句 order of statements
(1)	输出第二个整数：output the second integer
 

(2)	计算 (a + b) * c的值 compute value of (a+b)*c
 

(3)	带余除法 division with remainder
 

(4)	求反三位数：reverse a 3 digit number
 

(5)	交换两个整数 swap two integers
swap(a,b)
(6)	输出菱形 output a rhombus




 



第二章 printf语句与C++中的判断结构 
printf statement and C++ control structure

学习语言最好的方式就是实践，每当掌握一个新功能时，就要立即将这个功能应用到实践中。
															
一、printf输出格式 printf output format
注意：使用printf 时最好添加头文件 #include <cstdio>。
 
1.	Int、float、double、char等类型的输出格式：
(1)	Int：%d
(2)	Float: %f, 默认保留6位小数 default is 6 decimal points
(3)	Double: %lf， 默认保留6位小数 default is 6 decimal points
(4)	Char: %c, 回车也是一个字符，用’\n’表示 carriage return is a char ‘\n’

 
2.	所有输出的变量均可包含在一个字符串中：all output variable can be printed as a string:
 

练习：输入一个字符，用这个字符输出一个菱形：
Exercise: input a character, and use it to output a rhombus
 

练习：输入一个整数，表示时间，单位是秒。输出一个字符串，用”时:分:秒”的形式表示这个时间。
Exercise: input an integer as time in seconds, output a string showing the time as“hour:minute:second”
 

3.	扩展功能 extension 
(1)	Float, double等输出保留若干位小数时用：%.4f, %3lf
Specifying number of decimal points to keep
 

(2)	最小数字宽度 width of number
a.	%8.3f, 表示这个浮点数的最小宽度为8，保留3位小数，当宽度不足时在前面补空格。
%8.3f means the float width is 8 with 3 decimal digits, with space paddings in the FRONT
 
b.	%-8.3f，表示最小宽度为8，保留3位小数，当宽度不足时在后面补上空格
%8.3f means the float width is 8 with 3 decimal digits, with space paddings in the BACK

 
c.	%08.3f, 表示最小宽度为8，保留3位小数，当宽度不足时在前面补上0
 


二、if 语句
	1. 基本if-else语句
	当条件成立时，执行某些语句；否则执行另一些语句。
	 

	Else 语句可以省略：
 

  当只有一条语句时，大括号可以省略：
 

练习：输入一个整数，输出这个数的绝对值。
 


练习：输入两个整数，输出两个数中较大的那个。
 

If-else语句内部也可以是if-else语句。
练习：输入三个整数，输出三个数中最大的那个。
 


2. 常用比较运算符
  (1) 大于 >
(2) 小于 <
(3) 大于等于 >=
(4) 小于等于 <=
(5) 等于 ==
(6) 不等于 !=





	


3.	If-else 连写：
输入一个0到100之间的分数，
如果大于等于85，输出A；
如果大于等于70并且小于85，输出B；
如果大于等于60并且小于70，输出C；
如果小于60，输出 D；

 


练习：
1.	简单计算器输入两个数，以及一个运算符+, -, *, /，输出这两个数运算后的结果。
当运算符是/，且除数是0时，输出”Divided by zero!”; 当输入的字符不是+, -, *, /时，输出”Invalid operator!”。
 
2.	判断闰年。闰年有两种情况：
(1)	能被100整除时，必须能被400整除；
(2)	不能被100整除时，被4整除即可。
输入一个年份，如果是闰年输出yes，否则输出no。
 

三、条件表达式
	(1) 与 &&
	(2) 或 ||
	(3) 非

	例题：输入三个数，输出三个数中的最大值。
 

练习：用一条if语句，判断闰年。
 



第三章 C++中的循环结构

学习编程语言语法是次要的，思维是主要的。如何把头脑中的想法变成简洁的代码，至关重要。
																	

学习循环语句只需要抓住一点——代码执行顺序！

一、	while循环
可以简单理解为循环版的if语句。If语句是判断一次，如果条件成立，则执行后面的语句；while是每次判断，如果成立，则执行循环体中的语句，否则停止。
 
	练习：求1~100中所有数的立方和。

 
	
	练习：求斐波那契数列的第n项。f(1)=1, f(2)=1, f(3)=2, f(n)=f(n-1) + f(n-2)。
 

死循环：循环永久执行，无法结束。我们要避免写出死循环。
 

二、	do while循环
do while循环不常用。
do while语句与while语句非常相似。唯一的区别是，do while语句限制性循环体后检查条件。不管条件的值如何，我们都要至少执行一次循环。
 

三、	for 循环
基本思想：把控制循环次数的变量从循环体中剥离。
for (init-statement : condition: expression)
{
    statement
}
init-statement可以是声明语句、表达式、空语句，一般用来初始化循环变量；
condition 是条件表达式，和while中的条件表达式作用一样；可以为空，空语句表示true
expression 一般负责修改循环变量，可以为空

 

练习：求1~100中所有数的立方和。
练习：求斐波那契数列的第n项。f(1)=1, f(2)=1, f(3)=2, f(n)=f(n-1) + f(n-2)。

init-statement可以定义多个变量，expression也可以修改多个变量。
例如求 1 * 10 + 2 * 8 + 3 * 7 + 4 * 6：
 

四、	跳转语句
1.	break
可以提前从循环中退出，一般与if语句搭配。
例题：判断一个大于1的数是否是质数：
 

2.	continue
可以直接跳到当前循环体的结尾。作用与if语句类似。
例题：求1~100中所有偶数的和。
 

五、	多层循环

 

练习：打印1~100中的所有质数
 

练习：输入一个n，打印n阶菱形。n是奇数。
n=9时的结果：
 
 



第四章 C++中的数组
程序 = 逻辑 + 数据，数组是存储数据的强而有力的手段。
																	

1.	一维数组
1.1	数组的定义
数组的定义方式和变量类似。
 

1.2	数组的初始化
在main函数内部，未初始化的数组中的元素是随机的。
 

1.3	访问数组元素
通过下标访问数组。
 

	练习题1： 使用数组实现求斐波那契数列的第N项。
 

	练习题2：输入一个n，再输入n个整数。将这n个整数逆序输出。
 

	练习题3：输入一个n，再输入n个整数。将这个数组顺时针旋转k(k <= n)次，最后将结果输出。
 

	练习题4：输入n个数，将这n个数按从小到大的顺序输出。
 

	练习题5：计算2的N次方。N <= 10000
 

2.	多维数组
多维数组就是数组的数组。

Int a[3][4]; // 大小为3的数组，每个元素是含有4个整数的数组。

Int arr[10][20][30] = {0}; // 将所有元素初始化为0
// 大小为10的数组，它的每个元素是含有4个整数的数组
// 这些数组的元素是含有30个整数的数组

 


练习题：输入一个n行m列的矩阵，从左上角开始将其按回字形的顺序顺时针打印出来。
 
第五章 C++中的字符串
	字符串是计算机与人类沟通的重要手段。
																

1.	字符与整数的联系——ASCII码


 

每个常用字符都对应一个-128~127的数字，二者之间可以相互转化.
常用ASCII值：’A’-‘Z’ 是65~90，’a’-‘z’是97-122，’0’-‘9’是48-57。
 
	
	
	字符可以参与运算，运算时会将其当做整数：

	 

	练习：输入一行字符，统计出其中数字字符的个数，以及字母字符的个数。


2.	字符数组
字符串就是字符数组加上结束符’\0’。
可以使用字符串来初始化字符数组，但此时要注意，每个字符串结尾会暗含一个’\0’字符，因此字符数组的长度至少要比字符串的长度多1！
	 

2.1	字符数组的输入输出：
 
		读入一行字符串，包括空格：
		 
2.2	字符数组的常用操作
下面几个函数需要引入头文件:
#include <string.h>

(1)	strlen(str)，求字符串的长度
(2)	strcmp(a, b)，比较两个字符串的大小，a < b 返回-1，a == b 返回0，a > b返回1。这里的比较方式是字典序！
(3)	strcpy(a, b)，将字符串b复制给从a开始的字符数组。

 
2.3	遍历字符数组中的字符：
 

练习：给定一个只包含小写字母的字符串，请你找到第一个仅出现一次的字符。如果没有，输出“no”。

练习：把一个字符串中特定的字符全部用给定的字符替换，得到一个新的字符串。
3.	标准库类型 string
可变长的字符序列，比字符数组更加好用。需要引入头文件：
#include <string>

3.1	定义和初始化
 
3.2	string 上的操作
(1)	string的读写：
 
注意：不能用printf直接输出string，需要写成：printf(“%s”, s.c_str());

(2)	使用getline读取一整行
 

(3)	string的empty和size操作（注意size是无符号整数，因此 s.size() <= -1一定成立）：
 
(4)	string 的比较：
支持 > < >= <= == !=等所有比较操作，按字典序进行比较。

(5)	为string对象赋值：

string s1(10, ‘c’), s2;		// s1的内容是 cccccccccc；s2是一个空字符串
s1 = s2;					// 赋值：用s2的副本替换s1的副本
							// 此时s1和s2都是空字符串

(6)	两个string对象相加：

string s1 = “hello,  ”, s2 = “world\n”;
string s3 = s1 + s2;					// s3的内容是 hello, world\n
s1 += s2;								// s1 = s1 + s2

(7)	字面值和string对象相加：
做加法运算时，字面值和字符都会被转化成string对象，因此直接相加就是将这些字面值串联起来：

		string s1 = “hello”, s2 = “world”;		// 在s1和s2中都没有标点符号
		string s3 = s1 + “, “ + s2 + ‘\n’;
		
当把string对象和字符字面值及字符串字面值混在一条语句中使用时，必须确保每个加法运算符的两侧的运算对象至少有一个是string：

string s4 = s1 + “, “;	// 正确：把一个string对象和有一个字面值相加
string s5 = “hello” +”, “; // 错误：两个运算对象都不是string

string s6 = s1 + “, “ + “world”;  // 正确，每个加法运算都有一个运算符是string
string s7 = “hello” + “, “ + s2;  // 错误：不能把字面值直接相加，运算是从左到右进行的

	3.3 处理string对象中的字符

可以将string对象当成字符数组来处理：

 

或者使用基于范围的for语句：

 

练习：密码翻译，输入一个只包含小写字母的字符串，将其中的每个字母替换成它的后继字母，如果原字母是’z’，则替换成’a’。

练习：输入两个字符串，验证其中一个串是否为另一个串的子串。


第六章 C++中的函数
	函数让代码变得更加简洁。
																
1.	函数基础
一个典型的函数定义包括以下部分：返回类型、函数名字、由0个或多个形参组成的列表以及函数体。

1.1	编写函数
我们来编写一个求阶乘的程序。程序如下所示：
int fact(int val)
{
    int ret = 1;
    while (val > 1)
        ret *= val -- ;
    return ret;
}

函数名字是fact，它作用于一个整型参数，返回一个整型值。return语句负责结束fact并返回ret的值。

1.2	调用函数
int main()
{
    int j = fact(5);
    cout << "5! is " << j << endl;
    
    return 0;
}
函数的调用完成两项工作：一是用实参初始化函数对应的形参，二是将控制权转移给被调用函数。此时，主调函数的执行被暂时中断，被调函数开始执行。

1.3	形参和实参 parameter vs argument (formal parameter vs actual argument)
实参是形参的初始值。第一个实参初始化第一个形参，第二个实参初始化第二个形参，依次类推。形参和实参的类型和个数必须匹配。
fact(“hello”);		// 错误：实参类型不正确
fact(); 			// 错误：实参数量不足
fact(42, 10, 0); 	// 错误：实参数量过多
fact(3.14); 		// 正确：该实参能转换成int类型，等价于fact(3);

形参也可以设置默认值，但所有默认值必须是最后几个。当传入的实参个数少于形参个数时，最后没有被传入值的形参会使用默认值。

1.4	函数的形参列表
函数的形参列表可以为空，但是不能省略。
void f1() {/* …. */}			// 隐式地定义空形参列表
void f2(void) {/* … */} 		// 显式地定义空形参列表
形参列表中的形参通常用逗号隔开，其中每个形参都是含有一个声明符的声明。即使两个形参的类型一样，也必须把两个类型都写出来：
			int f3(int v1, v2) {/* … */} 		// 错误
			int f4(int v1, int v2) {/* … */} 	// 正确

1.5	函数返回类型
大多数类型都能用作函数的返回类型。一种特殊的返回类型是void，它表示函数不返回任何值。函数的返回类型不能是数组类型或函数类型，但可以是指向数组或者函数的指针。

1.6	局部变量、全局变量与静态变量
局部变量只可以在函数内部使用，全局变量可以在所有函数内使用。当局部变量与全局变量重名时，会优先使用局部变量。

2.	参数传递
2.1	传值参数
当初始化一个非引用类型的变量时，初始值被拷贝给变量。此时，对变量的改动不会影响初始值。
 

2.2	传引用参数
当函数的形参为引用类型时，对形参的修改会影响实参的值。使用引用的作用：避免拷贝、让函数返回额外信息。
 

2.3	数组形参
在函数中对数组中的值的修改，会影响函数外面的数组。

一维数组形参的写法：
		// 尽管形式不同，但这三个print函数是等价的
		void print(int *a) {/* … */}
		void print(int a[]) {/* … */}
		void print(int a[10]) {/* … */}

		 

		多维数组形参的写法：
		// 多维数组中，除了第一维之外，其余维度的大小必须指定
		void print(int (*a)[10]) {/* … */}
		void print(int a[][10]) {/* … */}
		 

3.	返回类型和return语句
return 语句终止当前正在执行的函数并将控制权返回到调用该函数的地方。return语句有两种形式：
return;
return expression;
	
3.1	无返回值函数
没有返回值的return语句只能用在返回类型是void的函数中。返回void的函数不要求非得有return语句，因为在这类函数的最后一句后面会隐式地执行return。
通常情况下，void函数如果想在它的中间位置提前退出，可以使用return语句。return的这种用法有点类似于我们用break语句退出循环。
		void swap(int &v1, int &v2)
{
    // 如果两个值相等，则不需要交换，直接退出
    if (v1 == v2)
        return;
    // 如果程序执行到了这里，说明还需要继续完成某些功能
    
    int tmp = v2;
    v2 = v1;
    v1 = tmp;
    // 此处无须显示的return语句
}
3.2	有返回值的函数
只要函数的返回类型不是void，则该函数内的每条return语句必须返回一个值。return语句返回值的类型必须与函数的返回类型相同，或者能隐式地转换函数的返回类型。
 

4.	函数递归
在一个函数内部，也可以调用函数本身。
 

第七章 类、结构体、指针、引用
	类可以将变量、数组和函数完美地打包在一起。
																	
1.	类与结构体
类的定义：
 

		类中的变量和函数被统一称为类的成员变量。
private后面的内容是私有成员变量，在类的外部不能访问；public后面的内容是公有成员变量，在类的外部可以访问。

	类的使用：

	#include <iostream>

using namespace std;

const int N = 1000010;

class Person
{
    private:
        int age, height;
        double money;
        string books[100];
    
    public:
        string name;
        
        void say()
        {
            cout << "I'm " << name << endl;
        }
        
        int set_age(int a)
        {
            age = a;
        }
        
        int get_age()
        {
            return age;
        }
    
        void add_money(double x)
        {
            money += x;
        }
} person_a, person_b, persons[100];

int main()
{
    Person c;
    
    c.name = "yxc";      // 正确！访问公有变量
    c.age = 18;          // 错误！访问私有变量
    c.set_age(18);       // 正确！set_age()是共有成员变量
    c.add_money(100);
    
    c.say();
    cout << c.get_age() << endl;
    
    return 0;
}

结构体和类的作用是一样的。不同点在于类默认是private，结构体默认是public。
 

2.	指针和引用
指针指向存放变量的值的地址。因此我们可以通过指针来修改变量的值。
 
数组名是一种特殊的指针。指针可以做运算：
 
引用和指针类似，相当于给变量起了个别名。

 

3.	链表
 

第八章 C++ STL
	STL是提高C++编写效率的一个利器。
																	
1.	#include <vector>	
vector是变长数组，支持随机访问，不支持在任意位置O(1)插入。为了保证效率，元素的增删一般应该在末尾进行。

	声明
		#include <vector> 	头文件
		vector<int> a;		相当于一个长度动态变化的int数组
		vector<int> b[233];	相当于第一维长233，第二位长度动态变化的int数组
		struct rec{…};
		vector<rec> c;		自定义的结构体类型也可以保存在vector中

	size/empty
size函数返回vector的实际长度（包含的元素个数），empty函数返回一个bool类型，表明vector是否为空。二者的时间复杂度都是O(1)。
所有的STL容器都支持这两个方法，含义也相同，之后我们就不再重复给出。

	clear
		clear函数把vector清空。

	迭代器
		迭代器就像STL容器的“指针”，可以用星号“*”操作符解除引用。
		一个保存int的vector的迭代器声明方法为：
		vector<int>::iterator it;
vector的迭代器是“随机访问迭代器”，可以把vector的迭代器与一个整数相加减，其行为和指针的移动类似。可以把vector的两个迭代器相减，其结果也和指针相减类似，得到两个迭代器对应下标之间的距离。

	begin/end
begin函数返回指向vector中第一个元素的迭代器。例如a是一个非空的vector，则*a.begin()与a[0]的作用相同。
所有的容器都可以视作一个“前闭后开”的结构，end函数返回vector的尾部，即第n个元素再往后的“边界”。*a.end()与a[n]都是越界访问，其中n=a.size()。
下面两份代码都遍历了vector<int>a，并输出它的所有元素。
for (int I = 0; I < a.size(); I ++) cout << a[i] << endl;
for (vector<int>::iterator it = a.begin(); it != a.end(); it ++) cout << *it << endl;

	front/back
		front函数返回vector的第一个元素，等价于*a.begin() 和 a[0]。
		back函数返回vector的最后一个元素，等价于*==a.end() 和 a[a.size() – 1]。

	push_back() 和 pop_back()
a.push_back(x) 把元素x插入到vector a的尾部。
		b.pop_back() 删除vector a的最后一个元素。

2.	#include <queue>
头文件queue主要包括循环队列queue和优先队列priority_queue两个容器。
	
	声明
		queue<int> q;
		struct rec{…}; queue<rec> q; 	//结构体rec中必须定义小于号
		priority_queue<int> q;		// 大根堆
		priority_queue<int, vector<int>, greater<int> q;	// 小根堆
		priority_queue<pair<int, int>>q;

	循环队列 queue
		push 从队尾插入
		pop 从队头弹出
		front 返回队头元素
		back 返回队尾元素

	优先队列 priority_queue
		push 把元素插入堆
		pop 删除堆顶元素
		top 	查询堆顶元素（最大值）

3.	#include <stack>
头文件stack包含栈。声明和前面的容器类似。
push 向栈顶插入
pop 弹出栈顶元素

4.	#include <deque>
双端队列deque是一个支持在两端高效插入或删除元素的连续线性存储空间。它就像是vector和queue的结合。与vector相比，deque在头部增删元素仅需要O(1)的时间；与queue相比，deque像数组一样支持随机访问。
[] 随机访问
begin/end，返回deque的头/尾迭代器
front/back 队头/队尾元素
push_back 从队尾入队
push_front 从队头入队
pop_back 从队尾出队
pop_front 从队头出队
clear 清空队列

5.	#include <set>
头文件set主要包括set和multiset两个容器，分别是“有序集合”和“有序多重集合”，即前者的元素不能重复，而后者可以包含若干个相等的元素。set和multiset的内部实现是一棵红黑树，它们支持的函数基本相同。

声明
		set<int> s;
struct rec{…}; set<rec> s;	// 结构体rec中必须定义小于号
multiset<double> s;

size/empty/clear
		与vector类似

迭代器
set和multiset的迭代器称为“双向访问迭代器”，不支持“随机访问”，支持星号(*)解除引用，仅支持”++”和--“两个与算术相关的操作。
设it是一个迭代器，例如set<int>::iterator it;
若把it++，则it会指向“下一个”元素。这里的“下一个”元素是指在元素从小到大排序的结果中，排在it下一名的元素。同理，若把it--，则it将会指向排在“上一个”的元素。

	begin/end
		返回集合的首、尾迭代器，时间复杂度均为O(1)。
		s.begin() 是指向集合中最小元素的迭代器。
s.end() 是指向集合中最大元素的下一个位置的迭代器。换言之，就像vector一样，是一个“前闭后开”的形式。因此--s.end()是指向集合中最大元素的迭代器。

	insert
		s.insert(x)把一个元素x插入到集合s中，时间复杂度为O(logn)。
		在set中，若元素已存在，则不会重复插入该元素，对集合的状态无影响。

	find
s.find(x) 在集合s中查找等于x的元素，并返回指向该元素的迭代器。若不存在，则返回s.end()。时间复杂度为O(logn)。

	lower_bound/upper_bound
		这两个函数的用法与find类似，但查找的条件略有不同，时间复杂度为 O(logn)。
s.lower_bound(x) 查找大于等于x的元素中最小的一个，并返回指向该元素的迭代器。
s.upper_bound(x) 查找大于x的元素中最小的一个，并返回指向该元素的迭代器。

	erase
设it是一个迭代器，s.erase(it) 从s中删除迭代器it指向的元素，时间复杂度为O(logn)
设x是一个元素，s.erase(x) 从s中删除所有等于x的元素，时间复杂度为O(k+logn)，其中k是被删除的元素个数。

	count
		s.count(x) 返回集合s中等于x的元素个数，时间复杂度为 O(k +logn)，其中k为元素x的个数。

6.	#include <map>
map容器是一个键值对key-value的映射，其内部实现是一棵以key为关键码的红黑树。Map的key和value可以是任意类型，其中key必须定义小于号运算符。

	声明
		map<key_type, value_type> name;
		例如：
		map<long, long, bool> vis;
		map<string, int> hash;
		map<pair<int, int>, vector<int>> test;

	size/empty/clear/begin/end均与set类似。

	Insert/erase
		与set类似，但其参数均是pair<key_type, value_type>。

	find
		h.find(x) 在变量名为h的map中查找key为x的二元组。

	[]操作符
		h[key] 返回key映射的value的引用，时间复杂度为O(logn)。
[]操作符是map最吸引人的地方。我们可以很方便地通过h[key]来得到key对应的value，还可以对h[key]进行赋值操作，改变key对应的value。


第九章 位运算与常用库函数
	C++帮我们实现好了很多有用的函数，我们要避免重复造轮子。
																	

1.	位运算
& 与
| 或
~ 非
^ 异或
>> 右移
<< 左移

常用操作：
(1)	求x的第k位数字  x >> k & 1
(2)	lowbit(x) = x & -x，返回x的最后一位1

2.	常用库函数、
(1)	reverse 翻转
翻转一个vector：
reverse(a.begin(), a.end());
翻转一个数组，元素存放在下标1~n：
reverse(a + 1, a + 1 + n);

(2)	unique 去重
返回去重之后的尾迭代器（或指针），仍然为前闭后开，即这个迭代器是去重之后末尾元素的下一个位置。该函数常用于离散化，利用迭代器（或指针）的减法，可计算出去重后的元素个数。
把一个vector去重：
int m = unique(a.begin(), a.end()) – a.begin();
把一个数组去重，元素存放在下标1~n：
int m = unique(a + 1, a + 1 + n) – (a + 1);

(3)	random_shuffle 随机打乱
用法与reverse相同

(4)	sort
对两个迭代器（或指针）指定的部分进行快速排序。可以在第三个参数传入定义大小比较的函数，或者重载“小于号”运算符。

把一个int数组（元素存放在下标1~n）从大到小排序，传入比较函数：

int a[MAX_SIZE];
bool cmp(int a, int b) {return a > b; }
sort(a + 1, a + 1 + n, cmp);

把自定义的结构体vector排序，重载“小于号”运算符：

struct rec{ int id, x, y; }
vector<rec> a;
bool operator <(const rec &a, const rec &b) {
		return a.x < b.x || a.x == b.x && a.y < b.y;
}
sort(a.begin(), a.end());

(5)	lower_bound/upper_bound  二分
lower_bound 的第三个参数传入一个元素x，在两个迭代器（指针）指定的部分上执行二分查找，返回指向第一个大于等于x的元素的位置的迭代器（指针）。
upper_bound 的用法和lower_bound大致相同，唯一的区别是查找第一个大于x的元素。当然，两个迭代器（指针）指定的部分应该是提前排好序的。

在有序int数组（元素存放在下标1~n）中查找大于等于x的最小整数的下标：
int I = lower_bound(a + 1, a + 1 + n,. x) – a;

在有序vector<int> 中查找小于等于x的最大整数（假设一定存在）：
int y = *--upper_bound(a.begin(), a.end(), x);



```
