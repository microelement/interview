Const 是C++中常用的类型修饰符,常类型是指使用类型修饰符const说明的类型，常类型的变量或对象的值是不能被更新的。
#一、Const作用
1. 可以定义const常量。
const用的最多的就是用来定义常量了。
```
const int Max = 100;
```
2. 便于进行类型检查
const常量有数据类型，而宏常量没有数据类型。编译器可以对前者进行类型安全检查，而对后者只进行字符替换，没有类型安全检查，并且在字符替换时可能会产生意料不到的错误。
```
void f(const int i) { .........}
      //对传入的参数进行类型检查，不匹配进行提示
```
3. 可以保护被修饰的东西防止意外的修改，增强程序的健壮性。
-1.让指针指向一个常量对象，这样可以防止使用该指针来修改所指向的值。
const int* p=&i;该声明指出了p是一个指向const int的指针，因此不能使用p来修改这个值。换句话说，*p的值为const，是不能修改的。 
这样就可以禁止通过p来修改i的值，例如*p=21这样的操作时不允许的。p的声明并不意味着它所指向的值就是一个const常量，而只是意味着对于p而言，这个值是const,但是这并不能说明i是一个不能修改的值，例如i=15,这样的操作时允许的。并且，p指向的位置是可以改变的。
-2.将指针本身声明为常量，这样可以防止修改指针指向的位置。
int* const p=&i;//注意const的位置
p=&k，操作不允许，说明了p所指向的位置是const，不可修改的。
-3.以前我们将常规变量的指针赋给常规指针，而这里将常规变量的地址赋给指向const的指针。因此有两种可能。
一种是:将const变量的地址赋给指向const的指针，第二种是将const的地址赋给常规指针。显然第一种可以，而第二种是不可以的。
例如:const  int  i=10;
int *p=&i;//invalid
如果这是可行的，那么就能通过p修改i的值，那么const int i这一句声明里面的const状态就相当于无效了。
当然，如果一定要转换，通过const_cast还是可以突破这种限制的。但是也是有限制的。
但是可以通过*p来修改其所指向位置的值。
void f(const int i) { i=10;//error! }
      //如果在函数体内修改了i，编译器就会报错
4. 可以很方便地进行参数的调整和修改
同宏定义一样，可以做到不变则已，一变都变
5. 为函数重载提供了一个参考
class A
{
           ......
  void f(int i)       {......} //一个函数
  void f(int i) const {......} //上一个函数的重载
           ......
};
6. 可以节省空间，避免不必要的内存分配
const定义常量从汇编的角度来看，只是给出了对应的内存地址，而不是象#define一样给出的是立即数，所以，const定义的常量在程序运行过程中只有一份拷贝，而#define定义的常量在内存中有若干个拷贝。
\#define PI 3.14159         //常量宏
const doulbe  Pi=3.14159;  //此时并未将Pi放入ROM中
              ......
double i=Pi;   //此时为Pi分配内存，以后不再分配！
double I=PI;  //编译期间进行宏替换，分配内存
double j=Pi;  //没有内存分配
double J=PI;  //再进行宏替换，又一次分配内存！
7. 提高了效率
编译器通常不为普通const常量分配存储空间，而是将它们保存在符号表中，这使得它成为一个编译期间的常量，没有了存储与读内存的操作，使得它的效率也很高
#二、Const的使用
##1、定义常量
(1)const修饰变量，以下两种定义形式在本质上是一样的。它的含义是：const修饰的类型为TYPE的变量value是不可变的。
```
 TYPE const ValueName = value;
 const TYPE ValueName = value;
```
(2)将const改为外部连接,作用于扩大至全局,编译时会分配内存,并且可以不进行初始化,仅仅作为声明,编译器认为在程序其他地方进行了定义.

     extend const int ValueName = value;

##2、指针使用CONST
(1)指针本身是常量不可变
     (char*) const pContent;
     const (char*) pContent;

(2)指针所指向的内容是常量不可变
     const (char) *pContent;
     (char) const *pContent;

(3)两者都不可变
      const char* const pContent;

(4)还有其中区别方法，沿着*号划一条线：
如果const位于*的左侧，则const就是用来修饰指针所指向的变量，即指针指向为常量；
如果const位于*的右侧，const就是修饰指针本身，即指针本身是常量。

