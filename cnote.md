# C记录

1 字面量,会默认进行向上转换.字符型转换成int型,浮点型转换成double型,除非显示指定后缀明确类型

2 整数和浮点比较时,要注意精度舍弃问题,以及十进制和二进制互相转换后再表示的问题,0.1+0.2!=0.3就像这种十进制转二进制,小数表示是有误差的,所以最好相减对比绝对值的差值,而已因为是二进制,所以增加或者缩小2倍就可以左移或者右移一位

3 表达式的值就是整个表达式执行之后的最后一个值,遇到,号和赋值时,要注意赋值运算符比,号优先级高.并且要注意序列点

6 数组初始化可以={} 或者使用指定初始化{[2]=3,5},定义数组必须指明一个常量.  int [] 这种不指明大小的形式,只能用在函数定义或结构中用来表明步长的,因为会被转换成指针，

7 指针加减是以指针类型的大小为步长,指针表示多维数组的时候,要注意各级指针的步长,系统无法自动判断多级指针的步长,或者也可以转换成带步长的指针数组

0 对于多维数组来说，如果用多级指针操作的话，要注意多级指针的步长

8 对于数组取值时,使用指针表示法,编译器更容易生成优化的代码

0 对于常规const，在c里面只起到限定作用，而不是真的常量,而在c++中可以用在声明数组大小中

0 声明一个二维数组指针 int (*a)[10]，之所以声明成这样是因为优先级问题

0 c99支持可变数组，数组都是在栈上分配的，一次性太大会栈溢出报错，这种数组不能初始化 int myarray(int row,int col,int array[row][col]);

0 复合字面量 (int []){3,4},前者是省略名字之后的复合字面量的类型，编译器会自动计算大小。而且这种类型可以直接赋值给数组

0 数组用字符串常量初始化时，此数组存的是字符串的一个副本，随后可以任意修改

0 使用指针指向字符串常量，只能获得字符串常量的地址，却不能修改值

0 函数作用域指的是goto标签,函数不能使用,只有goto关键字能使用

0 函数外的变量及函数名都是文件作用域,直到文件结束

0 c符号链接性默认是外连接,局部的是无连接,除非使用static显示声明为文件内链接.这是对于全局变量或者函数而言的

0 对于局部变量,它的意思是此变量是静态存储的,永久性变量,使用static声明的局部变量,其实编译时会移到函数外面,成为一个块作用域的全局变量

0 register请求将此变量用寄存器操作,作为函数形参时,表示此此变量用寄存器传递.用它声明的变量不能取地址

0 外部变量只能使用常量初始化

0 int const * name和const int * name是一样的,都表明name的内容是常量

0 如果static const int name声明在一个头文件中,其他文件包含这个文件,就会获得该变量的一个副本,相当于每个文件都定义了一个这个变量,因为static是文件内可见的

0 volatile可以防止编译器优化,不要使用合并操作

0 restrictb只能用于指针,表示指针是访问该数据的唯一方式,用于优化合并指针操作

0 int fun(int num[static 20]) static新增的含义,用在函数定义中表示限定参数至少有20个元素.为优化提供帮助.因为数组默认是用指针来传递,所以只能用这种方式来告诉编译器,数组的大小,不能用在函数原型中.有些编译器不支持

0 结构初始化可以使用数组的初始化的方式,或者使用结构初始化器 如struct book name={.val=0,.str="aaa"};

0 结构默认是按值传递的,传递的是一个副本,早期结构是不能传递的,只能用指针,现在可以了

0 结构名是地址的别名,直接使用代表的是其内容,也就是第一个元素,要获得地址本身还是要&,而数组名就是地址本身,直接使用还是地址

0 首先要清楚,普通变量就是一个地址的标签,使用变量名就相当于显式标签地址的内容

0 允许把结构赋值给另一个结构,结构可以做为参数传递或者返回,这时候都会被翻译成一个副本

0 复合字面量为创建匿名结构体提供了一种方式,(struct book){val=1,val2=2}
```
struct book{
    int num;
    int [];//伸缩型数组成员,使用时自己去动态申请内存

};
```
0 最新标准可以直接在结构体内嵌套定义结构体

0 并且嵌套定义的结构体可以在外部直接使用其定义新类型，但是c++必须使用作用域运算符，而且c++中使用结构体前面可以不加struct

0 c枚举变量可以使用自增运算符,但是c++不行

0 define 是文本级的替换,可以用于任何形式,但其作用域预编译时,用来定义类型别名的时候,不会进行类型检查,而且在定义多变量的时候也不如typedef来的好,是编译级支持

```
#define INT int
INT a,b;
= int a;
      b;//b没有类型

```
0 typedef只能用于类型

0 位运算,不会修改原值,而是会创建一个新值

0 要想重定义宏,必须和原宏一致.否则只能使用#undef取消后再重定义

0 最好不要用对宏的参数使用自增或者自减运算符,并且定义的时候要保证足够多的括号

```
    #define square(x) ((x)*(x));最好定义成这种
    100/square(10+5)
=>  100/((10+5)*(10+5));像这样才能避免优先级问题
    
    

```

0 #运算符和##运算符,前者表示将标记替换成字符串形式,后者表示链接两个标记变成一个新标记
```
  #define str(n) printf(#n ":%d",x##n);
  str(3)
=> printf("3:%d",x3);

```
0 变参宏...和__VA_ARGS__
```
#define print(num,...) printf(num,__VA_ARGS__);
print("%d,%f",23,.9)
=>printf("%d,%f",23,9);

```

defined运算符,测试宏是否定义过,并返回0,1.较新的编译器支持
```
#if defined (MAK)


```

define,ifdef,ifndef,else,undef,if ,elif ,else endif,defined

#line 100  or  line 21 "main.c"  重置__LINE__ 和 __FILE__ 返回的行号和文件名
#error 发出一个错误
#pragma c9x on 发送给编译器用的指令
#_Pragma("c9x on") 相当于上面的

# 预定义宏
```
    __DATE__ 预编译日期
    __TIME__ 编译代码的时间
    __FILE__ 当前源代码文件名
    __LINE__ 当前在源代码文件中的行号
    __STDC__ 为1时,表明实现遵循C标准
    __STDC_HOSTED__ 本机环境设置为1否则为0
    __STDC_VERSION__ 支持的标准
    __func__ 当前函数名   这是个预定义标识符

```

0 C11 泛型选择  关键字 _Generic   根据不同的类型选择不同的函数
```
#define typeinfo(x) _Generic(x, \
    int : #x":int",\
    default : #x" default"\
)

int temp = 4
typeinfo(temp);

=> "temp int"


```


0 inline 内联函数, 无法获取地址,如果强制获取地址,会无法生成内联函数.内联函数定义在头文件中,因为它具有内部连接性,一般和static配合使用，并且可以多重链接性定义,c++不允许

0 _Noreturn 用来函数说明关键字,表示此函数调用后不再返回主调函数.用于函数,生成优化代码

0 void类型的指针在c中可以不强制转换就能赋给具体类型,而c++则必须要强制转换

0 assert 动态断言,执行期

0 _Static_assert(BIX_MAX==256,"error");静态断言,编译期

0 可变参数stdarg
```c
int myfun(int am,...){
    va_list ap,bp;
    char *a,*b;
    va_start(ap,am);
    va_copy(bp,ap);
    a = va_arg(ap,int);
    b = va_arg(ap,int);
    va_end(ap);
]
```



# c++记录

  * 函数没有参数默认void,可以没有return,默认返回0;
  
  * 使用c++本身的代码文件注意命名空间
  
  * int a(5) 这种内置类型的单对象初始化 或 int a={0} int a{} 这种也可以用于单指,但主要用在列表初始化 都是c++的新初始化方式,没有数值默认为0
  
  * char 默认可能有符号也可能没有符号,要想确定除非指定符号
  
  * wchar_t name = L'b';可变字符
  
  * char16_t/char32_t  name = u/U't';固定长度
  
  * const 变量只能定义是初始化,后期无法通过地址修改
  
  * 注意变量计算时的变量提升,当两个数据不同时,会编译成先用更大的类型计算,然后再将结果转换成目标的变量类型
  
  * 可以这样定义一个数组char ch[]{'a','b'};
  
  * 原始字符串 R"+* str *+"   这里面的字符将按照原来的字符显示，R"( str )" 这中无法显示括号
  
  * 可以创建没有类型名的结构体struct {string name;int age;} info; 这种只能定义一次变量
  
  * 对空指针使用delete 或者 free是安全的
  
  * 对于使用int *name = new int[10] 这种格式分配的内存,应当使用delete [] name来释放,而如果是为一个类实体分配内存,则使用delete不带方括号
  
  * 对于多维数组,数组名代表数组首元素的地址,如果使用运算,那么每次的变化是首元素的字节数,而&数组,代表的是整个数组,使用运算,每次增加整个数组的大小
  
  * (*name).value 对于指针对象可以方式 name->value
  
  * for(int x : array) or for(double d : {3,4,5}) 基于范围的循环
  
  * 逻辑||和&&是个序列点,这表示左边如果有自增,则会使用自增后的运算符
  
  * switch 的case 无法处理浮点数据,并且需要显式break
  
  * 虽然函数不能直接返回数组,但是可以作为结构或者对象的成员来返回
  
  * c++ 函数不指定参数需要显式使用...,否则默认void 而c中这是默认的.并且如果实参与形参不匹配c++会自动转换成相同的类型,而c中只会截断

  * 内联函数不能递归,从汇编的角度思考就明白了
  
  * int &a = b; 左值引用是标签级别的赋值,标签指向同一内存地址,在定义时初始化完成，对其赋值就是操作标签指向的数据，如果引用形参和实参不一样，那么会生成一个临时匿名变量,不能引用右值,作为函数参数也不行
  
  * int &&a = 3 * b + 4;右值引用；引用常量,会新建一个变量然后引用.
  
  * 默认参数 出现在还是原型中，并且只能从右到左依次定义默认参数，不能跳过。
  
  * 函数重载不关心返回值,只有关心参数特征。也要注意没有完美匹配的时候自动转换问题，如果有多个转换后能匹配的函数就会报错，并且const和非const参数是算作重载的，注意引用和非引用被是不允许的，它们被视为同一特征标
  
  * 函数模板,typename和class 是一样的效果，老式实现是class，模板定义可以放在头文件中,可以重载
  
```c++
  template <typename T,class T2>
      auto swap(T &a , T2 &b, int num) -> decltype(a+b) {
        a ^= b;
        b = a ^ b;
        a ^= b;
        T c = a;
        return num;
  }
  p
  int a =10;
  double b=99;
  swap(a,b,2);

```


  * 函数优先级 非模板函数 > 显式具体化函数 > 显式实例化 > 模板函数(隐式实例化化)，这些之间可以互相重载。实例化是声明就够了，而具体化还需要有单独的定义。当同一种类型的显式具体和显式实例同时存在将会出错
  
```c++
    int swap(int &a,int &b);
    
    template<class T>
    void swap(T &a , T &b);
    
    template<>
    swap<int>(char (&a)[][3] , char (&b)[][3]);
    or
    template<> swap(char (&a)[][3] , char (&b)[][3]);  //swap的类型参数是可选的，因为可以根据后面的参数类型自动推断


    template swap<double>(double &a , double &b);
    template swap(char &a , char &b);
    
    swap<>(1,3);可以主动选择使用模板函数
    swap<int>(1.5,9.7)

```

  * 重载时优先级选择问题
    
    1 函数中const参数和非const参数同时存在时，不会产生二义性的只有参数是指针或者引用的情况，因为如果是普通变量，那么声明为const没有意义，毕竟都是局部变量
    2 当没有与实参完全匹配的函数时，会进行变量转换，提升转换优先于标准转换，需要进行转换的越少的函数越趋向于具体
    3 要注意完全匹配和最佳匹配的概念，完全匹配是指参数能够完美的配合，而最佳是指在完全匹配的基础上又能够指出更具体的内容，最佳优先于完全
    
  * 如果是多个参数的模板，想要返回类型可以使用后置返回类型，以及decltype关键字
  
  ```cpp
      template<class T1 , typename T2>
      auto swap(T1 a, T2 b)->decltype(a+b)
      {
        decltype(b+a) c = b +a;
        return c;
      }
  
  ```
  
  * decltype的类型决策是根据正常类型，函数返回值，以及右值（就是用括号括起来 如 decltype((T1+T2)) name  那么name就是一个引用类型）这个顺序来的
  
  * #define定义的符号是没有作用域的,只要在使用之前定义就行了
  * register关键字在c++中变得无意义,只是起到提示该变量是自动变量的作用
  * 有些编译器不允许初始化局部自动数组和结构,因为这需要生成额外的指令
  * 全局const的初始化可以使用非常量:const int a = func() * 5;是允许的
  * ::name;当在函数内以及函数外都有一个name变量时,这种方法表示使用外部的版本
  * 对于全局变量,在本文件内static声明的将覆盖外部的同名变量
  * mutable 可以改变const不能修改的限制
  ```c++
      struct my{
        int a;
        mutable char *b;
      };
      
      const my a={10,"ssa"};
      a.b= "sdf";    //使用mutable声明的变量允许修改
  ```
  
  * const变量默认内连接,可以使用extern指定为外链接.但是这样就只能有一个定义,不能放在头文件了
  * 对函数使用static必须对原型和定义同时使用
  * 语言连接性 extern "c" void fun();说明这是一个c函数,使用c的命名约定查找;   extern "c++" void fun()这是c++的默认省略
  * 定位new:  int bootload = new(0x7f78a) int[40] => new(sizeof(int)*40,0x7f78a)在指定位置分配内存,需要包含new头文件
  * using声明是单个指定:using std::cout,如果和局部或者全局出现同名就会报错.而using namespace std;是编译指令,可以一次性包含所有这个名称空间内的变量,并且如果和全局或者局部同名的话只会隐藏,不会报错.并且using编译指令是可以传递的既:a包含b b包含c  => a包含c.
  * namespace one{ namespace two{   namespace{     }      }     }  名称空间可以嵌套,并且可以声明匿名的名称空间,匿名的空间只能在定义它的文件中使用
  * 定义函数时,声明和定义都需要在同一个命名空间内


  * 类成员默认私有，而结构默认公有，其他行为于类相同
  * 定义函数时使用作用域解析运算符来标识所属的类,并且定义类的大括号之后要主要有一个分号表明结束
  * 类声明中的函数自动成为内联函数，当然也可以在类外使用inline来定义，但是内联函数要求每个使用它的文件都必须包含其定义，仅仅包含原型是不行的，所以还是在头文件中声明为好
  
  * 构造函数,如果没有提供构造，默认提供一个，否则就取消提供的默认构造。提供构造时候最好提供一个没有任何参数的，或者全带默认参数的的构造。构造可以有多个
  * 析构函数，一般用来释放new分配的内存，否则其实使用默认实现的空构造即可，构造不能有参数不能有返回值,析构一般不能显式调用。一般作用域过期之后，或者调用delete来是否new分配的对象时自动调用，析构只能有一个。可以显式调用析构函数
```c++

  //正常初始化
  MyClass mc; //调用默认构造，调用默认构造时，不需要显式指定括号
  MyClass mc("sd"); //调用指定构造；
  MyClass mc = MyClass("sd");
  MyClass mc();表示函数声明
  
  //使用列表初始化
  MyClass mc{};
  MyClass mc{"ad"};
  MyClass *mc = new MyClass{"c"};
  
  
  ~MyClass(){}；
  
  //加入MyClass有一个void show();的函数原型则：
  const MyClass mc= MyClass{"ad"};
  mc.show();//是不允许的，因为对象是const 而编译器不知道show会不会修改对象。
  //把原型和定义改为 void show() const就可以了。这种后置const用在类成员函数里，表示不会修改对象
  
  
  //在一些编译器里，会出现创建临时对象的问题。所以最好使用第一种定义方式
  MyClass mc("ad"); //这种不会创建临时对象
  MyClass mc = Myclass("ad"); //这种可能会创建一个临时对象，然后把临时对象复制给mc接着再删除自己调用析构
  mc = MyClass("ad");  //这种肯定会创建一个临时对象，然后复制给mc
  
  //只有一个参数的构造可以这样调用；允许使用赋值语法构造一个对象
  MyClass mc = "ad";
  
  
  //注意一点，因为自动变量是在栈中分配的，所以析构的调用顺序是先进后出

```

  * this指针，每个类的成员函数包括构造和析构，都有一个隐藏的this指针，指向调用此函数的对象
  
```c++
    class my{
        private:
            int am;
        public:
          const  my & comp(const my & other) const {
              if(my.am>this.am)  //注意虽然am是my类的私有成员，不能通过对象调用，但是因为是在类内部，所以可以无视这种规则
              return *this;
          }
    }


```
  
  * 类数组，对于类数组的初始化，
```c++
    myclass mc[20];//分配空间时,调用默认构造
    myclass mc[3]={myclass("sd"),myclass("sd") };  //被省略的元素会调用默认的构造函数，定义并初始化数组的方案是先调用默认的构造函数定义出数组，然后大括号使用构造创建临时变量，并将值依次复制给对应元素 \
                                                   //通过对比数组字符串的赋值就明白了。 因为自动变量是在栈里面分配的，先需要确定空间，然后再运行代码进行构造对象。结果就导致了这样
  
```
  

  * 类内常量
```c++
  class my{
      const int pint = 12;//这是不对的，因为类声明并不是创建对象实体，所以无法存储这个常量的值，除非前面加上static使其成为静态变量,不能在类声明中为变量赋值,但整型和枚举型静态const变量可以，否则可能会引发多重定义
      enum {pint = 12}; //或者可以使用匿名枚举，作用域为整个类
      char *str[pint];
      
      enum a {S ， M , L};  
      enum b {S , M , L}'
      enum c = S;  //这两个普通枚举会造成枚举常量冲突
      
      //可以这样解决
      enum class a{S，M,L};  //可以使用struct代替class. 这种称为作用域枚举，底层类型默认为int型
      enum class b{S,M,L};
      enum c = a::S; //使用时只能显式指定所属的作用域,才能使用其枚举常量
      
      enum class  a : int {S,M,L};  //作用域枚举可以改变底层类型
  }


```
  
  
  * 变量名其实就是一个地址的标签,表示从这个地址开始操作数据,存取等.如果name变量名是一个指针,那么name表示从name标签代表的位置开始取数据,如果*name就代表将name代表的地址读出来,然后以这个读出的地址值为起始位置开始操作数据.也就是将name里面存放的数据当作地址进行寻址操作
  * 不要返回局部变量的引用,和指针变量的引用,因为函数结束时,这些变量将不存在,所以最好返回临时变量本身,这将返回临时变量的复制副本
  * 可以重载不是任何类的操作符,但必须有一个参数是自定义类型
  * 运算符重载
```c++
//不可重载运算符 除了new 和delete以外带字符的
  .  成员运算符        a.c
  .* 成员指针运算符     a.*c
  :: 作用域运算符      a::b
  ?: 三元运算符

//只能以成员函数重载
  [] 下标运算符    a[1]
  -> 指针成员运算符 a->c
  () 函数调用     a();
  =  赋值运算符   a = 1;


```

  * 友元类,友元函数,友元成员函数.
    成员函数:让函数成为他类的友元可以使这个函数获得和他类相同的访问权限,可以访问私有成员,其实就是非成员函数,一般来说定义成成员函数可以少一个参数,因为其他一个靠this指针传递
```c++
  class my{
      int age;
      public:
      friend int operator*(int a,my &b); //声明友元时必须在类声明中带上friend,并且函数的定义部分不需要,因为这个函数不属于类,也不需要类的作用域解析运算符,但是可以定义为内联.友元函数在使用类中的某些数据时,如果不是通过成员运算符,那么必须带上作用域运算符,因为它不属于本类
      int operator*(my &a,int b);
  }
  
    int my::operator*(my &a,int b){...}
  
    通过友元的形式，定义时不需要friend
    int operator*(int a,my &b){
        return b.age * a; //访问类的私有数据
    }
  
    or
    
   也可以通过转发的形式,这不需要此函数成为类的友元
   int operator*(int a,my &b){
       return b*a;   //调用my的成员函数
   }
  
  
  my a;
  int b = 3 * a; 
  

```
  * 构造函数用作转换,只有一个未确认值的构造函数可以用作转换函数,将其他类型转换成本类类型
```c++
  class my{
      my(int a,char b = 's');
      explicit my(int a); //禁用隐式转换
  };
  
  my a = 10;  //隐式转换,是可以这样赋值的,这将调用my的构造函数创建一个临时值,然后再将临时值复制给a
  my b = my(34);  or my b = (my)34; //如果构造使用了explicit,会禁止隐式转换,但可以强制转换
  
 //当有可以应用隐式转换的构造函数,并且将其他值赋给一个函数参数,并且这个参数是一个某个类的对象时,或者作为返回某个类的对象,但是却提供的是其他值时,那么将使用其他值初始化一个此类临时对象,然后使用
  
```

  * 转换函数 本来转换成其他类型,不能有参数,不能指定返回值,必须是类成员函数
```c++
  class my{
      private:
        double a;
      public:
        operator int() const {return (int)a;};
        explicit  operator double() const {return (double)a+20;}; //c++11支持
    
  
  };

  my a;
  int b = a; or int b =(int) a; or  int b = int(a);//使用转换函数,将本类值转换成其他函数
  double c = (double) a; or double (a); 使用explicit声明的只能显式调用

```
  * 转换有两种形式,通过构造函数将其他类型转换成本类对象,通过强制转换函数将本类转换为其他类型,当通过操作符操作类型对象时,可以重载运算符,不进行转换,直接操作原类型.
  
  * 除了标准的构造和析构函数还默认提供
```c++
  复制构造函数      //将一个类对象，赋值给令一个本类对象的时候，用于将对象赋值给另一个本类对象时，或者用现有对象初始化时，或者用在按值传递的函数参数中，或者返回值中
  myclass::myclass(const myclass &);//默认提供的形式 默认提供的复制构造会逐个复制类内的成员，浅复制
  
  赋值运算符重载
  myclass & myclass::operator=(const myclass);  会默认提供一个这类形式的复制运算符
  何时使用复制构造何时使用赋值运算符呢？ 随编译器而定，但是可以肯定的是，初始化一个对象的时候肯定会调用构造，或许会附带使用赋值运算符
  myclass mt,mb;
  myclass my = mt; //可能调用复制构造，或者先调用复制构造创建一个临时变量，然后再使用赋值运算符重载函数
  my = mb;   //这种几乎都是调用赋值运算符重载函数
  
  取地址运算符重载   //返回this指针

  c++11还提供另外两个，用于右值的
  移动构造函数
  移动赋值运算符

```
  
  * 静态-类成员函数 可以直接在类内定义，而不会引起多重定义，如果声明和定义分开，则声明需要static 而定义不需要。属于类，不需要类对象就可以调用，并且可以操作类的静态成员
      
  * 成员初始化列表语法，只能用在构造函数,并且不能放在头文件里,只能放在定义的地方，用来初始化一些，在分配空间时就需要初始化的那些成员。比如类的 非静态 const 变量，或者引用变量，这些都是必须在构造函数代码运行之前初始化.如果使用非列表初始化类内的其他类对象,则首先调用其他的类的默认构造,然后再调用赋值运算符.
```c++
  myclass::myclass(int a) : memb(12.4) , memb2(a) {...};初始化的顺序与这个列表顺序无关，与变量在类声明中顺序相同
  
  //c++11中可以直接在类声明中进行初始化；
  
```
  * myclass::myclass() delete  //c++11新增的，可以将某个函数标记为删除，用来去除不会使用的函数，以防止编译器自动生成，c++98是将这些函数声明为私有函数


# 继承

  * 公有继承
```c++
  class my{
      private:
      ...
      public:
      ...
      
      protected:    //受保护的    在普通类中,与privete相同,不能使用类的对象调用受保护的成员,但是在继承中,派生类可以直接访问基类中受保护的成员
      virtual ~my(){}  //将基类函数声明为虚函数,并且只需要在声明中带上即可,定义中不需要,派生类也可以也可以带上virtual关键字,也可以不带.
      //如果不带这个关键字,那么派生类如果有和基类的相同函数时,那么类的函数表将会同时存在这两个函数,那么如果将派生类转换成基类之后,调用函数时,将会调用基类的函数.这回造成一些问题,因为我原本是派生类的对象,你却调用了基类的函数.
      //如果加上这个关键字,那么函数表中只会存在一个同名函数,并且是最后一个派生类中定义的.这样无论用什么方式将派生类转换成基类,当调用函数时,都会调用派生来的,并且如果基类的函数是虚函数,那么所有的子代类中此函数全部都是虚的
      //猜测:如果类中没有虚函数,那么函数调用就不会去查表,也不存在函数表,也可能存在一个空表,所有的函数调用都是在编译阶段确定的也就是静态联编,但是类如果包含virtual,那么编译器在发现这个函数声明为了虚函数就会去查找虚函数表进行调用
      //如果派生类和基类不存在同名时,可以不使用作用域解析运算符即可调用,否则就需要使用作用域解析运算符来表示像调用基类的,因为默认不用::运算符,那么默认调用派生类的
  };
  
  class ty : public my{
      // 公有派生,基类的所有公有和保护成员,对子类可见,私有成员不可见,派生类可以像使用自身成员一样使用基类的成员,不需要添加修饰符.
      //基类的指针和引用以及对象可以使用派生类的对象,不需要任何转换
  };
  ty::ty(int a) : my(a);
  //创建子类对象时,需要先调用基类的构造函数构造对象,如果不指定,编译器将使用基类默认的构造函数
  //析构的函数调用顺序与调用构造函数的顺序相反
  //只有类成员函数才能是虚函数,并且构造函数不能是虚的,友元也不能是虚的,因为它不属于类
  //虚函数是覆盖而不是重载,但是可以在基类中实现,多个重载的虚函数,然后子类也进行这些虚函数的实现. 派生类不能改变基类中定义的参数的顺序及类型.  不过如果基类的虚函数返回基类的引用或者指针,那么派生类可以修改返回类型为派生类的引用或指针,这是最新的标准
  //如果虚函数在基类中被重载,那么派生类也有实现所有的这些重载的虚函数


  //在继承中,派生类的构造函数使用成员名初始化成员对象,而对于继承的基类使用基类名初始化 如:
  myclass:myclass(int c,myclass & my) : a(c) ,baseclass(my);
  
  
 class ty : private my, std::string   //默认为私有继承
 {
     //私有继承,基类非私有成员都将变成派生类的私有成员,只能在派生类中使用这些基类成员,派生类的对象无法直接调用这些通过私有继承来的基类成员.私有继承中无法应用里氏替换原则,只能强制转换为基类
     //多重继承,如果需要获得基类对象可以对this指针强制转换 如: return (my &) *this 可以获得一个基类对象,用于访问
 
 }
  
class ty : protected my    //保护继承
  {
  
    //基类的非私有成员成为派生类的保护成员,可以在第三代类中继续访问
    //可以隐式向上转换,但是只能在派生类内.派生类外部还是需要强制转换
  
  }
  

class ty : private my
{

    public:
        using my::show;   //重新定义私有继承中基类的成员的访问权限
        my::show;         //以前的方式  
    //上述两种方式,可以重新定义通过私有继承或者保护继承得来的成员的访问权限,使其可以通过派生类对象调用
    //注意这两种方式都没有返回值及参数列表,只需要一个名字即可
}
  
  
  
```

  * 抽象基类
```c++
  class my{
  
      virtual void fun() =0;  //纯虚函数,可以提供定义,也可以不提供定义.有任何一个纯虚函数的类,都是抽象类,无法创建对象.派生类必须实现定义覆盖基类的同名纯虚函数
      
      
  };

```
  
  
  * 多重继承
```c++
  //多重继承所引发的问题,当派生类通过多重继承获得多个相同的基类时,应用里氏替换原则时必须显式进行强制转换到相应的类型上去
      one-tow ot;
  base = (one *) &ot;
  base = (two *) &ot

  //为了避免多重继承包含多个相同的基类问题,可以将中间的继承层次使用使用虚继承,也就是声明为虚基类,这样编译器会将对象从中间基类中剥离出来形成一个单独的,并且无论从中间基类继承了多少个最终都只会存在一个,就好像派生类直接继承了这个基类,而不是通过间接继承
  //在派生类中,需要显式指明要调用的基类构造函数,否则将调用默认的虚基类构造函数
  //猜测: 被虚继承的类,如果孙子继承了多个中间类.那么这个虚基类会被单独提取出来,就好像不存在中间类而是直接在孙子显式类继承的一样.
  class one : virtual public base     //public和virtual先后顺序不做限制
  class two : public virtual base
  class one-two : public one, public two       //因为base类是被虚继承的,那么在孙子类中会被单独提取出来,只会存在一个.
  => class one-two : public base ,public one ,public two    //就相当于这样,所以初始化时,是通过孙子类来调用base类的构造函数来初始化base类的部分
  //使用多重继承,可以引发二义性问题或者更严重的多次重调base类函数的问题,有时候可能通过转发就可以解决,但有时候可能需要重构整个类继承的生态
  //混合使用虚继承和普通继承的时候,将分别或者虚继承的单个base对象部分,以及多个普通继承而来的base对象部分,也就是包含多个base对象
  //类的直接继承层次中,导致的函数重名,派生类的函数会优先于同名的基类函数被使用.
  //虚基类继承层次中出现的二义性,与访问规则无关,也就是说,如果出现同名的成员,那么即使是私有成员也会造成二义性,或者优先应用从哪个基类得来的成员的问题

```


  * 模板类,模板类不是定义具体函数,而是靠编译器生成,所以应该将模板类的定义和声明放在一起组成头文件.  并且如果直接在模板类声明中直接定义内联函数,则该内联函数不需要在前面加上template<class T>这一段.因为函数在声明内,如果在声明外,那么就必须加上,不然编译器无法知道这个函数是否是为这个类定义的
```c++
  template <class T,class T2 = int> //可以有多个类型参数
  class my{
      public:
        int stack(T & name);
  };
  
  template <class T,class T2>
  T & stack<T>::static(T &name){
  
      T *a = new T;
      return *a;
  }

  my<int> m;   //和模板函数不同,必须显式的指定类型,并且前面不需要template
  //模板可以有多个类型参数
  //模板可以继承,可以为类型提供默认值,与模板函数不同,模板函数不能为类型参数提供默认值,但是能为非类型参数提供默认值  
  //隐式实例化,只有在定义并初始化的时候才会进行隐式实例化.
      my<int,double> m;   //不会实例化.
      m = my<int,double>;  //会进行实例化
  //显式实例化 template class my<int,double>;即使不创建对象,也会进行模板的实例化
  //显式具体化   重新定义一个专门匹配某种类型的类模板
  template<> class my<char *> {...};
  or
  class my<char *>{...};
  //上面两种都可以实现类的显式具体化,但是下面这种是老式的实现
  
  
  
  //部分具体化,值具体化某部分template<>  尖括号中的是未被初始化的类型,如果尖括号为空,那么就算显式具体化了
  template<class T1,class T2,class T3>   //模板原型
  class my{...};
  
  template<class T1>                    //部分实例化
  class my<T1,int,T1>{...};
  //通过上面的写法可以知道,部分具体化可以使用类型参数中的类型来进行部分实例化




//成员模板 在模板类中定义模板,下面演示一个在模板类中声明一个模板类,然后在类外定义实现.有些编译器可能不支持,那么就必须在类内直接实现
template<class T>
    class my{
        private:
        template<class V>
            class ty;                   //先在模板类中声明有一个模板类
        ty<T> a;
        ty<char *> b;
        
        ...
    };

template<class T>
    template<class V>
        class my<T>::ty{               //在外面定义模板类中的模板类
            private:
                V a;
            public:
                void show() const {...}
        
        };


//将模板用作参数
template<template<class T> class V>     //说明这个模板类的类型参数是 template<class T> class 的类型,也就是V是一个模板类型
    class my{...};
如:
    my<other> obj;
  则other是这样的:
template<class T> 
    class other{...};



```
    
  * 模板类和友元
```c++
 //非模板友元 , 
 template<class T>
  class my{
  
      public:
      friend void show(my<T> obj);  这个函数将是此模板所有具体化类型的友元,但是这个函数本身不是模板函数,定义的时候必须重载,将所有可能的模板类类型都重载一遍
  
  };
  
  void show(my<int> obj){...}
  void show(my<double> obj){...}
  ...
 
 //约束模板友元,即友元的类型取决与类被实例化时的类型
 template<class T> void func1();  声明一个模板函数,不带参数
 template<class T> void func2(T &);声明一个模板函数,带一个参数
 
 template<class T> 
  class my{
  
      public:
        friend void func1<T>();
        friend void func2<>(my<T> &);  //可以根据参数自动推断类型
  
  };
 
 
 
 //非约束模板友元,即友元的所有具体化都是类的每一个具体化友元.也就是说模板函数的所有具体化形式都是某个具体化模板类的的友元.形成了多对多的关系
 template<class T>
  class my{
      
      public:
        template<class U,class V> friend void show(U a,V b);    //这个模板的所有具体化形式都是这个类的友元,也就是不受约束,可以不和模板类的类型参数完全匹配
  
  };
 
 
 
 

```

  * 模板别名
  
```c++
    template<class T>
        using mytype = my<T,12>;         //为my这个模板提供了一个别名

    
    mytype<int> a;    // => my<int,12> a; //可以简化一些参数

```
  
  
  
  * 友元类
  
```c++
    class my{
        private:
            friend class you;   //可以这样声明一个友元类.这个友元类的声明,放在公有部分或者私有部分都可以
                                //声明友元类,可以访问另一个类的所有成员,包括私有成员
                                //但是声明友元时,要注意定义顺序的问题
    }





```
  
  * 友元成员函数
```c++
  class my;       //将一个类声明为另一个类的友元成员函数时,要注意类的声明顺序,以及函数的定义顺序,经my提前,并且将内联的函数延后定义
  
  class you{
      public:
        void show(my & obj);
  }
  
  class my{
      private:
      friend void you::show(my & obj); 
  
  }
  
  inline void you:show(my & obj){...}


```

  * 互为友元类
```c++
  class my{
      private:
        friend class you;
      public:
        void show(you &obj);  //如果这个方法使用you类的成员,那么此时还不能定义函数体,只能在声明了you类之后
  };

  class you{
      public:
        friend class my;
        void change(my &obj){...}  //这个函数能定义函数体,是因为此时已经解析了my类都有哪些成员
  
  };

inline void my::show(you &obj){....}   //现在可以定义my类中方法了



```
  
  
  * 共同友元,一个函数可以同时成为两个类的友元
```c++
  class my;    //前置声明
  class you{
      private:
        friend void show(my &o1,you &o2);
        friend void show(you &o2,my &o1);
  };

  class my{
      public:
        friend void show(my &o1,you &o2);
        friend void show(you &o1,my &o2);
  };

  friend void show(my &o1,you &o2){...}
  friend void show(you &o1,my &o2){...}
  


```
  
  * 嵌套类,和结构一样可以类可以嵌套声明,但是老式的编译器不支持
```c++
  class my{
  
      public: //嵌套类声明在公有部分,可以在类外使用
        class you{
            private:
                int age;
                stringname
            public:
                void show();
        
        };
        
        void show();
      private:
        you obj;
        
  };
  
  void my::show(){
      you obj2;
      obj.show();
  }
  void my::you::show(){...}   //嵌套类的函数定义
  
  //嵌套类的声明可以被继承,并且嵌套类的成员访问控制和普通的类一样,如果是私有成员,则外部不可见
  //模板也可以使用嵌套类,如:
  tamplate<class T>
  class my{
      class you{
        public:
            T a;
      };
  };
  
  
  
```
  
  
  * 异常
```c++
  void show() throw(string obj);  //c++11抛弃的这种声明引发异常的方式
  void show()  noexcept;这是c++11的一个关键字 表示不会引发异常
  noexcept();  这是一个运算符,判断操作数是否会引发异常
  
  //猜测:异常其实就是在catch开始的位置将处理函数的地址以及一个空值压入栈中,然后使用一个寄存器保存这个catch开始的地址,因为catch就像是switch的模式一样链式执行的,然后在throw的位置将返回值压入之前的栈位置,并到寄存器中将之前的地址取出来然后压入到栈中并返回,也就是长跳转到catch开始的地方进行处理,catch处理函数会检查返回值的类型是否匹配之后才会决定是否继续处理.在处理完一些必要的操作之后将栈指针一次性减到catch开始的位置,这就完成了栈回退.如果其中带有finally块或者包含有类对象,那么也会调用对象的析构函数.实现方式是,将做这些工作的指令编译在catch的最后面,也就是函数return指令的前面.然后在catch之前的位置做一个短跳转,跳过catch处理的指令.这样如果正常流程没有发生异常就能跳过catch的处理指令.
  //至于catch处理函数如何判断返回值,猜想可能是比对类的签名,如果标签不符,就转到下一个catch,如果全部的catch都不符合,就继续向上查找try-catch
  
  
  try{
    ...
  }
  catch (char * str){
  
  
  }
  catch (std::exception& e){        //exception基类在exception.h里
  
  
  }
  catch (...)   //表示匹配任何异常
  finally{
  
      return 0;
  }


  void show(){
      throw "error";  //抛出异常
  }


int *pd = new (std::nothrow) int;   //这种new在分配内存失败时返回空指针
int *pd = new (std::nowthrow) int[100];



//未捕获异常,将调用下列函数,有编译器提供的,系统系统负责调用
typedef void (*terminate_handler)();  //自定义函数类型
terminate_handler set_terminate(terminate_handler) throw();c++98
terminate_handler set_terminate(terminate_handler) noexcept;c++11
void terminate(); c++98
void terminate() noexcept;c++11




//引发了声明的异常类型之外的异常,也就是使用c++98格式声明的之外异常
typedef void (*unexpected_handler)();  //自定义函数类型
unexpected_handler set_unexpected() throw();
unexpected_handler set_unexpected() noexcept; c++11
void unexpected() throw();         //这个函数调用terminate();
void unexpected() noexcept;

```

  * RTTI,运行时类型识别.只适用于包含虚函数的类,因为只有这种情况才需要动态识别
```c++
  //测试能否 obj是否为str的子代类,如果是就返回对象的指针,否则就返回空指针
 string *str =  dynamic_cast<string *>(obj);
 
 
 //如果是引用,测试不通过将引发bad_cast异常
 string *str = dynamic_cast<string &>(obj);
 
 //typeid和type_info用来获取对象的信息,返回一个type_info对象的引用    //在typeinfo头文件
  type_info info = typeid(*obj)   //obj可以是表达式,只要最终结果是对象就行
  
  
  
  //类型转换
  
  dynamic_cast //同上,用于指针或引用
  
  const_cast  //可以用于对象或者指针及引用,用来消除const或volatile标签.
  int *a = const_cast<int *>(obj);使其可以修改,但能否真正修改还是却决于obj所指向的原值是否是const.并且要注意,转换的时候不能改变类型属性
  
  static_cast //用与基类和派生类的互相转换 
  
  static_cast //也可用于将一个左值转换为一个右值,进而使用类的右值函数
  > name obj = static_cast<name &&>(other)  //本类是应该使用正常的复制赋值函数的,但是经过转换会调用移动赋值函数 即调用operator=(name &&)进行移动赋值
  
  reinterpret_cast //可以很多转换,但是不能缩窄转换,比如将指针类型转换成short这是不行的,或者将数据指针与函数指针互转,这也是不行的.
  struct a{int a;short b;};
  int value = 0x124845;
  a *pd = reinterpret<a *>(&calue)
  
  
  

```
  
  
  
  
  * 智能指针模板,memory,用法相同
  
  ```c++
      auto_ptr<double> pd(new double);   //因为pd是类对象,有析构函数,所以作用域过期时会自动调用析构释放保存的内存.不能分配数组
                                         //赋给其他智能指针对象时,会转移所有权,老的对象无法在引用
      unique_ptr                         //和上面一样,但是更安全,当使用=运算符转移控制权时,会报错.它重载了运算符,不接受=赋值,智能使用构造函数初始化. 可以用数组形式分配
      shared_ptr                         //引用计数计数指针,计数为零才会释放持有的对象,不能分配数组
      weak_ptr
  ```
  
  * for(int i : array){...}                                                      // 基于范围的for循环,c++11,用于容易
  
  * 为了重载前缀及后缀运算符
  ```c++
  int operator++();这是前缀版本
  int operator++(int);这是后缀版本
  
  ```
  
  *  重载()函数调用运算符可以像调用函数那样调用对象,即函数对象
  * c++11新增了initializer_list模板,使用它作为参数的构造函数可以使用列表初始化语法,需要头文件initializer_list

*  c++文件操作

* c++11新标准

+ 初始化列表语法禁止缩窄

+ 移动语义,也就是将临时右值指定一个标签进行持久化使用,将临时变为持久. 如果没有移动语义,在某些编译器中,使用右值时,可能会创建多个临时对象,有时候也可以通过static_cast进行强制转换为右值

移动构造,赋值运算符  这两函数是c++11默认提供的函数之一,这些默认的函数行为和默认的复制或者赋值一样
```c++

name::name(name && val){
  this.count = val.count;
  this.pc = val.pc
  val.count = 0;
  val.pc = 0;
}
    

```

+ 函数属性 default ,delete

> name:name() = default;   //当因为编译器不自动生成需要的函数时,可以使用这个符号,表明使用默认定义的版本,只能用在编译器会自动生成的成员函数中
> name:name() = delete;    //当需要某个函数不可用时,就可以使用这个符号,表示无法调用此函数,可以用在任何成员函数

+ 委托构造函数,可以使用构造函数的成员初始化语法,来进行引用其他的构造函数,进行创建

+ 继承构造函数

```c++
    class name{
    public:
        name();
        name(int);
        name(double);
    };

    class other : public name{
    public:
        using name::name        //这让基类构造显式的出现在派生类中,进而可以用来构造派生类对象.
        other();
        other(int);
    };


    other a();
    other b(12.5); //use name(double)

```

+ 虚方法属性,override和final.也可以用在常规标识符中
     通常派生类如果实现了与子类不同特征的虚方法,那么基类的虚方法将被隐藏,为了防止派生类实现基类的虚方法时出现特征不同的意外,可以使用override来表示覆盖基类的虚方法,如果不匹配就会报错
     final用在基类中,表示禁止派生类重新定义此函数

```c++
    class name{
        public:
            virtual void fun() const final{...}
            virtual void fun() const {...} = final
            
            virtual void fun(int a) const ;
    };
    
    class other : public name{
        public:
            virtual void fun() const{...} //错误, name中此函数为final
            virtual void fun(double a) override {...}   //错误,name中没有可以被覆盖的函数
    
    };
    

```
     
+ lambda表达式,一种函数对象,内联,可以声明在函数内部
```c++
int b
int c
[b,&c](int a){return c=a >b;}

//[]可以访问外围作用域的变量,也就是参数传递之外还可以传入固有变量.
//[&|=] 可以混合使用,&表示按引用访问,=表示按值访问,默认是按值访问.如果前面加上变量名,表示按指定模式访问某个变量
//默认的返回值是自动推断的,没有return默认是void

auto fun = [&c,=b](int a) ->double {return c=a>b;}  //可以给表达式取个别名,以后就可以像使用函数名一样使用它

```
     
 + function函数包装器,可以解决模板在相同特征标会生成多个函数的情况
 
 + 可变参数模板
 
 ```c++
template<typename T,typename... ARGS> 
 void show(T value,ARGS... args) // 如果定义成ARGS&... args则表示使用引用传参
 {
     std::cout<< value <<endl<<",";
     show(args...);     //args会原地展开函数列表,如果不这样定义,直接展开args将会导致无限递归展开
 }
 
template<typename T>
 void show(T value){
    std::cout<< value<<endl;  //当只剩最后一个参数时就会调用这个模板定义的
 }
 
 ```


## 其他

+ alignof(int)返回对其要求
+ alignas(8) 按某字节对齐
+ constexpr 在编译阶段计算结果为常量的表达式,也就是表达式是变量,但是强制将其识别为常量


+ 指针成员,可以定义一个指针,引用一个类的成员,由于存在类这一中间类型,所以需要特殊语法

```c++
    class name{
        private:
            char name;
            int age;
         int show(int) const{...}
    };

int name::*pt = &name::name;     //这表示定义了一个指针,指向了name类的成员,相当于存储了一个成员在类中的偏移
name obj;
name *oth = new oth;
std::cout << obj.*pt << endl;   // 可以这样使用
std::cout << oth->*pt << endl;


void (name::*pf)(int)const;
pf = &name::show;     //必须使用&关键字取地址
(obj.*pf)(30);        //这样调用函数

以上用法都可以用在运行时进行决策

```

+ noexcept
> int halt(int) noexcept  ;  //用于函数表示不会引发异常
>noexcept(halt);  //返回ture,表示这个函数或者表达式使用noexcept声明
