一、sizeof
sizeof(...)是运算符，而不是一个函数。
    一个简单的例子：
int a;
cout<<sizeof a<<endl;
sizeof操作符的结果类型是size_t，它在头文件中typedef为unsigned int类型。
该类型保证能容纳实现所建立的最大对象的字节大小。由于在编译时计算，因此sizeof不能用来返回动态分配的内存空间的大小实际上，用sizeof来返回类型以及静态分配的对象、结构或数组所占的空间，返回值跟对象、结构、数组所存储的内容没有关系。
具体而言，当参数分别如下时，sizeof返回的值表示的含义如下：
    ##1.数组——编译时分配的数组空间大小；
    ##2.指针——存储该指针所用的空间大小（存储该指针的地址的长度，是长整型，应该为4）；
    ##3.类型——该类型所占的空间大小；
    ##4.对象——对象的实际占用空间大小；
    ##5.函数——函数的返回类型所占的空间大小。函数的返回类型不能是void。
**********
#二、strlen
    strlen(...)是函数，要在运行时才能计算。
    参数必须是字符型指针（char*）, 且必须是以'\0'结尾的。当数组名作为参数传入时，实际上数组就退化成指针了。
int ac[10];
    cout<<sizeof(ac)<<endl;
    cout<<strlen(ac)<<endl;     (ac相当于一个指针，但是strlen只能接受char*类型，所以编译时出错)
    它的功能是：返回字符串的长度。该字符串可能是自己定义的，也可能是内存中随机的，该函数实际完成的功能是从代表该字符串的第一个地址开始遍历，直到遇到结束符'\0'。返回的长度大小不包括'\0'。
***********
#三、举例：
    eg1.
	      char arr[10] = "Hello";
              int len_one = strlen(arr);
              int len_two = sizeof(arr); 
              cout << len_one << " and " << len_two << endl; 
    输出结果为：5 and 10
    点评：
             sizeof返回定义arr数组时，编译器为其分配的数组空间大小，不关心里面存了多少数据。
             strlen只关心存储的数据内容，不关心空间的大小和类型。


    eg2.
	      char * parr = new char[10];
              int len_one = strlen(parr);
              int len_two = sizeof(parr);
              int len_three = sizeof(*parr);
              cout << len_one << " and " << len_two << " and " << len_three << endl;
    输出结果：3 and 4 and 1


    点评：
             第一个输出结果3实际上每次运行可能不一样，这取决于parr里面存了什么（从parr[0]开始直到遇到第一个'\0'结束）；
             第二个结果实际上本意是想计算parr所指向的动态内存空间的大小，但是事与愿违，sizeof认为parr是个字符指针，因此返回的是该指针所占的空间（


指针的存储用的是长整型，所以为4第三个结果，由于*parr所代表的是parr所指的地址空间存放的字符，所以长度为1。
**********
下面是程序员面试宝典上面总结的：


1.sizeof操作符的结果类型是size_t，它在头文件中typedef为unsigned int类型。 
该类型保证能容纳实现所建立的最大对象的字节大小。 

2.sizeof是算符，strlen是函数。 

3.sizeof可以用类型做参数，strlen只能用char*做参数，且必须是以''\0''结尾的。 
sizeof还可以用函数做参数，比如： 
short f(); 
printf("%d\n", sizeof(f())); 
输出的结果是sizeof(short)，即2。 

4.数组做sizeof的参数不退化，传递给strlen就退化为指针了。 

5.大部分编译程序 在编译的时候就把sizeof计算过了 是类型或是变量的长度这就是sizeof(x)可以用来定义数组维数的原因 
char str[20]="0123456789"; 
int a=strlen(str); //a=10; 
int b=sizeof(str); //而b=20; 

6.strlen的结果要在运行的时候才能计算出来，时用来计算字符串的长度，不是类型占内存的大小。 


7.sizeof后如果是类型必须加括弧，如果是变量名可以不加括弧。这是因为sizeof是个操作符不是个函数。 

8.当适用了于一个结构类型时或变量， sizeof 返回实际的大小， 
当适用一静态地空间数组， sizeof 归还全部数组的尺寸。 
sizeof 操作符不能返回动态地被分派了的数组或外部的数组的尺寸 

9.数组作为参数传给函数时传的是指针而不是数组，传递的是数组的首地址， 
如： 
fun(char [8]) 
fun(char []) 
都等价于 fun(char *) 
在C++里参数传递数组永远都是传递指向数组首元素的指针，编译器不知道数组的大小 
如果想在函数内知道数组的大小， 需要这样做： 
进入函数后用memcpy拷贝出来，长度由另一个形参传进去 
fun(unsiged char *p1, int len) 
{ 
unsigned char* buf = new unsigned char[len+1] 
memcpy(buf, p1, len); 
} 

我们能常在用到 sizeof 和 strlen 的时候，通常是计算字符串数组的长度 
看了上面的详细解释，发现两者的使用还是有区别的，从这个例子可以看得很清楚： 

char str[20]="0123456789"; 
int a=strlen(str); //a=10; >>>> strlen 计算字符串的长度，以结束符 0x00 为字符串结束。 
int b=sizeof(str); //而b=20; >>>> sizeof 计算的则是分配的数组 str[20] 所占的内存空间的大小，不受里面存储的内容改变。 

上面是对静态数组处理的结果，如果是对指针，结果就不一样了 

char* ss = "0123456789"; 
sizeof(ss) 结果 4 ＝＝＝》ss是指向字符串常量的字符指针，sizeof 获得的是一个指针的之所占的空间,应该是 长整型的，所以是4 
sizeof(*ss) 结果 1 ＝＝＝》*ss是第一个字符 其实就是获得了字符串的第一位'0' 所占的内存空间，是char类 型的，占了 1 位 
strlen(ss)= 10 >>>> 如果要获得这个字符串的长度，则一定要使用 strlen。 
