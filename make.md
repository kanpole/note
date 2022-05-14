# GCC工具链与MAKE的使用


## GCC


### GCC编译步骤

    
    + gcc -o  #编译成的目标
    + gcc -E  #预编译 .i
    + gcc -S  #汇编  .s
    + gcc -c  #生成编译文件，但不链接 .o
    
    ```shell
    
        常用参数：
        + -Wl   #传递给连接器的参数，用逗号隔开
        + -Wall #启用严格警告
        + -I    #include路径
        + -L    #库路径
        + -l    #链接的库
        + -g    #添加调试信息
        + -O2   #优化级别 
        + -static    #链接时使用静态链接库
        + -fPic      #生成位置无关代码，一边个人库不会指定库加载的位置，但是某些系统级的库需要，比如中断函数要在某个固定位置
        + -shared    #链接时使用动态库
        + -rpath     #指定运行时动态库的查找路径
        + -rpath-link  #指定链接时间接库的查找路径
        + -march           #编译的程序模式 32or64
    ```
    
    + ld             #链接器，链接目标
    
    ```
        手动编译一次动态库，假设有一个one.c库
        gcc -E one.c -o one.i
        gcc -S -melf_x86_64 one.i -o one.s
        gcc -c -fpic -shared one.s -o one.o
        ld  -melf_x86_64 -fpic -shared one.o -o one.so
    
    
    
    
    ```
    
    
    + ar               #打包静态库
    > ar rcs           #打包库文件
    
    + ranlib           #重建索引，相当于ar -s
    + nm               #库信息查看工具
    + objdump          #反汇编产看工具
    + size             #头查看工具
    + string           #数据查看工具
    + ldd              #链接查看工具
    + linux命令 ulimit  #限制资源
### GBD调试

    ```c
        file         #加载程序
        list         #查看源码
        display      #查看汇编
        break        #下断点
        break if     #条件断点
        info         #查看
        watch expr   #监视表达式，发生变时中断
        delete       #删除断点
        continue     #继续执行到下个断点
        step         #步入，进入函数
        next         #步过，不进函数内部
        finish       #运行到当前函数返回，并打印函数堆栈等信息
        until        #运行到循环结束
        print        #查看变量
        ptype        #查看类型
        print array  #查看数组
        print *array@len  #查看动态内存
        print x=5         #修改运行时数据
    
    ```


## Make规则

  例子
  ```
  OBJS = main.o lib.o math.o
  TARGET = main
  CC  = gcc
  AR = ar -rcs
  LD = ld 

  SHARED = -fpic -shared $(MAR)

  CFLAGS := -c -w $(MAR) -g

  LFLAGS = -L/usr/lib/ -lstdc
  
  $(TARGET):$(OBJS);ls -a
    $(CC) $(CFLAGS) $(OBJS)  -o $(TAGGET) \
    ls -a
  
  one.o:one.h three.o                     #隐含规则可以省略编译命令

  .PHONY:clean
   clean:                                 #没有任何依赖的规则永远不会被自动执行, 并且总会被认为是最新的
      -rm *.o                             #-表示忽略执行错误，@表示禁用回显
      
  main.o::main.c                         #目标可以有多个重建规则，但是必须是同一种冒号格式，双冒号的处理与单冒号不同，哪个规则的依赖更新就只执行哪个规则的依赖
      command 
    
  main.o::main.h
      command
    



  #make中规则的命令如果属于同一行，那它们就会在同一个shell中执行，否则就是相互独立的，所以最后使用；和\来分割以及表示换行
    
  [-]include *.mk foo other              #包含其他make,-表示找不到包含的文件不会报错终止
  
  sources = main.c one.c
  sinclude $(sources:.c=.mk)             #sinclude表示包含多个文件
  
  环境变量MAKEFILES                         #此环境变量会在make执行时,优先读取,并且不会报错也不会作为终极目标
  MAKEFILE_LIST                           #这个变量存储了makefile链表,最后一个表示当前的mk文件
  %:force                                 #模式规则，表示所有目标，优先级最低

  
  
  cd subdir && $(MAKE)                                 #表示make程序
  $(MAKE) -C subdir                                    #-C表示进入subdir目录并执行make,当$(MAKE)作为规则命令，并且主make带有-n -t -q的时候，它还是会执行
  
  变量CURIDR                                            #会被自动赋值，表示当前进入的目录
  MAKEFLAGS                                            #表示传给make的参数
  MAKELEVEL                                            #代表了make嵌套的深度
  .EXPORT_ALL_VARIABLES                                #表示导出所有变量到子例程
  export variables = value
  unexport variables
  export                                               #导出所有变量到子例程
  
  
  
  #定义变量方式,有延后和立即展开
  =   #使用时展开
  :=  #定义时展开
  ?=  #没有定义就用=定义,否则不改变值
  +=  #添加,没有就用=定义,有直接添加
  

  
  #条件语句
  
  main.exe:main.c -lstdcpp| main.h ; gcc main.c                    #-l表示依赖库,规则和gcc相同,|之后的依赖更新不会导致规则重建
  
  VPATH = src : ../include                               #从当前目录开始,使用这些目录作为搜索目标和依赖的位置
  vpath %.h ../heards                                    #为某种文件或者模式单独设置目录
  vapth %.h 
  vpath                                                  #清除所有目录搜索规则
  
  GPATH = src : ../execute                          #当需要重建,但目标不存在时,将会在工作目录下构建目标,否则就在目标所在的目录下重建,和VPATH有所不同
  .LIBPATTERNS =lib%.so lib%.dll                    #这个变量控制了,在依赖中包含-lstdc++这种库时,如何解析
  
  .DELETE_ON_ERROR：%.o %.exe                       #有一堆这种特殊目标
  
  one.out two.out files.out:head.h                         #多目标
      @echo head.h $(subst out,,$@) >$@ 
  



OBJS = main.o two.o one.h                           #静态规则,模式替换规则,动态替换规则

    $(filter %.o,$(OBJS)):%.o:%.c
        gcc $< -o $@


    aout bout:%out,%.c
        gcc $< -o $@
        echo $*                                     #echo a b
        

```


###变量

```

    make a=val                        #可以在make运行时定义变量的值
                                      #引用一个没有定义的变量，默认为空

    $a                                #单字变量可以不带括号
    $abc = $(a)bc
    
    a=$(b)                            #将a赋值给其他变量时a的内容是$(b),这种赋值可以引用后面的变量
    b=c 


    
    a:=c
    b:=$(a)                           #定义变量时，b=c，定义的时候对变量解引用，不能引用后面未定义的变量,否则就是空
    
    a?=b                              #变量a没有值的时候才会被赋值
    
    
    a:a.c b.c
    d=$(a:.c=.o)                      #d= a.o b.o快速替换，相当于函数patsubst
 or d=$(a:%.c=%.o)
 
 
    a+=b                              #如果之前a没有定义就用=来定义
    
    a:=b.c
    a+=d.c                            #相当于a:= b.c d.c,立即展开
 
    overried CFLAGS += -g             #可以防止变量被命令行修改


    define name
        command \                     #多行变量定义,相当于 = 定义的变量
        command
    
    endef
    
    
    %.o:CFLAGS = -g -w                  #目标指定变量,模式指定变量,可以和其他变量同时存在，对它的修改不会影响全局，重建目标时优先使用
    
    
    ifeq ifneq ifdef ifndef else end    #条件测试语句
    ifeq($(CC),gcc)
       content
    else ifdef G++
       content
    endif
    
    
    
    
    

```

### 内嵌函数

```
字符串替换函数,不能使用模式
$(subst from,to,text)

模式字符串替换函数
$(patsubst)

去掉前后空格,中间空格保留一个
$(strip string)

在in里查找字符串,查到返回find否则为空
$(findstring find,in)


返回符合指定模式的结果
$(filter %.c %o , text)


反过滤函数
$(filter-out %.c %.o..   , text)


排序并去重复
$(sort text)


取单词函数,取text的第n个项,从1开始
$(word n,text)

范围取单词函数,取从s,到e的项
$(word s,e,text)

计算text中单词的数目
$(words text)


返回text的第一个单词
$(firstword text)


返回text中各项的目录部分,如果没有默认为./ .返回的结果中包含最后的/
$(dir text)

返回text中各项的非目录部分,只有目录的项对应返回空
$(notdir text)


取后缀函数
$(suffix text)

取前缀函数
$(basename text)


给text中的每一项添加后缀
$(addsuffix suffix,text)


添加前缀,给每一项的前面添加前缀
$(addprefix prefix,text)

链接函数,在list1的第一项后面添加list2的第一项,以此类推,多余的原样返回
$(join list1,list2)

返回当前目录下所有匹配模式的文件,可以多模式
$(wildcard *.c)

循环函数,循环将text中的每一项赋值给var,并执行eval的表达式
$(foreach var,text,eval)

添加函数,测试第一个值,为真返回第二项,否则返回第三项,如果两项都没有返回空
$(if $(GCC) ,$(GCC),$(G++))


调用自己定义的函数变量
$(call variable,param,param,...)
link = $(addsuffix $(1),$(2))
result :=$(link one,.c) 
result := one.c

获取变量的字面值,也就是不展开时的值,但是对与:=这中立即展开的会返回其立即展开后的值
$(value name)

对make表达式求值并返回
$(eval make-val)

返回变量的来源,即在哪定义的
$(origin name)


返回一个致命错误
$(error text)


返回一个警告给用户
$(warning text)




```



### 选项

```c
    -w #切换目录时，打印进入的目录
    -s #禁止打印命令本身
    -C #进入一个目录并执行
    -i #遇到命令错误不退出
    --keep-going #-遇到规则错误不退出
    -e #表示使用环境变量的值，文件内或者命令行变量不会替换环境变量
        -t #更新文件的时间戳，不执行命令，但是如果目标规则命令中带+号还是会执行
```



### 其他

```

    define name
        echo "$@"
        echo "$<" 
    endef

main.c:main.o
    @$(name)                   #加上@表示这个包内的所有命令都禁止回显

-----------------------
    target:;                   #空目标，总是被认为是最新的，以它为依赖的目标总是会被执行

    .PHONY                     #特殊目标，可以实现一些功能，用来兼容老式make程序

    



```





### 一个完整例子

```make
makefile
    VPATH = src:/heards
    vpath %.h ../heards
    CC := gcc
    CFLAGS := -g -w -O2 -melf_x86_64
    AR := ar rcs
    LD := ld
    LDFLAGS := -melf_x86_64
    INCLUDE := -I./include -I"%INCLUDE%/include"
    LIB := -L./lib -L"%LIB%/lib"
    STATIC := -static
    SHARED := -fpic -shared 
    

    PREFIX := ./
    BUILD=./build
    INSTALL := ./bin
    CLEAN = -rm -rf *.o *.i *.s ./build ./bin


    SOURCS := main.o
    LINKLIBS := -lc -lstdc++ -lgcc
    
    TARGET := one.exe
    
    include make2.mk
    include make3.mk
    
    
make2.mk
vpath %.h
OBJS =$(patsubst %.c,%.o,$(wildcar *.c)
$(TARGET) : $(SOURCES) | $(LINKLIBS) -lstdc++
    $(CC) -c $(CFLAGS) $(SHARED) $(INCLUDE) $(LIB)  -o $(TARGET)

%.o:%.c

print:*.c
    echo $?
    touch print
    
make3.mk
    vpath


%.d:%.c
    $(CC) -MM $(CFLAGS) $< > $@.$$$$;\
    sed 's ,\($*\)\.o[:]*,\1.o $@:, g' <$@.$$$$ > $@;\
    rm -f $@.$$$$


source =main.c one.c
sinclude $(source %.c=%.d)



.PHONY:clean;
clean:
    $(CLEAN)
```
