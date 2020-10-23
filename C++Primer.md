[TOC]

## 第一部分 C++基础

### 第2章 变量和基本类型

#### 2.1 基本内置类型

除去布尔型和扩展型的字符型之外，其他整形都可以划分为带符号的和无符号的两种。

单引号括起来的一个字符称为char型字符值，双引号括起来的零个或者多个字符构成了字符串型字符。

（编译器在每个字符串的结束处添加一个空字符‘\0’，这就导致了字符串的实际长度比内容多1）

**转义序列：**

如果反斜杠\后面跟随的八进制数字超过3个，只有前3个数字与\构成转义序列，相反\x要用到后面所有的十六进制数字。

####  2.2 变量

始化不是赋值，初始化的含义是创建变量是赋予其一个初始值，而赋值的含义是把对象的当前值擦除，而以一个新值来代替。

声明使得名字为程序所知，一个文件如果想使用别处定义的名字，则必须包含对那个名字的声明，而定义负责创建与名字关联的实体。

如果想声明一个变量而不是定义他，就要在变量名前添加关键字extern，而且不要显式地初始化变量。

`extern int i; //声明i`

`extern in i = 10; //定义i`

在函数内部，如果试图初始化一个由extern关键字标记的变量，将引发错误。

变量只能被定义一次，但是可以被多次声明

C++的标识符由字母、数字和下划线构成，其中必须以下划线或者字母开头，对大小写敏感。

  

```c++
# include <iostream>
int reused = 42; //全局变量
int main()
{	
	int reused = 0;
	std::cout << reused << std::endl;
	# 输出为局部变量0
	std::cout << ::reused << std::endl;
	# 输出为全局变量42

}
```

####  2.3 复合类型

C++最常用的两种复合类型：引用和指针

引用必须被初始化

因为引用不是一个对象，所以不能定义引用的引用

```c++
# 定义指针的两种方法
double dval;
double *pd = &dval;
double *pdd ;
pdd = &dval;
```

**空指针**

以下给出了几种定义空指针的方法

```c++
int *p = nullptr;
int *p = 0;

// 需要先 #include cstdlib
int *p = NULL;
```

void* 是一种特殊的指针类型，可以存放任意对象的地址

**复合类型的声明**

`int i = 1024,*p = &i,&r = i;`

在上面这个命令中，p是一个指针，r是一个引用

注意：

`int* p1,p2;`

这样的写法很容易产生错误，因为*仅仅修饰了p1，而对p2不产生影响。

引用本事不是一个对象，所以不能定义指向引用的指针，但是指针是对象，所以存在对指针的引用。

```
int i = 42；
int *p；
int *&r = p;
// r是一个对指针p的引用

```

**离变量最近的符号对变量的类型有最直接的影响，因为在上面的例子中，r是一个引用。声明符的其余部分用以确定r引用的类型是什么，符号*说明r引用的是一个指针，最后，声明的基本数据类型说明r引用的是一个int指针**

#### 2.4 const限定符

因为const对象一旦创建后其值就不能发生改变，所以const对象必须初始化。

默认情况下，const对象被设定为仅在文件内有效，当多个文件中出现了同名的const变量时，其实等同于在不同文件中分别定义了独立的变量。

但某些时候，有这样一种const变量，它的初始值不是一个常量表达式，但又确实有必要在文件间共享，对于这种变量不管是声明还是定义都添加上`extern`关键字，这样只需要定义一次就可以了。

**const的引用**

```c++
const int ci = 1024；
const int &r1 = ci;
//引用及其对应的对象都是常量
//通常，引用的类型必须和其引用对象类型一致
//允许为一个常量引用绑定非常量的对象、字面值、甚至是一般表达式
int i = 2048;
const int &r2 = i;
const int &r3 = 42;
const int &r4 = r2 * 2;

```

**指针和const**

不能用普通指针指向常量

也不能给指向常量的指针赋值

和常量引用一样，指向常量的指针也没有规定其指向的对象必须是一个常量。所谓指向常量的指针仅仅要求不能通过该指针改变对象的值，而没有规定那个对象的值不能通过其他路径改变。

**指针是对象，但是引用不是，所以可以把指针本身定义为常量，**常量指针必须初始化，一旦初始化完成，就不能再改变了。

```c++
int errNumb = 0;
int *const curErr = &errNumb;// curErr将一直指向errNumb
const double pi = 3.141595;
const double *const pip = &pi;// pip是一个指向常量对象的常量指针
//pip所指的对象和pip自己的地址都不能改变
//相反，可以用curErr修改errNumb的值
*curErr = 0；

```

**顶层const**

用顶层const表示指针本身是个常量，用名词底层const表示指针所指的对象是一个常量。

```c++
int i = 0；
int *const p1 = &i;	//顶层const
const int ci = 42;
const int *p2 = &ci; //底层const
const int *const p3 = p2;	//既有顶层const部分，又有底层const部分
p2 = p3;	//顶层const部分不受影响
```

**constexpr和常量表达式**



常量表达式是指值不会改变并且在编译过程就能得到计算结果的表达式。

允许将变量声明为constexpr类型以便由编译器来验证变量的值是否是一个常量表达式。声明为constexpr的变量一定是一个常量，而且必须用常量表达式初始化。

一般来说，如果你认定变量是一个常量表达式，那就把它声明为constexpr类型。

尽管指针和引用都能定义为constexpr，但它们的初始值却受到严格限制，一个constexpr指针的初始值必须是nullptr或者0，或者是存储在某个固定地址中的对象。

如果在constexpr声明中定义一个指针，限定符constexpr仅仅对指针有效，与指针所指的对象无关。

```c++
const int *p = nullptr;	//p是指向整形常量的指针
constexpr int *q = nullptr;	//q是指向整数的常量指针
```

####  2.5 处理类型

类型别名是一个名字，他是某种类型的同义词。

有两种方法可以定义类型别名，传统方法是使用关键字typedef：

```c++
typedef double wages;
typedef wages base,*p;	//base是double的同义词，p是double*的同义词
```

新规定了一种方法，使用别名声明来定义类型的别名：

```c++
using SI = Sale_item;	//SI是Sale_item的同义词
```

类型别名和类型的名字等价，只要是类型的名字能出现的地方，就能使用类型别名。

```c++
typedef char *pstring;
const pstring cstr = 0;	//cstr是指向char的常量指针
const pstring *ps;	//ps是一个指针，他的对象是指向char的常量指针
const char *cstr = 0; //和上面的式子不一样，cstr是一个指向const char的指针
```



**auto类型符**

用auto可以让编译器替我们分析表达式所属的类型

```c++
auto item = val1 + val2;	//item初始化为相加的结果
auto i = 0, *p = &i;	//正确，i是整型数据，p是整形指针
auto sz = 0, pi = 3.14;	//错误
```

因为一条声明语句只能有一个基本数据类型，所以该语句中所有变量的初始基本数据类型都一样。

当引用被用作初始值时，此时编译器以引用对象的类型作为auto的类型。

auto一般会忽略顶层const，同时底层const则会保留下来：

```c++
const int ci = i;
auto b = ci;	//b是一个整数（顶层const被忽略掉了）
auto c = &ci;	//c是一个指向整数常量的指针（底层const没有被忽略掉）
```

设置一个类型为auto的引用时，初始值中的顶层常量属性仍然保留。

**decltype**

希望从表达式的类型推断出要定义的变量类型，但是不想用该表达式的值初始化变量。

```
const int ci = 0, &cj = ci; 
decltype(ci) x = 0;	//x的类型是const int
decltype(cj) y = x;	//y的类型是const int&，y绑定到变量x
decltype(cj) z;	//错误，z是一个引用必须初始化
```

如果decltype使用的表达式不是一个变量，则返回表达式结果对应的类型。

```c++
int i = 42,*p = &i, &r = i;
decltype(r + 0) b;	//b是int型数据，而不是引用
decltype(*p) c;	//错误，c是引用，必须初始化

```

如果给变量加上一层或者多层括号，编译器会把他当成一个表达式，双层括号的结果永远是引用。

```c++
decltype((i)) d;	//错误，d是引用必须初始化
```

#### 2.6 自定义数据结构

**头文件保护符**

`#ifdef`当且仅当变量已定义为真时

`#ifndef`当且仅当变量未定义为真时

一旦检查结果为真，则实行后续操作直到遇到`#endif`指令为止。

注意：预处理变量无视C++语言中关于作用域的规则

整个程序中的预处理变量包括头文件保护符必须统一，通常的做法是基于头文件中类的名字来构建保护符的名字，以确保其唯一性，为了避免与程序中的其他实体发生名字冲突，一般把预处理变量的名字全部大写。



### 第3章 字符串、向量和数组

#### 3.1 命名空间的using声明

头文件不应包含using声明

#### 3.2 标准库类型string

初始化string对象的方式

```c++
string s1 = s2;
string s2(s1);
string s3("value");
string s4(n,'c'); //把s4初始化为由n个字符c组成的串
```

**直接初始化和拷贝初始化**

如果使用等号初始化一个变量，实际执行的拷贝初始化，反之，执行的是直接初始化。

**string的操作**

- `os<<s`	将s写到输出流os当中，返回os
- `is>>s`	从is中读取字符串赋给s，字符串以空格分割，返回is
- `getline(is,s)`	从is中读取一行赋给s，返回is
- `s.empty()`	s为空返回true，否则false
- `s.size()`	返回s中字符串的个数

用IO操作读取string对象的时候，会自动忽略开头的空白（即空格符、换行符、制表符）并从第一个真正的字符开始读起，直到遇见下一处空白为止。因此，可以多个输入或者多个输出连写在一起。

**getline**

有时候，我们希望能在得到的字符串中保留输入时的空白符，这时应该用getline函数。函数从给定的输入流中读入内容，直到遇到换行符为止（注意换行符被读进来了），然后把内容存到string对象中（不存换行符）。getline只要一遇到换行符就结束读取操作并返回结果，哪怕输入的一开始就是换行符。

**s.size()**

返回值是一个string::size_type，它是一个无符号的值，如果一条表达式中已经有了size()函数就不要再使用int了，这样可以避免一些错误。

**string对象的比较**

如果两个string对象的长度不同，且包含内容相同，则较短的小于较长的string对象

如果两个string对象在某些位置不同，则string对象比较的结果就是第一对相异字符比较的结果。

**string对象相加**

当把两个string对象和字符字面值混在一条语句中使用时，必须确定每个加法运算的两侧运算对象至少有一个是string：

```c++
string s1 = "hello" , s2 = "world";
string s3 = s1 + '' + s2;	//正确
string s4 = "hello" + "world";	//错误
```

![image-20191204203756466](/assets/image-20191204203756466.jpg)

C++中兼容了很多C中的库，C语言中的头文件形如`name.h`，C++将这些文件命名为`cname`

特别的，在名为cname的头文件中定义的名字从属于命名空间std，但定义在`.h`的头文件中的则不然。

**范围for语句**

这种语句用来遍历给定序列中的每个元素。

```c++
string str("some strings");
for(auto c : str)
	cout << c << endl;
```

如果想改变string对象中字符的值，必须把循环变量定义为引用变量。



#### 3.3 标准库类型vector

再问头文件中做如下声明：

```c++
#include <vector>
using std::vector;
```

C++既有类模板，也有函数模板，其中vector是一个类模板。在模板名字后面跟一对尖括号，在括号中放上信息。

```c++
vector<int> ivec;
vector<Sales_item> Sales_vec;	//保存Sales_item类型的对象
vector<vector<string>> file; //该向量中的元素是vector对象
```

vector能容纳绝大多数类型的对象作为其元素，但是因为引用不是对象，所以不存在包含引用的vector，但组成vector的元素也可以是vector。

![image-20191204204043033](/assets/image-20191204204043033.jpg)

当然，也可以采用列表初始化vector对象的方法，如果提供的是初始元素值的列表，则只能把初始值都放在花括号里进行列表的初始化，而不能放在圆括号里。

**想vector对象中添加元素**

使用vector的成员函数向其添加元素，push_back负责把一个值当成vector对象的尾元素压到对象的尾端。

```
string word；
vector<string> text;
while(cin >> word)
{
	text.push_back(word);
}
```

**注意：**如果循环体内部包含有向vector对象添加元素的语句，则不能使用范围for循环。

**vector操作**

![image-20191204204117056](/assets/image-20191204204117056.jpg)

同字符串的操作，只有把控制变量定义为引用类型，采用通过控制变量给vector对象赋值。

size函数返回的是：`vector<int>::size_type`

使用下标运算符能获得到指定的元素，但是不能通过下标形式添加元素，这也说明了，只能对已存在的元素执行下标操作。



3.4 迭代器介绍

迭代器都有名为begin和end的成员，其中begin成员负责返回指向第一个元素的迭代器，end成员返回指向容器（或string对象）尾元素的下一个位置的迭代器。

如果容器为空，则begin和end返回的是同一个迭代器，都是尾后迭代器。

因为end迭代器返回的并不实际指向某个元素，所以不能对其进行递增或者解引用操作。

![image-20191204204142245](/assets/image-20191204204142245.jpg)

**迭代器的类型**

```c++
vector<int>::iterator it;	//it可以读写vector<int>的元素
string::iterator it2;	//it2能读写

vector<int>::const_iterator it3;	//只读
string::const_iterator it4;	//只读
```

两个新函数cbegin，cend，无论vector本身是否是常量，返回值都是const_iterator。

**解引用和成员访问**

注意如下区别：

```c++
(*it).empty() //it是一个vector对象的迭代器，解引用it，然后调用结果对象的empty成员
*it.empty()	//错误，试图访问it的empty成员，但it是个迭代器，没有empty成员
it->empty() //同第一个，将解引用和成员访问放在一起。
```

**谨记，但凡是使用了迭代器的循环体，都不要向迭代器所属的容器添加元素**

![image-20191204204205504](/assets/image-20191204204205504.jpg)

只有两个迭代器指向的是同一个容器中的元素或者尾元素的下一个位置，才能进行减操作。



#### 3.5 数组

数组的大小确定不变，不能随意向数组中添加元素。

```c++
unsigned cnt = 42;
string bad[cnt]; //错误，cnt不是常量表达式
```

定义数组的时候必须指定数组的类型，不允许auto关键字由初始值的列表推断类型。另外，数组的元素为对象，所以不存在引用的数组。

由定义而言，不允许用一个数组给其他数组拷贝或者赋值。但是一些编译器支持，这就是编译器的扩展。

数组可以有引用，但是数组不能存放引用。

```c++
int arr[10] = {0};
int &refs[10] //错误，不存在引用的数组
int (*Parray)[10] = &arr;	//Parray指向一个含有10个整数的数组
int (&arrRef)[10] = arry;	//Parray引用一个含有10个整数的数组
```

**访问数组元素**

在使用数组下标的时候，通常将其定义为size_t类型，在cstddef头文件中定义。

**数组和指针**

```c++
int ia[] = {0,1,2,3,4,5,6,7,8,9};
auto ia2(ia);	//ia2是一个整形指针，指向ia的第一个元素
auto ia2(&ia[0]);	//同上
decltype(ia) ia3 = {0,1,2,3,4,5,6,7,8,9}; //返回的类型是10个整数构成的指针
```

iterator头文件中定义了两个函数，begin和end，注意这两个函数不是成员函数

```c++
int *beg = begin(ia);
int *last = end(ia);
```

如果两个指针相减，返回的是一种类型为ptrdiff_t的标准库类型，这是一种带符号类型。

数组内置的下标运算所用的索引值可以为负数。

![image-20191204204231604](/assets/image-20191204204231604.jpg)

此类函数的指针必须指向以空字符作为结束的标志。

**使用数组初始化vector对象**

```c++
int int_arr[] = {0,1,2,3,4,5};
vector<int> ivec(begin(int_arr),end(int_arr));
```



#### 3.6 多维数组

通常说的多维数组，其实是数组的数组。

对于，二维数组而言，通常把第一个维度称为行，第二个维度称为列。

```c++
int ia[3][4] = {
	{0,1,2,3},
	{4,5,6,7},
	{8,9,10,11},
};
int (&row)[4] = ia[1]; //把row绑定到ia的第二个4元素数组上
size_t cnt = 0;
for(auto &row : ia)
    for(auto &col : row){
        col = cnt;
        ++cnt;
    }
```

**使用范围for语句处理多维数组（读或者写），除了最内层的循环外，其他循环控制变量都要用引用类型**

**指针和多维数组**

```
int (*)p[4]; //指向含有4个整数的数组
int *p[4]; //整形指针的数组
```



### 第4章 表达式

#### 4.1基础

对于没有指定执行顺序的运算符来说，如果表达式指向并修改了同一个对象，将会引发错误并产生未定义的行为。

```c++
int i = 0;
cout << i << " " << ++i << endl; //<<没有明确规定如何运算对象求值
```



####4.2 算术运算符

```c++
bool b = true;
bool b2 = -b;	// b2是ture
```

所以说，布尔值一般不参与运算。

参与取余运算的对象必须是整数类型。

规定商一律向0取整。

一般规定，(-m)/n 和 m/(-n) 都等于 -(m/n) , m%(-n) 等于 m%n，(-m)%n 等于 -(m%n)。

#### 4.3 逻辑和关系运算符

```c++
for(const auto &s : text){
	cout << s;
	if(s.empty())
		cout << endl;
	else 
		cout << " ";
}
```

因为text的元素是string对象，可能非常大，所以将s声明为引用类型可以避免对元素的拷贝；又因为不需要对元素进行写操作，所以声明为常量的引用。

#### 4.4 赋值运算符

```c++
int i;
while(i != 42){
	i = get_value();
}
//更为简洁的写法
int i;
while((i = get_value()) != 42){
	//其他处理
}
```



#### 4.5 递增和递减运算符

```c++
while(beg != s.end() && !isspace(*beg))
	*beg = toupper(*beg++); //错误：赋值运算符左右都用到了beg，而且右端还改变了beg的值，所以该运算符是未定义的
```



#### 4.6 成员访问运算符

因为解引用的运算符的优先级低于点运算符，所以执行解引用运算符的子表达式两端必须加上括号。

`(*p).size()`



#### 4.7 条件运算符

`cond?expr1:expr2`

嵌套条件运算符

```c++
finalgrade = (grade > 90) ? "high pass" : (grade < 60) ? "fall" : "pass";
```



#### 4.8 位运算符

关于符号位如何处理没有明确规定，所以强烈建议仅将位运算符用于处理无符号类型。

位求反运算符（~），与（&），或（|），异或（^）运算符在两个运算对象上逐位执行相应的逻辑操作。

移位运算符的优先级介于中间，比算术运算符的优先级低，但比关系运算符、赋值运算符和条件运算符的优先级高。



#### 4.9 sizeof运算符

```c++
Sale_data data, *p;
sizeof data;	//data类型的大小，等同于sizeof(Sale_data)
sizeof p;		//指针所占空间的大小
sizeof *p;		//p所指类型空间的大小，等同于sizeof(Sale_data)
```

一些sizeof的说明：

- 对char或者类型为char的表达式执行sizeof运算，结果为1；
- 对引用类型执行sizeof运算得到被引用对象所占空间大小；
- 对指针执行sizeof操作得到指针本身所占空间大小；
- 对解引用执行sizeof操作得到指针指向的对象所占空间大小，指针不需要有效。
- 对数组执行sizeof得到整个数组所占空间的大小，等价于对数组中所有元素各执行一次sizeof运算并将结果求和，注意，sizeof不会把数组转化成指针来处理。
- 对string和vector对象执行sizeof运算只返回该类型固定部分的大小，不会计算对象中的元素占用了多少空间。

```c++
constexpr size_t sz = sizeof(ia)/sizeof(*ia); //计算数组的大小
```



#### 4.10 逗号运算符

#### 4.11 类型转化

算术转化的含义是把一种算术类型转化为另一种算术类型，其中运算符的运算对象将转化为最宽的类型。

**显示转化**

命名的强制类型转化：

`cast-name<type>(expression)`

cast-name如下：

- static_cast：任何具有明确定义的类型转化，只要不包含底层const，都可以使用static_cast。
- const_cast：const_cast只能改变运算对象的底层const。
- reinterpret_cast：不常使用。



### 第5章 语句

#### 5.1 简单语句

`;`空语句

#### 5.2 语句作用域

#### 5.3 条件语句

**switch**

case关键字和他对应的值一直被称为case标签，case标签必须是整形常量表达式。

```c++
char ch = getVal();
int ival = 42;
switch(ch){
	case 3.14;	//错误：不是一个整数
	case ival;	//错误：不是一个常量
}
```

有时候，我们省略掉break语句，使得程序能够连续的执行若干个case标签：

```c++
switch(ch)
{
	case 'a':
	case 'e':
	case 'i';
	case 'o';
	case 'u';
		++vowulCnt;
		break;	//统计元音字母的个数
}
```

 **default标签**

如果没有任何一个case标签能匹配上，switch表达式的值，程序将执行紧跟在default标签后面的语句。

一般不要再case语句中定义变量，虽然说，不同的case在同一个作用域中（case中没有使用{}）。



#### 5.4 迭代语句

**for语句**

注意，for语句中可以定义多个对象，但是只能有一个声明语句。

**范围for语句**

`for(declaration: expression)`

expression表示的必须是一个序列，比如用花括号括起来的初始值列表、数组、或者vector或string等类型对象，这些类型的共同特点是拥有返回迭代器的begin和end成员。如果需要对序列中的元素执行写操作，循环变量必须声明为引用类型。

**while**

因为对于do while 来说，先执行的是语句或者块，后判断条件，所以不允许在条件部分定义变量。



#### 5.5 跳转语句

**break**

**continue**：停止最近的循环中的当前迭代并立即开始下一次迭代，continue语句只能出现在for、while和do while循环的内部。



#### 5.6 try语句块和异常处理

**throw表达式：**异常检测部分使用throw表达式来表示他遇到的无法处理的问题，我们说throw引发了异常。

**try语句块：**异常处理部分用try语句块处理异常。try语句块以关键字try开始，并以一个或多个catch语句结束。

```c++
while(cin << intem1 << intem2)
{
	try{
		if(intem1 != intem2)
		 throw.runtime_error("Data must refer to same ISBN");
	}
	catch(runtime_error err)
	{
	cout << err.what()
	break;
	}
}
```

类型runtime_error是标准库中异常类型的一种，定义在stdexcept头文件中。

try语句块内部声明的变量在块的外部无法访问，特别是在catch子句内也不可以。

- exception头文件定义了最通用的异常类exception，它只报告异常的发生，不提供任何额外的信息。
- stdexcept头文件定义了几种常用的异常类。
- new头文件定义了bad_alloc异常类型。
- type_info头文件定义了bad_cast异常类型。

![image-20191222193027808](/assets/image-20191222193027808.jpg)

### 第6章 函数

#### 6.1 函数基础

在函数中，即使两个形参的类型一样，也必须把两个类型都写下来。

函数的返回类型不能是数组类型或者函数类型，但可以是指向数组或者函数的指针。

**局部静态对象**（定义为static类型）：在程序执行路径第一次经过对象定义语句时初始化，并且直到程序终止才被销毁。

定义函数的源文件应该把含有函数声明的头文件包含进来，编译器负责验证函数的定义和声明是否匹配。



#### 6.2 参数传递

当形参是引用类型时，我们说它对应的实参被引用传递。

当实参的值被拷贝给形参时，形参和实参是两个相互独立的对象，我们说这样的实参被值传递。

**指针形参：**

```c++
void reset(int *ip)
{
	ip = 0;	// 只改变了ip的局部变量
	*ip = 0;	// 改变指针ip所指对象的值
}

void main()
{
	int i = 42;
	reset(&i);
	cout << i << endl; //输出i = 0

}
```

**引用形参：**

```c++
void reset(int &ip)
{
	ip = 0;
}
int j = 42;
reset(j);
cout << j << endl; //输出为0
```

拷贝大的类类型对象或者容器对象比较低效，比如string对象可能会很长，所以应该尽量避免直接拷贝它们，使用引用形参是比较明智的方法。

如果函数无需改变引用形参的值，最好将其声明为常量引用。

如何让一个函数能返回两个值？一种方法是定义一个数据类型，另一个方法是给函数传入一个额外的引用实参，令其保存字符出现的次数。

和其他初始化一样，当用实参初始化形参时形参会忽略掉顶层const，形参顶层const被忽略掉了。

```c++
void fcn(const int i) // fcn能够读取i，但是不能向i写值
void fcn(int i)	// 错误，重复定义了fcn(int)
```

我们可以用一个非常量初始化一个底层const对象，但是反过来不行。

把函数不会改变的形参定义为（普通的）引用是一种比较常见的错误。

`string:: size_type find_char(string $s)`

则只能将find_char作用于string对象，下面调用会发生错误：

`find_char("Hello world")`

**数组形参**

因为不能拷贝数组，所以我们无法以值传递的方式使用数组参数。但是可以传递数组指针：

```c++
void print (const int*);
void print(const int[]);
void print(const int[10]);
```

以上三种print函数是等价的。

但是函数并不知道数组的确切尺寸，管理指针形参有三种常用方式：

**使用标记指定数组长度：**

C风格字符串最后一个字符后面跟着一个空字符：

```c++
void print(const char *cp)
{
	if(cp)
		while(*cp)
			cout << *cp++;
}
```

**使用标准库规范：**

传递指向数组首元素和尾元素的指针：

```c++
void print(const int *beg, const int *end)
{
	while(beg != end)
		cout << *beg++ <<endl;
}
```

**显示传递一个表示数组大小的形参：**

```c++
void print(const int a[], size_t size)
{
....
}
int j[] = {0,1};
print(j,end(j) - begin(j));
```

形参也可以是数组的引用，此时，引用形参绑定到对应的数组上，也就是绑定到数组上：

```c++
void print(int (&arr)[10])
{
	for(auto elem : arr)
		cout << elem << endl;
}
//注意这里的括号关系
f(int &arr[10]) //错误：将arr声明为了引用的数组
f(int (&arr)[10]) //正确：arr是具有10个整数的整数数组的引用
```

**传递多维数组：**

数组第二维（以及后面所有维度）的大小都是数组类型的一部分。

```c++
void print(int (*matrix)[10], int rowsize) //指向含有10个整数的数组的指针
{
...
}

int *matrix[10]; //10个指针构成的数组,错误
```

**initializer_list形参：**

是一种模板类型，和vector不一样，其中的元素永远是常量值。

```c++
void error(initializer_list<string> il)
{
	for(auto beg = il.begin(); beg != il.end(); ++beg)
		cout << *beg << " ";
	cout << endl;
}
if(expected != actual)
	error({"function",expected, actual});
else
	error({"function", "Okey"});
```

包含begin和end成员，一般用范围for循环。



#### 6.3 返回类型和return语句

通常情况下，void函数如果想在中间退出，可以用return语句。

注意，不要返回局部变量对象的引用或指针，函数终止意味着引用将指向不再有效的内存区域。

```c++
const string &manip()
{
	string ret;
	if(!ret.empty())
		return ret;	//呜呜，返回局部对象的引用
	else 
		return "Empty"; //错误，是一个局部临时量
}
```

特别的是，我们可以为返回类型是非常量引用的函数的结果赋值。

**列表初始化返回值：**

函数可以返回花括号包围的值的列表。

```c++
vector<string> process()
{	
	...
	return {"function", "Okey"};
}
```

**主函数main的返回值：**

```c++
int main()
{
	if (some_failture)
		return EXIT_FAILURE;
	else
		return EXIT_SUCCESS;
//定义在cstdlib头文件中
}
```

**返回数组指针：**

虽然函数不能返回数组，但是函数可以返回数组的指针或者引用。定义一个返回数组的指针或引用最直接的方法是使用类型别名。

```
typedef int arrT[10];
using arrT = int[10];
//两个等价声明，arrT是一个类型别名，表示含有10个整数的数组
arrT* func(int i);
//或者如下声明
int (*func(int i))[10];
```

**尾指返回类型：**

`auto func(int i) -> int(*)[10]`

**decltype：**

如果我们知道函数返回的指针将指向某个数组，就可以用decltype

```c++
int odd[] = {1,3,5};
int even[] = {0,2,4};
decltype(odd) *arrPtr(int i)
{
	return (i % 2) ? &odd :&even; 
}
//decltype并不能把数组类型转化成指针，所以arrPtr返回指针必须加一个*
```



#### 6.4 函数重载

如果同一作用域内的几个函数名字相同但是形参列表不同，我们称之为重载。

main函数不能重载。

不允许两个函数除了返回类型外其他所有的要素都相同。

一个拥有顶层const的形参无法和另一个没有顶层const的形参区分开：

```c++
lookup(int);
lookup(const int); //重复声明
lookup(int*);
lookup(int* const); //重复声明
//另一方面，如果形参是某种类型的指针或者引用，则通过区分其指向的是常量还是非常量可以显示函数重载，此时const是底层的。

lookup(int&);
lookup(const int&); //新函数
lookup(int*);
lookup(const int*); //新函数
```

在不同的作用域中无法重载函数名，在C++中，名字查找发生在类型检查之前。



#### 6.5 特殊用途语言特性

**默认实参：**

在函数的很多次调用中，某种形参都被赋予一个相同的值，此时，我们把这个反复出现的值称为函数的默认实参。

不过要注意，一旦某个形参被赋予了默认值，它后面的所有形参都必须有默认值。如果我们想使用默认实参，只要在调用函数的时候省略该实参就可以了。

通常，应该在函数声明中指定默认实参，并将该声明放在合适的头文件中。

```c++
string screen(sz = ht(), sz = wd, char = def);
void f2()
{
	def = '*';	//改变了默认实参值
	sz wd = 100;	//隐藏了外部定义的wd,但是没有改变默认值
	window = screen();
}
```



**内联函数**

将函数指定为内联函数，通常就是将它在每个调用点上“内联地”展开。

内联机制用于优化规模较小，流程直接，频繁调用的函数。

在函数返回值前加上关键字inline，这样就可以将它声明为内联函数。



**constexpr函数**

函数的返回类型及所有形参的类型都是字面值类型，而且函数中有且仅有一个return。

constexpr函数被隐形的指定为内联函数。

其内部也可以包含其他语句，只要这些语句在运行时不执行任何操作就行。

我们允许其返回值并非一个常量。



**assert预处理宏**

所谓预处理宏其实就是一个预处理变量，它的行为有点类似于内联函数，assert宏使用一个表达式作为它的条件。

`assert(expr)`

首先对express求值，如果表达式为假，输出信息并终止程序的执行，如果表达式为真，assert什么都不做。



**NDEBUG预处理变量**

如果定义了NDEBUG，则assert什么都不做。

我们可以使用一个#define语句定义NDEBUG，从而关闭调试模式。

或者：

`$ CC -D NDEBUG main.C`

相当于在main.c文件的一开始写#define NDEBUG

预处理定义了对于程序调试很有用的：

`_ _func_ _`：存放函数名。

`_ _FILE_ _`：存放文件名。

`_ _LINE_ _`：存放当前行号

`_ _TIME_ _`：存放编译时间

`_ _DATE_ _`：存放编译日期



#### 6.6 函数匹配

...



#### 6.7 函数指针

`bool lengthCompare(..)`

只需要：

`bool (*pf)(..)`//未初始化

即可。

注意:`bool *pf()`表示一个名为pf的函数，其返回值为bool*类型。

当我们把函数名作为一个值使用时：

```c++
pf = lengthCompare;
pf = &lengthCompare;//等价
//调用函数
bool b1 = pf(..);
bool b2 = (*pf)(..);
bool b3 = lengthCompare(..);//等价

```

当我们使用重载函数指针时，上下文必须清晰地界定到应该选那个函数。



**函数指针形参：**

`void useB(bool pf(..));`

等价于：

`void useB(bool (*pf)(..));`

但是这样的方法显得冗长

```c++
typedef decltype(lengthCompare) func2;
typedef decltype(lengthCompare) *Func2;//等价
void useB(func2);//自动将函数类型转化成指针
void useB(Func2);//等价
```



**返回指向函数指针**

```c++
using F = int(int*, int);	//F是函数类型，不是指针
using PF = int(*)(int*, int);	//PF是指针类型

PF f1(int);
F *f1(int);

//当然我们也可以用直接声明
int (*f1(int))(int*, int);
//或者使用尾置返回类型
auto f1(int) ->int (*)(int*, int);
```

